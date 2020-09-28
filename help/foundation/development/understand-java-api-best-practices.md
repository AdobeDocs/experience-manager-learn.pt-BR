---
title: Entenda as práticas recomendadas da API Java no AEM
description: AEM é construído em uma pilha rica de software de código aberto que expõe muitas APIs Java para uso durante o desenvolvimento. Este artigo explora as principais APIs e quando e por que elas devem ser usadas.
version: 6.2, 6.3, 6.4, 6.5
sub-product: fundação, ativos, sites
feature: null
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
translation-type: tm+mt
source-git-commit: fcb47ee3878f6a789b2151e283431c4806e12564
workflow-type: tm+mt
source-wordcount: '2028'
ht-degree: 1%

---


# Entenda as práticas recomendadas da API Java

A Adobe Experience Manager (AEM) foi criada em uma pilha rica de software de código aberto que expõe muitas APIs Java para uso durante o desenvolvimento. Este artigo explora as principais APIs e quando e por que elas devem ser usadas.

AEM é criado em 4 conjuntos principais de APIs do Java.

* **Adobe Experience Manager (AEM)**

   * abstrações de produtos, como páginas, ativos, workflows etc.

* **[!DNL Apache Sling]Web Framework**

   * REST e abstrações baseadas em recursos, como recursos, mapas de valor e solicitações HTTP.

* **JCR ([!DNL Apache Jackrabbit Oak])**

   * abstrações de dados e conteúdo, como nó, propriedades e sessões.

* **[!DNL OSGi (Apache Felix)]**

   * abstrações do container do aplicativo OSGi, como serviços e componentes (OSGi).

## Preferência da API Java &quot;regra de polegar&quot;

A regra geral é preferir as APIs/abstrações da seguinte ordem:

1. **AEM**
1. **[!DNL Sling]**
1. **JCR**
1. **OSGi**

Se uma API for fornecida pelo AEM, prefira-a em vez de [!DNL Sling], JCR e OSGi. Se AEM não fornecer uma API, então prefira [!DNL Sling] em vez de JCR e OSGi.

Esta ordem é uma regra geral, o que significa que existem exceções. Os motivos aceitáveis para quebrar essa regra são:

* Exceções bem conhecidas, como descrito abaixo.
* A funcionalidade necessária não está disponível em uma API de nível superior.
* Operando no contexto do código existente (código de produto personalizado ou AEM) que usa uma API menos preferencial, e o custo para migrar para a nova API é injustificável.

   * É melhor usar consistentemente a API de nível inferior do que criar uma combinação.

## AEM APIs

