---
title: Pontos de interrupção responsivos
description: Saiba como configurar novos pontos de interrupção responsivos para AEM Editor de página responsivo.
version: Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
kt: 11664
thumbnail: kt-11664.jpeg
source-git-commit: c82965636ddeef7dc165e0bea079c99f1a16e0ca
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---


# Pontos de interrupção responsivos

Saiba como configurar novos pontos de interrupção responsivos para AEM Editor de página responsivo.

## Criar pontos de interrupção de CSS

Primeiro, crie pontos de interrupção de mídia no CSS AEM Responsive Grid ao qual o site de AEM responsivo adere.

Em `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` crie seus pontos de interrupção a serem usados junto com o emulador móvel. Anote o `max-width` para cada ponto de interrupção, já que isso mapeia os pontos de interrupção de CSS para os pontos de interrupção AEM do Editor de página responsivo.

![Criar novos pontos de interrupção responsivos](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## Personalizar os pontos de interrupção do modelo

Abra o `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` arquivo e atualização `cq:responsive/breakpoints` com suas novas definições de nó de ponto de interrupção. Cada [Ponto de interrupção CSS](#create-new-css-breakpoints) deve ter um nó correspondente em `breakpoints` com `width` propriedade definida para o ponto de interrupção CSS `max-width`.

![Personalizar os pontos de interrupção responsivos do modelo](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## Criar emuladores

AEM emuladores devem ser definidos que permitam aos autores selecionar a exibição responsiva a ser editada no Editor de páginas.

Criar nós de emuladores em `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

Por exemplo, `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`. Copiar um nó do emulador de referência de `/libs/wcm/mobile/components/emulators` no CRXDE Lite para e atualize a cópia para agilizar a definição do nó.

![Criar novos emuladores](./assets/responsive-breakpoints/create-new-emulators.jpg)

## Criar grupo de dispositivos

Agrupe os emuladores em [disponibilizá-los no Editor de páginas AEM](#update-the-templates-device-group).

Criar `/apps/settings/mobile/groups/<name of device group>` estrutura do nó em `/ui.apps/src/main/content/jcr_root`.

![Criar novo grupo de dispositivos](./assets/responsive-breakpoints/create-new-device-group.jpg)

Crie um `.content.xml` no arquivo `/apps/settings/mobile/groups/<device group name>` e defina os novos emuladores usando um código semelhante ao abaixo:

![Criar novo dispositivo](./assets/responsive-breakpoints/create-new-device.jpg)

## Atualizar o grupo de dispositivos do modelo

Por fim, mapeie o grupo de dispositivos de volta ao modelo de página para que os emuladores estejam disponíveis no Editor de páginas para páginas criadas a partir desse modelo.

Abra o `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` e atualize o `cq:deviceGroups` propriedade para fazer referência ao novo grupo móvel (por exemplo, `cq:deviceGroups="[mobile/groups/customdevices]"`)
