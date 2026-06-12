# Checklist de performance — NestJS (backend)

O que procurar ao analisar a área alvo. É um roteiro de investigação, não uma lista de
tarefas. **Identifique primeiro o data layer** (Prisma, TypeORM, Mongoose, query bruto) e se a
API é REST ou GraphQL — as alavancas mais fortes quase sempre estão no acesso a dados.

## Banco de dados (geralmente o maior ofensor)
- **N+1 queries**: laço que dispara uma query por item em vez de um join/`include`/batch. Sintoma clássico de lentidão em listas.
- Índices ausentes em colunas usadas em `where`/`order by`/`join`.
- `SELECT *` ou carregar relações que não são usadas na resposta.
- Falta de paginação em endpoints que retornam coleções grandes.
- Pool de conexões mal dimensionado.
- Em GraphQL: resolvers sem dataloader, recriando o N+1 por campo.

## Cache
- Ausência de cache onde o dado muda pouco (cache-manager, Redis, cache de resposta).
- Recomputar a cada request algo que poderia ser memoizado/cacheado.
- TTL inadequado (curto demais = sem ganho; longo demais = dado velho).

## Event loop e trabalho bloqueante
- Trabalho CPU-bound no caminho do request (parsing pesado, criptografia, geração de imagem) travando o event loop.
- Operações síncronas onde deveriam ser assíncronas.

## Chamadas externas
- Chamadas a serviços externos em série que poderiam ser `Promise.all`.
- Sem timeout/retry/circuit breaker, deixando o request pendurado.
- Sem cache de respostas externas estáveis.

## Payload e serialização
- Respostas grandes demais (campos que o cliente não usa).
- `class-transformer`/serialização custosa em respostas volumosas.
- Validação de DTO pesada em rotas de alto tráfego.

## Pipeline do Nest
- Interceptors/guards/middlewares globais rodando em rotas que não precisam deles.
- Logging verboso em hot paths.

## Como medir (para a seção de baseline do plano)
- Tempo de resposta da rota: p50/p95/p99 (não só média).
- Tempo e quantidade de queries por request (log do ORM ou APM).
- Uso de CPU/memória sob carga.
- Throughput (req/s) em teste de carga, quando relevante.
