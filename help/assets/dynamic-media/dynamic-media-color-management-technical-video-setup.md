---
title: Como entender o gerenciamento de cores com AEM Dynamic Media
seo-title: Como entender o gerenciamento de cores com AEM Dynamic Media
description: Neste vídeo, exploramos o Gerenciamento dinâmico de cores do Media e como ele pode ser usado para fornecer recursos de pré-visualização de correção de cores no AEM Assets.
seo-description: Neste vídeo, exploramos o Gerenciamento dinâmico de cores do Media e como ele pode ser usado para fornecer recursos de pré-visualização de correção de cores no AEM Assets.
uuid: dc14d067-11a2-4662-acfd-f9f6f1d738ee
discoiquuid: b2b9ccc9-96b5-4bea-9995-2e6b353c469d
sub-product: dynamic-media
feature: image-profiles, video-profiles
topics: images, videos, renditions, authoring, integrations, publishing, metadata
audience: developer, architect, administrator
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 13%

---


# Como entender o gerenciamento de cores com AEM Dynamic Media{#understanding-color-management-with-aem-dynamic-media}

Neste vídeo, exploramos o Gerenciamento dinâmico de cores do Media e como ele pode ser usado para fornecer recursos de pré-visualização de correção de cores no AEM Assets.

>[!VIDEO](https://video.tv.adobe.com/v/16792/?quality=9&learn=on)

>[!NOTE]
>
>[Ative o Dynamic Media](https://docs.adobe.com/docs/en/aem/6-0/administer/integration/dynamic-media/enabling-dynamic-media.html) no AEM para usar esse recurso.

Este recurso está disponível para as versões AEM 6.1 e 6.2 como um Feature Pack.

## Modelo XML para o nó de configuração Gerenciamento de cores {#xml-template-for-the-color-management-configuration-node}

A seguir está o modelo XML do nó de configuração Gerenciamento de cores. Este modelo XML pode ser copiado no projeto de desenvolvimento AEM e configurado com as configurações apropriadas ao projeto.

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--
    XML Node definition for: /etc/dam/imageserver/configuration/jcr:content/settings

 Adobe Docs

 * Image Server Configuration: https://docs.adobe.com/docs/en/aem/6-2/administer/content/dynamic-media/config-dynamic.html#Configuring%20Dynamic%20Media%20Image%20Settings

* Default Color Profile Configuration: https://docs.adobe.com/docs/en/aem/6-1/administer/content/dynamic-media/config-dynamic.html#Configuring%20the%20default%20color%20profiles

    iccprofileXXX values:
        Node name of color profile found at: /etc/dam/imageserver/profiles

    iccblackpointcompensation values:
        true | false

    iccdither values:
        true | false

    iccrenderintent values:
        0 for perceptual
        1 for relative colorimetric
        2 for saturation
        3 for absolute colorimetric

-->

<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
    xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:primaryType="nt:unstructured"

        bkgcolor="FFFFFF"
        defaultpix="300,300"
        defaultthumbpix="100,100"
        expiration="{Long}36000000"
        jpegquality="80"
        maxpix="2000,2000"
        resmode="SHARP2"
        resolution="72"
        thumbnailtime="[1%,11%,21%,31%,41%,51%,61%,71%,81%,91%]"
        iccprofilergb=""
        iccprofilecmyk=""
        iccprofilegray=""
        iccprofilesrcrgb=""
        iccprofilesrccmyk=""
        iccprofilesrcgray=""
        iccblackpointcompensation="{Boolean}true"
        iccdither="{Boolean}false"
        iccrenderintent="{Long}0"
/>
```

### Lista dos perfis de cor de Adobe estão listados abaixo {#list-of-default-adobe-color-profiles-are-listed-below}

| Nome | Espaço de cor | Descrição |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB (1998) |
| AppleRGB | RGB | Apple RGB |
| CIERGB | RGB | RGB CIE |
| CoatedFogra27 | CMYK | Revestido FOGRA27 (ISO 12647-2:2004) |
| CoatedFogra39 | CMYK | Revestido FOGRA39 (ISO 12647-2:2004) |
| CoatedGraCol | CMYK | Revestido GRACoL 2006 (ISO 12647-2:2004) |
| ColorMatchRGB | RGB | ColorMatch RGB |
| EuropeISOCoated | CMYK | Europa ISO Revestido FOGRA27 |
| EuroscaleCoated | CMYK | Revestimento Euroscale v2 |
| EuroscaleUncovered | CMYK | Euroscale Uncovered v2 |
| JapanColorCoated | CMYK | Japan Color 2001 Coated |
| JapanColorNewspaper | CMYK | Jornal Japan Color 2002 |
| JapanColorUncovered | CMYK | Japão - Cor 2001 sem revestimento |
| JapanColorWebCoated | CMYK | Japan Color 2003 Web Coated |
| JapanWebCoated | CMYK | Japan Web Coated (Anúncio) |
| NewsprintSNAP2007 | CMYK | Jornal dos EUA (SNAP 2007) |
| NTSC | RGB | NTSC (1953) |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhoto RGB |
| PS4Default | CMYK | CMYK padrão Photoshop 4 |
| PS5Default | CMYK | CMYK padrão Photoshop 5 |
| SheetfeedCoated | CMYK | U.S. Sheetfeed Coated v2 |
| SheetfeedNãoRevelado | CMYK | U.S. Sheetfeed UnRevelado v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGB sRGB | IEC61966-2.1 |
| UncoatedFogra29 | CMYK | FOGRA29 não revestida (ISO 12647-2:2004) |
| WebCoated | CMYK | U.S. Web Coated (SWOP) v2 |
| WebCoatedFogra28 | CMYK | Revestido pela Web FOGRA28 (ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | Papel SWOP 2006 Grau 3 Revestido pela Web |
| WebCoatedGrade5 | CMYK | Papel SWOP 2006 Grau 5 Revestido pela Web |
| WebUncovered | CMYK | U.S. Web Uncovered v2 |
| WideGamutRGB | RGB | Gamut amplo RGB |

## Recursos adicionais{#additional-resources}

* [Configuração do Gerenciamento dinâmico de cores do Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
