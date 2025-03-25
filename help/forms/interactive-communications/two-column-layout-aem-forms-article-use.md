---
title: Criação de dois layouts de coluna para documentos de canal de impressão
description: Criar layouts de 2 colunas para o documento de canal de impressão
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 416e3401-ba9f-4da3-8b07-2d39f9128571
last-substantial-update: 2019-07-07T00:00:00Z
duration: 44
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---

# Layouts de duas colunas no documento de canal de impressão

Este pequeno artigo destacará as etapas necessárias para criar o layout de 2 colunas no canal de impressão. O caso de uso é gerar documentos de duas páginas, com a página 1 tendo o layout de duas colunas e a página 2 tendo o layout de coluna padrão 1.

Veja a seguir as etapas de alto nível envolvidas na criação de layouts de duas colunas usando o AEM Forms Designer.

* Criar 2 áreas de conteúdo na página 1 da página mestra
* Nomeie as 2 áreas de conteúdo como &quot;left column&quot; e &quot;right column&quot;
* Criar segunda página mestra com uma área de conteúdo (esse é o padrão)
* Selecione a guia Paginação (Subformulário sem título) (página 1) e (Subformulário sem título) (página 2) e defina as propriedades conforme mostrado nas capturas de tela abaixo.

![página1](assets/untitledsubform_paginationproperties.gif)

![página2](assets/untitled_subformpage2.gif)

Depois que as propriedades de paginação são definidas, podemos adicionar subformulários ou áreas de destino em (Subformulário sem título) (Página 1).

Então, podemos adicionar fragmentos de documento a esses subformulários ou áreas de destino. Quando a coluna da esquerda estiver cheia, o conteúdo fluirá para a coluna da direita.

Para testar isso no servidor local, baixe os ativos relacionados a este artigo. Rolar para baixo até a parte inferior desta página

* [Baixe e instale o Exemplo de documento do canal de impressão usando o gerenciador de pacotes](assets/print-channel-with-two-column-layout.zip)
* [Visualizar o Documento do Canal de Impressão](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
