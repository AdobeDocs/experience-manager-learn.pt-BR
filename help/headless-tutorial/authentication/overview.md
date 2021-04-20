---
title: Autenticação para o AEM as a Cloud Service a partir de um aplicativo externo
description: Explore como um aplicativo externo pode autenticar e interagir programaticamente com o AEM as a Cloud Service por HTTP usando tokens de acesso de desenvolvimento local e credenciais de serviço.
version: cloud-service
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
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '591'
ht-degree: 0%

---


# Autenticação por token para o AEM as a Cloud Service

Neste tutorial, explore bem como um aplicativo externo pode autenticar e interagir programaticamente com o AEM as a Cloud Service por HTTP usando tokens de acesso.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## Pré-requisitos

Certifique-se de que o seguinte esteja em vigor antes de seguir junto com este tutorial:

1. Acesso ao ambiente do AEM as a Cloud Service (preferencialmente um ambiente de desenvolvimento ou um programa de sandbox)
1. Associação no Perfil de produto do Administrador do AEM as a Cloud Service do ambiente do Author Services do AEM
1. Associação ou acesso ao Administrador da Org do Adobe IMS (será necessário executar uma inicialização única das [Credenciais de serviço](./service-credentials.md))
1. O [Site WKND](https://github.com/adobe/aem-guides-wknd) mais recente implantado em seu ambiente do Cloud Service

## Visão geral do aplicativo externo

Este tutorial usa um [aplicativo Node.js simples](./assets/aem-guides_token-authentication-external-application.zip) executado a partir da linha de comando para atualizar metadados de ativos no AEM as a Cloud Service usando [API HTTP de ativos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

O fluxo de execução do aplicativo Node.js é o seguinte:

![Aplicativo externo](./assets/overview/external-application.png)

1. O aplicativo Node.js é chamado a partir da linha de comando
1. Parâmetros da linha de comando definem:
   + O host do serviço Autor do AEM as a Cloud Service ao qual se conectar (`aem`)
   + A pasta de ativos do AEM cujos ativos serão atualizados (`folder`)
   + A propriedade de metadados e o valor a ser atualizado (`propertyName` e `propertyValue`)
   + O caminho local para o arquivo que fornece as credenciais necessárias para acessar o AEM as a Cloud Service (`file`)
1. O token de acesso usado para autenticação no AEM é derivado do arquivo JSON fornecido por meio do parâmetro de linha de comando `file`

   a. Se as Credenciais de Serviço usadas para desenvolvimento não local forem fornecidas no arquivo JSON (`file`), o token de acesso será recuperado das APIs do Adobe IMS
1. O aplicativo usa o token de acesso para acessar o AEM e listar todos os ativos na pasta especificada no parâmetro da linha de comando `folder`
1. Para cada ativo na pasta, o aplicativo atualiza seus metadados com base no nome e no valor da propriedade especificados nos parâmetros de linha de comando `propertyName` e `propertyValue`

Embora este aplicativo de exemplo seja Node.js, essas interações podem ser desenvolvidas usando diferentes linguagens de programação e executadas de outros sistemas externos.

## Token de acesso de desenvolvimento local

Tokens de acesso ao desenvolvimento local são gerados para um ambiente específico do AEM as a Cloud Service e fornecem acesso aos serviços de Autor e Publicação.  Esses tokens de acesso são temporários e só devem ser usados durante o desenvolvimento de aplicativos ou sistemas externos que interagem com o AEM por HTTP. Em vez de um desenvolvedor ter que obter e gerenciar credenciais de serviço vinculadas, ele pode gerar de forma rápida e fácil um token de acesso temporário que permite desenvolver sua integração.

+ [Como usar o token de acesso de desenvolvimento local](./local-development-access-token.md)

## Credenciais de Serviço

As credenciais de serviço são as credenciais vinculadas usadas em qualquer cenário de não desenvolvimento - mais obviamente produção - que facilitam a capacidade de um aplicativo externo ou do sistema de se autenticar e interagir com o AEM as a Cloud Service por HTTP. As próprias Credenciais de serviço não são enviadas ao AEM para autenticação, em vez disso, o aplicativo externo as usa para gerar um JWT, que é trocado com as APIs do Adobe IMS _for_ um token de acesso, que pode ser usado para autenticar solicitações HTTP para o AEM as a Cloud Service.

+ [Como usar credenciais de serviço](./service-credentials.md)

## Recursos adicionais

+ [Baixe o aplicativo de exemplo](./assets/aem-guides_token-authentication-external-application.zip)
+ Outras amostras de código da criação e troca de JWT
   + [Node.js, Java, Python, C#.NET e amostras de código PHP](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [Amostra de código baseada em JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)
