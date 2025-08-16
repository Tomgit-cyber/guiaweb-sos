Okay, vamos atualizar o `README.md` com base nas informações mais recentes do guia de desenvolvimento HTML fornecido nos arquivos.

**README.md**

```markdown
# SOS Unimed Vidas - MVP

Este repositório contém o código-fonte e a documentação para o desenvolvimento do aplicativo **SOS Unimed Vidas**, um sistema de alerta médico de emergência. O objetivo é criar um MVP (Produto Mínimo Viável) funcional e seguro.

## 🎯 Objetivo

Desenvolver um aplicativo móvel multiplataforma (Android/iOS) utilizando **Flutter** que permita aos pacientes acionarem rapidamente um alerta médico em situações de emergência. O aplicativo também incluirá funcionalidades administrativas para gestão dos atendimentos.

## 🧱 Arquitetura Geral

O aplicativo segue uma arquitetura de **navegação condicional baseada no perfil do usuário** após o login. Um único app oferece funcionalidades distintas para **Pacientes** e **Administradores**.

### 👥 Perfis de Usuário

*   **Paciente:**
    *   Login/Logout seguro via Firebase Authentication.
    *   Tela principal com informações pessoais.
    *   Botão de emergência médica para acionar alerta.
    *   Histórico de atendimentos.
    *   Perfil e configurações.
*   **Administrador:**
    *   Login/Logout seguro via Firebase Authentication.
    *   Dashboard com estatísticas (pacientes, atendimentos, alertas).
    *   Gerenciamento de pacientes.
    *   Visualização de atendimentos em andamento.
    *   Gestão de alertas pendentes.
    *   Ferramentas de teste (ex: Testar Conexão Firestore).

## 🛠️ Tecnologias Utilizadas

*   **Frontend & Mobile:** Flutter (Dart)
*   **Backend & Autenticação:** Firebase (Authentication, Firestore, Cloud Functions - potencial)
*   **IDE Recomendada:** Android Studio / VS Code
*   **Controle de Versão:** Git

## 📁 Estrutura do Projeto (Dart/Flutter)

```
lib/
├── main.dart                 # Ponto de entrada do aplicativo
├── services/                 # Lógica de negócios e integrações
│   ├── auth_service.dart     # Serviço de autenticação (Firebase Auth)
│   └── firestore_service.dart # Serviço de acesso ao banco de dados (Firestore)
├── views/                    # Telas do aplicativo
│   ├── login_screen.dart     # Tela de login
│   ├── home_screen.dart      # Tela principal (navegação condicional)
│   ├── patient/              # Telas específicas do perfil Paciente
│   │   ├── patient_home_screen.dart
│   │   ├── emergency_screen.dart
│   │   ├── history_screen.dart
│   │   └── profile_screen.dart
│   ├── admin/                # Telas específicas do perfil Administrador
│   │   ├── admin_home_screen.dart
│   │   ├── dashboard_screen.dart
│   │   ├── patient_management_screen.dart
│   │   ├── ongoing_attendances_screen.dart
│   │   └── alerts_screen.dart
│   ├── help_screen.dart      # Tela de ajuda
│   └── ...                   # Outras telas comuns
└── widgets/                  # Componentes reutilizáveis da UI
    └── ...                   # Ex: CustomButton, UserInfoCard, etc.
```

*(A estrutura acima reflete a separação de responsabilidades por perfil mencionada no guia.)*

## 🚀 Primeiros Passos (Setup)

### Pré-requisitos

*   **Flutter SDK:** Instalado e configurado no PATH.
*   **IDE:** Android Studio ou VS Code com plugins Flutter/Dart.
*   **Android/iOS:** Emulador configurado ou dispositivo físico para testes.
*   **Git:** Para controle de versão.

### Configuração do Ambiente

Siga os guias detalhados no HTML para configurar o ambiente Flutter em:

*   **ChromeOS/Linux**
*   **Windows**
*   **macOS**

Geralmente envolve:
1.  Baixar e extrair o Flutter SDK.
2.  Adicionar o `flutter/bin` ao PATH do sistema.
3.  Instalar/configurar o Android Studio e as dependências do Android (SDK, AVD).
4.  Executar `flutter doctor` para verificar a instalação.

### Rodando o Projeto

1.  Clone este repositório: `git clone <URL_DO_REPOSITORIO>`
2.  Navegue até o diretório do projeto: `cd sos_unimed_vidas`
3.  Obtenha as dependências: `flutter pub get`
4.  Execute o aplicativo: `flutter run` (certifique-se de ter um emulador/dispositivo conectado)

## 📖 Documentação & Guia de Desenvolvimento

Este projeto é acompanhado por um **Guia de Desenvolvimento Completo** em formato HTML. Este guia contém:

*   Visão geral do projeto e entregáveis.
*   Guias detalhados de configuração de ambiente para diferentes sistemas operacionais (ChromeOS/Linux, Windows, macOS).
*   Passo a passo para criar o projeto Flutter.
*   Estruturação da aplicação.
*   Criação da Interface do Usuário com o tema da Unimed.
*   Implementação da lógica de negócio (autenticação, navegação condicional).
*   Integrações com Firebase (Auth, Firestore).
*   Considerações para implantação (Analytics).
*   Processo de entrega (documentação, transferência).

**Acesso ao Guia:** O arquivo HTML do guia está incluído neste repositório (nome do arquivo `.html`). Abra-o em um navegador para acessar a documentação completa e interativa.

## 📦 Entregáveis

*   Código Fonte Completo no repositório Git.
*   Pacotes de Instalação (APK para Android, potencialmente IPA para iOS).
*   Configuração do projeto Firebase.
*   Documentação (este README e o Guia HTML).
*   Sessão de Transferência de Conhecimento.

## 🔐 Segurança

*   Autenticação de usuários via Firebase Authentication.
*   Controle de acesso baseado em perfil no Firestore.

## 📄 Licença

© 2025 Everton Simão Marques – Todos os direitos reservados.
Este guia contém informações técnicas confidenciais e é de uso exclusivo autorizado.
Reprodução total ou parcial proibida sem consentimento prévio por escrito.

*(Nota: O texto de licença foi extraído do rodapé do HTML fornecido.)*

```

**Próximos Passos:**

1.  **Adicione este conteúdo ao seu arquivo `README.md` no repositório Git.**
2.  **Confirme (commit) e envie (push) o `README.md` atualizado.**
3.  **Certifique-se de incluir o arquivo HTML do guia no repositório também, se ainda não estiver.**
4.  **Ajuste o nome do arquivo HTML no README se for diferente.**
5.  **Atualize a URL do repositório na seção "Rodando o Projeto".**

Este `README.md` fornece uma visão geral abrangente do projeto com base nas informações mais recentes do guia HTML.
