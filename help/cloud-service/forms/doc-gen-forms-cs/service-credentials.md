---
title: Credenciais de serviço do AEM Forms
description: Baixar credenciais de serviço da Developer Console do AEM.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
duration: 453
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# Credenciais de serviço do AEM Forms

As integrações com o AEM as a Cloud Service devem ser capazes de autenticar com segurança para AEM. O Developer Console da AEM gera Credenciais de serviço, que são usadas por aplicativos, sistemas e serviços externos para interagir programaticamente com os serviços do AEM Author ou do Publish por HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

O arquivo de credenciais de serviço baixado é armazenado como um arquivo de recurso chamado service_token.json no eclipse fornecido. Os valores no arquivo service_token são usados para gerar o JWT e trocar o JWT por um token de acesso. A classe de utilitário GetServiceCredentials é usada para buscar os valores de propriedade do arquivo de recursos service_token.json.
