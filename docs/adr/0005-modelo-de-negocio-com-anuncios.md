# ADR 0005: modelo de negócio baseado em anúncios

**Status:** aceito
**Data:** 2026-11-27

## Contexto

O projeto precisa de receita desde cedo, sem cobrar do leitor e sem cobrar do pesquisador. O fundador quer um motor de anúncios por trás da rede, como acontece no Facebook e no Instagram.

## Decisão

* A receita principal vem de **anúncios nativos** no feed e no Néctar.
* A segmentação é por comunidade, tema e contexto, nunca por dado pessoal sensível.
* Não há venda de dados.
* Anúncio é sempre identificado como patrocinado.
* Na fase 2 entra assinatura opcional sem anúncios.
* Na fase 3, se validada, taxa de intermediação do Néctar paga pelo patrocinador.

## Consequências

* O motor de anúncios é parte do produto, não plugin.
* O MVP começa com entrega manual, sem leilão automático, para validar inventário e preço.
* A política de anúncio precisa vetar saúde com promessa de cura e conteúdo enganoso.
* A confiança é ativo financeiro: um anúncio ruim custa mais do que rende.

## Alternativas descartadas

* Assinatura desde o início: barreira para o leigo, que é o público central.
* Cobrar do pesquisador para publicar: afasta quem produz conteúdo.
* Venda de dados: contraria o princípio de privacidade e cria risco jurídico.
