---
title: Java&trade de entrega de imagem otimizada para a Web; APIs
description: Saiba como usar AEM o Java&trade de entrega de imagem otimizada para a Web do as a Cloud Service; APIs para desenvolver experiências da Web de alto desempenho.
version: Cloud Service
feature: APIs, Sling Model, OSGI, HTL or HTML Template Language
topic: Performance, Development
role: Architect, Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-03-30T00:00:00Z
jira: KT-13014
thumbnail: KT-13014.jpeg
source-git-commit: 14d89d1a3c424de044df4f6d74546788256fa383
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 2%

---


# APIs Java™ de entrega de imagem otimizada para a Web

Saiba como usar AEM APIs Java™ de entrega de imagem otimizada para a Web da as a Cloud Service para desenvolver experiências da Web de alto desempenho.

AEM suporte as a Cloud Service [entrega de imagem otimizada para a Web](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html?lang=pt-BR) que gera automaticamente representações otimizadas da Web de imagens de ativos. A entrega de imagens otimizadas para a Web pode ser usada em três abordagens principais:

1. [Usar AEM componentes principais do WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)
2. Criar componente personalizado que [estende AEM componente de imagem do Componente WCM principal](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html#tackling-the-image-problem)
3. Crie um componente personalizado que usa a API Java™ AssetDelivery para gerar URLs de imagem otimizadas para a Web.

Este artigo explora o uso de APIs Java™ de imagem otimizada para a Web em um componente personalizado, de uma maneira que permite que o baseado em código funcione em AEM as a Cloud Service e no SDK AEM.

## APIs Java™

O [API AssetDelivery](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/cq/wcm/spi/AssetDelivery.html) é um serviço OSGi que gera URLs de entrega otimizadas para a Web para ativos de imagem. `AssetDelivery.getDeliveryURL(...)` as opções permitidas são [documentado aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html#can-i-use-web-optimized-image-delivery-with-my-own-component%3F).

O `AssetDelivery` O Serviço OSGi só é satisfeito quando executado em AEM as a Cloud Service. No AEM SDK, as referências ao `AssetDelivery` Retorno de serviço OSGi `null`. É melhor usar condicionalmente o URL otimizado para a Web ao executar em AEM as a Cloud Service e usar um URL de imagem de fallback no SDK AEM. Normalmente, a renderização da Web do ativo é um fallback suficiente.


### Uso da API no serviço OSGi

Marque a opção`AssetDelivery` referência como opcional nos serviços OSGi personalizados, para que o serviço OSGi personalizado permaneça disponível AEM SDK.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@Reference(cardinality = ReferenceCardinality.OPTIONAL)
private volatile AssetDelivery assetDelivery;
```

### Uso da API no Modelo do Sling

Marque a opção`AssetDelivery` referência como opcional em Modelos personalizados do Sling para que o Modelo personalizado do Sling permaneça disponível AEM SDK.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@OSGiService(injectionStrategy = InjectionStrategy.OPTIONAL)
private AssetDelivery assetDelivery;
```

### Uso condicional da API

Retorne condicionalmente o URL da imagem otimizada para a Web ou o URL de fallback com base no `AssetDelivery` Disponibilidade do serviço OSGi. O uso condicional permite que o código funcione ao executar o código no SDK AEM.

```java
if (assetDelivery != null ) {
    // When running on AEM as a Cloud Service use the real web-optimized image URL.
    return assetDelivery.getDeliveryURL(...);
} else {
    // When running on AEM SDK, use some fallback image so the experience does not break.
    // What the fallback is up to you! 
    return getFallbackURL(...);
}
```

## Exemplo de código

O código a seguir cria um componente de exemplo que exibe uma lista de ativos de imagem usando URLs de imagem otimizadas para a Web.

Quando o código é executado AEM as a Cloud Service, as representações de imagem da Web otimizadas são usadas no componente personalizado.

![Imagens otimizadas na Web AEM as a Cloud Service](./assets/web-optimized-image-delivery-java-apis/cloud-service.png)

_AEM as a Cloud Service suporta a API AssetDelivery, de modo que a renderização Web otimizada é usada_

Quando o código é executado AEM SDK, as renderizações estáticas da Web menos ideais são usadas, permitindo que o componente funcione durante o desenvolvimento local.

![Imagens de fallback otimizadas pela Web AEM SDK](./assets/web-optimized-image-delivery-java-apis/aem-sdk.png)

_AEM SDK não é compatível com a API AssetDelivery, portanto, a renderização Web estática de fallback (PNG ou JPEG) é usada_

A implementação é dividida em três partes lógicas:

1. O `WebOptimizedImage` O serviço OSGi atua como um &quot;proxy inteligente&quot; para o AEM fornecido `AssetDelivery` Serviço OSGi que pode lidar com a execução AEM SDK as a Cloud Service e AEM.
2. O `ExampleWebOptimizedImages` O Modelo do Sling fornece lógica de negócios de coletar a lista de ativos de imagem e seus urls otimizados para a Web para exibição.
3. O `example-web-optimized-images` AEM Componente, implementa o HTL para exibir a lista de imagens otimizadas para a Web.

O código de exemplo abaixo pode ser copiado na base de código e atualizado conforme necessário.

### Serviço OSGi

O `WebOptimizedImage` O serviço OSGi é dividido em uma interface pública endereçável (`WebOptimizedImage`e uma implementação interna (`WebOptimizedImageImpl`). O `WebOptimizedImageImpl` retorna um URL de imagem otimizada para a Web ao ser executado em AEM as a Cloud Service e um URL de renderização da Web estática no SDK AEM, permitindo que o componente permaneça funcional no SDK AEM.

#### Interface

A interface define o contrato de Serviço OSGi com o qual outros códigos, como Modelos do Sling, podem interagir.

```java
package com.adobe.aem.guides.wknd.core.images;

import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.annotation.versioning.ProviderType;

import java.util.Map;

/**
 * OSGi Service that acts as a facade for the AssetDelivery API, such that a fallback can be automatically served on the AEM SDK.
 *
 * This service can be extended to provide additional functionality, such as srcsets, etc.
 */
@ProviderType
public interface WebOptimizedImage {
    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options);
}
```

#### Implementação

A implementação do Serviço OSGi inclui uma referência opcional para AEM `AssetDelivery` Serviço OSGi e lógica de fallback para selecionar um URL de imagem adequado quando `AssetDelivery` é `null` no SDK do AEM. A lógica de fallback pode ser atualizada com base nos requisitos.

```java
package com.adobe.aem.guides.wknd.core.images.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.cq.wcm.spi.AssetDelivery;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.api.Rendition;
import com.day.cq.dam.api.RenditionPicker;
import com.day.cq.dam.commons.util.DamUtil;
import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;

import java.util.Map;
@Component
public class WebOptimizedImageImpl implements WebOptimizedImage {
    private static final String DEFAULT_FORMAT = "webp";
    @Reference(cardinality = ReferenceCardinality.OPTIONAL)
    private volatile AssetDelivery assetDelivery;

    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    @Override
    public String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        if (assetDelivery != null) {
            return getWebOptimizedUrl(resourceResolver, path, options);
        } else {
            return getFallbackUrl(resourceResolver, path);
        }
    }
    /**
     * Uses the AssetDelivery API to get the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    private String getWebOptimizedUrl(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        // These 3 options are required for the AssetDelivery API to work, else it will return null
        options.put("path", asset.getPath());
        options.put("format", StringUtils.defaultString((String) options.get("format"), DEFAULT_FORMAT));
        options.put("seoname", StringUtils.defaultString((String) options.get("seoname"), asset.getName()));

        // The resource only provides the security context into AEM so the asset's UUID can be looked up for the Web Optimized Image URL
        return assetDelivery.getDeliveryURL(resource, options);
    }

    /**
     * Fallback to the static web rendition if the AssetDelivery API is not available, meaning the code is running on the AEM SDK.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @return the path to the web rendition
     */
    private String getFallbackUrl(ResourceResolver resourceResolver, String path) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        return asset.getRendition(WebRenditionPicker).getPath();
    }

    /**
     * Picks the web rendition of the asset.
     */
    private static final RenditionPicker WebRenditionPicker = new RenditionPicker() {
        @Override
        public Rendition getRendition(Asset asset) {
            return asset.getRenditions().stream().filter(rendition -> StringUtils.startsWith(rendition.getName(), "cq5dam.web.")).findFirst().orElse(asset.getOriginal());
        }
    };
}
```

### Modelo Sling

O `ExampleWebOptimizedImages` O Modelo Sling é dividido em uma interface pública endereçável (`ExampleWebOptimizedImages`e uma implementação interna (`ExampleWebOptimizedImagesImpl`);

O `ExampleWebOptimizedImagesImpl` O Modelo do Sling coleta a lista de ativos de imagem a serem exibidos e chama o `WebOptimizedImage` Serviço OSGi para obter o URL da imagem otimizada para a Web. Como esse Modelo do Sling representa um Componente AEM, ele tem os métodos habituais, como `isEmpty()`, `getId()`e `getData()` no entanto, esses métodos não são diretamente relevantes para o uso de imagens otimizadas para a Web.

#### Interface

A interface define o contrato do Modelo do Sling com o qual outros códigos, como HTL, podem interagir.

```java
package com.adobe.aem.guides.wknd.core.models;

import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.fasterxml.jackson.annotation.JsonProperty;

import java.util.List;

public interface ExampleWebOptimizedImages {

    /**
     * @return a list of web optimized images for the component to display. Each item in the list has necessary information to render the image.
     */
    List<Img> getImages();

    /**
     * @return true if this component has no images to display.
     */
    boolean isEmpty();

    /**
     * @return String representing the unique identifier of the ExampleWebOptimizedImages component on a page
     */
    String getId();

    /**
     * @return JSON data to populate the data layer
     */
    @JsonProperty("dataLayer")
    default ComponentData getData() {
        return null;
    }

    /**
     * Describes a web optimized image.
     */
    interface Img {
        /**
         * @return the URL to the web optimized rendition of the image.
         */
        String getSrc();

        /**
         * @return the alt text of the web optimized image.
         */
        String getAlt();

        /**
         * @return the height of the web optimized image.
         */
        String getHeight();
        /**
         * @return the width of the web optimized image.
         */
        String getWidth();
    }
}
```

#### Implementação

O Modelo do Sling usa o `WebOptimizeImage` Serviço OSGi para coletar os URLs de imagem otimizada para a Web para os ativos de imagem que seu componente exibe.

Neste exemplo, uma consulta simples é usada para coletar ativos de imagem.

```java
package com.adobe.aem.guides.wknd.core.models.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages;
import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
import com.adobe.cq.wcm.core.components.util.ComponentUtils;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.commons.util.DamUtil;
import com.day.cq.wcm.api.Page;
import com.day.cq.wcm.api.components.ComponentContext;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.models.annotations.DefaultInjectionStrategy;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.Required;
import org.apache.sling.models.annotations.injectorspecific.OSGiService;
import org.apache.sling.models.annotations.injectorspecific.ScriptVariable;
import org.apache.sling.models.annotations.injectorspecific.Self;

import java.util.*;

@Model(
        adaptables = {SlingHttpServletRequest.class},
        adapters = {ExampleWebOptimizedImages.class},
        resourceType = {ExampleWebOptimizedImagesImpl.RESOURCE_TYPE},
        defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
)
public class ExampleWebOptimizedImagesImpl implements ExampleWebOptimizedImages {

    protected static final String RESOURCE_TYPE = "wknd/components/example-web-optimized-images";

    private static final int MAX_RESULTS = 10;

    @Self
    @Required
    private SlingHttpServletRequest request;

    @OSGiService
    private WebOptimizedImage webOptimizedImage;

    @ScriptVariable
    private Page currentPage;

    @ScriptVariable
    protected ComponentContext componentContext;

    private List<Img> images;

    // XPath query to find image assets to display
    private static final String XPATH_QUERY = "/jcr:root/content/dam/wknd-shared/en/adventures//element(*, dam:Asset) [ (jcr:contains(jcr:content/metadata/@dc:format, 'image/')) ]";
    @Override
    public List<Img> getImages() {

        if (images == null) {
            images = new ArrayList<>();

            // Set the AssetDelivery options to request a web-optimized rendition.
            // These options can be set as required by the implementation (Dialog, pass in from HTL via @RequestAttribute)
            final Map<String, Object> options = new HashMap<>();
            options.put("format", "webp");
            options.put("preferwebp", "true");
            options.put("width", "350");
            options.put("height", "350");

            final Iterator<Resource> results = request.getResourceResolver().findResources(XPATH_QUERY, "xpath");

            while (results.hasNext() && images.size() < MAX_RESULTS) {
                Resource resource = results.next();
                Asset asset = DamUtil.resolveToAsset(resource);

                // Get the image URL; the web-optimized rendition on AEM as a Cloud Service, or the static web rendition fallback on AEM SDK
                final String url = webOptimizedImage.getDeliveryURL(request.getResourceResolver(), resource.getPath(), options);

                // Add the image to the list that is passed to the HTL component to display
                // We'll add some extra attributes so that the HTL can display the image in a performant, SEO-friendly, and accessible way
                // ImgImpl can be extended to add additional attributes, such as srcset, etc.
                images.add(new ImgImpl(url, asset.getName(), (String) options.get("height"), (String) options.get("width")));
            }
        }

        return this.images;
    }

    @Override
    public boolean isEmpty() {
        return getImages().isEmpty();
    }

    @Override
    public String getId() {
        return ComponentUtils.getId(request.getResource(), currentPage, componentContext);
    }

    @Override
    public ComponentData getData() {
        if (ComponentUtils.isDataLayerEnabled(request.getResource())) {
            return DataLayerBuilder.forComponent()
                    .withId(() -> getId())
                    .withType(() -> RESOURCE_TYPE)
                    .build();
        }
        return null;
    }

    class ImgImpl implements Img {
        private final String url;
        private final String alt;
        private final int height;
        private final int width;

        public ImgImpl(String url, String alt, String height, String width) {
            this.url = url;
            this.alt = alt;
            this.height = Integer.parseInt(height);
            this.width = Integer.parseInt(width);
        }

        @Override
        public String getSrc() {
            return url;
        }

        @Override
        public String getAlt() {
            return alt;
        }

        @Override
        public String getHeight() {
            return height + "px";
        }

        @Override
        public String getWidth() {
            return width + "px";
        }
    }
}
```

### Componente AEM

Um Componente AEM é vinculado ao tipo de recurso Sling do `WebOptimizedImagesImpl` Implementação do Sling Model e é responsável pela exibição da lista de imagens.



O componente recebe uma lista de `Img` objetos via `getImages()` que incluem as imagens WEBP otimizadas para a Web ao executar em AEM as a Cloud Service . O componente recebe uma lista de `Img` objetos via `getImages()` que incluem imagens estáticas da Web PNG/JPEG ao executar em AEM SDK.

#### HTL

O HTL usa a variável `WebOptimizedImages` Modelo do Sling e renderiza a lista de  `Img` objetos retornam por `getImages()`.

```html
<style>
    .cmp-example-web-optimized-images__list {
        width: 100%;
        list-style: none;
        padding: 0;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        gap: 2rem;
    }

    .cmp-example-web-optimized-images-list__item {
        margin: 0;
        padding: 0;
    }
</style>

<div data-sly-use.exampleImages="com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages"
     data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-test.hasContent="${!exampleImages.empty}"
     data-cmp-data-layer="${exampleImages.data.json}">

    <h3>Example web-optimized images</h3>

    <ul class="cmp-example-web-optimized-images__list"
        data-sly-list.item="${exampleImages.images}">
        <li class="cmp-example-web-optimized-images-list__item">
            <img class="cmp-example-web-optimized-images__image"
                 src="${item.src}"
                 alt="${item.alt}"
                 width="${item.width}"/>
        </li>
    </ul>
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent, classAppend='cmp-example-web-optimized-images'}"></sly>
```