---
title: Armazenamento e Recuperação de Dados de Formulário do Banco de Dados MySQL Introdução
description: Tutorial em várias partes para orientá-lo pelas etapas envolvidas no armazenamento e na recuperação de dados de formulário
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
duration: 236
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# Armazenamento e recuperação de dados do formulário adaptável do banco de dados MySQL

Este tutorial guiará você pelas etapas envolvidas no salvamento e na recuperação de dados do formulário adaptável do banco de dados. Este tutorial usou o banco de dados MySQL para armazenar dados do Formulário adaptável. O banco de dados de sua escolha pode ser usado para armazenar os dados, desde que você tenha implantado os drivers específicos do banco de dados no AEM. Em um alto nível, as seguintes etapas são necessárias para obter o caso de uso:

* Usar a API do GuideBridge para obter acesso aos dados do Formulário adaptável

* Fazer uma chamada de POST para um servlet. Esse servlet armazena os dados no banco de dados. Os dados armazenados estão associados a um GUID

* Quando quiser preencher o formulário adaptável com os dados armazenados, você recuperará os dados associados ao GUID e preencherá o formulário adaptável usando o método **request.setAttribute**.

## Demonstração do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)


