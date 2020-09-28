---
title: AEM Forms com Marketing (Parte 1)
seo-title: AEM Forms com Marketing (Parte 1)
description: Tutorial para integrar o AEM Forms ao Marketing usando o Modelo de dados de formulário AEM Forms.
seo-description: Tutorial para integrar o AEM Forms ao Marketing usando o Modelo de dados de formulário AEM Forms.
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---


# AEM Forms com Marketing

Marketo, parte do Adobe oferece software Marketing Automation voltado para o marketing baseado em conta, incluindo email, dispositivos móveis, redes sociais, anúncios digitais, gerenciamento da Web e análises.

Usando o Modelo de dados de formulário da AEM Forms, agora é possível integrar AEM formulário com o Marketing de forma contínua.

[Saiba mais sobre o Modelo de dados de formulário](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

O Marketo expõe uma REST API que permite a execução remota de muitos dos recursos do sistema. Da criação de programas à importação de lead em massa, há muitas opções que permitem o controle detalhado de uma instância do Marketo. Usando o Modelo de dados de formulário, é muito simples integrar a AEM Forms ao Marketing.

Este tutorial o guiará pelas etapas envolvidas na integração do AEM Forms com o Marketing usando o Modelo de dados de formulário. Ao concluir o tutorial, você terá um pacote OSGi que fará a autenticação personalizada em relação ao Marketo. Você também terá configurado a fonte de dados usando o arquivo swagger fornecido.

Para começar, é altamente recomendável que você esteja familiarizado com os seguintes tópicos listados na seção Pré-requisito.

## Pré-requisitos

1. [Servidor AEM com o pacote AEM Forms Add on instalado](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Ambiente de desenvolvimento de AEM local
1. Familiarizar-se com o modelo de dados de formulário
1. Conhecimento básico dos arquivos Swagger
1. Criando Forms adaptável

**ID secreta do cliente e chave secreta do cliente**

A primeira etapa na integração do Marketo com a AEM Forms é obter as credenciais da API necessárias para fazer as chamadas REST usando a API. Você precisará do seguinte

1. client_id
1. client_secret
1. identity_endpoint
1. URL de autenticação.

[Siga a documentação oficial do Marketing para obter as propriedades acima mencionadas.](https://developers.marketo.com/rest-api/) Como alternativa, você também pode entrar em contato com o administrador da sua instância do Marketo.

**Antes de começar**

[Baixe e descompacte os ativos relacionados a este artigo.](assets/aemformsandmarketo.zip) O arquivo zip contém o seguinte:

1. BlankTemplatePackage.zip - Este é o modelo de formulário adaptável. Importe isso usando o gerenciador de pacote.
1. marketo.json - Este é o arquivo swagger que será usado para configurar a fonte de dados.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar - Este é o pacote que faz a autenticação personalizada. Sinta-se à vontade para usá-lo se não conseguir concluir o tutorial ou se o pacote não estiver funcionando como esperado.
