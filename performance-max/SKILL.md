---
name: performance-max
description: Diagnostica gargalos de performance em uma aplicacao Next.js (frontend) e NestJS (backend) e gera um plano de otimizacao estrategico e priorizado, salvo em docs/performance/. Use esta skill sempre que o usuario quiser deixar alguma parte do app mais rapida, leve ou responsiva, por exemplo melhore a performance do chat, essa rota ta lenta, reduzir o tempo de carregamento da pagina X, otimizar o checkout, Performance-Max no dashboard, ou qualquer pedido sobre latencia, tamanho de bundle, queries lentas, TTFB, render ou hidratacao, ou escalar uma feature especifica. Acione mesmo quando o usuario so nomeia uma area e diz que esta lenta, sem usar a palavra performance. NAO use para construir features novas do zero, refatoracoes genericas sem relacao com velocidade, nem para corrigir bugs funcionais.
---

# Performance-Max

Skill de diagnóstico e planejamento de performance para apps **Next.js + NestJS**.

A filosofia é simples e importa para tudo abaixo:

- **Foco no alvo, não no projeto inteiro.** O usuário sempre nomeia uma área ("o chat", "o checkout", "essa rota"). Você investiga *aquilo*, não varre o repositório todo. Isso mantém o plano útil e executável em vez de virar um relatório genérico.
- **Você planeja, não executa.** O resultado é um documento de estratégia. O usuário implementa depois (e usará a skill apropriada na hora — frontend-design, n8n-*, etc.). Por isso o plano é **descritivo**: diz *o quê* e *por quê*, não despeja blocos de código.
- **Fundamentado no código real.** As recomendações saem do que você *leu* no repositório, com evidência, e não de boas práticas soltas. Um plano genérico que serviria para qualquer app é um plano ruim.

## Fluxo geral

1. Entenda o alvo (vem do comando de acionamento).
2. Faça as perguntas iniciais — só as que ainda não foram respondidas.
3. Leia e analise o código do alvo. Detecte o stack real.
4. Diagnostique os gargalos, com evidência.
5. Monte o plano priorizado por impacto × esforço.
6. Salve em `docs/performance/per-<nome>.md` e mostre ao usuário.

---

## Passo 1 — Entenda o alvo

O acionamento quase sempre já contém a área ("melhore a parte de chat"). Use isso como escopo. Se o pedido vier vago demais ("o app tá lento") e não der pra delimitar um alvo, peça que a pessoa aponte qual parte priorizar — otimizar tudo de uma vez não cabe em um plano focado.

## Passo 2 — Perguntas iniciais

Antes de analisar, levante quatro coisas. **Não re-pergunte o que já estiver claro no comando** — se a pessoa já disse o sintoma, não pergunte de novo; siga para o que falta. O objetivo aqui é direcionar a análise, não burocratizar.

1. **Arquivos/rotas específicas** para olhar primeiro (onde a pessoa desconfia que está o problema).
2. **Sintoma atual** — o que está lento ou pesado hoje, e em que situação aparece.
3. **Meta / métrica-alvo** (opcional) — ex.: "carregar em menos de 1s", "p95 abaixo de 300ms". Se houver meta, ela vira a base da seção de critérios de sucesso.
4. **Restrições** — o que não pode quebrar ou mudar (contratos de API, libs travadas, prazos, áreas intocáveis).

Faça as perguntas de forma enxuta, idealmente de uma vez. A pessoa pode pular a meta — tudo bem, nesse caso o plano sai sem a seção de critérios de sucesso.

## Passo 3 — Analise o código do alvo

Leia os arquivos da área indicada. **Detecte o stack pelo próprio código** em vez de assumir — projetos variam:

- Frontend: App Router ou Pages Router? Quais componentes são server/client? Como os dados são buscados? Versão do Next (cheque o `package.json`, pois recursos e defaults mudam entre versões).
- Backend: qual ORM/data layer (Prisma, TypeORM, Mongoose, query bruto)? REST ou GraphQL? Onde estão as chamadas a banco e a serviços externos?

Para saber **o que procurar** em cada camada, consulte os checklists:

