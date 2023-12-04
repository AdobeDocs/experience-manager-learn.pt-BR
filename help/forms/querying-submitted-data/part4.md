---
title: AEM Forms com esquema e dados JSON [Part4]
description: Tutorial em várias partes para orientá-lo pelas etapas envolvidas na criação do Formulário adaptável com esquema JSON e na consulta dos dados enviados.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a8d8118d-f4a1-483f-83b4-77190f6a42a4
duration: 135
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# Consulta de dados enviados


A próxima etapa é consultar os dados enviados e exibir os resultados em forma de tabela. Para isso, estamos usando o seguinte software:

[QueryBuilder](https://querybuilder.js.org/) - Componente da interface do usuário para criar consultas

[Tabelas de dados](https://datatables.net/)- Para exibir os resultados da consulta em forma de tabela.

A interface de usuário a seguir foi criada para permitir a consulta dos dados enviados. Somente os elementos marcados como obrigatórios no esquema JSON são disponibilizados para consulta. Na captura de tela abaixo, estamos consultando todos os envios em que a pref de entrega é SMS.

A interface de exemplo para consultar os dados enviados não usa todos os recursos avançados disponíveis no QueryBuilder. Você é incentivado a experimentá-los por conta própria.

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>A versão atual deste tutorial não oferece suporte à consulta de várias colunas.

Quando você seleciona um formulário para executar o query, é feita uma chamada GET para **/bin/getdatakeysfromschema**. Essa chamada GET retorna os campos obrigatórios associados ao esquema dos formulários. Os campos obrigatórios são preenchidos na lista suspensa do QueryBuilder para que você crie a consulta.

O trecho de código a seguir faz uma chamada para o método getRequiredColumnsFromSchema do serviço JSONSchemaOperations. Passamos as propriedades e os elementos necessários do esquema para essa chamada de método. A matriz retornada por essa chamada de função é usada para preencher a lista suspensa do construtor de consultas

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

Quando o botão GetResult é clicado, uma chamada Get é feita para **&quot;/bin/querydata&quot;**. Passamos a consulta criada pela interface do usuário do QueryBuilder para o servlet por meio do parâmetro de consulta. O servlet então faz uma massa dessa consulta na consulta SQL que pode ser usada para consultar o banco de dados. Por exemplo, se você estiver procurando para recuperar todos os produtos chamados &quot;Mouse&quot;, a sequência de consulta do Construtor de consultas será `$.productname = 'Mouse'`. Esta query será convertida no seguinte

SELECIONE &#42; do aemformswithjson.  formsubmissions onde JSON_EXTRACT( formsubmissions .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;

O resultado dessa consulta é retornado para preencher a tabela na interface do usuário.

Para executar esse exemplo em seu sistema local, execute as seguintes etapas

1. [Verifique se você seguiu todas as etapas mencionadas aqui](part2.md)
1. [Importe o Dashboardv2.zip usando o Gerenciador de pacotes do AEM.](assets/dashboardv2.zip) Este pacote contém todos os pacotes necessários, definições de configuração, envio personalizado e página de exemplo para consultar dados.
1. Criar um formulário adaptável usando a amostra de esquema json
1. Configurar o formulário adaptável para enviar para a ação de envio personalizada &quot;customsubmithelpx&quot;
1. Preencha o formulário e envie
1. Aponte seu navegador para [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. Selecione o formulário e execute uma consulta simples
