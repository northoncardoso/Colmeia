# ADR 0008: partida fria com dados abertos

**Status:** aceito
**Data:** 2026-11-27

## Contexto

Rede social vazia não retém ninguém. A ColmeiaLab precisa de acervo no primeiro dia, sem pedir que pesquisadores digitem tudo do zero.

## Decisão

* Importar metadados públicos de **Crossref** e **OpenAlex** para popular fontes e sugerir publicações.
* O Mel nunca é gerado automaticamente por modelo de linguagem no MVP. A importação traz a fonte, não o resumo.
* Parcerias com universidades e programas de extensão para semear comunidades com curadores.
* Cada comunidade começa com um curador responsável e uma pauta mínima.

## Consequências

* O produto depende de API externa, que precisa de cache, tempo limite e tratamento de falha.
* Metadados importados precisam de atribuição e respeito à licença.
* A ausência de Mel automático limita o volume inicial, mas preserva a qualidade.
* O crescimento inicial é manual e depende de relacionamento institucional.

## Alternativas descartadas

* Gerar Mel automaticamente: risco de erro científico e de responsabilidade.
* Esperar o boca a boca: lento demais para validar o produto.
* Raspar sites de universidades: risco jurídico e dados sujos.
