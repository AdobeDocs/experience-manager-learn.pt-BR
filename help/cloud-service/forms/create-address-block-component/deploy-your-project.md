---
title: Criando componente de endereço
description: Criação do novo componente principal de endereço no AEM Forms Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
source-git-commit: a8fc8fa19ae19e27b07fa81fc931eca51cb982a1
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Implante seu projeto

Antes de começar a implantar o projeto no Cloud Service AEM Forms, é recomendável implantar o projeto na instância do AEM Forms pronta para nuvem local.

## Sincronizar alterações com o projeto AEM

Inicie o IntelliJ e navegue até a pasta Formulário adaptável na ``ui.apps`` conforme mostrado abaixo
![intellij](assets/intellij.png)

Clique com o botão direito do mouse em ``adaptiveForm`` e selecione Novo | Pacote Certifique-se de adicionar o nome **addressblock** ao pacote

Clique com o botão direito no pacote recém-criado ``addressblock`` e selecione ``repo | Get Command`` conforme mostrado abaixo
![repo-sync](assets/sync-repo.png)

Isso deve sincronizar o projeto com a instância do AEM Forms local pronta para nuvem. Você pode verificar o arquivo .content.xml para confirmar as propriedades
![pós-sincronização](assets/after-sync.png)

## Implantar projeto na instância local

Inicie uma nova janela de prompt de comando e navegue até a pasta raiz do projeto e crie o projeto usando o comando mostrado abaixo
![implantar](assets/build-project.png)

Depois que o projeto for implantado com êxito, o componente de Endereço poderá ser usado em um Formulário adaptável

## Implantar o projeto no ambiente de nuvem

Se tudo estiver bem em seu ambiente de desenvolvimento local, a próxima etapa é implantar no [instância da nuvem usando o cloud manager.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)



