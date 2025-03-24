---
title: Manipulação de PDF no Forms CS usando a operação de invocar DDX
description: Combine arquivos PDF usando invocar DDX.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 18
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 0%

---

# Introdução

Neste curso, usaremos a manipulação e o arquivamento de documentos do PDF pelo PDF usando o Forms CS. Usar esses microsserviços de um aplicativo externo envolve as seguintes etapas:

1. Gerar credenciais de serviço para uma conta técnica do AEM
1. Criar um JSON Web Token (JWT) a partir das credenciais de serviço e trocar o mesmo por um token de acesso
1. Configuração do acesso à conta técnica no AEM
1. Fazer chamadas HTTP usando o token de acesso para manipular arquivos PDF/gerar e validar arquivos PDF/A
