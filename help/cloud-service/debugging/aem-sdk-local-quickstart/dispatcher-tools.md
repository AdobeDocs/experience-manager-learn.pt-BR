---
title: Depuração das ferramentas do Dispatcher
description: As Ferramentas do Dispatcher fornecem um ambiente do Apache Web Server contêiner que pode ser usado para simular AEM como um Dispatcher do Dispatcher do Cloud Services AEM Publish localmente. A depuração dos logs e do conteúdo de cache das Ferramentas do Dispatcher pode ser essencial para garantir que o aplicativo de AEM completo e as configurações de cache e segurança de suporte estejam corretas.
feature: Dispatcher
kt: 5918
topic: Development
role: Developer
level: Beginner, Intermediate
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---


# Depuração das ferramentas do Dispatcher

As Ferramentas do Dispatcher fornecem um ambiente do Apache Web Server contêiner que pode ser usado para simular AEM como um Dispatcher do Dispatcher do Cloud Services AEM Publish localmente.

A depuração dos logs e do conteúdo de cache das Ferramentas do Dispatcher pode ser essencial para garantir que o aplicativo de AEM completo e as configurações de cache e segurança de suporte estejam corretas.

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
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

