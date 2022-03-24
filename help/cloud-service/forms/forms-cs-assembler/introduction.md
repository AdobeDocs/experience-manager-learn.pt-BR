---
title: Manipulação de PDF no Forms CS usando a operação invocar DDX
description: Monte arquivos PDF usando invocar DDX.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9980
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 1%

---

# Introdução

Neste curso, usaremos a manipulação de PDF e o arquivamento de documentos do PDF usando o Forms CS. O uso desses microsserviços de um aplicativo externo envolve as seguintes etapas:

1. Gerar credenciais de serviço para uma conta técnica AEM
1. Crie um JSON Web Token (JWT) a partir das credenciais do serviço e troque o mesmo por um token de acesso
1. Configure o acesso da conta técnica no AEM
1. Efetuar chamadas HTTP usando o token de acesso para manipular arquivos PDF/gerar e validar arquivos PDF/A
