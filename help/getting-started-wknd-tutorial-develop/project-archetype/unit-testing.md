---
title: Teste de unidade
description: Implemente um teste de unidade para validar o comportamento do modelo do Sling do componente de linha de identificação criado no tutorial “Componente personalizado”.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '2923'
ht-degree: 100%

---

# Teste de unidade {#unit-testing}

Este tutorial aborda a implementação de um teste de unidade que valida o comportamento do modelo do Sling do componente de linha de identificação criado no tutorial [Componente personalizado](./custom-component.md).

## Pré-requisitos {#prerequisites}

Consulte as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

_Se o Java™ 8 e o Java™ 11 estiverem instalados no sistema, o executor dos testes do VS Code poderá escolher o menor tempo de execução do Java™ ao executar os testes, resultando em falhas nos testes. Se isso ocorrer, desinstale o Java™ 8._

### Projeto inicial

>[!NOTE]
>
> Se você tiver concluído com sucesso o capítulo anterior, poderá reutilizar o projeto e ignorar as etapas para conferir o projeto inicial.

Confira o código de linha de base no qual o tutorial se baseia:

1. Confira a ramificação `tutorial/unit-testing-start` do [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Implante a base de código em uma instância do AEM local, usando as suas habilidades do Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando o AEM 6.5 ou 6.4, anexe o perfil `classic` a qualquer comando do Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

É sempre possível exibir o código concluído no [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) ou conferir o código localmente, alternando-se para a ramificação `tutorial/unit-testing-start`.

## Objetivo

1. Noções básicas sobre o teste de unidade.
1. Aprender sobre as estruturas e ferramentas usadas normalmente para testar o código do AEM.
1. Entender as opções de emular ou simular recursos do AEM ao gravar testes de unidade.

## Fundo {#unit-testing-background}

Neste tutorial, veremos como criar [testes de unidade](https://pt.wikipedia.org/wiki/Teste_de_unidade) para o [modelo do Sling](https://sling.apache.org/documentation/bundles/models.html) do componente de linha de identificação (criado em [Criar um componente personalizado do AEM](custom-component.md)). Testes de unidade são testes de tempo de compilação escritos em Java™ que verificam o comportamento esperado do código Java™. Cada teste de unidade geralmente é pequeno e valida a saída de um método (ou unidades de trabalho) em relação aos resultados esperados.

Usamos as práticas recomendadas do AEM e empregamos:

* [JUnit 5](https://junit.org/junit5/)
* [Estrutura de teste do Mockito](https://site.mockito.org/)
* [Estrutura de teste do wcm.io](https://wcm.io/testing/) (que se baseia em [simulações do Sling do Apache](https://sling.apache.org/documentation/development/sling-mock.html))

## Teste de unidade e Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

O [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/introduction.html?lang=pt-BR) integra a execução de testes de unidade e a [geração de relatórios de cobertura do código](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html?lang=pt-BR) ao seu pipeline de CI/CD para ajudar a incentivar e promover a prática recomendada de teste de unidade do código do AEM.

Embora o código dos testes de unidade seja uma boa prática para qualquer base de código, ao usar o Cloud Manager, é importante aproveitar seus recursos de geração de relatórios e testes de qualidade do código, que oferecem testes de unidade para execução pelo Cloud Manager.

## Atualizar as dependências do Maven de teste {#inspect-the-test-maven-dependencies}

A primeira etapa é inspecionar as dependências do Maven para permitir a gravação e execução dos testes. São necessárias quatro dependências:

1. JUnit5
1. Estrutura de teste do Mockito
1. Simulações do Sling do Apache
1. Estrutura de teste de simulações do AEM (pelo io.wcm)

As dependências de teste **JUnit5**, **Mockito e **Simulações do AEM** são adicionadas automaticamente ao projeto durante a instalação com o [arquétipo do Maven do AEM](project-setup.md).

1. Para exibir essas dependências, abra o POM do reator primário em **aem-guides-wknd/pom.xml**, navegue até `<dependencies>..</dependencies>` e exiba as dependências de JUnit, Mockito, simulações do Sling do Apache e testes de simulação do AEM do io.wcm em `<!-- Testing -->`.
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
   > O arquétipo **35** gera o projeto com `io.wcm.testing.aem-mock.junit5` versão **4.1.8**. Faça o downgrade para **4.1.0** a fim de prosseguir com o restante deste capítulo.

1. Abra **aem-guides-wknd/core/pom.xml** e veja se as dependências de teste correspondentes estão disponíveis.

   Uma pasta de origem paralela no projeto **principal** conterá os testes de unidade e quaisquer arquivos de teste de suporte. Esta pasta de **teste** fornece uma separação de classes de teste do código-fonte, mas permite que os testes atuem como se estivessem nos mesmos pacotes que o código-fonte.

## Criar o teste com JUnit {#creating-the-junit-test}

Os testes de unidade normalmente mapeiam de 1 para 1 com classes de Java™. Neste capítulo, escreveremos um teste de JUnit para o **BylineImpl.java**, que é o modelo do Sling que oferece compatibildiade com o componente de linha de identificação.

![Pasta de origem do teste de unidade](assets/unit-testing/core-src-test-folder.png)

*Local onde os testes de unidade estão armazenados.*

1. Crie um teste de unidade para o `BylineImpl.java`, criando uma nova classe de Java™ em `src/test/java`, em uma estrutura de pasta de pacote de Java™ que espelhe o local da classe de Java™ a ser testada.

   ![Criar um novo arquivo BylineImplTest.java](assets/unit-testing/new-bylineimpltest.png)

   Já que estamos testando

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   a criação de uma classe de Java™ de teste de unidade correspondente em

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   O sufixo `Test` no arquivo de teste unitário, em que `BylineImplTest.java` é uma convenção, que nos permite

   1. Identificá-lo facilmente como o arquivo de teste _para_ `BylineImpl.java`
   1. Além disso, diferenciar o arquivo de teste _de_ a classe que está sendo testada, `BylineImpl.java`

## Revisar o BylineImplTest.java {#reviewing-bylineimpltest-java}

Neste ponto, o arquivo de teste de JUnit é uma classe de Java™ vazia.

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

1. O primeiro método `public void setUp() { .. }` é anotado com o `@BeforeEach` de JUnit, que instrui o executor de teste de JUnit a executar este método antes de executar cada método de teste nesta classe. Isso fornece um local útil para inicializar o estado de teste comum exigido por todos os testes.

1. Os métodos subsequentes são os métodos de teste, cujos nomes são prefixados com `test` por convenção e marcados com a anotação `@Test`. Observe que, por padrão, todos os nossos testes estão definidos como reprovados, pois ainda não os implementamos.

   Para começar, usamos apenas um método de teste para cada método público na classe que estamos testando; portanto:

   | BylineImpl.java |              | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | é testado por | testGetName() |
   | getOccupations() | é testado por | testGetOccupations() |
   | isEmpty() | é testado por | testIsEmpty() |

   Esses métodos podem ser expandidos conforme necessário, como veremos adiante neste capítulo.

   Quando esta classe de teste de JUnit (também conhecida como caso de teste de JUnit) é executada, cada método marcado com o `@Test` será executado como um teste que pode ser aprovado ou reprovado.

![BylineImplTest gerado](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Execute o caso de teste de JUnit, clicando com o botão direito do mouse no arquivo `BylineImplTest.java` e tocando em **Executar**.
Como esperado, todos os testes falham, pois ainda não foram implementados.

   ![Executar como teste de junit](assets/unit-testing/run-junit-tests.png)

   *Clique com o botão direito do mouse em BylineImplTests.java > Executar*

## Revisar o BylineImpl.java {#reviewing-bylineimpl-java}

Ao escrever testes de unidade, há duas abordagens principais:

* [TDD ou desenvolvimento orientado por teste](https://pt.wikipedia.org/wiki/Test-driven_development), que envolve gravar os testes de unidade de forma incremental, imediatamente antes de a implementação ser desenvolvida; gravar um teste; e gravar a implementação para fazer com que o teste passe.
* Desenvolvimento com implementação primeiro, que envolve o desenvolvimento do código de trabalho primeiro e, em seguida, a gravação de testes que validam esse código.

Neste tutorial, a última abordagem será usada (já que criamos um **BylineImpl.java** de trabalho em um capítulo anterior). Por isso, precisamos revisar e entender os comportamentos de seus métodos públicos, mas também alguns de seus detalhes de implementação. Isso pode parecer contraintuitivo, pois um bom teste deve se preocupar apenas com as entradas e saídas, mas, ao trabalhar no AEM, há várias considerações de implementação que precisam ser entendidas a fim de construir testes de trabalho.

O TDD no contexto do AEM requer um nível de conhecimento e é melhor adotado por desenvolvedores do AEM proficientes em desenvolvimento no AEM e testes de unidade do código do AEM.

## Configurar o contexto de teste do AEM  {#setting-up-aem-test-context}

A maioria dos códigos escritos para o AEM depende de APIs do JCR, Sling ou AEM, que, por sua vez, exigem o contexto do AEM em execução para ser executados corretamente.

Como os testes de unidade são executados na criação, fora do contexto de uma instância do AEM em execução, esse contexto não existe. Para facilitar isso, as [simulações do AEM do wcm.io](https://wcm.io/testing/aem-mock/usage.html) cria um contexto de simulação que permite que essas APIs _em sua maioria_ atuem como se estivessem em execução no AEM.

1. Crie um contexto do AEM, usando o `AemContext` do **wcm.io** no **BylineImplTest.java** e adicionando-o como uma extensão de JUnit decorada com `@ExtendWith` ao arquivo **BylineImplTest.java**. A extensão cuida de todas as tarefas de inicialização e limpeza necessárias. Crie uma variável de classe para `AemContext` que possa ser usada para todos os métodos de teste.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Esta variável, `ctx`, expõe um contexto de simulação do AEM que fornece algumas abstrações do AEM e do Sling:

   * O modelo do Sling BylineImpl está registrado neste contexto
   * As estruturas de conteúdo de JCR de simulação são criadas neste contexto
   * Os serviços da OSGi personalizados podem ser registrados neste contexto
   * Fornece vários objetos de modelo de simulação e auxiliares comuns necessários, como objetos SlingHttpServletRequest, vários serviços de simulação da OSGi para Sling e AEM, como ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, Tag etc.
      * *Nem todos os métodos para esses objetos foram implementados.*
   * E [muito mais](https://wcm.io/testing/aem-mock/usage.html)!

   O objeto **`ctx`** atuará como ponto de entrada para a maioria do nosso contexto de simulação.

1. No método `setUp(..)`, que é executado antes de cada método `@Test`, defina um estado de teste de simulação comum:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** registra o modelo do Sling a ser testado no contexto do AEM de simulação, para que possa ser instanciado nos métodos `@Test`.
   * O **`load().json`** carrega estruturas de recursos no contexto de simulação, permitindo que o código interaja com esses recursos como se fossem fornecidos por um repositório real. As definições dos recursos no arquivo **`BylineImplTest.json`** são carregadas no contexto do JCR de simulação em **/content**.
   * **`BylineImplTest.json`** ainda não existe; portanto, vamos criá-lo e definir as estruturas de recursos do JCR necessárias para o teste.

1. Os arquivos JSON que representam as estruturas de recursos de simulação são armazenados em **core/src/test/resources**, seguindo o mesmo caminho de pacote do arquivo de teste JUnit Java™.

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

   Esse JSON define um recurso de simulação (nó do JCR) para o teste de unidade do componente de linha de identificação. Neste ponto, o JSON conta com o conjunto mínimo de propriedades necessárias para representar um recurso de conteúdo do componente de linha de identificação, o `jcr:primaryType` e `sling:resourceType`.

   Uma regra geral ao trabalhar com testes de unidade é criar o conjunto mínimo de conteúdo de simulação, contexto e código necessários para satisfazer cada teste. Evite a tentação de criar um contexto de simulação completo antes de escrever os testes, pois isso geralmente resulta em artefatos desnecessários.

   Agora, com a existência do **BylineImplTest.json**, quando `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` for executado, as definições de recursos de simulação serão carregadas no contexto no caminho **/content.**

## Testar getName() {#testing-get-name}

Agora que temos uma configuração básica de contexto de simulação, vamos escrever o nosso primeiro teste para **BylineImpl&#39;s getName()**. Esse teste precisa garantir que o método **getName()** retorne o nome criado corretamente armazenado na propriedade “**nome”** do recurso.

1. Atualize o método **testGetName**() no **BylineImplTest.java** da seguinte maneira:

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

   * **`String expected`** define o valor esperado. Nós o definiremos como “**Jane Doe**”.
   * **`ctx.currentResource`** define o contexto do recurso de simulação para avaliar o código em relação a ele; portanto, é definido como **/content/byline**, pois é aí que o recurso de conteúdo de linha de identificação de simulação é carregado.
   * **`Byline byline`** instancia o modelo do Sling de linha de identificação, adaptando-o do objeto de solicitação de simulação.
   * **`String actual`** invoca o método que estamos testando, `getName()`, no objeto de modelo do Sling de linha de identificação.
   * **`assertEquals`** afirma que o valor esperado corresponde ao valor retornado pelo objeto de modelo do Sling de linha de identificação. Se esses valores não forem iguais, o teste falhará.

1. Execute o teste... e ele falhará com um `NullPointerException`.

   Esse teste NÃO falha, porque nunca definimos uma propriedade `name` no JSON de simulação, que fará com que o teste falhe, mas a execução do teste não chegou a esse ponto. Esse teste falha devido a um `NullPointerException` no próprio objeto de linha de identificação.

1. No `BylineImpl.java`, se `@PostConstruct init()` acionar uma exceção, ela impedirá que o modelo do Sling seja instanciado e fará com que esse objeto de modelo do Sling seja nulo.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Acontece que, embora o serviço ModelFactory seja da OSGi fornecido por meio do `AemContext` (através do contexto do Sling do Apache), nem todos os métodos são implementados, incluindo `getModelFromWrappedRequest(...)`, que é chamado no método `init()` do BylineImpl. Isso resulta em um [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html), que, por sua vez, causa uma falha em `init()`, e a adaptação resultante do `ctx.request().adaptTo(Byline.class)` é um objeto nulo.

   Como as simulações fornecidas não podem acomodar o nosso código, precisamos implementar o contexto de simulação por conta própria. Para isso, podemos usar o Mockito para criar um objeto ModelFactory de simulação, que retorna um objeto de imagem de simulação quando `getModelFromWrappedRequest(...)` é invocado nele.

   Como, para instanciar o modelo do Sling de linha de identificação, esse contexto de simulação precisa estar presente, podemos adicioná-lo ao método `@Before setUp()`. Também precisamos adicionar `MockitoExtension.class` à anotação `@ExtendWith` acima da classe **BylineImplTest**.

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** marca a classe de caso de teste a ser executada com a [extensão Mockito JUnit Jupiter](https://www.javadoc.io/static/org.mockito/mockito-junit-jupiter/4.11.0/org/mockito/junit/jupiter/MockitoExtension.html), que permite o uso das anotações @Mock para definir objetos de simulação na camada da classe.
   * **`@Mock private Image`** cria um objeto de simulação do tipo `com.adobe.cq.wcm.core.components.models.Image`. Isso é definido na camada da classe, de modo que, conforme necessário, os métodos `@Test` possam alterar seu comportamento.
   * **`@Mock private ModelFactory`** cria um objeto de simulação do tipo ModelFactory. Esta é uma simulação pura do Mockito e não contém nenhum método implementado. Isso é definido na camada da classe, de modo que, conforme necessário, os métodos `@Test` possam alterar seu comportamento.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registra o comportamento da simulação para quando `getModelFromWrappedRequest(..)` é chamado no objeto ModelFactory de simulação. O resultado definido em `thenReturn (..)` é retornar o objeto de imagem de simulação. Esse comportamento só é chamado quando: o primeiro parâmetro é igual ao objeto de solicitação de `ctx`, o segundo parâmetro é qualquer objeto de recurso, e o terceiro parâmetro precisa ser a classe de imagem dos componentes principais. Aceitamos qualquer recurso, pois, em todos os nossos testes, estamos configurando o `ctx.currentResource(...)` para vários recursos de simulação definidos no **BylineImplTest.json**. Observe que adicionamos a rigidez **lenient()** porque vamos querer substituir esse comportamento do ModelFactory posteriormente.
   * **`ctx.registerService(..)`.** registra o objeto ModelFactory de simulação no AemContext, com a classificação de serviço mais alta. Isso é necessário, pois o ModelFactory usado no `init()` do BylineImpl é inserido por meio do campo `@OSGiService ModelFactory model`. Para que o AemContext injete o **nosso** objeto de simulação, que manipula chamadas para `getModelFromWrappedRequest(..)`, preicsamos registrá-lo como o serviço de classificação mais alta desse tipo (ModelFactory).

1. Execute novamente o teste e, novamente, ele falhará, mas, desta vez, a mensagem de por que ele falhou estará clara.

   ![asserção de falha do nome do teste](assets/unit-testing/testgetname-failure-assertion.png)

   Falha de *testGetName() devido à asserção*

   Recebemos um **AssertionError**, que significa que a condição de asserção do teste falhou e nos informa que o **valor esperado é “Jane Doe”**, mas o **valor real é nulo**. Isso faz sentido, porque a propriedade “**nome”** não foi adicionada à definição de recurso de simulação **/content/byline** em **BylineImplTest.json**; então, vamos adicioná-la:

1. Atualize o **BylineImplTest.json** para definir `"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. Execute o teste novamente, e **`testGetName()`** agora será aprovado!

   ![aprovação do nome do teste](assets/unit-testing/testgetname-pass.png)


## Teste de getOccupations() {#testing-get-occupations}

Muito bem! O primeiro teste foi aprovado. Vamos prosseguir e testar `getOccupations()`. Como a inicialização do contexto de simulação foi feita no método `@Before setUp()`, ela está disponível para todos os métodos `@Test` neste caso de teste, incluindo `getOccupations()`.

Lembre-se de que esse método precisa retornar uma lista de profissões classificada em ordem alfabética (decrescente) armazenada na propriedade de profissões.

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

   * **`List<String> expected`** define o resultado esperado.
   * **`ctx.currentResource`** define o recurso atual para avaliar o contexto em relação à definição do recurso de simulação em /content/byline. Isso garante que o **BylineImpl.java** seja executado no contexto do nosso recurso de simulação.
   * **`ctx.request().adaptTo(Byline.class)`** instancia o modelo do Sling de linha de identificação, adaptando-o do objeto de solicitação de simulação.
   * **`byline.getOccupations()`** invoca o método que estamos testando, `getOccupations()`, no objeto de modelo do Sling de linha de identificação.
   * **`assertEquals(expected, actual)`** afirma que a lista esperada é igual à lista real.

1. Lembre-se de que, assim como **`getName()`** acima, o **BylineImplTest.json** não define profissões. Portanto, este teste falhará se o executarmos, já que `byline.getOccupations()` retornará uma lista vazia.

   Atualize **BylineImplTest.json** para incluir uma lista de profissões, e elas serão definidas em ordem não alfabética para garantir que nossos testes confirmem que as profissões estão classificadas em ordem alfabética por **`getOccupations()`**.

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

1. Execute o teste; novamente, passamos! Parece que as profissões ordenadas estão funcionando.

   ![Aprovação da obtenção de profissões](assets/unit-testing/testgetoccupations-pass.png)

   *aprovações de testGetOccupations()*

## Testar isEmpty() {#testing-is-empty}

O último método para testar **`isEmpty()`**.

Testar `isEmpty()` é interessante, pois envolve testar várias condições. Ao revisar o método `isEmpty()` do **BylineImpl.java**, as seguintes condições precisam ser testadas:

* Retorna verdadeiro quando o nome está vazio
* Retorna verdadeiro quando as profissões são nulas ou estão vazias
* Retorna verdadeiro quando a imagem é nula ou não tem o URL de origem
* Retorna falso quando o nome, as profissões e a imagem (com um URL de origem) estão presentes

Para isso, precisamos criar métodos de teste, cada um testando uma condição específica, e novas estruturas de recursos de simulação em `BylineImplTest.json` para conduzir esses testes.

Essa verificação permitiu-nos ignorar o teste para quando `getName()`, `getOccupations()` e `getImage()` estão vazios, pois o comportamento esperado desse estado é testado via `isEmpty()`.

1. O primeiro teste analisará a condição de um componente totalmente novo que não tem propriedades definidas.

   Adicione uma nova definição de recurso a `BylineImplTest.json`, dando-lhe o nome semântico “**vazio**”.

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

   **`"empty": {...}`** define uma nova definição de recurso chamada “vazio” que tenha somente `jcr:primaryType` e `sling:resourceType`.

   Lembre-se de que carregamos `BylineImplTest.json` em `ctx` antes da execução de cada método de teste em `@setUp`. Portanto, essa nova definição do recurso fica imediatamente disponível para nós nos testes em **/content/empty.**

1. Atualize `testIsEmpty()` da maneira a seguir, definindo o recurso atual com a nova definição do recurso de simulação “**vazio**”.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Execute o teste e verifique se ele foi aprovado.

1. Em seguida, crie um conjunto de métodos para garantir que, se qualquer um dos pontos de dados necessários (nome, profissões ou imagem) estiver vazio, `isEmpty()` retornará verdadeiro.

   Para cada teste, uma definição de recurso de simulação diferente é usada; atualize **BylineImplTest.json** com as definições de recursos adicionais para **without-name** e **without-occupations**.

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

   **`testIsEmpty()`** testa com a definição do recurso de simulação vazio e declara que `isEmpty()` é verdadeiro.

   **`testIsEmpty_WithoutName()`** testa em relação a uma definição do recurso de simulação que tem profissões, mas nenhum nome.

   **`testIsEmpty_WithoutOccupations()`** testa em relação a uma definição do recurso de simulação que tem um nome, mas nenhuma profissão.

   **`testIsEmpty_WithoutImage()`** testa uma definição do recurso de modelo com um nome e profissões, mas define a imagem de simulação para retornar como nulo. Observe que queremos substituir o comportamento `modelFactory.getModelFromWrappedRequest(..)` definido em `setUp()` para garantir que o objeto de imagem retornado por esta chamada seja nulo. O recurso de canhotos do Mockito é estrito e não permite um código duplicado. Portanto, definimos a simulação com configurações **`lenient`** para observar explicitamente que estamos substituindo o comportamento no método `setUp()`.

   **`testIsEmpty_WithoutImageSrc()`** testa uma definição de recurso de simulação com um nome e profissões, mas define o modelo de imagem para retornar uma string em branco quando `getSrc()` for invocado.

1. Por fim, escreva um teste para garantir que **isEmpty()** retorne falso quando o componente estiver configurado corretamente. Para esta condição, podemos reutilizar **/content/byline**, que representa um componente de linha de identificação totalmente configurado.

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. Agora, execute todos os testes de unidade no arquivo BylineImplTest.java e revise o resultado do relatório de teste de Java™.

![Todos os testes foram aprovados](./assets/unit-testing/all-tests-pass.png)

## Execução de testes de unidade como parte da compilação {#running-unit-tests-as-part-of-the-build}

Os testes de unidade são executados e devem ser aprovados como parte da compilação do Maven. Isso garante que todos os testes sejam aprovados com sucesso antes da implantação de um aplicativo. A execução de metas do Maven, como pacote ou instalação, chama automaticamente e requer a aprovação de todos os testes de unidade do projeto.

```shell
$ mvn package
```

![sucesso do pacote do mvn](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

Da mesma forma, se alterarmos um método de teste para que falhe, a compilação falhará e relatará qual teste falhou, bem como o porquê.

![falha no pacote do mvn](assets/unit-testing/mvn-package-fail.png)

## Revisar o código {#review-the-code}

Veja o código concluído no [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente na ramificação do Git `tutorial/unit-testing-solution`.
