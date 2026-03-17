# Iron Cave - Planificação

> Atualizada a 17/mar

## 1) Visão Geral do Plano

> **Projeto:** Iron Cave App  
> **Versão do plano:** v1.0  
> **Data da última macro:** 07 de março de 2026  
> **Horizonte:** 3 semanas desde a última macro
> **Objetivo:** fechar frontend + fechar backend + preparar go-live

---

## 2) Estado Atual (base de estimativa)

### Frontend

- Base grande de páginas e componentes (multi-role: client, trainer, nutritionist, admin).
- Serviços FE com base Axios, TanStack Query e padrões de segurança já definidos.
- Parte da integração ainda usa mock em domínios específicos.

### Backend

- Contratos/documentação muito completos (API, modelos, segurança, erros, papéis/permissões).
- Implementação efetiva.

---

## 3) Macrofases (3 semanas)

### Macro 1 — Fundação e Segurança Base (5 dias)

Objetivo: certificar que o backend está com padrões corretos de autenticação, autorização, sessão, CSRF, observabilidade e CI.

### Macro 2 — Core de Domínio (5 dias)

Objetivo: finalizar o núcleo funcional da app (treino, nutrição, agenda, progresso), com APIs e fluxos principais operacionais.

### Macro 3 — Comunicação e Operação (5 dias)

Objetivo: finalizar fluxos de chat, notificações, admin, search/help e capacidades transversais para operação diária.

### Macro 4 — Integração Final e Release (5 semanas)

Objetivo: eliminar mocks remanescentes, fechar E2E, hardening final, correções críticas e go-live.

---

## 4) Sprints Detalhados

## Sprint 1

**Foco:** bootstrap backend + auth/security base.

**Entregáveis principais**

- Estrutura backend (app, módulos, configs, logging, erro canónico).
- JWT/cookies HttpOnly (`ic_access`, `ic_refresh`) + refresh flow.
- CSRF bootstrap (`GET /api/csrf-token`) + validação em mutações.
- `X-Request-Id` em request/response e correlação básica de logs.
- CI inicial (lint/test/build).

**DoD Sprint 1**

- Login/logout/refresh funcionais em ambiente local.
- Mutações sem CSRF falham corretamente.
- Pipeline CI bloqueia merge em falha.

---

## Sprint 2

**Foco:** utilizadores, perfis de acesso e chaves de registo.

**Entregáveis principais**

- RBAC/ABAC base com action slugs.
- Fluxos de registo (open e key mode).
- Endpoints de registration keys (create/list/revoke/send-email/recreate/validate).
- Guardrails de ownership.

**DoD Sprint 2**

- Profissionais só gerem as suas chaves.
- Cliente regista com chave válida e recebe sessão.
- Cobertura mínima de testes para guards críticos.

---

## Sprint 3

**Foco:** domínio treino (catálogo e planos).

**Entregáveis principais**

- CRUD exercícios.
- CRUD planos de treino.
- Atribuição de plano a cliente(s) com validações de ownership e estado.
- DTOs e paginação/filtros base.

**DoD Sprint 3**

- PT cria/edita/atribui planos sem bypass de permissões.
- FE consome endpoints reais deste domínio (sem mock no core de planos).

---

## Sprint 4

**Foco:** execução e progresso de treino.

**Entregáveis principais**

- Endpoints de histórico/detalhe de treinos.
- Registo de progresso/sessões de treino.
- Sumários para dashboard cliente (treino).
- Idempotency em POST críticos de progresso.

**DoD Sprint 4**

- `WorkoutsHistoryPage` e `WorkoutDetailPage` com dados reais.
- Repetição de POST idempotente não duplica efeitos.

---

## Sprint 5

**Foco:** domínio nutrição (bibliotecas e planos).

**Entregáveis principais**

- CRUD foods.
- CRUD recipes.
- CRUD mealplans + assign.
- Contratos de progresso nutricional.

**DoD Sprint 5**

- Nutricionista cria e atribui plano nutricional end-to-end.
- FE de nutrição principal sem dependência de mock no core.

---

## Sprint 6

**Foco:** hidratação + agenda/sessions/availability.

**Entregáveis principais**

- `/api/hydration/today`, `/api/hydration`, `/api/hydration/quick-add`.
- CRUD/fluxos de sessões (create/update/cancel/confirm).
- Availability e bloqueios/exceções de agenda.
- Regras de estado (pending/confirmed/cancelled/etc).

**DoD Sprint 6**

- Fluxo de agenda operacional para client/PT/nutritionist.
- Regras de transição de estado protegidas por teste.

---

## Sprint 7

**Foco:** comunicação (chat + notificações + websocket).

**Entregáveis principais**

- Threads/mensagens/read/upload attachment (com validações).
- Notificações (listar/read/read-all/unread-count).
- Handshake WS + allow-list de eventos + rate limits.

