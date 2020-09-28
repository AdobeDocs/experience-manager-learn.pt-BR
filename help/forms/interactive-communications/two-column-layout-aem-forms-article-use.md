---
title: Criação de dois layouts de coluna para documentos de canais de impressão
seo-title: Criação de dois layouts de coluna para documentos de canais de impressão
description: Criar 2 layouts de coluna para o documento de canal de impressão
seo-description: Criar 2 layouts de coluna para o documento de canal de impressão
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 314f798f7a80f9c554e5bea052f8a64ae397d0de
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---


# Dois layouts de coluna no Documento Canal de impressão

Este breve artigo destacará as etapas necessárias para criar o layout de 2 colunas no canal de impressão. O caso de uso é gerar 2 documentos de página com a página 1 com layout de 2 colunas e a página 2 com layout de 1 coluna padrão.

Veja a seguir as etapas de alto nível envolvidas na criação de 2 layouts de coluna usando o AEM Forms Designer.

* Criar 2 áreas de conteúdo na página 1 principal
* Nomeie as 2 áreas de conteúdo como &quot;coluna esquerda&quot; e &quot;coluna direita&quot;
* Criar segunda página principal com uma área de conteúdo (este é o padrão)
* Selecione a guia paginação (Subformulário sem título) (página 1) e (Subformulário sem título) (página 2) e defina as propriedades como mostrado nas capturas de tela abaixo.

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

Depois que as propriedades de paginação estiverem definidas, poderemos adicionar subformulários ou áreas de público alvo em (Subformulário sem título) (Página 1).

Em seguida, podemos adicionar fragmentos de documento a esses subformulários ou áreas de público alvo. Quando a coluna esquerda estiver cheia, o conteúdo fluirá para a coluna direita.

Para testar isso no servidor local, baixe os ativos relacionados a este artigo. Role para baixo até a parte inferior desta página

* [Baixe e instale o exemplo de Documento de impressão usando o gerenciador de pacotes](assets/print-channel-with-two-column-layout.zip)
* [Pré-visualização do Documento do Canal de impressão](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
