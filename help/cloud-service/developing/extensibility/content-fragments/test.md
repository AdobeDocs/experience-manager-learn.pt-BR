---
title: Teste de uma extensão do console de Fragmentos de conteúdo do AEM
description: Saiba como testar uma extensão do console de Fragmento de conteúdo do AEM antes de implantar na produção.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# Testar uma extensão

As extensões do Console de fragmentos de conteúdo do AEM podem ser testadas em relação a qualquer ambiente as a Cloud Service do AEM na organização Adobe à qual a extensão pertence.

O teste de uma extensão é feito por meio de um URL especialmente criado que instrui o console de Fragmento de conteúdo do AEM a carregar a extensão.

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

## URL do console de fragmentos de conteúdo do AEM

![URL do console de fragmentos de conteúdo do AEM](./assets/test/content-fragment-console-url.png){align="center"}

Para criar um URL que monte a extensão de não produção em um Console de fragmentos de conteúdo do AEM, o URL do Console de fragmentos de conteúdo do AEM desejado deve ser coletado. Navegue até o ambiente as a Cloud Service do AEM para testar a extensão e copie o URL do respectivo Console do Fragmento de conteúdo do AEM.

1. Faça login no ambiente as a Cloud Service AEM desejado.

   + Usar um ambiente de desenvolvimento de AEM para [teste de builds de desenvolvimento](#testing-development-builds)
   + Usar o ambiente de preparo do AEM ou desenvolvimento de controle de qualidade para [teste de builds de estágio](#testing-stage-builds)

1. Selecione o __Fragmentos de conteúdo__ ícone.
1. Aguarde o Console do Fragmento de conteúdo do AEM ser carregado no navegador.
1. Copie o URL do Console do Fragmento de conteúdo do AEM da barra de endereços do navegador. Ele deve se parecer com:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

Esse URL é usado abaixo ao criar os URLs para desenvolvimento e teste de preparo.

## Testar builds de desenvolvimento local

1. Abra uma linha de comando na raiz do projeto de extensão.
1. Execute a extensão Console do fragmento de conteúdo do AEM como um aplicativo local do App Builder

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

Anote o URL do aplicativo local, mostrado acima como `-> https://localhost:9080`

1. Adicione os dois parâmetros de consulta a seguir à [URL do console de fragmento de conteúdo do AEM](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, normalmente `&ext=https://localhost:9080`.

   Adicione os dois parâmetros de consulta acima (`devMode` e `ext`) como a __primeiro__ parâmetros de consulta no URL, pois o Console do fragmento de conteúdo usa uma rota de hash (`#/@wknd/aem/...`), então a pós-fixação incorreta dos parâmetros após a variável `#` não funcionará.

   O URL de teste deve ser semelhante a:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copie e cole o URL de teste no navegador.

   + Você pode ter que inicialmente, e depois periodicamente, você deve [aceitar o certificado HTTPS](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) para o host do aplicativo local (`https://localhost:9080`).

1. O Console do Fragmento de conteúdo do AEM é carregado com a versão local da extensão inserida nele para teste e alterações de recarregamento dinâmico enquanto o aplicativo local do App Builder está em execução.

>[!IMPORTANT]
>
>Lembre-se de que, ao usar essa abordagem, a extensão em desenvolvimento afeta apenas sua experiência, e todos os outros usuários do console de Fragmento de conteúdo AEM a acessam sem a extensão inserida.


## Testar builds de estágio

1. Abra uma linha de comando na raiz do projeto de extensão.
1. Certifique-se de que o espaço de trabalho do Palco esteja ativo (ou qualquer espaço de trabalho usado para teste).

   ```shell
   $ aio app use -w Stage
   ```

   Mesclar alterações em `.env` e `.aio`.

1. Implante o aplicativo App Builder de extensão atualizado. Se não estiver conectado, execute `aio login` primeiro.

   ```shell
   $ aio app deploy
   ...
   Your deployed actions:
   web actions:
     -> https://98765-123aquarat.adobeio-static.net/api/v1/web/aem-cf-console-admin-1/generic 
   To view your deployed application:
     -> https://98765-123aquarat.adobeio-static.net/index.html
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://98765-123aquarat.adobeio-static.net/index.html
   New Extension Point(s) in Workspace 'Production': 'aem/cf-console-admin/1'
   Successful deployment 🏄
   ```

1. Adicione os dois parâmetros de consulta a seguir à [URL do console de fragmento de conteúdo do AEM](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   Adicione os dois parâmetros de consulta acima (`devMode` e `ext`) como a __primeiro__ parâmetros de consulta no URL, pois o Console do fragmento de conteúdo usa uma rota de hash (`#/@wknd/aem/...`), então a pós-fixação incorreta dos parâmetros após a variável `#` não funcionará.

   O URL de teste deve ser semelhante a:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copie e cole o URL de teste no navegador.
1. O Console do Fragmento de conteúdo do AEM injeta a versão da extensão implantada no espaço de trabalho do Preparo no. Esse URL do Palco pode ser compartilhado com o controle de qualidade ou usuários empresariais para teste.

Lembre-se de que, ao usar essa abordagem, a extensão Preparada é inserida somente no Console do Fragmento de conteúdo do AEM quando o acesso for feito com o URL do estágio de artesanato.

1. As extensões implantadas podem ser atualizadas executando `aio app deploy` novamente e essas alterações serão refletidas automaticamente ao usar o URL de teste.
1. Para remover uma extensão para teste, execute `aio app undeploy`.
