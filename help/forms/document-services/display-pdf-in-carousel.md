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
duration: 66
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 1%

---

# Exibir vários documentos em pdf em um carrossel

Um caso de uso comum é exibir vários documentos de PDF para o preenchimento do formulário para revisão antes de enviar o formulário.

Para concluir este caso de uso, utilizamos a [API de Incorporação do Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[Uma demonstração ao vivo desta amostra pode ser vista aqui.](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

As etapas a seguir foram executadas para concluir a integração

## Criar um componente personalizado para exibir vários documentos PDF

Um componente personalizado (pdf-carousel) foi criado para navegar pelos documentos pdf

## Biblioteca do cliente

Uma biblioteca do cliente foi criada para exibir os PDF usando a API incorporada do Adobe PDF. Os PDF a serem exibidos são especificados nos componentes do carrossel pdf.

## Criar formulário adaptável

Crie um formulário adaptável com base em algumas guias (este exemplo tem 3 guias)
Adicione alguns componentes de formulário adaptáveis nas duas primeiras guias
Adicione o componente Carrossel pdf na terceira guia
Configure o componente pdf-carousel conforme mostrado na captura de tela abaixo
![pdf-carousel](assets/pdf-carousel-af-component.png)

**Incorporar Chave de API de PDF** - Esta é a chave que você pode usar para incorporar o pdf. Essa chave só funcionará com localhost. Você pode criar [sua própria chave](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) e associá-la a outro domínio.

**Especificar documentos PDF** - Aqui você pode especificar os documentos pdf que deseja exibir no carrossel.


## Implantar a amostra no servidor

Para testar isso no servidor local, siga as etapas:

1. [Importe a biblioteca do cliente](assets/pdf-carousel-client-lib.zip) para sua instância do AEM local [usando o gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importe o componente carrossel pdf](assets/pdf-carousel-component.zip) para sua instância de AEM local [usando o gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importe o Formulário Adaptável](assets/adaptive-form-pdf-carousel.zip) para sua instância do AEM local [usando o gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importe os pdf de exemplo para exibição](assets/pdf-carousel-sample-documents.zip) na instância de AEM local [usando o link de carregamento de arquivo de ativos](http://localhost:4502/assets.html/content/dam)
1. [Visualizar formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. Guia para a guia Documents to Review. Você deve ver três documentos PDF no componente Carrossel.
