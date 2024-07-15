---
title: Armazenamento e recuperação de dados de formulário com anexos do banco de dados MySQL
description: Tutorial em várias partes para orientá-lo pelas etapas envolvidas no armazenamento e na recuperação de dados de formulário com anexos
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
duration: 148
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 2%

---

# Armazenamento e recuperação de dados do formulário adaptável com 2FA

Este tutorial guiará você pelas etapas envolvidas no salvamento e na recuperação de dados do formulário adaptável com anexos usando 2FA. Este tutorial usou o banco de dados MySQL para armazenar dados do Formulário adaptável. O banco de dados de sua escolha pode ser usado para armazenar os dados, desde que você tenha implantado os drivers específicos do banco de dados no AEM. Em um alto nível, as seguintes etapas são necessárias para obter o caso de uso:

* Usar a API do GuideBridge para obter acesso aos dados do Formulário adaptável

* Fazer uma chamada de POST para um servlet. Esse servlet armazena os dados no banco de dados e os anexos de formulário no repositório do CRX. Os dados armazenados no banco de dados são associados a um GUID.

* Quando quiser preencher o formulário adaptável com os dados armazenados, você recuperará os dados associados ao GUID e preencherá o formulário adaptável usando o método **request.setAttribute**.

## Demonstração do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## Pré-requisitos

O público-alvo desse conteúdo deve ter alguma experiência nas seguintes áreas:

* Formulário adaptável
* Modelo de dados do formulário
* Serviços/componentes do OSGi
* Bibliotecas de clientes AEM


## Próximas etapas

[Configuração do Data Source](./configure-data-source.md)