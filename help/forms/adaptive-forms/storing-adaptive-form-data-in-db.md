---
title: Armazenamento de dados de formulário adaptável
seo-title: Armazenamento de dados de formulário adaptável
description: Armazenamento de dados de formulário adaptável no DataBase como parte do fluxo de trabalho do AEM
seo-description: Armazenamento de dados de formulário adaptável no DataBase como parte do fluxo de trabalho do AEM
feature: adaptive-forms,workflow
topics: integrations
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 0%

---


# Armazenamento de Submissões de Formulário Adaptável no Banco de Dados

Há várias maneiras de armazenar os dados de formulário enviados no banco de dados de sua escolha. Uma fonte de dados JDBC pode ser usada para armazenar diretamente os dados no banco de dados. Um pacote OSGI personalizado pode ser gravado para armazenar os dados no banco de dados. Este artigo usa a etapa de processo personalizada AEM fluxo de trabalho para armazenar os dados.
O caso de uso é acionar um fluxo de trabalho AEM em um envio de formulário adaptável e uma etapa no fluxo de trabalho armazena os dados enviados na base de dados.

**Siga as etapas mencionadas abaixo para que isso funcione no seu sistema**

* [Baixe o arquivo Zip e extraia seu conteúdo no disco rígido](assets/storeafdataindb.zip)

   * Importe StoreAFInDBWorkflow.zip para AEM usando o gerenciador de pacotes. O pacote tem um fluxo de trabalho de amostra que armazena os dados AF no DB. Abra o modelo de fluxo de trabalho. O fluxo de trabalho tem apenas uma etapa. Esta etapa chama o código gravado no pacote para armazenar os dados AF no Banco de Dados. Estou a transmitir um único argumento ao processo. Esse é o nome do Formulário adaptativo cujos dados estão sendo salvos.
   * Implante o insitdata.core-0.0.1-SNAPSHOT.jar usando o console da Web do Felix. Esse pacote tem o código para gravar os dados do formulário enviado no banco de dados

* Ir para o [ConfigMgr](http://localhost:4502/system/console/configMgr)

   * Procure &quot;JDBC Connection Pool&quot;. Crie um novo Pool de Conexões JDBC do Day Commons. Especifique as configurações específicas do banco de dados.

   * ![pool de conexões jdbc](assets/jdbc-connection-pool.png)
   * Procurar &quot;**Inserir Dados de Formulário no DB**&quot;
   * Especifique as propriedades específicas do banco de dados.
      * DataSourceName:Nome da fonte de dados configurada anteriormente.
      * TableName - Nome da tabela na qual deseja armazenar os dados AF
      * FormName - Nome da coluna para manter o nome do Formulário
      * ColumnName - Nome da coluna para conter os dados AF

   ![inserir dados](assets/insertdata.PNG)

* Criar um formulário adaptável.

* Associe o formulário adaptativo ao fluxo de trabalho AEM(StoreAFValuesinDB), conforme mostrado na captura de tela abaixo.

* Certifique-se de especificar &quot;data.xml&quot; no caminho do arquivo de dados, como mostrado na captura de tela abaixo

   ![submissão](assets/submissionafforms.png)

* Pré-visualização do formulário e envio

* Se tudo correr bem, você deverá ver os Dados do formulário sendo armazenados na tabela e coluna especificadas por você



