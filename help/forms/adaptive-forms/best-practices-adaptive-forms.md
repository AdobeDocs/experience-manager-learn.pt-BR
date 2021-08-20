---
title: Convenções de nomenclatura e práticas recomendadas a serem seguidas ao criar formulários adaptáveis
description: Convenções de nomenclatura e práticas recomendadas a serem seguidas ao criar formulários adaptáveis
feature: Formulários adaptáveis
version: 6.3,6.4,6.5
topic: Desenvolvimento
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 2%

---

# Práticas recomendadas    

Os formulários Adobe Experience Manager (AEM) podem ajudar você a transformar transações complexas em experiências digitais simples e deliciosas. O documento a seguir descreve algumas práticas recomendadas adicionais que precisam ser seguidas ao desenvolver o Adaptive Forms. Este documento deve ser usado junto com [este documento](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## Convenções de nomenclatura

* **Painéis**
   * Os nomes dos painéis são concatenadas, começando com um caractere em maiúsculas.

* **Campos de Forma**
   * Os nomes de campos são concatenadas, começando com um caractere em minúsculas.
   * Não inicie nomes de campos com números
   * Não inclua traços &quot;-&quot; em seus nomes. Eles corresponderão a um sinal de menos no código e atuarão como operadores no código.
   * Os nomes podem conter letras, dígitos, sublinhados e cifrões.
   * Os nomes devem começar com uma letra
   * Os nomes fazem distinção entre maiúsculas e minúsculas
   * Palavras reservadas (como palavras-chave JavaScript) não podem ser usadas como nomes. Fique atento a outras palavras reservadas específicas para AF, como   como &quot;painel&quot;, &quot;nome&quot;.
   * Não inclua traços &quot;-&quot; em seus nomes
* **Desenvolvimento do Forms**
   * Os fragmentos de formulário devem ser considerados ao desenvolver formulários grandes. Habilitar carregamento lento de fragmentos de formulário para carregamento mais rápido   times
   * **DataModel**
      * Recomenda-se associar o formulário adaptável ao modelo de dados apropriado
   * **Eventos de objeto**
      * O código relacionado à visibilidade de um objeto deve ser sempre colocado no evento de visibilidade desse objeto.
   * **Script**
      * Se o código que você está escrevendo em um Formulário adaptativo estender mais de cinco linhas visíveis, será necessário mover seu código para uma Biblioteca do cliente. Idealmente, adicione sua função à biblioteca do cliente e, em seguida, adicione as tags jsdoc apropriadas para permitir que a função fique visível no editor de regras do Adaptive Form.


