---
title: Teste de unidade
description: Este tutorial aborda a implementação de um Teste de unidade que valida o comportamento do Modelo Sling do componente Byline, criado no tutorial de Componente personalizado.
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
translation-type: tm+mt
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '3015'
ht-degree: 0%

---


# Teste de unidade {#unit-testing}

Este tutorial aborda a implementação de um Teste de unidade que valida o comportamento do Modelo Sling do componente Byline, criado no tutorial [Componente personalizado](./custom-component.md).

## Pré-requisitos {#prerequisites}

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

_Se o Java 8 e o Java 11 estiverem instalados no sistema, o executante de teste do Código VS pode escolher o tempo de execução do Java mais baixo ao executar os testes, resultando em falhas de teste. Se isso ocorrer, desinstale o Java 8._

### Projeto inicial

>[!NOTE]
>
> Se você tiver concluído com êxito o capítulo anterior, poderá reutilizar o projeto e ignorar as etapas para fazer check-out do projeto inicial.

Confira o código básico no qual o tutorial se baseia:

1. Verifique a ramificação `tutorial/unit-testing-start` de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Implante a base de código para uma instância AEM local usando suas habilidades Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando AEM 6.5 ou 6.4, anexe o perfil `classic` a qualquer comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) ou fazer check-out do código localmente ao alternar para a ramificação `tutorial/unit-testing-start`.

## Objetivo

1. Entenda as noções básicas de teste de unidade.
1. Saiba mais sobre estruturas e ferramentas comumente usadas para testar o código AEM.
1. Entenda as opções de zombaria ou simulação de recursos AEM ao gravar testes de unidade.

## Segundo plano {#unit-testing-background}

