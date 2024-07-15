---
title: Teste de unidade
description: Implemente um teste de unidade que valide o comportamento do Modelo Sling do componente Subtítulo, criado no tutorial Componente personalizado.
version: 6.5, Cloud Service
feature: APIs, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4089
mini-toc-levels: 1
thumbnail: 30207.jpg
doc-type: Tutorial
exl-id: b926c35e-64ad-4507-8b39-4eb97a67edda
recommendations: noDisplay, noCatalog
duration: 706
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2923'
ht-degree: 0%

---

# Teste de unidade {#unit-testing}

Este tutorial aborda a implementação de um teste de unidade que valida o comportamento do Modelo Sling do componente Subtítulo, criado no tutorial [Componente personalizado](./custom-component.md).

## Pré-requisitos {#prerequisites}

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

_Se o Java™ 8 e o Java™ 11 estiverem instalados no sistema, o executor de teste do VS Code poderá escolher o menor tempo de execução do Java™ ao executar os testes, resultando em falhas de teste. Se isso ocorrer, desinstale o Java™ 8._

### Projeto inicial

>[!NOTE]
>
> Se você concluiu com êxito o capítulo anterior, é possível reutilizar o projeto e ignorar as etapas para verificar o projeto inicial.

Verifique o código de linha base no qual o tutorial se baseia:

1. Confira a ramificação `tutorial/unit-testing-start` do [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Implante a base de código em uma instância de AEM local usando suas habilidades de Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando AEM 6.5 ou 6.4, anexe o perfil `classic` a qualquer comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) ou conferir o código localmente alternando para a ramificação `tutorial/unit-testing-start`.

## Objetivo

1. Entenda as noções básicas de teste de unidade.
1. Saiba mais sobre estruturas e ferramentas comumente usadas para testar o código do AEM.
1. Entenda as opções para zombar ou simular recursos de AEM ao gravar testes de unidade.

## Fundo {#unit-testing-background}

