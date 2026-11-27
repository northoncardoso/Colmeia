# ColmeiaLab: regras do projeto

## Contexto

A ColmeiaLab é uma rede social brasileira de divulgação científica. O público leigo é o centro do produto. Cada publicação tem uma fonte científica e um resumo em linguagem simples chamado Mel. A comunidade de patrocínio se chama Néctar. O logo é a abelha nativa Jataí. Colmeia é o conceito e a metáfora, não a marca isolada.

Repositório: https://github.com/northoncardoso/ColmeiaLab

## Skills de processo (superpowers)

As skills do pacote `obra/superpowers` estão versionadas em `.agents/skills/`, para que qualquer pessoa ou agente que clone o repositório tenha o mesmo processo. Elas são carregadas a partir do diretório local do projeto.

Fluxo obrigatório para qualquer trabalho:

1. `using-superpowers`: porta de entrada, escolhe a skill do momento.
2. `brainstorming`: antes de criar funcionalidade ou mudar comportamento. Classifica o trabalho e fecha o design.
3. `writing-plans`: depois do design aprovado, gera o plano de implementação.
4. `executing-plans` ou `subagent-driven-development`, com `dispatching-parallel-agents` quando as tarefas forem independentes.
5. `test-driven-development`: teste antes da implementação.
6. `systematic-debugging`: para qualquer bug, antes de propor correção.
7. `requesting-code-review` e `receiving-code-review`: na revisão.
8. `verification-before-completion`: antes de declarar concluído.
9. `finishing-a-development-branch`: para integrar a branch.
10. `using-git-worktrees`: para isolar mudanças grandes.

Design em `docs/superpowers/specs/`. Plano em `docs/superpowers/plans/`. Decisão de arquitetura em `docs/adr/`.

## Regras de produto

* O leitor leigo é cidadão de primeira classe. Toda tela precisa fazer sentido para quem não é pesquisador.
* Publicação só existe com fonte e com Mel. Sem os dois, não publica.
* Mel é reação, não moeda especulativa. Um Mel por pessoa por publicação.
* Anúncio nunca decide o que aparece no feed orgânico. Conteúdo patrocinado é sempre identificado.
* Nenhum dado pessoal é vendido. Segmentação de anúncio é por comunidade e contexto.
* Orgulho brasileiro é requisito de marca, não enfeite. Nomes, textos e identidade usam português do Brasil.
* Conteúdo de saúde não pode prometer cura. Denúncia de desinformação tem prioridade.

## Convenções

* Responder em português do Brasil, tom direto.
* Em texto gerado (código, comentários, docs, commits), não usar hífen nem travessão como pontuação. Preferir vírgulas, dois pontos ou frases curtas.
* Textos publicados no Git (issues, commits, PRs) devem ser formais, impessoais e técnicos, sem gírias e sem primeira pessoa.
* Antes de adotar biblioteca ou serviço, verificar se está atual e não foi descontinuado.
* Identificadores de código podem ficar em inglês. Textos de interface ficam em português.

## Como rodar e testar

Ainda não há código de produto. Quando a Fase 1 começar, este arquivo deve ser atualizado com os comandos reais de instalação, execução e teste.

## Definition of Done

* Teste escrito antes da regra, suíte inteira verde.
* Tipagem limpa.
* CI verde no PR antes do merge, nunca direto na main.
* Design e plano atualizados quando a decisão muda.
* Issue fechada citando o PR.
* Nenhum dado pessoal em log, seed ou teste.

## Glossário

* **ColmeiaLab**: marca e nome da rede social.
* **Colmeia**: conceito, metáfora e nome do coletivo. Não usar como marca isolada.
* **Mel**: reação de like e nome do resumo em linguagem simples.
* **Néctar**: comunidade de patrocínio de projetos científicos.
* **Jataí**: abelha nativa sem ferrão, símbolo e logo.
* **Comunidade**: espaço por área do conhecimento.
* **Curador**: pessoa que organiza comunidade e valida fontes.
