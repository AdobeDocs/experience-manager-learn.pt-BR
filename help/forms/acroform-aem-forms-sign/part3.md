---
title: Formas com o AEM Forms
description: Parte 3 de um tutorial que integra o Acroforms com o AEM Forms. Teste o fluxo de trabalho e o formulário adaptável no sistema.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 45
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 1%

---


# Testar esse recurso no sistema

[Baixar e importar este pacote para AEM](assets/acro-form-aem-form.zip)
Esse pacote contém o fluxo de trabalho de amostra e a página html que permite criar o esquema a partir do Acrobat carregado.

## Configurar fluxo de trabalho

1. [Abra o Modelo de Fluxo de Trabalho no modo de edição](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. Abra as propriedades de configuração da etapa MergeCrossData.
3. Clique na guia Processo.
4. Verifique se os argumentos que você está transmitindo são uma pasta válida no servidor.
5. Salve as alterações.

## Criar formulário adaptável

1. Crie um formulário adaptável usando o esquema criado na etapa anterior.
2. Arraste e solte alguns elementos do esquema no Formulário adaptável.
3. Configure a ação de envio do Formulário adaptável para enviar ao fluxo de trabalho do AEM (MergeFormData).
4. **Certifique-se de especificar o caminho do arquivo de dados como &quot;Data.xml&quot;. Isso é muito importante, pois o código de amostra procura um arquivo chamado Data.xml na carga do fluxo de trabalho.**
5. Pré-visualizar o formulário adaptável, preencher o formulário e enviar.
6. Você deve ver o PDF com os dados mesclados e salvos na pasta especificada na etapa 4 na seção configurar fluxo de trabalho

>[!NOTE]
>
>O pdf gerado pela mesclagem de dados com o acroformulário é salvo como pdfdocument.pdf na pasta de carga do fluxo de trabalho. Esse documento pode ser usado para processamento adicional como parte do workflow
