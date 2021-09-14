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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---

# Depuração AEM SDK usando logs

Ao acessar os registros AEM do SDK, as ferramentas locais do AEM SDK quickstart Jar ou do Dispatcher podem fornecer insights importantes sobre a depuração AEM aplicativos.

## Logs AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

Os registros atuam como linha de frente para depurar aplicativos de AEM, mas dependem do logon adequado no aplicativo de AEM implantado. O Adobe recomenda manter o desenvolvimento local e o AEM como configurações de registro de desenvolvimento como o possível, pois normaliza a visibilidade do log no início rápido local AEM SDK e AEM como um ambiente Cloud Service Dev Cloud Service, reduzindo o twidling de configuração e a reimplantação.

O [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) configura o registro em log no nível DEBUG dos pacotes Java do seu aplicativo AEM para desenvolvimento local através da configuração OSGi do Sling Logger localizada em

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

que registra no `error.log`.

Se o registro padrão for insuficiente para o desenvolvimento local, o registro ad hoc pode ser configurado por meio do console da Web Suporte a log do início rápido local AEM SDK, em ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)), no entanto, não é recomendado que as alterações ad hoc sejam mantidas no Git, a menos que essas mesmas configurações de log sejam necessárias em AEM como ambientes de desenvolvimento Cloud Service. Lembre-se, as alterações por meio do console Suporte de log são persistentes diretamente no repositório local do início rápido do SDK AEM.

As instruções de log do Java podem ser exibidas no arquivo `error.log`:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Muitas vezes, é útil &quot;rastrear&quot; o `error.log` que transmite sua saída para o terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ O Windows requer [aplicativos de tail de terceiros](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) ou o uso do [comando Get-Content do Powershell](https://stackoverflow.com/a/46444596/133936).

## Logs do Dispatcher

Os logs do Dispatcher são enviados para o stdout quando `bin/docker_run` é chamado, no entanto, os logs podem ser acessados diretamente com no conteúdo do Docker.

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

_O  `<CONTAINER ID>` no  `docker exec -it <CONTAINER ID> /bin/sh` deve ser substituído pela ID de CONTÊINER do Docker de destino listada no  `docker ps` comando._


### Copiando os logs do Docker para o sistema de arquivos local{#dispatcher-tools-copy-logs}

Os logs do Dispatcher podem ser copiados do contêiner Docker em `/etc/httpd/logs` para o sistema de arquivos local para inspeção usando sua ferramenta de análise de log favorita. Observe que esta é uma cópia point-in-time e não fornece atualizações em tempo real para os logs.

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

_O  `<CONTAINER_ID>` no  `docker cp <CONTAINER_ID>:/var/log/apache2 ./` deve ser substituído pela ID de CONTÊINER do Docker de destino listada no  `docker ps` comando._
