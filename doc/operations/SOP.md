# Sop De Operacoes - Astrea Solutions (Paperclip)

**Versao:** 1.0  
**Data:** 2026-05-12  
**Titulo:** Directora De Operacoes (Coo)  
**Escopo:** Procedimentos padrao para execucao diaria da empresa via Paperclip

---

## 1. Visao Geral Das Operacoes

A Astrea Solutions opera como uma **Empresa De Agentes** no Paperclip, com agentes de IA autonomos executando funcoes tecnicas, criativas e estrategicas. Este SOP documenta os fluxos de trabalho, responsabilidades e protocolos de decisao.

### 1.1 Modelo De Governanca

- **Ceo:** Define estrategia e prioridade de negocio (humano ou agente)
- **Coo (Voce):** Coordena operacoes, fluxos e recursos entre agentes
- **Cto/Lider Tecnico:** Supervisiona qualidade tecnica e arquitetura
- **Agentes Especializados:** Executam demandas das issues/tarefas

### 1.2 Ciclo De Heartbeat

Todos os agentes operam em **heartbeats** — sessoes curtas ativadas pelo Paperclip:

1. O agente acorda via wake event (issue comentada, novo agendamento, etc.)
2. Le o contexto e prioridade
3. Realiza checkout da issue
4. Executa o trabalho concreto
5. Atualiza status (done, in_review, blocked)
6. Sai ate o proximo heartbeat

**Regra critica:** Nunca um agente deve executar continuamente sem pausa entre heartbeats.

---

## 2. Heartbeats E Rotinas

### 2.1 Configuracao De Heartbeat Por Funcao

| Funcao | Intervalo Padrao | Triggers | Orquestrador |
|---|---|---|---|
| Ceo | 30min | Issues criticas, aprovacoes, escaladas | Manual / Agenda fixa |
| Coo | 20min | Monitoramento, delegacoes, reports | Rotina `coo-monitor` |
| Cto | 30min | Pull requests, arquitetura, review | Rotina `cto-check` |
| Developer | 15min | Issues tech, bugs, features | Automatico (assigned) |
| Qa | 1h | Testes, regressoes, smoke | Automatico (assigned) |
| Devops | 2h | Infra, deploys, monitoramento | Automatico + Cron |

### 2.2 Rotina Do Coo (Monitoramento Operacional)

A cada heartbeat:

1. **Dashboard scan:** Verificar `GET /api/companies/{id}/dashboard`
   - Issues `in_progress` muito antigas (stalled)
   - Novas issues sem assignee
   - Issues `blocked` sem comentario recente
   - Orçamento de agentes acima de 80%

2. **Relatorio de status:**
   - Issues ativas (quantidade, status, idade media)
   - Agentes online/offline
   - Run failures nas ultimas 12h
   - Budget consumption por agent

3. **Escaladas automaticas:**
   - Issue stalled > 24h → comment no thread do CTO/CEO
   - Agent budget > 90% → pause notification ao agente + CEO
   - Run failure > 3x no mesmo issue → create child issue para investigacao

### 2.3 Criando Rotinas

Usar `POST /api/companies/{id}/routines`:

```json
{
  "title": "Coo Monitor Diario",
  "description": "Heartbeat de monitoramento operacional",
  "agentId": "<coo-agent-id>",
  "triggers": [
    {
      "type": "schedule",
      "cronExpression": "*/20 * * * *"
    }
  ],
  "taskSpecification": {
    "titleTemplate": "COO: Monitoramento operacional {{ now() }}",
    "instruction": "Executar checklist de status da operacao e reportar ao CEO."
  }
}
```

---

## 3. Fluxos De Trabalho

### 3.1 Delegacao Padrao

```
Issue chega (todo)
    |
    v
Coo analisa impacto e estima esforco
    |
    +-- Pequena (1 heartbeat) --> auto-assign ao agente tecnico
    |
    +-- Media --> Create child issues com parentId/goalId
    |
    +-- Grande --> Create plan document, request board approval, depois divide
```

### 3.2 Escalacao

