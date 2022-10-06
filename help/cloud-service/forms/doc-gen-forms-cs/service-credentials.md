---
title: Credenciais do serviço AEM Forms
description: Baixe as credenciais do serviço AEM Console do desenvolvedor.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---

# Credenciais do serviço AEM Forms

As integrações com AEM as a Cloud Service devem ser autenticadas com segurança para AEM. AEM Console do desenvolvedor gera Credenciais de serviço, que são usadas por aplicativos, sistemas e serviços externos para interagir programaticamente com os serviços de Autor ou Publicação do AEM por HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

O arquivo de credenciais de serviço baixado é armazenado como um arquivo de recurso chamado service_token.json no eclipse fornecido. Os valores no arquivo service_token são usados para gerar o JWT e trocar o JWT por um Token de acesso. A classe de utilitário GetServiceCredentials é usada para buscar os valores da propriedade do arquivo de recurso service_token.json .
