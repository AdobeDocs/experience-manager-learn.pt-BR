---
title: Acrobora com AEM Forms
description: Um tutorial que aborda a criação de um formulário adaptável usando o Acroform e mesclando os dados para obter um PDF. O PDF com os dados unidos pode ser enviado para assinatura usando o Acrobat Sign.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 2%

---


# Criar Forms adaptável a partir de plataformas

As organizações têm uma grande variedade de formas. Alguns desses formulários são criados no Microsoft Word e convertidos em PDF. Por padrão, esses formulários não podem ser preenchidos usando o Adobe Reader ou Acrobat. Para tornar esses formulários preenchíveis usando o Acrobat ou o Reader, precisamos converter esses formulários em Acroform. Os formulários são criados usando o Acrobat. Este artigo aborda a criação de um formulário adaptável a partir da Acroform e a união dos dados de volta ao Acroform para obter o PDF. O PDF com os dados unidos também pode ser enviado para assinatura usando o Acrobat Sign.

>[!NOTE]
>
>Se você estiver usando o AEM Forms 6.5, use o recurso Automated forms conversion .

## Pré-requisitos

* AEM Forms 6.3 ou 6.4 instalado e configurado
* Acesso ao Adobe Acrobat
* Familiaridade com AEM/AEM Forms.

### Os itens a seguir são necessários para que esse recurso funcione em seu sistema

* Baixe e implante os pacotes usando o [Felix Web Console](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [Desenvolvimento comServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Baixe e importe este pacote para o AEM](assets/acro-form-aem-form.zip). Este pacote contém o fluxo de trabalho de amostra e a página html para criar o XSD do acroform
* Abra o [configMgr](http://localhost:4502/system/console/configMgr)
   * Procure por &quot;Apache Sling Service User Mapper Service&quot; e clique para abrir as propriedades
   * Clique no botão `+` ícone (mais) para adicionar o seguinte Mapeamento de serviço
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Clique em &quot;Salvar&quot;
