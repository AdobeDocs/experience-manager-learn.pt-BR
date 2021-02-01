---
title: Acrobat com AEM Forms
seo-title: Mesclar dados do formulário adaptável com o Acroform
description: Parte 2 da integração de formulários com a AEM Forms. Crie um schema a partir de uma Acroform.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 2%

---


# Criar Schema do acroform

A próxima etapa é criar um schema a partir da Acroform criada na etapa anterior. Um aplicativo de amostra é fornecido para criar o schema como parte deste tutorial. Para criar o schema, siga as seguintes instruções:

1. Faça logon em [CRXDE Lite](http://localhost:4502/crx/de)
2. Abrir no arquivo `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Altere `saveLocation` para uma pasta apropriada no disco rígido. Verifique se a pasta para a qual você está salvando já foi criada.
4. Aponte seu navegador para [Criar página XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) hospedada em AEM.
5. Arraste e solte a Acroform.
6. Verifique a pasta especificada na Etapa 3. O arquivo de schema é salvo nesse local.

## Carregar o Acroform

Para que esta demonstração funcione no seu sistema, será necessário criar uma pasta chamada `acroforms` no AEM Assets. Carregue a Acroform nesta pasta `acroforms`.

>[!NOTE]
>
>O código de amostra procura o acroform nesta pasta. O acroform é necessário para unir os dados enviados do formulário adaptável.
