---
title: Uso de testes automatizados com AEM Adaptive Forms
description: Teste automatizado do Adaptive Forms usando o Calvin SDK
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
last-substantial-update: 2020-09-10T00:00:00Z
duration: 117
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%

---

# Uso de testes automatizados com AEM Adaptive Forms {#using-automated-tests-with-aem-adaptive-forms}

Teste automatizado do Adaptive Forms usando o Calvin SDK

O SDK do Calvin é uma API de utilitários para desenvolvedores do Adaptive Forms testarem o Adaptive Forms. O SDK do Calvin é criado sobre o [Estrutura de teste de Hobbes.js](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=pt-BR). O SDK do Calvin está disponível com o AEM Forms 6.3 em diante.

Neste tutorial, você criará o seguinte:

* Conjunto de teste
* O conjunto de testes conterá um ou mais casos de teste
* Os Casos de Teste conterão uma ou mais ações

## Introdução {#getting-started}

[Baixe e instale os ativos usando o Gerenciador de pacotes](assets/testingadaptiveformsusingcalvinsdk1.zip)O pacote contém exemplos de scripts e vários Forms adaptáveis. Esses Forms adaptáveis são criados usando a versão AEM Forms 6.3. É recomendável criar novos formulários específicos para sua versão do AEM Forms, se você estiver testando isso no AEM Forms 6.4 ou superior. Os exemplos de scripts demonstram várias APIs de SDK Calvin disponíveis para testar o Adaptive Forms. As etapas gerais para testar o AEM Adaptive Forms são:

* Navegue até o formulário que precisa ser testado
* Definir valor do campo
* Enviar o formulário adaptável
* Verificar se há mensagens de erro

Os scripts de amostra no pacote demonstram todas as ações acima.
Vamos explorar o código de `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

O código acima cria um novo Conjunto de testes.

* Nesse caso, o nome do TestSuite é &#39; `Mortgage Form Test` &#39;.
* O caminho absoluto no AEM para o arquivo js que contém o conjunto de testes.
* O parâmetro de registro quando definido como &#39; `true` &#39;, disponibiliza o Conjunto de testes na interface de usuário de teste.

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
>Se você estiver testando esse recurso no AEM Forms 6.4 ou superior, crie um novo Formulário adaptável e use-o para fazer seus testes.Não é recomendado usar o Formulário adaptável fornecido com o pacote.

Os casos de teste podem ser adicionados ao conjunto de testes para serem executados em um formulário adaptável.

* Para adicionar um caso de teste ao conjunto de testes, use o `addTestCase` método do objeto TestSuite.
* A variável `addTestCase` O método usa um objeto TestCase como um parâmetro.
* Para criar um TestCase, use o `hobs.TestCase(..)` método.
* Observação: o primeiro parâmetro é o nome do Caso de teste que aparecerá na interface do usuário.
* Depois de criar um caso de teste, você pode adicionar ações ao caso de teste.
* Ações que incluem `navigateTo`, `asserts.isTrue` podem ser adicionadas como ações ao caso de teste.

## Execução dos testes automatizados {#running-the-automated-tests}

[Openthetestsuite](http://localhost:4502/libs/granite/testing/hobbes.html)Expanda o Conjunto de testes e execute os testes. Se tudo for executado com êxito, você verá a seguinte saída.

![calvinsdk](assets/calvinimage.png)

## Experimente os conjuntos de teste de amostra {#try-out-the-sample-test-suites}

Como parte do pacote de amostra, há três conjuntos de teste adicionais. Você pode experimentá-los incluindo os arquivos apropriados no arquivo js.txt da biblioteca do cliente, como mostrado abaixo:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
