---
title: Uso de testes automatizados com AEM Adaptive Forms
description: Teste automatizado do Adaptive Forms usando o Calvin SDK
feature: Adaptive Forms
doc-type: article
activity: develop
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
last-substantial-update: 2020-09-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 2%

---

# Uso de testes automatizados com AEM Adaptive Forms {#using-automated-tests-with-aem-adaptive-forms}

Teste automatizado do Adaptive Forms usando o Calvin SDK

O SDK do Calvin é uma API de utilitário para os desenvolvedores do Adaptive Forms testarem o Adaptive Forms. O SDK do Calvin é criado sobre [Estrutura de teste do Hobbes.js](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=pt-BR). O Calvin SDK está disponível a partir do AEM Forms 6.3.

Neste tutorial, você criará o seguinte:

* Conjunto de testes
* O Test Suite conterá um ou mais casos de teste
* Os casos de teste conterão uma ou mais ações

## Introdução {#getting-started}

[Baixe e instale os ativos usando o Gerenciador de pacotes](assets/testingadaptiveformsusingcalvinsdk1.zip)O pacote contém exemplos de scripts e vários Adaptive Forms.Esses Adaptive Forms são criados com a versão AEM Forms 6.3. É recomendável criar novos formulários específicos para a sua versão do AEM Forms, se você estiver testando isso no AEM Forms 6.4 ou posterior. Os scripts de amostra demonstram várias APIs do SDK do Calvin disponíveis para testar o Adaptive Forms. As etapas gerais para testar AEM Adaptive Forms são:

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

* O nome do TestSuite neste caso é &#39; `Mortgage Form Test` &quot;.
* Fornecido é o caminho absoluto em AEM para o arquivo js que contém o conjunto de teste.
* O parâmetro de registro quando definido como &#39; `true` &#39;, disponibiliza o Test Suite na interface de usuário de teste.

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
>Se você estiver testando esse recurso no AEM Forms 6.4 ou superior, crie um novo formulário adaptável e use-o para fazer o teste.Não é recomendado usar o formulário adaptável fornecido com o pacote.

Casos de teste podem ser adicionados ao conjunto de teste para serem executados em relação a um formulário adaptável.

* Para adicionar um caso de teste ao conjunto de teste, use a variável `addTestCase` método do objeto TestSuite.
* O `addTestCase` O método usa um objeto TestCase como parâmetro.
* Para criar o TestCase use a variável `hobs.TestCase(..)` método .
* Observação: O primeiro parâmetro é o nome do caso de teste que aparecerá na interface do usuário.
* Depois de criar um caso de teste, você pode adicionar ações ao caso de teste.
* Ações incluindo `navigateTo`, `asserts.isTrue` pode ser adicionado como ações ao caso de teste.

## Execução dos testes automatizados {#running-the-automated-tests}

[Openthetestsuite](http://localhost:4502/libs/granite/testing/hobbes.html)Expanda o Test Suite e execute os testes. Se tudo for executado com êxito, você verá a seguinte saída.

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
