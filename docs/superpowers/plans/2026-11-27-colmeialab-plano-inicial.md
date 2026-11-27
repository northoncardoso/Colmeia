# Plano inicial da ColmeiaLab

> **Para agentes:** SUB-SKILL OBRIGATÓRIA: use `superpowers:subagent-driven-development` (recomendado) ou `superpowers:executing-plans` para executar este plano tarefa a tarefa. Os passos usam caixas (`- [ ]`) para acompanhamento.

**Objetivo:** entregar o MVP web da ColmeiaLab, com comunidades, publicações com fonte e Mel, feed, reação Mel, Néctar e moderação.

**Arquitetura:** monorepo TypeScript com API Express, banco PostgreSQL via Prisma, frontend React com Vite e pacote compartilhado de tipos e validações. Autenticação por JWT, com senha em Argon2. O motor de anúncios do MVP é manual, com inventário e métricas, sem leilão automático.

**Stack:** Node.js 22, TypeScript, Express 5, Prisma, PostgreSQL 16, Zod, React 19, Vite, TanStack Query, Vitest, Supertest, Testing Library, GitHub Actions.

**Spec:** `docs/superpowers/specs/2026-11-27-colmeia-design.md`

## Restrições globais

* Todo texto de interface em português do Brasil.
* Nenhum hífen ou travessão como pontuação em texto gerado.
* Publicação só existe com fonte e com Mel entre 200 e 1.500 caracteres.
* Mel é único por par usuário e publicação.
* Nenhum dado pessoal em log, seed ou teste.
* Versões de dependência são travadas no primeiro commit da Fase 1, após verificação de que estão ativas.
* CI verde antes de qualquer merge. Nunca commitar direto na main.

## Foco de revisão

Estas são as falhas que o spec implica e que os testes de tarefa precisam cobrir:

1. Publicação com Mel vazio, curto demais ou longo demais precisa ser recusada no servidor, não só no cliente.
2. Dois pedidos simultâneos de Mel na mesma publicação precisam resultar em um único registro, não em erro 500.
3. Fonte com DOI inválido ou URL malformada precisa ser recusada sem derrubar a criação da publicação.
4. Usuário suspenso precisa perder a capacidade de publicar, comentar e apoiar, mantendo o histórico visível.
5. Exclusão de conta precisa anonimizar o autor sem quebrar comentários e publicações existentes.

---

## Fase 0: fundação

### Tarefa 1: validar marca, domínio e arroba

**Arquivos:**
* Criar: `docs/brand/2026-11-27-validacao-marca.md`

**Passos:**

- [ ] Consultar INPI para ColmeiaLab nas classes 9, 35, 41 e 42.
- [ ] Verificar domínios `colmeia.com.br`, `colmeia.app` e `colmeia.social`.
- [ ] Verificar arroba `@colmeia` nas redes principais.
- [ ] Registrar resultado, riscos e alternativas no documento.
- [ ] Abrir issue com o parecer.

### Tarefa 2: identidade visual mínima

**Arquivos:**
* Criar: `docs/brand/2026-11-27-identidade.md`
* Criar: `assets/logo-jatai.svg`
* Criar: `assets/paleta.md`

**Passos:**

- [ ] Definir paleta com amarelo mel, marrom escuro e branco.
- [ ] Desenhar a jataí em vetor simples, legível em 24 pixels.
- [ ] Definir tipografia e usos proibidos.
- [ ] Validar contraste conforme WCAG AA.
- [ ] Commitar os arquivos.

### Tarefa 3: protótipo navegável

**Arquivos:**
* Criar: `docs/prototipo/feed.md`
* Criar: `docs/prototipo/publicacao.md`
* Criar: `docs/prototipo/nectar.md`

**Passos:**

- [ ] Desenhar a home com comunidades e feed.
- [ ] Desenhar a leitura de publicação com Mel e fonte.
- [ ] Desenhar a criação de publicação em três passos.
- [ ] Desenhar a página de projeto do Néctar.
- [ ] Validar com cinco pessoas que não são pesquisadoras.

---

## Fase 1: MVP web

### Tarefa 4: monorepo, tooling e CI

**Arquivos:**
* Criar: `package.json`
* Criar: `tsconfig.base.json`
* Criar: `.github/workflows/ci.yml`
* Criar: `apps/api/package.json`
* Criar: `apps/web/package.json`
* Criar: `packages/shared/package.json`
* Criar: `packages/shared/src/index.ts`
* Teste: `apps/api/src/health.test.ts`

