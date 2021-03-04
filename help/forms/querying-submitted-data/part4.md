---
title: AEM Forms com esquema JSON e dados[Parte4]
seo-title: AEM Forms com esquema JSON e dados[Parte4]
description: Tutorial de várias partes para orientá-lo pelas etapas envolvidas na criação do formulário adaptável com esquema JSON e consulta dos dados enviados.
seo-description: Tutorial de várias partes para orientá-lo pelas etapas envolvidas na criação do formulário adaptável com esquema JSON e consulta dos dados enviados.
feature: Formulários adaptáveis
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Desenvolvimento
role: Desenvolvedor
level: Experienciado
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 1%

---


# Consulta de dados enviados


A próxima etapa é consultar os dados enviados e exibir os resultados de forma tabular. Para isso, usaremos o seguinte software

[QueryBuilder](https://querybuilder.js.org/)  - componente da interface do usuário para criar consultas

[Tabelas de dados](https://datatables.net/) - Para exibir os resultados da consulta de forma tabular.

A interface do usuário a seguir foi criada para permitir a consulta dos dados enviados. Somente os elementos marcados como obrigatórios no esquema JSON são disponibilizados para query. Na captura de tela abaixo, estamos consultando todos os envios em que o delivery pref é SMS.

A interface de exemplo para consultar os dados enviados não usa todos os recursos avançados disponíveis no QueryBuilder. Você é encorajado a experimentá-los sozinho.

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>A versão atual deste tutorial não suporta a consulta de várias colunas.

Quando você seleciona um formulário para executar sua consulta, uma chamada GET é feita para **/bin/getdatakeysfromschema**. Essa chamada GET retorna os campos obrigatórios associados ao esquema dos formulários. Os campos obrigatórios são então preenchidos na lista suspensa do QueryBuilder para que você crie a consulta.

O trecho de código a seguir faz uma chamada para o método getRequiredColumnsFromSchema do serviço JSONSchemaOperations. Passamos as propriedades e os elementos necessários do schema para essa chamada de método. A matriz retornada por essa chamada de função é então usada para preencher a lista suspensa do construtor de consultas

```java
public JSONArray getData(String formName) throws SQLException, IOException {

  org.json.JSONArray arrayOfDataKeys = new org.json.JSONArray();
  JSONObject jsonSchema = jsonSchemaOperations.getJSONSchemaFromDataBase(formName);
  Map<String, String> refKeys = new HashMap<String, String>();

  try {
   JSONObject properties = jsonSchema.getJSONObject("properties");
   JSONArray requiredFields = jsonSchema.has("required") ? jsonSchema.getJSONArray("required") : null;
   jsonSchemaOperations.getRequiredColumnsFromSchema(properties, arrayOfDataKeys, "", jsonSchema, refKeys,
     requiredFields);
  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return arrayOfDataKeys;

 }
```

Quando o botão GetResult é clicado, uma chamada Get é feita para **&quot;/bin/querydata&quot;**. Passamos a consulta criada pela interface do usuário do QueryBuilder para o servlet pelo parâmetro de consulta . Em seguida, o servlet massaja essa consulta em uma consulta SQL que pode ser usada para consultar o banco de dados. Por exemplo, se você estiver pesquisando para recuperar todos os produtos chamados &#39;Mouse&#39;, a sequência de consulta do Construtor de consultas será $.productname = &#39;Mouse&#39;. Esse query será convertido no seguinte

SELECIONE * from aemformswithjson .  envios de formulários, onde JSON_EXTRACT( envios de formulários .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;

O resultado dessa consulta é retornado para preencher a tabela na interface do usuário do .

Para executar este exemplo em seu sistema local, execute as seguintes etapas

1. [Siga todas as etapas mencionadas aqui](part2.md)
1. [Importe o Dashboardv2.zip usando o Gerenciador de pacotes do AEM.](assets/dashboardv2.zip) Este pacote contém todos os pacotes necessários, configurações, envio personalizado e página de amostra para consultar dados.
1. Criar um formulário adaptável usando o exemplo de esquema json
1. Configure o formulário adaptável para enviar para a ação de envio personalizada &quot;customsubmithelpx&quot;
1. Preencha o formulário e envie
1. Aponte seu navegador para [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. Selecione o formulário e execute uma consulta simples

