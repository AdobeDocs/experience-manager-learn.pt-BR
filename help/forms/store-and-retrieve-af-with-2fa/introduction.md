---
title: Armazenando e Recuperando Dados de Formulário com anexos do Banco de Dados MySQL
description: Tutorial de várias partes para orientá-lo pelas etapas envolvidas no armazenamento e recuperação de dados de formulário com anexos
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 2%

---

# Armazenamento e recuperação de dados de formulários adaptáveis com o 2FA

Este tutorial o guiará pelas etapas envolvidas na gravação e recuperação de dados de formulário adaptável com anexos usando o 2FA. Este tutorial usou o banco de dados MySQL para armazenar dados do Formulário adaptável. O banco de dados de sua escolha pode ser usado para armazenar os dados, desde que você tenha implantado os drivers específicos do banco de dados no AEM. Em um alto nível, as seguintes etapas são necessárias para alcançar o caso de uso:

* Use a API GuideBridge para obter acesso aos dados do formulário adaptável

* Faça uma chamada de POST para um servlet. Esse servlet armazena os dados no banco de dados e os anexos de formulário no repositório CRX. Os dados armazenados no banco de dados estão associados a um GUID.

* Quando quiser preencher o Formulário adaptativo com os dados armazenados, recupere os dados associados ao GUID e preencha o Formulário adaptável usando o **request.setAttribute** método .

## Demonstração do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## Pré-requisitos

Espera-se que o público-alvo desse conteúdo tenha alguma experiência nas seguintes áreas:

* Formulário adaptável
* Modelo de dados do formulário
* Serviços/componentes do OSGi
* Bibliotecas de clientes AEM


## Próximas etapas

[Configuração da Fonte de Dados](./configure-data-source.md)