---
title: Criar um modelo do Sling para o componente
description: Criar um modelo do Sling para o componente
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: f4a18f02-61a2-4fa3-bfbb-41bf696cd2a8
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---

# Criar um modelo do Sling para o componente

Um Modelo Sling no AEM é uma estrutura baseada em Java usada para simplificar o desenvolvimento da lógica de back-end para componentes. Ele permite que os desenvolvedores mapeiem dados de recursos do AEM (nós JCR) para objetos Java usando anotações, fornecendo uma maneira limpa e eficiente de lidar com dados dinâmicos para componentes.
Essa classe, ChannelsDownImpl, é uma implementação da interface CountryDropDown em um projeto AEM (Adobe Experience Manager). Ele fornece um componente suspenso em que os usuários podem selecionar um país com base em seu continente selecionado. Os dados suspensos são carregados dinamicamente de um arquivo JSON armazenado no AEM DAM (Digital Asset Manager).

**Campos na Classe**

* **multiSelect**: indica se a lista suspensa permite várias seleções.
Injetado das propriedades do componente usando @ValueMapValue com um valor padrão false.
* **request**: representa a solicitação HTTP atual. Útil para acessar informações específicas do contexto.
* **continente**: armazena o continente selecionado para a lista suspensa (por exemplo, &quot;ásia&quot;, &quot;europa&quot;).
Inserido na caixa de diálogo de propriedades do componente, com um valor padrão de &quot;asia&quot; se nenhum for fornecido.
* **resourceResolver**:usado para acessar e manipular recursos no repositório do AEM.
* **jsonData**: um JSONObject que armazena os dados analisados do arquivo JSON correspondente ao continente selecionado.

**Métodos na classe**

* **getContinent()** Um método simples para retornar o valor do campo de continente.
Registra o valor sendo retornado para fins de depuração.
* Método do ciclo de vida **init()** anotado com @PostConstruct, executado depois que a classe é construída e as dependências são inseridas. Constrói dinamicamente o caminho para o arquivo JSON com base no valor do continente.
Busca o arquivo JSON do DAM do AEM usando o resourceResolver.
Adapta o arquivo a um Ativo, lê seu conteúdo e o analisa em um JSONObject.
Registra erros ou avisos durante esse processo.
* **getEnums()** Recupera todas as chaves (códigos de país) dos dados JSON analisados.
Classifica as chaves alfabeticamente e as retorna como uma matriz.
Registra o número de códigos de país sendo retornados.
* **getEnumNames()** Extrai todos os nomes de países dos dados JSON analisados.
Classifica os nomes alfabeticamente e os retorna como uma matriz.
Registra o número total de países e cada nome de país recuperado.
* **isMultiSelect()** Retorna o valor do campo multiSelect para indicar se a lista suspensa permite várias seleções.



```java
package com.aem.customcorecomponent.core.models.impl;
import com.adobe.cq.export.json.ComponentExporter;
import com.adobe.cq.export.json.ExporterConstants;
import com.adobe.cq.forms.core.components.internal.form.ReservedProperties;
import com.adobe.cq.forms.core.components.util.AbstractOptionsFieldImpl;
import com.aem.customcorecomponent.core.models.CountriesDropDown;
import com.day.cq.dam.api.Asset;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.models.annotations.Default;
import org.apache.sling.models.annotations.Exporter;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.InjectionStrategy;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
import org.json.JSONObject;
import javax.annotation.PostConstruct;
import javax.inject.Inject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import java.util.*;
import java.util.stream.Collectors;

//@Model(adaptables = SlingHttpServletRequest.class,adapters = CountriesDropDown.class,defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)

@Model(
        adaptables = { SlingHttpServletRequest.class, Resource.class },
        adapters = { CountriesDropDown.class,
                ComponentExporter.class },
        resourceType = { "corecomponent/components/adaptiveForm/countries" })
@Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)

public class CountriesDropDownImpl extends AbstractOptionsFieldImpl implements CountriesDropDown {
    @ValueMapValue(injectionStrategy = InjectionStrategy.OPTIONAL, name = ReservedProperties.PN_MULTISELECT)
    @Default(booleanValues = false)
    protected boolean multiSelect;

    private static final Logger logger = LoggerFactory.getLogger(CountriesDropDownImpl.class);
    @Inject
    SlingHttpServletRequest request;

    @Inject
    @Default(values = "asia")
    public String continent;
    @Inject
    private ResourceResolver resourceResolver;
    private JSONObject jsonData;
    public String getContinent()
    {
        logger.info("Returning continent");
        return continent;
    }
    @PostConstruct
    public void init() {

        String jsonPath = "/content/dam/corecomponent/" + getContinent() + ".json"; // Update path as needed
        logger.info("Initializing JSON data for continent: {}", getContinent());

        try {
            // Fetch the JSON resource
            Resource jsonResource = resourceResolver.getResource(jsonPath);
            if (jsonResource != null) {
                // Adapt the resource to an Asset
                Asset asset = jsonResource.adaptTo(Asset.class);
                if (asset != null) {
                    // Get the original rendition and parse it as JSON
                    try (InputStream inputStream = asset.getOriginal().adaptTo(InputStream.class);
                         BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream, StandardCharsets.UTF_8))) {

                        String jsonString = reader.lines().collect(Collectors.joining());
                        jsonData = new JSONObject(jsonString);
                        logger.info("Successfully loaded JSON data for path: {}", jsonPath);
                    }
                } else {
                    logger.warn("Failed to adapt resource to Asset at path: {}", jsonPath);
                }
            } else {
                logger.warn("Resource not found at path: {}", jsonPath);
            }
        } catch (Exception e) {
            logger.error("An error occurred while initializing JSON data for path: {}", jsonPath, e);
        }
    }

    @Override
    public Object[] getEnums() {
        Set<String> keySet = jsonData.keySet();


// Convert the set of keys to a sorted array
        String[] countryCodes = keySet.toArray(new String[0]);
        Arrays.sort(countryCodes);

        logger.debug("Returning sorted country codes: " + countryCodes.length);

        return countryCodes;


    }

    @Override
    public String[] getEnumNames() {
        String[] countries = new String[jsonData.keySet().size()];
        logger.info("Fetching countries - Total number: " + countries.length);

// Populate the array with country names
        int index = 0;
        for (String code : jsonData.keySet()) {
            String country = jsonData.getString(code);
            logger.debug("Retrieved country: " + country);
            countries[index++] = country;
        }

// Sort the array alphabetically
        Arrays.sort(countries);
        logger.debug("Returning " + countries.length + " sorted countries");

        return countries;

    }

    @Override
    public Boolean isMultiSelect() {
        return multiSelect;
    }
}
```

## Próximas etapas

[Criar, implantar e testar](./build.md)
