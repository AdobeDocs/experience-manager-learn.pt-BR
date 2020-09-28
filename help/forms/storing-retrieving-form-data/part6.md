---
title: Armazenamento e Recuperação de Dados de Formulário do Banco de Dados MySQL
description: Tutorial de várias peças para orientá-lo pelas etapas envolvidas no armazenamento e recuperação de dados do formulário
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 6ae8110d4f4bc80682c35b0dab3fe7a62cad88f3
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---


# Implantar no servidor

>[!NOTE]
Os seguintes itens são necessários para que isso seja executado no seu sistemaAEM Forms(versão 6.3 ou superior)MYSQL Database

Para testar esse recurso em sua instância do AEM Forms, siga as etapas a seguir

* Baixe e descompacte os ativos [do](assets/store-retrieve-form-data.zip) tutorial no seu sistema local
* Implante e start os pacotes techmarketingdemos.jar e mysqldriver.jar usando o console da Web [Felix](http://localhost:4502/system/console/configMgr)
* Importe o aemformstutorial.sql usando o MYSQL Workbench. Isso criará o schema e as tabelas necessários no banco de dados para que este tutorial funcione.
* Importe StoreAndRetrieve.zip usando [AEM gerenciador de pacote.](http://localhost:4502/crx/packmgr/index.jsp) Este pacote contém o modelo de formulário adaptável, a biblioteca do cliente do componente de página e a amostra de configuração de formulário adaptável e fonte de dados.
* Faça logon no [configMgr.](http://localhost:4502/system/console/configMgr) Procure &quot;Apache Sling Connection Pooling DataSource. Abra a entrada da fonte de dados associada ao aemformstutorial e insira o nome de usuário e a senha específicos da instância do banco de dados.
* Abra o formulário [adaptativo](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Preencha alguns detalhes e clique no botão &quot;Salvar e continuar mais tarde&quot;
* Você deve recuperar um URL com um GUID.
* Copie o URL e cole-o em uma nova guia do navegador. **Verifique se não há espaço vazio no final do URL**
* O formulário adaptável deve ser preenchido com os dados da etapa anterior
