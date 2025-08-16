# Documento Completo de Proposta Técnica e Estrutura de Desenvolvimento - Projeto SOS Vidas (Unimed Sul Capixaba)

## 1. Visão Geral do Projeto

O **SOS Vidas** é um aplicativo estratégico voltado à prestação de serviços de suporte emergencial, prevenção e acompanhamento de saúde sob gestão da **Unimed Sul Capixaba**. O projeto busca integrar atendimento rápido, ferramentas digitais e relatórios de eficiência, garantindo rastreabilidade e escalabilidade futura.

### Objetivos Principais:

* Disponibilizar suporte digital emergencial para beneficiários.
* Integrar painel de gestão (dashboard) com indicadores atualizados.
* Incluir módulos responsivos de guia passo a passo e guia técnico.
* Permitir expansão futura via APIs e backend.
* Viabilizar relatórios dinâmicos para equipe interna.

---

## 2. Estrutura do Projeto

### 2.1. Infraestrutura (não inclusa no MVP)

* Servidores cloud (AWS, GCP ou Azure).
* Balanceador de carga.
* Certificados SSL/TLS.
* Banco de dados (PostgreSQL recomendado).
* Ferramentas de monitoramento (Grafana, Prometheus).

⚠️ Importante: a **infraestrutura física e cloud não faz parte do MVP** (valor solicitado), sendo de responsabilidade da Unimed.

### 2.2. Frontend

* Framework base: **Flutter** (mobile multiplataforma).
* Design responsivo.
* Layout adaptado para tablets e smartphones.
* Identidade visual Unimed (verde/azul alternáveis).

### 2.3. Backend / Integrações (fase posterior)

* Node.js (preferencial) ou Python/Django.
* Integração futura com API interna da Unimed.
* Autenticação via Firebase.
* Notificações push.

---

## 3. Funcionalidades Principais do MVP

1. **Painel SOS Vidas**:

   * Aba "Guia do Aplicativo" (passo a passo completo).
   * Aba "Guia Técnico" (fases, tarefas, ferramentas e boas práticas).
   * Alternância de cores (verde/azul Unimed).
   * Filtro e botão de recarregar informações.

2. **Guia Passo a Passo**:

   * Ciclo de fases do desenvolvimento.
   * Checklists interativos.
   * Indicação de status (feito/em andamento/pendente).

3. **Guia Técnico**:

   * Estrutura responsiva.
   * Divisão em camadas (infra, backend, frontend, API, relatórios).
   * Exemplos e boas práticas.

4. **Relatórios Internos**:

   * Geração de relatórios semanais automáticos.
   * Exportação em PDF e Excel.

---

## 4. Fluxo de Desenvolvimento

### Fase 1 - Preparação

* Definição do repositório (GitHub privado).
* Criação do projeto Flutter.
* Estruturação inicial do guia passo a passo e técnico.

### Fase 2 - MVP Funcional

* Implementação do painel SOS Vidas.
* Integração do guia responsivo.
* Inclusão dos relatórios básicos.

### Fase 3 - Expansão

* API interna.
* Notificações push.
* Dashboards avançados.

### Fase 4 - Escalabilidade

* Migração para cloud Unimed.
* Monitoramento em tempo real.

---

## 5. Divisão de Receita

* **70% Unimed Sul Capixaba**
* **30% Proponentes/Desenvolvedores**
* Meio de pagamento: **PIX dedicado**

---

## 6. Prazo e Entregas

* **Entrega do MVP:** até 30 dias após assinatura.
* Entregas parciais semanais.
* Relatórios de progresso em tempo real.

---

## 7. Boas Práticas Adotadas

* Clean Architecture no Flutter.
* Documentação detalhada de cada fase.
* Uso de testes unitários e de integração.
* Código versionado em repositório Git.
* Controle de acesso com autenticação segura.

---

## 8. Próximos Passos

1. Aprovação formal desta proposta.
2. Criação do repositório Git privado.
3. Definição do cronograma detalhado.
4. Início do desenvolvimento fase 1.

---

📌 **Nota Final:** Este documento está no **nível máximo de detalhamento técnico inicial** possível sem iniciar a implementação. A partir daqui, a evolução dependerá de ajustes em conjunto com a equipe técnica da Unimed de Cachoeiro do Itapemirim.
