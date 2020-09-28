---
title: Depuração das ferramentas do Dispatcher
description: As Ferramentas do Dispatcher fornecem um ambiente do Apache Web Server com contêiner que pode ser usado para simular AEM como um Dispatcher do serviço de Publicação AEM localmente. A depuração dos registros e do conteúdo do cache das Ferramentas do Dispatcher pode ser vital para garantir que o aplicativo AEM completo e as configurações de cache e segurança de suporte estejam corretas.
feature: dispatcher, aem-sdk
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---


# Depuração das ferramentas do Dispatcher

As Ferramentas do Dispatcher fornecem um ambiente do Apache Web Server com contêiner que pode ser usado para simular AEM como um Dispatcher do serviço de Publicação AEM localmente.
A depuração dos registros e do conteúdo do cache das Ferramentas do Dispatcher pode ser vital para garantir que o aplicativo AEM completo e as configurações de cache e segurança de suporte estejam corretas.

>[!NOTE]
>
>Como as Ferramentas do Dispatcher são baseadas em container, sempre que são reiniciadas, os registros anteriores e o conteúdo do cache são destruídos.

## Logs de ferramentas do Dispatcher

Os registros das Ferramentas do Dispatcher estão disponíveis por meio do comando `stdout` ou do `bin/docker_run` comando, ou com mais detalhes, no container Docker em `/etc/https/logs`.

Consulte [Logs](./logs.md#dispatcher-logs) do Dispatcher para obter instruções sobre como acessar diretamente os logs de container do Dispatcher Tools.

## Cache de ferramentas do Dispatcher

### Acessar registros no container Docker

O cache do Dispatcher pode estar acessando diretamente no container Docker em ` /mnt/var/www/html`.

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

### Copiando os registros do Docker para o sistema de arquivos local

Os registros do Dispatcher podem ser copiados do container Docker em `/mnt/var/www/html` para o sistema de arquivos local para inspeção usando suas ferramentas favoritas. Observe que esta é uma cópia point-in-time e não fornece atualizações em tempo real para o cache.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

