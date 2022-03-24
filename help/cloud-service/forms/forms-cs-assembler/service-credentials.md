---
title: Credenciais do Serviço AEM
description: Baixe as credenciais do serviço AEM Console do desenvolvedor.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 9980
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# Credenciais de Serviço

As integrações com AEM as a Cloud Service devem ser autenticadas com segurança para AEM. O Console do desenvolvedor da AEM gera Credenciais de serviço, que são usadas por aplicativos, sistemas e serviços externos para interagir programaticamente com os serviços de Autor ou Publicação do AEM por HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

O arquivo de credenciais de serviço baixado é armazenado como um arquivo de recurso chamado service_token.json no eclipse fornecido. Os valores no arquivo service_token são usados para gerar o JWT e trocar o JWT por um Token de acesso. A classe de utilitário GetServiceCredentials é usada para buscar os valores da propriedade do arquivo de recurso service_token.json .
