---
title: Testar a solução
description: Execute o ExecuteAssemblerService.java para testar a solução
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---

# Importar projeto do Eclipse

* Baixe e descompacte o [arquivo zip](./assets/pdf-manipulation.zip)
* Inicie o Eclipse e importe o projeto para o Eclipse
* O projeto inclui as seguintes pastas na pasta de recursos:
   * ddxFiles - Esta pasta contém o arquivo ddx para descrever a saída que você deseja gerar
   * pdffiles - Essa pasta contém os arquivos pdf que você deseja montar e arquivos pdf para testar utilitários PDFA
   * credenciais - Esta pasta contém o arquivo pdfa-options.json

![resources-file](./assets/resources.png)

## Testar Montagem de Arquivos PDF

* Copie e cole suas credenciais de serviço no arquivo de recurso service_token.json no projeto.
* Abra o arquivo AssemblePDFFiles.java e especifique a pasta na qual deseja salvar os arquivos PDF gerados
* Abra ExecuteAssemblerService.java. Defina o valor da variável _AEM_FORMS_CS_ para apontar para a sua instância.
* Exclua as linhas apropriadas para testar a montagem de dois ou mais arquivos PDF
* Execute o ExecuteAssemblerService.java como aplicativo java

### Testar utilitários PDFA

* Copie e cole suas credenciais de serviço no arquivo de recurso service_token.json no projeto.
* Abra o arquivo PDFAUtilities.java e especifique a pasta na qual deseja salvar os arquivos PDF gerados.
* Abra ExecuteAssemblerService.java. Defina o valor da variável _AEM_FORMS_CS_ para apontar para a sua instância.
* Exclua as linhas apropriadas para testar as operações do PDFA.
* Execute o ExecuteAssemblerService.java como aplicativo java.



>[!NOTE]
> Na primeira vez que você executar o programa java, aparecerá o erro HTTP 403. Para superar isso, certifique-se de fornecer o [permissões apropriadas para o usuário da conta técnica no AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**Usuários do AEM Forms** Foi o papel que usei para este curso.
