---
title: Autenticação para AEM como Cloud Service de um aplicativo externo
description: Explore como um aplicativo externo pode autenticar e interagir programaticamente com o AEM as a Cloud Service sobre HTTP usando tokens de acesso de desenvolvimento local e credenciais de serviço.
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '644'
ht-degree: 0%

---

# Autenticação por token para AEM como Cloud Service

O AEM expõe uma variedade de pontos de extremidade HTTP que podem ter interação headless, de GraphQL AEM Content Services para API HTTP do Assets. Geralmente, esses consumidores sem interface podem precisar se autenticar para AEM a fim de acessar o conteúdo ou as ações protegidas. Para facilitar isso, o AEM oferece suporte à autenticação por token de solicitações HTTP de aplicativos, serviços ou sistemas externos.

Neste tutorial, explore bem como um aplicativo externo pode autenticar e interagir programaticamente com o para AEM como um Cloud Service sobre HTTP usando tokens de acesso.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## Pré-requisitos

Certifique-se de que o seguinte esteja em vigor antes de seguir junto com este tutorial:

1. Acesso ao am AEM como um ambiente Cloud Service (preferencialmente um ambiente de desenvolvimento ou um programa de sandbox)
1. Associação no AEM as a Cloud Service environment&#39;s Author services AEM Administrator Product Profile
1. Associação ou acesso ao Administrador da Org do Adobe IMS (será necessário executar uma inicialização única das [Credenciais de serviço](./service-credentials.md))
1. O [Site WKND](https://github.com/adobe/aem-guides-wknd) mais recente implantado no seu ambiente Cloud Service

## Visão geral do aplicativo externo

Este tutorial usa um aplicativo simples do Node.js](./assets/aem-guides_token-authentication-external-application.zip) executado a partir da linha de comando para atualizar metadados de ativos no AEM como um Cloud Service usando [API HTTP de ativos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).[

O fluxo de execução do aplicativo Node.js é o seguinte:

![Aplicativo externo](./assets/overview/external-application.png)

1. O aplicativo Node.js é chamado a partir da linha de comando
1. Parâmetros da linha de comando definem:
   + O AEM como um host de serviço de Autor do Cloud Service ao qual se conectar (`aem`)
   + A pasta de ativos AEM cujos ativos serão atualizados (`folder`)
   + A propriedade de metadados e o valor a ser atualizado (`propertyName` e `propertyValue`)
   + O caminho local para o arquivo que fornece as credenciais necessárias para acessar o AEM como um Cloud Service (`file`)
1. O token de acesso usado para autenticação no AEM é derivado do arquivo JSON fornecido por meio do parâmetro de linha de comando `file`

   a. Se as Credenciais de Serviço usadas para desenvolvimento não local forem fornecidas no arquivo JSON (`file`), o token de acesso será recuperado das APIs do Adobe IMS
1. O aplicativo usa o token de acesso para acessar AEM e listar todos os ativos na pasta especificada no parâmetro da linha de comando `folder`
1. Para cada ativo na pasta, o aplicativo atualiza seus metadados com base no nome e no valor da propriedade especificados nos parâmetros de linha de comando `propertyName` e `propertyValue`

Embora este aplicativo de exemplo seja Node.js, essas interações podem ser desenvolvidas usando diferentes linguagens de programação e executadas de outros sistemas externos.

## Token de acesso de desenvolvimento local

Tokens de acesso ao desenvolvimento local são gerados para um AEM específico como um ambiente de Cloud Service e que fornece acesso aos serviços de Autor e Publicação.  Esses tokens de acesso são temporários e só devem ser usados durante o desenvolvimento de aplicativos ou sistemas externos que interagem com AEM por HTTP. Em vez de um desenvolvedor ter que obter e gerenciar credenciais de serviço vinculadas, ele pode gerar de forma rápida e fácil um token de acesso temporário que permite desenvolver sua integração.

+ [Como usar o token de acesso de desenvolvimento local](./local-development-access-token.md)

## Credenciais de Serviço

As Credenciais de Serviço são as credenciais vinculadas usadas em qualquer cenário que não seja de desenvolvimento - mais obviamente de produção - que facilitam a capacidade de um aplicativo ou sistema externo de se autenticar e interagir com o AEM como um Cloud Service sobre HTTP. As próprias Credenciais de serviço não são enviadas ao AEM para autenticação, em vez disso, o aplicativo externo as usa para gerar um JWT, que é trocado por APIs do Adobe IMS _for_ um token de acesso, que pode ser usado para autenticar solicitações HTTP para AEM como Cloud Service.

+ [Como usar credenciais de serviço](./service-credentials.md)

## Recursos adicionais

+ [Baixe o aplicativo de exemplo](./assets/aem-guides_token-authentication-external-application.zip)
+ Outras amostras de código da criação e troca de JWT
   + [Node.js, Java, Python, C#.NET e amostras de código PHP](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [Amostra de código baseada em JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)
