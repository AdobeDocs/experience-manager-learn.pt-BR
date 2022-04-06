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
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# Implante os ativos de amostra no servidor

Siga as instruções abaixo para obter essa funcionalidade funcionando no servidor AEM

* Crie uma pasta chamada icrascunhos na unidade c
* [Criar o esquema do banco de dados](assets/icdrafts.sql)
* [Importe a biblioteca do cliente](assets/icdrafts.zip)
* [Importar o formulário adaptável](assets/SavedDraftsAdaptiveForm.zip)
* Criar fonte de dados chamada _SaveAndContinue_

![Criar fonte de dados](assets/data-source.png)

* [Implantar o pacote de ícones](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Certifique-se de que _Habilitar Salvar Usando CCRDocumentInstanceService_ na configuração OSGI, como mostrado abaixo
   ![Ativar rascunhos](assets/enable-drafts.png)
* Abra qualquer comunicação interativa. Clique em Salvar como rascunho para salvar
* [Exibir rascunhos salvos](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>Os arquivos xml são armazenados na pasta raiz da instalação do servidor AEM. O projeto do eclipse é fornecido a você para personalizar a solução de acordo com suas necessidades.

O projeto do eclipse com implementação de amostra pode ser [baixado aqui](assets/icdrafts-eclipse-project.zip)
