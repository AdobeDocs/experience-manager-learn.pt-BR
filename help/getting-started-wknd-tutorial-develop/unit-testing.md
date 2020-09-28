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
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '3544'
ht-degree: 0%

---


# Teste de unidade {#unit-testing}

Este tutorial aborda a implementação de um Teste de unidade que valida o comportamento do Modelo Sling do componente Byline, criado no tutorial de Componente [](./custom-component.md) personalizado.

## Pré-requisitos {#prerequisites}

Confira o código básico no qual o tutorial se baseia:

1. Clonar o repositório [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) .
1. Confira o `unit-testing/start` ramo

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
$ cd ~/code/aem-guides-wknd
$ git checkout unit-testing/start
```

Você sempre pode visualização o código finalizado no [GitHub](https://github.com/adobe/aem-guides-wknd/tree/unit-testing/solution) ou fazer check-out do código localmente ao alternar para a ramificação `unit-testing/solution`.

## Objetivo

1. Entenda as noções básicas de teste de unidade.
1. Saiba mais sobre estruturas e ferramentas comumente usadas para testar o código AEM.
1. Entenda as opções de zombaria ou simulação de recursos AEM ao gravar testes de unidade.

## Segundo plano {#unit-testing-background}

Neste tutorial, exploraremos como escrever testes [de](https://en.wikipedia.org/wiki/Unit_testing) unidade para o [Sling Model](https://sling.apache.org/documentation/bundles/models.html) do componente Byline (criado em [Criação de um componente](custom-component.md)de AEM personalizado). Os testes de unidade são testes de tempo de criação escritos em Java que verificam o comportamento esperado do código Java. Em geral, cada teste de unidade é pequeno e valida a saída de um método (ou unidades de trabalho) em relação aos resultados esperados.

Usaremos AEM práticas recomendadas e:

* [JUnit 5](https://junit.org/junit5/)
* [Estrutura de teste Mockito](https://site.mockito.org/)
* [wcm.io Test Framework](https://wcm.io/testing/) (que se baseia em [Mocks](https://sling.apache.org/documentation/development/sling-mock.html)Apache Sling)

>[!VIDEO](https://video.tv.adobe.com/v/30207/?quality=12&learn=on)

## Teste de unidade e gerenciador da Adobe Cloud {#unit-testing-and-adobe-cloud-manager}

[O Adobe Cloud Manager](https://docs.adobe.com/content/help/pt-BR/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html) integra a execução de teste de unidade e o relatórios [de cobertura de](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) código em seu pipeline de CI/CD para ajudar a incentivar e promover as melhores práticas de teste de unidade AEM código.

Embora o código de teste de unidade seja uma boa prática para qualquer base de código, ao usar o Gerenciador de nuvem, é importante aproveitar seus testes de qualidade de código e recursos de relatórios, fornecendo testes de unidade para que o Gerenciador de nuvem seja executado.

## Inspect as dependências do Maven de teste {#inspect-the-test-maven-dependencies}

O primeiro passo é inspecionar as dependências Maven para suportar a gravação e a execução dos testes. Há quatro dependências necessárias:

1. JUnit5
1. Estrutura de teste Mockito
1. Mocas de martelo do Apache
1. AEM Mocks Test Framework (por io.wcm)

As dependências de teste **JUnit5**, **Mockito** e **AEM Mocks** são automaticamente adicionadas ao projeto durante a configuração usando o arquétipo [AEM Maven](project-setup.md).

1. Para visualização dessas dependências, abra o POM do reator principal em **aem-guides-wknd/pom.xml**, navegue até o `<dependencies>..</dependencies>` e verifique se as seguintes dependências estão definidas:

   ```xml
   <dependencies>
       ...
       <!-- Testing -->
       <dependency>
           <groupId>org.junit</groupId>
           <artifactId>junit-bom</artifactId>
           <version>5.5.2</version>
           <type>pom</type>
           <scope>import</scope>
       </dependency>
       <dependency>
           <groupId>org.slf4j</groupId>
           <artifactId>slf4j-simple</artifactId>
           <version>1.7.25</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-core</artifactId>
           <version>2.25.1</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-junit-jupiter</artifactId>
           <version>2.25.1</version>
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
           <version>2.5.2</version>
           <scope>test</scope>
       </dependency>
       ...
   </dependencies>
   ```

1. Abra **aem-guides-wknd/core/pom.xml** e visualização de que as dependências de teste correspondentes estejam disponíveis:

   ```xml
   ...
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
   </dependency>
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
   </dependency>
   ...
   ```

   Uma pasta de origem paralela no projeto **principal** conterá os testes de unidade e quaisquer arquivos de teste de suporte. Essa pasta **de teste** fornece a separação das classes de teste do código-fonte, mas permite que os testes funcionem como se vivessem nos mesmos pacotes que o código-fonte.

## Criação do teste JUnit {#creating-the-junit-test}

Os testes de unidade normalmente mapeiam de 1 a 1 com classes Java. Neste capítulo, gravaremos um teste JUnit para o **BylineImpl.java**, que é o Modelo Sling que suporta o componente Byline.

![explorador de pacotes de teste de unidade](assets/unit-testing/core-src-test-folder.png)

*Local onde os testes de Unidade são armazenados.*

1. Podemos fazer isso no Eclipse, clicando com o botão direito do mouse na classe Java para testar e selecionando **Novo > Outro > Java > JUnit > Caso** de teste JUnit.

   ![Clique com o botão direito do mouse em BylineImpl.java para criar um teste de unidade](assets/unit-testing/junit-test-case-1.png)

1. Na primeira tela do assistente, valide o seguinte:

   * O tipo de teste JUnit é **Novo teste** Jupiter JUnit, pois essas são as dependências JUnit Maven configuradas no **pom.xml**.
   * A **embalagem** é o pacote java da classe que está sendo testada (`BylineImpl.java`)
   * A pasta Origem aponta para o projeto **principal** , (`aem-guides-wknd.core/src/test/java`) que instrui o Eclipse para onde os arquivos de teste de unidade são armazenados.
   * O stub `setUp()` de método será criado manualmente; veremos como isso é usado mais tarde.
   * E a classe em teste é `BylineImpl.java`, como esta é a classe Java que queremos testar.

   ![etapa 2 do assistente de teste da unidade](assets/unit-testing/junit-wizard-testcase.png)

   *Assistente de caso de teste JUnit - etapa 2*

1. Clique no botão **Avançar** na parte inferior do assistente.

   Esta próxima etapa ajuda na geração automática de métodos de teste. Normalmente, cada método público da classe Java tem pelo menos um método de teste correspondente, validando seu comportamento. Frequentemente, um teste de unidade terá vários métodos de teste testando um único método público, cada um representando um conjunto diferente de entradas ou estados.

   No assistente, selecione todos os métodos em `BylineImpl`, exceto `init()` qual é um método usado pelo Modelo Sling internamente (via `@PostConstruct`). Nós testaremos o teste com eficácia `init()` testando todos os outros métodos, já que os outros dependem da `init()` execução com sucesso.

   Novos métodos de teste podem ser adicionados a qualquer momento à classe de teste JUnit, esta página do assistente é meramente para conveniência.

   ![etapa 3 do assistente de teste da unidade](assets/unit-testing/junit-test-case-3.png)

   *Assistente de caso de teste JUnit (continuação)*

1. Clique no botão Finish (Concluir) na parte inferior do assistente para gerar o arquivo de teste JUnit5.
1. Verifique se o arquivo de teste JUnit5 foi criado na estrutura do pacote correspondente em **aem-guides-wknd.core** > **/src/test/java** como um arquivo chamado `BylineImplTest.java`.

## Revisando BylineImplTest.java {#reviewing-bylineimpltest-java}

Nosso arquivo de teste tem vários métodos gerados automaticamente. Neste ponto, não há nada AEM específico sobre esse arquivo de teste JUnit.

O primeiro método é o `public void setUp() { .. }` que é anotado com `@BeforeEach`.

A `@BeforeEach` anotação é uma anotação JUnit que instrui o teste JUnit em execução a executar este método antes de executar cada método de teste nesta classe.

Os métodos subsequentes são os próprios métodos de teste e são marcados como tal com a `@Test` anotação. Observe que, por padrão, todos os nossos testes estão configurados para falhar.

Quando essa classe de teste JUnit (também conhecida como caso de teste JUnit) é executada, cada método marcado com o `@Test` será executado como um teste que pode ser aprovado ou reprovado.

![BylineImplTest gerado](assets/unit-testing/bylineimpltest-new.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Execute o JUnit Test Case clicando com o botão direito do mouse no nome da classe e em **Run As > JUnit Test**.

   ![Executar como teste de junção](assets/unit-testing/run-as-junit-test.png)

   *Clique com o botão direito do mouse em BylineImplTests.java > Executar como > Teste JUnit*

1. Como esperado, todos os testes falharam.

   ![falha nos testes](assets/unit-testing/all-tests-fail.png)

   *Visualização JUnit no Eclipse > Janela > Mostrar Visualização > Java > JUnit*

## Revisando BylineImpl.java {#reviewing-bylineimpl-java}

Ao gravar testes de unidade, há duas abordagens principais:

* [Desenvolvimento](https://en.wikipedia.org/wiki/Test-driven_development)orientado por TDD ou teste, que envolve a gravação gradual dos testes de unidade, imediatamente antes do desenvolvimento da implementação; escreva um teste, escreva a implementação para fazer o teste passar.
* Implementação - primeiro desenvolvimento, que envolve o desenvolvimento de código de trabalho primeiro e, em seguida, a gravação de testes que validam esse código.

Neste tutorial, a última abordagem é usada (já que criamos um **BylineImpl.java** em funcionamento em um capítulo anterior). Por isso, temos de rever e compreender os comportamentos dos seus métodos públicos, mas também alguns dos seus detalhes de implementação. Isso pode parecer contrário, uma vez que um bom teste só deve se preocupar com os dados recebidos e os resultados, no entanto, quando se trabalha em AEM, há uma variedade de considerações de implementação que devem ser compreendidas para se construir os testes em andamento.

O TDD no contexto do AEM requer um nível de especialização e é melhor adotado por AEM desenvolvedores com capacidade para AEM desenvolvimento e teste de unidade do código AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30208/?quality=12&learn=on)

## Configuração AEM contexto de teste  {#setting-up-aem-test-context}

A maioria dos códigos escritos para AEM depende de APIs JCR, Sling ou AEM, que, por sua vez, exigem que o contexto de um AEM em execução seja executado corretamente.

Como os testes de unidade são executados na compilação, fora do contexto de uma instância AEM em execução, não há esse recurso. Para facilitar isso, o Mocks [AEM do](https://wcm.io/testing/aem-mock/usage.html) wcm.io cria um contexto simulado que permite que essas APIs atuem principalmente como se estivessem em execução em AEM.

1. Crie um contexto AEM usando **wcm.io&#39;s** `AemContext` em **BylineImplTest.java** adicionando-o como uma extensão JUnit decorada com `@ExtendWith` o arquivo **BylineImplTest.java** . A extensão cuida de todas as tarefas de inicialização e limpeza necessárias. Crie uma variável de classe para `AemContext` que possa ser usada para todos os métodos de teste.

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

   * O Modelo Sling do BylineImpl será registrado neste contexto
   * As estruturas de conteúdo do JCR Mock são criadas neste contexto
   * Os serviços OSGi personalizados podem ser registrados neste contexto
   * Fornece uma variedade de objetos de maquete e auxiliares comuns necessários, como objetos SlingHttpServletRequest, uma variedade de serviços de mock Sling e AEM OSGi, como ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, TagManager etc.
      * *Observe que nem todos os métodos para esses objetos são implementados!*
   * And [much more](https://wcm.io/testing/aem-mock/usage.html)!

   O **`ctx`** objeto servirá de ponto de entrada para a maior parte do nosso contexto simulado.

1. No `setUp(..)` método, que é executado antes de cada `@Test` método, defina um estado comum de teste de amostra:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** registra o Modelo Sling a ser testado, no modelo AEM contexto, para que possa ser instanciado nos `@Test` métodos.
   * **`load().json`** carrega as estruturas de recursos no contexto simulado, permitindo que o código interaja com esses recursos como se fossem fornecidos por um repositório real. As definições de recursos no arquivo **`BylineImplTest.json`** são carregadas no contexto JCR mock em **/content**.
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

   Este JSON define uma definição de recurso de modelo para o teste de unidade de componente Byline. Nesse ponto, o JSON tem o conjunto mínimo de propriedades necessário para representar um recurso de conteúdo de componente de Linha de identificação, o `jcr:primaryType` e `sling:resourceType`.

   Uma regra geral para eles ao trabalhar com testes de unidade é criar o conjunto mínimo de conteúdo de simulação, contexto e código necessário para satisfazer cada teste. Evite a tentação de construir um contexto simulado completo antes de escrever os testes, já que geralmente resulta em artefatos desnecessários.

   Agora, com a existência de **BylineImplTest.json**, quando `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` é executada, as definições de recurso mock são carregadas no contexto no caminho **/conteúdo.**

## Testando getName() {#testing-get-name}

Agora que temos uma configuração básica de contexto simulado, vamos escrever nosso primeiro teste para getName() **do** BylineImpl. Este teste deve garantir que o método **getName()** retorne o nome de criação correto armazenado na propriedade &quot;**name&quot;** do recurso.

1. Atualize o método **testGetName**() em **BylineImplTest.java** da seguinte forma:

   ```java
   import com.adobe.aem.guides.wknd.core.components.Byline;
   import static org.junit.jupiter.api.Assertions.assertEquals;
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

   * **`String expected`** define o valor esperado. Definiremos isso como &quot;**Jane**&quot;.
   * **`ctx.currentResource`** define o contexto do recurso mock para avaliar o código, de modo que este seja definido como **/content/byline** , pois é onde o recurso de conteúdo mock byline é carregado.
   * **`Byline byline`** instancia o Modelo de divisão em linha adaptando-o do objeto de solicitação de modelagem.
   * **`String actual`** chama o método que estamos testando `getName()`, no objeto do Modelo de divisão em linha.
   * **`assertEquals`** afirma que o valor esperado corresponde ao valor retornado pelo objeto Sling Model byline. Se esses valores não forem iguais, o teste falhará.

