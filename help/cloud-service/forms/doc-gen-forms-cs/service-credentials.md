---
title: Credenciais do Serviço AEM
description: Baixe as credenciais do serviço AEM Console do desenvolvedor.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: Formulários adaptáveis
topic: Desenvolvimento
kt: 8192
thumbnail: 330519.jpg
source-git-commit: f2a94910fbc29b705f82a66d8248cbcf54366874
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 2%

---


# Credenciais de Serviço

As integrações com o AEM como um Cloud Service devem ser capazes de autenticar com segurança para AEM. O Console do desenvolvedor da AEM gera Credenciais de serviço, que são usadas por aplicativos, sistemas e serviços externos para interagir programaticamente com os serviços de Autor ou Publicação do AEM por HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

O arquivo de credenciais de serviço baixado é armazenado como um arquivo de recurso chamado service_token.json no eclipse fornecido. Os valores no arquivo service_token são usados para gerar o JWT e trocar o JWT por um Token de acesso. A classe de utilitário GetServiceCredentials é usada para buscar os valores da propriedade do arquivo de recurso service_token.json .
