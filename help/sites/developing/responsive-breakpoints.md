---
title: Pontos de interrupção responsivos
description: Saiba como configurar novos pontos de interrupção responsivos para o Editor de página responsivo AEM.
version: Experience Manager as a Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
jira: KT-11664
thumbnail: kt-11664.jpeg
exl-id: 8b48c28f-ba7f-4255-be96-a7ce18ca208b
duration: 52
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Pontos de interrupção responsivos

Saiba como configurar novos pontos de interrupção responsivos para o Editor de página responsivo AEM.

## Criar pontos de interrupção de CSS

Primeiro, crie pontos de interrupção de mídia no CSS da Grade responsiva do AEM ao qual o site responsivo do AEM adere.

No arquivo `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less`, crie seus pontos de interrupção para serem usados junto com o emulador móvel. Anote o `max-width` para cada ponto de interrupção, pois isso mapeia os pontos de interrupção de CSS para os pontos de interrupção responsivos do Editor de página do AEM.

![Criar novos pontos de interrupção responsivos](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## Personalizar os pontos de interrupção do modelo

Abra o arquivo `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` e atualize `cq:responsive/breakpoints` com suas novas definições de nó de ponto de interrupção. Cada [ponto de interrupção de CSS](#create-new-css-breakpoints) deve ter um nó correspondente em `breakpoints` com sua propriedade `width` definida como `max-width` do ponto de interrupção de CSS.

![Personalizar os pontos de interrupção responsivos do modelo](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## Criar emuladores

Os emuladores de AEM devem ser definidos para permitir que os autores selecionem a visualização responsiva para editar no Editor de páginas.

Criar nós de emuladores em `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

Por exemplo, `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`. Copie um nó do emulador de referência de `/libs/wcm/mobile/components/emulators` no CRXDE Lite para e atualize a cópia para expedir a definição do nó.

![Criar novos emuladores](./assets/responsive-breakpoints/create-new-emulators.jpg)

## Criar grupo de dispositivos

Agrupar os emuladores para [disponibilizá-los no Editor de Páginas do AEM](#update-the-templates-device-group).

Crie a estrutura do nó `/apps/settings/mobile/groups/<name of device group>` em `/ui.apps/src/main/content/jcr_root`.

![Criar novo grupo de dispositivos](./assets/responsive-breakpoints/create-new-device-group.jpg)

Crie um arquivo `.content.xml` em `/apps/settings/mobile/groups/<device group name>` e defina
os novos emuladores usando um código semelhante ao abaixo:

![Criar novo dispositivo](./assets/responsive-breakpoints/create-new-device.jpg)

## Atualizar o grupo de dispositivos do modelo

Por fim, mapeie o grupo de dispositivos de volta ao modelo de página para que os emuladores estejam disponíveis no Editor de páginas para páginas criadas com base nesse modelo.

Abra o arquivo `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` e atualize a propriedade `cq:deviceGroups` para fazer referência ao novo grupo móvel (por exemplo, `cq:deviceGroups="[mobile/groups/customdevices]"`)
