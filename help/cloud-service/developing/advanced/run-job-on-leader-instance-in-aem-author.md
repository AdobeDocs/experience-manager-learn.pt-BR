---
title: Como executar um trabalho na instância líder no AEM as a Cloud Service
description: Saiba como executar um trabalho na instância líder no AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
topic: Development
feature: OSGI, Cloud Manager
role: Architect, Developer
level: Intermediate, Experienced
doc-type: Article
duration: 0
last-substantial-update: 2024-10-23T00:00:00Z
jira: KT-16399
thumbnail: KT-16399.jpeg
exl-id: b8b88fc1-1de1-4b5e-8c65-d94fcfffc5a5
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 0%

---

# Como executar um trabalho na instância líder no AEM as a Cloud Service

Saiba como executar um trabalho na instância líder no serviço de Autor do AEM como parte do AEM as a Cloud Service e entenda como configurá-lo para ser executado apenas uma vez.

Os trabalhos do Sling são tarefas assíncronas que operam em segundo plano, projetadas para lidar com eventos acionados pelo usuário ou pelo sistema. Por padrão, essas tarefas são distribuídas igualmente por todas as instâncias (pods) no cluster.

Para obter mais informações, consulte [Evento do Apache Sling e manuseio de trabalhos](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html).

## Criar e processar trabalhos

Para fins de demonstração, vamos criar um trabalho _simples que instrui o processador do trabalho a registrar uma mensagem_.

### Criar um trabalho

Use o código abaixo para _criar_ um trabalho do Apache Sling:

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import java.util.HashMap;
import java.util.Map;

import org.apache.sling.event.jobs.JobManager;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(immediate = true)
public class SimpleJobCreaterImpl {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobCreaterImpl.class);

    // Define the topic on which the job will be created
    protected static final String TOPIC = "wknd/simple/job/topic";

    // Inject a JobManager
    @Reference
    private JobManager jobManager;

    @Activate
    protected final void activate() throws Exception {
        log.info("SimpleJobCreater activated successfully");
        createJob();
        log.info("SimpleJobCreater created a job");
    }

    private void createJob() {
        // Create a job and add it on the above defined topic
        Map<String, Object> jobProperties = new HashMap<>();
        jobProperties.put("action", "log");
        jobProperties.put("message", "Job metadata is: Created in activate method");
        jobManager.addJob(TOPIC, jobProperties);
    }
}
```

Os pontos principais a serem observados no código acima são:

- A carga do trabalho tem duas propriedades: `action` e `message`.
- Usando o método `addJob(...)` do [JobManager](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/apache/sling/event/jobs/JobManager.html), o trabalho é adicionado ao tópico `wknd/simple/job/topic`.

### Processar um trabalho

Use o código abaixo para _processar_ o trabalho do Apache Sling acima:

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import org.apache.sling.event.jobs.Job;
import org.apache.sling.event.jobs.consumer.JobConsumer;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = JobConsumer.class, property = {
        JobConsumer.PROPERTY_TOPICS + "=" + SimpleJobCreaterImpl.TOPIC
}, immediate = true)
public class SimpleJobConsumerImpl implements JobConsumer {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobConsumerImpl.class);

    @Override
    public JobResult process(Job job) {
        // Get the action and message properties
        String action = job.getProperty("action", String.class);
        String message = job.getProperty("message", String.class);

        // Log the message
        if ("log".equals(action)) {
            log.info("Processing WKND Job, and {}", message);
        }

        // Return a successful result
        return JobResult.OK;
    }

}
```

Os pontos principais a serem observados no código acima são:

- A classe `SimpleJobConsumerImpl` implementa a interface `JobConsumer`.
- É um serviço registrado para consumir trabalhos do tópico `wknd/simple/job/topic`.
- O método `process(...)` processa o trabalho registrando a propriedade `message` da carga do trabalho.

### Processamento de trabalho padrão

Quando você implanta o código acima em um ambiente do AEM as a Cloud Service e o executa no serviço AEM Author, que opera como um cluster com várias JVMs de Autor do AEM, o trabalho será executado uma vez em cada instância (pod) do Autor do AEM, o que significa que o número de trabalhos criados corresponderá ao número de pods. O número de pods sempre será maior que um (para ambientes não-RDE), mas flutuará com base no gerenciamento de recursos internos da AEM as a Cloud Service.

O trabalho é executado em cada instância do AEM Author (pod) porque o `wknd/simple/job/topic` está associado à fila principal do AEM, que distribui trabalhos em todas as instâncias disponíveis.

Isso geralmente é problemático se o trabalho for responsável por alterar o estado, como criar ou atualizar recursos ou serviços externos.

Se você quiser que o trabalho seja executado apenas uma vez no serviço AEM Author, adicione a [configuração da fila de trabalhos](#how-to-run-a-job-on-the-leader-instance) descrita abaixo.

Você pode verificá-lo revisando os logs do serviço de Autor do AEM no [Cloud Manager](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs#cloud-manager).

![Trabalho processado por todas as instâncias](./assets/run-job-once/job-processed-by-all-instances.png)


Você deve ver:

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-nxxcx] *INFO* [sling-oak-observation-15] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method

<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-r4zk7] *INFO* [sling-oak-observation-11] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```

Há duas entradas de log, uma para cada instância de Autor do AEM (`68775db964-nxxcx` e `68775db964-r4zk7`), indicando que cada instância (pod) processou o trabalho.

## Como executar uma tarefa na instância líder

Para executar um trabalho _apenas uma vez_ no serviço AEM Author, crie uma nova fila de trabalhos do Sling do tipo **Ordenado** e associe seu tópico de trabalho (`wknd/simple/job/topic`) a essa fila. Com essa configuração, somente a instância líder do autor do AEM (pod) poderá processar a tarefa.

No módulo `ui.config` do projeto do AEM, crie um arquivo de configuração OSGi (`org.apache.sling.event.jobs.QueueConfiguration~wknd.cfg.json`) e armazene-o na pasta `ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author`.

```json
{
    "queue.name":"WKND Queue - ORDERED",
    "queue.topics":[
      "wknd/simple/job/topic"
    ],
    "queue.type":"ORDERED",
    "queue.retries":1,
    "queue.maxparallel":1.0
  }
```

Os principais pontos a serem observados na configuração acima são:

- O tópico da fila está definido como `wknd/simple/job/topic`.
- O tipo de fila está definido como `ORDERED`.
- O número máximo de trabalhos paralelos está definido como `1`.

Depois de implantar a configuração acima, a tarefa será processada exclusivamente pela instância líder, garantindo que seja executada apenas uma vez em todo o serviço do AEM Author.

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-7475cf85df-qdbq5] *INFO* [FelixLogListener] Events.Service.org.apache.sling.event Service [QueueMBean for queue WKND Queue - ORDERED,7755, [org.apache.sling.event.jobs.jmx.StatisticsMBean]] ServiceEvent REGISTERED
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
<DD.MM.YYYY HH:mm:ss.SSS> [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```
