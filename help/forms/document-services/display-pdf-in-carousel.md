---
title: Exibir vários documentos pdf
description: Faça o ciclo de vários documentos pdf em um formulário adaptável.
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
kt: 10292
source-git-commit: cb5b3eb77a57fa8a2918710b7dbcd1b0a58b74bd
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 4%

---

# Exibir vários documentos pdf em um carrossel

Um caso de uso comum é exibir vários documentos PDF para o usuário a serem revisados antes de enviar o formulário.

Para realizar esse caso de uso, utilizamos a variável [API de incorporação do Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[Uma demonstração ao vivo dessa amostra pode ser realizada aqui.](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

As etapas a seguir foram executadas para concluir a integração

## Criar um componente personalizado para exibir vários documentos de PDF

Um componente personalizado (pdf-carrossel) foi criado para percorrer documentos pdf

## Biblioteca do cliente

Uma biblioteca do cliente foi criada para exibir as PDF usando a API do Adobe PDF Embed. As PDF a serem exibidas são especificadas nos componentes do carrossel de pdf.

## Criar formulário adaptável

Crie um formulário adaptável com base em algumas guias (Essa amostra tem 3 guias) Adicione alguns componentes de formulário adaptáveis nas duas primeiras guias Adicione o componente do carrossel pdf na terceira guia Configurar o componente do carrossel pdf, conforme mostrado na captura de tela abaixo
![pdf-carousel](assets/pdf-carousel-af-component.png)

**Incorporar chave de API do PDF** - Essa é a chave que você pode usar para incorporar o pdf. Essa chave só funcionará com localhost. Você pode criar [sua própria chave](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) e associá-lo a outro domínio.

**Especificar Documentos de PDF** - Aqui você pode especificar os documentos pdf que deseja exibir no carrossel.


## Implante a amostra no servidor

Para testar isso no servidor local, siga as etapas:

1. [Importe a biblioteca do cliente](assets/pdf-carousel-client-lib.zip) na instância de AEM local [usando o gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importe o componente do carrossel pdf](assets/pdf-carousel-component.zip) na instância de AEM local [usando o gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importar o formulário adaptável ](assets/adaptive-form-pdf-carousel.zip) na instância de AEM local [usando o gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importe os pdfs de amostra para exibir](assets/pdf-carousel-sample-documents.zip) na instância de AEM local [usando o link de upload do arquivo de ativos](http://localhost:4502/assets.html/content/dam)
1. [Visualizar formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. Guia para a guia Documents to Review . Você deve ver três documentos do PDF no componente carrossel.
