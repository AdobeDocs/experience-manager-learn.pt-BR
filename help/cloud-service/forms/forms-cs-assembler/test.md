---
title: Testar a solução do montador do Forms
description: Execute o ExecuteAssemblerService.java para testar a solução
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
duration: 55
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Importar projeto Eclipse

* Baixe e descompacte o [arquivo zip](./assets/pdf-manipulation.zip)
* Iniciar o Eclipse e importar o projeto para o Eclipse
* O projeto inclui as seguintes pastas na pasta de recursos:
   * ddxFiles - esta pasta contém o arquivo ddx para descrever a saída que você deseja gerar
   * pdffiles - Essa pasta contém os arquivos pdf que você deseja reunir e os arquivos pdf para testar os utilitários PDFA
   * credenciais - Esta pasta contém o arquivo pdfa-options.json

![arquivo-recursos](./assets/resources.png)

## Teste de montagem de arquivos PDF

* Copie e cole suas credenciais de serviço no arquivo de recurso service_token.json no projeto.
* Abra o arquivo AssemblePDFFiles.java e especifique a pasta na qual deseja salvar os arquivos PDF gerados
* Abra ExecuteAssemblerService.java. Defina o valor da variável _AEM_FORMS_CS_ para apontar para sua instância.
* Remova o comentário das linhas apropriadas para testar a montagem de dois ou mais arquivos PDF
* Execute o ExecuteAssemblerService.java como aplicativo java

### Testar utilitários PDFA

* Copie e cole suas credenciais de serviço no arquivo de recurso service_token.json no projeto.
* Abra o arquivo PDFAUtilities.java e especifique a pasta na qual deseja salvar os arquivos PDF gerados.
* Abra ExecuteAssemblerService.java. Defina o valor da variável _AEM_FORMS_CS_ para apontar para sua instância.
* Remova o comentário das linhas apropriadas para testar operações de PDFA.
* Execute o ExecuteAssemblerService.java como aplicativo java.



>[!NOTE]
> Na primeira vez que você executa o programa java, receberá o erro HTTP 403. Para passar por isso, certifique-se de fornecer as [permissões apropriadas ao usuário da conta técnica no AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**Usuários do AEM Forms** é a função que usei neste curso.
