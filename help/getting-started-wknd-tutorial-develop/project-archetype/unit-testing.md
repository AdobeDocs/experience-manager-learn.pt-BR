---
title: Teste de unidade
description: Este tutorial aborda a implementação de um Teste de unidade que valida o comportamento do Modelo do Sling do componente Byline, criado no tutorial Componente personalizado .
sub-product: sites
version: 6.5, Cloud Service
type: Tutorial
feature: APIs, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
exl-id: b926c35e-64ad-4507-8b39-4eb97a67edda
source-git-commit: df9ff5e6811d35118d1beee6baaffa51081cb3c3
workflow-type: tm+mt
source-wordcount: '3025'
ht-degree: 0%

---

# Teste de unidade {#unit-testing}

Este tutorial aborda a implementação de um Teste de Unidade que valida o comportamento do Modelo Sling do componente Byline, criado no [Componente personalizado](./custom-component.md) tutorial.

## Pré-requisitos {#prerequisites}

Revise as ferramentas necessárias e as instruções para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

_Se o Java 8 e o Java 11 estiverem instalados no sistema, o executante do teste do Código VS poderá escolher o tempo de execução do Java mais baixo ao executar os testes, resultando em falhas de teste. Se isso ocorrer, desinstale o Java 8._

### Projeto inicial

>[!NOTE]
>
> Se você concluiu o capítulo anterior com êxito, é possível reutilizar o projeto e ignorar as etapas para verificar o projeto inicial.

Confira o código base que o tutorial constrói em:

1. Confira o `tutorial/unit-testing-start` ramificação de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Implante a base de código em uma instância de AEM local usando suas habilidades Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando AEM 6.5 ou 6.4, anexe a `classic` para qualquer comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) ou verifique o código localmente, alternando para a ramificação `tutorial/unit-testing-start`.

## Objetivo

1. Entenda as noções básicas do teste de unidade.
1. Saiba mais sobre estruturas e ferramentas usadas frequentemente para testar o código AEM.
1. Entenda as opções para fazer zombaria ou simular AEM recursos ao gravar testes de unidade.

## Segundo plano {#unit-testing-background}

