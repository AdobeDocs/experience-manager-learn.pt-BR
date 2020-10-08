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
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---


# Criação de Acroform

Os formulários são formulários criados usando o Acrobat. Você pode criar um novo formulário do zero usando o Acrobat ou pegar um formulário existente criado no Microsoft Word e convertê-lo no Acroform usando o Acrobat. As etapas a seguir precisam ser seguidas para converter um formulário criado no Microsoft Word em Acroform.

* Abrir documento de palavras usando o Acrobat
* Use a ferramenta de formulário Preparar da Acrobat para identificar os campos do formulário no formulário.
* Salve o pdf. Verifique se o nome do arquivo não tem espaços nele.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>Se desejar enviar o formulário preenchível para assinatura usando o Adobe Sign, nomeie os campos de acordo. Por exemplo, você pode nomear um campo como **Sig_es_:signer1:signature**. Esta é a sintaxe que a Adobe Sign entende.

>[!NOTE]
>
>Se você estiver enviando um documento baseado em XFA, precisará nivelar o documento e as tags de assinatura Adobe Sign precisarão estar presentes como texto estático no documento.

[documento de Tags de Texto Adobe Sign](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>Verifique se o nome do arquivo acroform não tem espaços nele. O código de amostra atual não lida com espaços.
>
>Os nomes de campos de formulário só podem conter o seguinte:
>
>* espaço único
>* sublinhado único
>* caracteres alfanuméricos

