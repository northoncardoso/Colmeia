# ColmeiaLab: design do produto

**Data:** 2026-11-27
**Status:** aprovado para planejamento
**Autor:** Northon Cardoso

**Nota de marca:** a validação da issue #7 recomendou não usar Colmeia isolado. A marca principal passou a ser **ColmeiaLab**, com Colmeia como nome do conceito. Ver o ADR 0001 e `docs/brand/2026-11-27-validacao-marca.md`.

## 1. Problema

A produção científica brasileira cresce, mas não chega ao público. O artigo fica em PDF, muitas vezes em inglês, atrás de paywall e cheio de jargão. Quem não é pesquisador não tem porta de entrada.

Do outro lado, o pesquisador divulga em redes genéricas, perde o vínculo com a fonte e não tem canal direto para captar recursos. O patrocinador não encontra projeto confiável, com escopo, orçamento e prestação de contas.

As redes existentes resolvem pedaços do problema. O ResearchGate conecta pesquisadores entre si. O Facebook e o Instagram distribuem conteúdo, mas não verificam fonte. Nenhuma das duas coloca o leigo no centro e trata a ciência como conteúdo de primeira classe.

## 2. Proposta

A ColmeiaLab é uma rede social de divulgação científica com o público leigo no centro. Cada publicação tem duas camadas obrigatórias:

1. **Fonte**: DOI, link para o artigo ou PDF, com metadados.
2. **Mel**: resumo em linguagem simples, com no máximo 1.500 caracteres, escrito por quem publica e editável pela comunidade com histórico.

O leitor encontra conteúdo nas **Comunidades** por área do conhecimento ou no feed. Quem entendeu e gostou dá **Mel**. Quem quer apoiar um projeto vai ao **Néctar**. O motor de anúncios sustenta a operação.

## 3. Princípios

1. **O leigo é cidadão de primeira classe.** Toda tela, texto e fluxo precisa fazer sentido para quem nunca leu um artigo.
2. **Fonte antes de opinião.** Publicação sem fonte não existe. Opinião é comentário, não publicação.
3. **Mel é reação, não vaidade.** Um Mel por pessoa por publicação. Não há compra de Mel, nem Mel como moeda.
4. **Anúncio não manda no conteúdo.** O feed orgânico é cronológico e por comunidade. Conteúdo pago é identificado.
5. **Orgulho brasileiro.** Marca, nomes e identidade em português do Brasil. A jataí é abelha nativa, não símbolo importado.
6. **Privacidade por padrão.** Nenhum dado pessoal é vendido. Segmentação de anúncio é por comunidade e contexto.

## 4. Identidade

| Elemento | Decisão |
| --- | --- |
| Nome da rede | ColmeiaLab |
| Conceito | Colmeia |
| Logo | Jataí, abelha nativa sem ferrão |
| Like | Mel |
| Resumo em linguagem simples | Mel |
| Comunidade de patrocínio | Néctar |
| Comunidades de área | Comunidades |
| Slogan de trabalho | A ciência que chega |

O duplo uso de Mel, como reação e como resumo, é intencional. A reação é dada ao Mel. Quem leu e entendeu dá Mel ao Mel.

## 5. Personas

### 5.1 Leitor curioso

Não é pesquisador. Chega por indicação, busca ou anúncio. Quer entender um tema em poucos minutos. Não conhece DOI e não sabe o que é revisão por pares. Precisa de contexto, exemplo e link para se aprofundar.

### 5.2 Pesquisador

Tem artigo, projeto ou resultado. Quer divulgar, ser citado e captar recursos. Tem pouco tempo e desconfia de rede social. Precisa de publicação rápida, verificação de identidade e métricas honestas.

### 5.3 Patrocinador

Empresa, fundação ou pessoa física. Quer apoiar projeto com escopo claro, orçamento visível e prestação de contas. Tem medo de fraude e de má reputação.

### 5.4 Curador

Pesquisador ou profissional de comunicação. Organiza comunidade, revisa Mel, valida fonte e media conflito. Precisa de ferramenta de moderação e de histórico auditável.

## 6. Escopo do MVP

### Dentro

* Cadastro com email e senha, perfil público.
* Verificação de pesquisador por ORCID ou currículo Lattes.
* Oito comunidades padrão por grande área do CNPq.
* Publicação com fonte, Mel, tags e comunidade.
* Feed cronológico por comunidade e feed de quem segue.
* Reação Mel, uma por pessoa por publicação.
* Comentários em dois níveis.
* Denúncia de publicação e de comentário.
* Néctar com projeto, meta, orçamento, prazo e link externo de apoio.
* Painel de moderação.
* Motor de anúncios em modo manual, com inventário e métricas.
* Política de privacidade e termos de uso.

### Fora

* Aplicativo mobile nativo.
* Mensagem direta.
* Vídeo e transmissão ao vivo.
* Pagamento dentro da plataforma.
* Feed algorítmico de recomendação.
* Marketplace de serviços.
* API pública.

## 7. Modelo de dados conceitual

