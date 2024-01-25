---
title: Noções básicas sobre gerenciamento de cores com o AEM Dynamic Media
description: Neste vídeo, exploramos o Dynamic Media Color Management e como ele pode ser usado para fornecer recursos de visualização da correção de cores no para AEM Assets.
feature: Image Profiles, Video Profiles
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Feature Video
exl-id: a733532b-db64-43f6-bc43-f7d422d5071a
duration: 302
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 11%

---

# Noções básicas sobre gerenciamento de cores com o AEM Dynamic Media{#understanding-color-management-with-aem-dynamic-media}

Neste vídeo, exploramos o Dynamic Media Color Management e como ele pode ser usado para fornecer recursos de visualização da correção de cores no para AEM Assets.

>[!VIDEO](https://video.tv.adobe.com/v/16792?quality=12&learn=on)

>[!NOTE]
>
>[Ativar o Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=pt-BR) no AEM para usar esse recurso.

Este recurso está disponível para as versões AEM 6.1 e 6.2 como um Pacote de recursos.

## Modelo XML do nó de configuração de gerenciamento de cores {#xml-template-for-the-color-management-configuration-node}

Este é o modelo XML para o nó de configuração do gerenciamento de cores. Esse modelo XML pode ser copiado para o projeto de desenvolvimento AEM e configurado com as configurações apropriadas para o projeto.

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

### A lista de perfis de cores de Adobe padrão está listada abaixo {#list-of-default-adobe-color-profiles-are-listed-below}

| Nome | Espaço de cor | Descrição |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB (1998) |
| AppleRGB | RGB | RGB Apple |
| CIERGB | RGB | RGB CIE |
| FograRevestida27 | CMYK | FOGRA27 revestida (ISO 12647-2:2004) |
| FograRevestida39 | CMYK | FOGRA39 revestida (ISO 12647-2:2004) |
| GraCol revestido | CMYK | GRACoL 2006 revestido (ISO 12647-2:2004) |
| ColorMatchRGB | RGB | ColorMatch RGB |
| EuropeISOCoated | CMYK | Europa ISO Revestido FOGRA27 |
| EuroscaleCoated | CMYK | Euroscale Coated v2 |
| EuroscaleNão revestido | CMYK | Euroscale não revestido v2 |
| JapãoRevestidoPorCor | CMYK | Japão Colorido 2001 Revestido |
| JapãoColorNewspaper | CMYK | Jornal Japan Color 2002 |
| JapãoCorNãoRevestida | CMYK | Japão Colorido 2001 Sem Revestimento |
| JapãoCorRevestidoWeb | CMYK | Japan Color 2003 Web Coated |
| JapãoRevestidoWeb | CMYK | Revestimento Para Web No Japão (Anúncio) |
| Papel de jornalSNAP2007 | CMYK | Papel de jornal dos EUA (SNAP 2007) |
| NTSC | RGB | NTSC (1953) |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | RGB ProPhoto |
| PS4Default | CMYK | Photoshop 4 Padrão CMYK |
| PS5Padrão | CMYK | CMYK padrão do Photoshop 5 |
| RevestidoFolheado | CMYK | U.S. Sheetfed Coated v2 |
| Não revestidaFolha | CMYK | U.S. Sheetfed sem revestimento v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGB sRGB | IEC61966-2.1 |
| Fogra29 não revestida | CMYK | FOGRA29 não revestida (ISO 12647-2:2004) |
| Revestido pela Web | CMYK | U.S. Web Coated (SWOP) v2 |
| WebCoatedFogra28 | CMYK | FOGRA28 revestida com revestimento web (ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | Papel revestido para web SWOP 2006 Grade 3 |
| WebCoatedGrade5 | CMYK | Papel revestido para web SWOP 2006 Grade 5 |
| WebNão revestida | CMYK | Web sem revestimento dos EUA v2 |
| WideGamutRGB | RGB | RGB de gamut amplo |

## Recursos adicionais{#additional-resources}

* [Configuração do gerenciamento de cores do Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