Neste tutorial, exploraremos como escrever [Testes de Unidade](https://en.wikipedia.org/wiki/Unit_testing) para o [Modelo Sling](https://sling.apache.org/documentation/bundles/models.html) do componente Byline (criado em [Criação de um componente AEM personalizado](custom-component.md)). Os testes de unidade são testes de tempo de criação escritos em Java que verificam o comportamento esperado do código Java. Em geral, cada teste de unidade é pequeno e valida a saída de um método (ou unidades de trabalho) em relação aos resultados esperados.

Usaremos AEM práticas recomendadas e:

* [JUnit 5](https://junit.org/junit5/)
* [Estrutura de teste Mockito](https://site.mockito.org/)
* [wcm.io Test Framework](https://wcm.io/testing/)  (que se baseia em  [Mocks](https://sling.apache.org/documentation/development/sling-mock.html) Apache Sling)

## Teste de unidade e Gerenciador da Adobe Cloud {#unit-testing-and-adobe-cloud-manager}

[O Adobe Cloud ](https://docs.adobe.com/content/help/pt-BR/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html) Manager integra execução de teste de unidade e relatório de cobertura de  [código ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) em seu pipeline de CI/CD para ajudar a incentivar e promover as melhores práticas de teste de unidade AEM código.

Embora o código de teste de unidade seja uma boa prática para qualquer base de código, ao usar o Gerenciador de nuvem, é importante aproveitar seus testes de qualidade de código e recursos de relatórios, fornecendo testes de unidade para que o Gerenciador de nuvem seja executado.

## Inspect as dependências do Maven de teste {#inspect-the-test-maven-dependencies}

O primeiro passo é inspecionar as dependências Maven para suportar a gravação e a execução dos testes. Há quatro dependências necessárias:

1. JUnit5
1. Estrutura de teste Mockito
1. Mocas de martelo do Apache
1. AEM Mocks Test Framework (por io.wcm)

As dependências de teste **JUnit5**, **Mockito** e **AEM Mocks** são automaticamente adicionadas ao projeto durante a configuração utilizando o arquétipo [AEM Maven](project-setup.md).

1. Para visualização dessas dependências, abra o POM do reator pai em **aem-guides-wknd/pom.xml**, navegue até `<dependencies>..</dependencies>` e verifique se as seguintes dependências estão definidas:

   ```xml
   <dependencies>
       ...       
       <!-- Testing -->
       <dependency>
           <groupId>org.junit</groupId>
           <artifactId>junit-bom</artifactId>
           <version>5.6.2</version>
           <type>pom</type>
           <scope>import</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-core</artifactId>
           <version>3.3.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-junit-jupiter</artifactId>
           <version>3.3.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>junit-addons</groupId>
           <artifactId>junit-addons</artifactId>
           <version>1.4</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>io.wcm</groupId>
           <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
           <!-- Prefer the latest version of AEM Mock Junit5 dependency -->
           <version>3.0.2</version>
           <scope>test</scope>
       </dependency>        
       ...
   </dependencies>
   ```

1. Abra **aem-guides-wknd/core/pom.xml** e visualização de que as dependências de teste correspondentes estejam disponíveis:

   ```xml
   ...
   <!-- Testing -->
   <dependency>
       <groupId>org.junit.jupiter</groupId>
       <artifactId>junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-core</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>junit-addons</groupId>
       <artifactId>junit-addons</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <exclusions>
           <exclusion>
               <groupId>org.apache.sling</groupId>
               <artifactId>org.apache.sling.models.impl</artifactId>
           </exclusion>
           <exclusion>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-simple</artifactId>
           </exclusion>
       </exclusions>
       <scope>test</scope>
   </dependency>
   <!-- Required to be able to support injection with @Self and @Via -->
   <dependency>
       <groupId>org.apache.sling</groupId>
       <artifactId>org.apache.sling.models.impl</artifactId>
       <version>1.4.4</version>
       <scope>test</scope>
   </dependency>
   ...
   ```

   Uma pasta de origem paralela no projeto **core** conterá os testes de unidade e quaisquer arquivos de teste de suporte. Essa pasta **test** fornece a separação das classes de teste do código-fonte, mas permite que os testes funcionem como se estivessem nos mesmos pacotes que o código-fonte.

## Criando o teste JUnit {#creating-the-junit-test}

Os testes de unidade normalmente mapeiam de 1 a 1 com classes Java. Neste capítulo, gravaremos um teste JUnit para o **BylineImpl.java**, que é o Modelo Sling que suporta o componente Byline.

![Pasta src de teste de unidade](assets/unit-testing/core-src-test-folder.png)

*Local onde os testes de Unidade são armazenados.*

1. Crie um teste de unidade para `BylineImpl.java`, criando uma nova classe Java em `src/test/java` em uma estrutura de pasta de pacote Java que espelhe a localização da classe Java a ser testada.

   ![Criar um novo arquivo BylineImplTest.java](assets/unit-testing/new-bylineimpltest.png)

   Como estamos testando

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   criar uma classe Java de teste de unidade correspondente em

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

2. Mas também diferencie o arquivo de teste    O sufixo `Test` no arquivo de teste de unidade, `BylineImplTest.java` é uma convenção que nos permite
1. Identifique-o facilmente como o arquivo de teste _para_ `BylineImpl.java`
2. Mas também, diferencie o arquivo de teste _de_ da classe que está sendo testada, `BylineImpl.java`

## Revisando BylineImplTest.java {#reviewing-bylineimpltest-java}

Neste ponto, o arquivo de teste JUnit é uma classe Java vazia. Atualize o arquivo com o seguinte código:

```java
package com.adobe.aem.guides.wknd.core.models.impl;

import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class BylineImplTest {

    @BeforeEach
    void setUp() throws Exception {

    }

    @Test 
    void testGetName() { 
        fail("Not yet implemented");
    }
    
    @Test 
    void testGetOccupations() { 
        fail("Not yet implemented");
    }

    @Test 
    void testIsEmpty() { 
        fail("Not yet implemented");
    }
}
```

1. O primeiro método `public void setUp() { .. }` é anotado com `@BeforeEach` da JUnit, que instrui o executante de teste JUnit a executar este método antes de executar cada método de teste nesta classe. Isso fornece um local útil para inicializar o estado de teste comum exigido por todos os testes.

2. Os métodos subsequentes são os métodos de teste, cujos nomes recebem o prefixo `test` por convenção e são marcados com a anotação `@Test`. Observe que, por padrão, todos os nossos testes estão prontos para falhar, já que ainda não os implementamos.

   Para começar, nós start um único método de teste para cada método público na classe que estamos testando, portanto:

   | BylineImpl.java |  | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | é testado por | testGetName() |
   | getOccupations() | é testado por | testGetOccupations() |
   | isEmpty() | é testado por | testIsEmpty() |

   Esses métodos podem ser expandidos conforme necessário, como veremos mais adiante neste capítulo.

   Quando essa classe de teste JUnit (também conhecida como caso de teste JUnit) é executada, cada método marcado com `@Test` será executado como um teste que pode ser aprovado ou reprovado.

![BylineImplTest gerado](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Execute o JUnit Test Case clicando com o botão direito do mouse no arquivo `BylineImplTest.java` e tocando em **Run**.
Como esperado, todos os testes falharam, já que ainda não foram implementados.

   ![Executar como teste de junção](assets/unit-testing/run-junit-tests.png)

   *Clique com o botão direito do mouse em BylineImplTests.java > Executar*

## Revisando BylineImpl.java {#reviewing-bylineimpl-java}

Ao gravar testes de unidade, há duas abordagens principais:

* [Desenvolvimento](https://en.wikipedia.org/wiki/Test-driven_development) orientado por TDD ou teste, que envolve a gravação gradual dos testes de unidade, imediatamente antes do desenvolvimento da implementação; escreva um teste, escreva a implementação para fazer o teste passar.
* Implementação - primeiro desenvolvimento, que envolve o desenvolvimento de código de trabalho primeiro e, em seguida, a gravação de testes que validam esse código.

Neste tutorial, a última abordagem é usada (já que criamos um **BylineImpl.java** em um capítulo anterior). Por isso, temos de rever e compreender os comportamentos dos seus métodos públicos, mas também alguns dos seus detalhes de implementação. Isto pode parecer contrário, uma vez que um bom teste só deve preocupar-se com os fatores de produção e os resultados, contudo, quando se trabalha em AEM, há uma variedade de considerações de implementação que devem ser compreendidas para se construírem testes de trabalho.

O TDD no contexto do AEM requer um nível de especialização e é melhor adotado por AEM desenvolvedores com capacidade para AEM desenvolvimento e teste de unidade do código AEM.

## Configuração AEM contexto de teste {#setting-up-aem-test-context}

A maioria dos códigos escritos para AEM depende de APIs JCR, Sling ou AEM, que, por sua vez, exigem que o contexto de um AEM em execução seja executado corretamente.

Como os testes de unidade são executados na compilação, fora do contexto de uma instância AEM em execução, não há esse contexto. Para facilitar isso, o AEM Mocks](https://wcm.io/testing/aem-mock/usage.html) de [wcm.io cria um contexto simulado que permite que essas APIs _principalmente_ atuem como se estivessem em execução no AEM.

1. Crie um contexto AEM usando **wcm.io&#39;s** `AemContext` em **BylineImplTest.java** adicionando-o como uma extensão JUnit decorada com `@ExtendWith` no arquivo **BylineImplTest.java**. A extensão cuida de todas as tarefas de inicialização e limpeza necessárias. Crie uma variável de classe para `AemContext` que possa ser usada para todos os métodos de teste.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Essa variável, `ctx`, expõe um modelo AEM contexto que fornece várias abstrações AEM e Sling:

   * O Modelo Sling do BylineImpl será registrado neste contexto
   * As estruturas de conteúdo do JCR Mock são criadas neste contexto
   * Os serviços OSGi personalizados podem ser registrados neste contexto
   * Fornece uma variedade de objetos de maquete e auxiliares comuns necessários, como objetos SlingHttpServletRequest, uma variedade de serviços de mock Sling e AEM OSGi, como ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, TagManager etc.
      * *Observe que nem todos os métodos para esses objetos são implementados!*
   * E [muito mais](https://wcm.io/testing/aem-mock/usage.html)!

   O objeto **`ctx`** atuará como o ponto de entrada para a maioria do contexto simulado.

1. No método `setUp(..)`, que é executado antes de cada método `@Test`, defina um estado de teste de modelo comum:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** registra o Modelo Sling a ser testado, no modelo AEM contexto, para que possa ser instanciado nos  `@Test` métodos.
   * **`load().json`** carrega as estruturas de recursos no contexto simulado, permitindo que o código interaja com esses recursos como se fossem fornecidos por um repositório real. As definições de recursos no arquivo **`BylineImplTest.json`** são carregadas no contexto de mock JCR em **/content**.
   * **`BylineImplTest.json`** ainda não existe, então vamos criá-lo e definir as estruturas de recursos do JCR necessárias para o teste.

1. Os arquivos JSON que representam as estruturas de recurso mock são armazenados em **core/src/test/resources** seguindo o mesmo caminho de pacote que o arquivo de teste Java JUnit.

   Crie um novo arquivo JSON em **core/test/resources/com/adobe/aem/guides/wknd/core/models/impl** chamado **BylineImplTest.json** com o seguinte conteúdo:

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   Este JSON define um recurso mock (nó JCR) para o teste de unidade do componente Byline. Nesse ponto, o JSON tem o conjunto mínimo de propriedades necessário para representar um recurso de conteúdo de componente Byline, os `jcr:primaryType` e `sling:resourceType`.

   Uma regra geral ao trabalhar com testes de unidade é criar o conjunto mínimo de conteúdo de simulação, contexto e código necessário para satisfazer cada teste. Evite a tentação de construir um contexto simulado completo antes de escrever os testes, já que geralmente resulta em artefatos desnecessários.

   Agora, com a existência de **BylineImplTest.json**, quando `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` é executado, as definições de recurso mock são carregadas no contexto no caminho **/content.**

## Testando getName() {#testing-get-name}

Agora que temos uma configuração básica de contexto simulado, vamos escrever nosso primeiro teste para **getName()** do BylineImpl. Este teste deve garantir que o método **getName()** retorne o nome de autor correto armazenado na propriedade &quot;**name&quot;** do recurso.

1. Atualize o método **testGetName**() em **BylineImplTest.java** da seguinte forma:

   ```java
   import com.adobe.aem.guides.wknd.core.components.Byline;
   ...
   @Test
   public void testGetName() {
       final String expected = "Jane Doe";
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       String actual = byline.getName();
   
       assertEquals(expected, actual);
   }
   ```

   * **`String expected`** define o valor esperado. Definiremos como &quot;**Jane Done**&quot;.
   * **`ctx.currentResource`** define o contexto do recurso mock para avaliar o código, de modo que este seja definido como  **/content/** bylineas, pois é onde o recurso de conteúdo mock byline é carregado.
   * **`Byline byline`** instancia o Modelo de divisão em linha adaptando-o do objeto de solicitação de modelagem.
   * **`String actual`** chama o método que estamos testando  `getName()`, no objeto do Modelo de divisão em linha.
   * **`assertEquals`** afirma que o valor esperado corresponde ao valor retornado pelo objeto Sling Model byline. Se esses valores não forem iguais, o teste falhará.

1. Executar o teste... e falha com um `NullPointerException`.

   Observe que este teste NÃO falha porque nunca definimos uma propriedade `name` no teste JSON mock, que fará com que o teste falhe, no entanto a execução do teste não chegou a esse ponto! Esse teste falha devido a um `NullPointerException` no próprio objeto byline.

1. Em `BylineImpl.java`, se `@PostConstruct init()` lançar uma exceção, isso impedirá que o Modelo Sling instancie e fará com que esse objeto do Modelo Sling seja nulo.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Acontece que, embora o serviço ModelFactory OSGi seja fornecido por meio do `AemContext` (por meio do Apache Sling Context), nem todos os métodos são implementados, incluindo `getModelFromWrappedRequest(...)` que é chamado no método `init()` do BylineImpl. Isso resulta em [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html), o que, por termo, faz com que `init()` falhe, e a adaptação resultante de `ctx.request().adaptTo(Byline.class)` é um objeto nulo.

   Como os modelos fornecidos não podem acomodar nosso código, precisamos implementar o contexto simulado nós mesmos. Para isso, podemos usar o Mockito para criar um objeto ModelFactory que retorna um objeto de imagem simulada quando `getModelFromWrappedRequest(...)` é chamado sobre ele.

   Como para instanciar o Modelo de divisão em linha, esse contexto de simulação deve estar em vigor, podemos adicioná-lo ao método `@Before setUp()`. Também precisamos adicionar `MockitoExtension.class` à anotação `@ExtendWith` acima da classe **BylineImplTest**.

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import org.mockito.junit.jupiter.MockitoExtension;
   import org.mockito.Mock;
   
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   
   import org.apache.sling.models.factory.ModelFactory;
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   import org.junit.jupiter.api.extension.ExtendWith;
   
   import static org.junit.jupiter.api.Assertions.*;
   import static org.mockito.Mockito.*;
   import org.apache.sling.api.resource.Resource;
   
   @ExtendWith({ AemContextExtension.class, MockitoExtension.class })
   public class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   
       @Mock
       private Image image;
   
       @Mock
       private ModelFactory modelFactory;
   
       @BeforeEach
       public void setUp() throws Exception {
           ctx.addModelsForClasses(BylineImpl.class);
   
           ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   
           lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()), any(Resource.class), eq(Image.class)))
                   .thenReturn(image);
   
           ctx.registerService(ModelFactory.class, modelFactory, org.osgi.framework.Constants.SERVICE_RANKING,
                   Integer.MAX_VALUE);
       }
   
       @Test
       void testGetName() { ...
   }
   ```

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** marca a classe Test Case (Caso de teste) a ser executada com a  [extensão Jupiter JUnit ](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) Mockito, que permite o uso das anotações @Mock para definir objetos mock no nível Class.
   * **`@Mock private Image`** cria um objeto modelo do tipo  `com.adobe.cq.wcm.core.components.models.Image`. Observe que isso é definido no nível da classe para que, conforme necessário, os métodos `@Test` possam alterar seu comportamento conforme necessário.
   * **`@Mock private ModelFactory`** cria um objeto modelo do tipo ModelFactory. Observe que este é um verdadeiro zombaria Mockito e não tem métodos implementados nele. Observe que isso é definido no nível da classe para que, conforme necessário, os métodos `@Test`possam alterar seu comportamento conforme necessário.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registra o comportamento simulado para quando  `getModelFromWrappedRequest(..)` é chamado no objeto modeloFactory. O resultado definido em `thenReturn (..)` é retornar o objeto de imagem de modelo. Observe que esse comportamento só é invocado quando: o primeiro parâmetro é igual ao objeto request de `ctx`, o segundo parâmetro é qualquer objeto Resource e o terceiro parâmetro deve ser a classe Core Components Image. Aceitamos qualquer Recurso porque, ao longo de nossos testes, estaremos configurando `ctx.currentResource(...)` para vários recursos mock definidos em **BylineImplTest.json**. Observe que adicionamos o rigor **lenient()**, pois posteriormente desejaremos substituir esse comportamento do ModelFactory.
   * **`ctx.registerService(..)`.** registra o objeto modeloFactory no AemContext, com a classificação de serviço mais alta. Isso é necessário, pois ModelFactory usado em `init()` do BylineImpl é injetado pelo campo `@OSGiService ModelFactory model`. Para que o AemContext injete **our** objeto mock, que lida com chamadas para `getModelFromWrappedRequest(..)`, devemos registrá-lo como o Serviço de classificação mais alta desse tipo (ModelFactory).

1. Execute novamente o teste e, novamente, ele falhará, mas desta vez a mensagem é clara por que ele falhou.

   ![declaração de falha de nome de teste](assets/unit-testing/testgetname-failure-assertion.png)

   *falha testGetName() devido à asserção*

   Recebemos um **AssertionError** que significa que a condição de asserção no teste falhou, e nos informa que o **valor esperado é &quot;Jane Doe&quot;**, mas o valor real **é nulo**. Isso faz sentido, pois a propriedade &quot;**name&quot;** não foi adicionada à definição de recurso mock **/content/byline** em **BylineImplTest.json**, por isso vamos adicioná-la:

1. Atualize **BylineImplTest.json** para definir `"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. Execute novamente o teste e **`testGetName()`** agora passa!

   ![aprovação do nome de teste](assets/unit-testing/testgetname-pass.png)


## Testando getOccupations() {#testing-get-occupations}

Ok, ótimo! Nosso primeiro teste foi bem-sucedido! Vamos continuar e testar `getOccupations()`. Como a inicialização do contexto do modelo foi feita no método `@Before setUp()`, isso estará disponível para todos os métodos `@Test` neste caso de teste, incluindo `getOccupations()`.

Lembre-se de que esse método deve retornar uma lista ordenada alfabeticamente de ocupações (descendentes) armazenada na propriedade de ocupações.

1. Atualize **`testGetOccupations()`** da seguinte forma:

   ```java
   import java.util.List;
   import com.google.common.collect.ImmutableList;
   ...
   @Test
   public void testGetOccupations() {
       List<String> expected = new ImmutableList.Builder<String>()
                               .add("Blogger")
                               .add("Photographer")
                               .add("YouTuber")
                               .build();
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       List<String> actual = byline.getOccupations();
   
       assertEquals(expected, actual);
   }
   ```

   * **`List<String> expected`** defina o resultado esperado.
   * **`ctx.currentResource`** define o recurso atual para avaliar o contexto com base na definição do recurso de mock em /content/byline. Isso garante que o **BylineImpl.java** seja executado no contexto do nosso recurso mock.
   * **`ctx.request().adaptTo(Byline.class)`** instancia o Modelo de divisão em linha adaptando-o do objeto de solicitação de modelagem.
   * **`byline.getOccupations()`** chama o método que estamos testando  `getOccupations()`, no objeto do Modelo de divisão em linha.
   * **`assertEquals(expected, actual)`** afirma que a lista esperada é igual à lista real.

1. Lembre-se, assim como **`getName()`** acima, o **BylineImplTest.json** não define ocupações, portanto, esse teste falhará se for executado, já que `byline.getOccupations()` retornará uma lista vazia.

   Atualize **BylineImplTest.json** para incluir uma lista de ocupações e elas serão definidas em ordem não alfabética para garantir que nossos testes validem se as ocupações são classificadas alfabeticamente por **`getOccupations()`**.

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe",
       "occupations": ["Photographer", "Blogger", "YouTuber"]
       }
   }
   ```

1. Executar o teste e nós passamos novamente! Parece que as ocupações organizadas funcionam!

   ![Obter o passe de Ocupações](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations() passa*

## Testando isEmpty() {#testing-is-empty}

O último método para testar **`isEmpty()`**.

Testar `isEmpty()` é interessante, pois requer testes para uma variedade de condições. Revisando o método **BylineImpl.java** de `isEmpty()`, as seguintes condições devem ser testadas:

* Retornar true quando o nome estiver vazio
* Retornar true quando as ocupações forem nulas ou vazias
* Retorna true quando a imagem for nula ou não tiver um URL src
* Retornar falso quando o nome, as ocupações e a Imagem (com um URL src) estiverem presentes

Para isso, precisamos criar novos métodos de teste, cada um testando uma condição específica, bem como novas estruturas de recursos de maquete em `BylineImplTest.json` para conduzir esses testes.

Observe que essa verificação nos permitiu ignorar o teste para quando `getName()`, `getOccupations()` e `getImage()` estão vazios, já que o comportamento esperado desse estado é testado via `isEmpty()`.

1. O primeiro teste testará a condição de um novo componente, que não tem propriedades definidas.

   Adicione uma nova definição de recurso a `BylineImplTest.json`, dando-lhe o nome semântico &quot;**empty**&quot;

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   **`"empty": {...}`** defina uma nova definição de recurso chamada &quot;vazio&quot; que tenha somente um  `jcr:primaryType` e  `sling:resourceType`.

   Lembre-se de carregarmos `BylineImplTest.json` em `ctx` antes da execução de cada método de teste em `@setUp`, de modo que esta nova definição de recurso está imediatamente disponível para nós em testes em **/content/empty.**

1. Atualize `testIsEmpty()` da seguinte forma, definindo o recurso atual para a nova definição de recurso de zombaria &quot;**empty**&quot;.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Execute o teste e verifique se ele foi aprovado.

1. Em seguida, crie um conjunto de métodos para garantir que, se algum dos pontos de dados necessários (nome, ocupações ou imagem) estiver vazio, `isEmpty()` retorne true.

   Para cada teste, é usada uma definição discreta de recurso mock, atualize **BylineImplTest.json** com as definições adicionais de recurso para **without-name** e **without-ocuations**.

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       },
       "without-name": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "occupations": "[Photographer, Blogger, YouTuber]"
       },
       "without-occupations": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe"
       }
   }
   ```

   Crie os seguintes métodos de teste para testar cada um desses estados.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutName() {
       ctx.currentResource("/content/without-name");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutOccupations() {
       ctx.currentResource("/content/without-occupations");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImage() {
       ctx.currentResource("/content/byline");
   
       lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()),
           any(Resource.class),
           eq(Image.class))).thenReturn(null);
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImageSrc() {
       ctx.currentResource("/content/byline");
   
       when(image.getSrc()).thenReturn("");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   **`testIsEmpty()`** faz testes em relação à definição vazia do recurso de maquete e afirma que isso  `isEmpty()` é verdade.

   **`testIsEmpty_WithoutName()`** testa uma definição de recurso simulada que tem ocupações, mas nenhum nome.

   **`testIsEmpty_WithoutOccupations()`** testa uma definição de recurso simulada que tem um nome, mas não ocupa.

   **`testIsEmpty_WithoutImage()`** faz testes em uma definição de recurso simulada com um nome e ocupações, mas define a imagem simulada para retornar a nulo. Observe que queremos substituir o comportamento `modelFactory.getModelFromWrappedRequest(..)`definido em `setUp()` para garantir que o objeto de imagem retornado por esta chamada seja nulo. O recurso de bordas Mockito é restrito e não quer código duplicado. Portanto, definimos o modelo com as configurações **`lenient`** para observar explicitamente que estamos substituindo o comportamento no método `setUp()`.

   **`testIsEmpty_WithoutImageSrc()`** faz testes em uma definição de recurso de simulação com um nome e ocupações, mas define a Imagem de simulação para retornar uma string em branco quando  `getSrc()` é chamada.

1. Por fim, escreva um teste para garantir que **isEmpty()** retorne false quando o componente estiver configurado corretamente. Para essa condição, podemos reutilizar **/content/byline**, que representa um componente Byline totalmente configurado.

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. Agora, execute todos os testes de unidade no arquivo BylineImplTest.java e reveja a saída do Relatório de teste Java.

![Todos os testes foram bem-sucedidos](./assets/unit-testing/all-tests-pass.png)

## Execução de testes de unidade como parte da compilação {#running-unit-tests-as-part-of-the-build}

Os testes de unidade são executados para serem aprovados como parte da construção de maven. Isso garante que todos os testes sejam bem-sucedidos antes da implantação de um aplicativo. A execução de objetivos Maven, como pacote ou instalação, chama automaticamente e requer a aprovação de todos os testes de unidade no projeto.

```shell
$ mvn package
```

![sucesso do pacote mvn](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

Da mesma forma, se mudarmos um método de teste para falhar, a construção falhará e reportará quais testes falharam e por quê.

![falha no pacote mvn](assets/unit-testing/mvn-package-fail.png)

## Revise o código {#review-the-code}

Visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente no bloco Git `tutorial/unit-testing-solution`.