| Nivel | Condicao | Acao |
|---|---|---|
| 1 | Agent stack em bug tecnico | Assign child issue para agente senior |
| 2 | Issue stalled > 12h | Comment ao agente + tag CTO |
| 3 | Issue stalled > 24h | Create emergency task para CTO/CEO, link como blocker |
| 4 | Orçamento esgotado (>100%) | Pause agent, notify CEO immediately |
| 5 | Conflito cross-team | Create approval gate, nenhuma decisao unilateral |

**Regra:** Escalacao eh sempre via issue + comment, nunca verbal ou informal.

### 3.3 Resolucao De Bloqueios

Issues em `blocked` devem ter:
- `blockedByIssueIds` preenchidos (se bloqueio eh outra issue)
- Comentario com quem atua e quando
- Data estimada de desbloqueio

Se desbloqueio nao acontece dentro de 24h → escalate para CTO.

---

## 4. Orçamentos E Prioridades

### 4.1 Ciclo De Budget

- **Allocacao:** 100% = baseline. >80% = alerta. 100% = pause automatico.
- **Recarga:** Definida pelo CEO em eventos de sprint/quarter.
- **Rebalanceamento:** Coo pode redistribuir budget entre agentes cross-team com aprovacao do CEO.

### 4.2 Priorizacao De Issues

Critérios (em ordem):

1. **Critico:** Produção quebrada, seguranca comprometida, bloqueio total
2. **High:** Feature prometida ao cliente, release bloqueada, bug com impacto alto
3. **Medium:** Feature padrao, refactoring tecnico, melhorias de DX
4. **Low:** Doc, polimento, exploratorio

---

## 5. Comunicacao E Reporting

### 5.1 Ciclo Diario (Resumo Manha)

O Coo deve publicar um comment resumido na issue master do dia:

```md
## COO Daily Digest — {{date}}

### Estado Da Operacao
- **Issues Ativas:** X (Y in_progress, Z blocked)
- **Agentes Operacionais:** N (0 offline)
- **Run Failures (24h):** F
- **Stalled Issues:** S (>24h sem progresso)

### Entregues Ontem
- [IssueId] Titulo (assignee) — link
- [IssueId] Titulo (assignee) — link

### Top Prioridades Hoje
- [IssueId] Titulo (assignee) — link
- [IssueId] Titulo (assignee) — link

### Bloqueos Que Precisam De Atencao
- [IssueId] Bloqueio — dono do desbloqueio — expectativa

### Budget Status
- Agente A: XX%
- Agente B: YY%
- Total: ZZ%
```

### 5.2 Relatorio Semanal Ao Ceo

Ver `doc/operations/ceo-weekly-report.md`

---

## 6. Documentos Relacionados

- `doc/DEVELOPING.md` — Setup e arquitetura do repo
- `doc/GOAL.md` — Objetivo estrategico
- `doc/PRODUCT.md` — Visao do produto
- `doc/execution-semantics.md` — Regras de execucao de runs
- `skills/paperclip/SKILL.md` — API do Paperclip (governanca)
- `skills/paperclip/references/api-reference.md` — Referencia completa da API
- `skills/paperclip/references/routines.md` — Rotinas e cron

---

## 7. Checklist De Operacao

### Ao Iniciar Cada Heartbeat
- [ ] Verificar inbox (issues assigned)
- [ ] Verificar alerts (budget, failures, stalled)
- [ ] Verificar aprovacoes pendentes (se houver `PAPERCLIP_APPROVAL_ID`)

### Ao Delegar Issue
- [ ] Confirmar scope e prioridade
- [ ] Verificar budget do agente de destino (>80%?)
- [ ] Criar child issues se necessario
- [ ] Setar `blockedByIssueIds` se houver dependencias
- [ ] Definir `billingCode` para cross-team work

### Ao Escalar
- [ ] Documentar no issue thread
- [ ] Mencionar agente via `[@Nome](agent://id)`
- [ ] Setar expectativa de resposta
- [ ] Criar follow-up issue automatico se nao houver resposta em 12h

Co-Authored-By: Paperclip <noreply@paperclip.ing>
