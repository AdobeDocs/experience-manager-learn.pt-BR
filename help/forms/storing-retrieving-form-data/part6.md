---
title: Armazenamento e recuperação de dados de formulário do banco de dados MySQL - Implantar
description: Tutorial em várias partes para orientá-lo pelas etapas envolvidas no armazenamento e na recuperação de dados de formulário
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: 6.4,6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
duration: 47
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 1%

---

# Implantar no servidor

>[!NOTE]
>
>Os itens a seguir são necessários para que isso funcione no sistema
>
>* AEM Forms(versão 6.3 ou superior)
>* Banco de Dados MySql

Para testar esse recurso na sua instância do AEM Forms, siga as etapas a seguir

* Baixe e implante os arquivos [Jar do Driver MySql](assets/mysqldriver.jar) usando o [console Web felix](http://localhost:4502/system/console/bundles)
* Baixe e implante o [pacote OSGi](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) usando o [console Web felix](http://localhost:4502/system/console/bundles)
* Baixe e instale o [pacote que contém a biblioteca do cliente, o modelo de formulário adaptável e o componente da página personalizada](assets/store-and-fetch-af-with-data.zip) usando o [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Importar o [formulário adaptável de amostra](assets/sample-adaptive-form.zip) usando a [interface FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importe o [form-data-db.sql](assets/form-data-db.sql) usando o MySql Workbench. Isso criará o esquema e as tabelas necessárias no banco de dados para que este tutorial funcione.
* Faça logon no [configMgr.](http://localhost:4502/system/console/configMgr) Procure por &quot;DataSource com pool de conexão do Apache Sling. Crie uma nova entrada de fonte de dados agrupada da conexão Apache Sling chamada **SaveAndContinue** usando as seguintes propriedades:

| Nome de propriedade | Valor |
| ------------------------|---------------------------------------|
| Nome da fonte de dados | `SaveAndContinue` |
| Classe de driver JDBC | `com.mysql.cj.jdbc.Driver` |
| URI de conexão JDBC | `jdbc:mysql://localhost:3306/aemformstutorial` |

* Abra o [Formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Preencha alguns detalhes e clique no botão &quot;Salvar e continuar mais tarde&quot;.
* Você deve recuperar um URL com um GUID.
* Copie o URL e cole-o em uma nova guia do navegador. **Verifique se não há espaço vazio no final da URL.**
* O formulário adaptável deve ser preenchido com os dados da etapa anterior.
