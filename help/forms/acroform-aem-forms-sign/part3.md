---
title: Acrobat com AEM Forms
seo-title: Mesclar dados do formulário adaptável com o Acroform
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 1%

---


# Teste esse recurso em seu sistema

[Baixe e importe este pacote para AEM](assets/acro-form-aem-form.zip)Este pacote contém o fluxo de trabalho de amostra e a página html que permite criar o schema a partir do Acroform carregado.

## Configurar fluxo de trabalho

1. [Abra o Modelo de fluxo de trabalho no modo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html)de edição.
2. Abra as propriedades de configuração da etapa MergeAcroformData.
3. Clique na guia Processo.
4. Verifique se os argumentos que você está transmitindo são uma pasta válida no servidor.
5. Salve as alterações.

## Criar formulário adaptável

1. Crie um formulário adaptável usando o schema criado na etapa anterior.
2. Arraste e solte alguns elementos do schema no Formulário adaptável.
3. Configure a ação de envio do Formulário adaptativo para enviar ao fluxo de trabalho AEM (MergeAcroformData).
4. **Certifique-se de especificar o caminho do arquivo de dados como &quot;Data.xml&quot;. Isso é muito importante, pois o código de amostra procura por um arquivo chamado Data.xml na carga do fluxo de trabalho.**
5. Formulário adaptativo de pré-visualização, preencha o formulário e envie.
6. Você deve ver o PDF com os dados unidos salvos na pasta especificada na etapa 4 no fluxo de trabalho de configuração

>[!NOTE]
>
>O pdf gerado pela união de dados com o acroform é salvo como pdfdocument.pdf na pasta de carga do fluxo de trabalho. Esse documento pode ser usado para processamento adicional como parte do fluxo de trabalho
