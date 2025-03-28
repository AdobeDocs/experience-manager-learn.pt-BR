---
title: Atualizando projeto do Cloud Service com o arquétipo mais recente
description: Atualização do projeto do AEM Forms Cloud Service com o arquétipo mais recente
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: AEM Project Archetype
jira: KT-9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
duration: 67
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# Migração do arquétipo do aem antigo

Para atualizar seu projeto AEM Forms existente com o arquétipo maven mais recente, será necessário copiar manualmente seu código/configurações etc., do projeto antigo para o novo projeto.

As etapas a seguir foram seguidas para migrar o projeto criado usando o arquétipo 30 para o projeto arquétipo 33

## Criar projeto Maven usando o arquétipo mais recente

* Abra o prompt de comando e navegue até c:\cloudmanager
* Crie um projeto maven usando o arquétipo mais recente.
* Copie e cole o conteúdo do [arquivo de texto](assets/creating-maven-project.txt) na janela do prompt de comando. Talvez seja necessário alterar DarchetypeVersion=33 dependendo da [versão mais recente](https://github.com/adobe/aem-project-archetype/releases). O Arquétipo 33 inclui novos temas do AEM Forms.
Como estamos criando o novo projeto maven na pasta cloudmanager que já tem o projeto aem-banking-application, você deve alterar a **DartifactId** de aem-banking-application para algo diferente. Eu usei o aem-banking-application1 para este artigo.

>[!NOTE]
>
>Se você implantar esse novo projeto como está, a instância do Cloud Service não terá HandleFormSubmission e SubmitToAEMervlet. Isso ocorre porque toda vez que você implanta um projeto usando o Cloud Manager, qualquer item na pasta `/apps` é excluído e substituído.

## Copie seu código java

Depois que o projeto for criado com êxito, você pode começar a copiar códigos/configurações etc., do projeto antigo para o novo projeto

* Copiar o servlet HandleFormSubmission de ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
para
  ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* Copiar o CustomSubmit de
  ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` de aem-banking-application para projeto aem-banking-application1

* importar o novo projeto para o IntelliJ

* Atualize o filter.xml no módulo ui.apps do projeto aem-banking-application1 para incluir a seguinte linha
  ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

Depois de copiar todo o código para o novo projeto, você pode enviar este projeto para o Cloud Manager.

>[!NOTE]
>
>Para sincronizar o conteúdo (Forms Adaptável, Modelo de Dados de Formulário etc.) no novo projeto, você terá que criar a estrutura de pastas apropriada no projeto IntelliJ e sincronizar o projeto IntelliJ com a instância do AEM usando o comando Obter da ferramenta de repositório.
