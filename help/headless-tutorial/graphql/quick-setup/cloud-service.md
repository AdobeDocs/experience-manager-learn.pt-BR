---
title: Configuração rápida do AEM sem periféricos para AEM as a Cloud Service
description: A configuração rápida do AEM sem periféricos oferece uma abordagem prática do AEM sem periféricos usando o conteúdo do projeto de amostra do WKND Site e um aplicativo React que consome o conteúdo por meio das APIs do AEM sem periféricos GraphQL.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
duration: 781
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 0%

---

# Configuração rápida do AEM sem periféricos para AEM as a Cloud Service

A configuração rápida do AEM Headless oferece uma abordagem prática do AEM Headless usando o conteúdo do projeto de amostra do WKND Site e uma amostra do aplicativo React (um SPA) que consome o conteúdo por meio das APIs do AEM Headless GraphQL.

## Pré-requisitos

Para seguir esta configuração rápida, é necessário:

+ Ambiente de sandbox da AEM as a Cloud Service (preferencialmente desenvolvimento)
+ Acesso ao AEM as a Cloud Service e ao Cloud Manager
   + Acesso de __Administrador do AEM__ ao AEM as a Cloud Service
   + __Cloud Manager - Acesso do Gerente de implantação__ ao Cloud Manager
+ As seguintes ferramentas devem ser instaladas localmente:
   + [Node.js v18](https://nodejs.org/en/)
   + [Git](https://git-scm.com/)
   + Um IDE (por exemplo, [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

## 1. Criar um repositório Git do Cloud Manager

Primeiro, crie um repositório Git do Cloud Manager usado para implantar o Site WKND. O site WKND é um exemplo de projeto de site do AEM que contém conteúdo (fragmentos de conteúdo) e um terminal GraphQL AEM usado pelo aplicativo React da configuração rápida.

_Screencast de etapas_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. Navegue até [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Selecione o __Programa__ do Cloud Manager que contém o ambiente AEM as a Cloud Service a ser usado para esta configuração rápida
1. Criar um repositório Git para o projeto do Site WKND
   1. Selecione __Repositórios__ na navegação superior
   1. Selecione __Adicionar repositório__ na barra de ações superior
   1. Nomeie o novo repositório Git: `aem-headless-quick-setup-wknd`
      + Os nomes dos repositórios Git devem ser exclusivos para cada organização da Adobe,
   1. Selecione __Salvar__ e aguarde a inicialização do repositório Git

## 2. Encaminhar amostra de projeto do site WKND para o repositório Git da Cloud Manager

Com o repositório Git do Cloud Manager criado, clone o código fonte do projeto do Site WKND do GitHub e envie-o para o repositório Git do Cloud Manager. Agora, o Cloud Manager acessa e implanta o projeto do Site WKND no ambiente do AEM as a Cloud Service.

_Screencast de etapas_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. Na linha de comando, clone do GitHub o código-fonte do projeto do site WKND de amostra

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Adicionar o repositório Git do Cloud Manager como remoto
   1. Selecione __Repositórios__ na navegação superior
   1. Selecione __Acessar informações do repositório__ na barra de ação superior
   1. Executar comando encontrado em __Adicionar um remoto ao repositório Git__ a partir da linha de comando

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. Envie o código-fonte do projeto de amostra do seu repositório Git local para o repositório Git da Cloud Manager

   ```shell
   $ git push adobe main:main
   ```

   Quando as credenciais forem solicitadas, forneça o __Nome de usuário__ e a __Senha__ do modal __Informações do repositório__ da Cloud Manager.

## 3. Implantar o site WKND no AEM as a Cloud Service

Com o projeto do Site WKND enviado para o repositório Git do Cloud Manager, ele não pode ser implantado no AEM as a Cloud Service usando os pipelines do Cloud Manager.

Lembre-se, o projeto do Site WKND fornece conteúdo de amostra que o aplicativo React consome por meio das APIs do AEM Headless GraphQL.

_Screencast de etapas_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. Anexe um __pipeline de implantação de não produção__ ao novo repositório Git
   1. Selecione __Pipelines__ na navegação superior
   1. Selecione __Adicionar pipeline__ na barra de ação superior
   1. Na guia __Configuração__
      1. Selecione a opção __Pipeline de implantação__
      1. Defina o __Nome do Pipeline de Não Produção__ como `Dev Deployment pipeline`
      1. Selecione __Acionador Da Implantação > Sobre Alterações Do Git__
      1. Selecionar __Comportamento de Falhas de Métricas Importantes > Continuar Imediatamente__
      1. Selecionar __Continuar__
   1. Na guia __Código Source__
      1. Selecione a opção __Código de pilha completa__
      1. Selecione o __ambiente de desenvolvimento do AEM as a Cloud Service__ na caixa de seleção __Ambientes de implantação qualificados__
      1. Selecione `aem-headless-quick-setup-wknd` na caixa de seleção __Repositório__
      1. Selecione `main` na caixa de seleção __Ramificação Git__
      1. Selecione __Salvar__
1. Executar o __Pipeline de Implantação de Desenvolvimento__
   1. Selecione __Visão geral__ na navegação superior
   1. Localize o __pipeline de Implantação de Desenvolvimento__ recém-criado na seção __Pipelines__
   1. Selecione o __...__ à direita da entrada do pipeline
   1. Selecione __Executar__ e confirme no modal
   1. Selecione o __...__ à direita do pipeline em execução
   1. Selecione __Exibir detalhes__
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

1. Abra a pasta `~/Code/aem-guides-wknd-graphql/react-app` no IDE.
1. No IDE, abra o arquivo `.env.development`.
1. Aponte para o URI do host do serviço de publicação __AEM as a Cloud Service__ a partir da propriedade `REACT_APP_HOST_URI`.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   Para localizar o URI do host do serviço de publicação do AEM as a Cloud Service:

   1. No Cloud Manager, selecione __Ambientes__ na navegação superior
   1. Selecionar ambiente de __desenvolvimento__
   1. Localize a tabela __Serviço de Publicação (AEM e Dispatcher)__ no link __Segmentos de ambiente__
   1. Copie o endereço do link e use-o como o URI do serviço de publicação do AEM as a Cloud Service

1. No IDE, salve as alterações em `.env.development`
1. Na linha de comando, execute o aplicativo React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. O aplicativo React, executado localmente, começa em [http://localhost:3000](http://localhost:3000) e exibe uma lista de aventuras, que são originadas no AEM as a Cloud Service usando as APIs GraphQL do AEM Headless.

## 5. Editar conteúdo no AEM

Com o aplicativo WKND React de amostra se conectando e consumindo conteúdo das APIs do AEM Headless GraphQL, o conteúdo do autor é criado no serviço de autor do AEM e você pode ver como a experiência do aplicativo React é atualizada em conjunto.

_Screencast de etapas_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. Fazer logon no serviço de Autor do AEM as a Cloud Service
1. Navegue até __Assets > Arquivos > Compartilhado WKND > Inglês > Aventuras__
1. Abra a Pasta __Ciclismo Sul de Utah__
1. Selecione o Fragmento de conteúdo __Ciclo Sul de Utah__ e selecione __Editar__ na barra de ação superior
1. Atualize alguns campos do Fragmento de conteúdo, por exemplo:
   + Título: `Cycling Utah's National Parks`
   + Duração da Viagem: `6 Days`
   + Dificuldade: `Intermediate`
   + Preço: `3500`
   + Imagem Principal: `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. Selecione __Salvar__ na barra de ações superior
1. Selecione __Publicação rápida__ na barra de ações superior __...__
1. Atualize o Aplicativo React em execução em [http://localhost:3000](http://localhost:3000).
1. No aplicativo React, selecione a aventura Ciclismo atualizada e verifique as alterações de conteúdo feitas no fragmento de conteúdo.

1. Usando a mesma abordagem, no serviço AEM Author:
   1. Desfaça a publicação de um fragmento de conteúdo Adventure existente e verifique se ele foi removido da experiência do aplicativo React
   1. Crie e publique um novo Fragmento de conteúdo de aventura e verifique se ele aparece na experiência do aplicativo React

   >[!TIP]
   >
   > Se você não estiver familiarizado com a criação e publicação de novos fragmentos de conteúdo ou com o cancelamento da publicação de fragmentos de conteúdo existentes, assista ao screencast acima.

## Parabéns.

Parabéns! Você usou o AEM Headless com sucesso para potencializar um aplicativo React!

Para entender em detalhes como o aplicativo React consome conteúdo do AEM as a Cloud Service, confira o [tutorial do AEM Headless](../multi-step/overview.md). O tutorial explora como os fragmentos de conteúdo no AEM foram criados e como este aplicativo React consome o conteúdo como JSON.

### Próximas etapas

+ [Iniciar o tutorial do AEM Headless](../multi-step/overview.md)
