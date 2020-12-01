---
title: Depuração AEM SDK usando registros
description: Os registros atuam como linha de frente para depurar aplicativos AEM, mas dependem do logon adequado no aplicativo AEM implantado.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 2%

---


# Depuração AEM SDK usando registros

Acessar os registros do SDK AEM, as ferramentas de Início Rápido local do SDK AEM ou as ferramentas do Dispatcher podem fornecer insights importantes para a depuração AEM aplicativos.

## Logs AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

Os registros atuam como linha de frente para depurar aplicativos AEM, mas dependem do logon adequado no aplicativo AEM implantado. A Adobe recomenda manter o desenvolvimento local e a AEM como um Cloud Service Dev logging settings tão semelhantes quanto possível, pois normaliza a visibilidade do registro no início rápido local do SDK AEM e AEM como um Cloud Service Dev ambiente, reduzindo a ociosidade e a reimplantação da configuração.

O [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) configura o registro em log no nível DEBUG dos pacotes Java da sua aplicação AEM para desenvolvimento local através da configuração Sling Logger OSGi encontrada em

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

que faz logon em `error.log`.

Se o registro padrão for insuficiente para o desenvolvimento local, o registro ad hoc poderá ser configurado por meio do console da Web do suporte ao registro do SDK local, em ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)), no entanto, não é recomendado que as alterações ad hoc sejam mantidas no Git, a menos que essas mesmas configurações de registro sejam necessárias em AEM como ambientes de desenvolvedor Cloud Service. Lembre-se de que as alterações por meio do console Suporte de log são mantidas diretamente no repositório local do Início rápido do SDK AEM.

As declarações do log Java podem ser visualizações no arquivo `error.log`:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Muitas vezes, é útil &quot;rastrear&quot; o `error.log` que transmite sua saída para o terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ O Windows requer [aplicativos de rastreamento de terceiros](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) ou o uso do [comando Get-Content do Powershell](https://stackoverflow.com/a/46444596/133936).

## Logs do Dispatcher

Os logs do Dispatcher são enviados para stdout quando `bin/docker_run` é chamado, no entanto, os logs podem ser acessados diretamente com o no Docker contém.

### Acessar registros no container Docker

Os registros do Dispatcher podem estar acessando diretamente no container Docker em `/etc/httpd/logs`.

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

### Copiando os registros do Docker para o sistema de arquivos local

Os registros do Dispatcher podem ser copiados do container do Docker em `/etc/httpd/logs` para o sistema de arquivos local para inspeção usando sua ferramenta de análise de log favorita. Observe que esta é uma cópia point-in-time e não fornece atualizações em tempo real para os registros.

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

