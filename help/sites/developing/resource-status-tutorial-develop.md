---
title: Desenvolvimento de status de recursos no AEM Sites
description: 'As APIs de status de recurso do Adobe Experience Manager são uma estrutura que pode ser conectada para expor mensagens de status AEM várias interfaces do usuário da Web do editor. '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.4, 6.5
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# Desenvolvimento de status de recursos {#developing-resource-statuses-in-aem-sites}

As APIs de status de recurso do Adobe Experience Manager são uma estrutura que pode ser conectada para expor mensagens de status AEM várias interfaces do usuário da Web do editor.

## Visão geral {#overview}

A estrutura Status do recurso para editores fornece APIs do lado do servidor e do lado do cliente para exibir e interagir com os status do editor, de maneira padrão e uniforme.

As barras de status do editor estão nativamente disponíveis nos editores Página, Fragmento de experiência e Modelo do AEM.

Exemplos de casos de uso para provedores de status de recurso personalizados são:

* Notificar autores quando uma página estiver dentro de 2 horas da ativação agendada
* Notificando os autores que uma página foi ativada nos últimos 15 minutos
* Notificando os autores que uma página foi editada nos últimos 5 minutos, e por quem

![Visão geral do status de recurso do editor de AEM](assets/sample-editor-resource-status-screenshot.png)

## Estrutura do provedor de status de recurso {#resource-status-provider-framework}

Ao desenvolver status de recursos personalizados, o trabalho de desenvolvimento é composto de:

1. A implementação ResourceStatusProvider, que é responsável por determinar se um status é necessário, e as informações básicas sobre o status: título, mensagem, prioridade, variante, ícone e ações disponíveis.
2. Opcionalmente, o GraniteUI JavaScript que implementa a funcionalidade de qualquer ação disponível.

   ![arquitetura de status de recurso](assets/sample-editor-resource-status-application-architecture.png)

3. O recurso de status fornecido como parte dos editores Página, Fragmento de experiência e Modelo recebe um tipo por meio dos recursos &quot;[!DNL statusType]&quot;.

   * Editor de página: `editor`
   * Editor de fragmento de experiência: `editor`
   * Editor de modelo: `template-editor`

4. O recurso de status `statusType` corresponde a registrado `CompositeStatusType` OSGi configurado `name` propriedade.

   Para todas as correspondências, a variável `CompositeStatusType's` são coletados e usados para coletar `ResourceStatusProvider` implementações que têm esse tipo, por meio de `ResourceStatusProvider.getType()`.

5. A correspondência `ResourceStatusProvider` é transmitido, `resource` no editor, e determina se a variável `resource` tem status a ser exibido. Se o status for necessário, essa implementação será responsável pela criação de 0 ou muitos `ResourceStatuses` para retornar, cada um representando um status a ser exibido.

   Normalmente, um `ResourceStatusProvider` retorna 0 ou 1 `ResourceStatus` per `resource`.

6. ResourceStatus é uma interface que pode ser implementada pelo cliente, ou a `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` pode ser usada para criar um status. Um status é composto de:

   * Título
   * Mensagem
   * Ícone
   * Variante
   * Prioridade
   * Ações
   * Dados

7. Opcionalmente, se `Actions` sejam `ResourceStatus` , clientlibs de suporte são necessários para vincular a funcionalidade aos links de ação na barra de status.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. Qualquer JavaScript ou CSS de suporte para as ações deve ser enviado por proxy por meio das respectivas bibliotecas de clientes de cada editor para garantir que o código de front-end esteja disponível no editor.

   * Categoria do editor de páginas: `cq.authoring.editor.sites.page`
   * Categoria do editor de Fragmento de experiência: `cq.authoring.editor.sites.page`
   * Categoria do editor de modelos: `cq.authoring.editor.sites.template`

## Visualizar o código {#view-the-code}

[Consulte o código no GitHub](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## Recursos adicionais {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
