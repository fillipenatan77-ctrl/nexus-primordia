---
name: nexus-status
description: Verifica saúde do sistema NEXUS — versão deployada, triggers ativos, status de backups (source/snapshot/diário/props), idempotência, ciclo PRIMORDIA dia X/272. Use quando o usuário pedir "status nexus", "como está o nexus", "saúde do sistema", "diagnóstico nexus", "qual o dia do ciclo", "o nexus está rodando?".
---

# nexus-status

Diagnóstico vivo do NEXUS PRIMORDIA. Combina rotas /exec deployadas + cálculo local do ciclo + leitura de Notion Health Check DB.

## O que fazer

1. Calcular dia atual do ciclo PRIMORDIA:
   - Início: `2026-04-02`
   - Duração: 272 dias
   - Dia atual = `floor((hoje - início) / 86400000) + 1`
   - Reportar: `Dia X/272 · Estação Y · Lente Z`

2. Buscar status operacional via Notion Health Check DB:
   - Data source: `collection://0e6b7aad-bee8-45ed-8d2e-4efaf0d10ad8`
   - Última checagem (mais recente por `Data`)
   - Reportar 5 sinais: Calendar / Gmail / Drive / Contacts / WhatsApp

3. Buscar últimas 5 execuções via Notion Execuções DB:
   - Data source: `collection://33d0a034-654c-4836-a722-7ae3b8c4e9e8`
   - Reportar: `Tipo · Status · Timestamp`

4. Health detalhado (opcional, sob pedido explícito) — chamar /exec:
   - `https://script.google.com/.../exec?action=health_full`
   - `https://script.google.com/.../exec?action=cards_health`
   - `https://script.google.com/.../exec?action=v14_backup_status`
   - `https://script.google.com/.../exec?action=v14_idempotency_audit`

## Output

Resposta direta em pt-BR (preferência do usuário: "Manter explicações objetivas e diretas"):

```
NEXUS Dia X/272 · <Estação> · <Lente do dia>

Backups: source ✅ · snapshot ✅ · diário ❌ · props ✅ (3/4 = 75%)
Idempotência: 0 stale, 0 duplicates, 0 recomendações
Triggers: 17 ativos · próximo dispatch <HHhMM>

Última execução: <tipo> · <status> · <timestamp>
```

Se algum sinal estiver vermelho (status ≠ "ok"), expandir com causa raiz e ação sugerida.

## Estações do Palácio (rotação por dia do ciclo)

`['Fundação','Liminaridade','Fogo Interior','Reflexão Estratégica','Ágora','Forja','Lapidário','Coroa Tríplice','Espelho','Bússola Inversa','Templo do Eixo','Vau','Tessitura','Selo Quaternário','Acrópole','Catedra','Convergência','Selo Final']`

índice = `(diaCiclo - 1) % 18`

## Lentes (rotação por dia da semana)

```
Domingo: Síntese Dominical (integrar a semana)
Segunda: Liderança (Jung + Sun Tzu)
Terça: PNL e Linguagem (Bandler/Grinder + Milton Erickson)
Quarta: Filosofia Estoica (Marco Aurélio + Sêneca)
Quinta: Psicanálise (Freud + Lacan)
Sexta: Memorização (Palácio + ancoragens)
Sábado: Motivacional (Shakespeare pt-BR + clássicos)
```
