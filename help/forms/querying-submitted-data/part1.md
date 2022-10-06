---
title: AEM Forms com esquema JSON e dados[Parte 1]
seo-title: AEM Forms with JSON Schema and Data[Part1]
description: Tutorial de várias partes para orientá-lo pelas etapas envolvidas na criação do formulário adaptável com esquema JSON e consulta dos dados enviados.
seo-description: Multi-Part tutorial to walk you through the steps involved in creating Adaptive Form with JSON schema and querying the submitted data.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Criar formulário adaptável com base no esquema JSON


A capacidade de criar o Adaptive Forms com base no esquema JSON foi introduzida com a versão AEM Forms 6.3. Os detalhes sobre a criação do Adaptive Forms com esquema JSON são explicados detalhadamente nesta [artigo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/adaptive-form-json-schema-form-model.html).

Depois de criar o Formulário adaptável com base no esquema JSON, a próxima etapa é armazenar os dados enviados no banco de dados. Para essa finalidade, usaremos o novo tipo de dados JSON introduzido por vários fornecedores de banco de dados. Para a finalidade deste artigo, usaremos o banco de dados MySql 8 para armazenar os dados enviados.

O banco de dados MySql 8 foi usado para este artigo. O MySQL introduziu um novo tipo de dados chamado [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). Isso facilita o armazenamento e a consulta de objetos JSON. Estamos armazenando os dados enviados em uma coluna do tipo JSON no nosso banco de dados.

A captura de tela a seguir mostra os dados de formulário enviados armazenados no tipo de dados JSON. A coluna &quot;formdata&quot; é do tipo JSON. Também armazenamos o nome do formulário associado aos dados no nome do formulário da coluna

>[!NOTE]
>
>Certifique-se de que o nome do arquivo de esquema json seja adequado. Por exemplo, ele precisa ser nomeado no seguinte formato &lt;name>schema.json. Portanto, seu arquivo de esquema pode ser hipoteca.schema.json ou credit.schema.json.


![armazenamento de dados](assets/datastored.gif)


[Exemplos de esquemas JSON que podem ser usados para criar o Adaptive Forms.](assets/samplejsonschemas.zip). Baixe e descompacte o arquivo zip para obter os esquemas JSON
