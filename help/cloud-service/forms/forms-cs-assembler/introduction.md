---
title: Manipulação de PDF no Forms CS usando a operação de invocar DDX
description: Combine arquivos de PDF usando invocar DDX.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 18
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 0%

---

# Introdução

Neste curso, usaremos a manipulação de PDF e o arquivamento de documentos PDF usando o Forms CS. Usar esses microsserviços de um aplicativo externo envolve as seguintes etapas:

1. Gerar credenciais de serviço para uma conta técnica do AEM
1. Criar um JSON Web Token (JWT) a partir das credenciais de serviço e trocar o mesmo por um token de acesso
1. Configuração do acesso à conta técnica no AEM
1. Fazer chamadas HTTP usando o token de acesso para manipular arquivos PDF/gerar e validar arquivos PDF/A
