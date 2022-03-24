---
title: Testar a solução
description: Execute o ExecuteAssemblerService.java para testar a solução
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---

# Importar projeto do Eclipse

* Baixe e descompacte o [arquivo zip](./assets/pdf-manipulation.zip)
* Inicie o Eclipse e importe o projeto para o Eclipse
* O projeto inclui as seguintes pastas na pasta de recursos:
   * ddxFiles - Esta pasta contém o arquivo ddx para descrever a saída que você deseja gerar
   * pdffiles - Esta pasta contém os arquivos pdf que você deseja montar

![resources-file](./assets/resources.png)

## Testar a solução

* Copie e cole suas credenciais de serviço no arquivo de recurso service_token.json no projeto.
* Abra o arquivo AssemblePDFFiles.java e especifique a pasta na qual deseja salvar os arquivos PDF gerados
* Abra ExecuteAssemblerService.java. Defina o valor da variável assembleURL para apontar para a sua instância.
* Execute o ExecuteAssemblerService.java como aplicativo java

>[!NOTE]
> Na primeira vez que você executar o programa java, aparecerá o erro HTTP 403. Para superar isso, certifique-se de fornecer o [permissões apropriadas para o usuário da conta técnica no AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**Usuários do AEM Forms** Foi o papel que usei para este curso.
