---
title: Integrar o AEM Forms as a Cloud Service e o Marketo
description: Saiba como integrar o AEM Forms e o Marketo usando o Modelo de dados de formulário do AEM Forms.
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: c3145149-bfa4-4dcb-acde-c359e9348f99
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 1%

---

# Integrar o AEM Forms e o Marketo

A Marketo, parte da Adobe fornece software de Automação de marketing com foco no marketing baseado em conta, incluindo email, dispositivos móveis, anúncios sociais, digitais, gerenciamento da Web e análises.

Usando o modelo de dados de formulário do AEM Forms, agora podemos integrar o formulário AEM ao Marketo sem interrupções.

[Saiba mais sobre o Modelo de Dados de Formulário](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

O Marketo expõe uma API REST que permite a execução remota de muitos dos recursos do sistema. Desde a criação de programas até a importação de leads em massa, há muitas opções que permitem o controle refinado de uma instância do Marketo. Usar o modelo de dados de formulário é muito simples integrar o AEM Forms com o Marketo.

Este tutorial guiará você pelas etapas relativas à integração do AEM Forms com o Marketo usando o modelo de dados de formulário. Ao concluir o tutorial, você terá um pacote OSGi que fará a autenticação personalizada no Marketo. Você também terá configurado a fonte de dados usando o arquivo swagger fornecido.

Para começar, é altamente recomendável que você esteja familiarizado com os seguintes tópicos listados na seção Pré-requisitos.

## Pré-requisitos

1. Acesso à instância as a Cloud Service do AEM Forms
1. Familiarizar-se com o modelo de dados de formulário
1. Conhecimento básico dos arquivos do Swagger
1. Criação de Forms adaptável

**ID e Chave Secreta do Cliente**

A primeira etapa na integração do Marketo com o AEM Forms é obter as credenciais de API necessárias para fazer as chamadas REST usando a API. Você precisará do seguinte

1. client_id
1. client_secret
1. identity_endpoint

[Siga a documentação oficial do Marketo para obter as propriedades mencionadas acima.](https://developers.marketo.com/rest-api/) Como alternativa, você também pode contatar o administrador da sua instância do Marketo.

**Antes de começar**

* [Baixe e descompacte os ativos relacionados a este tutorial](assets/marketo.zip)

O arquivo zip contém o seguinte:

1. marketo.json - Esse é o arquivo swagger usado para configurar a fonte de dados.
1. Altere a propriedade do host no marketo.json para apontar para a instância do marketo

## Próximas etapas

[Criar Source de dados](./part2.md)
