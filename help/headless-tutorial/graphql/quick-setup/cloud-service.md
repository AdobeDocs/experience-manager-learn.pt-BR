---
title: Configuração rápida sem cabeçalho AEM para AEM as a Cloud Service
description: A configuração rápida do AEM Headless leva você com AEM Headless usando o conteúdo do projeto de amostra do WKND Site e um aplicativo React que consome o conteúdo sobre AEM APIs GraphQL headless.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
source-git-commit: 94a57490edb00da072446ee8ca07c12c413ce1ac
workflow-type: tm+mt
source-wordcount: '1072'
ht-degree: 2%

---

# Configuração rápida sem cabeçalho AEM para AEM as a Cloud Service

A configuração rápida do AEM Headless leva você com AEM Headless usando o conteúdo do projeto de amostra do WKND Site e uma amostra do React App (a SPA) que consome o conteúdo por meio AEM APIs GraphQL headless.

## Pré-requisitos

Os itens a seguir são necessários para seguir esta configuração rápida:

+ AEM ambiente Sandbox as a Cloud Service (de preferência, desenvolvimento)
+ Acesso ao AEM as a Cloud Service e ao Cloud Manager
   + __Administrador AEM__ acesso a AEM as a Cloud Service
   + __Cloud Manager - Gerenciador de implantação__ acesso ao Cloud Manager
