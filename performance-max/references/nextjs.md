# Checklist de performance — Next.js (frontend)

O que procurar ao analisar a área alvo. Não é uma lista de tarefas: é um roteiro de
investigação. Antes de aplicar qualquer item, **confirme a versão do Next no `package.json`** —
defaults e recursos (caching, streaming, prerender) mudam bastante entre versões.

## Estratégia de renderização
- Uso excessivo de `"use client"`: componentes que poderiam ser Server Components virando client à toa, inflando o bundle enviado ao navegador.
- App Router x Pages Router: o modelo de dados e cache é diferente; identifique qual está em uso antes de recomendar.
- Conteúdo dinâmico que poderia ser estático (ou vice-versa): páginas renderizadas a cada request sem necessidade.

## Busca de dados
- **Waterfalls**: requests sequenciais que poderiam ser paralelos (vários `await` em série em vez de `Promise.all`).
- Falta de streaming/`Suspense`: a página inteira espera o dado mais lento em vez de mostrar o que já está pronto.
- Cache de `fetch`/revalidação mal configurado: refetch desnecessário ou dado obsoleto.
- Dados buscados no client que poderiam vir do servidor (round-trips extras).

## Bundle e JavaScript
- Dependências pesadas no caminho crítico (libs de data, gráficos, editores) sem `dynamic import`/lazy.
- Barrel imports (`import { x } from 'lib'`) que arrastam o pacote inteiro e quebram tree-shaking.
- Falta de code splitting em rotas/componentes grandes.
- Polyfills ou libs duplicadas.

## Imagens, fontes e mídia
- Imagens sem `next/image` (sem otimização, sem `sizes`, sem formato moderno).
- Imagem do LCP sem `priority`.
- Fontes sem `next/font` causando layout shift e bloqueio de render.

## Hidratação e re-render
- Árvores client gigantes que custam a hidratar.
- Re-renders desnecessários: contexto global usado em excesso, falta de memoização onde há custo real, `key` instável em listas.
- Efeitos caros rodando no render path.

## Rede e terceiros
- Scripts de terceiros sem estratégia (`next/script` com `strategy` adequada).
- Muitos round-trips à API onde um endpoint agregado resolveria.
- Ausência de CDN/cache de borda para conteúdo estático.

## Como medir (para a seção de baseline do plano)
- Core Web Vitals: LCP, INP, CLS, TTFB — via Lighthouse ou Web Vitals em produção.
- Tamanho de bundle: bundle analyzer.
- Renders/efeitos: React Profiler.