**Interfaces:**
* Produz: `GET /health` com corpo `{ "status": "ok" }`.
* Produz: script `npm test` na raiz que roda todas as suítes.

- [ ] **Passo 1: escrever o teste que falha**

```ts
import request from "supertest";
import { app } from "./app";

it("responde ok no health check", async () => {
  const resposta = await request(app).get("/health");
  expect(resposta.status).toBe(200);
  expect(resposta.body).toEqual({ status: "ok" });
});
```

- [ ] **Passo 2: rodar e ver falhar**

Rodar: `npm test --workspace apps/api`
Esperado: falha porque `app` não existe.

- [ ] **Passo 3: implementar `apps/api/src/app.ts` com Express e rota `/health`**
- [ ] **Passo 4: rodar e ver passar**
- [ ] **Passo 5: configurar CI com lint, typecheck e test em Node 22**
- [ ] **Passo 6: commitar**

### Tarefa 5: banco e modelo de dados

**Arquivos:**
* Criar: `prisma/schema.prisma`
* Criar: `prisma/migrations/`
* Criar: `apps/api/src/db/prisma.ts`
* Teste: `apps/api/src/db/schema.test.ts`

**Interfaces:**
* Produz: modelos `User`, `Profile`, `Community`, `Post`, `Source`, `Mel`, `Comment`, `NectarProject`, `Support`, `Ad`, `Report`, `AuditEvent`.
* Produz: `prisma` como cliente singleton.

- [ ] **Passo 1: escrever o teste de restrição única de Mel**

```ts
it("não aceita dois mels do mesmo usuário na mesma publicação", async () => {
  await prisma.mel.create({ data: { userId, postId } });
  await expect(prisma.mel.create({ data: { userId, postId } })).rejects.toThrow();
});
```

- [ ] **Passo 2: rodar e ver falhar**
- [ ] **Passo 3: criar o schema com `@@unique([userId, postId])` em `Mel`**
- [ ] **Passo 4: rodar migração em banco de teste e ver passar**
- [ ] **Passo 5: commitar**

### Tarefa 6: autenticação e perfil

**Arquivos:**
* Criar: `apps/api/src/modules/auth/auth.routes.ts`
* Criar: `apps/api/src/modules/auth/auth.service.ts`
* Criar: `apps/api/src/modules/auth/auth.test.ts`
* Criar: `apps/web/src/pages/Cadastro.tsx`
* Criar: `apps/web/src/pages/Login.tsx`

**Interfaces:**
* Produz: `POST /auth/register` com `{ name, email, password }` e retorno `{ token, user }`.
* Produz: `POST /auth/login` com `{ email, password }` e retorno `{ token, user }`.
* Produz: `GET /me` com `Authorization: Bearer <token>`.
* Produz: `hashPassword` e `verifyPassword` com Argon2.

- [ ] **Passo 1: teste de cadastro com email duplicado retorna 409**
- [ ] **Passo 2: rodar e ver falhar**
- [ ] **Passo 3: implementar cadastro, login e middleware de autenticação**
- [ ] **Passo 4: rodar e ver passar**
- [ ] **Passo 5: criar telas de cadastro e login**
- [ ] **Passo 6: commitar**

### Tarefa 7: comunidades padrão

**Arquivos:**
* Criar: `prisma/seed.ts`
* Criar: `apps/api/src/modules/communities/communities.routes.ts`
* Criar: `apps/api/src/modules/communities/communities.test.ts`
* Criar: `apps/web/src/pages/Comunidade.tsx`

**Interfaces:**
* Produz: oito comunidades: Ciências Exatas, Ciências Biológicas, Engenharias, Ciências da Saúde, Ciências Agrárias, Ciências Sociais Aplicadas, Ciências Humanas, Linguística, Letras e Artes.
* Produz: `GET /communities` e `GET /communities/:slug`.

- [ ] **Passo 1: teste que o seed cria oito comunidades e nenhuma duplicada**
- [ ] **Passo 2: rodar e ver falhar**
- [ ] **Passo 3: implementar seed idempotente e rotas de leitura**
- [ ] **Passo 4: rodar e ver passar**
- [ ] **Passo 5: criar página de comunidade**
- [ ] **Passo 6: commitar**

