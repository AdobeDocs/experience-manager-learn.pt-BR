---
title: Depuração do SDK do AEM usando logs
description: Os registros atuam como linha de frente para depurar aplicativos de AEM, mas dependem do logon adequado no aplicativo AEM implantado.
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

# Depuração do SDK do AEM usando logs

Ao acessar os registros do SDK do AEM, o SDK local do AEM do quickstart Jar ou as Ferramentas do Dispatcher podem fornecer informações importantes sobre como depurar aplicativos do AEM.

## Logs do AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

Os registros atuam como linha de frente para depurar aplicativos de AEM, mas dependem do logon adequado no aplicativo AEM implantado. A Adobe recomenda manter as configurações de registro de desenvolvimento local e desenvolvimento as a Cloud Service AEM AEM as a Cloud Service AEM o mais semelhantes possível, pois normaliza a visibilidade do registro nos ambientes de desenvolvimento do SDK local e do, reduzindo as oscilações e a reimplantação da configuração.

A variável [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype) configura o registro no nível DEBUG para os pacotes Java do aplicativo AEM para desenvolvimento local por meio da configuração OSGi do Sling Logger encontrada em

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

que registra na `error.log`.

Se o registro padrão for insuficiente para o desenvolvimento local, o registro ad hoc poderá ser configurado por meio do console da Web Log Support do SDK AEM, no ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)), no entanto, não é recomendável que as alterações ad hoc sejam persistentes no Git, a menos que essas mesmas configurações de log também sejam necessárias em ambientes de Desenvolvimento as a Cloud Service AEM. Lembre-se, as alterações feitas por meio do console Log Support persistem diretamente no repositório do SDK do AEM local de início rápido.

As instruções de log Java podem ser visualizadas na variável `error.log` arquivo:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Geralmente, é útil &quot;rastrear&quot; o `error.log` que transmite sua saída para o terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ O Windows requer [Aplicativos tail de terceiros](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) ou a utilização de [Comando Get-Content do Powershell](https://stackoverflow.com/a/46444596/133936).

## Logs do Dispatcher

Os logs do Dispatcher são enviados para o stdout quando `bin/docker_run` é chamado, no entanto, os logs podem ser acessados diretamente com no contêiner Docker.

### Acesso a logs no container do Docker{#dispatcher-tools-access-logs}

Os logs do Dispatcher podem ser acessados diretamente no contêiner do Docker em `/etc/httpd/logs`.

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

_A variável `<CONTAINER ID>` in `docker exec -it <CONTAINER ID> /bin/sh` deve ser substituída pela ID de CONTÊINER do Docker de destino listada na `docker ps` comando._


### Copiar os logs do Docker para o sistema de arquivos local{#dispatcher-tools-copy-logs}

Os logs do Dispatcher podem ser copiados do contêiner do Docker em `/etc/httpd/logs` ao sistema de arquivos local para inspeção usando sua ferramenta favorita de análise de log. Observe que essa é uma cópia point-in-time e não fornece atualizações em tempo real aos logs.

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

_A variável `<CONTAINER_ID>` in `docker cp <CONTAINER_ID>:/var/log/apache2 ./` deve ser substituída pela ID de CONTÊINER do Docker de destino listada na `docker ps` comando._
