---
title: Teste de uma extensão do console Fragmento de conteúdo AEM
description: Saiba como testar uma extensão do console Fragmento de conteúdo AEM antes de implantar na produção.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: 56e2cbadaceb9961de28454bfbed56a98df34c44
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---


# Testar uma extensão

AEM extensões do console do fragmento de conteúdo podem ser testadas em relação a qualquer ambiente AEM as a Cloud Service na organização do Adobe à qual a extensão pertence.

O teste de uma extensão é feito por meio de um URL especialmente criado que instrui o console Fragmento de conteúdo AEM a carregar a extensão.

## URL do console Fragmento do conteúdo do AEM

![URL do console Fragmento do conteúdo do AEM](./assets/test/content-fragment-console-url.png){align="center"}

Para criar um URL que monte a extensão de não-produção em um console de Fragmento de conteúdo AEM, o URL do console de Fragmento de conteúdo AEM desejado deve ser coletado. Navegue até o ambiente as a Cloud Service AEM para testar a extensão e copie o URL do console do Fragmento de conteúdo AEM.

1. Faça logon no env as a Cloud Service desejado.

   + Usar um ambiente de desenvolvimento de AEM para [testando criações de desenvolvimento](#testing-development-builds)
   + Use o ambiente de preparo AEM ou desenvolvimento de controle de qualidade para [teste de construções de estágio](#testing-stage-builds)

1. Selecione o __Fragmentos de conteúdo__ ícone .
1. Aguarde até que o console Fragmento do conteúdo do AEM seja carregado no navegador.
1. Copie o URL do console do fragmento de conteúdo do AEM na barra de endereços do navegador, ele deve parecer:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

Esse URL é usado abaixo ao criar os URLs para desenvolvimento e teste de estágio.

## Testar criações de desenvolvimento local

1. Abra uma linha de comando para a raiz do projeto de extensão.
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

1. Adicione os dois parâmetros de consulta a seguir ao [URL do console do fragmento de conteúdo do AEM](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, normalmente `&ext=https://localhost:9080`.

   Adicione os dois parâmetros de consulta acima (`devMode` e `ext`) como a __first__ consulte parâmetros no URL, pois o console do fragmento de conteúdo usa uma rota de hash (`#/@wknd/aem/...`), portanto, a correção incorreta dos parâmetros após a `#` não funcionará.

   O URL de teste deve ser semelhante a:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copie e cole o URL de teste no navegador.

   + Você pode ter que começar, e depois periodicamente, você deve [aceitar o certificado HTTPS](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) para o host do aplicativo local (`https://localhost:9080`).

1. O console Fragmento do conteúdo do AEM é carregado com a versão local da extensão injetada para teste e as alterações de recarregamento automático enquanto o aplicativo local do App Builder estiver em execução.

>[!IMPORTANT]
>
>Lembre-se de que, ao usar essa abordagem, a extensão em desenvolvimento afeta apenas sua experiência e todos os outros usuários do console Fragmento de conteúdo AEM a acessam sem a extensão injetada.


## Criar estágios de teste

1. Abra uma linha de comando para a raiz do projeto de extensão.
1. Certifique-se de que o espaço de trabalho de Preparo esteja ativo (ou qualquer espaço de trabalho usado para testes).

   ```shell
   $ aio app use -w Stage
   ```
   Mesclar quaisquer alterações no `.env` e `.aio`.
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

1. Adicione os dois parâmetros de consulta a seguir ao [URL do console do fragmento de conteúdo do AEM](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   Adicione os dois parâmetros de consulta acima (`devMode` e `ext`) como a __first__ consulte parâmetros no URL, pois o console do fragmento de conteúdo usa uma rota de hash (`#/@wknd/aem/...`), portanto, a correção incorreta dos parâmetros após a `#` não funcionará.

   O URL de teste deve ser semelhante a:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copie e cole o URL de teste no navegador.
1. O console Fragmento do conteúdo do AEM injeta a versão da extensão implantada no Espaço de trabalho do Stage. Esse URL de preparo pode ser compartilhado com o controle de qualidade ou usuários comerciais para testes.

Lembre-se, ao usar essa abordagem, a extensão Preparado só é injetada no do Console do Fragmento de Conteúdo AEM quando o acesso com o URL do Estágio da arte é realizado.

1. As extensões implantadas podem ser atualizadas ao executar `aio app deploy` novamente, e essas alterações refletem automaticamente ao usar o URL de teste.
1. Para remover uma extensão para teste, execute `aio app undeploy`.



