---
title: Armazenando e Recuperando Dados de Formulário do Banco de Dados MySQL
description: Tutorial de várias partes para orientá-lo pelas etapas envolvidas no armazenamento e recuperação de dados do formulário
feature: Formulários adaptáveis
type: Tutorial
version: 6.3,6.4,6.5
topic: Desenvolvimento
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 1%

---


# Armazenando e Recuperando Dados de Formulário Adaptável do Banco de Dados do MySQL

Este tutorial o guiará pelas etapas envolvidas na gravação e recuperação de dados de formulário adaptável do banco de dados. Este tutorial usou o banco de dados MySQL para armazenar dados do Formulário adaptável. O banco de dados de sua escolha pode ser usado para armazenar os dados, desde que você tenha implantado os drivers específicos do banco de dados no AEM. Em um alto nível, as seguintes etapas são necessárias para alcançar o caso de uso:

* Use a API GuideBridge para obter acesso aos dados do formulário adaptável

* Faça uma chamada de POST para um servlet. Este servlet armazena os dados no banco de dados. Os dados armazenados estão associados a um GUID

* Quando quiser preencher o Formulário adaptativo com os dados armazenados, recupere os dados associados ao GUID e preencha o Formulário adaptável usando o método **request.setAttribute**.

## Demonstração do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)