* [**JavaDocs da API AEM**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM APIs fornecem abstrações e funcionalidades específicas para casos de uso produzidos.

Por exemplo, AEM [PageManager](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html) e APIs [Page](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html) fornecem abstrações para `cq:Page` nós em AEM que representam páginas da Web.

Embora esses nós estejam disponíveis por meio de [!DNL Sling] APIs como Recursos e JCR APIs como Nós, AEM APIs fornecem abstrações para casos de uso comuns. O uso das APIs AEM garante um comportamento consistente entre AEM produto, e personalizações e extensões para AEM.

### com.adobe.* vs com.day.* APIs

AEM APIs têm uma preferência intrapacote, identificada pelos seguintes pacotes Java, em ordem de preferência:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` suporta casos de uso de produtos, ao passo que `com.adobe.granite` suporta casos de uso de plataformas entre produtos, como fluxo de trabalho ou tarefas (que são usadas em produtos: AEM Assets, Sites etc.).

`com.day.cq` contém APIs &quot;originais&quot;. Essas APIs tratam das principais abstrações e funcionalidades que existiam antes e/ou em torno da aquisição do Adobe [!DNL Day CQ]. Essas APIs são suportadas e não devem ser evitadas, a menos que `com.adobe.cq` ou `com.adobe.granite` uma alternativa (mais nova) seja fornecida.

Novas abstrações como [!DNL Content Fragments] e [!DNL Experience Fragments] são construídas no `com.adobe.cq` espaço em vez de serem `com.day.cq` descritas abaixo.

### APIs de query

AEM suporta vários idiomas de query. Os 3 idiomas principais são [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath e [AEM Construtor](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html)de Query.

A preocupação mais importante é manter uma linguagem de query consistente em toda a base de códigos, para reduzir a complexidade e o custo de entender.

Todos os idiomas do query têm efetivamente os mesmos perfis de desempenho, já que [!DNL Apache Oak] os transpila para o JCR-SQL2 para execução final do query, e o tempo de conversão para o JCR-SQL2 é insignificante em comparação com o tempo do query propriamente dito.

A API preferencial é [AEM Construtor](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html)de Query, que é a abstração de mais alto nível e fornece uma API robusta para a construção, execução e recuperação de resultados de query, e fornece o seguinte:

* Construção de query simples e parametrizada (parâmetros de query modelados como um mapa)
* API [Java nativa e APIs HTTP](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [Depurador de Query OOTB](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [Previsões](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) OOTB suportando requisitos comuns de query

* API extensível, permitindo o desenvolvimento de predicados de [query personalizados](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* O JCR-SQL2 e o XPath podem ser executados diretamente via [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) e APIs [](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html)JCR, retornando os resultados para [[!DNL Sling] Recursos](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) ou Nós [](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html)JCR, respectivamente.

>[!CAUTION]
>
>AEM API do QueryBuilder vaza um objeto ResourceResolver. Para atenuar esse vazamento, siga esta amostra [de](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164)código.


## [!DNL Sling] APIs

* [**JavaDocs da API Apache[!DNL Sling]**](https://sling.apache.org/apidocs/sling10/)

[O Apache [!DNL Sling]](https://sling.apache.org/) é a estrutura web RESTful que sustenta AEM. [!DNL Sling] fornece roteamento de solicitação HTTP, modela nós JCR como recursos, fornece contexto de segurança e muito mais.

[!DNL Sling] As APIs têm o benefício adicional de serem criadas para extensão, o que significa que geralmente é mais fácil e seguro aumentar o comportamento de aplicativos criados com [!DNL Sling] APIs do que as APIs JCR menos extensíveis.

### Uso comum de [!DNL Sling] APIs

* Acessar nós JCR como [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) e acessar seus dados via [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Fornecendo contexto de segurança por meio do [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Criação e remoção de recursos por meio dos métodos [de](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)criação/movimentação/cópia/exclusão do ResourceResolver.
* Atualizando propriedades por meio do [ModivariableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Criando blocos componentes de processamento de solicitação

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Filtros Servlet](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Blocos básicos para processamento de trabalho assíncrono

   * [Manipuladores de evento e trabalho](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Programações](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Modelos Sling](https://sling.apache.org/documentation/bundles/models.html)

* [Usuários do serviço](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## APIs JCR

* **[JavaDocs JCR 2.0](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

As APIs [do](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html) JCR (Java Content Repository) 2.0 fazem parte de uma especificação para implementações do JCR (no caso de AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/)). Toda implementação do JCR deve estar em conformidade e implementar essas APIs e, portanto, é a API de nível mais baixo para interagir com AEM conteúdo.

O próprio JCR é um armazenamento de dados NoSQL hierárquico/baseado em árvore AEM usa como repositório de conteúdo. O JCR tem uma grande variedade de APIs compatíveis, que vão desde a CRUD de conteúdo até a consulta de conteúdo. Apesar desta API robusta, é raro que eles sejam preferidos em relação às AEM e [!DNL Sling] abstrações de nível superior.

Sempre prefira as APIs JCR em vez das APIs do Apache Jackrabbit Oak. As APIs do JCR são para ***interagir*** com um repositório do JCR, enquanto as APIs do Oak são para ***implementar*** um repositório do JCR.

### Conceitos errados comuns sobre APIs JCR

Embora o JCR seja AEM repositório de conteúdo, suas APIs NÃO são o método preferencial para interagir com o conteúdo. Em vez disso, prefere as APIs AEM (Página, Ativos, Tag etc.) ou APIs de recursos Sling, conforme proporcionam melhores abstrações.

>[!CAUTION]
>
>O uso amplo das interfaces de Sessão e Nó das APIs de JCR em um aplicativo AEM é o cheirante de código. Verifique se as [!DNL Sling] APIs não devem ser usadas.

### Uso comum de APIs JCR

* [gerenciamento de controles de acesso](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [Gerenciamento autorizado (usuários/grupos)](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* Observação do JCR (ouvindo eventos do JCR)
* Criação de estruturas de nó profundo

   * Embora as APIs Sling suportem a criação de recursos, as APIs JCR têm métodos de conveniência em [JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html) e [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html) que agilizam a criação de estruturas profundas.

## APIs OSGi

* [**JavaDocs OSGi R6**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[Anotações de componentes do OSGi Declarative Services 1.2 JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[JavaDocs de anotações de metatype do OSGi Declarative Services 1.2](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**JavaDocs da estrutura OSGi**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Há pouca sobreposição entre as APIs do OSGi e as APIs de nível superior (AEM, [!DNL Sling]e JCR), e a necessidade de usar as APIs do OSGi é rara e exige um alto nível de conhecimento AEM desenvolvimento.

### APIs OSGi vs Apache Felix

O OSGi define uma especificação à qual todos os container OSGi devem implementar e estar em conformidade. AEM implementação do OSGi, o Apache Felix, também fornece várias de suas próprias APIs.

* Preferir APIs OSGi (`org.osgi`) sobre APIs Apache Felix (`org.apache.felix`).

### Uso comum de APIs OSGi

* Anotações do OSGi para declaração de serviços e componentes do OSGi.

   * Preferir [OSGi Declarative Services (DS) 1.2 Anotações](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) sobre anotações [Felix SCR Anotações](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) para declaração de serviços e componentes OSGi

* APIs OSGi para [desfazer/registrar dinamicamente os serviços/componentes](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)OSGi no código.

   * Preferir o uso de anotações do OSGi DS 1.2 quando o gerenciamento condicional de serviço/componente do OSGi não for necessário (o que acontece com mais frequência).

## Exceções à regra

As exceções a seguir são comuns às regras definidas acima.

### AEM APIs de ativos

* Preferir [ por `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html)cima.

   * Embora as APIs de `com.day.cq` ativos forneçam ferramentas mais complementares para AEM casos de uso do gerenciamento de ativos.
   * As APIs do Granite Assets suportam casos de uso de gerenciamento de ativos de baixo nível (versão, relações).

