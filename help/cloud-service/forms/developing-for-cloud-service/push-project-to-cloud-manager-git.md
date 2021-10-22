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
source-git-commit: 9063c3dfd9ab9ac537850694ce6545a3fdc840e9
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---


# Encaminhar AEM projeto para o git repo do cloud manager

Na etapa anterior, sincronizamos o AEM Project com a Adaptive Forms e os Themes criados na instância AEM.
Agora precisamos adicionar essas alterações ao nosso repositório Git local e, em seguida, enviar o repositório Git local para o repositório Git do cloud manager.
Abra o prompt de comando e navegue até c:\cloudmanager\aem-banking-app Execute the following commands

```
git add .**
```

Isso adiciona os novos arquivos à ramificação de preparo do repositório Git local

```
git commit -m "My First AF"
```

Isso confirma os arquivos na ramificação principal do repositório Git local

```
git push -f bankingapp master:"MyFirstAF"
```

No comando acima, estamos enviando nossa ramificação principal do repositório Git local para a ramificação MyFirstAF do repositório do cloud manager identificado pelo nome amigável do aplicativo bancário.



