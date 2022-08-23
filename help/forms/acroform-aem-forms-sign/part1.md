---
title: Acrobora com AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Parte 1 da integração do Acroforms com o AEM Forms. Criar um formulário adaptável usando o Acroform e unir os dados para obter um PDF.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---


# Criação de Acroform

Os formulários são criados usando o Acrobat. Você pode criar um novo formulário do zero usando o Acrobat ou pegar um formulário existente criado no Microsoft Word e convertê-lo no Acroform usando o Acrobat. As etapas a seguir precisam ser seguidas para converter um formulário criado no Microsoft Word em Acroform.

* Abrir documento do Word usando o Acrobat
* Use a ferramenta Acrobat Prepare form para identificar os campos do formulário.
* Salve o pdf. Verifique se o nome do arquivo não tem espaços nele.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>Se desejar enviar o formulário de preenchimento para assinatura usando o Adobe Sign, nomeie os campos de acordo. Por exemplo, é possível nomear um campo **Sig_es_:signer1:assinatura**. Essa é a sintaxe que o Adobe Sign entende.

>[!NOTE]
>
>Se você estiver enviando um documento baseado em XFA, precisará nivelar o documento e as tags de assinatura Adobe Sign precisam estar presentes como texto estático no documento.

[Documento de Tags de texto do Adobe Sign](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>Certifique-se de que o nome do arquivo acroform não tenha espaços nele. O código de amostra atual não lida com espaços.
>
>Os nomes dos campos de formulário só podem conter o seguinte:
>
>* espaço único
>* sublinhado único
>* caracteres alfanuméricos

