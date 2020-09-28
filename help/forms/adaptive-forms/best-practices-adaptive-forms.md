---
title: Como nomear convenções e práticas recomendadas a serem seguidas ao criar formulários adaptáveis
seo-title: Como nomear convenções e práticas recomendadas a serem seguidas ao criar formulários adaptáveis
description: Como nomear convenções e práticas recomendadas a serem seguidas ao criar formulários adaptáveis
seo-description: Como nomear convenções e práticas recomendadas a serem seguidas ao criar formulários adaptáveis
feature: adaptive-forms
topics: best-practices
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 5b05dbe45babfcfcfc81995d9d48bc9b26b9b6c8
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 1%

---

# Práticas recomendadas    

Os formulários Adobe Experience Manager (AEM) podem ajudá-lo a transformar transações complexas em experiências digitais simples e deliciosas. O documento a seguir descreve algumas práticas recomendadas adicionais que precisam ser seguidas ao desenvolver o Adaptive Forms. Este documento deve ser usado juntamente com [este documento](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## Convenções de nomenclatura

* **Painéis**
   * Os nomes dos painéis são maiúsculas e minúsculas começando com um caractere maiúsculo.

* **Campos de Forma**
   * Nomes de campo são letras maiúsculas e minúsculas, começando com um caractere minúsculo.
   * Não start nomes de campos com números
   * Não inclua traços &quot;-&quot; em seus nomes. Isso equivale a um sinal de menos no código e atuará como operadores no código.
   * Os nomes podem conter letras, dígitos, sublinhados e sinais em dólar.
   * Os nomes devem começar com uma carta
   * Os nomes fazem distinção entre maiúsculas e minúsculas
   * Palavras reservadas (como palavras-chave JavaScript) não podem ser usadas como nomes. Fique atento a outras palavras reservadas específicas para AF, como &quot;painel&quot;, &quot;nome&quot;.
   * Não inclua traços &quot;-&quot; em seus nomes
* **Forms de desenvolvimento**
   * Os fragmentos de formulário devem ser considerados ao desenvolver formulários grandes. Ativar carregamento lento de fragmentos de formulário para tempos de carregamento mais rápidos
   * **DataModel**
      * É recomendável associar o formulário adaptativo ao modelo de dados apropriado
   * **Eventos de objeto**
      * O código relacionado à visibilidade de um objeto deve ser sempre colocado no evento de visibilidade desse objeto.
   * **Script**
      * Se o código que você está escrevendo em um Formulário adaptável ultrapassar 5 linhas visíveis, é necessário mover seu código para uma Biblioteca de clientes. Idealmente, adicione sua função à biblioteca do cliente e, em seguida, adicione as tags jsdoc apropriadas para permitir que a função fique visível no editor de regras do Formulário adaptável.


