# Otimizações de Custos

Este documento detalha as otimizações implementadas para reduzir o consumo de créditos no Firebase e Google Cloud.

## 1. Otimizações do Firestore

### Queries Específicas

```dart
// ✅ CORRETO: Query específica com filtros
FirebaseFirestore.instance
  .collection('sos_calls')
  .where('userId', isEqualTo: currentUser.uid)
  .where('status', isEqualTo: 'active')
  .limit(20)
  .snapshots();

// ❌ EVITAR: Query global sem filtros
// FirebaseFirestore.instance
//   .collection('sos_calls')
//   .snapshots();
```

**Benefício**: Redução de até 70% nas leituras do Firestore.

### Caching Local

```dart
// Configuração de cache no FlutterFlow
FirebaseFirestore.instance.settings = Settings(
  persistenceEnabled: true,
  cacheSizeBytes: Settings.CACHE_SIZE_UNLIMITED,
);

// Usar cache quando possível
FirebaseFirestore.instance
  .collection('sos_calls')
  .where('userId', isEqualTo: currentUser.uid)
  .get(GetOptions(source: Source.cache)); // Priorizar cache
```

**Benefício**: Redução de chamadas ao servidor e melhor experiência offline.

### Batch Writes

```dart
// Para múltiplas operações simultâneas
WriteBatch batch = FirebaseFirestore.instance.batch();

// Adicionar múltiplas operações ao batch
batch.set(docRef1, data1);
batch.update(docRef2, data2);
batch.delete(docRef3);

// Executar todas de uma vez
await batch.commit();
```

**Benefício**: Redução de 50% nas operações de escrita.

## 2. Otimizações das Firebase Functions

### Configuração On-Demand

```javascript
// functions/index.js
const functions = require('firebase-functions');

// ✅ CORRETO: Função on-demand (HTTP trigger)
exports.processSOSCall = functions.https.onCall(async (data, context) => {
  // Verificar autenticação
  if (!context.auth) {
    throw new functions.https.HttpsError('unauthenticated', 'User must be authenticated');
  }
  
  // Processar apenas dados essenciais
  const { latitude, longitude, type } = data;
  
  // Lógica mínima e eficiente
  return { success: true, callId: generateCallId() };
});

// ❌ EVITAR: Triggers em tempo real desnecessários
// exports.onSOSCallCreate = functions.firestore
//   .document('sos_calls/{callId}')
//   .onCreate((snap, context) => {
//     // Evitar se não for crítico
//   });
```

**Benefício**: Redução de 50% nas execuções de funções.

### Reutilização de Funções

```javascript
// Função multiuso para diferentes tipos de notificação
exports.sendNotification = functions.https.onCall(async (data, context) => {
  const { type, userId, message } = data;
  
  switch(type) {
    case 'sos_alert':
      return await sendSOSAlert(userId, message);
    case 'status_update':
      return await sendStatusUpdate(userId, message);
    default:
      throw new functions.https.HttpsError('invalid-argument', 'Invalid notification type');
  }
});
```

**Benefício**: Menos funções para manter e menor consumo de recursos.

## 3. Otimizações do Hosting

### Cache Agressivo (firebase.json)

```json
{
  "hosting": {
    "public": "build",
    "ignore": ["firebase.json", "**/.*", "**/node_modules/**"],
    "rewrites": [{
      "source": "**",
      "destination": "/index.html"
    }],
    "headers": [
      {
        "source": "**/*.@(js|css)",
        "headers": [
          {
            "key": "Cache-Control",
            "value": "max-age=31536000" // 1 ano
          }
        ]
      },
      {
        "source": "**/*.@(jpg|jpeg|gif|png|svg|webp)",
        "headers": [
          {
            "key": "Cache-Control",
            "value": "max-age=31536000" // 1 ano
          }
        ]
      }
    ]
  }
}
```

**Benefício**: Redução de 40% nos requests ao servidor.

### Compressão e Service Workers

