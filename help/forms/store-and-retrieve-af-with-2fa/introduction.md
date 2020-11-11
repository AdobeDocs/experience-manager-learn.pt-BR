---
title: Armazenamento e recuperação de dados de formulário com anexos do banco de dados MySQL
description: Tutorial de várias partes para orientá-lo pelas etapas envolvidas no armazenamento e recuperação de dados de formulário com anexos
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---


# Armazenamento e recuperação de dados de formulário adaptável com o 2FA

Este tutorial o guiará pelas etapas envolvidas na gravação e recuperação de Dados de formulário adaptável com anexos usando o 2FA. Este tutorial usou o banco de dados MySQL para armazenar dados do Formulário adaptável. O banco de dados de sua escolha pode ser usado para armazenar os dados, desde que você tenha implantado os drivers específicos do banco de dados em AEM. Em um nível alto, as etapas a seguir são necessárias para obter o caso de uso:

* Usar a API GuideBridge para obter acesso aos dados do Formulário adaptável

* Faça uma chamada de POST para um servlet. Este servlet armazena os dados no banco de dados e os anexos de formulário no repositório CRX. Os dados armazenados no banco de dados estão associados a um GUID.

* Quando quiser preencher o Formulário adaptável com os dados armazenados, recupere os dados associados ao GUID e preencha o Formulário adaptável usando o método **request.setAttribute** .

## Demonstração do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## Pré-requisitos

Espera-se que a audiência deste conteúdo tenha alguma experiência nas seguintes áreas:

* Formulário adaptativo
* Modelo de dados do formulário
* Serviços/componentes OSGi
* Bibliotecas do cliente AEM
