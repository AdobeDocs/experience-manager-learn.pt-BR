---
title: Implante os ativos de amostra no servidor
description: Teste a funcionalidade salvar como rascunho para Comunicações interativas
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
source-git-commit: db99787c48e49a9861de893e6cb7fbb7b31807b8
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 2%

---

# Implante os ativos de amostra no servidor

Siga as instruções abaixo para obter essa funcionalidade funcionando no servidor AEM

* [Criar o esquema do banco de dados](assets/icdrafts.sql)
* [Importe a biblioteca do cliente](assets/icdrafts.zip)
* [Importar o formulário adaptável](assets/SavedDraftsAdaptiveForm.zip)
* Criar fonte de dados chamada _SaveAndContinue_

![Criar fonte de dados](assets/data-source.png)

| Nome da Propriedade | Valor da propriedade |
|---|---|
| Nome da origem de dados | SaveAndContinue |
| Classe de driver JDBC | com.mysql.cj.jdbc.Driver |
| URL de conexão JDBC | jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&amp;useSSL=false&amp;characterEncoding=utf8&amp;useUnicode=true |

* [Implantar o pacote de ícones](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Certifique-se de que _Habilitar Salvar Usando CCRDocumentInstanceService_ na configuração OSGI, como mostrado abaixo
   ![Ativar rascunhos](assets/enable-drafts.png)
* Abra qualquer comunicação interativa. Clique em Salvar como rascunho para salvar
* [Exibir rascunhos salvos](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>Os arquivos xml são armazenados na pasta raiz da instalação do servidor AEM. O projeto do eclipse é fornecido a você para personalizar a solução de acordo com suas necessidades.

O projeto do eclipse com implementação de amostra pode ser [baixado aqui](assets/icdrafts-eclipse-project.zip)
