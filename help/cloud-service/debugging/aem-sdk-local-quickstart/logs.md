---
title: Depuração do AEM SDK usando logs
description: Os registros atuam como linha de frente para depurar aplicativos do AEM, mas dependem do logon adequado no aplicativo AEM implantado.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
duration: 411
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# Depuração do AEM SDK usando logs

Ao acessar os registros do AEM SDK, o Jar de início rápido local do AEM SDK ou as Ferramentas do Dispatcher podem fornecer informações importantes sobre a depuração de aplicativos do AEM.

## Logs do AEM

>[!VIDEO](https://video.tv.adobe.com/v/38153?quality=12&learn=on&captions=por_br)

Os registros atuam como linha de frente para depurar aplicativos do AEM, mas dependem do logon adequado no aplicativo AEM implantado. A Adobe recomenda manter as configurações de desenvolvimento local e de registro em log do AEM as a Cloud Service Dev o mais semelhantes possível, pois normaliza a visibilidade do registro no ambiente de inicialização rápida local do AEM SDK e nos ambientes de desenvolvimento do AEM as a Cloud Service, reduzindo a confusão e a reimplantação da configuração.

O [Arquétipo de projeto do AEM](https://github.com/adobe/aem-project-archetype) configura o registro em log no nível DEBUG para os pacotes Java do aplicativo AEM para desenvolvimento local através da configuração OSGi do Sling Logger, encontrada em

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

que faz logon no `error.log`.

Se o registro padrão for insuficiente para o desenvolvimento local, o registro ad hoc poderá ser configurado por meio do console da Web Suporte a logs do AEM SDK, em ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)). No entanto, não é recomendado que as alterações ad hoc sejam mantidas no Git, a menos que essas mesmas configurações de log também sejam necessárias em ambientes de desenvolvimento do AEM as a Cloud Service. Lembre-se, as alterações feitas por meio do console de Suporte ao registro são mantidas diretamente no repositório do SDK AEM.

As instruções de log Java podem ser exibidas no arquivo `error.log`:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Geralmente é útil para &quot;tail&quot; o `error.log` que transmite sua saída para o terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ O Windows requer [aplicativos tail de terceiros](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) ou o uso do [comando Get-Content do Powershell](https://stackoverflow.com/a/46444596/133936).

## Logs do Dispatcher

Os logs do Dispatcher são enviados para stdout quando `bin/docker_run` é chamado. No entanto, os logs podem ser acessados diretamente com no Docker contém.

### Acesso a logs no container do Docker{#dispatcher-tools-access-logs}

Os logs do Dispatcher podem estar acessando diretamente no contêiner Docker em `/etc/httpd/logs`.

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

_O `<CONTAINER ID>` em `docker exec -it <CONTAINER ID> /bin/sh` deve ser substituído pela ID do CONTÊINER do Docker de destino listada no comando `docker ps`._


### Copiar os logs do Docker para o sistema de arquivos local{#dispatcher-tools-copy-logs}

Os logs do Dispatcher podem ser copiados do contêiner do Docker em `/etc/httpd/logs` para o sistema de arquivos local para inspeção usando sua ferramenta de análise de log favorita. Observe que essa é uma cópia point-in-time e não fornece atualizações em tempo real aos logs.

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

_O `<CONTAINER_ID>` em `docker cp <CONTAINER_ID>:/var/log/apache2 ./` deve ser substituído pela ID do CONTÊINER do Docker de destino listada no comando `docker ps`._
