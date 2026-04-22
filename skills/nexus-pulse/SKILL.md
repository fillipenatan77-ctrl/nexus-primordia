---
name: nexus-pulse
description: Cria/recria o artefato "nexus-pulse" — dashboard HTML persistente na sidebar Cowork mostrando ciclo, estação, lente, health, execuções, diário e citação do dia. Use quando o usuário disser "abrir pulse", "deploy do pulse", "recriar dashboard nexus", "pulse não está atualizando", "quero ver o nexus ao vivo".
---

# nexus-pulse

NEXUS Pulse é o artefato Cowork ao vivo do sistema. Esta skill (re)cria ou atualiza esse artefato.

## O que fazer

1. Verificar se artefato `nexus-pulse` já existe via `mcp__cowork__list_artifacts`. Se existir, usar `update_artifact`; se não, `create_artifact`.

2. Estrutura do artefato (HTML autocontido, ~350 linhas):

   **Painéis**:
   - Header: Dia X/272 · Estação · Lente · Fase
   - Health: 5 dots (Calendar/Gmail/Drive/Contacts/WhatsApp) — verde/amarelo/vermelho
   - Execuções: últimas 5 entries do Notion DB
   - Diário: últimas 3 entries
   - Citação do dia: 1 random do banco (95 entries)
   - Síntese: gerada via `window.cowork.sample()` no client

3. **Data sources canônicos** (declarar em `mcp_tools` do artefato):
   ```
   notion-search: mcp__27151919-1780-4f42-816a-26c7570fa67c__notion-search
   notion-fetch:  mcp__27151919-1780-4f42-816a-26c7570fa67c__notion-fetch
   ```

   Collections:
   ```
   health:   collection://0e6b7aad-bee8-45ed-8d2e-4efaf0d10ad8
   execucoes: collection://33d0a034-654c-4836-a722-7ae3b8c4e9e8
   diario:    collection://bb5982a5-4c93-4439-999a-9b3e52d19290
   citacoes:  collection://d84baffd-fd5d-4999-9c0a-52d35901e401
   ```

4. **Cálculos client-side** (deterministic, sem chamadas extras):
   ```js
   const CYCLE_START_ISO = '2026-04-02';
   const CYCLE_LENGTH = 272;
   const ESTACOES = [...18 nomes];
   const LENTES = { 0: '...', 1: '...', ..., 6: '...' }; // 7 lentes por weekday

   function ciclo() {
     const hoje = new Date();
     const inicio = new Date(CYCLE_START_ISO);
     const dias = Math.floor((hoje - inicio) / 86400000) + 1;
     return {
       dia: dias,
       total: CYCLE_LENGTH,
       estacao: ESTACOES[(dias - 1) % 18],
       lente: LENTES[hoje.getDay()],
       fase: dias <= 30 ? 'Fundação' : dias <= 90 ? 'Ignição' : dias <= 180 ? 'Meio-ciclo' : dias <= 240 ? 'Convergência' : 'Selo'
     };
   }
   ```

5. **Render**: chamar refresh on load + button manual. Cache local 60s para evitar throttle Notion.

6. Após criar/atualizar, retornar URL do artefato.

## Quando NÃO recriar

- Se o usuário só quer ver dados, abrir o artefato existente (link).
- Se houver mudança apenas em data sources canônicas, fazer apenas `update_artifact` com diff mínimo.

## Observações

- Artefato persiste entre sessões; sobrevive a `/clear`.
- Usuário precisa estar logado no Notion connector pra renderizar dados; sem login, painéis mostram "—" (graceful degradation).
- Não armazenar credenciais em `localStorage` (proibido em artefatos Cowork).
