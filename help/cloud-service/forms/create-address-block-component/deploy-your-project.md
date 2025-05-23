---
title: Criando componente de endereço
description: Criação de um novo componente principal de endereço no AEM Forms as a Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
exl-id: be25be52-2914-4820-9356-678a326f8edc
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Implante seu projeto

Antes de começar a implantar o projeto no AEM Forms as a Cloud Service, é recomendável implantar o projeto na instância do AEM Forms pronta para nuvem local.

## Sincronizar alterações com seu projeto do AEM

Inicie o IntelliJ e navegue até a pasta adaptiveForm na pasta ``ui.apps``, conforme mostrado abaixo
![intellij](assets/intellij.png)

Clique com o botão direito do mouse no nó ``adaptiveForm`` e selecione Novo | Pacote
Adicione o nome **addressblock** ao pacote

Clique com o botão direito do mouse no pacote recém-criado ``addressblock`` e selecione ``repo | Get Command`` conforme mostrado abaixo
![repo-sync](assets/sync-repo.png)

Isso deve sincronizar o projeto com a instância do AEM Forms local pronta para nuvem. Você pode verificar o arquivo .content.xml para confirmar as propriedades
![pós-sincronização](assets/after-sync.png)

## Implantar projeto na instância local

Inicie uma nova janela de prompt de comando e navegue até a pasta raiz do projeto e crie o projeto usando o comando mostrado abaixo
![implantar](assets/build-project.png)

Depois que o projeto for implantado com êxito, a variável
O componente de Endereço agora pode ser usado em um Formulário adaptável

## Implantar o projeto no ambiente de nuvem

Se tudo estiver bem no seu ambiente de desenvolvimento local, a próxima etapa é implantar na instância da nuvem [usando o cloud manager.](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)
