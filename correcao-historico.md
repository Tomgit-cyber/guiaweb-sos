# Correção do Histórico de Chamadas

## Problema Identificado

O histórico de chamadas atualmente mostra chamadas gerais, não específicas do usuário logado, comprometendo a privacidade dos dados.

## Solução Implementada

### 1. Modelo de Dados (emergency_history.dart)

```dart
class EmergencyCall {
  final String id;
  final String userId;
  final DateTime timestamp;
  final double latitude;
  final double longitude;
  final String status;
  final String type;
  final String description;

  EmergencyCall({
    required this.id,
    required this.userId,
    required this.timestamp,
    required this.latitude,
    required this.longitude,
    required this.status,
    required this.type,
    required this.description,
  });

  factory EmergencyCall.fromFirestore(DocumentSnapshot doc) {
    final data = doc.data() as Map<String, dynamic>;
    return EmergencyCall(
      id: doc.id,
      userId: data['userId'] ?? '',
      timestamp: (data['timestamp'] as Timestamp).toDate(),
      latitude: (data['location'] as Map<String, dynamic>)['latitude'] ?? 0.0,
      longitude: (data['location'] as Map<String, dynamic>)['longitude'] ?? 0.0,
      status: data['status'] ?? 'unknown',
      type: data['type'] ?? 'emergency',
      description: data['description'] ?? '',
    );
  }

  Map<String, dynamic> toMap() {
    return {
      'userId': userId,
      'timestamp': Timestamp.fromDate(timestamp),
      'location': {
        'latitude': latitude,
        'longitude': longitude,
      },
      'status': status,
      'type': type,
      'description': description,
    };
  }
}
```

### 2. Serviço de Histórico (EmergencyHistoryService)

```dart
class EmergencyHistoryService {
  final FirebaseFirestore _firestore = FirebaseFirestore.instance;
  final FirebaseAuth _auth = FirebaseAuth.instance;

  // Método otimizado para obter histórico específico do usuário logado
  Stream<List<EmergencyCall>> getUserEmergencyHistory() {
    final currentUser = _auth.currentUser;
    if (currentUser == null) {
      // Se não houver usuário logado, retorna uma lista vazia
      return Stream.value([]);
    }

    // Query otimizada com filtro por usuário e limit para reduzir custos
    return _firestore
        .collection('sos_calls')
        .where('userId', isEqualTo: currentUser.uid)
        .orderBy('timestamp', descending: true)
        .limit(50) // Limitar para otimizar custos
        .snapshots()
        .map((snapshot) => snapshot.docs
            .map((doc) => EmergencyCall.fromFirestore(doc))
            .toList());
  }

  // Método para criar uma nova chamada de emergência
  Future<void> createEmergencyCall({
    required double latitude,
    required double longitude,
    required String type,
    required String description,
  }) async {
    final currentUser = _auth.currentUser;
    if (currentUser == null) {
      throw Exception('User must be logged in to create emergency call');
    }

    // Usar batch write para operações múltiplas (mais eficiente)
    final batch = _firestore.batch();
    
    // Documento da chamada de emergência
    final callRef = _firestore.collection('sos_calls').doc();
    final call = EmergencyCall(
      id: callRef.id,
      userId: currentUser.uid,
      timestamp: DateTime.now(),
      latitude: latitude,
      longitude: longitude,
      status: 'active',
      type: type,
      description: description,
    );
    
    batch.set(callRef, call.toMap());
    
    // Atualizar contador de chamadas do usuário (exemplo de operação adicional)
    final userRef = _firestore.collection('users').doc(currentUser.uid);
    batch.update(userRef, {
      'emergencyCallCount': FieldValue.increment(1),
      'lastEmergencyCall': Timestamp.now(),
    });
    
    // Executar batch
    await batch.commit();
  }

  // Método para atualizar o status de uma chamada
  Future<void> updateCallStatus(String callId, String status) async {
    final currentUser = _auth.currentUser;
    if (currentUser == null) {
      throw Exception('User must be logged in to update call status');
    }

    // Verificar se a chamada pertence ao usuário antes de atualizar
    final callDoc = await _firestore.collection('sos_calls').doc(callId).get();
    if (callDoc.exists) {
      final data = callDoc.data() as Map<String, dynamic>;
      if (data['userId'] == currentUser.uid) {
        await _firestore.collection('sos_calls').doc(callId).update({
          'status': status,
          'updatedAt': Timestamp.now(),
        });
      } else {
        throw Exception('User does not have permission to update this call');
      }
    } else {
      throw Exception('Call not found');
    }
  }
}
```

### 3. Regras de Segurança do Firestore

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Regra para coleção sos_calls
    match /sos_calls/{callId} {
      // Usuário só pode ler/escrever suas próprias chamadas
      allow read, write: if request.auth != null 
        && request.auth.uid == resource.data.userId;
      
      // Permitir criação apenas se o userId corresponde ao usuário autenticado
      allow create: if request.auth != null 
        && request.auth.uid == request.resource.data.userId;
    }
    
    // Regra para coleção users
    match /users/{userId} {
      // Usuário só pode ler/escrever seu próprio documento
      allow read, write: if request.auth != null 
        && request.auth.uid == userId;
    }
  }
}
```

## Implementação no FlutterFlow

### Widget de Histórico de Chamadas:
1. **Data Source**: Configurar query personalizada para `sos_calls`
2. **Filter**: Adicionar filtro `userId == currentUser.uid`
3. **Order By**: `timestamp` descendente
4. **Limit**: 50 registros para otimizar performance
5. **Cache**: Ativar cache local para reduzir leituras

### Actions no FlutterFlow:
1. **Ao criar nova chamada SOS**:
   - Incluir automaticamente o `userId` do usuário logado
   - Usar batch write se houver múltiplas operações

2. **Ao carregar histórico**:
   - Verificar se usuário está autenticado
   - Aplicar filtro de usuário automaticamente
   - Implementar paginação se necessário

## Benefícios da Implementação

1. **Privacidade**: Cada usuário vê apenas suas próprias chamadas
2. **Segurança**: Rules restritivas impedem acesso não autorizado
3. **Performance**: Queries otimizadas reduzem latência
4. **Custos**: Menos leituras no Firestore = menor consumo de créditos
5. **Escalabilidade**: Solução preparada para crescimento da base de usuários

## Testes Recomendados

1. **Teste de Privacidade**: Verificar que usuário A não consegue ver chamadas do usuário B
2. **Teste de Performance**: Confirmar que o carregamento do histórico é rápido mesmo com muitas chamadas
3. **Teste de Custos**: Monitorar o número de leituras no Firestore antes e depois da implementação