### Tarefa 8: publicação com fonte e Mel

**Arquivos:**
* Criar: `apps/api/src/modules/posts/posts.routes.ts`
* Criar: `apps/api/src/modules/posts/posts.service.ts`
* Criar: `apps/api/src/modules/posts/posts.test.ts`
* Criar: `apps/api/src/modules/sources/crossref.ts`
* Criar: `apps/web/src/pages/NovaPublicacao.tsx`

**Interfaces:**
* Produz: `POST /posts` com `{ communityId, title, mel, source }`.
* Produz: `validateMel(texto): { ok: boolean; motivo?: string }`.
* Produz: `fetchCrossref(doi): Promise<SourceMetadata | null>`.
* Consome: autenticação da Tarefa 6 e comunidades da Tarefa 7.

- [ ] **Passo 1: testes de Mel com 199, 200, 1500 e 1501 caracteres**
- [ ] **Passo 2: rodar e ver falhar**
- [ ] **Passo 3: implementar validação e criação de publicação**
- [ ] **Passo 4: teste de fonte com DOI inválido retorna 422 e não cria publicação**
- [ ] **Passo 5: implementar validação de fonte e busca no Crossref com tempo limite**
- [ ] **Passo 6: rodar e ver passar**
- [ ] **Passo 7: criar formulário em três passos**
- [ ] **Passo 8: commitar**

### Tarefa 9: feed

**Arquivos:**
* Criar: `apps/api/src/modules/feed/feed.routes.ts`
* Criar: `apps/api/src/modules/feed/feed.test.ts`
* Criar: `apps/web/src/pages/Home.tsx`

**Interfaces:**
* Produz: `GET /feed?community=&cursor=&limit=20` com paginação por cursor.
* Produz: `GET /feed/following` para quem segue pessoas e comunidades.

- [ ] **Passo 1: teste de paginação sem repetir publicação entre páginas**
- [ ] **Passo 2: rodar e ver falhar**
- [ ] **Passo 3: implementar feed cronológico com cursor por `createdAt` e `id`**
- [ ] **Passo 4: rodar e ver passar**
- [ ] **Passo 5: criar home com abas Descobrir e Seguindo**
- [ ] **Passo 6: commitar**

### Tarefa 10: reação Mel

**Arquivos:**
* Criar: `apps/api/src/modules/mels/mels.routes.ts`
* Criar: `apps/api/src/modules/mels/mels.test.ts`
* Criar: `apps/web/src/components/BotaoMel.tsx`

**Interfaces:**
* Produz: `POST /posts/:id/mel` e `DELETE /posts/:id/mel`, idempotentes.
* Produz: contagem de Mel na resposta da publicação.

- [ ] **Passo 1: teste de dois pedidos simultâneos resultando em um registro**
- [ ] **Passo 2: rodar e ver falhar**
- [ ] **Passo 3: implementar com `upsert` e tratar conflito de chave única**
- [ ] **Passo 4: rodar e ver passar**
- [ ] **Passo 5: criar botão com estado otimista e rollback em erro**
- [ ] **Passo 6: commitar**

### Tarefa 11: comentários

**Arquivos:**
* Criar: `apps/api/src/modules/comments/comments.routes.ts`
* Criar: `apps/api/src/modules/comments/comments.test.ts`
* Criar: `apps/web/src/components/Comentarios.tsx`

**Interfaces:**
* Produz: `POST /posts/:id/comments` com `{ body, parentId? }`.
* Produz: `GET /posts/:id/comments` com dois níveis.
* Produz: bloqueio de comentário de usuário suspenso.

- [ ] **Passo 1: teste de comentário de usuário suspenso retorna 403**
- [ ] **Passo 2: rodar e ver falhar**
- [ ] **Passo 3: implementar criação, listagem e verificação de estado**
- [ ] **Passo 4: rodar e ver passar**
- [ ] **Passo 5: criar componente de comentários**
- [ ] **Passo 6: commitar**

### Tarefa 12: denúncias e moderação

**Arquivos:**
* Criar: `apps/api/src/modules/reports/reports.routes.ts`
* Criar: `apps/api/src/modules/reports/reports.test.ts`
* Criar: `apps/web/src/pages/Moderacao.tsx`

**Interfaces:**
* Produz: `POST /reports` com `{ targetType, targetId, reason }`.
* Produz: `GET /moderation/reports` para curador.
* Produz: `PATCH /moderation/reports/:id` com decisão e justificativa.
* Produz: `AuditEvent` em toda decisão.

