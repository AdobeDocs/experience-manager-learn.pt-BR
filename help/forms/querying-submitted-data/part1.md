---
title: AEM Forms com Schema JSON e dados[parte 1]
seo-title: AEM Forms com Schema JSON e dados[Parte1]
description: Tutorial de várias peças para orientá-lo pelas etapas envolvidas na criação do Formulário adaptável com o schema JSON e consulta dos dados enviados.
seo-description: Tutorial de várias peças para orientá-lo pelas etapas envolvidas na criação do Formulário adaptável com o schema JSON e consulta dos dados enviados.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---


# Criar formulário adaptável com base no Schema JSON


A capacidade de criar um Forms adaptável baseado no schema JSON foi introduzida com a versão AEM Forms 6.3. Os detalhes sobre a criação do Adaptive Forms com o schema JSON são explicados em detalhes neste [artigo](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html).

Depois de criar o Formulário adaptável com base no schema JSON, a próxima etapa é armazenar os dados enviados no banco de dados. Para essa finalidade, usaremos o novo tipo de dados JSON introduzido por vários fornecedores de banco de dados. Para a finalidade deste artigo, usaremos o banco de dados MySql 8 para armazenar os dados enviados.

O banco de dados MySql 8 foi usado para este artigo. O MySQL introduziu um novo tipo de dados chamado [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). Isso facilita o armazenamento e o query de objetos JSON. Estocaremos os dados enviados em uma coluna do tipo JSON em nosso banco de dados.

A seguinte captura de tela mostra os dados de formulário enviados armazenados no tipo de dados JSON. A coluna &quot;formdata&quot; é do tipo JSON. Também armazenamos o nome do formulário associado aos dados no nome do formulário da coluna

>[!NOTE]
>
>Certifique-se de que seu arquivo de schema json tenha o nome correto. Por exemplo, ele precisa ser nomeado no seguinte formato &lt;name>schema.json. Portanto, seu arquivo de schema pode ser hipoteca.schema.json ou credit.schema.json.


![dados](assets/datastored.gif)


[Amostra de Schemas JSON que podem ser usados para criar o Forms adaptável.](assets/samplejsonschemas.zip). Baixe e descompacte o arquivo zip para obter os schemas JSON