**DoD Sprint 7**

- Unread counts atualizam com consistência.
- Eventos WS rejeitam ações sem autorização.

---

## Sprint 8

**Foco:** profile/security/search/help/admin APIs.

**Entregáveis principais**

- Profile client/pro (GET/PATCH).
- Security sessions (list/revoke/revoke-all) + change password.
- Search global + help endpoints essenciais.
- Endpoints admin prioritários (users/notifications/analytics base).

**DoD Sprint 8**

- Painéis admin e profile com dados reais mínimos.
- Sessões ativas e revogação funcional por utilizador.

---

## Sprint 9

**Foco:** integração massiva FE <-> BE e remoção de mocks.

**Entregáveis principais**

- Troca sistemática de mocks por APIs reais nas páginas prioritárias.
- Alinhamento final de DTOs/envelopes/erros.
- Telemetria funcional ponta-a-ponta com `X-Request-Id`.
- Correção de regressões de contrato.

**DoD Sprint 9**

- Fluxos principais E2E por role sem fallback mock.
- Taxa de erro funcional abaixo do limite acordado (ver secção 8).

---

## Sprint 10

**Foco:** hardening final e release.

**Entregáveis principais**

- Testes E2E críticos por role e por domínio.
- Revisão de segurança (CSRF/CORS/RBAC/ABAC/rate limit/logs sensíveis).
- Performance pass (queries quentes, endpoints lentos, payloads grandes).
- Bugfixes críticos e checklist de go-live.

**DoD Sprint 10**

- Critérios de release cumpridos.
- Go-live aprovado.

---

## 5) Backlog Prioritário (P0/P1/P2)

### P0 (obrigatório para release)

- Auth/sessão/cookies/CSRF.
- RBAC/ABAC e ownership.
- Core treino, nutrição, agenda, hidratação.
- Chat básico + notificações básicas.
- Integração FE sem mocks nos fluxos críticos.
- E2E críticos por role.

### P1 (fortemente recomendado para release robusto)

- Admin analytics refinado.
- Search com ranking refinado.
- Help contextual completo.
- Observabilidade avançada e dashboards técnicos.

### P2 (pós-release ou se houver folga)

- Otimizações avançadas de UX/performance.
- Expansões de IA e automações secundárias.
- Funcionalidades não críticas de conveniência.

---

## 6) Definition of Done Global

Uma funcionalidade só está “Done” quando:

1. Endpoint/fluxo implementado e integrado no frontend.
2. Regras de segurança aplicadas (auth, permissão, ownership, CSRF quando aplicável).
3. Erros canónicos e envelopes corretos.
4. Logs/telemetria mínimos disponíveis.
5. Testes mínimos (unit + integration + smoke E2E) a passar.
6. Sem regressão visível nos fluxos já estáveis.

---

## 7) Métricas de Controlo Semanal

### Entrega

- `% de endpoints P0 implementados`.
- `% de páginas P0 sem mock`.
- `Lead time médio por PR`.

### Qualidade

- `Taxa de falhas CI`.
- `Bugs críticos abertos por sprint`.
- `Tempo médio de correção de regressão`.

### Segurança/Confiabilidade

- `% de mutações com CSRF validado`.
- `% de endpoints com checks de autorização testados`.
- `Taxa de erro 5xx nos ambientes de teste`.

### Target recomendado

- Até fim da Sprint 6: >= 60% P0 concluído.
- Até fim da Sprint 8: >= 90% P0 concluído.
- Sprint 10: 0 bugs críticos bloqueantes para release.

---

## 8) Riscos e Mitigação

### Risco 1 — Scope creep semanal

- **Impacto:** quebra imediata do plano agressivo.
- **Mitigação:** freeze semanal + “trade-off explícito” (entra novo item, sai item equivalente).

### Risco 2 — Conflitos entre trabalho paralelo

- **Impacto:** retrabalho e atrasos de integração.
- **Mitigação:** dividir ownership por módulo/ficheiro e fazer merge frequente.

### Risco 3 — Segurança tratada tarde

- **Impacto:** regressões graves perto do release.
- **Mitigação:** security gates por sprint, não só no final.

### Risco 4 — Dependências externas (email/storage/etc)

- **Impacto:** bloqueios técnicos inesperados.
- **Mitigação:** feature flags + fallback local + mocks controlados apenas para infra externa.

### Risco 5 — Volume de testes insuficiente

- **Impacto:** instabilidade no go-live.
- **Mitigação:** cobertura mínima obrigatória por domínio antes de declarar “Done”.

---

## 9) Plano de Contingência (se escorregar)

Se houver derrapagem > 1 sprint:

1. Congelar P2 imediatamente.
2. Repriorizar P1 para pós-go-live.
3. Manter release com escopo P0 + hardening mínimo.
4. Criar mini-release de estabilização 1 semana após go-live.
