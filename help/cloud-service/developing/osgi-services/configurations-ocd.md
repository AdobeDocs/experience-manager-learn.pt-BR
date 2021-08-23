---
title: Configurações OSGi como objetos ObjectClassDefinition
description: Saiba como criar e designar ObjectClassDefinition como representações de configuração OSGi em um serviço OSGi
role: Developer
level: Beginner
topic: Desenvolvimento
feature: OSGI
kt: 8269
thumbnail: 335730.jpeg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 3%

---


# Configurações OSGi como objetos ObjectClassDefinition

Saiba como criar e designar ObjectClassDefinition como representações de configuração OSGi em um serviço OSGi.

>[!VIDEO](https://video.tv.adobe.com/v/335730/?quality=12&learn=on)

## Recursos

+ [@Designate JavaDocs](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/metatype/annotations/Designate.html)
+ [@ObjectClassDefinition JavaDocs](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/metatype/annotations/ObjectClassDefinition.html)
+ [@AttributeDefinition JavaDocs](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/metatype/annotations/AttributeDefinition.html)
+ [@Ativate JavaDocs](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/component/annotations/Activate.html)

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
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(
    service = { Activities.class }
)
@Designate(ocd = ActivitiesImpl.Config.class)
public class ActivitiesImpl implements Activities {
    private static final Logger log = LoggerFactory.getLogger(ActivitiesImpl.class);

    @ObjectClassDefinition(
        name = "WKND Examples - Activities Service",
        description = "OSGi Service providing information about WKND Adventures activities"
    )
    @interface Config {

        @AttributeDefinition(
            name = "WKND Activities",
            description = "Human-friendly list of activity names"
        )
        String[] activities() default { "Hiking", "Jogging", "Walking" };

        @AttributeDefinition(
            name = "Random Activity Seed",
            description = "Seed used to randomize activity selection"
        )
        int random_seed() default 10;

    }

    private String[] activities;

    private Random random;

    /**
     * @return the name of a random WKND adventure activity
     */
    public String getRandomActivity() {
        int randomIndex = random.nextInt(activities.length);
        return activities[randomIndex];
    }    

    @Activate
    protected void activate(Config config) {

        this.activities = config.activities();

        final int randomSeed = config.random_seed(); 
          
        this.random = new Random(randomSeed);

        log.info("Activated ActivitiesImpl with activities [ {} ]", String.join(", ", this.activities));
    }

    @Deactivate
    protected void deactivate() {
        log.info("ActivitiesImpl has been deactivated!");
    }
}
```

### com.adobe.aem.wknd.examples.core.adventures.impl.ActivitiesImpl.cfg.json

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/com.adobe.aem.wknd.examples.core.adventures.impl.ActivitiesImpl.cfg.json`

```javascript
{
    "random.seed:Integer":12,
    "activities":[
      "Skateboarding",
      "Mountain Biking"
    ]
}
```
