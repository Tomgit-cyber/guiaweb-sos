# Atualizações para o Guia SOS Unimed Vidas

Este repositório contém as atualizações a serem incorporadas ao guia web existente em https://guiaweb-sos.vercel.app/#.

## Conteúdo das Atualizações

1. **Correção do Histórico de Chamadas**
   - Implementação para mostrar apenas chamadas específicas do usuário logado
   - Security Rules restritivas para garantir privacidade
   - Modelo de dados atualizado

2. **Otimizações de Custos**
   - Queries específicas no Firestore
   - Caching local
   - Functions on-demand
   - Rate limit de geolocalização
   - Notificações apenas para eventos críticos
   - Analytics enxuto

3. **Resultados Esperados**
   - Economia de custos estimada
   - Melhorias de privacidade
   - Melhorias de performance
   - Métricas de sucesso

## Estrutura de Arquivos

```
/docs/
├── correcao-historico.md      # Correção do histórico de chamadas
├── otimizacoes-custos.md      # Otimizações de custos
└── resultados-esperados.md    # Resultados esperados
```

## Instruções para Incorporação

Para incorporar estas atualizações ao guia web existente:

1. **Clone o repositório do guia web**:
   ```bash
   git clone https://github.com/Tomgit-cyber/guiaweb-sos.git
   cd guiaweb-sos
   ```

2. **Crie uma nova branch**:
   ```bash
   git checkout -b atualizacao-correcoes-otimizacoes
   ```

3. **Copie os arquivos de atualização**:
   - Copie os arquivos da pasta `/docs` para a pasta correspondente no repositório do guia web
   - Atualize os links de navegação para incluir as novas seções

4. **Faça commit das mudanças**:
   ```bash
   git add .
   git commit -m "Atualização: Correção de histórico por usuário e otimizações de custos"
   ```

5. **Faça push para o repositório**:
   ```bash
   git push origin atualizacao-correcoes-otimizacoes
   ```

6. **Crie um Pull Request** no GitHub para revisão

## Prioridade das Atualizações

1. **Correção Crítica (Privacidade)**:
   - Histórico de Chamadas por Usuário
   - Security Rules restritivas
   - Modelo de dados atualizado

2. **Otimizações de Custo**:
   - Queries específicas Firestore
   - Caching local
   - Functions on-demand
   - Rate limit geolocalização

## Próximos Passos Após Atualização

1. **Deploy automático** pelo Vercel (quando PR for mergeado)
2. **Verificação em**: https://guiaweb-sos.vercel.app/
3. **Teste das correções** no app principal
4. **Monitoramento de consumo** de créditos

