---
title: Práticas recomendadas para API do Java&trade; no AEM
description: O AEM é construído em uma pilha avançada de software de código aberto que expõe muitas APIs Java&trade; para uso durante o desenvolvimento. Este artigo explora as principais APIs e quando e por que elas devem ser usadas.
version: Experience Manager 6.4, Experience Manager 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
doc-type: Article
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
last-substantial-update: 2022-06-24T00:00:00Z
thumbnail: aem-java-bp.jpg
duration: 416
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1726'
ht-degree: 0%

---

# Práticas recomendadas da API Java™

O Adobe Experience Manager (AEM) é construído em uma pilha avançada de software de código aberto que expõe muitas APIs Java™ para uso durante o desenvolvimento. Este artigo explora as principais APIs e quando e por que elas devem ser usadas.

O AEM é construído em quatro conjuntos principais de APIs Java™.

* **Adobe Experience Manager (AEM)**

   * Abstrações de produto, como páginas, ativos, fluxos de trabalho etc.

* **Apache Sling Web Framework**

   * REST e abstrações baseadas em recursos, como recursos, mapas de valores e solicitações HTTP.

* **JCR (Apache Jackrabbit Oak)**

   * Dados e abstrações de conteúdo, como nó, propriedades e sessões.

* **OSGi (Apache Felix)**

   * Abstrações do contêiner de aplicativo OSGi, como serviços e componentes (OSGi).

## Preferência da API Java™ &quot;regra geral&quot;

A regra geral é preferir as APIs/abstrações na seguinte ordem:

1. **AEM**
1. **Sling**
1. **JCR**
1. **OSGi**

Se uma API for fornecida pelo AEM, prefira ela a [!DNL Sling], JCR e OSGi. Se a AEM não fornecer uma API, prefira [!DNL Sling] a JCR e OSGi.

Essa ordem é uma regra geral, o que significa que existem exceções. Os motivos aceitáveis para romper com essa regra são:

* Exceções bem conhecidas, conforme descrito abaixo.
* A funcionalidade necessária não está disponível em uma API de nível superior.
* Operar no contexto de código existente (código de produto personalizado ou AEM) que usa uma API menos preferencial, e o custo de mover para a nova API é injustificável.

   * É melhor usar de forma consistente a API de nível inferior do que criar uma combinação.

## APIs do AEM

* [**JavaDocs da API do AEM**](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html)

As APIs do AEM fornecem abstrações e funcionalidades específicas para casos de uso produzidos.

Por exemplo, as APIs [PageManager](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) e [Page](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) do AEM fornecem abstrações para nós `cq:Page` no AEM que representam páginas da Web.

Embora esses nós estejam disponíveis por meio de [!DNL Sling] APIs como Recursos e JCR APIs como Nós, as APIs do AEM fornecem abstrações para casos de uso comuns. O uso das APIs do AEM garante um comportamento consistente entre o AEM e o produto, além de personalizações e extensões para o AEM.

### com.adobe.&#42; vs. com.day.&#42; APIs

As APIs do AEM têm uma preferência dentro do pacote, identificada pelos seguintes pacotes Java™, em ordem de preferência:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

O pacote `com.adobe.cq` oferece suporte a casos de uso de produtos, enquanto o `com.adobe.granite` oferece suporte a casos de uso de plataformas entre produtos, como fluxo de trabalho ou tarefas (que são usadas em produtos: AEM Assets, Sites e assim por diante).

O pacote `com.day.cq` contém APIs &quot;originais&quot;. Essas APIs abordam abstrações e funcionalidades principais que existiam antes e/ou em torno da aquisição da [!DNL Day CQ] pela Adobe. Essas APIs são suportadas e devem ser evitadas, a menos que os pacotes `com.adobe.cq` ou `com.adobe.granite` NÃO forneçam uma alternativa (mais recente).

