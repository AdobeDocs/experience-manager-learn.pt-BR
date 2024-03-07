---
title: Formas com o AEM Forms
description: Parte 1 da integração do Acroforms com o AEM Forms. Criação de um formulário adaptável usando o Acroform e mesclagem de dados para obter um PDF.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 156
source-git-commit: 4f196539ea73d25b480064f7fc349f0ea29d5e0a
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---


# Criação do Acroform

Acroforms são formulários criados usando o Acrobat. Você pode criar um novo formulário do zero usando o Acrobat ou pegar um formulário existente criado no Microsoft Word e convertê-lo em Acroform usando o Acrobat. As etapas a seguir precisam ser seguidas para converter um formulário criado no Microsoft Word em Acroform.

* Abrir documento do Word usando Acrobat
* Use a ferramenta de formulário Acrobat Prepare para identificar os campos do formulário.
* Salve o pdf. Verifique se o nome do arquivo não contém espaços.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=12&learn=on)

>[!NOTE]
>
>Se desejar enviar o acroformulário preenchível para assinatura usando o Acrobat Sign, nomeie os campos adequadamente. Por exemplo, é possível nomear um campo **`Sig_es_:signer1:signature`**. Essa é a sintaxe que o Acrobat Sign entende.

>[!NOTE]
>
>Se você estiver enviando um documento baseado em XFA, será necessário nivelar o documento e as tags de assinatura da Acrobat Sign precisarão estar presentes como texto estático no documento.

[Documento de tags de texto do Acrobat Sign](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>Certifique-se de que o nome de arquivo acroform não tenha espaços. O código de amostra atual não lida com espaços.
>
>Os nomes dos campos de formulário podem conter apenas o seguinte:
>
>* espaço simples
>* sublinhado simples
>* caracteres alfanuméricos
