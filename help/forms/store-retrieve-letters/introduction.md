---
title: Salvar e retomar cartas
seo-title: Save and resume letters
description: Saiba como salvar e recuperar letras de rascunho
seo-description: Learn how to save and retrieve draft letters
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Introdução

As Comunicações interativas permitem que os agentes que preparam correspondências ad-hoc salvem correspondências parcialmente concluídas e recuperem as mesmas para continuar trabalhando. A AEM Forms fornece a [Interface do provedor de serviços](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). Espera-se que o cliente implemente essa interface para obter a funcionalidade Salvar e Retomar .

Este artigo usa o banco de dados MySQL para armazenar os metadados da instância da carta. Os dados da carta são armazenados no sistema de arquivos.

O vídeo a seguir demonstra o caso de uso:

>[!VIDEO](https://video.tv.adobe.com/v/342129?quality=12&learn=on)

## Pré-requisitos

Você precisará do seguinte para implementar a solução para atender às suas necessidades

* Experiência de trabalho com o AEM Forms
* AEM Server 6.5 com o Forms Add on
* Deve ser familiar na criação de pacotes OSGI
