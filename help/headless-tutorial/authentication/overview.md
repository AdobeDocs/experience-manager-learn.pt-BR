---
title: Autenticar-se no AEM as a Cloud Service a partir de um aplicativo externo
description: Saiba como um aplicativo externo pode autenticar-se e interagir programaticamente com o AEM as a Cloud Service por HTTP, usando tokens de acesso de desenvolvimento local e credenciais de serviço.
version: Experience Manager as a Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
duration: 253
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: ht
source-wordcount: '621'
ht-degree: 100%

---

# Autenticação baseada em token para o AEM as a Cloud Service

O AEM expõe uma variedade de pontos de acesso HTTP que podem ser usados para interagir sem cabeçalho, incluindo o GraphQL, os Serviços de conteúdo do AEM e a API HTTP do Assets. Geralmente, esses consumidores sem cabeçalho podem precisar autenticar-se no AEM para acessar conteúdo ou ações protegidos. Para facilitar isso, o AEM permite a autenticação baseada em tokens de solicitações HTTP de aplicativos, serviços ou sistemas externos.

Neste tutorial, vamos ver como um aplicativo externo pode autenticar-se e interagir programaticamente com o AEM as a Cloud Service por HTTP, usando tokens de acesso.

>[!VIDEO](https://video.tv.adobe.com/v/3417766?quality=12&learn=on&captions=por_br)

## Pré-requisitos

Certifique-se de contar com os itens a seguir antes de continuar com este tutorial:

1. Acesso a um ambiente do AEM as a Cloud Service (preferencialmente, um ambiente de desenvolvimento ou um programa de sandbox)
1. Associação ao perfil de produto do administrador do AEM dos serviços do Author do ambiente do AEM as a Cloud Service
1. Associação ou acesso ao administrador da organização do Adobe IMS (será necessário executar uma inicialização das [Credenciais de serviço](./service-credentials.md) uma única vez)
1. O [site da WKND](https://github.com/adobe/aem-guides-wknd) mais recente implantado no seu ambiente do Cloud Service

## Visão geral do aplicativo externo

Este tutorial usa um [aplicativo Node.js simples](./assets/aem-guides_token-authentication-external-application.zip) executado da linha de comando para atualizar os metadados de ativos no AEM as a Cloud Service por meio da [API HTTP do Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=pt-BR).

O fluxo de execução do aplicativo Node.js é o seguinte:

![Aplicativo externo](./assets/overview/external-application.png)

1. O aplicativo Node.js é invocado pela linha de comando
1. Os parâmetros da linha de comando definem:
   + O host do serviço de Author do AEM as a Cloud Service ao qual se conectar (`aem`)
   + A pasta de ativos do AEM cujos ativos serão atualizados (`folder`)
   + A propriedade e o valor dos metadados a serem atualizados (`propertyName` e `propertyValue`)
   + O caminho local do arquivo que fornece as credenciais necessárias para acessar o AEM as a Cloud Service (`file`)
1. O token de acesso usado para autenticação no AEM é derivado do arquivo JSON fornecido pelo parâmetro da linha de comando `file`

   a. Se as credenciais de serviço usadas para desenvolvimento não local forem fornecidas no arquivo JSON (`file`), o token de acesso será recuperado das APIs do Adobe IMS
1. O aplicativo usa o token de acesso para acessar o AEM e listar todos os ativos na pasta especificada no parâmetro da linha de comando `folder`
1. Para cada ativo na pasta, o aplicativo atualiza seus metadados com base no nome e valor da propriedade especificados nos parâmetros da linha de comando `propertyName` e `propertyValue`

Embora o aplicativo de exemplo seja o Node.js, essas interações podem ser desenvolvidas com base em diferentes linguagens de programação e executadas a partir de outros sistemas externos.

## Token de acesso de desenvolvimento local

Os tokens de acesso de desenvolvimento local são gerados para um ambiente específico do AEM as a Cloud Service e fornecem acesso aos serviços do Author e do Publish.  Esses tokens de acesso são temporários e só devem ser usados durante o desenvolvimento de aplicativos externos ou sistemas que interagem com o AEM por HTTP. Em vez de um desenvolvedor ter que obter e gerenciar credenciais de serviço de boa-fé, ele pode gerar automaticamente, de maneira rápida e fácil, um token de acesso temporário, permitindo-lhe desenvolver a integração.

+ [Como usar o token de acesso de desenvolvimento local](./local-development-access-token.md)

## Credenciais de serviço

As credenciais de serviço são as credenciais de boa-fé usadas nos cenários que não são de desenvolvimento (normalmente, de produção) para facilitar a autenticação e a interação de um aplicativo externo ou sistema com o AEM as a Cloud Service via HTTP. As credenciais de serviço em si não são enviadas ao AEM para autenticação. O aplicativo externo usa-as para gerar um JWT, que é trocado com as APIs do Adobe IMS _por_ um token de acesso, que pode ser usado para autenticar solicitações HTTP no AEM as a Cloud Service.

+ [Como usar as credenciais de serviço](./service-credentials.md)

## Recursos adicionais

+ [Baixar o aplicativo de exemplo](./assets/aem-guides_token-authentication-external-application.zip)
+ Outras amostras de código de criação e troca de JWT
   + [Amostras de código em Node.js, Java, Python, C#.NET e PHP](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples)
   + [Amostra de código baseada em JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)
