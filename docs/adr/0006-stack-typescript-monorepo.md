# ADR 0006: stack TypeScript com monorepo

**Status:** aceito
**Data:** 2026-11-27

## Contexto

O fundador domina React, React Native e Node.js. O produto precisa de web no MVP e de aplicativo no futuro, com tipos compartilhados entre servidor, web e mobile.

## Decisão

* Monorepo com npm workspaces.
* `apps/api`: Node.js 22, Express 5, TypeScript, Prisma, PostgreSQL, Zod, JWT e Argon2.
* `apps/web`: React 19, Vite, TypeScript, React Router e TanStack Query.
* `packages/shared`: tipos, schemas Zod e constantes de domínio.
* Testes: Vitest, Supertest e Testing Library.
* CI: GitHub Actions.

## Consequências

* Um só idioma no repositório inteiro.
* O pacote compartilhado evita divergência de contrato entre API e web.
* O aplicativo Expo da fase 4 reaproveita `packages/shared`.
* Express 5 exige atenção a mudanças de comportamento em rotas assíncronas.
* As versões exatas são travadas no primeiro commit de código, após verificação de que estão ativas.

## Alternativas descartadas

* Fastify: bom desempenho, mas o fundador tem mais familiaridade com Express.
* Next.js: o MVP é uma rede social com feed e autenticação, não um site de conteúdo.
* Backend separado em outro idioma: aumentaria custo sem ganho no estágio atual.
* SQLite: simples, mas o produto precisa de busca textual, concorrência e migração para nuvem.
