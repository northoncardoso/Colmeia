# ADR 0007: privacidade, moderação e anúncios responsáveis

**Status:** aceito
**Data:** 2026-11-27

## Contexto

Uma rede de conteúdo científico lida com dado pessoal, opinião e potencial desinformação. O modelo de anúncio aumenta o risco de conflito de interesse, principalmente em saúde.

## Decisão

* Coletar o mínimo necessário: email, nome e dados de perfil declarados.
* Não usar dado de saúde para segmentação, nem permitir que anunciante faça isso.
* Toda decisão de moderação gera evento de auditoria com ator, data e justificativa.
* Denúncia tem triagem em 48 horas.
* Exclusão de conta anonimiza o autor e preserva o histórico da conversa.
* Exportação de dados disponível em JSON.
* Logs não contêm email, senha, token ou conteúdo privado.

## Consequências

* O time precisa manter fila de moderação desde o MVP.
* A política de privacidade e os termos precisam de revisão jurídica antes do lançamento.
* A auditoria aumenta o custo de operação, mas reduz risco reputacional e jurídico.
* Anunciante não recebe relatório individual de usuário, apenas agregado.

## Alternativas descartadas

* Moderação só reativa, sem fila: risco de desinformação se espalhar antes da resposta.
* Exclusão com apagamento total: quebraria conversas e facilitaria apagar prova de fraude.
* Segmentação por interesse de saúde: veto por risco jurídico e ético.
