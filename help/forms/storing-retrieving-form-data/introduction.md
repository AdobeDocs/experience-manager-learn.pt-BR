---
title: Armazenamento e Recuperação de Dados de Formulário do Banco de Dados MySQL
description: Tutorial de várias peças para orientá-lo pelas etapas envolvidas no armazenamento e recuperação de dados do formulário
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 0%

---


# Armazenamento e Recuperação de Dados de Formulário Adaptável do Banco de Dados MySQL

Este tutorial o guiará pelas etapas envolvidas ao salvar e recuperar os dados do formulário adaptável do banco de dados. Este tutorial usou o banco de dados MySQL para armazenar dados do Formulário adaptável. O banco de dados de sua escolha pode ser usado para armazenar os dados, desde que você tenha implantado os drivers específicos do banco de dados em AEM. Em um nível alto, as etapas a seguir são necessárias para obter o caso de uso:

* Usar a API GuideBridge para obter acesso aos dados do Formulário adaptável

* Faça uma chamada de POST para um servlet. Este servlet armazena os dados no banco de dados. Os dados armazenados estão associados a um GUID

* Quando quiser preencher o Formulário adaptativo com os dados armazenados, recupere os dados associados ao GUID e preencha o Formulário adaptável usando o método **request.setAttribute**.

## Demonstração do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)
