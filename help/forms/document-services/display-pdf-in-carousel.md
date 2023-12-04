---
title: Exibir vários documentos em pdf
description: Percorra vários documentos em pdf em um formulário adaptável.
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
jira: KT-10292
exl-id: c1d248c3-8208-476e-b0ae-cab25575cd6a
last-substantial-update: 2021-10-12T00:00:00Z
duration: 92
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 1%

---

# Exibir vários documentos em pdf em um carrossel

Um caso de uso comum é exibir vários documentos de PDF para o preenchimento do formulário para revisão antes de enviar o formulário.

Para realizar esse caso de uso, utilizamos o [API incorporada do Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[Uma demonstração ao vivo dessa amostra pode ser vista aqui.](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

As etapas a seguir foram executadas para concluir a integração

## Criar um componente personalizado para exibir vários documentos PDF

Um componente personalizado (pdf-carousel) foi criado para navegar pelos documentos pdf

## Biblioteca do cliente

Uma biblioteca do cliente foi criada para exibir os PDF usando a API incorporada do Adobe PDF. Os PDF a serem exibidos são especificados nos componentes do carrossel pdf.

## Criar formulário adaptável

Crie um formulário adaptável com base em algumas guias (Este exemplo tem 3 guias) Adicione alguns componentes de formulário adaptável nas duas primeiras guias Adicione o componente Carrossel pdf na terceira guia Configure o componente Carrossel pdf como mostrado na captura de tela abaixo
![pdf-carousel](assets/pdf-carousel-af-component.png)

**Incorporar chave de API do PDF** - Essa é a chave que você pode usar para incorporar o pdf. Essa chave só funcionará com localhost. Você pode criar [sua própria chave](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) e associá-lo a outro domínio.

**Especificar Documentos PDF** - Aqui, é possível especificar os documentos pdf que você deseja exibir no carrossel.


## Implantar a amostra no servidor

Para testar isso no servidor local, siga as etapas:

1. [Importar a biblioteca do cliente](assets/pdf-carousel-client-lib.zip) na instância local do AEM [uso do gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importar o componente de carrossel pdf](assets/pdf-carousel-component.zip) na instância local do AEM [uso do gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importar o formulário adaptável](assets/adaptive-form-pdf-carousel.zip) na instância local do AEM [uso do gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importar os pdf de exemplo para exibição](assets/pdf-carousel-sample-documents.zip) na instância local do AEM [usando o link de upload do arquivo de ativos](http://localhost:4502/assets.html/content/dam)
1. [Visualizar formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. Guia para a guia Documents to Review. Você deve ver três documentos PDF no componente Carrossel.
