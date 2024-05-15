---
title: Autenticação para AEM as a Cloud Service de um aplicativo externo
description: Saiba como um aplicativo externo pode autenticar e interagir programaticamente com o AEM as a Cloud Service por HTTP usando Tokens de acesso de desenvolvimento local e Credenciais de serviço.
version: Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
duration: 253
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 0%

---

# Autenticação baseada em token para AEM as a Cloud Service

O AEM expõe uma variedade de endpoints HTTP que podem ser usados para interagir com headless, desde o GraphQL, o AEM Content Services até a API HTTP de ativos. Geralmente, esses consumidores headless podem precisar se autenticar no AEM para acessar conteúdo ou ações protegidos. Para facilitar isso, o AEM oferece suporte à autenticação baseada em token de solicitações HTTP de aplicativos, serviços ou sistemas externos.

Neste tutorial, explore como um aplicativo externo pode autenticar e interagir programaticamente com o AEM as a Cloud Service por HTTP usando tokens de acesso.

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## Pré-requisitos

Certifique-se de que os itens a seguir estão em vigor antes de seguir este tutorial:

1. Acesso a um ambiente do AEM as a Cloud Service (de preferência um ambiente de desenvolvimento ou um programa de sandbox)
1. Associação ao perfil de produto Administrador dos serviços do autor do ambiente as a Cloud Service AEM AEM
1. Associação ou acesso ao Administrador da organização IMS da Adobe (será necessário executar uma inicialização única do [Credenciais de serviço](./service-credentials.md))
1. O mais recente [Site da WKND](https://github.com/adobe/aem-guides-wknd) implantado em seu ambiente Cloud Service

## Visão geral do aplicativo externo

Este tutorial usa um [aplicativo Node.js simples](./assets/aem-guides_token-authentication-external-application.zip) execute a partir da linha de comando para atualizar metadados de ativos no AEM as a Cloud Service usando o [API HTTP de ativos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

O fluxo de execução do aplicativo Node.js é o seguinte:

![Aplicativo externo](./assets/overview/external-application.png)

1. O aplicativo Node.js é chamado da linha de comando
1. Os parâmetros da linha de comando definem:
   + O host do serviço de Autor as a Cloud Service do AEM ao qual se conectar (`aem`)
   + A pasta de ativos do AEM cujos ativos são atualizados (`folder`)
   + A propriedade e o valor dos metadados que serão atualizados (`propertyName` e `propertyValue`)
   + O caminho local para o arquivo que fornece as credenciais necessárias para acessar o AEM as a Cloud Service (`file`)
1. O token de acesso usado para autenticar no AEM é derivado do arquivo JSON fornecido por meio do parâmetro de linha de comando `file`

   a. Se as Credenciais de serviço usadas para desenvolvimento não local forem fornecidas no arquivo JSON (`file`), o token de acesso é recuperado das APIs do Adobe IMS
1. O aplicativo usa o token de acesso para acessar o AEM e listar todos os ativos na pasta especificada no parâmetro de linha de comando `folder`
1. Para cada ativo na pasta, o aplicativo atualiza seus metadados com base no nome e no valor da propriedade especificados nos parâmetros de linha de comando `propertyName` e `propertyValue`

Embora esse aplicativo de exemplo seja o Node.js, essas interações podem ser desenvolvidas usando diferentes linguagens de programação e executadas de outros sistemas externos.

## Token de acesso de desenvolvimento local

Os tokens de acesso de desenvolvimento local são gerados para um ambiente específico do AEM as a Cloud Service e fornecem acesso aos serviços do Author e Publish.  Esses tokens de acesso são temporários e só devem ser usados durante o desenvolvimento de aplicativos externos ou sistemas que interagem com AEM por HTTP. Em vez de um desenvolvedor ter que obter e gerenciar credenciais de serviço do bonafide, ele pode gerar automaticamente, de maneira rápida e fácil, um token de acesso temporário, permitindo que desenvolva sua integração.

+ [Como usar o token de acesso de desenvolvimento local](./local-development-access-token.md)

## Credenciais de serviço

As Credenciais de serviço são as credenciais bonafide usadas em qualquer cenário que não seja de desenvolvimento, obviamente de produção, que facilitam um aplicativo externo ou a capacidade do sistema de autenticar e interagir com o AEM as a Cloud Service via HTTP. As próprias Credenciais de serviço não são enviadas ao AEM para autenticação. Em vez disso, o aplicativo externo as usa para gerar um JWT, que é substituído pelas APIs do Adobe IMS _para_ um token de acesso, que pode ser usado para autenticar solicitações HTTP no AEM as a Cloud Service.

+ [Como usar as Credenciais de serviço](./service-credentials.md)

## Recursos adicionais

+ [Baixar o aplicativo de exemplo](./assets/aem-guides_token-authentication-external-application.zip)
+ Outras amostras de código de criação e troca de JWT
   + [Amostras de código Node.js, Java, Python, C#.NET e PHP](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)
   + [Amostra de código baseada em JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)