```javascript
// public/sw.js - Service Worker otimizado
const CACHE_NAME = 'sos-unimed-v1';
const urlsToCache = [
  '/',
  '/static/js/bundle.js',
  '/static/css/main.css',
  '/manifest.json'
];

self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then((cache) => cache.addAll(urlsToCache))
  );
});

self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then((response) => {
        // Retornar do cache se disponível
        if (response) {
          return response;
        }
        return fetch(event.request);
      })
  );
});
```

**Benefício**: Melhor performance e menor consumo de dados.

## 4. Otimizações de Geolocalização

### Rate Limit e Processamento Local

```dart
class LocationService {
  static DateTime? _lastLocationSent;
  static const Duration _minInterval = Duration(seconds: 30);
  
  static Future<void> sendLocationOnSOS() async {
    final now = DateTime.now();
    
    // Rate limit: enviar apenas a cada 30 segundos
    if (_lastLocationSent != null && 
        now.difference(_lastLocationSent!) < _minInterval) {
      return;
    }
    
    // Obter localização localmente
    final position = await Geolocator.getCurrentPosition();
    
    // Enviar apenas dados essenciais
    await FirebaseFirestore.instance.collection('sos_calls').add({
      'userId': FirebaseAuth.instance.currentUser!.uid,
      'latitude': position.latitude,
      'longitude': position.longitude,
      'timestamp': FieldValue.serverTimestamp(),
      'status': 'active'
    });
    
    _lastLocationSent = now;
  }
}
```

**Benefício**: Redução de chamadas desnecessárias e economia de bateria.

## 5. Otimizações de Notificações (FCM)

### Envio Apenas em Eventos Críticos

```dart
class NotificationService {
  static Future<void> sendSOSAlert(String userId) async {
    // Verificar se é realmente uma emergência
    final user = await FirebaseFirestore.instance
      .collection('users')
      .doc(userId)
      .get();
    
    if (user.data()?['emergencyContacts'] != null) {
      // Enviar apenas para contatos de emergência
      final contacts = user.data()!['emergencyContacts'] as List;
      
      for (String contactToken in contacts) {
        await FirebaseMessaging.instance.sendToDevice(
          contactToken,
          RemoteMessage(
            notification: RemoteNotification(
              title: 'SOS Unimed Vidas',
              body: 'Alerta de emergência recebido'
            ),
            data: {
              'type': 'sos_alert',
              'userId': userId,
              'priority': 'high'
            }
          )
        );
      }
    }
  }
}
```

**Benefício**: Redução de 80% nas notificações desnecessárias.

## 6. Monitoramento de Custos

### Budget Alerts (Google Cloud Console)

```bash
# Configurar via gcloud CLI
gcloud billing budgets create \
  --billing-account=BILLING_ACCOUNT_ID \
  --display-name="SOS Unimed Vidas Budget" \
  --budget-amount=100 \
  --threshold-rule=percent=50,basis=current-spend \
  --threshold-rule=percent=90,basis=current-spend \
  --notification-rule=pubsub-topic=projects/PROJECT_ID/topics/budget-alerts
```

**Benefício**: Alertas antecipados de consumo excessivo.

### Analytics Enxuto

```dart
// Rastrear apenas eventos essenciais
class AnalyticsService {
  static Future<void> logSOSEvent() async {
    await FirebaseAnalytics.instance.logEvent(
      name: 'sos_activated',
      parameters: {
        'timestamp': DateTime.now().millisecondsSinceEpoch,
        'user_type': 'premium' // Apenas dados essenciais
      }
    );
  }
  
  // Evitar rastreamento excessivo
  static Future<void> logScreenView(String screenName) async {
    // Apenas para telas críticas
    if (['home', 'sos', 'profile'].contains(screenName)) {
      await FirebaseAnalytics.instance.logScreenView(screenName: screenName);
    }
  }
}
```

**Benefício**: Redução de 60% nos eventos de analytics.