Neste tutorial, exploraremos como gravar [Testes de Unidade](https://en.wikipedia.org/wiki/Unit_testing) para o [Modelo Sling](https://sling.apache.org/documentation/bundles/models.html) do nosso componente Subtítulo (criado no [Criação de um componente AEM personalizado](custom-component.md)). Testes de unidade são testes de tempo de compilação escritos em Java™ que verificam o comportamento esperado do código Java™. Cada teste de unidade é normalmente pequeno e valida a saída de um método (ou unidades de trabalho) em relação aos resultados esperados.

Usamos as práticas recomendadas de AEM e empregamos:

* [JUnit 5](https://junit.org/junit5/)
* [Estrutura de teste Mockito](https://site.mockito.org/)
* [wcm.io Test Framework](https://wcm.io/testing/) (que se baseia em [Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html))

## Teste de unidade e Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

O [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/introduction.html?lang=pt-BR) integra a execução de teste de unidade e os [relatórios de cobertura de código](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html) ao seu pipeline de CI/CD para ajudar a incentivar e promover a prática recomendada do código AEM de teste de unidade.

Embora o código de teste de unidade seja uma boa prática para qualquer base de código, ao usar o Cloud Manager é importante aproveitar seus recursos de relatórios e testes de qualidade de código, fornecendo testes de unidade para o Cloud Manager executar.

## Atualizar as dependências do Maven de teste {#inspect-the-test-maven-dependencies}

A primeira etapa é inspecionar as dependências do Maven para oferecer suporte à gravação e execução dos testes. São necessárias quatro dependências:

1. JUnit5
1. Estrutura de teste Mockito
1. Apache Sling Mocks
1. Estrutura de teste AEM Mocks (by io.wcm)

As dependências de teste **JUnit5**, **Mockito e **AEM Mocks** são adicionadas automaticamente ao projeto durante a instalação usando o [arquétipo Maven AEM](project-setup.md).

1. Para exibir essas dependências, abra o POM do Reator Pai em **aem-guides-wknd/pom.xml**, navegue até `<dependencies>..</dependencies>` e exiba as dependências de Testes de Mock do AEM, Mockito, Apache Sling Mocks e JUnit por io.wcm em `<!-- Testing -->`.
1. Verifique se `io.wcm.testing.aem-mock.junit5` está definido como **4.1.0**:

   ```xml
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <version>4.1.0</version>
       <scope>test</scope>
   </dependency>
   ```

   >[!CAUTION]
   >
   > O arquétipo **35** gera o projeto com `io.wcm.testing.aem-mock.junit5` versão **4.1.8**. Faça o downgrade para **4.1.0** para seguir o restante deste capítulo.

1. Abra **aem-guides-wknd/core/pom.xml** e veja se as dependências de teste correspondentes estão disponíveis.

   Uma pasta de origem paralela no projeto **core** conterá os testes de unidade e quaisquer arquivos de teste de suporte. Esta pasta **test** fornece separação de classes de teste do código-fonte, mas permite que os testes atuem como se estivessem nos mesmos pacotes que o código-fonte.

## Criando o teste JUnit {#creating-the-junit-test}

Testes de unidade normalmente mapeiam 1-para-1 com classes Java™. Neste capítulo, escreveremos um teste JUnit para o **BylineImpl.java**, que é o Modelo Sling que dá suporte ao componente Byline.

![Pasta src de teste de unidade](assets/unit-testing/core-src-test-folder.png)

*Local onde os testes de Unidade estão armazenados.*

1. Crie um teste de unidade para o `BylineImpl.java` fazendo uma nova classe Java™ em `src/test/java` em uma estrutura de pasta de pacote Java™ que espelha o local da classe Java™ a ser testada.

   ![Criar um novo arquivo BylineImplTest.java](assets/unit-testing/new-bylineimpltest.png)

   Já que estamos testando

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   criar uma classe de Java™ de teste de unidade correspondente em

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   O sufixo `Test` no arquivo de teste unitário, `BylineImplTest.java` é uma convenção, que nos permite

   1. Identifique facilmente como o arquivo de teste _para_ `BylineImpl.java`
   1. Mas também diferencie o arquivo de teste _de_ a classe que está sendo testada, `BylineImpl.java`

## Revisão de BylineImplTest.java {#reviewing-bylineimpltest-java}

Neste ponto, o arquivo de teste JUnit é uma classe Java™ vazia.

1. Atualize o arquivo com o seguinte código:

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

1. O primeiro método `public void setUp() { .. }` é anotado com o `@BeforeEach` da JUnit, que instrui o executor de teste JUnit a executar este método antes de executar cada método de teste nesta classe. Isso fornece um local útil para inicializar o estado de teste comum exigido por todos os testes.

1. Os métodos subsequentes são os métodos de teste, cujos nomes são prefixados com `test` por convenção e marcados com a anotação `@Test`. Observe que, por padrão, todos os nossos testes estão definidos como reprovados, pois ainda não os implementamos.

   Para começar, começamos com um único método de teste para cada método público na classe que estamos testando, portanto:

   | BylineImpl.java |              | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | é testado por | testGetName() |
   | getOccupations() | é testado por | testGetOccupations() |
   | isEmpty() | é testado por | testIsEmpty() |

   Esses métodos podem ser expandidos conforme necessário, conforme veremos mais adiante neste capítulo.

   Quando esta classe de teste JUnit (também conhecida como Caso de Teste JUnit) é executada, cada método marcado com o `@Test` será executado como um teste que pode ser aprovado ou reprovado.

![BylineImplTest gerado](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Execute o caso de teste JUnit clicando com o botão direito do mouse no arquivo `BylineImplTest.java` e tocando em **Executar**.
Como esperado, todos os testes falham, pois ainda não foram implementados.

   ![Executar como teste junit](assets/unit-testing/run-junit-tests.png)

   *Clique com o botão direito em BylineImplTests.java > Executar*

## Revisão de BylineImpl.java {#reviewing-bylineimpl-java}

Ao escrever testes de unidade, há duas abordagens principais:

* [TDD ou Desenvolvimento Controlado por Teste](https://en.wikipedia.org/wiki/Test-driven_development), que envolve gravar os testes de unidade de forma incremental, imediatamente antes da implementação ser desenvolvida; gravar um teste, gravar a implementação para fazer o teste passar.
* Desenvolvimento de implementação primeiro, que envolve o desenvolvimento de código de trabalho primeiro e, em seguida, a gravação de testes que validam esse código.

Neste tutorial, a última abordagem é usada (já que criamos um **BylineImpl.java** de trabalho em um capítulo anterior). Por causa disso, devemos revisar e entender os comportamentos de seus métodos públicos, mas também alguns de seus detalhes de implementação. Isso pode parecer contrário, uma vez que um bom teste só deve se preocupar com as entradas e saídas, no entanto, quando se trabalha com AEM, há várias considerações de implementação que são necessárias para ser entendida a fim de construir testes de trabalho.

O TDD no contexto do AEM requer um nível de experiência e é mais bem adotado por desenvolvedores do AEM proficientes em desenvolvimento do AEM e teste de unidade do código do AEM.

## Configuração do contexto de teste de AEM  {#setting-up-aem-test-context}

A maioria dos códigos escritos para AEM depende de APIs JCR, Sling ou AEM, que, por sua vez, exigem o contexto de uma execução correta do AEM.

Como os testes de unidade são executados na criação, fora do contexto de uma instância AEM em execução, esse contexto não existe. Para facilitar isso, o AEM Mocks](https://wcm.io/testing/aem-mock/usage.html) do [wcm.io cria um contexto simulado que permite que essas APIs _na maioria_ atuem como se estivessem sendo executadas no AEM.

1. Crie um contexto AEM usando **wcm.io** `AemContext` em **BylineImplTest.java** adicionando-o como uma extensão JUnit decorada com `@ExtendWith` ao arquivo **BylineImplTest.java**. A extensão cuida de todas as tarefas de inicialização e limpeza necessárias. Crie uma variável de classe para `AemContext` que possa ser usada para todos os métodos de teste.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Essa variável, `ctx`, expõe um contexto de AEM simulado que fornece algumas abstrações de AEM e Sling:

   * O modelo Sling BylineImpl está registrado neste contexto
   * Estruturas de conteúdo JCR fictícias são criadas neste contexto
   * Os serviços OSGi personalizados podem ser registrados neste contexto
   * Fornece vários objetos de modelo e auxiliares comuns necessários, como objetos SlingHttpServletRequest, vários serviços de OSGi de Sling e AEM como ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, Tag etc.
      * *Nem todos os métodos para estes objetos foram implementados!*
   * E [muito mais](https://wcm.io/testing/aem-mock/usage.html)!

   O objeto **`ctx`** atuará como ponto de entrada para a maioria de nosso contexto fictício.

1. No método `setUp(..)`, que é executado antes de cada método `@Test`, defina um estado de teste de modelo comum:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * O **`addModelsForClasses`** registra o Modelo do Sling a ser testado no Contexto de AEM de modelo, para que ele possa ser instanciado nos métodos `@Test`.
   * O **`load().json`** carrega estruturas de recursos no contexto fictício, permitindo que o código interaja com esses recursos como se eles fossem fornecidos por um repositório real. As definições de recurso no arquivo **`BylineImplTest.json`** são carregadas no contexto JCR fictício em **/content**.
   * **`BylineImplTest.json`** ainda não existe, então vamos criá-lo e definir as estruturas de recursos JCR necessárias para o teste.

1. Os arquivos JSON que representam as estruturas de recurso de modelo são armazenados em **core/src/test/resources** seguindo o mesmo caminho de pacote que o arquivo de teste JUnit Java™.

   Crie um arquivo JSON em `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl` chamado **BylineImplTest.json** com o seguinte conteúdo:

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   Esse JSON define um recurso de modelo (nó JCR) para o teste de unidade do componente Linha de bytes. Neste ponto, o JSON tem o conjunto mínimo de propriedades necessárias para representar um recurso de conteúdo do componente Subtítulo, o `jcr:primaryType` e `sling:resourceType`.

   Uma regra geral ao trabalhar com testes de unidade é criar o conjunto mínimo de conteúdo de modelo, contexto e código necessário para satisfazer cada teste. Evite a tentação de criar um contexto fictício completo antes de escrever os testes, pois isso geralmente resulta em artefatos desnecessários.

   Agora com a existência de **BylineImplTest.json**, quando `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` é executado, as definições de recursos simulados são carregadas no contexto no caminho **/content.**

## Testando getName() {#testing-get-name}

Agora que temos uma configuração básica de contexto de modelo, vamos escrever nosso primeiro teste para getName()**de** BylineImpl. Este teste deve garantir que o método **getName()** retorne o nome criado corretamente armazenado na propriedade &quot;**name&quot;** do recurso.

1. Atualize o método **testGetName**() em **BylineImplTest.java** da seguinte maneira:

   ```java
   import com.adobe.aem.guides.wknd.core.models.Byline;
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

   * **`String expected`** define o valor esperado. Definiremos como &quot;**Jane Concluída**&quot;.
   * **`ctx.currentResource`** define o contexto do recurso de modelo para avaliar o código em relação a ele, portanto, está definido como **/content/byline**, pois é aí que o recurso de conteúdo de byline de modelo é carregado.
   * **`Byline byline`** instancia o Modelo de Sling Byline adaptando-o do objeto de Solicitação simulada.
   * **`String actual`** invoca o método que estamos testando, `getName()`, no objeto Modelo Sling de Subtítulo.
   * **`assertEquals`** afirma que o valor esperado corresponde ao valor retornado pelo objeto Sling Model de byline. Se esses valores não forem iguais, o teste falhará.

1. Execute o teste... e ele falhará com um `NullPointerException`.

   Este teste NÃO falha porque nunca definimos uma propriedade `name` no JSON fictício, que fará com que o teste falhe, no entanto, a execução do teste não chegou a esse ponto! Este teste falha devido a um `NullPointerException` no próprio objeto byline.

1. No `BylineImpl.java`, se `@PostConstruct init()` acionar uma exceção, ela impedirá que o Modelo Sling seja instanciado e fará com que esse objeto de Modelo Sling seja nulo.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Acontece que, embora o serviço OSGi ModelFactory seja fornecido por meio do `AemContext` (por meio do Apache Sling Context), nem todos os métodos são implementados, incluindo `getModelFromWrappedRequest(...)`, que é chamado no método `init()` do BylineImpl. Isso resulta em um [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html), que no termo causa falha em `init()`, e a adaptação resultante do `ctx.request().adaptTo(Byline.class)` é um objeto nulo.

   Como os mocks fornecidos não podem acomodar nosso código, devemos implementar o contexto do mock por conta própria. Para isso, podemos usar o Mockito para criar um objeto ModelFactory simulado, que retorna um objeto Image simulado quando `getModelFromWrappedRequest(...)` é invocado sobre ele.

   Como para instanciar o Modelo Sling de byline, esse contexto de modelo deve estar em vigor, podemos adicioná-lo ao método `@Before setUp()`. Também precisamos adicionar `MockitoExtension.class` à anotação `@ExtendWith` acima da classe **BylineImplTest**.

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** marca a classe de Caso de Teste a ser executada com a [Extensão de Júpiter JUnit Mockito](https://www.javadoc.io/static/org.mockito/mockito-junit-jupiter/4.11.0/org/mockito/junit/jupiter/MockitoExtension.html) que permite o uso das anotações @Mock para definir objetos simulados no nível de Classe.
   * **`@Mock private Image`** cria um objeto fictício do tipo `com.adobe.cq.wcm.core.components.models.Image`. Isso é definido no nível da classe para que, conforme necessário, os métodos `@Test` possam alterar seu comportamento conforme necessário.
   * **`@Mock private ModelFactory`** cria um objeto fictício do tipo ModelFactory. Esta é uma simulação pura de Mockito e não tem métodos implementados nela. Isso é definido no nível da classe para que, conforme necessário, os métodos `@Test` possam alterar seu comportamento conforme necessário.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registra o comportamento de modelo para quando `getModelFromWrappedRequest(..)` é chamado no objeto ModelFactory de modelo. O resultado definido em `thenReturn (..)` é retornar o objeto Image simulado. Esse comportamento só é chamado quando: o primeiro parâmetro é igual ao objeto de solicitação de `ctx`, o segundo parâmetro é qualquer objeto de Recurso e o terceiro parâmetro deve ser a classe de Imagem dos Componentes principais. Aceitamos qualquer Recurso porque em todos os nossos testes estamos configurando o `ctx.currentResource(...)` para vários recursos simulados definidos no **BylineImplTest.json**. Observe que adicionamos a rigidez **lenient()** porque desejaremos substituir esse comportamento de ModelFactory posteriormente.
   * **`ctx.registerService(..)`.** registra o objeto ModelFactory fictício no AemContext, com a classificação de serviço mais alta. Isso é necessário porque o ModelFactory usado no `init()` do BylineImpl é inserido por meio do campo `@OSGiService ModelFactory model`. Para que o AemContext injete o objeto fictício **our**, que manipula chamadas para `getModelFromWrappedRequest(..)`, devemos registrá-lo como o Serviço de classificação mais alta desse tipo (ModelFactory).

1. Execute novamente o teste e, novamente, ele falhará, mas desta vez a mensagem ficará clara por que ele falhou.

   ![asserção de falha de nome de teste](assets/unit-testing/testgetname-failure-assertion.png)

   Falha de *testGetName() devido à asserção*

   Recebemos um **AssertionError**, que significa que a condição de declaração no teste falhou e nos informa que o **valor esperado é &quot;Jane Doe&quot;**, mas o **valor real é nulo**. Isso faz sentido porque a propriedade &quot;**name&quot;** não foi adicionada à definição de recurso fictícia **/content/byline** em **BylineImplTest.json**, então vamos adicioná-la:

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

1. Execute o teste novamente e **`testGetName()`** agora está aprovado!

   ![passe do nome do teste](assets/unit-testing/testgetname-pass.png)


## Teste de getOccupations() {#testing-get-occupations}

Ok, ótimo! O primeiro teste foi bem-sucedido! Vamos continuar e testar `getOccupations()`. Como a inicialização do contexto de modelo foi feita no método `@Before setUp()`, isso está disponível para todos os métodos `@Test` neste Caso de Teste, incluindo `getOccupations()`.

Lembre-se de que esse método deve retornar uma lista de ocupações classificada alfabeticamente (decrescente) armazenada na propriedade ocupações.

1. Atualize **`testGetOccupations()`** da seguinte maneira:

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
   * **`ctx.currentResource`** define o recurso atual para avaliar o contexto em relação à definição de recurso fictício em /content/byline. Isso garante que o **BylineImpl.java** seja executado no contexto de nosso recurso fictício.
   * **`ctx.request().adaptTo(Byline.class)`** instancia o Modelo de Sling Byline adaptando-o do objeto de Solicitação simulada.
   * **`byline.getOccupations()`** invoca o método que estamos testando, `getOccupations()`, no objeto Modelo Sling de Subtítulo.
   * **`assertEquals(expected, actual)`** afirmações que a lista esperada é a mesma que a lista real.

1. Lembre-se, assim como **`getName()`** acima, o **BylineImplTest.json** não define ocupações. Portanto, este teste falhará se o executarmos, já que `byline.getOccupations()` retornará uma lista vazia.

   Atualize **BylineImplTest.json** para incluir uma lista de ocupações, e elas são definidas em ordem não alfabética para garantir que nossos testes validem se as ocupações são classificadas alfabeticamente por **`getOccupations()`**.

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

1. Faça o teste, e novamente nós passamos! Parece que as ocupações ordenadas funcionam!

   ![Obter passagem de Ocupações](assets/unit-testing/testgetoccupations-pass.png)

   *passagens de {testGetOccupations()*

## Testando isEmpty() {#testing-is-empty}

O último método para testar **`isEmpty()`**.

Testar `isEmpty()` é interessante, pois requer testar várias condições. Ao revisar o método `isEmpty()` de **BylineImpl.java**, as seguintes condições devem ser testadas:

* Retorna verdadeiro quando o nome está vazio
* Retorna verdadeiro quando as ocupações são nulas ou vazias
* Retorna verdadeiro quando a imagem é nula ou não tem URL src
* Retorna falso quando o nome, as ocupações e a Imagem (com um URL src) estiverem presentes

Para isso, precisamos criar métodos de teste, cada um testando uma condição específica e novas estruturas de recursos simulados em `BylineImplTest.json` para conduzir esses testes.

Esta verificação nos permitiu ignorar o teste para quando `getName()`, `getOccupations()` e `getImage()` estão vazios, pois o comportamento esperado desse estado é testado via `isEmpty()`.

1. O primeiro teste testará a condição de um componente totalmente novo, que não tem propriedades definidas.

   Adicione uma nova definição de recurso a `BylineImplTest.json`, dando a ele o nome semântico &quot;**empty**&quot;

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

   **`"empty": {...}`** defina uma nova definição de recurso chamada &quot;vazio&quot; que tenha somente `jcr:primaryType` e `sling:resourceType`.

   Lembre-se de que carregamos `BylineImplTest.json` em `ctx` antes da execução de cada método de teste em `@setUp`. Portanto, essa nova definição de recurso está imediatamente disponível para nós em testes em **/content/empty.**

1. Atualize `testIsEmpty()` da seguinte maneira, definindo o recurso atual para a nova definição de recurso de modelo &quot;**vazio**&quot;.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Execute o teste e verifique se ele é aprovado.

1. Em seguida, crie um conjunto de métodos para garantir que, se qualquer um dos pontos de dados necessários (nome, ocupações ou imagem) estiver vazio, `isEmpty()` retornará true.

   Para cada teste, uma definição de recurso de modelo discreta é usada, atualize **BylineImplTest.json** com as definições de recurso adicionais para **sem-nome** e **sem-ocupações**.

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

   Crie os métodos de teste a seguir para testar cada um desses estados.

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

   **`testIsEmpty()`** testa com a definição de recurso de modelo vazia e declara que `isEmpty()` é verdadeiro.

   **`testIsEmpty_WithoutName()`** testa contra uma definição de recurso fictício que tem ocupações, mas nenhum nome.

   **`testIsEmpty_WithoutOccupations()`** testa contra uma definição de recurso fictício que tem um nome mas nenhuma ocupação.

   **`testIsEmpty_WithoutImage()`** testa uma definição de recurso de modelo com um nome e ocupações, mas define a Imagem de modelo para retornar como nulo. Observe que queremos substituir o comportamento `modelFactory.getModelFromWrappedRequest(..)` definido em `setUp()` para garantir que o objeto Image retornado por esta chamada seja nulo. O recurso de stubs Mockito é estrito e não deseja código duplicado. Portanto, definimos o modelo com configurações **`lenient`** para observar explicitamente que estamos substituindo o comportamento no método `setUp()`.

   **`testIsEmpty_WithoutImageSrc()`** testa uma definição de recurso simulado com um nome e ocupações, mas define o modelo Imagem para retornar uma sequência de caracteres em branco quando `getSrc()` é invocado.

1. Por fim, escreva um teste para garantir que **isEmpty()** retorne false quando o componente estiver configurado corretamente. Para esta condição, podemos reutilizar **/content/byline**, que representa um componente Byline totalmente configurado.

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. Agora execute todos os testes de unidade no arquivo BylineImplTest.java e revise a saída do Relatório de teste Java™.

![Todos os testes foram bem-sucedidos](./assets/unit-testing/all-tests-pass.png)

## Execução de testes de unidade como parte da compilação {#running-unit-tests-as-part-of-the-build}

Os testes de unidade são executados e devem ser aprovados como parte da compilação maven. Isso garante que todos os testes sejam bem-sucedidos antes da implantação de um aplicativo. A execução de metas do Maven, como pacote ou instalação, chama automaticamente e requer a aprovação de todos os testes de unidade no projeto.

```shell
$ mvn package
```

![êxito do pacote mvn](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

Da mesma forma, se alterarmos um método de teste para falha, a build falhará e relatará qual teste falhou e por quê.

![Falha no pacote mvn](assets/unit-testing/mvn-package-fail.png)

## Revisar o código {#review-the-code}

Visualize o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente em na ramificação Git `tutorial/unit-testing-solution`.
