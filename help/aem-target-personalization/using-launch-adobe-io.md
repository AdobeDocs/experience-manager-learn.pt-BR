---
title: Integração do Adobe Experience Manager com a Adobe Target usando tags e o Adobe Developer
description: Apresentação passo a passo sobre como integrar o Adobe Experience Manager ao Adobe Target usando tags e Adobe Developer
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b1d7ce04-0127-4539-a5e1-802d7b9427dd
duration: 663
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '985'
ht-degree: 2%

---

# Uso de tags pela Adobe Developer Console

## Pré-requisitos

* [Instância de autoria e publicação do AEM](./implementation.md#set-up-aem) em execução nas portas localhost 4502 e 4503, respectivamente
* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - `https://<yourcompany>.experiencecloud.adobe.com`
   * Provisionamento de Experience Cloud com as seguintes soluções
      * [Coleta de dados](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe Developer Console](https://developer.adobe.com/console/)

     >[!NOTE]
     >Você deve ter permissão para desenvolver, aprovar, Publish, gerenciar extensões e gerenciar ambientes na coleção de dados. Se você não conseguir concluir nenhuma dessas etapas porque as opções da interface do usuário não estão disponíveis para você, entre em contato com o administrador do Experience Cloud para solicitar acesso. Para obter mais informações sobre permissões de marcas, [consulte a documentação](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html?lang=pt-BR).

* **Extensões do navegador Chrome**
   * Adobe Experience Cloud Debugger (https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)

## Usuários envolvidos

Para essa integração, os seguintes públicos-alvo devem ser envolvidos e, para executar algumas tarefas, talvez seja necessário acesso administrativo.

* Desenvolvedor
* Administrador do AEM
* Administrador do Experience Cloud

## Introdução

O AEM oferece uma integração imediata com tags. Essa integração permite que os administradores do AEM configurem tags facilmente por meio de uma interface fácil de usar, reduzindo assim o nível de esforço e o número de erros ao configurar essas duas ferramentas. E apenas adicionar a extensão Adobe Target às tags nos ajudará a usar todos os recursos do Adobe Target nas páginas da Web do AEM.

Nesta seção, abordaremos as seguintes etapas de integração:

* Tags
   * Criar uma propriedade de tags
   * Adicionar extensão do Target
   * Criar um elemento de dados
   * Criar uma regra de página
   * Configurar ambientes
   * Criar e publicar
* AEM
   * Criar um Cloud Service
   * Criar

### Tags

#### Criar uma propriedade de tags

Uma propriedade do é um container que você preenche com extensões, regras, elementos de dados e bibliotecas à medida que implanta tags no site.

1. Navegue até sua organização [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`https://<yourcompany>.experiencecloud.adobe.com`)
1. Faça logon usando sua Adobe ID e verifique se você está na organização correta.
1. No alternador de soluções, clique em **Experience Platform**, na seção **Coleção de dados** e selecione **Marcas**.

![Experience Cloud - tags](assets/using-launch-adobe-io/exc-cloud-launch.png)

1. Verifique se você está na organização correta e continue com a criação de uma propriedade de tags.
   ![Experience Cloud - tags](assets/using-launch-adobe-io/launch-create-property.png)

   *Para obter mais informações sobre como criar propriedades, consulte [Criar uma Propriedade](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=pt-BR#create-or-configure-a-property) na documentação do produto.*
1. Clique no botão **Nova propriedade**
1. Forneça um nome para a propriedade (Por exemplo, *Tutorial do AEM Target*)
1. Como domínio, digite *localhost.com*, pois este é o domínio no qual o site de demonstração do WKND está sendo executado. Embora o campo &#39;*Domínio*&#39; seja obrigatório, a propriedade de marcas funcionará em qualquer domínio que for implementada. O objetivo principal desse campo é preencher previamente as opções de menu no Construtor de regras.
1. Clique no botão **Salvar**.

   ![marcas - Nova propriedade](assets/using-launch-adobe-io/exc-launch-property.png)

1. Abra a propriedade que você acabou de criar e clique na guia Extensões.

#### Adicionar extensão do Target

A extensão do Adobe Target oferece suporte a implementações do lado do cliente usando o SDK JavaScript do Target para a Web moderna, `at.js`. Os clientes que ainda usam a biblioteca mais antiga do Target, `mbox.js`, [devem atualizar para at.js](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html?lang=pt-BR) para usar tags.

A extensão do Target consiste em duas partes principais:

* A configuração da extensão, que gerencia as principais configurações da biblioteca
* Ações de regras para fazer o seguinte:
   * Carregar Target (at.js)
   * Adicionar params a todas as mBoxes
   * Adicionar params à mBox global
   * Acionar mBox global

1. Em **Extensões**, você pode ver a lista de Extensões já instaladas para sua propriedade de marcas. ([A Extensão Principal do Adobe Launch](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) é instalada por padrão)
2. Clique na opção **Catálogo de Extensão** e procure pelo Target no filtro.
3. Selecione a versão mais recente da at.js do Adobe Target e clique na opção **Instalar**.
   ![Marcas - Nova Propriedade](assets/using-launch-adobe-io/launch-target-extension.png)

4. Clique no botão **Configurar** e você poderá observar a janela de configuração com suas credenciais de conta do Target importadas e a versão da at.js para essa extensão.
   ![Target - Configuração de Extensão](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Quando o Target é implantado por meio de códigos incorporados de tags assíncronas, você deve codificar um trecho pré-ocultado em suas páginas antes dos códigos incorporados das tags para gerenciar a oscilação de conteúdo. Mais tarde, aprenderemos mais sobre o snipper que está pré-ocultando. Você pode baixar o trecho oculto previamente [aqui](assets/using-launch-adobe-io/prehiding.js)

5. Clique em **Salvar** para concluir a adição da extensão de Destino à propriedade de marcas e agora você poderá ver a extensão de Destino listada na lista de extensões **Instaladas**.

6. Repita as etapas acima para procurar a extensão &quot;Experience Cloud ID Service&quot; e instale-a.
   ![Extensão - Serviço de ID do Experience Cloud](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Configurar ambientes

1. Clique na guia **Ambiente** da propriedade do site e você poderá ver a lista de ambientes criada para a propriedade do site. Por padrão, temos uma instância criada para desenvolvimento, preparo e produção.

![Elemento de Dados - Nome da Página](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Criar e publicar

1. Clique na guia **Publicação** da propriedade do site e vamos criar uma biblioteca para criar e implantar nossas alterações (elementos de dados, regras) em um ambiente de desenvolvimento.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Publish suas alterações do ambiente de desenvolvimento para o de preparo.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Execute a **Build para a opção de preparo**.
4. Depois que a compilação for concluída, execute **Aprovar para Publicação**, que move suas alterações de um ambiente de Preparo para um ambiente de Produção.
   ![Preparando para produção](assets/using-launch-adobe-io/build-staging.png)
5. Por fim, execute a opção **Build e Publish para produção** para enviar suas alterações para produção.
   ![Build e Publish para produção](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Conceda acesso à integração do Adobe Developer para selecionar espaços de trabalho com a [função apropriada para permitir que uma equipe central faça alterações orientadas por API em apenas alguns espaços de trabalho](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html?lang=pt-BR).

1. Crie a integração IMS no AEM usando credenciais da Adobe Developer. (01:12 a 03:55)
2. Em Coleção de dados, crie uma propriedade. (coberto [acima](#create-launch-property))
3. Usando a integração IMS da Etapa 1, crie a integração de tags para importar sua propriedade de tags.
4. No AEM, mapeie a integração de tags a um site usando a configuração do navegador. (05:28 a 06:14)
5. Valide a integração manualmente. (06:15 a 06:33)
6. Uso do plug-in do navegador do Adobe Experience Cloud Debugger. (06:51 a 07:22)

Neste ponto, você integrou com êxito o [AEM ao Adobe Target usando tags](./using-aem-cloud-services.md#integrating-aem-target-options), conforme detalhado na Opção 1.

Para usar as ofertas de Fragmento de experiência do AEM para potencializar suas atividades de personalização, vamos prosseguir para o próximo capítulo e integrar o AEM ao Adobe Target usando os serviços de nuvem herdados.
