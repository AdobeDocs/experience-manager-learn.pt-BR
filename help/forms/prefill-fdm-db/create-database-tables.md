---
title: Criar tabelas de banco de dados
description: Criar banco de dados a ser usado pelo modelo de dados de formulário
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
translation-type: tm+mt
source-git-commit: b085a2c75f8e0b4860d503774ea01a108773ad09
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 0%

---


# Criar tabelas de banco de dados

O modelo de dados de formulário pode ser baseado em fontes RDBMS, RESTfull, SOAP ou OData. O foco deste curso é o preenchimento prévio do formulário adaptável usando o modelo de dados de formulário respaldado pela fonte de dados RDBMS. Para a finalidade deste tutorial, foi usado o banco de dados MYSQL. Criamos as duas tabelas a seguir para demonstrar o caso de uso

* **nova** tabela - Esta tabela armazena as informações novas

   ![newhire](assets/newhire-table.png)


* **quadro de beneficiários** - Esta reserva novos beneficiários

   ![beneficiários](assets/beneficiaries-table.png)

Você pode importar o arquivo [](assets/db-schema.sql) sql usando a bancada MySQL para criar tabelas com alguns dados de amostra.