---
title: Salvar e retomar cartas
description: Saiba como salvar e recuperar rascunhos de cartas
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
duration: 160
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# Introdução

As Comunicações interativas permitem que os agentes que preparam correspondências ad-hoc salvem correspondências parcialmente concluídas e recuperem as mesmas para continuar trabalhando. A AEM Forms fornece a [Interface do Provedor de Serviços](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). Espera-se que o cliente implemente essa interface para obter a funcionalidade Salvar e retomar.

Este artigo usa o banco de dados MySQL para armazenar os metadados da instância de carta. Os dados da carta são armazenados no sistema de arquivos.

O vídeo a seguir demonstra o caso de uso:

>[!VIDEO](https://video.tv.adobe.com/v/342129?quality=12&learn=on)

## Pré-requisitos

Você precisará do seguinte para implementar a solução que atenda às suas necessidades

* Experiência de trabalho com o AEM Forms
* Servidor AEM 6.5 com Forms Add on
* Deve se familiarizar com a criação de pacotes OSGI
