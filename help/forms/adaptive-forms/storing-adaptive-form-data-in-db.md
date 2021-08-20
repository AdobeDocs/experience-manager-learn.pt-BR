---
title: Armazenamento de dados de formulário adaptável
description: Armazenamento de dados de formulários adaptáveis no DataBase como parte do fluxo de trabalho do AEM
feature: Forms adaptável, Modelo de dados de formulário
version: 6.3,6.4,6.5
topic: Desenvolvimento
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '380'
ht-degree: 0%

---


# Armazenamento de envios de formulário adaptável no banco de dados

Há várias maneiras de armazenar os dados de formulário enviados no banco de dados de sua escolha. Uma fonte de dados JDBC pode ser usada para armazenar diretamente os dados no banco de dados. Um pacote OSGI personalizado pode ser gravado para armazenar os dados no banco de dados. Este artigo usa a etapa de processo personalizado AEM fluxo de trabalho para armazenar os dados.
O caso de uso é acionar um fluxo de trabalho AEM em um envio de formulário adaptável e uma etapa no fluxo de trabalho armazena os dados enviados para a base de dados.

**Siga as etapas mencionadas abaixo para que isso funcione em seu sistema**

* [Baixe o arquivo Zip e extraia seu conteúdo para o disco rígido](assets/storeafdataindb.zip)

   * Importe o StoreAFInDBWorkflow.zip no AEM usando o gerenciador de pacotes. O pacote tem um fluxo de trabalho de amostra que armazena os dados AF no DB. Abra o modelo de fluxo de trabalho. O fluxo de trabalho tem apenas uma etapa. Esta etapa chama o código gravado no pacote para armazenar os dados AF no Banco de Dados. Estou a transmitir um único argumento ao processo. Este é o nome do formulário adaptativo cujos dados estão sendo salvos.
   * Implante o insertdata.core-0.0.1-SNAPSHOT.jar usando o console da Web Felix. Esse pacote tem o código para gravar os dados de formulário enviados no banco de dados

* Vá para [ConfigMgr](http://localhost:4502/system/console/configMgr)

   * Procure por &quot;Pool de Conexões JDBC&quot;. Crie um novo Pool de Conexões JDBC do Day Commons. Especifique as configurações específicas do seu banco de dados.

   * ![pool de conexão jdbc](assets/jdbc-connection-pool.png)
   * Procure por &quot;**Inserir dados do formulário no DB**&quot;
   * Especifique as propriedades específicas do banco de dados.
      * DataSourceName:Name da fonte de dados configurada anteriormente.
      * TableName - Nome da tabela na qual você deseja armazenar os dados AF
      * FormName - Nome da coluna para conter o nome do Formulário
      * ColumnName - Nome da coluna para conter os dados AF

   ![insertdata](assets/insertdata.PNG)

* Crie um formulário adaptável.

* Associe o formulário adaptável a AEM fluxo de trabalho (StoreAFValuesinDB), conforme mostrado na captura de tela abaixo.

* Certifique-se de especificar &quot;data.xml&quot; no caminho do arquivo de dados, conforme mostrado na captura de tela abaixo

   ![submissão](assets/submissionafforms.png)

* Visualize o formulário e envie-o

* Se tudo correr bem, você verá os dados do formulário armazenados na tabela e coluna especificadas por você



