---
title: Uso do Componente de tabela no Documento de canal de impressão do AEM Forms
seo-title: Uso do Componente de tabela no Documento de canal de impressão do AEM Forms
description: O vídeo a seguir apresenta as etapas necessárias para usar o componente de tabela nas Comunicações interativas para documentos do canal de impressão.
feature: Comunicação interativa
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
topic: Desenvolvimento
role: Desenvolvedor
level: Intermediário
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 2%

---


# Uso do Componente de tabela no Documento de canal de impressão do AEM Forms {#using-table-component-in-aem-forms-print-channel-document}

O vídeo a seguir apresenta as etapas necessárias para usar o componente de tabela nas Comunicações interativas para documentos do canal de impressão.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

Tabelas são usadas para exibir dados de maneira tabular. As linhas da tabela precisam crescer ou diminuir dependendo dos dados retornados pela fonte de dados. Para usar uma tabela no documento de canal de impressão, precisamos criar um arquivo de layout (arquivo xdp) usando o AEM Forms Designer. Nesse arquivo de layout, adicionamos a tabela com o número necessário de colunas. Verifique se o tipo de objeto de campo de coluna é TextField ou Numeric Field, dependendo de suas necessidades. Para cada coluna, os campos garantem que o vínculo de dados esteja definido como Usar nome.

>[!NOTE]
>
>Para tornar a tabela dinâmica, verifique se você marcou a Linha como repetitiva.

**Experimente-o no seu próprio servidor**

* [Baixe e descompacte o arquivo de ativos no seu disco rígido](assets/usingtablesinprintchannel.zip)

* Importe os dois arquivos zip para o AEM usando o gerenciador de pacotes

* Os ativos associados a este artigo estão incluídos no seguinte:

   * Fragmento do layout

   * Modal de dados do formulário

   * Documento de comunicação interativa
   * sampleretirementaccountdata.json

* Abra o Documento de comunicação interativa no [modo de edição](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* Adicione o fragmento de layout TableDemo à seção contribuições.
* Vincule as células da tabela aos elementos apropriados do Modelo de dados de formulário, conforme mostrado no vídeo

* Visualizar documento de comunicação interativa com o arquivo de dados json de amostra fornecido a você