### APIs de query

* AEM QueryBuilder não oferece suporte a determinadas funções de query, como [sugestões](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), verificação ortográfica e dicas de índice, entre outras funções menos comuns. É preferível query com essas funções JCR-SQL2.

### [!DNL Sling] Registro no servlet {#sling-servlet-registration}

* [!DNL Sling] registro no servlet, preferir anotações [OSGi DS 1.2 com @SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) sobre `@SlingServlet`

### [!DNL Sling] Registro do filtro {#sling-filter-registration}

* [!DNL Sling] registro de filtro, preferir anotações [OSGi DS 1.2 com @SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) em vez de `@SlingFilter`

## Trechos de código úteis

A seguir estão trechos úteis do código Java que ilustram as práticas recomendadas para casos de uso comuns usando APIs discutidas. Esses trechos também ilustram como mover de APIs menos preferenciais para APIs mais preferenciais.

### Sessão JCR para [!DNL Sling] ResourceResolver

#### Fechar automaticamente o Resolvedor de recursos Sling

Desde AEM 6.2, o [!DNL Sling] ResourceResolver está `AutoClosable` em uma instrução [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) . Usando essa sintaxe, não `resourceResolver .close()` é necessária uma chamada explícita para.

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

try (ResourceResolver resourceResolver = rrf.getResourceResolver(authInfo)) {
    // Do work with the resourceResolver
} catch (LoginException e) { .. }
```

#### Sling ResourceResolver fechado manualmente

ResourceResolvers pode ser fechado manualmente em um `finally` bloco, se a técnica de fechamento automático mostrada acima não puder ser usada.

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

ResourceResolver resourceResolver = null;

try {
    resourceResolver = rrf.getResourceResolver(authInfo);
    // Do work with the resourceResolver
} catch (LoginException e) {
   ...
} finally {
    if (resourceResolver != null) { resourceResolver.close(); }
}
```

