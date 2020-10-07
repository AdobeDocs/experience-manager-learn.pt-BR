---
title: Relatórios de ativos no AEM Assets
description: 'A AEM Assets fornece uma estrutura de relatórios de nível corporativo que é dimensionada para repositórios grandes por meio de uma experiência intuitiva do usuário. '
feature: reports
topics: authoring, operations, performance, metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
kt: 648
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 2%

---


# Relatórios de ativos{#using-reports-in-aem-assets}

A AEM Assets fornece uma estrutura de relatórios de nível corporativo que é dimensionada para repositórios grandes por meio de uma experiência intuitiva do usuário.

>[!VIDEO](https://video.tv.adobe.com/v/22140/?quality=12&learn=on)

## Fórmulas do Microsoft Excel {#excel-formulas}

As fórmulas a seguir são usadas no vídeo para gerar o gráfico Ativos por tamanho no Microsoft Excel.

### Normalização do tamanho do ativo em bytes {#asset-size-normalization-to-bytes}

```
=IF(RIGHT(D2,2)="KB",
      LEFT(D2,(LEN(D2)-2))*1024,
  IF(RIGHT(D2,2)="MB",
      LEFT(D2,(LEN(D2)-2))*1024*1024,
  IF(RIGHT(D2,2)="GB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024,
  IF(RIGHT(D2,2)="TB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024*1024, 0))))
```

### Contagem de ativos por tamanho {#asset-count-by-size}

#### Menos de 200 KB {#less-than-kb}

```
=COUNTIFS(E2:E1000,"< 200000")
```

#### 200 KB a 500 KB {#kb-to-kb}

```
=COUNTIFS(E2:E1000,">= 200000", E2:E1000,"<= 500000")
```

#### Mais de 500 KB {#greater-than-kb}

```
=COUNTIFS(E2:E1000,"> 500000")
```

## Recursos adicionais{#additional-resources}

Baixar [todos os arquivos Excel de ativos com gráfico](./assets/asset-reports/all-assets.xlsx)
