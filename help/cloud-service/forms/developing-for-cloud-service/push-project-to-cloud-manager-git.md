---
title: Encaminhar AEM projeto para o repositório do cloud manager
description: Encaminhe o repositório Git local para o repositório do cloud manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---


# Encaminhar AEM projeto para o git rep do cloud manager

Na etapa anterior, sincronizamos o AEM Project com a Adaptive Forms e os Themes criados na instância AEM.
Agora precisamos adicionar essas alterações ao nosso repositório Git local e, em seguida, enviar o repositório Git local para o repositório Git do cloud manager

abra o prompt de comando e navegue até c:\cloudmanager\aem-banking-app Execute the following commands

```
git add .**
```

Isso adiciona os novos arquivos à ramificação de preparo do repositório Git local

```
git commit -m "My First AF"
```

Isso confirma os arquivos na ramificação principal do repositório Git local

```
git push -f bankingapp master:"My First AF"
```

No comando acima, estamos enviando nossa ramificação principal do repositório Git local para a ramificação My First AF do repositório do cloud manager identificada pelo nome amigável bankingapp



