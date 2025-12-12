---
title: Formas com o AEM Forms
description: Um tutorial que aborda a criação de um formulário adaptável usando o Acroform e a mesclagem de dados para obter uma PDF. A PDF com os dados mesclados pode ser enviada para assinatura usando o Acrobat Sign.
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 45
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# Criação de Forms adaptável a partir do Acroforms

As organizações têm uma grande variedade de formas. Alguns desses formulários são criados no Microsoft Word e convertidos em PDF. Esses formulários, por padrão, não podem ser preenchidos com o Adobe Reader ou o Acrobat. Para tornar esses formulários preenchíveis usando o Acrobat ou o Reader, precisamos convertê-los em formulários do Acroform. Acroforms são formulários criados usando o Acrobat. Este artigo aborda a criação de um Formulário adaptável do Acrobat e a mesclagem de dados no Acrobat para obter o PDF. A PDF com os dados mesclados também pode ser enviada para assinatura usando o Acrobat Sign.

>[!NOTE]
>
>Se você estiver usando o AEM Forms 6.5, use o recurso de Conversão automática de formulários.

## Pré-requisitos

* AEM Forms 6.3 ou 6.4 instalado e configurado
* Acesso ao Adobe Acrobat
* Familiaridade com o AEM/AEM Forms.

### Os seguintes itens são necessários para que esse recurso funcione no seu sistema

* Baixe e implante os pacotes usando o [Felix Web Console](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Baixe e importe este pacote para o AEM](assets/acro-form-aem-form.zip). Este pacote contém o fluxo de trabalho de amostra e a página html para criar XSD a partir de acroform
* Abra o [configMgr](http://localhost:4502/system/console/configMgr)
   * Pesquise por &#39;Apache Sling Service User Mapper Service&#39; e clique em para abrir as propriedades
   * Clique no ícone `+` (mais) para adicionar o seguinte Service Mapping
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Clique em &#39;Salvar&#39;
