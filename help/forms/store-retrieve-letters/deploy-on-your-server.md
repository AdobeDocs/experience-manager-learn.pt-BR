---
title: Implantar os ativos de amostra no servidor
description: Testar a funcionalidade Salvar como rascunho para as Comunicações interativas
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
duration: 28
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 2%

---

# Implantar os ativos de amostra no servidor

Siga as instruções abaixo para fazer com que essa funcionalidade funcione no seu servidor AEM

* [Criar o esquema de banco de dados](assets/icdrafts.sql)
* [Importar a biblioteca do cliente](assets/icdrafts.zip)
* [Importar o formulário adaptável](assets/SavedDraftsAdaptiveForm.zip)
* Criar fonte de dados chamada _SaveAndContinue_

![Criar fonte de dados](assets/data-source.png)

| Nome de propriedade | Valor de propriedade |
|---|---|
| Nome da fonte de dados | `SaveAndContinue` |
| Classe de driver JDBC | `com.mysql.cj.jdbc.Driver` |
| URL de conexão JDBC | `jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&useSSL=false&characterEncoding=utf8&useUnicode=true` |

* [Implantar o pacote icdrafts](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Verifique se você _Habilitar Salvar Usando CCRDocumentInstanceService_ na configuração OSGI, como mostrado abaixo
  ![Ativar rascunhos](assets/enable-drafts.png)
* Abra qualquer comunicação interativa. Clique no botão Salvar como rascunho para salvar
* [Visualizar Rascunhos Salvos](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>Os arquivos xml são armazenados na pasta raiz da instalação do servidor AEM. O projeto do eclipse é fornecido a você para personalizar a solução de acordo com suas necessidades.

O projeto do eclipse com implementação de amostra pode ser [baixado aqui](assets/icdrafts-eclipse-project.zip)
