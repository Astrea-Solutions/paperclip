# Org Chart — Astrea Solutions

**Company Prefix:** AST  
**Version:** 1.0  
**Updated:** 2026-05-12  
**Responsible:** COO

---

## Chart Overview

```
                          CEO (Humano/Agente)
                                |
        +-----------------------+-----------------------+
        |                       |                       |
      COO                     CTO                    Advisor
   Operations            Technology/Engineering      Strategy
        |                       |
  +-----+-----+           +-----+-----+
  |     |     |           |     |     |
 Dev   QA    DevOps    Frontend Backend Infra
```

## Roles E Agents

### C-Level

| Role | Agent ID | Adapter Type | Status | Budget (USD/mo) | Notes |
|---|---|---|---|---|---|
| CEO | — | — | — | — | Strategic direction, approvals, board decisions |
| COO | ad593325-8c26-4f2a-a517-372356622397 | openclaw_gateway | Active | TBD | Operations, coordination, reporting |
| CTO | — | — | TBD | TBD | Architecture, code review, technical escalation |

### Technical Team

| Role | Agent ID | Specialization | Status | Budget (USD/mo) |
|---|---|---|---|---|
| Senior Developer | TBD | Full-stack, Next.js, React | TBD | TBD |
| QA Agent | TBD | Playwright, regression, smoke | TBD | TBD |
| DevOps Agent | TBD | Docker, CI/CD, infra | TBD | TBD |
| Frontend Agent | TBD | UI/UX, Tailwind, shadcn/ui | TBD | TBD |
| Backend Agent | TBD | API, Drizzle, PostgreSQL | TBD | TBD |

### Gaps De Cobertura Identificados

1. **CTO não contratado:** Sem agente tecnico senior para escalar questoes arquiteturais e review de PRs complexas.
2. **QA ausente:** Sem agente dedicado a testes automatizados e regressoes.
3. **DevOps ausente:** Sem agente dedicado a infraestrutura, deploy e monitoramento.
4. **Budget tracking:** Orçamentos por agente não estão definidos.

## Hiring Pipeline

| Priority | Role | Rationale | ETA |
|---|---|---|---|
| P0 | CTO | Necessario para escalar review tecnico e arquitetura | ASAP |
| P1 | QA Agent | Qualidade de entrega precisa de cobertura automatica | +1 semana |
| P1 | DevOps Agent | Automação de deploy e monitoramento | +2 semanas |
| P2 | Frontend Specialist | Profundidade em UI/design para produto | +1 mes |
| P2 | Backend Specialist | Escalabilidade do API e banco | +1 mes |

### Processo De Contratacao

1. CEO define requisitos e role description
2. COO prepara `AGENTS.md` baseado no template (use `paperclip-create-agent` skill)
3. Board aprova via `request_board_approval`
4. COO contrata via `POST /api/companies/{id}/agents` com `desiredSkills`
5. Onboarding com skill assignment e primeira issue de warm-up

## Responsabilidades Por Funcao

### CEO
- Define `GOAL.md`, `PRODUCT.md`, `SPEC.md`
- Aprova apropriacoes de orçamento (> threshold)
- Decide prioridades estrategicas
- Board-level approvals

### COO
- Monitorar dashboard diariamente
- Detectar issues stalled, bloqueios e run failures
- Escalar para CTO/CEO quando necessario
- Consolidar reports semanais
- Gerenciar orçamento de agentes
- Configurar e manter rotinas/heartbeats

### CTO
- Review tecnico de PRs criticos
- Arquitetura e decisoes de design de sistemas
- Escalar questoes de seguranca e performance
- Mentoria tecnica dos agents juniors

### Developers
- Checkout de issues, execucao, atualizacao de status
- Criar child issues para trabalhos paralelos
- Escrever tests para features implementadas
- Reportar bugs com repro minimal

### QA
- Executar suite de testes em regressoes
- Reportar failures com logs e evidencias
- Verificar criterios de aceitacao nas issues

### DevOps
- Monitorar saude da infraestrutura
- Gerenciar builds, deploys e ambientes
- Configurar alertas e logs agregados

## Budget Per Role

| Role | Est. Tokens/mo | Est. Cost (USD) | Notes |
|---|---|---|---|
| CEO | ~0 (manual) | $0 | Human-driven |
| COO | ~500K | ~$15 | Monitoring + coordination |
| CTO | ~2M | ~$60 | Deep code review + architecture |
| Developer | ~5M | ~$150 | Full-time coding |
| QA | ~2M | ~$60 | Test automation |
| DevOps | ~1M | ~$30 | Infra automation |
| Total | ~10.5M | ~$315/mo | Baseline |

---

*Este org chart deve ser revisado a cada sprint (14 dias) pelo COO e aprovado pelo CEO.*
