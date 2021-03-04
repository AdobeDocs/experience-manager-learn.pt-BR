---
title: Armazenando e Recuperando Dados de Formulário do Banco de Dados MySQL
description: Tutorial de várias partes para orientá-lo pelas etapas envolvidas no armazenamento e recuperação de dados do formulário
feature: formulários adaptáveis
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '279'
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

* Baixe e implante os arquivos [MySql Driver Jar](assets/mysqldriver.jar) usando o [console Web felix](http://localhost:4502/system/console/bundles)
* Baixe e implante o [pacote OSGi](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) usando o [console da Web felix](http://localhost:4502/system/console/bundles)
* Baixe e instale o pacote [contendo a biblioteca do cliente, o modelo de formulário adaptável e o componente de página personalizado](assets/store-and-fetch-af-with-data.zip) usando o [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Importe o [formulário adaptativo de amostra](assets/sample-adaptive-form.zip) usando a [interface FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importe o [form-data-db.sql](assets/form-data-db.sql) usando o MySql Workbench. Isso criará o schema e as tabelas necessárias no banco de dados para que este tutorial funcione.
* Faça logon em [configMgr.](http://localhost:4502/system/console/configMgr) Procure por &quot;Fonte de dados agrupada da conexão Apache Sling&quot;. Crie uma nova entrada de fonte de dados agrupada da conexão Apache Sling chamada **SaveAndContinue** usando as seguintes propriedades:

| Nome da Propriedade | Valor |
------------------------|---------------------------------------
| Nome da origem de dados | SaveAndContinue |
| Classe de Driver JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexão JDBC | jdbc:mysql://localhost:3306/aemformstutorial |


* Abra o [Formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Preencha alguns detalhes e clique no botão &quot;Salvar e continuar mais tarde&quot;.
* Você deve recuperar um URL com um GUID.
* Copie o URL e cole-o em uma nova guia do navegador. **Verifique se não há espaço vazio no final do URL.**
* O formulário adaptável deve ser preenchido com os dados da etapa anterior.
