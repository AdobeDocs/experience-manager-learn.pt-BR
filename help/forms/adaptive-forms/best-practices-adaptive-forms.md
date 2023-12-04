---
title: Convenções de nomenclatura e práticas recomendadas a serem seguidas ao criar formulários adaptáveis
description: Convenções de nomenclatura e práticas recomendadas a serem seguidas ao criar formulários adaptáveis
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: fbfc74d7-ba7c-495a-9e3b-63166a3025ab
last-substantial-update: 2020-09-10T00:00:00Z
duration: 83
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---

# Práticas recomendadas

Os formulários Adobe Experience Manager (AEM) podem ajudar você a transformar transações complexas em experiências digitais simples e atraentes. O documento a seguir descreve algumas práticas recomendadas adicionais que precisam ser seguidas ao desenvolver o Adaptive Forms. Este documento deve ser usado em conjunto com [este documento](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## Convenções de nomenclatura

* **Painéis**
   * Os nomes de painel são camel case começando com um caractere maiúsculo.

* **Campos de formulário**
   * Os nomes de campos são camel case começando com um caractere em minúsculas.
   * Não iniciar nomes de campo com números
   * Não inclua traços &quot;-&quot; em seus nomes. Eles serão equivalentes a um sinal de menos em seu código e atuarão como operadores em seu código.
   * Os nomes podem conter letras, dígitos, sublinhados e cifrões.
   * Os nomes devem começar com uma letra
   * Os nomes diferenciam maiúsculas de minúsculas
   * Palavras reservadas (como palavras-chave JavaScript) não podem ser usadas como nomes. Cuidado com outras palavras reservadas específicas da AF, como &quot;panel&quot;,&quot;name&quot;.
   * Não inclua traços &quot;-&quot; em seus nomes
* **Desenvolvimento do Forms**
   * Os fragmentos de formulário devem ser considerados ao desenvolver formulários grandes. Ativar carregamento lento de fragmentos de formulário para tempos de carregamento mais rápidos
   * **Modelo de dados**
      * É recomendável associar o formulário adaptável ao modelo de dados apropriado

   * **Eventos de objeto**
      * O código relacionado à visibilidade de um objeto deve sempre ser colocado no evento de visibilidade desse objeto.
   * **Script**
      * Se o código que você está gravando em um formulário adaptável ultrapassar as 5 linhas visíveis, será necessário mover seu código para uma Biblioteca do cliente. Idealmente, adicione sua função à biblioteca do cliente e, em seguida, adicione as tags jsdoc apropriadas para permitir que a função fique visível no editor de regras do Formulário adaptável.
