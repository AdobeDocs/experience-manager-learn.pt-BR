---
title: Autenticação para AEM como Cloud Service de um aplicativo externo
description: Explore como um aplicativo externo pode autenticar e interagir de forma programática com AEM como um Cloud Service por HTTP usando Tokens de acesso de desenvolvimento local e credenciais de serviço.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
translation-type: tm+mt
source-git-commit: 0b1150cd7ca32382cfaa880f9f956b55bfb65a33
workflow-type: tm+mt
source-wordcount: '585'
ht-degree: 0%

---


# Autenticação baseada em token para AEM como Cloud Service

Neste tutorial, explore como um aplicativo externo pode autenticar e interagir de forma programática para AEM como um Cloud Service sobre HTTP usando tokens de acesso.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## Pré-requisitos

Verifique se os seguintes itens estão no lugar antes de seguir, juntamente com este tutorial:

1. Acesso a um ambiente am AEM (de preferência, um ambiente de desenvolvimento ou um programa Sandbox)
1. Associação ao AEM como um autor de ambiente AEM um Perfil de produto administrador
1. Associar-se ou acessar o Administrador de Organizações do Adobe IMS (eles precisarão executar uma inicialização única das [Credenciais de Serviço](./service-credentials.md))
1. O mais recente [Site WKND](https://github.com/adobe/aem-guides-wknd) implantado no seu ambiente de Cloud Service

## Visão geral do aplicativo externo

Este tutorial usa um aplicativo [simple Node.js](./assets/aem-guides_token-authentication-external-application.zip) executado da linha de comando para atualizar os metadados do ativo em AEM como um Cloud Service usando [API HTTP do Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

O fluxo de execução do aplicativo Node.js é o seguinte:

![Aplicativo externo](./assets/overview/external-application.png)

1. O aplicativo Node.js é chamado da linha de comando
1. Os parâmetros da linha de comando definem:
   + O AEM como um host de serviço de autor de Cloud Service para conexão (`aem`)
   + A pasta de ativos AEM cujos ativos serão atualizados (`folder`)
   + A propriedade e o valor de metadados a serem atualizados (`propertyName` e `propertyValue`)
   + O caminho local para o arquivo que fornece as credenciais necessárias para acessar AEM como Cloud Service (`file`)
1. O token de acesso usado para autenticação em AEM é derivado do arquivo JSON fornecido pelo parâmetro de linha de comando `file`

   a. Se as credenciais de serviço usadas para desenvolvimento não local forem fornecidas no arquivo JSON (`file`), o token de acesso será recuperado das APIs Adobe IMS
1. O aplicativo usa o token de acesso para acessar AEM e lista todos os ativos na pasta especificada no parâmetro de linha de comando `folder`
1. Para cada ativo da pasta, o aplicativo atualiza seus metadados com base no nome da propriedade e no valor especificados nos parâmetros de linha de comando `propertyName` e `propertyValue`

Embora este aplicativo de exemplo seja Node.js, essas interações podem ser desenvolvidas usando diferentes linguagens de programação e executadas de outros sistemas externos.

## Token de acesso de desenvolvimento local

Tokens de acesso de desenvolvimento local são gerados para um AEM específico como um ambiente Cloud Service e fornecem acesso aos serviços de autor e publicação.  Esses tokens de acesso são temporários e devem ser usados somente durante o desenvolvimento de aplicativos ou sistemas externos que interagem com AEM via HTTP. Em vez de um desenvolvedor ter que obter e gerenciar credenciais de serviço fictícias, ele pode gerar de forma rápida e fácil um token de acesso temporário que lhes permite desenvolver sua integração.

+ [Como usar o Token de acesso de desenvolvimento local](./local-development-access-token.md)

## Credenciais de serviço

Credenciais de serviço são credenciais subordinadas usadas em qualquer cenário de não desenvolvimento - mais obviamente produção - que facilitam a capacidade de um aplicativo externo ou sistema de autenticar e interagir com AEM como Cloud Service sobre HTTP. As próprias credenciais de serviço não são enviadas para AEM para autenticação, em vez disso, o aplicativo externo as usa para gerar um JWT, que é trocado com as APIs _for_ de um token de acesso do Adobe IMS, que podem ser usadas para autenticar solicitações HTTP para AEM como Cloud Service.

+ [Como usar as credenciais de serviço](./service-credentials.md)

## Recursos adicionais

+ [Baixe o aplicativo de exemplo](./assets/aem-guides_token-authentication-external-application.zip)
+ Outras amostras de código da criação e troca de JWT
   + [Amostras de código do Node.js, Java, Python, C#.NET e PHP](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [Amostra de código baseada em JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)
