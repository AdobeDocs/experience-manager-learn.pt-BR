---
title: Criar esquema
description: Crie um esquema com base nos dados que precisam ser importados para o formulário adaptável
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: b286c3e9-70df-46e8-b0bc-21599ab1ec06
duration: 41
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 1%

---

# Introdução

A primeira etapa é criar um esquema com base nos dados que serão usados para preencher o formulário adaptável.

## O XFA é baseado em um esquema

Use o esquema para criar seu formulário adaptável

## O XFA não se baseia em um esquema

* Abra o XDP no AEM Forms Designer.
* Clique em Arquivo | Propriedades do formulário | Visualizar.
* Clique em Gerar dados de visualização.
* Clique em Gerar.
* Forneça um nome de arquivo significativo, como `form-data.xml`

Você pode usar qualquer uma das ferramentas online gratuitas para [gerar XSD](https://www.freeformatter.com/xsd-generator.html) a partir dos dados xml gerados na etapa anterior.

Crie um formulário adaptável com base no esquema da etapa anterior.

>[!NOTE]
>É sempre recomendável examinar os dados gerados no envio do Formulário adaptável. Isso dará uma boa ideia do formato XML dos dados que precisam ser mesclados com o formulário adaptável.

Dados enviados do formulário adaptável
![dados enviados](./assets/af-submitted-data.png)

Dados exportados do PDF
![dados-exportados](./assets/exported-data.png)

Nos dados exportados, será necessário extrair o nó **_topmostSubform_** com os namespaces apropriados preservados para mesclar dados com o formulário adaptável com êxito.

## Próximas etapas

[Criar serviço OSGi](./create-osgi-service.md)