- `references/nextjs.md` — alavancas de performance de frontend.
- `references/nestjs.md` — alavancas de performance de backend.

Leia o arquivo relevante à camada que você está tocando. Em alvos full-stack, leia os dois.

## Passo 4 — Diagnóstico

Liste os gargalos que você efetivamente encontrou, por camada, **citando a evidência** (arquivo, função, padrão observado). Se algo for suspeita e não certeza, diga que é suspeita e o que confirmaria. Honestidade sobre incerteza vale mais que um diagnóstico confiante e errado.

## Passo 5 — Monte o plano

Use o **template do plano** abaixo. Regras que não mudam:

- **Baseline sempre.** Toda otimização precisa de um "antes" mensurável, senão não dá pra saber se funcionou. Indique métrica e ferramenta concretas para medir antes e depois.
- **Priorize por impacto × esforço**, quick wins primeiro. A pessoa deve conseguir começar pelo que dá mais retorno com menos trabalho.
- **Descritivo, sem blocos de código.** Explique a mudança e o motivo; o código nasce na execução.
- **Cada recomendação aponta a skill de execução** (ex.: frontend-design para UI/componentes, n8n-* para workflows). Isso conecta o plano à etapa seguinte.
- **Critérios de sucesso só quando houver meta.** Sem meta declarada, omita a seção 6.

## Passo 6 — Salve o plano

Salve em `docs/performance/` (crie a pasta se não existir), com o nome `per-<nome-do-plano>.md` em kebab-case — ex.: `per-otimizacao-chat.md`. O prefixo `per-` identifica os planos desta skill de relance.

Confirme o caminho final ao usuário e ofereça um resumo curto do que o plano prioriza. Lembre que o plano é uma proposta — você não executou nada.

---

## Template do plano

Gere em PT-BR, seguindo esta estrutura:

```markdown
# [Título do plano — ex.: Otimização de performance do chat]

> Gerado pela skill Performance-Max em [data]
> Status: proposta (não executado)

## 1. Alvo e escopo
- Área: [...]
- Arquivos/rotas no escopo: [...]
- Fora do escopo: [...]

## 2. Contexto
- Sintoma relatado: [...]
- Meta: [métrica-alvo, ou "não definida"]
- Restrições / o que não pode mudar: [...]

## 3. Baseline e como medir
Como capturar o "antes" e comparar com o "depois". Liste métrica + ferramenta.
Frontend (quando aplicável): LCP, INP, CLS, TTFB via Lighthouse/Web Vitals; tamanho de
bundle via analyzer; renders via React Profiler.
Backend (quando aplicável): tempo de resposta p95/p99 da rota; tempo e nº de queries por
request; uso de CPU/memória.

| Métrica | Ferramenta | Valor atual (antes) | Alvo / depois |
|---|---|---|---|
| [ex.: LCP] | [ex.: Lighthouse] | [medir] | [preencher] |

## 4. Diagnóstico (gargalos encontrados)
Por camada, com evidência do código (arquivo / função / padrão).

## 5. Plano priorizado (impacto × esforço — quick wins primeiro)

### 1. [Título] — Impacto: [Alto/Médio/Baixo] · Esforço: [Baixo/Médio/Alto]
- O quê: [a mudança, em nível estratégico]
- Por quê: [o gargalo que resolve]
- Ganho esperado: [o que melhora e por que]
- Skill para a execução: [ex.: frontend-design / n8n-* / nenhuma]
- Risco: [o que observar]

### 2. [...]

## 6. Critérios de sucesso   ← incluir SOMENTE se houver meta definida
- A otimização é considerada bem-sucedida quando [meta + como verificar].

## 7. Riscos e o que NÃO tocar
- Com base nas restrições informadas: [...]
```

---

## Princípios

- **Só diagnostica e planeja.** Nunca aplique as mudanças, mesmo que pareçam triviais.
- **Evidência > opinião.** Toda recomendação se ancora em algo que você leu no código.
- **Específico > genérico.** Se o item serviria para qualquer app Next/Nest, ele provavelmente não saiu da análise — refaça.
- **Incerteza explícita.** Marque suspeitas como suspeitas e diga o que as confirmaria.
