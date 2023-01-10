---
title: Selecionar e baixar conteúdo da pasta DAM
description: Implante os ativos tutoriais na instância de AEM local
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: a2bbb26751c9182056b4fe6d36eeeec964001df8
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 1%

---

# Implantar no sistema

Siga as etapas listadas abaixo para que esse caso de uso funcione na instância de AEM local.

* [Configure para usar o usuário do fd-service seguindo as etapas mencionadas neste artigo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en). Certifique-se de ter implantado o pacote DevelopingWithServiceUser.

* [Implantar o pacote de informativos](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Este pacote contém o código para listar o conteúdo da pasta e reunir os informativos selecionados.

* [Importe o pacote usando o Gerenciador de Pacotes](assets/newsletter.zip). Este pacote contém a biblioteca do cliente e arquivos pdf de amostra para testar a solução.

* [Importe o formulário adaptável de amostra](assets/sample-adaptive-form.zip). Este formulário listará os boletins informativos que podem ser selecionados.

* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Selecione alguns boletins informativos para baixar.Os boletins informativos selecionados serão combinados em um pdf e retornados para você.




