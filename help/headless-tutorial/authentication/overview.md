---
title: Autenticação para AEM as a Cloud Service de um aplicativo externo
description: Explore como um aplicativo externo pode autenticar e interagir programaticamente com AEM as a Cloud Service por HTTP usando tokens de acesso de desenvolvimento local e credenciais de serviço.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '643'
ht-degree: 1%

---

# Autenticação por token para AEM as a Cloud Service

O AEM expõe uma variedade de pontos de extremidade HTTP que podem ter interação headless, de GraphQL AEM Content Services para API HTTP do Assets. Geralmente, esses consumidores sem interface podem precisar se autenticar para AEM a fim de acessar o conteúdo ou as ações protegidas. Para facilitar isso, o AEM oferece suporte à autenticação por token de solicitações HTTP de aplicativos, serviços ou sistemas externos.

Neste tutorial, explore bem como um aplicativo externo pode autenticar e interagir programaticamente com o para AEM as a Cloud Service por HTTP usando tokens de acesso.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## Pré-requisitos

Certifique-se de que o seguinte esteja em vigor antes de seguir junto com este tutorial:

1. Acesso a um ambiente as a Cloud Service AEM (preferencialmente um ambiente de desenvolvimento ou um programa de sandbox)
1. Associação nos serviços de autor do ambiente AEM as a Cloud Service AEM Perfil de produto do administrador
1. Associação ou acesso ao Administrador da Org do Adobe IMS (será necessário executar uma inicialização única do [Credenciais de Serviço](./service-credentials.md))
1. O mais recente [Site WKND](https://github.com/adobe/aem-guides-wknd) implantado em seu ambiente Cloud Service

## Visão geral do aplicativo externo

Este tutorial usa um [aplicativo Node.js simples](./assets/aem-guides_token-authentication-external-application.zip) execute a partir da linha de comando para atualizar os metadados do ativo em AEM as a Cloud Service usando [API HTTP de ativos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

O fluxo de execução do aplicativo Node.js é o seguinte:

![Aplicativo externo](./assets/overview/external-application.png)

1. O aplicativo Node.js é chamado a partir da linha de comando
1. Parâmetros da linha de comando definem:
   + O host do serviço de Autor as a Cloud Service AEM ao qual se conectar (`aem`)
   + A pasta de ativos AEM cujos ativos são atualizados (`folder`)
   + A propriedade de metadados e o valor a ser atualizado (`propertyName` e `propertyValue`)
   + O caminho local para o arquivo que fornece as credenciais necessárias para acessar AEM as a Cloud Service (`file`)
1. O token de acesso usado para autenticação no AEM é derivado do arquivo JSON fornecido por meio do parâmetro da linha de comando `file`

   a. Se as Credenciais de Serviço usadas para desenvolvimento não local forem fornecidas no arquivo JSON (`file`), o token de acesso é recuperado das APIs do Adobe IMS
1. O aplicativo usa o token de acesso para acessar AEM e listar todos os ativos na pasta especificada no parâmetro da linha de comando `folder`
1. Para cada ativo na pasta, o aplicativo atualiza seus metadados com base no nome da propriedade e no valor especificados nos parâmetros da linha de comando `propertyName` e `propertyValue`

Embora este aplicativo de exemplo seja Node.js, essas interações podem ser desenvolvidas usando diferentes linguagens de programação e executadas de outros sistemas externos.

## Token de acesso de desenvolvimento local

Tokens de acesso ao desenvolvimento local são gerados para um ambiente específico AEM as a Cloud Service e fornecem acesso aos serviços de Autor e Publicação.  Esses tokens de acesso são temporários e só devem ser usados durante o desenvolvimento de aplicativos ou sistemas externos que interagem com AEM por HTTP. Em vez de um desenvolvedor ter que obter e gerenciar credenciais de serviço vinculadas, ele pode gerar de forma rápida e fácil um token de acesso temporário que permite desenvolver sua integração.

+ [Como usar o token de acesso de desenvolvimento local](./local-development-access-token.md)

## Credenciais de Serviço

As Credenciais de Serviço são as credenciais vinculadas usadas em qualquer cenário que não seja de desenvolvimento - mais obviamente de produção - que facilitam a capacidade de um aplicativo externo ou do sistema de se autenticar e interagir com AEM as a Cloud Service por HTTP. As próprias Credenciais de serviço não são enviadas ao AEM para autenticação, em vez disso, o aplicativo externo as usa para gerar um JWT, que é trocado com as APIs do Adobe IMS _para_ um token de acesso, que pode ser usado para autenticar solicitações HTTP para AEM as a Cloud Service.

+ [Como usar credenciais de serviço](./service-credentials.md)

## Recursos adicionais

+ [Baixe o aplicativo de exemplo](./assets/aem-guides_token-authentication-external-application.zip)
+ Outras amostras de código da criação e troca de JWT
   + [Node.js, Java, Python, C#.NET e amostras de código PHP](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [Amostra de código baseada em JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)