- [ ] **Passo 1: teste de denúncia duplicada do mesmo usuário no mesmo alvo**
- [ ] **Passo 2: rodar e ver falhar**
- [ ] **Passo 3: implementar denúncia, fila e decisão com auditoria**
- [ ] **Passo 4: rodar e ver passar**
- [ ] **Passo 5: criar painel de moderação**
- [ ] **Passo 6: commitar**

### Tarefa 13: Néctar

**Arquivos:**
* Criar: `apps/api/src/modules/nectar/nectar.routes.ts`
* Criar: `apps/api/src/modules/nectar/nectar.test.ts`
* Criar: `apps/web/src/pages/Nectar.tsx`
* Criar: `apps/web/src/pages/Projeto.tsx`

**Interfaces:**
* Produz: `POST /nectar/projects` com `{ title, summary, goalCents, budget, deadline }`.
* Produz: `GET /nectar/projects` e `GET /nectar/projects/:slug`.
* Produz: `POST /nectar/projects/:id/support` com `{ amountCents, receiptUrl }`.
* Produz: bloqueio de projeto sem pesquisador verificado.

- [ ] **Passo 1: teste de projeto sem verificação retorna 403**
- [ ] **Passo 2: rodar e ver falhar**
- [ ] **Passo 3: implementar criação, listagem e apoio externo**
- [ ] **Passo 4: rodar e ver passar**
- [ ] **Passo 5: criar páginas de lista e de projeto**
- [ ] **Passo 6: commitar**

### Tarefa 14: motor de anúncios manual

**Arquivos:**
* Criar: `apps/api/src/modules/ads/ads.routes.ts`
* Criar: `apps/api/src/modules/ads/ads.test.ts`
* Criar: `apps/web/src/components/Anuncio.tsx`

**Interfaces:**
* Produz: `POST /ads` para admin, com criativo, comunidade e orçamento.
* Produz: `GET /ads/serve?community=` com seleção manual e registro de impressão.
* Produz: `GET /ads/metrics` para admin.

- [ ] **Passo 1: teste de anúncio sem identificação de patrocínio ser recusado**
- [ ] **Passo 2: rodar e ver falhar**
- [ ] **Passo 3: implementar cadastro, entrega e métrica**
- [ ] **Passo 4: rodar e ver passar**
- [ ] **Passo 5: criar componente com rótulo "Patrocinado"**
- [ ] **Passo 6: commitar**

### Tarefa 15: privacidade, termos e observabilidade

**Arquivos:**
* Criar: `docs/legal/politica-privacidade.md`
* Criar: `docs/legal/termos-de-uso.md`
* Criar: `apps/api/src/modules/privacy/privacy.routes.ts`
* Criar: `apps/api/src/modules/privacy/privacy.test.ts`

**Interfaces:**
* Produz: `GET /me/export` com dados do usuário em JSON.
* Produz: `DELETE /me` com anonimização e preservação do histórico.
* Produz: logs estruturados sem email, senha ou token.

- [ ] **Passo 1: teste de exclusão que anonimiza autor e mantém comentário**
- [ ] **Passo 2: rodar e ver falhar**
- [ ] **Passo 3: implementar exportação e exclusão**
- [ ] **Passo 4: rodar e ver passar**
- [ ] **Passo 5: escrever política e termos com revisão jurídica**
- [ ] **Passo 6: commitar**

---

## Fase 2: confiança e crescimento

* Verificação de pesquisador por ORCID e Lattes.
* Importação de metadados do OpenAlex.
* Curadoria com histórico de edição do Mel.
* Notificações por email.
* Painel de métricas do produto.
* Assinatura sem anúncios.

## Fase 3: Néctar completo

* Prestação de contas estruturada.
* Selo de projeto auditado.
* Taxa de intermediação para patrocinador.
* Relatório público de impacto.

## Fase 4: mobile e escala

* Aplicativo Expo para iOS e Android.
* Feed personalizado com explicabilidade.
* API pública de leitura.
* Programa de curadores.

## Mapa de issues

Cada tarefa da Fase 1 vira uma issue com o mesmo nome. As issues de marca, identidade e protótipo pertencem à Fase 0. As fases 2, 3 e 4 ficam em épicos separados, sem detalhamento até a validação do MVP.