+ As seguintes ferramentas devem ser instaladas localmente:
   + [Node.js v10+](https://nodejs.org/en/)
   + [npm 6+](https://www.npmjs.com/)
   + [Git](https://git-scm.com/)
   + Um IDE (por exemplo, [Código Microsoft® Visual Studio](https://code.visualstudio.com/))

## 1. Crie um repositório Git do Cloud Manager

Primeiro, crie um repositório Git do Cloud Manager usado para implantar o site WKND. O site WKND é um exemplo AEM projeto de site que contém conteúdo (Fragmentos de conteúdo) e um ponto de extremidade GraphQL AEM usado pelo aplicativo React da configuração rápida.

_Screencast de etapas_
>[!VIDEO](https://video.tv.adobe.com/v/339073/?quality=12&learn=on)

1. Navegar para [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Selecione o Cloud Manager __Programa__ que contém o ambiente as a Cloud Service AEM a ser usado para essa configuração rápida
1. Criar um repositório Git para o projeto do site WKND
   1. Selecionar __Repositórios__ no início da navegação
   1. Selecionar __Adicionar Repositório__ na barra de ação superior
   1. Nomeie o novo repositório Git: `aem-headless-quick-setup`
   1. Selecionar __Salvar__ e aguarde até que o repositório Git inicialize

## 2. Encaminhe o projeto de site WKND de amostra para o Repositório Git do Cloud Manager

Com o repositório Git do Cloud Manager criado, clone o código-fonte do projeto do Site WKND do GitHub e envie-o para o repositório Git do Cloud Manager. Agora, o Cloud Manager para acessar e implantar o projeto do Site WKND no ambiente as a Cloud Service AEM.

_Screencast de etapas_
>[!VIDEO](https://video.tv.adobe.com/v/339074/?quality=12&learn=on)

1. Na linha de comando, clone o código-fonte do projeto de site WKND de amostra do GitHub

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Adicionar o repositório Git do Cloud Manager como um repositório remoto
   1. Selecionar __Repositórios__ no início da navegação
   1. Selecionar __Acessar informações do repositório__ na barra de ação superior
   1. Execute o comando found em __Adicionar um repositório remoto ao repositório Git__ na linha de comando

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup/
      ```

1. Encaminhe o código-fonte do projeto de amostra do seu repositório Git local para o repositório Git do Cloud Manager

   ```shell
   $ git push adobe master:main
   ```

   Quando solicitado a fornecer credenciais, forneça o __Nome do usuário__ e __Senha__ do Cloud Manager __Informações do repositório__ modal.

## 3. Implante o site WKND para AEM as a Cloud Service

Com o projeto do site WKND enviado ao repositório Git do Cloud Manager, ele não pode ser implantado em AEM as a Cloud Service usando pipelines do Cloud Manager.

Lembre-se, o projeto de site da WKND fornece conteúdo de amostra que o aplicativo React consome AEM APIs GraphQL sem cabeçalho.

_Screencast de etapas_
>[!VIDEO](https://video.tv.adobe.com/v/339075/?quality=12&learn=on)

1. Anexar um __pipeline de implantação de não produção__ ao novo repositório Git
   1. Selecionar __Pipelines__ no início da navegação
   1. Selecionar __Adicionar pipeline__ na barra de ação superior
   1. No __Configuração__ guia
      1. Selecionar __Pipeline de implantação__ opção
      1. Defina as __Nome do pipeline de não produção__ para `Dev Deployment pipeline`
      1. Selecionar __Acionador De Implantação > Em Alterações No Git__
      1. Selecionar __Comportamento de falhas importantes da métrica > Continuar imediatamente__
      1. Selecionar __Continuar__
   1. No __Código fonte__ guia
      1. Selecionar __Código de pilha completo__ opção
      1. Selecione o __Ambiente de desenvolvimento as a Cloud Service AEM__ do __Ambientes de implantação qualificados__ caixa de seleção
      1. Selecionar `aem-headless-quick-setup` no __Repositório__ caixa de seleção
      1. Selecionar `main` do __Ramificação Git__ caixa de seleção
      1. Selecione __Salvar__
1. Execute o __Pipeline de implantação de desenvolvimento__
   1. Selecionar __Visão geral__ no início da navegação
   1. Localize os __pipeline de implantação de desenvolvimento__ no __Pipelines__ seção
   1. Selecione o __...__ à direita da entrada do pipeline
   1. Selecionar __Executar__ e confirmar no modal
   1. Selecione o __...__ à direita do pipeline que está em execução
   1. Selecionar __Exibir detalhes__
1. A partir dos detalhes da execução do pipeline, monitore o progresso até que ele seja concluído com êxito. A execução do pipeline deve levar de 45 a 60 minutos.

## 4. Baixe e execute o aplicativo WKND React

Com AEM inicialização as a Cloud Service com o conteúdo do projeto do site WKND, baixe e inicie o aplicativo WKND React de amostra que consome o conteúdo do site WKND sobre AEM APIs GraphQL sem interface.

_Screencast de etapas_
>[!VIDEO](https://video.tv.adobe.com/v/339076/?quality=12&learn=on)

1. Na linha de comando, clone o código-fonte do aplicativo React do GitHub.

   ```shell
   $ cd ~/Code
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra a pasta `~/Code/aem-guides-wknd-graphql` no IDE.
1. No IDE, abra o arquivo `react-app/.env.development`.
1. Aponte para o AEM as a Cloud Service __Publicar__ URI do host do serviço do  `REACT_APP_HOST_URI` propriedade.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com/
   ...
   ```

   Para encontrar o URI de host do serviço de Publicação as a Cloud Service AEM:

   1. No Cloud Manager, selecione __Ambientes__ no início da navegação
   1. Selecionar __Desenvolvimento__ ambiente
   1. Localize a variável __Serviço de publicação (AEM e Dispatcher)__ link __Segmentos do ambiente__ tabela
   1. Copie o endereço do link e use-o como URI do serviço de publicação as a Cloud Service AEM

1. No IDE, salve as alterações em `.env.development`
1. Na linha de comando, execute o aplicativo React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. O aplicativo React, executado localmente, começa em [http://localhost:3000](http://localhost:3000) e exibe uma lista de aventuras, que são provenientes de AEM as a Cloud Service usando APIs GraphQL sem periféricos AEM.

## 5. Editar conteúdo no AEM

Com o exemplo de aplicativo WKND React se conectando e consumindo conteúdo das APIs GraphQL sem cabeçalho do AEM, crie conteúdo no serviço AEM Author e veja como a experiência do aplicativo React é atualizada em conjunto.

_Screencast de etapas_
>[!VIDEO](https://video.tv.adobe.com/v/339077/?quality=12&learn=on)

1. Faça logon AEM serviço de criação as a Cloud Service
1. Navegar para __Ativos > Arquivos > WKND > Inglês > Aventuras__
1. Abra o __Cycling Southern Utah__ Pasta
1. Selecione o __Cycling Southern Utah__ Fragmento do conteúdo e selecione __Editar__ na barra de ação superior
1. Atualize alguns dos campos do Fragmento de conteúdo, por exemplo:
   + Título: `Cycling Utah's National Parks`
   + Extensão do Percurso: `6 Days`
   + Dificuldade: `Intermediate`
   + Preço: `$3500`
   + Imagem principal: `/content/dam/wknd/en/activities/cycling/mountain-biking.jpg`
1. Selecionar __Salvar__ na barra de ação superior
1. Selecionar __Publicação rápida__ na barra de ação superior __...__
1. Atualize o aplicativo React em execução em [http://localhost:3000](http://localhost:3000).
1. No aplicativo React, selecione o atualizado e verifique as alterações de conteúdo feitas no Fragmento de conteúdo.

1. Usando a mesma abordagem, no serviço de Autor do AEM:
   1. Cancele a publicação de um Fragmento do conteúdo da Aventura existente e verifique se ele foi removido da experiência do aplicativo React
   1. Crie e publique um novo Fragmento do conteúdo de empreendimento e verifique se ele aparece na experiência do aplicativo React

   >[!TIP]
   >
   > Se você não estiver familiarizado com a criação e a publicação de Fragmentos de conteúdo novos ou com o cancelamento da publicação de fragmentos de conteúdo existentes, assista à transmissão de tela acima.

## Parabéns!

Parabéns! Você usou com sucesso o AEM Headless para alimentar um aplicativo React!

Para entender em detalhes como o aplicativo React consome conteúdo de AEM as a Cloud Service, confira [o tutorial AEM Headless](../multi-step/overview.md). O tutorial explica como os Fragmentos de conteúdo no AEM foram criados e como esse aplicativo React consome seu conteúdo como JSON.

### Próximas etapas

+ [Inicie o tutorial AEM Headless](../multi-step/overview.md)