### Caminho JCR para [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### Nó JCR para [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] para AEM ativo

#### Abordagem recomendada

`DamUtil.resolveToAsset(..)`resolve qualquer recurso sob o objeto Asset (Ativo) `dam:Asset` , subindo a árvore conforme necessário.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Abordagem alternativa

A adaptação de um recurso a um ativo exige que o próprio recurso seja o `dam:Asset` nó.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Recurso para AEM página

#### Abordagem recomendada

`pageManager.getContainingPage(..)` resolve qualquer recurso sob o objeto Page `cq:Page` , subindo a árvore conforme necessário.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Abordagem alternativa {#alternative-approach-1}

A adaptação de um recurso a uma Página exige que o próprio recurso seja o `cq:Page` nó.

```java
Page page = resource.adaptTo(Page.class);
```

### Ler AEM propriedades da página

Use os getters do objeto Page para obter propriedades conhecidas (`getTitle()`, `getDescription()`etc.) e `page.getProperties()` para obter o `[cq:Page]/jcr:content` ValueMap para recuperar outras propriedades.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Ler AEM propriedades de metadados do ativo

A API Asset fornece métodos convenientes para ler propriedades do `[dam:Asset]/jcr:content/metadata` nó. Observe que este não é um ValueMap, o segundo parâmetro (valor padrão e conversão de tipo automático) não é suportado.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Ler [!DNL Sling] propriedades [!DNL Resource] {#read-sling-resource-properties}

Quando as propriedades são armazenadas em locais (propriedades ou recursos relativos) onde as APIs AEM (Página, Ativo) não podem acessar diretamente, os [!DNL Sling] Recursos e os ValueMaps podem ser usados para obter os dados.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

Nesse caso, o objeto AEM pode ter que ser convertido em um objeto [!DNL Sling] [!DNL Resource] para localizar com eficiência a propriedade ou o subrecurso desejado.

#### AEM página para [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM Asset to [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Gravar propriedades usando ModiableValueMap [!DNL Sling]do

Use [!DNL Sling]ModiableValueMap [](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) para gravar propriedades em nós. Isso só pode gravar no nó imediato (caminhos de propriedade relativos não são suportados).

Observe que a chamada para `.adaptTo(ModifiableValueMap.class)` requer permissões de gravação para o recurso, caso contrário, ela retornará um valor nulo.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### Criar uma página AEM

Sempre use o PageManager para criar páginas conforme um Modelo de página é necessário para definir e inicializar corretamente Páginas no AEM.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Create a [!DNL Sling] Resource

O ResourceResolver oferece suporte a operações básicas para a criação de recursos. Ao criar abstrações de nível superior (Páginas AEM, Ativos, Tags etc.) usar os métodos fornecidos pelos respectivos gerentes.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Excluir um [!DNL Sling] recurso

O ResourceResolver suporta a remoção de um recurso. Ao criar abstrações de nível superior (Páginas AEM, Ativos, Tags etc.) usar os métodos fornecidos pelos respectivos gerentes.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
