---
title: Criar uma configuração do Launch Cloud Service no AEM
description: Saiba como criar uma configuração do Launch Cloud Service no AEM. A configuração do Cloud Service do Launch pode ser aplicada a um site existente, e as bibliotecas de tags podem ser observadas no carregamento dos ambientes do Author e Publish.
topics: integrations
audience: administrator
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5982
thumbnail: 38566.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
source-git-commit: 2b37ba961e194b47e034963ceff63a0b8e8458ae
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---

# Criar uma configuração do Launch Cloud Service no AEM {#create-launch-cloud-service}

>[!NOTE]
>
>O processo de renomear o Adobe Experience Platform Launch como um conjunto de tecnologias de coleta de dados está sendo implementado na interface do usuário, no conteúdo e na documentação do produto AEM, portanto, o termo Launch ainda está sendo usado aqui.

Saiba como criar uma configuração do Launch Cloud Service no Adobe Experience Manager. AEM configuração do Launch Cloud Service pode ser aplicada a um site existente e as bibliotecas de tags podem ser observadas no carregamento nos ambientes de Autor e Publicação.

## Criar o serviço em nuvem do Launch

Crie a configuração do serviço de nuvem do Launch usando as etapas abaixo.

1. No **Ferramentas** selecione **Cloud Services** e clique em **Configurações do Adobe Launch**

1. Selecione a pasta de configuração do site ou selecione **Site WKND** (se estiver usando um projeto de guia WKND) e clique em **Criar**

1. No _Geral_ nomeie a configuração usando o **Título** e selecione **Adobe Launch** do _Configuração associada do Adobe IMS_ lista suspensa. Em seguida, selecione o nome da sua empresa no _Empresa_ e selecione a propriedade criada anteriormente no menu _Propriedade_ lista suspensa.

1. No _Estágios_ e _Produção_ mantenha as configurações padrão. No entanto, é recomendável revisar e alterar as configurações para a configuração real de produção, especificamente o _Carregar biblioteca de forma assíncrona_ alternar com base em seus requisitos de desempenho e otimização. Observe também que a variável _URI da biblioteca_ é diferente para Preparo e Produção.

1. Finalmente, clique em **Criar** para concluir os serviços em nuvem do Launch.

   ![Iniciar configuração Cloud Services](assets/launch-cloud-services-config.png)

## Aplicar o serviço em nuvem do Launch ao site

Para carregar a propriedade Tag e suas bibliotecas no site AEM, a configuração do serviço de nuvem do Launch é aplicada ao site. Na etapa anterior, a configuração do serviço de nuvem é criada na pasta de nome do site (Site WKND) para que seja aplicada automaticamente, vamos verificá-la.

1. No **Navegação** selecione **Sites** ícone .

1. Selecione a página raiz do Site de AEM e clique em **Propriedades**. Em seguida, navegue até o **Avançado** e em **Configuração** verifique se o valor da Configuração da nuvem aponta para o site específico `conf` pasta.

   ![Aplicar configuração do Cloud Services ao site](assets/apply-cloud-services-config-to-site.png)

## Verificar o carregamento da propriedade Tag nas páginas Autor e Publicação

Agora é hora de verificar se a propriedade Tag e suas bibliotecas estão carregadas na página do site AEM.

1. Abra sua página favorita no **Exibir como publicado** no console do navegador, você deverá ver a mensagem de log. É a mesma mensagem do snippet do código JavaScript da regra da propriedade de tag que é acionada quando _Biblioteca carregada (início da página)_ for acionado.

1. Para verificar em Publicar, publique primeiro seu **Iniciar o serviço em nuvem** e abra a página do site na instância de publicação.

   ![Propriedade de tag em páginas de autor e publicação](assets/tag-property-on-author-publish-pages.png)

Parabéns! Você concluiu a integração da Tag AEM e de coleta de dados que injeta o código JavaScript no seu site AEM sem atualizar o código AEM do projeto.

## Desafio - atualizar e publicar regra na propriedade de tag

Usar lições aprendidas com as anteriores [Criar uma propriedade de tag](./create-tag-property.md) para concluir o desafio simples, atualize a Regra existente para adicionar uma declaração de console adicional e use _Fluxo de publicação_ implante-o no site AEM.

## Próximas etapas

[Depuração de uma implementação de Tags](debug-tags-implementation.md)
