---
title: Uso do componente de Tabela no Documento de canal de impressão do AEM Forms
description: O vídeo a seguir mostra as etapas necessárias para usar o componente de tabela nas Comunicações interativas para documentos de canal de impressão.
feature: Interactive Communication
doc-type: technical video
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 54afd047-c6e6-4557-9336-39420f30df88
last-substantial-update: 2019-07-07T00:00:00Z
duration: 300
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Uso do componente de Tabela no Documento de canal de impressão do AEM Forms {#using-table-component-in-aem-forms-print-channel-document}

O vídeo a seguir mostra as etapas necessárias para usar o componente de tabela nas Comunicações interativas para documentos de canal de impressão.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=12&learn=on)

As tabelas são usadas para exibir dados na forma de tabelas. As linhas na tabela precisam aumentar ou diminuir, dependendo dos dados retornados pela fonte de dados. Para usar uma tabela no documento de canal de impressão, precisamos criar um arquivo de layout (arquivo xdp) usando o AEM Forms Designer. Neste arquivo de layout, adicionamos a tabela com o número necessário de colunas. Verifique se o tipo de objeto do campo de coluna é TextField ou Numeric Field, dependendo de suas necessidades. Para cada coluna, os campos garantem que a associação de dados esteja definida como Usar nome.

>[!NOTE]
>
>Para tornar a tabela dinâmica, verifique se você marcou a linha como repetitiva.

**Experimente no seu próprio servidor**

* [Baixe e descompacte o arquivo de ativos no disco rígido](assets/usingtablesinprintchannel.zip)

* Importe os dois arquivos zip para o AEM usando o gerenciador de pacotes

* Estão incluídos nos ativos associados a este artigo:

   * Fragmento do layout

   * Modal de dados do formulário

   * Documento de comunicação interativa
   * sampleretirementaccountdata.json

* Abra o documento de comunicação interativa no [modo de edição](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* Adicione o fragmento de layout TableDemo à seção de contribuições.
* Vincule as células da tabela aos elementos apropriados do Modelo de dados de formulário, conforme mostrado no vídeo

* Visualizar o documento de comunicação interativa com o arquivo de dados json de amostra fornecido a você
