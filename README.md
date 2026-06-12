# Performance-Max

Skill de diagnóstico e planejamento de performance para aplicações **Next.js + NestJS**.

Analisa gargalos reais do seu código, gera planos priorizados por impacto × esforço, e orienta a execução com skills específicas — sem blocos de código genéricos.

## Instalação

```bash
npx skills add @lucas_dev96/performance-max
```

## Como usar

Invoque a skill quando precisar otimizar uma área específica da sua aplicação:

```
/performance-max melhore a performance do chat
```

```
/performance-max essa rota tá lenta
```

```
/performance-max reduzir o tempo de carregamento da página de checkout
```

## O que ela faz

1. **Coleta contexto** — pergunta rotas/arquivos suspeitos, sintomas, métricas-alvo e restrições
2. **Analisa o código** — lê a implementação real do alvo (Next.js frontend, NestJS backend)
3. **Diagnostica** — identifica gargalos com evidência (n+1 queries, renders desnecessários, bundle bloaters, etc.)
4. **Planeja** — monta um plano priorizado por impacto × esforço
5. **Salva** — documenta tudo em `docs/performance/` para você executar depois

## Exemplo de saída

Você recebe um documento estruturado em `docs/performance/per-otimizacao-chat.md` com:

- **Alvo e escopo** — exatamente o que será otimizado
- **Baseline** — como medir antes/depois (LCP, p95, bundle size, etc.)
- **Diagnóstico** — gargalos encontrados com arquivo e linha
- **Plano priorizado** — quick wins primeiro
  - O quê fazer
  - Por quê (o gargalo que resolve)
  - Ganho esperado
  - Skill para execução (frontend-design, etc.)
  - Riscos
- **Critérios de sucesso** — quando a otimização é considerada bem-sucedida

## Requisitos

- Claude Code (CLI ou VSCode)
- Projeto Next.js + NestJS no diretório local
- Acesso ao código-fonte

## Quando usar

✅ Otimizar uma feature específica (chat, checkout, dashboard)  
✅ Reduzir tamanho de bundle, latência de API, tempo de render  
✅ Melhorar Core Web Vitals (LCP, INP, CLS)  

❌ Construir features novas do zero  
❌ Refatorações genéricas sem relação com speed  
❌ Corrigir bugs funcionais  

## Filosofia

A skill segue três princípios:

1. **Foco no alvo** — investiga a área que você nomeia, não varre o repositório inteiro
2. **Planejamento, não execução** — você recebe um plano descritivo que implementa depois
3. **Fundamentado no código** — recomendações saem do que foi lido, com evidência

## Estrutura do repositório

```
performance-max/
  ├── SKILL.md              # Definição da skill
  ├── README.md             # Este arquivo
  ├── package.json          # Metadados npm
  └── references/
      ├── nextjs.md         # Checklist de otimizações Next.js
      └── nestjs.md         # Checklist de otimizações NestJS
```

## License

MIT

---

Desenvolvido para desenvolvedores 😎
# performance-max
