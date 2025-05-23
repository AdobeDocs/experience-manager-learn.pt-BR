---
title: Entrega de imagens otimizadas para a Web APIs do Java&trade;
description: Saiba como usar as APIs do AEM as a Cloud Service para entrega de imagens otimizadas para a Web do Java&trade; para desenvolver experiências da Web de alto desempenho.
version: Experience Manager as a Cloud Service
feature: APIs, Sling Model, OSGI, HTL or HTML Template Language
topic: Performance, Development
role: Architect, Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-03-30T00:00:00Z
jira: KT-13014
thumbnail: KT-13014.jpeg
exl-id: c6bb9d6d-aef0-42d5-a189-f904bbbd7694
duration: 352
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '810'
ht-degree: 0%

---

# APIs Java™ de entrega de imagens otimizadas para a Web

Saiba como usar as APIs Java™ de entrega de imagens otimizadas para a Web da AEM as a Cloud Service para desenvolver experiências da Web de alto desempenho.

O AEM as a Cloud Service oferece suporte à [entrega de imagens otimizadas para a Web](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html?lang=pt-BR), que gera automaticamente representações otimizadas de imagens da Web de ativos. A entrega de imagens otimizadas para a Web pode ser usada em três abordagens principais:

1. [Usar Componentes WCM do AEM Core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)
2. Criar componente personalizado que [estende o componente de imagem do Componente WCM do AEM Core](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html?lang=pt-BR#tackling-the-image-problem)
3. Crie um componente personalizado que usa a API Java™ do Asset Delivery para gerar URLs de imagem otimizados para a Web.

Este artigo aborda o uso de APIs Java™ de imagem otimizadas para a Web em um componente personalizado, de maneira que permita que as APIs baseadas em código funcionem no AEM as a Cloud Service e no AEM SDK.

## APIs Java™

A [API AssetDelivery](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/cq/wcm/spi/AssetDelivery.html) é um serviço OSGi que gera URLs de entrega otimizadas para a Web para ativos de imagem. `AssetDelivery.getDeliveryURL(...)` as opções permitidas estão [documentadas aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html?lang=pt-BR#can-i-use-web-optimized-image-delivery-with-my-own-component%3F).

O Serviço OSGi `AssetDelivery` só é satisfeito durante a execução no AEM as a Cloud Service. No AEM SDK, as referências ao serviço OSGi `AssetDelivery` retornam `null`. É melhor usar condicionalmente o URL otimizado para a Web ao ser executado no AEM as a Cloud Service e usar um URL de imagem de fallback no AEM SDK. Normalmente, a representação da Web do ativo é um fallback suficiente.


### Uso da API no serviço OSGi

Marque a referência `AssetDelivery` como opcional nos Serviços OSGi personalizados para que o Serviço OSGi personalizado permaneça disponível no AEM SDK.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@Reference(cardinality = ReferenceCardinality.OPTIONAL)
private volatile AssetDelivery assetDelivery;
```

### Uso da API no modelo Sling

Marque a referência `AssetDelivery` como opcional em Modelos Sling personalizados para que o Modelo Sling personalizado permaneça disponível no AEM SDK.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@OSGiService(injectionStrategy = InjectionStrategy.OPTIONAL)
private AssetDelivery assetDelivery;
```

### Uso condicional da API

Retorne condicionalmente a URL da imagem otimizada para a Web ou a URL de fallback com base na disponibilidade do serviço OSGi `AssetDelivery`. O uso condicional permite que o código funcione ao executá-lo no AEM SDK.

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

Quando o código é executado no AEM as a Cloud Service, as representações de imagem da Web otimizadas para a Web são usadas no componente personalizado.

![Imagens otimizadas para a Web no AEM as a Cloud Service](./assets/web-optimized-image-delivery-java-apis/cloud-service.png)

_O AEM as a Cloud Service oferece suporte à API AssetDelivery, portanto, a representação da Web otimizada para a Web é usada_

Quando o código é executado no AEM SDK, as representações estáticas da Web menos ideais são usadas, permitindo que o componente funcione durante o desenvolvimento local.

![Imagens de fallback otimizadas para a Web no AEM SDK](./assets/web-optimized-image-delivery-java-apis/aem-sdk.png)

_O AEM SDK não oferece suporte à API AssetDelivery, portanto, a representação da web estática de fallback (PNG ou JPEG) é usada_

A implementação é dividida em três partes lógicas:

1. O serviço OSGi do `WebOptimizedImage` atua como um &quot;proxy inteligente&quot; para o Serviço OSGi do `AssetDelivery` fornecido pela AEM que pode lidar com a execução no AEM as a Cloud Service e no AEM SDK.
2. O Modelo Sling do `ExampleWebOptimizedImages` fornece lógica de negócios para coletar a lista de ativos de imagem e seus URLs otimizados para a Web para exibição.
3. O componente AEM `example-web-optimized-images` implementa o HTL para exibir a lista de imagens otimizadas para a Web.

O código de exemplo abaixo pode ser copiado em sua base de código e atualizado conforme necessário.

### Serviço OSGi

O serviço OSGi `WebOptimizedImage` está dividido em uma interface pública endereçável (`WebOptimizedImage`) e uma implementação interna (`WebOptimizedImageImpl`). O `WebOptimizedImageImpl` retorna uma URL de imagem otimizada para a Web ao ser executado no AEM as a Cloud Service e uma URL de representação da Web estática no AEM SDK, permitindo que o componente permaneça funcional no AEM SDK.

#### Interface

A interface define o contrato de serviço OSGi com o qual outros códigos, como Modelos Sling, podem interagir.

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

A implementação do Serviço OSGi inclui uma referência opcional ao serviço OSGi `AssetDelivery` da AEM e lógica de fallback para seleção de uma URL de imagem adequada quando `AssetDelivery` for `null` no AEM SDK. A lógica de fallback pode ser atualizada com base nos requisitos.

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

O Modelo Sling `ExampleWebOptimizedImages` está dividido em uma interface pública endereçável (`ExampleWebOptimizedImages`) e uma implementação interna (`ExampleWebOptimizedImagesImpl`);

O Modelo do Sling `ExampleWebOptimizedImagesImpl` coleta a lista de ativos de imagem a serem exibidos e invoca o Serviço OSGi `WebOptimizedImage` personalizado para obter a URL da imagem otimizada para a Web. Como esse Modelo Sling representa um componente do AEM, ele tem os métodos usuais como `isEmpty()`, `getId()` e `getData()`. No entanto, esses métodos não são diretamente relevantes para o uso de imagens otimizadas para a Web.

#### Interface

A interface define o contrato do Modelo do Sling com o qual outro código, como HTL, pode interagir.

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

O Modelo Sling usa o serviço OSGi `WebOptimizeImage` personalizado para coletar as URLs de imagem otimizadas para a Web para os ativos de imagem que seu componente exibe.

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

### Componente do AEM

Um componente do AEM está vinculado ao tipo de recurso Sling da implementação do Modelo Sling `WebOptimizedImagesImpl` e é responsável pela exibição da lista de imagens.



O componente recebe uma lista de `Img` objetos via `getImages()` que incluem as imagens WEBP otimizadas para a Web ao serem executadas no AEM as a Cloud Service. O componente recebe uma lista de objetos `Img` via `getImages()` que incluem imagens da Web PNG/JPEG estáticas quando executado no AEM SDK.

#### HTL

O HTL usa o Modelo Sling `WebOptimizedImages` e renderiza a lista de objetos `Img` retornados por `getImages()`.

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
