---
title: 'Uso de testes automatizados com AEM Forms adaptável '
seo-title: 'Uso de testes automatizados com AEM Forms adaptável '
description: Teste automatizado do Forms adaptativo usando o Calvin SDK
seo-description: Teste automatizado do Forms adaptativo usando o Calvin SDK
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
uuid: 3ad4e6d6-d3b1-4e4d-9169-847f74ba06be
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 0%

---


# Uso de testes automatizados com AEM Forms adaptável {#using-automated-tests-with-aem-adaptive-forms}

Teste automatizado do Forms adaptativo usando o Calvin SDK

O Calvin SDK é uma API de utilitário para desenvolvedores do Adaptive Forms para testar o Adaptive Forms. O Calvin SDK foi criado sobre a estrutura [de teste do](https://docs.adobe.com/docs/en/aem/6-3/develop/ref/test-api/index.html)Hobbes.js. O Calvin SDK está disponível a partir do AEM Forms 6.3.

Neste tutorial, você criará o seguinte:

* Test Suite
* O Test Suite conterá um ou mais casos de teste
* Os casos de teste conterão uma ou mais ações

## Introdução {#getting-started}

[Baixe e instale os ativos usando o Package](assets/testingadaptiveformsusingcalvinsdk1.zip)Manager O pacote contém scripts de amostra e vários adaptadores Forms.Esses adaptáveis Forms são criados usando a versão AEM Forms 6.3. É recomendável criar novos formulários específicos para a sua versão do AEM Forms se você estiver testando isso no AEM Forms 6.4 ou posterior. Os scripts de amostra demonstram várias APIs do Calvin SDK disponíveis para testar o Adaptive Forms. As etapas gerais para testar AEM Forms adaptável são:

* Navegue até o formulário que precisa ser testado
* Definir o valor do campo
* Enviar o formulário adaptativo
* Verificar se há mensagens de erro

Os scripts de amostra no pacote demonstram todas as ações acima.
Vamos explorar o código de `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

O código acima cria um novo Test Suite.

* O nome do TestSuite nesse caso é &#39; `Mortgage Form Test` &#39;.
* Fornecido é o caminho absoluto em AEM para o arquivo js que contém o conjunto de testes.
* O parâmetro de registro quando definido como &#39; `true` &#39; torna o Test Suite disponível na interface de teste.

```javascript
.addTestCase(new hobs.TestCase("Calculate amount to borrow")
        // navigate to the mortgage form  which is to be tested
        .navigateTo("/content/forms/af/cal/mortgageform.html?wcmmode=disabled")
  .asserts.isTrue(function () {
            return calvin.isFormLoaded()
        })
```

>[!NOTE]
>
>Se o seu computador estiver testando esse recurso no AEM Forms 6.4 ou superior, crie um novo Formulário adaptável e use-o para fazer o teste.Não é recomendável usar o formulário adaptativo fornecido com o pacote.

Casos de teste podem ser adicionados ao conjunto de testes para serem executados em um formulário adaptável.

* Para adicionar um caso de teste ao conjunto de testes, use o `addTestCase` método do objeto TestSuite.
* O `addTestCase` método usa um objeto TestCase como parâmetro.
* Para criar o TestCase, use o `hobs.TestCase(..)` método.
* Observação: O primeiro parâmetro é o nome do Caso de teste que aparecerá na interface do usuário.
* Depois de criar um caso de teste, você pode adicionar ações ao caso de teste.
* As ações, incluindo `navigateTo`, `asserts.isTrue` podem ser adicionadas como ações ao caso de teste.

## Execução dos testes automatizados {#running-the-automated-tests}

[Abra](http://localhost:4502/libs/granite/testing/hobbes.html)o conjunto de testesExpanda o conjunto de testes e execute os testes. Se tudo for executado com êxito, você verá a seguinte saída.

![calvinsdk](assets/calvinimage.png)

## Experimente os conjuntos de teste de amostra {#try-out-the-sample-test-suites}

Como parte do pacote de amostra existem três conjuntos de testes adicionais. Você pode experimentá-los incluindo os arquivos apropriados no arquivo js.txt da biblioteca de clientes, como mostrado abaixo:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
