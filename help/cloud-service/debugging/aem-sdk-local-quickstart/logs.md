---
title: Depuração AEM SDK usando logs
description: Os registros atuam como linha de frente para depurar aplicativos de AEM, mas dependem do logon adequado no aplicativo de AEM implantado.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---

# Depuração AEM SDK usando logs

Ao acessar os registros AEM do SDK, as ferramentas locais do AEM SDK quickstart Jar ou do Dispatcher podem fornecer insights importantes sobre a depuração AEM aplicativos.

## Logs AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

Os registros atuam como linha de frente para depurar aplicativos de AEM, mas dependem do logon adequado no aplicativo de AEM implantado. O Adobe recomenda manter o desenvolvimento local e AEM configurações de registro de desenvolvimento as a Cloud Service como possível, pois normaliza a visibilidade do log nos ambientes de início rápido local do SDK AEM e de desenvolvimento da as a Cloud Service, reduzindo a duplicação da configuração e a reimplantação.

O [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype) configura o registro em log no nível DEBUG dos pacotes Java do aplicativo AEM para desenvolvimento local por meio da configuração OSGi do Sling Logger localizada em

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

que faz logon no `error.log`.

Se o registro padrão for insuficiente para o desenvolvimento local, o registro ad hoc poderá ser configurado por meio do console da Web Suporte a log do início rápido local AEM SDK, em ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)), no entanto, não é recomendado que as alterações ad hoc sejam mantidas no Git, a menos que essas mesmas configurações de log sejam necessárias AEM ambientes de desenvolvimento as a Cloud Service também. Lembre-se, as alterações por meio do console Suporte de log são persistentes diretamente no repositório local do início rápido do SDK AEM.

As instruções de log do Java podem ser visualizadas na variável `error.log` arquivo:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Muitas vezes, é útil &quot;rastrear&quot; o `error.log` que envia a sua saída para o terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ O Windows requer [Aplicativos de cauda de terceiros](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) ou a utilização de [Comando Get-Content do Powershell](https://stackoverflow.com/a/46444596/133936).

## Logs do Dispatcher

Os logs do Dispatcher são gerados para stdout quando `bin/docker_run` é chamado, no entanto, os logs podem ser acessados diretamente com no Docker contém.

### Acessar logs no container Docker{#dispatcher-tools-access-logs}

Os logs do Dispatcher podem ser acessados diretamente no container do Docker em `/etc/httpd/logs`.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /etc/httpd/logs
/ # ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log

# When finished viewing the logs files, exit the Docker container's shell
/# exit
```

_O `<CONTAINER ID>` em `docker exec -it <CONTAINER ID> /bin/sh` deve ser substituído pela ID do CONTÊINER do Docker listada no `docker ps` comando._


### Copiando os logs do Docker para o sistema de arquivos local{#dispatcher-tools-copy-logs}

Os logs do Dispatcher podem ser copiados do contêiner do Docker em `/etc/httpd/logs` ao sistema de arquivos local para inspeção usando sua ferramenta de análise de log favorita. Observe que esta é uma cópia point-in-time e não fornece atualizações em tempo real para os logs.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/etc/httpd/logs logs 
$ cd logs
$ ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log
```

_O `<CONTAINER_ID>` em `docker cp <CONTAINER_ID>:/var/log/apache2 ./` deve ser substituído pela ID do CONTÊINER do Docker listada no `docker ps` comando._
