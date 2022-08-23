---
title: Armazenando e Recuperando Dados de Formulário da Introdução ao Banco de Dados do MySQL
description: Tutorial de várias partes para orientá-lo pelas etapas envolvidas no armazenamento e recuperação de dados do formulário
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# Armazenando e Recuperando Dados de Formulário Adaptável do Banco de Dados do MySQL

Este tutorial o guiará pelas etapas envolvidas na gravação e recuperação de dados de formulário adaptável do banco de dados. Este tutorial usou o banco de dados MySQL para armazenar dados do Formulário adaptável. O banco de dados de sua escolha pode ser usado para armazenar os dados, desde que você tenha implantado os drivers específicos do banco de dados no AEM. Em um alto nível, as seguintes etapas são necessárias para alcançar o caso de uso:

* Use a API GuideBridge para obter acesso aos dados do formulário adaptável

* Faça uma chamada de POST para um servlet. Este servlet armazena os dados no banco de dados. Os dados armazenados estão associados a um GUID

* Quando quiser preencher o Formulário adaptativo com os dados armazenados, recupere os dados associados ao GUID e preencha o Formulário adaptável usando o **request.setAttribute** método .

## Demonstração do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)
