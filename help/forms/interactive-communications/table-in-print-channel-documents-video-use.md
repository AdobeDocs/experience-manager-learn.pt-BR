---
title: Uso do componente Tabela no Documento Canal de impressão AEM Forms
seo-title: Uso do componente Tabela no Documento Canal de impressão AEM Forms
description: O vídeo a seguir apresenta as etapas necessárias para usar o componente de tabela no Interative Communications para documentos de canal de impressão.
feature: interactive-communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# Uso do componente Tabela no Documento Canal de impressão AEM Forms {#using-table-component-in-aem-forms-print-channel-document}

O vídeo a seguir apresenta as etapas necessárias para usar o componente de tabela no Interative Communications para documentos de canal de impressão.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

Tabelas são usadas para exibir dados de maneira tabular. As linhas na tabela precisam aumentar ou diminuir dependendo dos dados retornados pela fonte de dados. Para usar uma tabela no documento do canal de impressão, é necessário criar um arquivo de layout (arquivo xdp) usando o AEM Forms Designer. Neste arquivo de layout, adicionamos a tabela com o número necessário de colunas. Verifique se o tipo de objeto de campo de coluna é TextField ou Numeric Field, dependendo de seus requisitos. Para cada coluna, os campos garantem que o vínculo de dados esteja definido como Usar nome.

>[!NOTE]
>
>Para tornar a tabela dinâmica, certifique-se de ter marcado a Linha como repetida.

**Experimente no seu próprio servidor**

* [Baixe e descompacte o arquivo de ativos no disco rígido](assets/usingtablesinprintchannel.zip)

* Importe os dois arquivos zip para AEM usando o gerenciador de pacote

* Os ativos associados a este artigo são os seguintes:

   * Fragmento do layout

   * Modal de dados do formulário

   * Documento de comunicação interativa
   * sampleretirementaccountdata.json

* Abra o Documento Interative Communication no [modo de edição](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* Adicione o fragmento de layout TableDemo à seção de contribuições.
* Vincule as células da tabela aos elementos apropriados do Modelo de dados de formulário, conforme mostrado no vídeo

* Pré-visualização do Documento de comunicação interativa com o arquivo de dados json de amostra fornecido a você

