# NEXUS PRIMORDIA — Cowork plugin

Bundle do sistema pessoal NEXUS PRIMORDIA: ciclo de 272 dias, leitura diária, cards 9:16, dispatcher WhatsApp 8x/dia, consultor pessoal, dashboard ao vivo.

**Versão**: 0.1.0
**Autor**: Fillipe Caldeira (`fillipenatan77@gmail.com`)
**Backend**: Google Apps Script (NEXUS_v9.10) — 13 arquivos, 18 rotas /exec, 17 triggers ativos
**Hub**: Notion `3427e2094f0081f997cfc0c675ae8c30`

---

## Skills incluídas

| Skill | Função | Triggers principais |
|-------|--------|---------------------|
| `nexus-status` | Health check + ciclo dia X/272 + estação + lente | "status nexus", "como está o nexus", "qual o dia do ciclo" |
| `nexus-consultor` | Consultor pessoal estruturado (lente do dia + estação + recomendações) | "me aconselhe", "modo consultor", "estou travado" |
| `nexus-pulse` | (Re)cria artefato Cowork ao vivo na sidebar | "abrir pulse", "deploy pulse", "recriar dashboard" |
| `nexus-diario` | Adiciona entrada no Diário Notion | "anota no diário", "registra isso" |

---

## Pré-requisitos

- **Notion connector** habilitado (para todas as skills exceto `nexus-status` no modo offline)
- **Apps Script `/exec` deployado** (URL configurada nas skills) — opcional para `nexus-consultor` (tem fallback local)
- **Conta Google** com acesso aos databases canônicos:
  - Health Check: `0a28322e61ea4d14adaabfa2ffac9bb1`
  - Execuções: `18d1209108774789a4c958018ab32682`
  - Diário: `ecb3886937b34af3a8695750b6af221d`
  - Citações: `8568b5a4505c464abc3734d07dca5340`

---

## Constantes do ciclo PRIMORDIA

```
Início: 2026-04-02
Duração: 272 dias
Fases: Fundação (1-30) · Ignição (31-90) · Meio-ciclo (91-180) · Convergência (181-240) · Selo (241-272)
```

18 estações do palácio em rotação por dia · 7 lentes por dia da semana — detalhes em `skills/nexus-status/SKILL.md`.

---

## Não-bloqueantes conhecidos

- Apps Script API desabilitada no GCP — `v10_backupSourceCode` cria folder/manifest mas não baixa source files. Fix opcional: ativar em https://script.google.com/home/usersettings.
- Cowork Scheduled Tasks: 56 disabled, exclusão definitiva pendente (MCP não expõe `delete`).

---

## Roadmap próximas versões

- **0.2.0**: MCP server stdio expondo /exec routes diretamente (sem precisar URL pública)
- **0.3.0**: Agent `consultor-pessoal-async` para respostas longas em background
- **0.4.0**: Hook `SessionStart` que carrega contexto NEXUS automaticamente
- **1.0.0**: Distribuição via Cowork plugin marketplace (após GitHub host configurado)
