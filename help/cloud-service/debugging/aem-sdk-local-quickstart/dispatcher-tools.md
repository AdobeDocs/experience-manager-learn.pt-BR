---
title: Depuração das ferramentas do Dispatcher
description: As Ferramentas do Dispatcher fornecem um ambiente do Apache Web Server contêiner que pode ser usado para simular localmente o Dispatcher do AEM as a Cloud Services Publish do AEM Services. A depuração dos logs e do conteúdo de cache das Ferramentas do Dispatcher pode ser essencial para garantir que o aplicativo AEM completo e as configurações de cache e segurança de suporte estejam corretas.
feature: Dispatcher
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
topic: Development
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 1%

---


# Depuração das ferramentas do Dispatcher

As Ferramentas do Dispatcher fornecem um ambiente do Apache Web Server contêiner que pode ser usado para simular localmente o Dispatcher do AEM as a Cloud Services Publish do AEM Services.
A depuração dos logs e do conteúdo de cache das Ferramentas do Dispatcher pode ser essencial para garantir que o aplicativo AEM completo e as configurações de cache e segurança de suporte estejam corretas.

>[!NOTE]
>
>Como as Ferramentas do Dispatcher são baseadas em contêiner, sempre que ele é reiniciado os registros anteriores e o conteúdo do cache são destruídos.

## Logs de ferramentas do Dispatcher

Os logs de Ferramentas do Dispatcher estão disponíveis por meio do comando `stdout` ou `bin/docker_run`, ou com mais detalhes, disponíveis no container Docker em `/etc/https/logs`.

Consulte [Logs do Dispatcher](./logs.md#dispatcher-logs) para obter instruções sobre como acessar diretamente os logs do contêiner Docker das Ferramentas do Dispatcher.

## Cache de Ferramentas do Dispatcher

### Acessar logs no container Docker

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

### Copiando os logs do Docker para o sistema de arquivos local

Os registros do Dispatcher podem ser copiados do contêiner Docker em `/mnt/var/www/html` para o sistema de arquivos local para inspeção usando suas ferramentas favoritas. Observe que esta é uma cópia point-in-time e não fornece atualizações em tempo real para o cache.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

