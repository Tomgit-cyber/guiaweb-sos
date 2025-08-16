**README.md (Versão Atualizada)**

```markdown
# 🚀 SOS Unimed Vidas - MVP (Assistente Técnico & Documentação)
**Versão Oficial: 02/08/25**
Desenvolvido por **Everton Simão Marques** para **Unimed Cachoeiro do Itapemirim**

Este repositório contém o código-fonte, a documentação técnica e o **Assistente Técnico Interativo** para o desenvolvimento e manutenção do aplicativo **SOS Unimed Vidas**, um sistema de alerta médico de emergência. O objetivo é facilitar a transição de conhecimento e garantir a continuidade do projeto para a equipe de TI da Unimed.



## 🎯 Objetivo do Projeto

Desenvolver um aplicativo móvel multiplataforma (Android/iOS) utilizando **Flutter** que permita aos pacientes (associados) acionarem rapidamente um alerta médico em situações de emergência. O aplicativo também incluirá funcionalidades administrativas para gestão dos atendimentos.

---

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
*   **Documentação Interativa:** HTML, CSS, JavaScript (Hospedável no Vercel)



## 📁 Estrutura do Projeto (Dart/Flutter)


lib/
├── main.dart                 # Ponto de entrada do aplicativo
├── auth/                     # Autenticação e modelos de usuário
│   ├── auth_service.dart
│   └── user_model.dart
├── services/                 # Lógica de negócios e integrações
│   ├── firestore_service.dart
│   └── notification_service.dart (futuro)
├── views/                    # Telas do aplicativo
│   ├── login/
│   │   └── login_screen.dart
│   ├── patient/              # Telas específicas do perfil Paciente
│   │   ├── patient_home_screen.dart
│   │   ├── emergency_screen.dart
│   │   └── profile_screen.dart (inclui histórico)
│   └── admin/                # Telas específicas do perfil Administrador
│       ├── admin_home_screen.dart
│       ├── dashboard_screen.dart
│       ├── patient_management_screen.dart
│       └── alerts_screen.dart
└── widgets/                  # Componentes reutilizáveis da UI
    └── ...                   # Ex: CustomButton, UserInfoCard, etc.


## 📦 Entregáveis

*   Código Fonte Completo no repositório Git.
*   Pacotes de Instalação (APK para Android, potencialmente IPA para iOS).
*   Configuração do projeto Firebase.
*   Documentação Técnica Completa (este README e o Guia HTML Interativo).
*   Sessão de Transferência de Conhecimento.

---

## 🧭 Como Utilizar este Repositório

1.  **Acesso ao Código:** Clone o repositório para obter o código-fonte do aplicativo Flutter.
2.  **Acesso à Documentação Interativa:** Hospede o arquivo HTML do guia (fornecido neste repositório) no Vercel ou servidor compatível. Acesse-o via navegador para um guia passo a passo completo com marcação de progresso.
    *   O guia aborda: Configuração de Ambiente (ChromeOS/Linux, Windows, macOS), Criação do Projeto, Estruturação, UI, Lógica de Negócio, Integrações Firebase, Testes, Build e Implantação.
3.  **Rodando o Projeto Flutter:**
    *   Certifique-se de ter o Flutter SDK instalado.
    *   Navegue até o diretório do projeto.
    *   Execute `flutter pub get` para instalar dependências.
    *   Execute `flutter run` (com emulador/dispositivo conectado).

---

## 🧠 Base Tecnológica (Assistente Interativo)

*   **Frontend:** HTML5, CSS3 (Grid/Flexbox), JavaScript Vanilla
*   **Hospedagem:** Vercel (ou outro serviço compatível)
*   **Armazenamento:** LocalStorage (persistência de estado do progresso)
*   **Design:** Gradientes verdes da Unimed, tipografia Open Sans/Roboto.


## 👨‍💻 Autoria e Contribuição

**Desenvolvido por:** Everton Simão Marques
Baseado nas diretrizes operacionais do projeto **SOS Unimed Vidas** e nas necessidades específicas da **Unimed Cachoeiro do Itapemirim**.


## 📜 Licença e Direitos

**© 2025 Everton Simão Marques – Todos os direitos reservados.**
Este guia contém informações técnicas confidenciais e é de uso exclusivo autorizado.
Reprodução total ou parcial proibida sem consentimento prévio por escrito.

Para uso interno da Unimed Cachoeiro do Itapemirim.


## 🧾 Visão Estratégica

Este projeto representa mais que um aplicativo — é uma **ferramenta estratégica de transição de conhecimento**. Seu design foca em:

*   **Redução de Risco:** Minimiza falhas na transição através de documentação clara e interativa.
*   **Eficiência:** Acelera o onboarding de novos membros com guias práticos e executáveis.
*   **Padronização:** Garante conformidade com práticas estabelecidas de desenvolvimento.
*   **Continuidade:** Serve como base sólida para futuras manutenções e evoluções.

Ao combinar código funcional com um assistente técnico persistente, este projeto estabelece um novo padrão para **entrega e transferência de tecnologia em ambientes corporativos**.


Esta versão oferece uma visão abrangente, profissional e tecnicamente precisa do projeto.
