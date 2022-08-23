---
title: Acrobora com AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Parte 3 de um tutorial que integra o Acroforms ao AEM Forms. Teste o fluxo de trabalho e o formulário adaptável no sistema.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 1%

---


# Teste esse recurso no sistema

[Baixe e importe este pacote para o AEM](assets/acro-form-aem-form.zip)
Este pacote contém o fluxo de trabalho de amostra e a página html, que permite criar o esquema a partir do Acroform carregado.

## Configurar fluxo de trabalho

1. [Abra o Modelo de fluxo de trabalho no modo de edição](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. Abra as propriedades de configuração da etapa MergeAcroformData .
3. Clique na guia Processo .
4. Verifique se os argumentos que você está transmitindo são uma pasta válida no seu servidor.
5. Salve as alterações.

## Criar formulário adaptável

1. Crie um formulário adaptável usando o esquema criado na etapa anterior.
2. Arraste e solte alguns elementos de esquema no Formulário adaptável.
3. Configure a ação de envio do formulário adaptável para enviar para AEM fluxo de trabalho (MergeAcroformData).
4. **Especifique o caminho do arquivo de dados como &quot;Data.xml&quot;. Isso é muito importante, pois o código de amostra procura um arquivo chamado Data.xml na carga útil do workflow.**
5. Visualizar formulário adaptável, preencher o formulário e enviar.
6. Você deve ver o PDF com os dados unidos à pasta especificada na etapa 4 no fluxo de trabalho de configuração

>[!NOTE]
>
>O pdf gerado pela mesclagem de dados com o acroform é salvo como pdfdocument.pdf na pasta payload do workflow. Este documento pode ser usado para processamento adicional como parte do fluxo de trabalho
