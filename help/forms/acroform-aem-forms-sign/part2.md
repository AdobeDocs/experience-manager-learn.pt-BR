---
title: Formas com o AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Parte 2 da integração do Acroforms com o AEM Forms. Crie um esquema a partir de um Acrobat.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 2%

---


# Criar esquema a partir do acroformulário

A próxima etapa é criar um schema a partir da acroforma criada na etapa anterior. Um aplicativo de amostra é fornecido para criar o esquema como parte deste tutorial. Para criar o schema, siga as seguintes instruções:

1. Fazer logon em [CRXDE Lite](http://localhost:4502/crx/de)
2. Abrir no arquivo `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Altere o `saveLocation` para uma pasta apropriada no disco rígido. Verifique se a pasta em que você está salvando já foi criada.
4. Aponte seu navegador para [Criar XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) página hospedada no AEM.
5. Arraste e solte a acroforma.
6. Verifique a pasta especificada na Etapa 3. O arquivo de esquema é salvo neste local.

## Fazer upload da acroforma

Para que esta demonstração funcione em seu sistema, será necessário criar uma pasta chamada `acroforms` no AEM Assets. Carregue o acroforma nesta `acroforms` pasta.

>[!NOTE]
>
>O exemplo de código procura o acroformulário nesta pasta. O acroformulário é necessário para mesclar os dados enviados do formulário adaptável.