Neste tutorial, exploraremos como escrever [Testes de unidade](https://en.wikipedia.org/wiki/Unit_testing) para o componente Byline [Modelo Sling](https://sling.apache.org/documentation/bundles/models.html) (criado na [Criação de um componente de AEM personalizado](custom-component.md)). Os testes de unidade são testes de tempo de criação gravados em Java que verificam o comportamento esperado do código Java. Normalmente, cada teste de unidade é pequeno e valida a saída de um método (ou unidades de trabalho) em relação aos resultados esperados.

Usaremos AEM práticas recomendadas e:

* [JUnit 5](https://junit.org/junit5/)
* [Estrutura de teste do Mockito](https://site.mockito.org/)
* [Estrutura de teste do wcm.io](https://wcm.io/testing/) (com base em [Mocks do Apache Sling](https://sling.apache.org/documentation/development/sling-mock.html))

## Teste de unidade e gerenciador de nuvem do Adobe {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=pt-BR) integra a execução do teste de unidade e [relatório de cobertura de código](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) no seu pipeline de CI/CD para ajudar a incentivar e promover as melhores práticas de teste de unidade AEM código.

Embora o código de teste de unidade seja uma boa prática para qualquer base de código, ao usar o Cloud Manager, é importante aproveitar seus recursos de teste e relatório de qualidade de código fornecendo testes de unidade para que o Cloud Manager seja executado.

## Atualizar as dependências de teste do Maven {#inspect-the-test-maven-dependencies}

O primeiro passo é inspecionar as dependências de Maven para suportar a gravação e a execução dos testes. Há quatro dependências necessárias:

1. JUnit5
1. Estrutura de teste do Mockito
1. Mocks do Apache Sling
1. Estrutura de teste do AEM Mocks (por io.wcm)

O **JUnit5**, **Mockito** e **AEM Mocks** as dependências de teste são adicionadas automaticamente ao projeto durante a configuração usando o [AEM arquétipo Maven](project-setup.md).

1. Para exibir essas dependências, abra o POM do reator pai em **aem-guides-wknd/pom.xml**, navegue até o `<dependencies>..</dependencies>` e visualize as dependências para JUnit, Mockito, Mocks do Apache Sling e Testes de Mock AEM por io.wcm em `<!-- Testing -->`.
1. Certifique-se de que `io.wcm.testing.aem-mock.junit5` está definida como **4.1.0**:

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
   > Arquétipo **35º** gera o projeto com `io.wcm.testing.aem-mock.junit5` version **4.1.8**. Faça o download para **4.1.0** para seguir o restante deste capítulo.

1. Abrir **aem-guides-wknd/core/pom.xml** e veja se as dependências de teste correspondentes estão disponíveis.

   Uma pasta de origem paralela no **núcleo** projeto conterá os testes de unidade e quaisquer arquivos de teste de suporte. Essa **teste** A pasta fornece a separação das classes de teste do código-fonte, mas permite que os testes atuem como se estivessem nos mesmos pacotes que o código-fonte.

## Criação do teste JUnit {#creating-the-junit-test}

Os testes de unidade normalmente mapeiam de 1 a 1 com classes Java. Neste capítulo, escreveremos um teste JUnit para o **BylineImpl.java**, que é o Modelo do Sling que suporta o componente Byline.

![Pasta src de teste de unidade](assets/unit-testing/core-src-test-folder.png)

*Local onde são armazenados os testes de unidade.*

1. Crie um teste de unidade para a `BylineImpl.java` ao fazer uma nova classe Java em `src/test/java` em uma estrutura de pastas do pacote Java que reflete o local da classe Java a ser testada.

   ![Criar um novo arquivo BylineImplTest.java](assets/unit-testing/new-bylineimpltest.png)

   Como estamos testando

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   crie uma classe Java de teste de unidade correspondente em

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   O `Test` sufixo no ficheiro de ensaio da unidade, `BylineImplTest.java` é uma convenção que nos permite

   1. Identifique-o facilmente como o arquivo de teste _para_ `BylineImpl.java`
   1. Mas também diferencie o arquivo de teste _from_ a classe a ensaiar, `BylineImpl.java`



## Revisão de BylineImplTest.java {#reviewing-bylineimpltest-java}

Neste ponto, o arquivo de teste JUnit é uma classe Java vazia.

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

1. O primeiro método `public void setUp() { .. }` tem anotações com JUnit&#39;s `@BeforeEach`, que instrui o executador de teste JUnit a executar esse método antes de executar cada método de teste nesta classe. Isso fornece um local útil para inicializar um estado de teste comum exigido por todos os testes.

1. Os métodos subsequentes são os métodos de teste, cujos nomes recebem o prefixo `test` por convenção e marcados com `@Test` anotação. Observe que, por padrão, todos os nossos testes estão definidos para falhar, pois ainda não os implementamos.

   Para começar, começamos com um único método de teste para cada método público na classe que estamos testando, portanto:

   | BylineImpl.java |  | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | é testado por | testGetName() |
   | getOccupations() | é testado por | testGetOccupations() |
   | isEmpty() | é testado por | testIsEmpty() |

   Esses métodos podem ser expandidos conforme necessário, como veremos mais adiante neste capítulo.

   Quando essa classe de teste JUnit (também conhecida como caso de teste JUnit) é executada, cada método marcado com o `@Test` será executado como um teste que pode ser aprovado ou falhar.

![BylineImplTest gerado](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Execute o caso de teste JUnit clicando com o botão direito do mouse no `BylineImplTest.java` e tocar em **Executar**.
Como esperado, todos os testes falharam, pois ainda não foram implementados.

   ![Executar como teste de junção](assets/unit-testing/run-junit-tests.png)

   *Clique com o botão direito do mouse em BylineImplTests.java > Executar*

## Revisão de BylineImpl.java {#reviewing-bylineimpl-java}

Ao gravar testes de unidade, há duas abordagens principais:

* [TDD ou desenvolvimento orientado para teste](https://en.wikipedia.org/wiki/Test-driven_development), o que implica que os testes unitários sejam escritos de forma incremental, imediatamente antes do desenvolvimento da aplicação; gravar um teste, gravar a implementação para fazer o teste passar.
* Implementação - primeiro desenvolvimento, que envolve o desenvolvimento de código de trabalho primeiro e, em seguida, a gravação de testes que validam esse código.

Neste tutorial, a última abordagem é usada (já que criamos um **BylineImpl.java** em um capítulo anterior). Por isso, devemos rever e entender os comportamentos dos seus métodos públicos, mas também alguns dos seus detalhes de implementação. Tal pode parecer contrário, uma vez que um bom teste só deve incidir sobre os fatores de produção e os resultados, contudo, quando se trabalha em AEM, há uma variedade de considerações de aplicação que têm de ser entendidas para a realização de testes de trabalho.

O TDD no contexto de AEM requer um nível de especialização e é melhor adotado por AEM desenvolvedores com capacidade AEM desenvolvimento e teste de unidade de código AEM.

## Configuração AEM contexto de teste  {#setting-up-aem-test-context}

A maioria dos códigos escritos para AEM depende de APIs JCR, Sling ou AEM, que, por sua vez, exigem que o contexto de um AEM em execução seja executado corretamente.

Como os testes de unidade são executados na criação, fora do contexto de uma instância de AEM em execução, não há esse contexto. Para facilitar esta tarefa, [mocks de AEM do wcm.io](https://wcm.io/testing/aem-mock/usage.html) cria um contexto de modelo que permite que essas APIs _maioria_ agir como se estivessem em AEM.

1. Criar um contexto de AEM usando **wcm.io&#39;s** `AemContext` em **BylineImplTest.java** adicionando-o como uma extensão JUnit decorada com `@ExtendWith` para **BylineImplTest.java** arquivo. A extensão cuida de todas as tarefas de inicialização e limpeza necessárias. Crie uma variável de classe para `AemContext` que pode ser utilizado para todos os métodos de ensaio.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Essa variável, `ctx`, expõe um modelo AEM contexto que fornece várias abstrações de AEM e Sling:

   * O Modelo de Sling BylineImpl será registrado neste contexto
   * As estruturas de conteúdo do JCR de bloco são criadas neste contexto
   * Os serviços OSGi personalizados podem ser registrados neste contexto
   * Fornece uma variedade de modelos de objetos e ajuda comuns necessários, como objetos SlingHttpServletRequest , uma variedade de modelos de Sling e AEM serviços OSGi, como ModelFactory, PageManager, Page, Template, ComponentManager, ComponentManager, Component, TagManager, Tag, etc.
      * *Observe que nem todos os métodos para esses objetos estão implementados!*
   * E [muito mais](https://wcm.io/testing/aem-mock/usage.html)!

   O **`ctx`** O objeto atuará como o ponto de entrada para a maior parte do nosso contexto de modelo.

1. No `setUp(..)` , que é executado antes de cada `@Test` , defina um estado de teste de modelo comum:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** registra o Modelo do Sling a ser testado, no modelo AEM Contexto, para que possa ser instanciado no `@Test` métodos.
   * **`load().json`** carrega estruturas de recursos no contexto do modelo, permitindo que o código interaja com esses recursos como se fossem fornecidos por um repositório real. As definições de recursos no arquivo **`BylineImplTest.json`** são carregadas no contexto mock JCR em **/conteúdo**.
   * **`BylineImplTest.json`** ainda não existe, portanto, vamos criá-lo e definir as estruturas de recurso do JCR necessárias para o teste.

1. Os arquivos JSON que representam as estruturas de recursos do modelo são armazenados em **core/src/test/resources** seguindo o mesmo caminho de pacote que o arquivo de teste Java JUnit.

   Crie um novo arquivo JSON em `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl` nomeado **BylineImplTest.json** com o seguinte conteúdo:

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   Esse JSON define um recurso mock (nó JCR) para o teste de unidade do componente Byline. Nesse ponto, o JSON tem o conjunto mínimo de propriedades necessárias para representar um recurso de conteúdo do componente Byline, a variável `jcr:primaryType` e `sling:resourceType`.

   Uma regra geral ao trabalhar com testes de unidade é criar o conjunto mínimo de conteúdo, contexto e código do mock necessário para satisfazer cada teste. Evite a tentação de construir um contexto de zombaria completo antes de escrever os testes, pois isso geralmente resulta em artefatos desnecessários.

   Agora com a existência de **BylineImplTest.json**, quando `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` for executado, as definições de recurso mock serão carregadas no contexto no caminho **/conteúdo.**

## Teste de getName() {#testing-get-name}

Agora que temos uma configuração básica de contexto de modelo, vamos escrever nosso primeiro teste para **GetName() de BylineImpl**. Este ensaio deve assegurar o método **getName()** retorna o nome de autor correto armazenado no &quot; do recurso **name&quot;** propriedade.

1. Atualize o **testGetName**() método em **BylineImplTest.java** como se segue:

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
   * **`ctx.currentResource`** define o contexto do recurso mock para avaliar o código, para que isso seja definido como **/content/byline** já que é onde o recurso de conteúdo mock by line é carregado.
   * **`Byline byline`** instancia o Modelo de sling Byline adaptando-o do objeto de solicitação de mock.
   * **`String actual`** chama o método que estamos testando. `getName()`, no objeto Modelo de sling em linha.
   * **`assertEquals`** afirma que o valor esperado corresponde ao valor retornado pelo objeto do Modelo de sling byline. Se esses valores não forem iguais, o teste falhará.

1. Execute o teste... e ele falhará com um `NullPointerException`.

   Observe que esse teste NÃO falha porque nunca definimos um valor de `name` no mock JSON, que fará com que o teste falhe, no entanto, a execução do teste não chegou a esse ponto! Esse teste falha devido a um `NullPointerException` no próprio objeto byline.

1. No `BylineImpl.java`, se `@PostConstruct init()` O aciona uma exceção, pois impede que o Modelo do Sling instancie e faz com que o objeto do Modelo do Sling seja nulo.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Acontece que, embora o serviço OSGi ModelFactory seja fornecido por meio do `AemContext` (por meio do Contexto do Apache Sling), nem todos os métodos são implementados, incluindo `getModelFromWrappedRequest(...)` que é chamado em BylineImpl&#39;s `init()` método . Isso resulta em uma [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html), as causas `init()` para falhar e a consequente adaptação do `ctx.request().adaptTo(Byline.class)` é um objeto nulo.

   Como os modelos fornecidos não podem acomodar nosso código, precisamos implementar o contexto do modelo nós mesmos Para isso, podemos usar o Mockito para criar um objeto ModeloFactory mock, que retorna um objeto de imagem mock quando `getModelFromWrappedRequest(...)` é chamado sobre ele.

   Como para até mesmo instanciar o Modelo do Sling Byline, esse contexto do modelo deve estar em vigor, podemos adicioná-lo ao `@Before setUp()` método . Precisamos também de adicionar o `MockitoExtension.class` para `@ExtendWith` anotação acima da **BylineImplTest** classe .

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** marca a classe Caso de teste a ser executada com [Extensão Jupiter do Mockito JUnit](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) o que permite o uso das anotações @Mock para definir objetos mock no nível Class.
   * **`@Mock private Image`** cria um objeto mock do tipo `com.adobe.cq.wcm.core.components.models.Image`. Observe que isso é definido no nível da classe para que, conforme necessário, `@Test` métodos podem alterar seu comportamento conforme necessário.
   * **`@Mock private ModelFactory`** cria um objeto modelo do tipo ModelFactory. Observe que este é um zombro Mockito puro e não tem métodos implementados nele. Observe que isso é definido no nível da classe para que, conforme necessário, `@Test`métodos podem alterar seu comportamento conforme necessário.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registra o comportamento mock para quando `getModelFromWrappedRequest(..)` é chamado no objeto modeloFactory . O resultado definido em `thenReturn (..)` é retornar o objeto de modelo de Imagem. Observe que esse comportamento é chamado somente quando: o primeiro parâmetro é igual a `ctx`Objeto de solicitação do , o segundo parâmetro é qualquer objeto de recurso e o terceiro parâmetro deve ser a classe Imagem dos componentes principais . Aceitamos qualquer recurso porque durante nossos testes definiremos as variáveis `ctx.currentResource(...)` para vários recursos mock definidos na variável **BylineImplTest.json**. Observe que adicionamos a variável **lenient()** rigor porque desejaremos, mais tarde, substituir esse comportamento do ModelFactory.
   * **`ctx.registerService(..)`.** registra o objeto modeloFactory no AemContext, com a classificação de serviço mais alta. Isso é necessário, pois o ModelFactory usado no `init()` é injetado através do `@OSGiService ModelFactory model` campo. Para que o AemContext seja injetado **our** objeto mock, que lida com chamadas para `getModelFromWrappedRequest(..)`, devemos registrá-lo como o Serviço de classificação mais alta desse tipo (ModelFactory).

1. Execute o teste novamente e ele falhará novamente, mas desta vez a mensagem estará clara por que ele falhou.

   ![declaração de falha do nome de teste](assets/unit-testing/testgetname-failure-assertion.png)

   *Falha de testGetName() devido à asserção*

   Recebemos um **AssertionError** o que significa que a condição de asserção no teste falhou, e isso nos informa o **o valor esperado é &quot;Jane Doe&quot;** mas o **o valor real é nulo**. Isso faz sentido porque o &quot;**name&quot;** propriedade não foi adicionada ao mock **/content/byline** definição de recurso em **BylineImplTest.json**, então vamos adicioná-lo:

1. Atualizar **BylineImplTest.json** para definir `"name": "Jane Doe".`

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

   ![passagem do nome de teste](assets/unit-testing/testgetname-pass.png)


## Teste de getOccupations() {#testing-get-occupations}

Muito bem! Nosso primeiro teste foi bem-sucedido! Vamos continuar e testar `getOccupations()`. Como a inicialização do contexto do modelo foi realizada no `@Before setUp()`estará disponível para todos `@Test` métodos neste caso de teste, incluindo `getOccupations()`.

Lembre-se de que esse método deve retornar uma lista de ocupações (decrescentes) classificadas alfabeticamente armazenada na propriedade de ocupações.

1. Atualizar **`testGetOccupations()`** como se segue:

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
   * **`ctx.currentResource`** define o recurso atual para avaliar o contexto em relação à definição de recurso de modelo em /content/byline. Isso garante que o **BylineImpl.java** é executado no contexto do nosso recurso mock.
   * **`ctx.request().adaptTo(Byline.class)`** instancia o Modelo de sling Byline adaptando-o do objeto de solicitação de mock.
   * **`byline.getOccupations()`** chama o método que estamos testando. `getOccupations()`, no objeto Modelo de sling em linha.
   * **`assertEquals(expected, actual)`** afirma que a lista esperada é igual à lista real.

1. Lembre-se, como **`getName()`** acima, a variável **BylineImplTest.json** não define ocupações, portanto, esse teste falhará se for executado, já que `byline.getOccupations()` retornará uma lista vazia.

   Atualizar **BylineImplTest.json** para incluir uma lista de ocupações, e elas serão definidas em ordem não alfabética para garantir que nossos testes validem que as ocupações sejam classificadas alfabeticamente por **`getOccupations()`**.

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

1. Execute o teste e novamente nós passamos! Parece ter as ocupações organizadas funcionando!

   ![Obter o Passe de Ocupações](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations() passa*

## Testar isEmpty() {#testing-is-empty}

O último método a testar **`isEmpty()`**.

Teste `isEmpty()` é interessante, pois requer testes para uma variedade de condições. Revisão **BylineImpl.java**&#39;s `isEmpty()` Devem ser ensaiadas as seguintes condições:

* Retornar verdadeiro quando o nome estiver vazio
* Retornar verdadeiro quando as ocupações forem nulas ou vazias
* Retorna true quando a imagem for nula ou não tiver um URL src
* Retornar falso quando o nome, as ocupações e a Imagem (com um URL src) estiverem presentes

Para isso, precisamos criar novos métodos de teste, cada um testando uma condição específica, bem como novas estruturas de recursos de modelo em `BylineImplTest.json` para conduzir esses testes.

Observe que essa verificação nos permitiu ignorar o teste de quando `getName()`, `getOccupations()` e `getImage()` estão vazias, pois o comportamento esperado desse estado é testado por meio de `isEmpty()`.

1. O primeiro teste testará a condição de um componente totalmente novo, que não tenha propriedades definidas.

   Adicione uma nova definição de recurso a `BylineImplTest.json`, atribuindo-lhe o nome semântico &quot;**empty**&quot;

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

   **`"empty": {...}`** defina uma nova definição de recurso chamada &quot;empty&quot; que só tem uma `jcr:primaryType` e `sling:resourceType`.

   Lembrar que carregamos `BylineImplTest.json` em `ctx` antes da execução de cada método de ensaio em `@setUp`, portanto, essa nova definição de recurso está imediatamente disponível para nós em testes em **/content/empty.**

1. Atualizar `testIsEmpty()` como segue, definindo o recurso atual para o novo &quot;**empty**&quot; definição de recurso mock.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Execute o teste e verifique se ele foi aprovado.

1. Em seguida, crie um conjunto de métodos para garantir que, se qualquer um dos pontos de dados necessários (nome, ocupações ou imagem) esteja vazio, `isEmpty()` retorna true.

   Para cada teste, uma definição de recurso de modelo discreta é usada, atualizar **BylineImplTest.json** com definições de recursos adicionais para **sem nome** e **sem ocupações**.

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

   **`testIsEmpty()`** faz o teste em relação à definição de recurso de modelo vazio e afirma que `isEmpty()` é verdadeiro.

   **`testIsEmpty_WithoutName()`** O testa uma definição de recurso de modelo que tem ocupações, mas nenhum nome.

   **`testIsEmpty_WithoutOccupations()`** O testa uma definição de recurso de modelo que tem um nome, mas nenhuma ocupação.

   **`testIsEmpty_WithoutImage()`** O testa uma definição de recurso de modelo com um nome e ocupações, mas define o modelo de Imagem para retornar a nulo. Observe que queremos substituir a variável `modelFactory.getModelFromWrappedRequest(..)`comportamento definido em `setUp()` para garantir que o objeto Image retornado por esta chamada seja nulo. O recurso de bordas do Mockito é restrito e não deseja código duplicado. Por isso, colocamos a espada com **`lenient`** configurações para observar explicitamente que estamos substituindo o comportamento na variável `setUp()` método .

   **`testIsEmpty_WithoutImageSrc()`** testa uma definição de recurso de modelo com um nome e ocupações, mas define o modelo de Imagem para retornar uma string em branco quando `getSrc()` é chamado.

1. Por fim, escreva um teste para garantir que **isEmpty()** retorna false quando o componente está configurado corretamente. Para essa condição, podemos reutilizar **/content/byline** que representa um componente Byline totalmente configurado.

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. Agora, execute todos os testes de unidade no arquivo BylineImplTest.java e revise a saída do Relatório de teste do Java.

![Todos os testes foram bem-sucedidos](./assets/unit-testing/all-tests-pass.png)

## Execução de testes de unidade como parte da compilação {#running-unit-tests-as-part-of-the-build}

Os testes de unidade são executados e precisam ser aprovados como parte da compilação maven. Isso garante que todos os testes sejam bem-sucedidos antes da implantação de um aplicativo. A execução de metas Maven, como pacote ou instalação, chama automaticamente e requer a aprovação de todos os testes de unidade no projeto.

```shell
$ mvn package
```

![êxito do pacote mvn](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

Da mesma forma, se alterarmos um método de teste para falhar, a build falhará e reportará quais testes falharam e por quê.

![falha no pacote mvn](assets/unit-testing/mvn-package-fail.png)

## Revise o código {#review-the-code}

Exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente na chave Git `tutorial/unit-testing-solution`.
