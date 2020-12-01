---
title: AEM Forms com Schema JSON e dados[Parte4]
seo-title: AEM Forms com Schema JSON e dados[Parte4]
description: Tutorial de várias peças para orientá-lo pelas etapas envolvidas na criação do Formulário adaptável com o schema JSON e consulta dos dados enviados.
seo-description: Tutorial de várias peças para orientá-lo pelas etapas envolvidas na criação do Formulário adaptável com o schema JSON e consulta dos dados enviados.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 0%

---


# Consultando dados enviados


A próxima etapa é query dos dados enviados e exibir os resultados de forma tabular. Para conseguir isso, usaremos o seguinte software

[QueryBuilder](https://querybuilder.js.org/)  - componente de interface para criar query

[Tabelas](https://datatables.net/) de dados - Para exibir os resultados do query de forma tabular.

A interface do usuário a seguir foi criada para permitir a consulta dos dados enviados. Somente os elementos marcados como obrigatórios no schema JSON são disponibilizados para o query. Na captura de tela abaixo, estamos consultando todos os envios nos quais o pref de entrega é SMS.

A UI de amostra para query dos dados enviados não usa todos os recursos avançados disponíveis no QueryBuilder. Você é encorajado a experimentá-los sozinho.

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>A versão atual deste tutorial não suporta a consulta de várias colunas.

Quando você seleciona um formulário para executar seu query, uma chamada de GET é feita para **/bin/getdatakeysfromschema**. Essa chamada de GET retorna os campos obrigatórios associados ao schema dos formulários. Os campos obrigatórios são então preenchidos na lista suspensa do QueryBuilder para que você crie o query.

O trecho de código a seguir faz uma chamada para o método getRequiredColumnsFromSchema do serviço JSONSchemaOperations. Enviamos as propriedades e os elementos obrigatórios do schema para essa chamada de método. A matriz retornada por esta chamada de função é então usada para preencher a lista suspensa do construtor de query

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

Quando o botão GetResult é clicado, uma chamada Get é feita para **&quot;/bin/querydata&quot;**. Passamos o query criado pela interface do usuário do QueryBuilder para o servlet pelo parâmetro do query. O servlet massageia esse query no query SQL que pode ser usado para query do banco de dados. Por exemplo, se você estiver procurando recuperar todos os produtos chamados &#39;Mouse&#39;, a sequência de query do Construtor de Query será $.productname = &#39;Mouse&#39;. Esse query será convertido em

SELECIONE * de aemformswithjson .  formulários onde JSON_EXTRACT( formsubmete .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;

O resultado desse query é retornado para preencher a tabela na interface do usuário.

Para executar este exemplo no sistema local, execute as seguintes etapas

1. [Verifique se você seguiu todas as etapas mencionadas aqui](part2.md)
1. [Importe o Dashboardv2.zip usando AEM Gerenciador de pacotes.](assets/dashboardv2.zip) Este pacote contém todos os pacotes necessários, configurações, envio personalizado e página de amostra para os dados do query.
1. Criar um formulário adaptável usando o schema json de amostra
1. Configure o formulário adaptativo para enviar à ação de envio personalizada &quot;customsubmithelpx&quot;
1. Preencha o formulário e envie
1. Aponte seu navegador para [painel.html](http://localhost:4502/content/AemForms/dashboard.html)
1. Selecione o formulário e execute o query simples

