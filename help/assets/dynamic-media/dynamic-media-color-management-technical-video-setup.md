---
title: Como entender o gerenciamento de cores com o AEM Dynamic Media
description: Neste vídeo, exploramos o Gerenciamento de cores da Dynamic Media e como ele pode ser usado para fornecer recursos de visualização de correção de cores no para AEM Assets.
sub-product: dynamic-media
feature: Perfis de imagem, Perfis de vídeo
version: 6.3, 6.4, 6.5
topic: Gerenciamento de conteúdo
role: Developer
level: Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 15%

---


# Como entender o gerenciamento de cores com o AEM Dynamic Media{#understanding-color-management-with-aem-dynamic-media}

Neste vídeo, exploramos o Gerenciamento de cores da Dynamic Media e como ele pode ser usado para fornecer recursos de visualização de correção de cores no para AEM Assets.

>[!VIDEO](https://video.tv.adobe.com/v/16792/?quality=9&learn=on)

>[!NOTE]
>
>[Ative o Dynamic ](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html) Media AEM para usar esse recurso.

Esse recurso está disponível para as versões AEM 6.1 e 6.2 como um Feature Pack.

## Modelo XML para o nó de configuração do Gerenciamento de cores {#xml-template-for-the-color-management-configuration-node}

Este é o modelo XML para o nó de configuração do Gerenciamento de cores. Esse modelo XML pode ser copiado no projeto de desenvolvimento de AEM e configurado com as configurações apropriadas ao projeto.

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

### A lista de perfis de cores do Adobe padrão está listada abaixo {#list-of-default-adobe-color-profiles-are-listed-below}

| Nome | Espaço de cor | Descrição |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB (1998) |
| AppleRGB | RGB | Apple RGB |
| CIERGB | RGB | CIE RGB |
| CoatedFogra27 | CMYK | Revestido FOGRA27 (ISO 12647-2:2004) |
| CoatedFogra39 | CMYK | Revestido FOGRA39 (ISO 12647-2:2004) |
| ColunaRevestida | CMYK | GRACoL 2006 revestido (ISO 12647-2:2004) |
| ColorMatchRGB | RGB | ColorMatch RGB |
| EuropeISOCoated | CMYK | Europa ISO Revestido FOGRA27 |
| EuroscaleCoated | CMYK | Euroscale Coated v2 |
| EuroescalaNãoRevestido | CMYK | Euroscale Não Revestido v2 |
| JapãoColorCoated | CMYK | Japão - Cor 2001 Revestida |
| JapãoCorJornal | CMYK | Jornal japonês Color 2002 |
| JapãoCorNãoRevestida | CMYK | Japão - Cor 2001 não revestida |
| JapãoColorWebCoated | CMYK | Japão Color 2003 Web Coated |
| JapãoWebCoated | CMYK | Web Coated (Anúncio) para o Japão |
| NewsprintSNAP2007 | CMYK | Jornal dos EUA (SNAP 2007) |
| NTSC | RGB | NTSC (1953) |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhoto RGB |
| PS4Default | CMYK | CMYK padrão do Photoshop 4 |
| PS5Default | CMYK | CMYK padrão do Photoshop 5 |
| CheifadoRevestido | CMYK | U.S. Sheetfeed Coated v2 |
| PlacaSemRevestimento | CMYK | U.S.A., Placa Não Revestida v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGB sRGB | IEC61966-2.1 |
| Fogra29 Não Revestido | CMYK | FOGRA29 não revestida (ISO 12647-2:2004) |
| WebCoated | CMYK | US Web Coated (SWOP) v2 |
| WebCoatedFogra28 | CMYK | Web Coated FOGRA28 (ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | Papel SWOP Revestido da Web 2006 Grau 3 |
| WebCoatedGrade5 | CMYK | Papel SWOP Revestido da Web 2006 Grau 5 |
| WebUn-Revelado | CMYK | U.S. Web Untrated v2 |
| WideGamutRGB | RGB | Largura de gama RGB |

## Recursos adicionais{#additional-resources}

* [Configuração do gerenciamento de cores Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
