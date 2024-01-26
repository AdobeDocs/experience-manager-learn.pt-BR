---
title: Integração do Adobe Experience Manager com o Adobe Target usando o Experience Platform Launch e o Adobe Developer
description: Passo a passo sobre como integrar o Adobe Experience Manager com o Adobe Target usando o Experience Platform Launch e o Adobe Developer
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b1d7ce04-0127-4539-a5e1-802d7b9427dd
duration: 747
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1021'
ht-degree: 4%

---

# Utilização do Adobe Experience Platform Launch pelo console do Adobe Developer

## Pré-requisitos

* [Autor e instância de publicação do AEM](./implementation.md#set-up-aem) executando nas portas localhost 4502 e 4503, respectivamente
* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - `https://<yourcompany>.experiencecloud.adobe.com`
   * Provisionamento de Experience Cloud com as seguintes soluções
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Console do Adobe Developer](https://developer.adobe.com/console/)

     >[!NOTE]
     >Você deve ter permissão para desenvolver, aprovar, publicar, gerenciar extensões e gerenciar ambientes no Launch. Se você não conseguir concluir nenhuma dessas etapas porque as opções da interface do usuário não estão disponíveis para você, entre em contato com o administrador do Experience Cloud para solicitar acesso. Para obter mais informações sobre as permissões do Launch, [consulte a documentação](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html).

* **Plug-ins do navegador**
   * Adobe Experience Cloud Debugger ([Cromo](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob))
   * Launch e o Switch do DTM ([Cromo](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## Usuários envolvidos

Para essa integração, os seguintes públicos-alvo devem ser envolvidos e, para executar algumas tarefas, talvez seja necessário acesso administrativo.

* Desenvolvedor
* Administrador do AEM
* Administrador do Experience Cloud

## Introdução

O AEM oferece uma integração pronta para uso com o Experience Platform Launch. Essa integração permite que os administradores de AEM configurem facilmente o Experience Platform Launch por meio de uma interface fácil de usar, reduzindo assim o nível de esforço e o número de erros ao configurar essas duas ferramentas. E apenas adicionar a extensão Adobe Target ao Experience Platform Launch nos ajudará a usar todos os recursos do Adobe Target nas páginas da web do AEM.

Nesta seção, abordaremos as seguintes etapas de integração:

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

Uma propriedade do é um container que você preenche com extensões, regras, elementos de dados e bibliotecas à medida que implanta tags no site.

1. Navegue até suas organizações [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`https://<yourcompany>.experiencecloud.adobe.com`)
2. Faça logon usando sua Adobe ID e verifique se você está na organização correta.
3. No alternador de soluções, clique em **Launch** e, em seguida, selecione a **Ir para o Launch** botão.

   ![Experience Cloud - Launch](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. Verifique se você está na organização correta e continue com a criação de uma propriedade do Launch.
   ![Experience Cloud - Launch](assets/using-launch-adobe-io/launch-create-property.png)

   *Para obter mais informações sobre a criação de propriedades, consulte [Criar uma propriedade](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=en#create-or-configure-a-property) na documentação do produto.*
5. Clique no link **Nova propriedade** botão
6. Forneça um nome para a propriedade (por exemplo, *Tutorial do AEM Target*)
7. Como domínio, insira *localhost.com* pois este é o domínio em que o site de demonstração do WKND está em execução. Embora o &#39;*Domínio*&#39; for obrigatório, a propriedade do Launch funcionará em qualquer domínio que for implementada. O objetivo principal desse campo é preencher previamente as opções de menu no Construtor de regras.
8. Clique em **Salvar** botão.

   ![Launch - Nova propriedade](assets/using-launch-adobe-io/exc-launch-property.png)

9. Abra a propriedade que você acabou de criar e clique na guia Extensões.

#### Adicionar extensão do Target

A extensão do Adobe Target é compatível com implementações do lado do cliente usando o SDK JavaScript do Target para a Web moderna, `at.js`. Clientes que ainda usam a biblioteca mais antiga do Target, `mbox.js`, [deve atualizar para o at.js](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html) para usar o Launch.

A extensão do Target consiste em duas partes principais:

* A configuração da extensão, que gerencia as principais configurações da biblioteca
* Ações de regras para fazer o seguinte:
   * Carregar Target (at.js)
   * Adicionar params a todas as mBoxes
   * Adicionar params à mBox global
   * Acionar mBox global

1. Em **Extensões**, você pode ver a lista de extensões que já estão instaladas para sua propriedade do Launch. ([Extensão principal do Experience Platform Launch](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) é instalado por padrão)
2. Clique no link **Catálogo de extensões** e procure por Target no filtro.
3. Selecione a versão mais recente da at.js do Adobe Target e clique em **Instalar** opção.
   ![Iniciar - Nova propriedade](assets/using-launch-adobe-io/launch-target-extension.png)

4. Clique em **Configurar** e você poderá observar a janela de configuração com suas credenciais de conta do Target importadas e a versão da at.js para essa extensão.
   ![Target - Configuração de extensão](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Quando o Target é implantado por meio de códigos incorporados assíncronos do Launch, você deve codificar um trecho pré-ocultado em suas páginas antes dos códigos incorporados do Launch para gerenciar a oscilação de conteúdo. Mais tarde, aprenderemos mais sobre o snipper que está pré-ocultando. Você pode baixar o trecho pré-ocultação [aqui](assets/using-launch-adobe-io/prehiding.js)

5. Clique em **Salvar** para concluir a adição da extensão do Target à propriedade do Launch, e você poderá ver a extensão do Target listada em **Instalado** lista de extensões.

6. Repita as etapas acima para procurar a extensão &quot;Experience Cloud ID Service&quot; e instale-a.
   ![Extensão - Experience Cloud ID Service](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Configurar ambientes

1. Clique no link **Ambiente** para a propriedade do site, e você pode ver a lista de ambientes criada para a propriedade do site. Por padrão, temos uma instância criada para desenvolvimento, preparo e produção.

![Elemento de dados - Nome da página](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Criar e publicar

1. Clique no link **Publicação** para a propriedade do site, e vamos criar uma biblioteca para criar e implantar nossas alterações (elementos de dados, regras) em um ambiente de desenvolvimento.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Publique as alterações de Desenvolvimento em um ambiente de preparo.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Execute o **Opção Criar para preparo**.
4. Depois que a criação for concluída, execute **Aprovar para publicação**, que move suas alterações de um ambiente de preparo para um ambiente de produção.
   ![Preparo para produção](assets/using-launch-adobe-io/build-staging.png)
5. Por fim, execute o **Criar e publicar na produção** opção para enviar as alterações para produção.
   ![Criar e publicar para produção](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Conceda acesso à integração do Adobe Developer para selecionar espaços de trabalho com o apropriado [função para permitir que uma equipe central faça alterações orientadas por API em apenas alguns espaços de trabalho](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html).

1. Crie a integração IMS no AEM usando credenciais da Adobe Developer. (01:12 a 03:55)
2. No Experience Platform Launch, crie uma propriedade. (coberto [acima](#create-launch-property))
3. Usando a integração IMS da Etapa 1, crie a integração Experience Platform Launch para importar sua propriedade do Launch.
4. No AEM, mapeie a integração de Experience Platform Launch para um site usando a configuração do navegador. (05:28 a 06:14)
5. Valide a integração manualmente. (06:15 a 06:33)
6. Uso do plug-in de navegador do Launch/DTM. (06:34 às 06:50)
7. Uso do plug-in do navegador do Adobe Experience Cloud Debugger. (06:51 a 07:22)

Neste ponto, você integrou o com êxito [AEM com Adobe Target usando Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options) conforme descrito na opção 1.

Para usar as ofertas de Fragmento de experiência do AEM para potencializar suas atividades de personalização, vamos prosseguir para o próximo capítulo e integrar o AEM ao Adobe Target usando os serviços de nuvem herdados.
