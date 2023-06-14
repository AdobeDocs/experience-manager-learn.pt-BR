---
title: Enviar projeto AEM para o repositório do Cloud Manager
description: Enviar o repositório Git local para o repositório do cloud manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
source-git-commit: 10ff0d87991d7766d5ca9563062a2f7be6035e43
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 1%

---

# Enviar projeto AEM para o repositório Git do Cloud Manager

Na etapa anterior, sincronizamos nosso projeto AEM com o Forms adaptável e Temas criados na instância AEM.
Agora precisamos adicionar essas alterações ao repositório Git local e, em seguida, enviar o repositório Git local para o repositório Git do cloud manager.
Abra o prompt de comando e navegue até c:\cloudmanager\aem-banking-app Execute os seguintes comandos

```
git add .
```

Isso adiciona os novos arquivos à ramificação de preparo do repositório Git local

```
git commit -m "My First AF"
```

Isso confirma os arquivos na ramificação principal do repositório Git local

```
git push -f bankingapp master:"MyFirstAF"
```

No comando acima, estamos enviando nossa ramificação principal do repositório Git local para a ramificação MyFirstAF do repositório do Cloud Manager identificado pelo nome amigável do bankingapp.

## Próximas etapas

[Implantar o projeto no ambiente de desenvolvimento](./deploy-to-dev-environment.md)
