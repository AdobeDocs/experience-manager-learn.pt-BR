---
title: Ferramentas de depuração do Dispatcher
description: As Ferramentas do Dispatcher fornecem um ambiente contido do Apache Web Server que pode ser usado para simular o AEM Cloud Service AEM como um Dispatcher do serviço de publicação do localmente. A depuração dos registros das ferramentas do Dispatcher e do conteúdo do cache pode ser essencial para garantir que o aplicativo AEM completo e as configurações de cache e segurança estejam corretas.
feature: Dispatcher
jira: KT-5918
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: f0adf7a6-c7c2-449a-9fa5-402c54b812e5
duration: 87
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Ferramentas de depuração do Dispatcher

As Ferramentas do Dispatcher fornecem um ambiente contido do Apache Web Server que pode ser usado para simular o AEM Cloud Service AEM como um Dispatcher do serviço de publicação do localmente.

A depuração dos registros das ferramentas do Dispatcher e do conteúdo do cache pode ser essencial para garantir que o aplicativo AEM completo e as configurações de cache e segurança estejam corretas.

>[!NOTE]
>
>Como as Ferramentas do Dispatcher são baseadas em contêiner, cada vez que ele é reiniciado, os logs anteriores e o conteúdo do cache são destruídos.

## Logs de ferramentas do Dispatcher

Os logs das Ferramentas do Dispatcher estão disponíveis por meio da `stdout` ou o `bin/docker_run` ou com mais detalhes, disponíveis no contêiner Docker em `/etc/https/logs`.

Consulte [Logs do Dispatcher](./logs.md#dispatcher-logs) para obter instruções sobre como acessar diretamente os logs do contêiner Docker das Ferramentas do Dispatcher.

## Cache de ferramentas do Dispatcher

### Acesso a logs no container do Docker

O cache do Dispatcher pode ser acessado diretamente no container do Docker em ` /mnt/var/www/html`.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /mnt/var/www/html

# When finished viewing the cache, exit the Docker container's shell
/# exit
```

### Copiar os logs do Docker para o sistema de arquivos local

Os logs do Dispatcher podem ser copiados do contêiner do Docker em `/mnt/var/www/html` ao sistema de arquivos local para inspeção usando suas ferramentas favoritas. Observe que essa é uma cópia point-in-time e não fornece atualizações em tempo real para o cache.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```
