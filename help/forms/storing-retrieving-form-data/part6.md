---
title: Armazenamento e Recuperação de Dados de Formulário do Banco de Dados MySQL - Implantar
description: Tutorial de várias partes para orientá-lo pelas etapas envolvidas no armazenamento e recuperação de dados do formulário
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: 6.3,6.4,6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 2%

---

# Implante no servidor

>[!NOTE]
>
>Os itens a seguir são necessários para que isso seja executado em seu sistema
>
>* AEM Forms (versão 6.3 ou superior)
>* Banco de Dados MySql


Para testar esse recurso na instância do AEM Forms, siga as etapas a seguir

* Baixe e implante o [Jar do Controlador MySql](assets/mysqldriver.jar) arquivos usando o [console da web felix](http://localhost:4502/system/console/bundles)
* Baixe e implante o [Pacote OSGi](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) usando o [console da web felix](http://localhost:4502/system/console/bundles)
* Baixe e instale o [pacote contendo a biblioteca do cliente, o modelo de formulário adaptável e o componente de página personalizada](assets/store-and-fetch-af-with-data.zip) usando o [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Importe o [formulário adaptável de amostra](assets/sample-adaptive-form.zip) usando o [Interface FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importe o [form-data-db.sql](assets/form-data-db.sql) usando o MySql Workbench. Isso criará o schema e as tabelas necessárias no banco de dados para que este tutorial funcione.
* Faça logon em [configMgr.](http://localhost:4502/system/console/configMgr) Procure por &quot;Fonte de dados agrupada da conexão Apache Sling&quot;. Criar uma nova entrada de fonte de dados agrupada da conexão Apache Sling chamada **SaveAndContinue** usando as seguintes propriedades:

| Nome da Propriedade | Valor |
| ------------------------|---------------------------------------|
| Nome da origem de dados | SaveAndContinue |
| Classe de Driver JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexão JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

* Abra o [Formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Preencha alguns detalhes e clique no botão &quot;Salvar e continuar mais tarde&quot;.
* Você deve recuperar um URL com um GUID.
* Copie o URL e cole-o em uma nova guia do navegador. **Verifique se não há espaço vazio no final do URL.**
* O formulário adaptável deve ser preenchido com os dados da etapa anterior.
