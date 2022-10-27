---
title: AEM Forms com Marketo (Parte 1)
description: Tutorial para integrar o AEM Forms com o Marketo usando o Modelo de dados de formulário AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# AEM Forms Com Marketo

A Marketo, parte do Adobe, fornece software de Automação de marketing voltado para o marketing baseado em conta, incluindo email, dispositivos móveis, anúncios sociais, digitais, gerenciamento da Web e análise.

Usando o Modelo de dados de formulário do AEM Forms, agora é possível integrar AEM formulário com o Marketo perfeitamente.

[Saiba mais sobre o Modelo de dados de formulário](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

O Marketo expõe uma REST API que permite a execução remota de muitos dos recursos do sistema. Da criação de programas à importação de lead em massa, há muitas opções que permitem o controle detalhado de uma instância do Marketo. Usando o Modelo de dados de formulário, é muito simples integrar o AEM Forms ao Marketo.

Este tutorial o guiará pelas etapas envolvidas na integração do AEM Forms com o Marketo usando o Modelo de dados de formulário. Ao concluir o tutorial, você terá um pacote OSGi que fará a autenticação personalizada com o Marketo. Você também terá configurado a fonte de dados usando o arquivo swagger fornecido.

Para começar, é altamente recomendável que você esteja familiarizado com os seguintes tópicos listados na seção Pré-requisito .

## Pré-requisitos

1. [AEM servidor com o pacote AEM Forms Add on instalado](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Ambiente de desenvolvimento de AEM local
1. Familiarizar-se com o Modelo de dados de formulário
1. Conhecimento básico dos arquivos Swagger
1. Criando Forms adaptável

**ID secreta do cliente e chave secreta do cliente**

A primeira etapa na integração do Marketo com o AEM Forms é obter as credenciais da API necessárias para fazer as chamadas REST usando a API. Você precisará do seguinte

1. client_id
1. client_secret
1. identity_endpoint
1. URL de autenticação

[Siga a documentação oficial do Marketo para obter as propriedades mencionadas acima.](https://developers.marketo.com/rest-api/) Como alternativa, você também pode entrar em contato com o administrador da instância do Marketo.

**Antes de começar**

[Baixe e descompacte os ativos relacionados a este artigo.](assets/aemformsandmarketo.zip) O arquivo zip contém o seguinte:

1. BlankTemplatePackage.zip - Este é o modelo de formulário adaptável. Importe isso usando o gerenciador de pacotes.
1. marketo.json - Este é o arquivo do swagger usado para configurar a fonte de dados.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar - Este é o pacote que faz a autenticação personalizada. Você pode usar isso se não conseguir concluir o tutorial ou se o pacote não estiver funcionando como esperado.
