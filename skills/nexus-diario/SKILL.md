---
name: nexus-diario
description: Adiciona entrada no Diário NEXUS no Notion (database ecb3886937b34af3a8695750b6af221d). Use quando o usuário disser "anota no diário", "registra isso", "adiciona ao diário do nexus", "abre uma entrada no diário", ou compartilhar reflexão/insight/citação que mereça registro.
---

# nexus-diario

Append-only journal do NEXUS. Cada entrada é uma reflexão diária, citação encontrada, insight de consultoria, ou observação de campo.

## O que fazer

1. Coletar (do contexto da conversa ou perguntar UMA vez):
   - **Texto** (obrigatório): o conteúdo da entrada
   - **Slot** (opcional): morning / afternoon / night / liturgia / psicanalise / lideranca / memorizacao / motivacional / memória
   - **Quote** (opcional): se for uma citação, autor da citação
   - **Tags** (opcional, multi): livre

2. Criar página no Notion via `mcp__27151919-1780-4f42-816a-26c7570fa67c__notion-create-pages`:
   - Database: `collection://bb5982a5-4c93-4439-999a-9b3e52d19290`
   - Properties:
     - `Name` (title): primeiras 60 chars do texto + " · Dia X/272"
     - `Slot` (select): se fornecido
     - `Quote` (rich_text): se citação
     - `Author` (rich_text): se citação
     - `Cycle Day` (number): dia atual do ciclo PRIMORDIA (calcular)
     - `Tags` (multi_select): se fornecidas
   - Content (children blocks): texto completo como paragraph(s)

3. Calcular `Cycle Day`:
   ```
   const inicio = new Date('2026-04-02');
   const hoje = new Date();
   const dia = Math.floor((hoje - inicio) / 86400000) + 1;
   ```

4. Confirmar ao usuário com link da entrada criada.

## Tom

- Não editorializar o texto do usuário; copiar literal.
- Se texto contém múltiplos parágrafos, preservar quebras (criar um block paragraph por parágrafo).
- Se texto é citação clássica, sugerir também adicionar à database `Citações` (`d84baffd-fd5d-4999-9c0a-52d35901e401`) — mas só sugerir, não duplicar automaticamente.

## Anti-padrões

- ❌ Criar entrada sem texto explícito
- ❌ Resumir/parafrasear texto do usuário
- ❌ Adicionar tags inferidas sem confirmação
- ❌ Substituir texto por interpretação
