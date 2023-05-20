---
title: AEM Headless configuração rápida para AEM as a Cloud Service
description: A configuração rápida do AEM sem periféricos oferece uma abordagem prática do AEM sem periféricos usando conteúdo do projeto de amostra do WKND Site e um aplicativo React que consome o conteúdo por meio das APIs do GraphQL do AEM sem periféricos.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '1081'
ht-degree: 2%

---

# AEM Headless configuração rápida para AEM as a Cloud Service

A configuração rápida do AEM sem periféricos oferece uma abordagem prática do AEM sem periféricos usando conteúdo do projeto de amostra do WKND Site e uma amostra do aplicativo React (um SPA) que consome o conteúdo por meio das APIs do GraphQL do AEM sem periféricos.

## Pré-requisitos

Para seguir esta configuração rápida, é necessário:

+ Ambiente de sandbox as a Cloud Service AEM (preferencialmente desenvolvimento)
+ Acesso ao AEM as a Cloud Service e ao Cloud Manager
   + __Administrador AEM__ acesso ao AEM as a Cloud Service
   + __Cloud Manager - Gerenciador de implantação__ acesso ao Cloud Manager
+ As seguintes ferramentas devem ser instaladas localmente:
   + [Node.js v18](https://nodejs.org/en/)
   + [Git](https://git-scm.com/)
   + Um IDE (por exemplo [Código do Microsoft® Visual Studio](https://code.visualstudio.com/))

## 1. Criar um repositório Git do Cloud Manager

Primeiro, crie um repositório Git do Cloud Manager que é usado para implantar o Site WKND. O Site WKND é um projeto de exemplo de site AEM que contém conteúdo (fragmentos de conteúdo) e um endpoint do GraphQL AEM usado pelo aplicativo React da configuração rápida.

_Screencast de etapas_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. Navegue até [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Selecione o Cloud Manager __Programa__ que contém o ambiente do AEM as a Cloud Service a ser usado para essa configuração rápida
1. Criar um repositório Git para o projeto do Site WKND
   1. Selecionar __Repositórios__ na navegação superior
   1. Selecionar __Adicionar repositório__ na barra de ação superior
   1. Nomeie o novo repositório Git: `aem-headless-quick-setup-wknd`
      + Os nomes dos repositórios Git devem ser exclusivos por organização Adobe,
   1. Selecionar __Salvar__ e aguarde até que o repositório Git seja inicializado

## 2. Envie uma amostra do projeto do site WKND para o repositório Git do Cloud Manager

Com o repositório Git do Cloud Manager criado, clone o código-fonte do projeto do Site WKND do GitHub e envie-o para o repositório Git do Cloud Manager. Agora, o Cloud Manager permite acessar e implantar o projeto do Site WKND no ambiente as a Cloud Service AEM.

_Screencast de etapas_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. Na linha de comando, clone do GitHub o código-fonte do projeto do site WKND de amostra

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Adicionar o repositório Git do Cloud Manager como remoto
   1. Selecionar __Repositórios__ na navegação superior
   1. Selecionar __Acessar informações do repositório__ na barra de ação superior
   1. Comando Executar encontrado em __Adicione um remoto ao seu repositório Git__ da linha de comando

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. Envie o código-fonte do projeto de amostra do seu repositório Git local para o repositório Git do Cloud Manager

   ```shell
   $ git push adobe main:main
   ```

   Quando solicitado a fornecer credenciais, forneça a __Nome de usuário__ e __Senha__ do Cloud Manager __Informações do repositório__ modal.

## 3. Implantar o site da WKND no AEM as a Cloud Service

Com o projeto do Site WKND enviado para o repositório Git do Cloud Manager, ele não pode ser implantado no AEM as a Cloud Service usando os pipelines do Cloud Manager.

Lembre-se, o projeto Site WKND fornece conteúdo de amostra que o aplicativo React consome por meio de APIs AEM Headless GraphQL.

_Screencast de etapas_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. Anexar um __Pipeline de implantação de não produção__ ao novo repositório Git
   1. Selecionar __Pipelines__ na navegação superior
   1. Selecionar __Adicionar pipeline__ na barra de ação superior
   1. No __Configuração__ guia
      1. Selecionar __Pipeline de implantação__ opção
      1. Defina o __Nome do pipeline de não produção__ para `Dev Deployment pipeline`
      1. Selecionar __Acionador de implantação > Sobre alterações do Git__
      1. Selecionar __Comportamento de falhas de métricas importantes > Continuar imediatamente__
      1. Selecionar __Continuar__
   1. Na guia __Código-fonte__
      1. Selecionar __Código de pilha completa__ opção
      1. Selecione o __Ambiente de desenvolvimento as a Cloud Service AEM__ do __Ambientes de implantação qualificados__ caixa selecionar
      1. Selecionar `aem-headless-quick-setup-wknd` no __Repositório__ caixa selecionar
      1. Selecionar `main` do __Ramificação Git__ caixa selecionar
      1. Selecione __Salvar__
1. Execute o __Pipeline de implantação de desenvolvimento__
   1. Selecionar __Visão geral__ na navegação superior
   1. Localize o recém-criado __Pipeline de implantação de desenvolvimento__ no __Pipelines__ seção
   1. Selecione o __..__ à direita da entrada do pipeline
   1. Selecionar __Executar__, e confirme no modal
   1. Selecione o __..__ à direita do pipeline em execução
   1. Selecionar __Exibir detalhes__
1. Nos detalhes de execução do pipeline, monitore o progresso até que ele seja concluído com êxito. A execução do pipeline deve levar entre 30 e 40 minutos.

## 4. Baixe e execute o aplicativo WKND React

Com o AEM as a Cloud Service inicializado com o conteúdo do projeto do Site WKND, baixe e inicie o aplicativo WKND React de amostra que consome o conteúdo do Site WKND por meio das APIs do AEM Headless GraphQL.

_Screencast de etapas_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. Na linha de comando, clone o código-fonte do aplicativo React do GitHub.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abrir a pasta `~/Code/aem-guides-wknd-graphql/react-app` no IDE.
1. No IDE, abra o arquivo `.env.development`.
1. Aponte para o AEM as a Cloud Service __Publish__ URI do host do serviço do  `REACT_APP_HOST_URI` propriedade.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   Para encontrar o URI do host do serviço de publicação do AEM as a Cloud Service:

   1. No Cloud Manager, selecione __Ambientes__ na navegação superior
   1. Selecionar __Desenvolvimento__ ambiente
   1. Localize o __Serviço de publicação (AEM e Dispatcher)__ link __Segmentos de ambiente__ tabela
   1. Copie o endereço do link e use-o como o URI do serviço de publicação as a Cloud Service do AEM

1. No IDE, salve as alterações em `.env.development`
1. Na linha de comando, execute o aplicativo React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. O aplicativo React, executado localmente, inicia em [http://localhost:3000](http://localhost:3000) e exibe uma lista de aventuras, que são originadas do AEM as a Cloud Service usando as APIs do GraphQL do AEM Headless.

## 5. Editar conteúdo no AEM

Com o aplicativo WKND React de amostra se conectando e consumindo conteúdo das APIs AEM Headless do GraphQL, crie conteúdo no serviço do AEM Author e veja como a experiência do aplicativo React é atualizada em conjunto.

_Screencast de etapas_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. Fazer logon no serviço de Autor do AEM as a Cloud Service
1. Navegue até __Ativos > Arquivos > Compartilhado com WKND > Inglês > Aventuras__
1. Abra o __Ciclismo Sul de Utah__ Pasta
1. Selecione o __Ciclismo Sul de Utah__ Fragmento do conteúdo e selecione __Editar__ na barra de ação superior
1. Atualize alguns campos do Fragmento de conteúdo, por exemplo:
   + Título: `Cycling Utah's National Parks`
   + Duração do Percurso: `6 Days`
   + Dificuldade: `Intermediate`
   + Preço: `3500`
   + Imagem principal: `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. Selecionar __Salvar__ na barra de ação superior
1. Selecionar __Publicação rápida__ na barra de ação superior __..__
1. Atualizar o aplicativo React em execução em [http://localhost:3000](http://localhost:3000).
1. No aplicativo React, selecione a aventura Ciclismo atualizada e verifique as alterações de conteúdo feitas no fragmento de conteúdo.

1. Usando a mesma abordagem, no serviço do AEM Author:
   1. Desfaça a publicação de um fragmento de conteúdo Adventure existente e verifique se ele foi removido da experiência do aplicativo React
   1. Crie e publique um novo Fragmento de conteúdo de aventura e verifique se ele aparece na experiência do aplicativo React

   >[!TIP]
   >
   > Se você não estiver familiarizado com a criação e publicação de novos fragmentos de conteúdo ou com o cancelamento da publicação de fragmentos de conteúdo existentes, assista ao screencast acima.

## Parabéns!

Parabéns! Você usou o AEM Headless com sucesso para alimentar um aplicativo React!

Para entender em detalhes como o aplicativo React consome conteúdo do AEM as a Cloud Service, confira [o tutorial AEM Headless](../multi-step/overview.md). O tutorial explora como os fragmentos de conteúdo no AEM foram criados e como este aplicativo React consome o conteúdo como JSON.

### Próximas etapas

+ [Iniciar o tutorial AEM Headless](../multi-step/overview.md)
