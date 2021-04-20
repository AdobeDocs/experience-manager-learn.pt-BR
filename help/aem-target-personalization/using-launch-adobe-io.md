---
title: Integração do Adobe Experience Manager com o Adobe Target usando o Experience Platform Launch e o Adobe I/O
seo-title: Integração do Adobe Experience Manager com o Adobe Target usando o Experience Platform Launch e o Adobe I/O
description: Passo a passo sobre como integrar o Adobe Experience Manager ao Adobe Target usando o Experience Platform Launch e o Adobe I/O
seo-description: Passo a passo sobre como integrar o Adobe Experience Manager ao Adobe Target usando o Experience Platform Launch e o Adobe I/O
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1103'
ht-degree: 2%

---


# Uso do Adobe Experience Platform Launch via console do Adobe I/O

## Pré-requisitos

* [Criação e publicação de ](./implementation.md#set-up-aem) instruções do AEM na porta 4502 e 4503 do host local, respectivamente
* **Experience Cloud**
   * Acesso às suas organizações na Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud provisionada com as seguintes soluções
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Console do Adobe I/O](https://console.adobe.io)

      >[!NOTE]
      >Você deve ter permissão para desenvolver, aprovar, publicar, gerenciar extensões e gerenciar ambientes no Launch. Se não conseguir concluir nenhuma dessas etapas porque as opções da interface do usuário não estão disponíveis para você, entre em contato com o administrador da Experience Cloud para solicitar acesso. Para obter mais informações sobre permissões do Launch, [consulte a documentação](https://docs.adobelaunch.com/administration/user-permissions).


* **Plug-ins do navegador**
   * Adobe Experience Cloud Debugger ([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * Iniciar e Comutador DTM ([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## Usuários envolvidos

Para essa integração, os seguintes públicos-alvo precisam estar envolvidos e, para executar algumas tarefas, pode ser necessário acesso administrativo.

* Desenvolvedor
* Administrador do AEM
* Administrador da Experience Cloud

## Introdução

O AEM oferece uma integração pronta para uso com o Experience Platform Launch. Essa integração permite que os administradores do AEM configurem facilmente o Experience Platform Launch por meio de uma interface fácil de usar, reduzindo assim o nível de esforço e o número de erros, ao configurar essas duas ferramentas. E apenas adicionando a extensão do Adobe Target ao Experience Platform Launch nos ajudará a usar todos os recursos do Adobe Target nas páginas da Web do AEM.

Nesta seção, cobriremos as seguintes etapas de integração:

* Lançamento
   * Criar uma propriedade do Launch
   * Adicionar extensão do Target
   * Criar um elemento de dados
   * Criar uma regra de página
   * Configurar ambientes
   * Criar e publicar
* AEM
   * Criar um Cloud Service
   * Criar

### Lançamento

#### Criar uma propriedade do Launch

Uma propriedade é um contêiner que você preenche com extensões, regras, elementos de dados e bibliotecas à medida que implanta tags no site.

1. Navegue até sua organização [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.experiencecloud.adobe.com)
2. Faça logon usando sua Adobe ID e verifique se você está na organização correta.
3. No alternador de soluções, clique em **Launch** e selecione o botão **Ir para o Launch**.

   ![Experience Cloud - Launch](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. Certifique-se de estar na organização correta e continue com a criação de uma propriedade do Launch.
   ![Experience Cloud - Launch](assets/using-launch-adobe-io/launch-create-property.png)

   *Para obter mais informações sobre como criar propriedades, consulte  [Criar uma ](https://docs.adobelaunch.com/administration/companies-and-properties#create-a-property) propriedade na documentação do produto.*
5. Clique no botão **Nova propriedade**
6. Forneça um nome para sua propriedade (por exemplo, *Tutorial do AEM Target*)
7. Como o domínio, insira *localhost.com*, pois este é o domínio em que o site de demonstração WKND está sendo executado. Embora o campo &#39;*Domain*&#39; seja obrigatório, a propriedade do Launch funcionará em qualquer domínio em que for implementada. A finalidade principal desse campo é preencher previamente as opções de menu no Construtor de regras.
8. Clique no botão **Save**.

   ![Launch - Nova propriedade](assets/using-launch-adobe-io/exc-launch-property.png)

9. Abra a propriedade que acabou de criar e clique na guia Extensões .

#### Adicionar extensão do Target

A extensão do Adobe Target suporta implementações do lado do cliente usando o SDK JavaScript do Target para a Web moderna, `at.js`. Os clientes que ainda usam a biblioteca mais antiga do Target, `mbox.js`, [devem atualizar para at.js](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/upgrading-from-atjs-1x-to-atjs-20.html) para usar o Launch.

A extensão do Target consiste em duas partes principais:

* A configuração da extensão, que gerencia as principais configurações da biblioteca
* Ações de regras para fazer o seguinte:
   * Carregar o Target (at.js)
   * Adicionar params a todas as mboxes
   * Adicionar params à mBox global
   * Acionar mbox global

1. Em **Extensões**, você pode ver a lista de Extensões que já estão instaladas para a propriedade do Launch. ([A Extensão principal do Experience Platform Launch](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html) está instalada por padrão)
2. Clique na opção **Catálogo de extensões** e pesquise pelo Target no filtro.
3. Selecione a versão mais recente do Adobe Target at.js e clique na opção **Instalar** .
   ![Launch - Nova propriedade](assets/using-launch-adobe-io/launch-target-extension.png)

4. Clique no botão **Configure** e você pode observar a janela de configuração com suas credenciais de conta do Target importadas e a versão da at.js para essa extensão.
   ![Target - Configuração de extensão](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Quando o Target é implantado por meio de códigos incorporados assíncronos do Launch, é necessário codificar um trecho de pré-ocultação nas páginas antes dos códigos incorporados do Launch para gerenciar a oscilação de conteúdo. Mais tarde aprenderemos sobre o snipper de pré-ocultação. Você pode baixar o trecho pré-ocultação [aqui](assets/using-launch-adobe-io/prehiding.js)

5. Clique em **Salvar** para concluir a adição da extensão do Target à propriedade do Launch e agora você pode ver a extensão do Target listada na lista de extensões **Instaladas**.

6. Repita as etapas acima para pesquisar a extensão &quot;Serviço da Experience Cloud ID&quot; e instalá-la.
   ![Extensão - Serviço da Experience Cloud ID](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Configurar ambientes

1. Clique na guia **Environment** para a propriedade do site e poderá ver a lista de ambientes que é criada para a propriedade do site. Por padrão, temos uma instância criada para desenvolvimento, armazenamento temporário e produção.

![Elemento de dados - Nome da página](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Criar e publicar

1. Clique na guia **Publicação** para a propriedade do site, e vamos criar uma biblioteca para criar e implantar as alterações (elementos de dados, regras) em um ambiente de desenvolvimento.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Publique suas alterações do ambiente de desenvolvimento para um ambiente de preparo.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Execute a opção **Criar para Preparo**.
4. Quando a build estiver concluída, execute **Aprovar para publicação**, o que move as alterações de um ambiente de preparo para um ambiente de produção.
   ![Armazenamento temporário para produção](assets/using-launch-adobe-io/build-staging.png)
5. Por fim, execute a opção **Build and Publish to production** para enviar as alterações para produção.
   ![Criar e publicar na produção](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Conceda à integração do Adobe I/O acesso para selecionar espaços de trabalho com a função [apropriada para permitir que uma equipe central faça alterações orientadas por API em apenas alguns espaços de trabalho](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html).

1. Criar integração IMS no AEM usando credenciais do Adobe I/O. (01:12 para 03:55)
2. No Experience Platform Launch, crie uma propriedade. (coberto [acima](#create-launch-property))
3. Usando a integração IMS da Etapa 1, crie a integração do Experience Platform Launch para importar a propriedade do Launch.
4. No AEM, mapeie a integração do Experience Platform Launch para um site usando a configuração do navegador. (05:28 às 06:14)
5. Valide a integração manualmente. (06:15 às 06:33)
6. Uso do plug-in do navegador do Launch/DTM. (06:34 às 06:50)
7. Usar o plug-in do navegador do Adobe Experience Cloud Debugger. (06:51 às 07:22)

Neste ponto, você integrou com êxito o [AEM ao Adobe Target usando o Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options), conforme detalhado na Opção 1.

Para usar as ofertas de Fragmento de experiência do AEM para potencializar suas atividades de personalização, vamos prosseguir para o próximo capítulo e integrar o AEM com o Adobe Target usando os serviços de nuvem herdados.
