---
title: Desenvolvimento de status de recursos no AEM Sites
description: 'As APIs de status de recursos da Adobe Experience Manager são uma estrutura que pode ser conectada para expor mensagens de status em AEM várias UIs da Web do editor. '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# Desenvolvimento de status de recursos {#developing-resource-statuses-in-aem-sites}

As APIs de status de recursos da Adobe Experience Manager são uma estrutura que pode ser conectada para expor mensagens de status em AEM várias UIs da Web do editor.

## Visão geral {#overview}

A estrutura Status do recurso para editores fornece APIs do lado do servidor e do cliente para exibir e interagir com status do editor, de maneira padrão e uniforme.

As barras de status do editor estão originalmente disponíveis nos editores Página, Fragmento de experiência e Modelo do AEM.

Exemplos de casos de uso para provedores de status de recursos personalizados são:

* Notificar autores quando uma página estiver dentro de 2 horas de ativação agendada
* Notificando os autores que uma página foi ativada nos últimos 15 minutos
* Notificando os autores que uma página foi editada dentro dos últimos 5 minutos, e por quem

![Visão geral do status do recurso do editor AEM](assets/sample-editor-resource-status-screenshot.png)

## Estrutura do provedor de status de recursos {#resource-status-provider-framework}

Ao desenvolver status de recursos personalizados, o trabalho de desenvolvimento é composto de:

1. A implementação ResourceStatusProvider, responsável por determinar se um status é obrigatório, e as informações básicas sobre o status: título, mensagem, prioridade, variante, ícone e ações disponíveis.
2. Opcionalmente, o JavaScript GraniteUI que implementa a funcionalidade de quaisquer ações disponíveis.

   ![arquitetura de status de recurso](assets/sample-editor-resource-status-application-architecture.png)

3. O recurso de status fornecido como parte dos editores Página, Fragmento de experiência e Modelo recebe um tipo por meio da propriedade &quot;[!DNL statusType]&quot; de recursos.

   * Editor de páginas: `editor`
   * Editor de fragmentos de experiência: `editor`
   * Editor de modelo: `template-editor`

4. O `statusType` do recurso de status corresponde à propriedade `CompositeStatusType` OSGi configurada `name` registrada.

   Para todas as correspondências, os tipos `CompositeStatusType's` são coletados e usados para coletar as implementações `ResourceStatusProvider` que têm esse tipo, por meio de `ResourceStatusProvider.getType()`.

5. O `ResourceStatusProvider` correspondente é transmitido ao `resource` no editor e determina se o `resource` tem o status a ser exibido. Se o status for necessário, essa implementação será responsável pela criação de 0 ou muitas `ResourceStatuses` para retornar, cada uma representando um status a ser exibido.

   Normalmente, um `ResourceStatusProvider` retorna 0 ou 1 `ResourceStatus` por `resource`.

6. ResourceStatus é uma interface que pode ser implementada pelo cliente, ou o `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` útil pode ser usado para construir um status. Um status é composto de:

   * Título
   * Mensagem
   * Ícone
   * Variante
   * Prioridade
   * Ações
   * Dados

7. Opcionalmente, se `Actions` for fornecido para o objeto `ResourceStatus`, será necessário oferecer suporte a clientlibs para vincular a funcionalidade aos links de ação na barra de status.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. Qualquer JavaScript ou CSS de suporte para as ações deve ser enviado por proxy pelas respectivas bibliotecas de clientes de cada editor para garantir que o código front-end esteja disponível no editor.

   * Categoria do editor de páginas: `cq.authoring.editor.sites.page`
   * Categoria do editor de fragmento de experiência: `cq.authoring.editor.sites.page`
   * Categoria do editor de modelos: `cq.authoring.editor.sites.template`

## Visualização do código {#view-the-code}

[Consulte o código no GitHub](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## Recursos adicionais {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
