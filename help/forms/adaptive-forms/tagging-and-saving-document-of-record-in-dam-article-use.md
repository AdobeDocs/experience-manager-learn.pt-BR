---
title: Marcação e armazenamento do AEM Forms DoR no DAM
seo-title: Marcação e armazenamento do AEM Forms DoR no DAM
description: Este artigo abordará o caso de uso de armazenamento e marcação do DoR gerado pelos AEM Forms no AEM DAM. A marcação do documento é feita com base nos dados de formulário enviados.
seo-description: Este artigo abordará o caso de uso de armazenamento e marcação do DoR gerado pelos AEM Forms no AEM DAM. A marcação do documento é feita com base nos dados de formulário enviados.
uuid: b9ba13ed-52d5-4389-a7d5-bf85e58fea49
feature: Formulários adaptáveis,Fluxo de trabalho
topics: developing
audience: implementer
doc-type: article
activity: develop
version: 6.4,6.5
discoiquuid: 53961454-633b-4cd8-aef7-e64ab4e528e4
topic: Desenvolvimento
role: Desenvolvedor
level: Experienciado
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 0%

---


# Marcação e armazenamento do AEM Forms DoR no DAM {#tagging-and-storing-aem-forms-dor-in-dam}

Este artigo abordará o caso de uso de armazenamento e marcação do DoR gerado pelos AEM Forms no AEM DAM. A marcação do documento é feita com base nos dados de formulário enviados.

Uma tarefa comum dos clientes é armazenar e marcar o Documento de registro (DoR) gerado pelos AEM Forms no AEM DAM. A marcação do documento precisa se basear nos dados enviados dos formulários adaptativos. Por exemplo, se o status de emprego nos dados enviados for &quot;Descontinuado&quot;, queremos adicionar uma tag ao documento com a tag &quot;Descontinuado&quot; e armazená-lo no DAM.

O caso de uso é o seguinte:

* Um usuário preenche o Formulário adaptável. Na forma adaptativa, o estado civil do usuário (ex-solteiro) e o status de emprego (ex-descontinuado) são capturados.
* No envio do formulário, um fluxo de trabalho do AEM é acionado. Esse workflow marca o documento com o status civil (Único) e o status de emprego (Descontinuado) e armazena o documento no DAM.
* Depois que o documento é armazenado no DAM, o administrador deve ser capaz de pesquisar o documento por essas tags. Por exemplo, a pesquisa em Único ou Aposentado buscaria as DoRs apropriadas.

Para atender a esse caso de uso, uma etapa do processo personalizado foi gravada. Nesta etapa, buscamos os valores dos elementos de dados apropriados a partir dos dados enviados. Em seguida, construímos o bloco da tag usando esse valor. Por exemplo, se o valor do elemento de status civil for &quot;Único&quot;, o título da tag se tornará **Pico:EmpregoStatus/Único. **Usando a API do TagManager , encontramos a tag e aplicamos a tag ao DoR.

O trecho de código a seguir mostra como localizar a tag e aplicar a tag ao documento.

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

Para que esse exemplo funcione em seu sistema, siga as etapas listadas abaixo:
* [Implantar o pacote Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Baixe e implante o pacote](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) setvalue . Esse é o pacote OSGI personalizado que define as tags a partir dos dados de formulário enviados.

* [Download do formulário adaptável de amostra](assets/tag-and-store-in-dam-assets.zip)

* [Ir para Formulários e Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Clique em Criar | Upload de arquivo e upload do sampleadaptiveform.zip

* [Importe os ativos do artigo ](assets/tag-and-store-in-dam-assets.zip) usando o gerenciador de pacotes do AEM
* Abra o [formulário de amostra no modo de visualização](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled). Preencha a seção Pessoas e envie o formulário.
* [Navegue até Pasta de pico no DAM](http://localhost:4502/assets.html/content/dam/Peak). Você deve ver DoR na pasta Pico. Verifique as propriedades do documento. Ele deve ser marcado adequadamente.
Parabéns!! A amostra foi instalada com êxito no sistema

* Vamos explorar o [workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) que é acionado no envio do formulário.
* A primeira etapa do fluxo de trabalho cria um nome de arquivo exclusivo concatenando o nome do candidato e o país de residência.
* A segunda etapa do fluxo de trabalho passa pela hierarquia de tags e pelos elementos dos campos de formulário que precisam ser marcados. A etapa do processo extrai o valor dos dados enviados e constrói o título da tag que precisa marcar o documento.
* Se quiser armazenar DoR em uma pasta diferente no DAM, especifique o local da pasta usando as propriedades de configuração, conforme especificado na captura de tela abaixo.

Os outros dois parâmetros são específicos do DoR e do Caminho do arquivo de dados, conforme especificado nas opções de envio do formulário adaptável. Certifique-se de que os valores especificados aqui correspondem aos valores especificados nas opções de envio do formulário adaptável.

![Dor de tag](assets/tag_dor_service_configuration.gif)

