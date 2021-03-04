---
title: 'Uso de testes automatizados com AEM Adaptive Forms '
seo-title: 'Uso de testes automatizados com AEM Adaptive Forms '
description: Teste automatizado de formulários adaptáveis usando o SDK do Calvin
seo-description: Teste automatizado de formulários adaptáveis usando o SDK do Calvin
feature: Formulários adaptáveis
topics: development
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
uuid: 3ad4e6d6-d3b1-4e4d-9169-847f74ba06be
topic: Desenvolvimento
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 1%

---


# Uso de testes automatizados com AEM Adaptive Forms {#using-automated-tests-with-aem-adaptive-forms}

Teste automatizado de formulários adaptáveis usando o SDK do Calvin

O SDK do Calvin é uma API de utilitário para os desenvolvedores de formulários adaptáveis testarem formulários adaptáveis. O SDK do Calvin é criado sobre a [estrutura de teste Hobbes.js](https://docs.adobe.com/docs/en/aem/6-3/develop/ref/test-api/index.html). O Calvin SDK está disponível com o AEM Forms 6.3 em diante.

Neste tutorial, você criará o seguinte:

* Conjunto de teste
* O Test Suite conterá um ou mais casos de teste
* Os casos de teste conterão uma ou mais ações

## Introdução {#getting-started}

[Baixe e instale os ativos usando o ](assets/testingadaptiveformsusingcalvinsdk1.zip)Gerenciador de pacotesO pacote contém scripts de amostra e vários formulários adaptáveis.Esses formulários adaptáveis são criados usando a versão AEM Forms 6.3. É recomendável criar novos formulários específicos para sua versão do AEM Forms se você estiver testando isso no AEM Forms 6.4 ou superior. Os scripts de amostra demonstram várias APIs do SDK do Calvin disponíveis para testar Formulários adaptáveis. As etapas gerais para testar os AEM Adaptive Forms são:

* Navegue até o formulário que precisa ser testado
* Definir o valor do campo
* Enviar o formulário adaptável
* Verificar mensagens de erro

Os scripts de amostra no pacote demonstram todas as ações acima.
Vamos explorar o código de `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

O código acima cria um novo Test Suite.

* O nome do TestSuite, neste caso, é &#39; `Mortgage Form Test` &#39;.
* Fornecido é o caminho absoluto no AEM para o arquivo js que contém o conjunto de testes.
* O parâmetro de registro quando definido como &#39; `true` &#39;, torna o Test Suite disponível na interface de teste.

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
>Se você estiver testando esse recurso no AEM Forms 6.4 ou superior, crie um novo Formulário adaptável e use-o para fazer seu teste.Não é recomendado usar o formulário adaptável fornecido com o pacote.

Casos de teste podem ser adicionados ao conjunto de teste para serem executados em relação a um formulário adaptável.

* Para adicionar um caso de teste ao conjunto de teste, use o método `addTestCase` do objeto TestSuite.
* O método `addTestCase` usa um objeto TestCase como parâmetro.
* Para criar o TestCase, use o método `hobs.TestCase(..)`.
* Observação: O primeiro parâmetro é o nome do caso de teste que aparecerá na interface do usuário.
* Depois de criar um caso de teste, você pode adicionar ações ao caso de teste.
* Ações incluindo `navigateTo`, `asserts.isTrue` podem ser adicionadas como ações ao caso de teste.

## Execução dos testes automatizados {#running-the-automated-tests}

[](http://localhost:4502/libs/granite/testing/hobbes.html)Abra o conjunto de testeExpanda o conjunto de teste e execute os testes. Se tudo for executado com êxito, você verá a seguinte saída.

![calvinsdk](assets/calvinimage.png)

## Experimente os conjuntos de teste de amostra {#try-out-the-sample-test-suites}

Como parte do pacote de amostra, há três conjuntos de testes adicionais. Você pode experimentá-los incluindo os arquivos apropriados no arquivo js.txt da biblioteca do cliente, conforme mostrado abaixo:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
