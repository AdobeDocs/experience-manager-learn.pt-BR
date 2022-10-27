---
title: Criação de dois layouts de coluna para documentos de canal de impressão
seo-title: Creating two column layouts for print channel documents
description: Criar 2 layouts de coluna para documento de canal de impressão
seo-description: Create 2 column layouts for print channel document
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 416e3401-ba9f-4da3-8b07-2d39f9128571
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Dois layouts de coluna no documento de canal de impressão

Este breve artigo destacará as etapas necessárias para criar o layout de 2 colunas no canal de impressão. O caso de uso é gerar 2 documentos de página com página 1 com layout de 2 colunas e página 2 com layout de 1 coluna padrão.

Veja a seguir as etapas de alto nível envolvidas na criação de layouts de duas colunas usando o AEM Forms Designer.

* Criar 2 áreas de conteúdo na página 1 principal
* Nomeie as 2 áreas de conteúdo como &quot;coluna da esquerda&quot; e &quot;coluna da direita&quot;
* Crie a segunda página principal com uma área de conteúdo (esse é o padrão)
* Selecione a guia paginação (Subformulário sem título) (página 1) e (Subformulário sem título) (página 2) e defina as propriedades conforme mostrado nas capturas de tela abaixo.

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

Depois que as propriedades de paginação forem definidas, poderemos adicionar subformulários ou áreas de destino em (Subformulário sem título) (Página 1).

Em seguida, podemos adicionar fragmentos de documento a esses subformulários ou áreas de destino. Quando a coluna da esquerda está cheia, o conteúdo fluirá para a coluna da direita.

Para testar isso no servidor local, baixe os ativos relacionados a este artigo. Role para baixo até a parte inferior desta página

* [Baixe e instale o Exemplo de documento do canal de impressão usando o gerenciador de pacotes](assets/print-channel-with-two-column-layout.zip)
* [Visualizar o documento do canal de impressão](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