1. Executar o teste... E falha com um `NullPointerException`.

   Observe que esse teste NÃO falha porque nunca definimos uma `name` propriedade no teste JSON simulado, que fará com que o teste falhe, no entanto a execução do teste não chegou a esse ponto! Esse teste falha devido a uma falha `NullPointerException` no próprio objeto byline.

1. No vídeo [Reviewing BylineImpl.java](#reviewing-bylineimpl-java) acima, discutimos como se `@PostConstruct init()` lançar uma exceção isso impede que o Modelo Sling instancie, e isso é o que está acontecendo aqui.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Acontece que, embora o serviço OSGi ModelFactory seja fornecido via o `AemContext` (por meio do Contexto Apache Sling), nem todos os métodos são implementados, inclusive `getModelFromWrappedRequest(...)` que é chamado no `init()` método BylineImpl. Isso resulta em [AbstractMethodError](https://docs.oracle.com/javase/8/docs/api/java/lang/AbstractMethodError.html), que, por termo, causa `init()` falha, e a adaptação resultante do objeto `ctx.request().adaptTo(Byline.class)` é um objeto nulo.

   Como os modelos fornecidos não podem acomodar nosso código, precisamos implementar o contexto simulado nós mesmos. Para isso, podemos usar o Mockito para criar um objeto ModelFactory que retorna um objeto de imagem simulada quando `getModelFromWrappedRequest(...)` é chamado sobre ele.

   Como para instanciar o Modelo de Sling de Linha de identificação, esse contexto de modelo deve estar em vigor, podemos adicioná-lo ao `@Before setUp()` método. Também é necessário adicionar o objeto `MockitoExtension.class` à `@ExtendWith` anotação acima da classe **BylineImplTest** .

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
   
   import static org.junit.jupiter.api.Assertions.assertEquals;
   import static org.junit.jupiter.api.Assertions.fail;
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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** marca a classe Test Case (Caso de teste) a ser executada com a extensão [Jupiter JUnit](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) Mockito, que permite o uso das anotações @Mock para definir objetos mock no nível Class.
   * **`@Mock private Image`** cria um objeto modelo do tipo `com.adobe.cq.wcm.core.components.models.Image`. Observe que isso é definido no nível da classe para que, conforme necessário, `@Test` os métodos possam alterar seu comportamento, conforme necessário.
   * **`@Mock private ModelFactory`** cria um objeto modelo do tipo ModelFactory. Observe que este é um verdadeiro zombaria Mockito e não tem métodos implementados nele. Observe que isso é definido no nível da classe para que, conforme necessário, `@Test`os métodos possam alterar seu comportamento, conforme necessário.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registra o comportamento simulado para quando `getModelFromWrappedRequest(..)` é chamado no objeto modeloFactory. O resultado definido em `thenReturn (..)` é retornar o objeto mock Image. Observe que esse comportamento só é invocado quando: o primeiro parâmetro é igual ao objeto de solicitação `ctx`do, o segundo parâmetro é qualquer objeto Resource e o terceiro parâmetro deve ser a classe Core Components Image. Aceitamos qualquer Recurso porque ao longo de nossos testes estaremos definindo o `ctx.currentResource(...)` para vários recursos de simulação definidos no **BylineImplTest.json**. Observe que adicionamos o **rigor lenient()** porque, posteriormente, desejaremos substituir esse comportamento do ModelFactory.
   * **`ctx.registerService(..)`.** registra o objeto modeloFactory no AemContext, com a classificação de serviço mais alta. Isso é necessário, pois o ModelFactory usado no BylineImpl&#39;s `init()` é injetado por meio do `@OSGiService ModelFactory model` campo. Para que o AemContext injete o **nosso** objeto-maquete, que lida com chamadas para `getModelFromWrappedRequest(..)`, devemos registrá-lo como o Serviço de classificação mais alta desse tipo (ModelFactory).

1. Execute novamente o teste e, novamente, ele falhará, mas desta vez a mensagem é clara por que ele falhou.

   ![declaração de falha de nome de teste](assets/unit-testing/testgetname-failure-assertion.png)

   *falha testGetName() devido à asserção*

   Recebemos um **AssertionError** , o que significa que a condição de asserção no teste falhou, e ele nos informa que o valor **esperado é &quot;Jane Doe&quot;** , mas o valor **real é nulo**. Isso faz sentido, pois a propriedade &quot;**name&quot;** não foi adicionada à definição de recurso mock **/content/byline** em **BylineImplTest.json**, portanto, vamos adicioná-la:

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

1. Reexecute o teste e **`testGetName()`** agora passe!

## Testando getOccupations() {#testing-get-occupations}

Ok, ótimo! Nosso primeiro teste foi bem-sucedido! Vamos continuar e testar `getOccupations()`. Uma vez que a inicialização do contexto do modelo foi feita no `@Before setUp()`método, isso estará disponível para todos os `@Test` métodos neste caso de teste, incluindo `getOccupations()`.

Lembre-se de que esse método deve retornar uma lista ordenada alfabeticamente de ocupações (descendentes) armazenada na propriedade de ocupações.

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
   * **`ctx.currentResource`** define o recurso atual para avaliar o contexto com base na definição do recurso de mock em /content/byline. Isso garante que o **BylineImpl.java** seja executado no contexto de nosso recurso mock.
   * **`ctx.request().adaptTo(Byline.class)`** instancia o Modelo de divisão em linha adaptando-o do objeto de solicitação de modelagem.
   * **`byline.getOccupations()`** chama o método que estamos testando `getOccupations()`, no objeto do Modelo de divisão em linha.
   * **`assertEquals(expected, actual)`** afirma que a lista esperada é igual à lista real.

1. Lembre-se, assim como **`getName()`** acima, o **BylineImplTest.json** não define ocupações, então esse teste falhará se o executarmos, pois `byline.getOccupations()` retornará uma lista vazia.

   Atualize **BylineImplTest.json** para incluir uma lista de ocupações e elas serão definidas em ordem não alfabética para garantir que nossos testes validem se as ocupações são classificadas por **`getOccupations()`**.

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

   ![Obter o passe de Ocupações](assets/unit-testing/testgetoccupations-success.png)

   *testGetOccupations() passa*

## Teste isEmpty() {#testing-is-empty}

O último método a ser testado **`isEmpty()`**.

Testes `isEmpty()` são interessantes, pois exigem testes para uma variedade de condições. Revisando o **método de** BylineImpl.java `isEmpty()` , as seguintes condições devem ser testadas:

* Retornar true quando o nome estiver vazio
* Retornar true quando as ocupações forem nulas ou vazias
* Retorna true quando a imagem for nula ou não tiver um URL src
* Retornar falso quando o nome, as ocupações e a Imagem (com um URL src) estiverem presentes

Para isso, precisamos criar novos métodos de teste, cada um testando uma condição específica, bem como novas estruturas de recursos de maquete para `BylineImplTest.json` conduzir esses testes.

Observe que essa verificação nos permitiu ignorar o teste para quando, `getName()`e `getOccupations()` estão vazios, pois o comportamento esperado desse estado é testado via `getImage()` `isEmpty()`.

1. O primeiro teste testará a condição de um novo componente, que não tem propriedades definidas.

   Adicione uma nova definição de recurso a `BylineImplTest.json`, dando-lhe o nome semântico &quot;**vazio**&quot;

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

   **`"empty": {...}`** defina uma nova definição de recurso chamada &quot;vazio&quot; que tenha apenas um `jcr:primaryType` e `sling:resourceType`.

   Lembre-se de que carregamos `BylineImplTest.json` antes da execução de cada método de teste em `ctx` , para que essa nova definição de recurso esteja imediatamente disponível em testes em `@setUp`**/content/empty.**

1. Atualize `testIsEmpty()` da seguinte forma, definindo o recurso atual para a nova definição de recurso mock &quot;**vazio**&quot;.

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

   Para cada teste, é usada uma definição discreta de recurso mock, atualize **BylineImplTest.json** com as definições adicionais de recurso para **sem nome** e **sem ocupações**.

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

   **`testIsEmpty()`** faz testes em relação à definição vazia do recurso de maquete e afirma que isso `isEmpty()` é verdade.

   **`testIsEmpty_WithoutName()`** testa uma definição de recurso simulada que tem ocupações, mas nenhum nome.

   **`testIsEmpty_WithoutOccupations()`** testa uma definição de recurso simulada que tem um nome, mas não ocupa.

   **`testIsEmpty_WithoutImage()`** faz testes em uma definição de recurso simulada com um nome e ocupações, mas define a imagem simulada para retornar a nulo. Observe que queremos substituir o `modelFactory.getModelFromWrappedRequest(..)`comportamento definido em `setUp()` para garantir que o objeto de imagem retornado por esta chamada seja nulo. O recurso de bordas Mockito é restrito e não quer código duplicado. Portanto, definimos o modelo com **`lenient`** configurações para observar explicitamente que estamos substituindo o comportamento no `setUp()` método.

   **`testIsEmpty_WithoutImageSrc()`** faz testes em uma definição de recurso de simulação com um nome e ocupações, mas define a Imagem de simulação para retornar uma string em branco quando `getSrc()` é chamada.

1. Por fim, escreva um teste para garantir que **isEmpty()** retorne false quando o componente estiver configurado corretamente. Para essa condição, podemos reutilizar **/content/byline** que representa um componente de Byline totalmente configurado.

   ```java
   @Test
   public void testIsNotEmpty() {
   ctx.currentResource("/content/byline");
   when(image.getSrc()).thenReturn("/content/bio.png");
   
   Byline byline = ctx.request().adaptTo(Byline.class);
   
   assertFalse(byline.isEmpty());
   }
   ```

## Cobertura de código {#code-coverage}

Cobertura de código é a quantidade de código fonte coberta pelos testes de unidade. Os IDEs modernos fornecem ferramentas que verificam automaticamente qual código fonte é executado durante os testes da unidade. Embora a cobertura do código em si não seja um indicador da qualidade do código, é útil entender se existem áreas importantes do código-fonte não testadas por testes de unidade.

1. No Explorador de projetos do Eclipse, clique com o botão direito do mouse em **BylineImplTest.java** e selecione **Cobertura como > Teste JUnit**

   Verifique se a visualização de resumo da Cobertura está aberta (Janela > Mostrar Visualização > Outro > Java > Cobertura).

   Isso executará os testes de unidade dentro desse arquivo e fornecerá um relatório indicando a cobertura do código. A perfuração na classe e nos métodos dá indicações mais claras de quais partes do arquivo são testadas e quais não são.

   ![executar como cobertura de código](assets/unit-testing/bylineimpl-coverage.png)

   *Resumo da cobertura de código*

   O Eclipse fornece uma visualização rápida de quanto de cada classe e método é coberto pelo teste de unidade. A cor par do Eclipse codifica as linhas do código:

   * **Verde** é o código executado por pelo menos um teste
   * **Amarelo** indica uma ramificação que não é avaliada por nenhum teste
   * **Vermelho** indica o código que não é executado por nenhum teste

1. No relatório de cobertura, foi identificada a ramificação que é executada quando o campo de ocupação é nulo e retorna uma lista vazia, e nunca é avaliada. Isso é indicado pelo fato de as linhas 571 e 86 terem cor amarela, terem indicado uma ramificação do if/else não ser executada e a linha 75 em vermelho indicando que a linha de código nunca é executada.

   ![código de cores da cobertura](assets/unit-testing/coverage-color-coding.png)

1. Isso pode ser solucionado adicionando um teste para `getOccupations()` que afirma que uma lista vazia é retornada quando não há valor de ocupação no recurso. Adicione o novo método de teste a seguir a **BylineImplTests.java**.

   ```java
   @Test
   public void testGetOccupations_WithoutOccupations() {
       List<String> expected = Collections.emptyList();
   
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       List<String> actual = byline.getOccupations();
   
       assertEquals(expected, actual);
   }
   ```

   **`Collections.emptyList();`** define o valor esperado como uma lista vazia.

   **`ctx.currentResource("/content/empty")`** define o recurso atual como /content/empty, que sabemos que não tem uma propriedade de ocupação definida.

1. Reexecutando a Cobertura como, ele relata que **BylineImpl.java** está agora com 100% de cobertura, no entanto, ainda há uma ramificação que não é avaliada em isEmpty(), o que tem a ver novamente com as ocupações. Nesse caso, as ocupações == null estão sendo avaliadas, no entanto, o ocuations.isEmpty() não está sendo avaliado, pois não há definição de recurso mock definida `"occupations": []`.

   ![Cobertura com testGetOccupations_WithoutOccupations()](assets/unit-testing/getoccupations-withoutoccupations.png)

   *Cobertura com testGetOccupations_WithoutOccupations()*

1. Isso pode ser facilmente resolvido criando outro método de teste que é usado em uma definição de recurso de maquete que define as ocupações para a matriz vazia.

   Adicione uma nova definição de recurso de modelo a **BylineImplTest.json** , que é uma cópia de **&quot;sem ocupações&quot;** , e adicione uma propriedade de ocupação definida à matriz vazia, nomeando-a **&quot;sem ocupações-matriz vazia&quot;**.

   ```json
   "without-occupations-empty-array": {
      "jcr:primaryType": "nt:unstructured",
      "sling:resourceType": "wknd/components/content/byline",
      "name": "Jane Doe",
      "occupations": []
    }
   ```

   Crie um novo método **@Test** no `BylineImplTest.java` qual use esse novo recurso mock, asserções `isEmpty()` retorna true.

   ```java
   @Test
   public void testIsEmpty_WithEmptyArrayOfOccupations() {
       ctx.currentResource("/content/without-occupations-empty-array");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   ![Cobertura com testIsEmpty_WithEmptyArrayOfOccupations()](assets/unit-testing/testisempty_withemptyarrayofoccupations.png)

   *Cobertura com testIsEmpty_WithEmptyArrayOfOccupations()*

1. Com esta última adição, `BylineImpl.java` desfruta de 100% de cobertura de código com toda a sua definição de caminho condicional avaliada.

   Os testes validam o comportamento esperado de `BylineImpl` sem ao mesmo tempo em que dependem de um conjunto mínimo de detalhes de implementação.

## Execução de testes de unidade como parte da construção {#running-unit-tests-as-part-of-the-build}

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

## Revisar o código {#review-the-code}

Visualização o código finalizado no [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente no bloco Git `unit-testing/solution`.
