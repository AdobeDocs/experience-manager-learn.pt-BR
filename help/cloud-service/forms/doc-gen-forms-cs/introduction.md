---
title: Microserviços de geração de documentos no AEM Forms CS
description: Considere os microsserviços de geração de documentos de um aplicativo externo.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: Serviços de documento
topic: Desenvolvimento
kt: 7432
thumbnail: 332439.jpg
source-git-commit: 33cb3d18b744d9a8e54a87152079b42ed09212f2
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 4%

---

# Introdução

Neste curso, usaremos os microsserviços de geração de documentos para gerar um pdf ao mesclar dados com um template XDP. O uso desses microsserviços de um aplicativo externo envolve as seguintes etapas:

1. Gerar credenciais de serviço para uma conta técnica AEM
1. Crie um JSON Web Token (JWT) a partir das credenciais do serviço e troque o mesmo por um token de acesso
1. Configure o acesso da conta técnica no AEM
1. Efetuar chamadas HTTP usando o token de acesso