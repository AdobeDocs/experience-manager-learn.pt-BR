---
title: Parametrizar modelos do Sling a partir do HTL
description: Saiba como transmitir parâmetros de HTL para um Modelo Sling em AEM.
version: Cloud Service
topic: Development
feature: Sling Model
role: Developer
jira: KT-15923
level: Intermediate, Experienced
exl-id: 5d852617-720a-4a00-aecd-26d0ab77d9b3
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Parametrizar modelos do Sling a partir do HTL

O Adobe Experience Manager (AEM) oferece uma estrutura robusta para a criação de aplicativos web dinâmicos e adaptáveis. Um de seus recursos avançados é a capacidade de parametrizar modelos Sling, melhorando sua flexibilidade e reutilização. Este tutorial guiará você pela criação de um modelo Sling com parâmetros e seu uso em HTL (Linguagem de modelo de HTML) para renderizar o conteúdo dinâmico.

## Script HTL

Neste exemplo, o script HTL envia dois parâmetros para o Modelo Sling `ParameterizedModel`. O modelo manipula esses parâmetros em seu método `getValue()` e retorna o resultado para exibição.

Este exemplo passa dois parâmetros de Cadeia de Caracteres, no entanto, você pode passar qualquer tipo de valor ou objeto para o Modelo Sling, desde que o tipo de campo [Modelo Sling, anotado com `@RequestAttribute`](#sling-model-implementation) corresponda ao tipo de objeto ou valor passado do HTL.

```html
<sly data-sly-use.myModel="${'com.example.core.models.ParameterizedModel' @ myParameterOne='Hello', myParameterTwo='World'}"
     data-sly-test.isReady="${myModel.isReady()}">

    <p>
        ${myModel.value}
    </p>

</sly>

<sly data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!isReady}">
</sly>
```

- **Parametrização:** A instrução `data-sly-use` cria uma instância de `ParameterizedModel` com `myParameterOne` e `myParameterTwo`.
- **Renderização Condicional:** `data-sly-test` verifica se o modelo está pronto antes de exibir o conteúdo.
- **Chamada de Espaço Reservado:** O `placeholderTemplate` trata de casos em que o modelo não está pronto.

## Implementação do modelo Sling

Veja como implementar o Modelo do Sling:

```java
package com.example.core.models.impl;

import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.SlingHttpServletRequest;
import org.osgi.service.component.annotations.Model;
import org.osgi.service.component.annotations.RequestAttribute;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Model(
    adaptables = { SlingHttpServletRequest.class },
    adapters = { ParameterizedModel.class }
)
public class ParameterizedModelImpl implements ParameterizedModel {
    private static final Logger log = LoggerFactory.getLogger(ParameterizedModelImpl.class);

    // Use the @RequestAttribute annotation to inject the parameter set in the HTL.
    // Note that the Sling Model field can be any type, but must match the type of object or value passed from HTL.
    @RequestAttribute
    private String myParameterOne;

    // If the HTL parameter name is different from the Sling Model field name, use the name attribute to specify the HTL parameter name
    @RequestAttribute(name = "myParameterTwo")
    private String mySecondParameter;

    // Do some work with the parameter values
    @Override
    public String getValue() {
        return myParameterOne + " " + mySecondParameter + ", from the parameterized Sling Model!";
    }

    @Override
    public boolean isReady() {
        return StringUtils.isNotBlank(myParameterOne) && StringUtils.isNotBlank(mySecondParameter);
    }
}
```

- **Anotação de Modelo:** A anotação `@Model` designa essa classe como um Modelo Sling, adaptável de `SlingHttpServletRequest` e implementando a interface `ParameterizedModel`.
- **Atributos da Solicitação:** a anotação `@RequestAttribute` injeta parâmetros HTL no modelo.
- **Métodos:** `getValue()` concatena os parâmetros, e `isReady()` verifica se os parâmetros não estão em branco.

A interface `ParameterizedModel` é definida da seguinte maneira:

```java
package com.example.core.models;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface ParameterizedModel {
    /**
     * Get an example string value from the model. This value is the concatenation of the two parameters.
     * 
     * @return the value of the model
     */
    String getValue();

    /**
     * Check if the model is ready to be used.
     *
     * @return {@code true} if the model is ready, {@code false} otherwise
     */
    boolean isReady();
}
```

## Exemplo de saída

Com os parâmetros `"Hello"` e `"World"`, o script HTL gera a seguinte saída:

```html
<p>
    Hello World, from the parameterized Sling Model!
</p>
```

Isso demonstra como os modelos Sling parametrizados em AEM podem ser influenciados com base nos parâmetros de entrada fornecidos por HTL.
