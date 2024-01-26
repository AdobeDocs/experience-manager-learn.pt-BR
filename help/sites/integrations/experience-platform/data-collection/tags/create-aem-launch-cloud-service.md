---
title: Criar uma configuração do Launch Cloud Service no AEM Sites
description: Saiba como criar uma configuração de Cloud Service do Launch no AEM. A configuração do Cloud Service do Launch pode ser aplicada a um site existente, e as bibliotecas de tags podem ser observadas nos ambientes do Author e Publish.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5982
thumbnail: 38566.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
duration: 139
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 0%

---

# Criar uma configuração do Cloud Service do Launch no AEM {#create-launch-cloud-service}

>[!NOTE]
>
>O processo de renomear o Adobe Experience Platform Launch como um conjunto de tecnologias de coleção de dados está sendo implementado na interface, no conteúdo e na documentação do produto AEM, portanto, o termo Launch ainda está sendo usado aqui.

Saiba como criar uma configuração do Launch Cloud Service no Adobe Experience Manager. A configuração do Cloud Service de inicialização do AEM pode ser aplicada a um site existente, e as bibliotecas de tags podem ser observadas nos ambientes Autor e Publicação.

## Criar serviço em nuvem do Launch

Crie a configuração do Launch Cloud Service usando as etapas abaixo.

1. No **Ferramentas** selecione **Cloud Service** e clique em **Configurações do Adobe Launch**

1. Selecione a pasta de configuração do site ou selecione **Site da WKND** (se estiver usando um projeto do guia WKND) e clique em **Criar**

1. No _Geral_ nomeie a configuração usando o ícone **Título** e selecione **Adobe Launch** do _Configuração IMS da Adobe associada_ lista suspensa. Em seguida, selecione o nome da sua empresa na _Empresa_ e selecione a propriedade criada anteriormente na lista suspensa _Propriedade_ lista suspensa.

1. No _Estágios_ e _Produção_ mantenha as configurações padrão. No entanto, é recomendável revisar e alterar as configurações para a configuração de produção real, especificamente o _Carregar biblioteca de forma assíncrona_ alternar com base nos requisitos de desempenho e otimização. Observe também que a _URI da biblioteca_ O valor de é diferente para Preparo e Produção.

1. Por fim, clique em **Criar** para concluir os serviços em nuvem do Launch.

   ![Iniciar configuração do Cloud Service](assets/launch-cloud-services-config.png)

## Aplicar o Launch Cloud Service ao site

Para carregar a propriedade Tag e suas bibliotecas no site AEM, a configuração do Launch Cloud Service é aplicada ao site. Na etapa anterior, a configuração do serviço de nuvem é criada na pasta de nome do site (Site WKND), para que seja aplicada automaticamente, vamos verificá-la.

1. No **Navegação** selecione **Sites** ícone.

1. Selecione a página raiz do site AEM e clique em **Propriedades**. Em seguida, navegue até o **Avançado** guia e abaixo **Configuração** verifique se o valor da Configuração na nuvem aponta para a configuração específica do site `conf` pasta.

   ![Aplicar configuração do Cloud Service ao site](assets/apply-cloud-services-config-to-site.png)

## Verificar o carregamento da propriedade Tag nas páginas Autor e Publicação

Agora é hora de verificar se a propriedade Tag e suas bibliotecas estão carregadas na página do site AEM.

1. Abra a página de seu site favorito na **Exibir como publicado** no console do navegador, você deverá ver a mensagem de log. É a mesma mensagem do trecho de código JavaScript da propriedade de tag Rule que é acionada quando _Biblioteca carregada (início da página)_ evento é acionado.

1. Para verificar em Publicar, primeiro publique seu **Iniciar serviço em nuvem** e abra a página do site na instância de Publicação.

   ![Marcar propriedade nas páginas Autor e Publicar](assets/tag-property-on-author-publish-pages.png)

Parabéns! Você concluiu a integração do AEM e da tag de coleta de dados que injeta o código JavaScript no site AEM sem atualizar o código do projeto AEM.

## Desafio - atualizar e publicar regra na propriedade Tag

Utilizar os ensinamentos retirados do [Criar uma propriedade de tag](./create-tag-property.md) para concluir o desafio simples, atualize a regra existente para adicionar outra instrução do console e use _Fluxo de publicação_ implantá-lo no site AEM.

## Próximas etapas

[Depuração de uma implementação de tags](debug-tags-implementation.md)
