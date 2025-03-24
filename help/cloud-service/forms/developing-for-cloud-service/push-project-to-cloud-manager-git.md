---
title: Enviar projeto do AEM para o repositório do cloud manager
description: Enviar o repositório Git local para o repositório do cloud manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
duration: 32
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 1%

---

# Enviar projeto do AEM para o repositório Git do cloud manager

Na etapa anterior, sincronizamos nosso projeto do AEM com o Forms adaptável e Temas criados na instância do AEM.
Agora precisamos adicionar essas alterações ao repositório Git local e, em seguida, enviar o repositório Git local para o repositório Git do cloud manager.
Abra o prompt de comando e navegue até c:\cloudmanager\aem-banking-app
Execute os seguintes comandos

```
git add .
```

Isso adiciona os novos arquivos à ramificação de preparo do repositório Git local

```
git commit -m "My First AF"
```

Isso confirma os arquivos na ramificação mestre do repositório Git local

```
git push -f bankingapp master:"MyFirstAF"
```

No comando acima, estamos enviando a ramificação mestre do repositório Git local para a ramificação MyFirstAF do repositório do Cloud Manager identificado pelo nome amigável do bankingapp.

## Próximas etapas

[Implantar o projeto no ambiente de desenvolvimento](./deploy-to-dev-environment.md)
