---
title: Integrar o AEM Forms e o Marketo
description: Saiba como integrar o AEM Forms e o Marketo usando o Modelo de dados de formulário do AEM Forms.
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 77
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 1%

---

# Integrar o AEM Forms e o Marketo


A Marketo, parte da Adobe, fornece software de Automação de marketing com foco no marketing baseado em conta, incluindo email, dispositivos móveis, anúncios sociais, digitais, gerenciamento da Web e análises.

Usando o modelo de dados de formulário do AEM Forms, agora podemos integrar o AEM Form com o Marketo sem interrupções.

[Saiba mais sobre o Modelo de Dados de Formulário](https://helpx.adobe.com/br/experience-manager/6-5/forms/using/data-integration.html)

O Marketo expõe uma API REST que permite a execução remota de muitos dos recursos do sistema. Desde a criação de programas até a importação de leads em massa, há muitas opções que permitem o controle refinado de uma instância do Marketo. Usar o modelo de dados de formulário é muito simples integrar o AEM Forms com o Marketo.

>[!NOTE]
>
>Este tutorial é personalizado especificamente para o AEM Forms 6.5. Se você deseja integrar o AEM Forms as a Cloud Service com o Adobe Marketo Engage, consulte a [documentação dedicada para essa integração](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/forms/integrate/services/integrate-adaptive-form-with-market-engage/integrate-form-to-marketo-engage).

Este tutorial guiará você pelas etapas relativas à integração do AEM Forms com o Marketo usando o modelo de dados de formulário. Ao concluir o tutorial, você terá um pacote OSGi que fará a autenticação personalizada no Marketo. Você também terá configurado a fonte de dados usando o arquivo swagger fornecido.

Para começar, é altamente recomendável que você esteja familiarizado com os seguintes tópicos listados na seção Pré-requisitos.

## Pré-requisitos

1. [Servidor do AEM com o pacote complementar do AEM Forms instalado](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Ambiente de desenvolvimento local do AEM
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

* [Baixe e descompacte os ativos relacionados a este tutorial](assets/marketo-integration-assets.zip)

O arquivo zip contém o seguinte:

1. BlankTemplatePackage.zip - este é o modelo de formulário adaptável. Importe isso usando o gerenciador de pacotes.
1. marketo.json - Esse é o arquivo swagger usado para configurar a fonte de dados.
1. Altere a propriedade do host no marketo.json para apontar para a instância do marketo

## Próximas etapas

[Criar Source de dados](./part2.md)
