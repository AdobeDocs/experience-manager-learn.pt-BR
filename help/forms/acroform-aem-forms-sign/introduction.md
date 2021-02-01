---
title: Acrobat com AEM Forms
description: Um tutorial que percorre a criação de um formulário adaptável usando o Acroform e mesclando os dados para obter um PDF. O PDF com os dados unidos pode ser enviado para assinatura usando o Adobe Sign.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---


# Criação de Forms adaptável a partir de formulários

As organizações têm uma grande variedade de formulários. Alguns desses formulários são criados no Microsoft Word e convertidos em PDF. Por padrão, esses formulários não podem ser preenchidos usando o Adobe Reader ou o Acrobat. Para tornar esses formulários preenchíveis usando o Acrobat ou o Reader, é necessário converter esses formulários em Acroform. Os formulários são formulários criados usando o Acrobat. Este artigo aborda a criação de um formulário adaptável a partir da Acroform e mescla os dados de volta ao Acroform para obter o PDF. O PDF com os dados unidos também pode ser enviado para assinatura usando o Adobe Sign.

>[!NOTE]
>
>Se você estiver usando o AEM Forms 6.5, use o recurso Automated forms conversion.

## Pré-requisitos

* AEM Forms 6.3 ou 6.4 instalado e configurado
* Acesso ao Adobe Acrobat
* Familiaridade com AEM/AEM Forms.

### Os seguintes itens são necessários para que esse recurso funcione no sistema

* Baixe e implante os pacotes usando o [Felix Web Console](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Baixe e importe este pacote para AEM](assets/acro-form-aem-form.zip). Este pacote contém o fluxo de trabalho de amostra e a página html para criar XSD do acroform
* Abra o [configMgr](http://localhost:4502/system/console/configMgr)
   * Procure &quot;Apache Sling Service User Mapper Service&quot; e clique para abrir as propriedades
   * Clique no ícone `+` (mais) para adicionar o seguinte Mapeamento de serviços
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Clique em &#39;Salvar&#39;
