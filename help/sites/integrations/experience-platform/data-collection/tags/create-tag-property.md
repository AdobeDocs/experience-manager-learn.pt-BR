---
title: Criar uma propriedade de tag
description: Saiba como criar uma propriedade de tag com a configuração mínima para integração com o AEM. Os usuários são introduzidos na interface do usuário da tag e sabem mais sobre extensões, regras e fluxos de trabalho de publicação.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5980
thumbnail: 38553.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
duration: 606
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# Criar uma propriedade de tag {#create-tag-property}

Saiba como criar uma propriedade de tag com a configuração mínima para integrar com o Adobe Experience Manager. Os usuários são introduzidos na interface do usuário da tag e sabem mais sobre extensões, regras e fluxos de trabalho de publicação.

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## Criação de propriedade de tag

Para criar uma propriedade de tag, conclua as etapas a seguir.

1. No navegador, navegue até a página [Adobe Experience Cloud Home](https://experience.adobe.com/) e faça logon usando sua Adobe ID.

1. Clique no aplicativo **Coleção de dados** na seção _Acesso rápido_ da página inicial do Adobe Experience Cloud.

1. Clique no item de menu **Marcas** da navegação à esquerda e clique em **Nova propriedade** no canto superior direito.

1. Nomeie sua propriedade de Marca usando o campo obrigatório **Nome**. No campo Domínios, digite seu nome de domínio ou, se estiver usando o ambiente do AEM as a Cloud Service, digite `adobeaemcloud.com` e clique em **Salvar**.

   ![Propriedades da Marca](assets/tag-properties.png)

## Criar uma nova regra

Abra a propriedade de Marca recém-criada clicando no seu nome na exibição **Propriedades da Marca**. Além disso, no cabeçalho _Minha atividade recente_, você deve ver que a extensão principal foi adicionada a ela. A extensão de tag principal é a extensão padrão e fornece tipos de evento fundamentais, como carregamento de página, navegador, formulário e outros tipos de evento. Consulte a [Visão geral da extensão principal](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html?lang=pt-BR) para obter mais informações.

As regras permitem especificar o que deve acontecer quando o visitante interagir com o site AEM. Para simplificar, vamos registrar duas mensagens no console do navegador para demonstrar como a integração de tags de coleta de dados pode injetar código do JavaScript no site do AEM sem atualizar o código do projeto AEM.

Para criar uma regra, conclua as etapas a seguir.

1. Clique em **Regras** da seção _CRIAÇÃO_ da navegação à esquerda e clique em **Criar nova regra**

1. Nomeie sua regra usando o campo obrigatório **Nome**.

1. Clique em **Adicionar** na seção _EVENTOS_ e, no formulário _Configuração de Evento_, na lista suspensa **Tipo de Evento**, selecione a opção _Biblioteca Carregada (Parte Superior da Página)_ e clique em **Manter Alterações**.

1. Clique em **Adicionar** da seção _AÇÕES_ e, no formulário _Configuração de Ação_, na lista suspensa **Tipo de Ação**, selecione a opção _Código Personalizado_ e clique em **Abrir Editor**.

1. No modal _Editar Código_, insira o seguinte trecho de código do JavaScript, clique em **Salvar** e, por fim, clique em **Manter Alterações**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. Clique em **Salvar** para concluir o processo de criação da regra.

   ![Nova regra](assets/new-rule.png)

## Adicionar biblioteca e publicá-la

A propriedade da Marca _Rules_ é ativada usando uma biblioteca. Pense na biblioteca como um pacote que contém código JavaScript. Ative a regra recém-criada seguindo as etapas.

1. Clique em **Fluxo de Publicação** na seção _PUBLICAÇÃO_ da navegação à esquerda e em **Adicionar Biblioteca**

1. Nomeie sua biblioteca usando o campo **Nome** e selecione a opção _Desenvolvimento(desenvolvimento)_ para a lista suspensa **Ambiente**.

1. Para selecionar todos os recursos alterados desde a criação da propriedade Tag, clique em **+ Adicionar todos os recursos alterados**. Essa ação adiciona a regra recém-criada e o recurso da extensão principal à biblioteca. Finalmente, clique em **Salvar e criar no desenvolvimento**.

1. Depois que a biblioteca for criada para a pista de natação **Desenvolvimento**, usando _elipses_, selecione **Enviar para Aprovação**

1. Em seguida, na pista de natação **Enviada** usando _elipses_, selecione a opção **Aprovar para publicação**, da mesma forma **Criar e Publish para produção** na pista de natação **Aprovada**.

![Biblioteca publicada](assets/published-library.png)


A etapa acima conclui a criação da propriedade Tag simples, que tem uma regra para registrar uma mensagem no console do navegador quando a página é carregada. Além disso, a regra e a extensão principal são publicadas ao criar uma biblioteca.

## Próximas etapas

[Conectar o AEM com a propriedade de tag usando IMS](connect-aem-tag-property-using-ims.md)


## Recursos adicionais {#additional-resources}

* [Criar uma Propriedade de Marca](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=pt-BR)
