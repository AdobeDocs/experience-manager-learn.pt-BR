---
title: Formas com o AEM Forms
description: Um tutorial que aborda a criação de um formulário adaptável usando o Acroform e a mesclagem de dados para obter um PDF. O PDF com os dados mesclados pode ser enviado para assinatura usando o Acrobat Sign.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 52
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---


# Criação de Forms adaptável a partir do Acroforms

As organizações têm uma grande variedade de formas. Alguns desses formulários são criados no Microsoft Word e convertidos em PDF. Esses formulários, por padrão, não podem ser preenchidos usando o Adobe Reader ou o Acrobat. Para tornar esses formulários preenchíveis usando Acrobat ou Reader, precisamos convertê-los em Acroform. Acroforms são formulários criados usando o Acrobat. Este artigo aborda a criação de um Formulário adaptável do Acrobat e a mesclagem de dados no Acrobat para obter o PDF. O PDF com os dados mesclados também pode ser enviado para assinatura usando o Acrobat Sign.

>[!NOTE]
>
>Se você estiver usando o AEM Forms 6.5, use o recurso de Automated forms conversion.

## Pré-requisitos

* AEM Forms 6.3 ou 6.4 instalado e configurado
* Acesso ao Adobe Acrobat
* Familiaridade com AEM/AEM Forms.

### Os seguintes itens são necessários para que esse recurso funcione no seu sistema

* Baixe e implante os pacotes usando o [Felix Web Console](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Baixar e importar este pacote para AEM](assets/acro-form-aem-form.zip). Este pacote contém o fluxo de trabalho de amostra e a página html para criar XSD a partir de acroform
* Abra o [configMgr](http://localhost:4502/system/console/configMgr)
   * Pesquise por &#39;Apache Sling Service User Mapper Service&#39; e clique em para abrir as propriedades
   * Clique em `+` ícone (mais) para adicionar o seguinte Service Mapping
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Clique em &#39;Salvar&#39;
