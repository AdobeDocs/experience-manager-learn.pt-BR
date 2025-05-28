---
title: Dynamic Media para transparência e processamento em lote de Automação de conteúdo
description: Saiba como usar o Dynamic Media no AEM para criar representações virtuais, gerenciar a transparência e automatizar o processamento de imagens para reutilização de conteúdo escalável.
feature: Image Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner, Intermediate, Experienced
doc-type: Feature Video
duration: 542
last-substantial-update: 2025-05-28T00:00:00Z
jira: KT-18197
source-git-commit: 2ffe4706856f0dbf63f2916af010f23bdb7b0045
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 5%

---


# Dynamic Media para transparência e processamento em lote de Automação de conteúdo

Saiba como usar o Dynamic Media no AEM para criar representações virtuais, gerenciar a transparência e automatizar o processamento de imagens para reutilização de conteúdo escalável.

>[!VIDEO](https://video.tv.adobe.com/v/3459589/?learn=on&enablevpops)


## Exemplo de ativos do Dynamic Media

A seguir estão exemplos de ativos do Dynamic Media e seus URLs usados no vídeo.

>[!BEGINTABS]

>[!TAB Exemplos de transparência de imagem]

A seguir estão os URLs de amostra do Servidor de imagens do Dynamic Media usados no vídeo.

| Visualização | Descrição | URL do Dynamic Media |
|-----------|------------------|---------|
| ![Padrão](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255){width="250"} | Padrão | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255) |
| ![Composto com camada de imagem de fundo contínua](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | Composto com camada de imagem de plano de fundo contínua | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![Plano de fundo vermelho](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | Plano de fundo vermelho | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![Caminho recortado para oval](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255){width="250"} | Recortado para caminho oval | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255) |


>[!TAB Exemplos de caminho de imagem]

A seguir estão os URLs de amostra do Servidor de imagens do Dynamic Media usados no vídeo.

| Visualização | Descrição | URL do Dynamic Media |
|-----------|------------------|---------|
| ![Normalizado para 80 pixels de largura (sem transparência)](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800){width="250"} | Normalizado para 80 pixels de largura (sem transparência){width="250"} | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800) |
| ![Cortar para caminho](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800){width="250"} | Cortar no caminho | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800) |
| ![Recortar para Caminho](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800){width="250"} | Recortar para caminho | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800) |
| ![Recortar para Caminho e Recortar para caminho](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800){width="250"} | Recortar para caminho e Recortar para caminho | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800) |
| ![Recortar para outro caminho](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800){width="250"} | Recortar para outro caminho | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800) |
| ![Recortar para outro caminho e criar um plano de fundo vermelho](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800){width="250"} | Recorte para outro caminho e torne o plano de fundo vermelho | [Link](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800) |

>[!ENDTABS]


## APIs do servidor de imagem do Dynamic Media

* [API de disponibilização e renderização de imagens do Dynamic Media](https://experienceleague.adobe.com/en/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/c-http-protocol-reference)
* [Visualização do Dynamic Media Snapshot](https://snapshot.scene7.com/)