* **Usuario**: conta, email, senha, papel, estado.
* **Perfil**: nome, bio, instituição, área, ORCID, Lattes, selo de verificação.
* **Comunidade**: nome, slug, descrição, área do CNPq, regras, curadores.
* **Publicacao**: autor, comunidade, título, Mel, corpo opcional, estado.
* **Fonte**: tipo, DOI, URL, título original, autores, ano, veículo.
* **Mel**: usuário, publicação, data. Chave única por par.
* **Comentario**: autor, publicação, pai, corpo, estado.
* **ProjetoNectar**: pesquisador, título, resumo, meta, arrecadado, prazo, estado.
* **Apoio**: projeto, apoiador, valor, origem, recibo.
* **Anuncio**: anunciante, criativo, comunidade, orçamento, estado.
* **Denuncia**: denunciante, alvo, motivo, estado, decisão.
* **EventoAuditoria**: ator, ação, alvo, data, metadados.

## 8. Fluxos principais

### 8.1 Publicar

1. Pessoa autenticada escolhe comunidade.
2. Informa a fonte: DOI, URL ou PDF.
3. O sistema busca metadados em Crossref ou OpenAlex e preenche o que puder.
4. Escreve o Mel, com contador de caracteres e dica de linguagem simples.
5. Revisa e publica.
6. A publicação entra na comunidade e no feed de seguidores.

### 8.2 Consumir

1. Visitante abre a home e vê comunidades em destaque e publicações recentes.
2. Filtra por comunidade ou busca por tema.
3. Lê o Mel, vê a fonte, comenta ou dá Mel.
4. Segue comunidade ou pessoa.

### 8.3 Apoiar no Néctar

1. Pesquisador verificado cria projeto com meta, orçamento e prazo.
2. Curador do Néctar revisa e aprova.
3. Projeto fica público com botão de apoio.
4. Apoiador é levado ao meio externo de pagamento.
5. Pesquisador registra o recibo e publica prestação de contas.
6. Projeto encerra com relatório público.

### 8.4 Anunciar

1. Anunciante envia criativo e escolhe comunidade.
2. Sistema valida política de anúncio.
3. Anúncio entra em leilão por impressão.
4. Métricas ficam disponíveis para anunciante e para auditoria interna.
5. Denúncia de anúncio vai para moderação.

## 9. Regras de negócio

* Publicação exige fonte válida e Mel entre 200 e 1.500 caracteres.
* Mel é único por usuário e publicação. Pode ser removido, não duplicado.
* Pesquisador só recebe selo após verificação de ORCID ou Lattes.
* Projeto do Néctar exige meta, orçamento, prazo e responsável verificado.
* Néctar não movimenta dinheiro no MVP. O apoio acontece em link externo.
* Anúncio de saúde não pode prometer cura, diagnóstico ou resultado garantido.
* Denúncia tem prazo de triagem de 48 horas e decisão registrada.
* Conta suspensa perde a possibilidade de publicar e de apoiar, mas mantém o histórico.
* Exclusão de conta anonimiza o autor e preserva a integridade da conversa.

## 10. Modelo de negócio

A receita principal vem de anúncios nativos no feed e no Néctar. O modelo é leilão por impressão, com segmentação por comunidade, tema e contexto. Não há venda de dado pessoal, nem segmentação por condição de saúde.

Na fase 2 entra assinatura opcional sem anúncios, para leitores. Na fase 3, se validada, taxa de intermediação sobre projetos do Néctar, paga pelo patrocinador, nunca pelo pesquisador.

Princípio comercial: a ColmeiaLab precisa ser lucrativa sem transformar o leitor em produto. Se o anúncio prejudicar a confiança, ele sai.

## 11. Métricas

**Métrica norte:** leitores leigos recorrentes por semana.

**Secundárias:**

* Publicações com Mel completo por semana.
* Taxa de Mel por visualização.
* Conclusão de leitura do Mel.
* Comentários por publicação.
* Projetos do Néctar ativos e financiados.
* Retenção D7 e D30.
* Receita por mil impressões.
* Denúncias procedentes por mil publicações.

## 12. Riscos e mitigação

| Risco | Mitigação |
| --- | --- |
| Desinformação científica | Fonte obrigatória, curadoria, denúncia e selo de verificação |
| Partida fria | Parcerias com universidades e importação de metadados abertos |
| Moderação cara | Ferramentas de triagem, curadores voluntários e limites claros |
| LGPD | Minimização de dados, política clara e exclusão com anonimização |
| Conflito de interesse em anúncio | Política setorial e auditoria independente |
| Dependência de anúncio | Assinatura na fase 2 e receita do Néctar na fase 3 |
| Pesquisador sem tempo | Publicação em menos de cinco minutos, com metadados automáticos |

## 13. Decisões abertas

* Licença do código.
* Domínio e arroba definitivos.
* Identidade visual final e versão vetorial do logo.
* Modelo de verificação de pesquisador além de ORCID e Lattes.
* Política detalhada de anúncios por setor.
* Regras de curadoria e eleição de curadores.
* Estratégia de importação inicial de artigos.

## 14. Critérios de sucesso do MVP

* 500 publicações com Mel completo em 90 dias.
* 5.000 leitores únicos em 90 dias.
* 20 projetos no Néctar, com pelo menos 5 apoiados.
* Retenção D7 acima de 20 por cento.
* Nenhum incidente grave de desinformação sem resposta em 48 horas.
