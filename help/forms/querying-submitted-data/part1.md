---
title: AEM Forms com esquema JSON e dados[Parte 1]
seo-title: AEM Forms com esquema JSON e dados[Parte1]
description: Tutorial de várias partes para orientá-lo pelas etapas envolvidas na criação do formulário adaptável com esquema JSON e consulta dos dados enviados.
seo-description: Tutorial de várias partes para orientá-lo pelas etapas envolvidas na criação do formulário adaptável com esquema JSON e consulta dos dados enviados.
feature: formulários adaptáveis
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 0%

---


# Criar formulário adaptável com base no esquema JSON


A capacidade de criar Formulários adaptáveis com base no esquema JSON foi introduzida com o AEM Forms 6.3. Os detalhes sobre a criação de formulários adaptáveis com esquema JSON são explicados detalhadamente neste [artigo](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html).

Depois de criar o Formulário adaptável com base no esquema JSON, a próxima etapa é armazenar os dados enviados no banco de dados. Para essa finalidade, usaremos o novo tipo de dados JSON introduzido por vários fornecedores de banco de dados. Para a finalidade deste artigo, usaremos o banco de dados MySql 8 para armazenar os dados enviados.

O banco de dados MySql 8 foi usado para este artigo. O MySQL introduziu um novo tipo de dados chamado [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). Isso facilita o armazenamento e a consulta de objetos JSON. Armazenaremos os dados enviados em uma coluna do tipo JSON no nosso banco de dados.

A captura de tela a seguir mostra os dados de formulário enviados armazenados no tipo de dados JSON. A coluna &quot;formdata&quot; é do tipo JSON. Também armazenamos o nome do formulário associado aos dados no nome do formulário da coluna

>[!NOTE]
>
>Certifique-se de que o nome do arquivo de esquema json seja adequado. Por exemplo, ele precisa ser nomeado no seguinte formato &lt;name>schema.json. Assim, seu arquivo de esquema pode ser hipoteca.schema.json ou credit.schema.json.


![armazenamento de dados](assets/datastored.gif)


[Exemplos de esquemas JSON que podem ser usados para criar formulários adaptáveis.](assets/samplejsonschemas.zip). Baixe e descompacte o arquivo zip para obter os esquemas JSON

