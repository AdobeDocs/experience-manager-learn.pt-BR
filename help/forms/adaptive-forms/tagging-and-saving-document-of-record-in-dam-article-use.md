---
title: Marcação e armazenamento do AEM Forms DoR no DAM
seo-title: Marcação e armazenamento do AEM Forms DoR no DAM
description: Este artigo abordará o caso de uso de armazenamento e marcação do DoR gerado pela AEM Forms no DAM AEM. A marcação do documento é feita com base nos dados de formulário enviados.
seo-description: Este artigo abordará o caso de uso de armazenamento e marcação do DoR gerado pela AEM Forms no DAM AEM. A marcação do documento é feita com base nos dados de formulário enviados.
uuid: b9ba13ed-52d5-4389-a7d5-bf85e58fea49
feature: adaptive-forms,workflow
topics: developing
audience: implementer
doc-type: article
activity: develop
version: 6.4,6.5
discoiquuid: 53961454-633b-4cd8-aef7-e64ab4e528e4
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 0%

---


# Marcação e armazenamento do AEM Forms DoR no DAM {#tagging-and-storing-aem-forms-dor-in-dam}

Este artigo abordará o caso de uso de armazenamento e marcação do DoR gerado pela AEM Forms no DAM AEM. A marcação do documento é feita com base nos dados de formulário enviados.

Uma solicitação comum dos clientes é armazenar e marcar o Documento de Record(DoR) gerado pela AEM Forms no DAM AEM. A marcação do documento precisa ser baseada nos dados submetidos pela Adaptive Forms. Por exemplo, se o status de emprego nos dados enviados for &quot;Aposentado&quot;, desejamos marcar o documento com a tag &quot;Aposentado&quot; e armazenar o documento no DAM.

O caso de utilização é o seguinte:

* Um usuário preenche o Formulário adaptativo. Na forma adaptativa, o estado civil do usuário (ex Único) e o status de emprego (Ex Aposentado) são capturados.
* No envio do formulário, um Fluxo de trabalho AEM é acionado. Esse fluxo de trabalho marca o documento com o status civil (Único) e o status de emprego (Aposentado) e armazena o documento no DAM.
* Assim que o documento for armazenado no DAM, o administrador poderá pesquisar o documento por essas tags. Por exemplo, a pesquisa em Único ou Aposentado buscaria os DoRs apropriados.

Para satisfazer esse caso de uso, uma etapa do processo personalizado foi gravada. Nesta etapa, extraímos os valores dos elementos de dados apropriados dos dados enviados. Em seguida, construímos o bloco de tags usando esse valor. Por exemplo, se o valor do elemento de status civil for &quot;Único&quot;, o título da tag se tornará **Pico:Status do Emprego/Único. **Usando a API do TagManager, encontramos a tag e aplicamos a tag ao DoR.

O trecho de código a seguir mostra como localizar a tag e aplicar a tag ao documento.

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

Para que este exemplo funcione no seu sistema, siga as etapas listadas abaixo:
* [Implantar o pacote Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Baixe e implante o conjunto](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) setvalue. Esse é o pacote OSGI personalizado que define as tags dos dados de formulário enviados.

* [Download do formulário adaptável de amostra](assets/tag-and-store-in-dam-assets.zip)

* [Ir para Forms e Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Clique em Criar | Carregar e carregar o sampleadaptiveform.zip

* [Importar a ](assets/tag-and-store-in-dam-assets.zip) avaliação do artigo usando AEM gerenciador de pacote
* Abra o formulário de amostra [no modo de pré-visualização](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled). Preencha a seção Pessoas e envie o formulário.
* [Navegue até a pasta Pico no DAM](http://localhost:4502/assets.html/content/dam/Peak). Você deve ver DoR na pasta Pico. Verifique as propriedades do documento. Deve ser devidamente marcada.
Parabéns!! A amostra foi instalada com êxito no sistema

* Vamos explorar o [fluxo de trabalho](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) que é acionado no envio do formulário.
* A primeira etapa do fluxo de trabalho cria um nome de arquivo exclusivo concatenando o nome do candidato e o país de residência.
* A segunda etapa do fluxo de trabalho passa pela hierarquia de tags e pelos elementos dos campos de formulário que precisam ser marcados. A etapa do processo extrai o valor dos dados enviados e constrói o título da tag que precisa marcar o documento.
* Se quiser armazenar DoR em uma pasta diferente no DAM, especifique o local da pasta usando as propriedades de configuração, conforme especificado na captura de tela abaixo.

Os outros dois parâmetros são específicos do DoR e do Caminho do arquivo de dados, conforme especificado nas opções de envio do formulário adaptativo. Certifique-se de que os valores especificados aqui correspondem aos valores especificados nas opções de envio do formulário adaptativo.

![Dor de tag](assets/tag_dor_service_configuration.gif)