Novas abstrações, como [!DNL Content Fragments] e [!DNL Experience Fragments], são criadas no espaço `com.adobe.cq` em vez de `com.day.cq`, conforme descrito abaixo.

### APIs de consulta

O AEM oferece suporte a vários idiomas de consulta. Os três idiomas principais são [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath e [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=pt-BR).

A preocupação mais importante é manter uma linguagem de consulta consistente em toda a base de código, para reduzir a complexidade e os custos de compreensão.

Todas as linguagens de consulta têm efetivamente os mesmos perfis de desempenho, já que [!DNL Apache Oak] as compila para JCR-SQL2 para execução de consulta final, e o tempo de conversão para JCR-SQL2 é insignificante em comparação ao próprio tempo de consulta.

A API preferida é o [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=pt-BR), que é a abstração de mais alto nível e fornece uma API robusta para construção, execução e recuperação de resultados para consultas, além de fornecer o seguinte:

* Construção de consulta simples e parametrizada (parâmetros de consulta modelados como um Mapa)
* [APIs Java™ e HTTP nativas](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=pt-BR)
* [AEM Query Debugger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=pt-BR)
* [Predicados do AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-predicate-reference.html?lang=pt-BR) com suporte a requisitos de consulta comuns

* API extensível, permitindo o desenvolvimento de [predicados de consulta](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=pt-BR) personalizados
* JCR-SQL2 e XPath podem ser executados diretamente por meio de [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) e [APIs JCR](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html), retornando resultados como [[!DNL Sling] Recursos](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) ou [Nós JCR](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html), respectivamente.

>[!CAUTION]
>
>A API do AEM QueryBuilder vaza um objeto ResourceResolver. Para atenuar esse vazamento, siga esta [amostra de código](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).
>

## [!DNL Sling] APIs

* [**Apache [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) é a estrutura Web RESTful que serve de base para o AEM. [!DNL Sling] fornece roteamento de solicitações HTTP, modela nós JCR como recursos, fornece contexto de segurança e muito mais.

[!DNL Sling] APIs têm a vantagem adicional de serem compiladas para extensão, o que significa que geralmente é mais fácil e seguro aumentar o comportamento de aplicativos compilados usando APIs [!DNL Sling] do que as APIs JCR menos extensíveis.

### Usos comuns de [!DNL Sling] APIs

* Acessando nós JCR como [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) e acessando seus dados via [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Fornecendo contexto de segurança via [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Criando e removendo recursos por meio dos [métodos create/move/copy/delete](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html) de ResourceResolver.
* Atualizando propriedades por meio de [ModifiedValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Processamento de solicitação de construção de blocos de construção

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Filtros de Servlet](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Blocos de construção de processamento de trabalho assíncrono

   * [Manipuladores de Eventos e Trabalhos](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Agendador](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Modelos sling](https://sling.apache.org/documentation/bundles/models.html)

* [Usuários do serviço](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html?lang=pt-BR)

## APIs JCR

* **[JavaDocs do JCR 2.0](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

As [APIs JCR (Java™ Content Repository) 2.0](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) fazem parte de uma especificação para implementações JCR (no caso do AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/)). Toda implementação do JCR deve estar em conformidade com e implementar essas APIs, e, portanto, é a API de nível mais baixo para interagir com o conteúdo do AEM.

O JCR em si é um armazenamento de dados NoSQL hierárquico/em árvore que o AEM usa como seu repositório de conteúdo. O JCR tem uma grande variedade de APIs compatíveis, que vão desde a CRUD de conteúdo até a consulta de conteúdo. Apesar dessa API robusta, é raro que eles sejam preferidos sobre o AEM de nível superior e [!DNL Sling] abstrações.

Sempre prefira as APIs JCR às APIs do Apache Jackrabbit Oak. As APIs JCR são para ***interagir*** com um repositório JCR, enquanto as APIs do Oak são para ***implementar*** um repositório JCR.

### Equívocos comuns sobre APIs JCR

Embora o JCR seja o repositório de conteúdo do AEM, suas APIs NÃO são o método preferido para interagir com o conteúdo. Em vez disso, prefira as APIs do AEM (Página, Assets, Tag e assim por diante) ou as APIs de recurso do Sling, pois fornecem melhores abstrações.

>[!CAUTION]
>
>O uso amplo das interfaces de nó e sessão das APIs JCR em um aplicativo do AEM é baseado em código. Certifique-se de que as APIs [!DNL Sling] sejam usadas em seu lugar.

### Usos comuns de APIs JCR

* [Gerenciamento do controle de acesso](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html?lang=pt-BR)
* [Gerenciamento autorizado (usuários/grupos)](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* Observação JCR (escutando eventos JCR)
* Criação de estruturas de nó profundo

   * Embora as APIs do Sling sejam compatíveis com a criação de recursos, as APIs JCR têm métodos de conveniência em [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) e [JcrUtil](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/jcr/JcrUtil.html) que aceleram a criação de estruturas profundas.

## APIs OSGi

* [**JavaDocs OSGi R6**](https://docs.osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[JavaDocs de Anotações de Componente do OSGi Declarative Services 1.2](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[JavaDocs de Anotações de Metatipo do OSGi Declarative Services 1.2](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**JavaDocs do OSGi Framework**](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Há pouca sobreposição entre as APIs OSGi e as APIs de nível superior (AEM, [!DNL Sling] e JCR), e a necessidade de usar APIs OSGi é rara e requer um alto nível de conhecimento em desenvolvimento pela AEM.

### OSGi versus APIs Apache Felix

O OSGi define uma especificação que todos os contêineres OSGi devem implementar e estar em conformidade. A implementação OSGi do AEM, Apache Felix, também fornece várias de suas próprias APIs.

* Preferir APIs OSGi (`org.osgi`) a APIs Apache Felix (`org.apache.felix`).

### Usos comuns de APIs OSGi

* Anotações OSGi para declarar serviços e componentes OSGi.

   * Preferir [Anotações 1.2 do OSGi Declarative Services (DS)](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) a [Anotações Felix SCR](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) para declarar serviços e componentes OSGi

* APIs OSGi para componentes/serviços OSGi dinamicamente no código [desfazer/registrar](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * Prefira o uso de anotações OSGi DS 1.2 quando o gerenciamento condicional de serviço/componente OSGi não for necessário (o que geralmente ocorre).

## Exceções à regra

Veja a seguir exceções comuns às regras definidas acima.

### APIs OSGi

Ao lidar com abstrações OSGi de baixo nível, como definir ou ler nas propriedades do componente OSGi, as abstrações mais recentes fornecidas por `org.osgi` são preferíveis em relação às abstrações Sling de nível superior. As abstrações Sling concorrentes não foram marcadas como `@Deprecated` e sugerem a alternativa `org.osgi`.

Observe também que a definição do nó de configuração OSGi prefere `cfg.json` ao formato `sling:OsgiConfig`.

### APIs de ativos do AEM

* Preferir [`com.day.cq.dam.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/dam/api/package-summary.html) a [`com.adobe.granite.asset.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Embora as APIs do Assets `com.day.cq` forneçam ferramentas mais complementares para os casos de uso de gerenciamento de ativos da AEM.
   * As APIs do Granite Assets são compatíveis com casos de uso de gerenciamento de ativos de baixo nível (versão, relações).

### APIs de consulta

* O AEM QueryBuilder não oferece suporte a determinadas funções de consulta, como [sugestões](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), verificação ortográfica e dicas de índice, entre outras funções menos comuns. É preferível consultar com essas funções o JCR-SQL2.

### Registro do Servlet [!DNL Sling] {#sling-servlet-registration}

* Registro do servlet [!DNL Sling], prefira as anotações [OSGi DS 1.2 com @SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) a `@SlingServlet`

### Registro de filtro [!DNL Sling] {#sling-filter-registration}

* [!DNL Sling] registro de filtro, prefira [anotações do OSGi DS 1.2 com @SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) a `@SlingFilter`

## Trechos de código úteis

A seguir estão trechos de código Java™ úteis que ilustram as práticas recomendadas para casos de uso comuns usando APIs discutidas. Esses fragmentos também ilustram como mover de APIs menos preferenciais para APIs mais preferenciais.

### Sessão JCR para [!DNL Sling] ResourceResolver

#### Fechamento automático do Sling ResourceResolver

A partir do AEM 6.2, o ResourceResolver [!DNL Sling] é `AutoClosable` em uma instrução [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html). Usando esta sintaxe, uma chamada explícita para `resourceResolver .close()` não é necessária.

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

ResourceResolvers pode ser fechado manualmente em um bloco `finally`, se a técnica de fechamento automático mostrada acima não puder ser usada.

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

### [!DNL Sling] [!DNL Resource] para o AEM Asset

#### Método recomendado

A função `DamUtil.resolveToAsset(..)` resolve qualquer recurso sob `dam:Asset` para o objeto de Ativo, subindo na árvore, conforme necessário.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Abordagem alternativa

Adaptar um recurso a um Ativo requer que o próprio recurso seja o nó `dam:Asset`.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### Página de [!DNL Sling] Recurso para AEM

#### Método recomendado

`pageManager.getContainingPage(..)` resolve qualquer recurso sob `cq:Page` para o objeto Página, subindo na árvore, conforme necessário.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Abordagem alternativa {#alternative-approach-1}

Adaptar um recurso a uma Página requer que o próprio recurso seja o nó `cq:Page`.

```java
Page page = resource.adaptTo(Page.class);
```

### Ler propriedades de página do AEM

Use os getters do objeto Page para obter propriedades conhecidas (`getTitle()`, `getDescription()` e assim por diante) e `page.getProperties()` para obter o ValueMap `[cq:Page]/jcr:content` para recuperar outras propriedades.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Ler propriedades de metadados de ativos do AEM

A API de Ativo fornece métodos convenientes para a leitura de propriedades do nó `[dam:Asset]/jcr:content/metadata`. Este não é um ValueMap, o segundo parâmetro (valor padrão e conversão de tipo automático) não é compatível.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Ler propriedades de [!DNL Sling] [!DNL Resource] {#read-sling-resource-properties}

Quando as propriedades são armazenadas em locais (propriedades ou recursos relativos) aos quais as APIs do AEM (Página, Ativo) não podem acessar diretamente, os Recursos do [!DNL Sling] e os Mapas de Valores poderão ser usados para obter os dados.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

Nesse caso, talvez seja necessário converter o objeto AEM em um [!DNL Sling] [!DNL Resource] para localizar com eficiência a propriedade ou o subrecurso desejado.

#### Página do AEM para [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### Ativo AEM para [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Gravar propriedades usando ModisibleValueMap de [!DNL Sling]

Use [ModisibleValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) de [!DNL Sling] para gravar propriedades em nós. Isso só pode gravar no nó imediato (caminhos de propriedade relativos não são compatíveis).

Observe que a chamada para `.adaptTo(ModifiableValueMap.class)` requer permissões de gravação para o recurso, caso contrário, ela retornará um valor nulo.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### Criar uma página do AEM

Sempre use o PageManager para criar páginas, pois é necessário um Modelo de página para definir e inicializar páginas corretamente no AEM.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Criar um recurso [!DNL Sling]

O ResourceResolver dá suporte a operações básicas para a criação de recursos. Ao criar abstrações de nível superior (Páginas do AEM, Assets, Tags e assim por diante), use os métodos fornecidos pelos respectivos Gerentes.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Excluir um recurso [!DNL Sling]

ResourceResolver dá suporte à remoção de um recurso. Ao criar abstrações de nível superior (Páginas do AEM, Assets, Tags e assim por diante), use os métodos fornecidos pelos respectivos Gerentes.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
