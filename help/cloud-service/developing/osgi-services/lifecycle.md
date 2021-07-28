---
title: Ciclo de vida do componente OSGi
description: Saiba mais sobre o ciclo de vida do componente OSGi, incluindo como vincular um serviço OSGi aos eventos de ciclo de vida ativados, modificados e desativados.
role: Developer
level: Beginner
topic: Desenvolvimento
kt: 8228
thumbnail: 335475.jpeg
source-git-commit: 680043f5717bf938bf6f0b960d9ed5939d13544c
workflow-type: tm+mt
source-wordcount: '95'
ht-degree: 5%

---


# Ciclo de vida do componente OSGi

Saiba mais sobre o ciclo de vida do componente OSGi, incluindo como vincular um serviço OSGi ao:

+ Ativar
+ Modificado
+ e desativar

...eventos do ciclo de vida.

>[!VIDEO](https://video.tv.adobe.com/v/335475/?quality=12&learn=on)

## Recursos

+ [@Ativate JavaDocs](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/component/annotations/Activate.html)
+ [@Modified JavaDocs](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/component/annotations/Modified.html)
+ [@Deactivate JavaDocs](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/component/annotations/Deactivate.html)

## Código

### ActivitiesImpl.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/impl/ActivitiesImpl.java`

```java
package com.adobe.aem.wknd.examples.core.adventures.impl;

import java.util.Random;

import com.adobe.aem.wknd.examples.core.adventures.Activities;

import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Deactivate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(
    service = { Activities.class }
)
public class ActivitiesImpl implements Activities {
    private static final Logger log = LoggerFactory.getLogger(ActivitiesImpl.class);

    private String[] activities;

    private final Random random = new Random();

    /**
     * @return the name of a random WKND adventure activity
     */
    public String getRandomActivity() {
        int randomIndex = random.nextInt(activities.length);
        return activities[randomIndex];
    }    

    @Activate
    protected void activate() {
        this.activities = new String[] { 
            "Running", "Cycling",  "Skateboarding"
        };

        log.info("Activated ActivitiesImpl with activities [ {} ]", String.join(", ", this.activities));
    }

    @Deactivate
    protected void deactivate() {
        log.info("ActivitiesImpl has been deactivated!");
    }
}
```
