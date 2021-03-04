---
title: Armazenando e Recuperando Dados de Formulário com anexos do Banco de Dados MySQL
description: Tutorial de várias partes para orientá-lo pelas etapas envolvidas no armazenamento e recuperação de dados de formulário com anexos
feature: Formulários adaptáveis
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Desenvolvimento
role: Desenvolvedor
level: Experienciado
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 4%

---


# Armazenamento e recuperação de dados de formulários adaptáveis com o 2FA

Este tutorial o guiará pelas etapas envolvidas na gravação e recuperação de dados de formulário adaptável com anexos usando o 2FA. Este tutorial usou o banco de dados MySQL para armazenar dados do Formulário adaptável. O banco de dados de sua escolha pode ser usado para armazenar os dados, desde que você tenha implantado os drivers específicos do banco de dados no AEM. Em um alto nível, as seguintes etapas são necessárias para alcançar o caso de uso:

* Use a API GuideBridge para obter acesso aos dados do formulário adaptável

* Faça uma chamada POST para um servlet. Esse servlet armazena os dados no banco de dados e os anexos de formulário no repositório CRX. Os dados armazenados no banco de dados estão associados a um GUID.

* Quando quiser preencher o Formulário adaptativo com os dados armazenados, recupere os dados associados ao GUID e preencha o Formulário adaptável usando o método **request.setAttribute**.

## Demonstração do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## Pré-requisitos

Espera-se que o público-alvo desse conteúdo tenha alguma experiência nas seguintes áreas:

* Formulário adaptativo
* Modelo de dados do formulário
* Serviços/componentes do OSGi
* Bibliotecas de clientes do AEM
