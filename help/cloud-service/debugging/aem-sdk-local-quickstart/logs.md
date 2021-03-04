---
title: Depuração do SDK do AEM usando logs
description: Os registros atuam como linha de frente para depurar aplicativos do AEM, mas dependem do logon adequado no aplicativo AEM implantado.
feature: Ferramentas do desenvolvedor
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Desenvolvimento
role: Desenvolvedor
level: Iniciante, Intermediário
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 3%

---


# Depuração do SDK do AEM usando logs

Ao acessar os logs do SDK do AEM, as ferramentas locais de inicialização rápida do Jar ou do Dispatcher do SDK do AEM podem fornecer informações importantes sobre a depuração de aplicativos do AEM.

## Logs do AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

Os registros atuam como linha de frente para depurar aplicativos do AEM, mas dependem do logon adequado no aplicativo AEM implantado. A Adobe recomenda manter as configurações de log de desenvolvimento local e de desenvolvimento do AEM as a Cloud Service da mesma maneira possível, pois normaliza a visibilidade do log nos ambientes de desenvolvimento do AEM SDK de inicialização rápida local e do AEM as a Cloud Service, reduzindo a duplicação da configuração e a reimplantação.

O [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype) configura o registro em log no nível DEBUG dos pacotes Java do aplicativo AEM para desenvolvimento local por meio da configuração OSGi do Sling Logger localizada em

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

que registra no `error.log`.

Se o registro padrão for insuficiente para o desenvolvimento local, o registro ad hoc pode ser configurado por meio do console da Web Suporte a log do quickstart local do AEM SDK, em ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)), no entanto, não é recomendado que as alterações ad hoc sejam mantidas no Git, a menos que essas mesmas configurações de log sejam necessárias também em ambientes de desenvolvimento do AEM as a Cloud Service. Lembre-se, as alterações por meio do console Suporte de log são persistentes diretamente no repositório local do início rápido do AEM SDK.

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
