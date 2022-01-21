---
title: Atualização do projeto do serviço em nuvem com o arquétipo mais recente
description: Atualização do projeto do serviço em nuvem do AEM Forms com o arquétipo mais recente
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 9534
source-git-commit: cea9a9dc003b76369db1b7fedb9549062885258d
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 1%

---

# Migração do arquétipo antigo do aem

Para atualizar seu projeto AEM Forms existente com o arquétipo de maven mais recente, você terá que copiar manualmente seu código/configurações, etc., do projeto antigo para o novo projeto.

As etapas a seguir foram seguidas para migrar o projeto criado usando o arquétipo 30 para o arquétipo 33

## Criar projeto maven usando o arquétipo mais recente

* Abra o prompt de comando e navegue até c:\cloudmanager
* Crie um projeto maven usando o arquétipo mais recente.
* Copie e cole o conteúdo do [arquivo de texto](assets/creating-maven-project.txt) na janela da tela de comandos. Talvez seja necessário alterar DarchetypeVersion=33 dependendo do [versão mais recente](https://github.com/adobe/aem-project-archetype/releases). O arquétipo 33 inclui novos temas da AEM Forms.
Como estamos criando o novo projeto maven na pasta cloudmanager que já tem o projeto aem-banking-application, você deve alterar a variável **DartifactId** do aplicativo aem-banking a algo diferente. Eu usei o aem-banking-application1 para este artigo.

>[!NOTE]
>
>Se você implantar esse novo projeto como está a instância do serviço de nuvem não terá HandleFormSubmission e SubmitToAEMServlet. Isso ocorre porque toda vez que você implanta um projeto usando o cloud manager, qualquer item na pasta de aplicativos será excluído e substituído.

## Copiar seu código java

Depois que o projeto for criado com êxito, você poderá começar a copiar códigos/configurações, etc., do projeto antigo para esse novo projeto

* Copie o servlet HandleFormSubmission de ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
para

   ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* Copie o CustomSubmit de
   ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` do aem-banking-application ao aem-banking-application1 project

* importar o novo projeto para o IntelliJ

* Atualize o filter.xml no módulo ui.apps do projeto aem-banking-application1 para incluir a seguinte linha
   ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

Depois de copiar todo o código para o novo projeto, você pode enviar este projeto para o cloud manager.

>[!NOTE]
>
>Para sincronizar o conteúdo (Forms adaptável, Modelo de dados de formulário etc.) no novo projeto, será necessário criar a estrutura de pastas apropriada no projeto IntelliJ e, em seguida, sincronizar o projeto IntelliJ com a instância do AEM usando o comando Get da ferramenta de repo.
