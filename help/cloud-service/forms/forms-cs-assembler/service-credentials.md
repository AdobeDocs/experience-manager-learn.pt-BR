---
title: Credenciais de serviço do AEM
description: Baixe as credenciais de serviço do Developer Console da AEM.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-9980
exl-id: 4c5173f1-d57d-43ac-83e6-399ce4ead203
duration: 453
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 0%

---

# Credenciais de serviço

As integrações com o AEM as a Cloud Service devem ser capazes de autenticar com segurança no AEM. O Developer Console da AEM gera Credenciais de serviço, que são usadas por aplicativos, sistemas e serviços externos para interagir programaticamente com os serviços de Autor ou Publicação do AEM por HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/342227?quality=12&learn=on&captions=por_br)

O arquivo de credenciais de serviço baixado é armazenado como um arquivo de recurso chamado service_token.json no eclipse fornecido. Os valores no arquivo service_token são usados para gerar o JWT e trocar o JWT por um token de acesso. A classe de utilitário GetServiceCredentials é usada para buscar os valores de propriedade do arquivo de recursos service_token.json.
