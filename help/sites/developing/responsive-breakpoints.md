---
title: Pontos de interrupção responsivos
description: Saiba como configurar novos pontos de interrupção responsivos para o Editor de página responsivo para AEM.
version: Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
jira: KT-11664
thumbnail: kt-11664.jpeg
exl-id: 8b48c28f-ba7f-4255-be96-a7ce18ca208b
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Pontos de interrupção responsivos

Saiba como configurar novos pontos de interrupção responsivos para o Editor de página responsivo para AEM.

## Criar pontos de interrupção de CSS

Primeiro, crie pontos de interrupção de mídia no CSS da Grade responsiva AEM que o site AEM responsivo segue.

Entrada `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` crie seus pontos de interrupção para serem usados junto com o emulador móvel. Anote o `max-width` para cada ponto de interrupção, pois isso mapeia os pontos de interrupção de CSS para os pontos de interrupção responsivos do Editor de páginas do AEM.

![Criar novos pontos de interrupção responsivos](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## Personalizar os pontos de interrupção do modelo

Abra o `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` arquivo e atualização `cq:responsive/breakpoints` com as novas definições de nó do ponto de interrupção. Each [Ponto de interrupção de CSS](#create-new-css-breakpoints) deve ter um nó correspondente em `breakpoints` com seu `width` definido como o do ponto de interrupção de CSS `max-width`.

![Personalizar os pontos de interrupção responsivos do modelo](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## Criar emuladores

Os emuladores de AEM devem ser definidos para permitir que os autores selecionem a visualização responsiva a ser editada no Editor de páginas.

Criar nós de emuladores em `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

Por exemplo, `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`. Copiar um nó do emulador de referência de `/libs/wcm/mobile/components/emulators` em CRXDE Lite para e atualize a definição de nó copiar para expedir.

![Criar novos emuladores](./assets/responsive-breakpoints/create-new-emulators.jpg)

## Criar grupo de dispositivos

Agrupar os emuladores em [disponibilizá-las no Editor de páginas AEM](#update-the-templates-device-group).

Criar `/apps/settings/mobile/groups/<name of device group>` estrutura do nó em `/ui.apps/src/main/content/jcr_root`.

![Criar novo grupo de dispositivos](./assets/responsive-breakpoints/create-new-device-group.jpg)

Criar um `.content.xml` arquivo em `/apps/settings/mobile/groups/<device group name>` e defina os novos emuladores usando um código semelhante ao abaixo:

![Criar novo dispositivo](./assets/responsive-breakpoints/create-new-device.jpg)

## Atualizar o grupo de dispositivos do modelo

Por fim, mapeie o grupo de dispositivos de volta ao modelo de página para que os emuladores estejam disponíveis no Editor de páginas para páginas criadas com base nesse modelo.

Abra o `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` arquivo e atualize o `cq:deviceGroups` para fazer referência ao novo grupo móvel (por exemplo, `cq:deviceGroups="[mobile/groups/customdevices]"`)
