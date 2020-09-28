---
title: Logs
description: Os registros atuam como linha de frente para depurar aplicativos AEM em AEM como Cloud Service, mas dependem do logon adequado no aplicativo AEM implantado.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
translation-type: tm+mt
source-git-commit: 1eb15af3d9d2904856218aaad4d5c52233603a71
workflow-type: tm+mt
source-wordcount: '925'
ht-degree: 3%

---


# Depuração de AEM como Cloud Service usando registros

Os registros atuam como linha de frente para depurar aplicativos AEM em AEM como Cloud Service, mas dependem do logon adequado no aplicativo AEM implantado.

Toda a atividade de log para um determinado serviço de AEM de ambiente (Autor, Publicar/Publicar Dispatcher) é consolidada em um único arquivo de log, mesmo se diferentes pods nesse serviço gerarem as declarações de log.

As IDs de pod são fornecidas em cada instrução de log e permitem a filtragem ou a coleta de declarações de log. As IDs do pod estão no formato de:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Exemplo: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Logs do serviço de autor e publicação do AEM

Os serviços de autor e publicação do AEM fornecem registros AEM servidor de tempo de execução:

+ `aemerror` é o log de erros do Java (encontrado `/crx-quickstart/error.log` no Início rápido local do SDK do AEM). Estes são os níveis [de log](#log-levels) recomendados para os registradores personalizados por tipo de ambiente:
   + Desenvolvimento: `DEBUG`
   + Estágio: `WARN`
   + Produção: `ERROR`
+ `aemaccess` Solicitações HTTP do lista para o serviço AEM com detalhes
+ `aemrequest` Solicitações HTTP do lista feitas para AEM serviço e respectiva resposta HTTP

## Logs do Dispatcher de publicação do AEM

Somente o AEM Publish Dispatcher fornece o servidor da Web do Apache e os logs do Dispatcher, pois esses aspectos só existem na camada de publicação do AEM e não na camada de autor do AEM.

+ `httpdaccess` Solicitações HTTP do lista feitas no servidor da Web Apache/Dispatcher do serviço AEM.
+ `httperror`  Mensagens de registro do lista do servidor Web Apache e ajuda na depuração de módulos Apache suportados, como `mod_rewrite`.
   + Desenvolvimento: `DEBUG`
   + Estágio: `WARN`
   + Produção: `ERROR`
+ `aemdispatcher` Mensagens de registro do lista dos módulos do Dispatcher, incluindo filtragem e serviço de mensagens de cache.
   + Desenvolvimento: `DEBUG`
   + Estágio: `WARN`
   + Produção: `ERROR`

## Cloud Manager

O Adobe Cloud Manager permite o download de logs, por dia, por meio de uma ação de Logs de download de ambientes.

![Gerenciador de nuvem - Logs de download](./assets/logs/download-logs.png)

Esses registros podem ser baixados e inspecionados por meio de qualquer ferramenta de análise de log.

## CLI de E/S de Adobe com o plug-in do Cloud Manager

O Adobe Cloud Manager oferece suporte ao acesso a AEM como registros de Cloud Service por meio da CLI [de E/S do](https://github.com/adobe/aio-cli) Adobe com o plug-in [Cloud Manager para a CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager)de E/S do Adobe.

Primeiro, [configure a E/S do Adobe com o plug-in](../../local-development-environment/development-tools.md#aio-cli)do Cloud Manager.

Verifique se a ID do Programa e a ID do Ambiente relevantes foram identificadas e use as opções [de log disponíveis para](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) lista para lista das opções de log usadas para [rastrear](#aio-cli-tail-logs) ou [baixar](#aio-cli-download-logs) logs.

```
$ aio cloudmanager:list-programs
Program Id Name      Enabled 
14304      Program 1 true    
11454      Program 2 true 
11502      Program 3 true    

$ aio config:set cloudmanager_programid <PROGRAM ID>

$ aio cloudmanager:list-environments        
Environment Id Name            Type  Description 
22295          program-3-dev   dev               
22310          program-3-prod  prod              
22294          program-3-stage stage   

$ aio cloudmanager:list-available-log-options <ENVIRONMENT ID>
Environment Id Service    Name          
22295          author     aemaccess     
22295          author     aemerror      
22295          author     aemrequest    
22295          publish    aemaccess     
22295          publish    aemerror      
22295          publish    aemrequest    
22295          dispatcher httpdaccess   
22295          dispatcher httpderror    
22295          dispatcher aemdispatcher 
```

### Registros à direita{#aio-cli-tail-logs}

A CLI de E/S do Adobe fornece a capacidade de rastrear registros em tempo real de AEM como um Cloud Service usando o comando [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) . A navegação é útil para observar a atividade do registro em tempo real, pois as ações são executadas no AEM como um ambiente Cloud Service.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Outras ferramentas de linha de comando, como `grep` podem ser usadas em conjunto com `tail-logs` o para ajudar a isolar declarações de interesse de log, por exemplo:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... exibe somente instruções de log geradas a partir `com.example.MySlingModel` ou que contêm essa string.

### Download de logs{#aio-cli-download-logs}

A CLI de E/S do Adobe fornece a capacidade de baixar registros do AEM como Cloud Service usando o comando [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)). Isso fornece o mesmo resultado final que o download dos logs da interface do usuário da Web do Cloud Manager, com a diferença de ser o `download-logs` comando que consolida os logs ao longo dos dias, com base na quantidade de dias de registros solicitados.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Como entender os logs

Os registros AEM como Cloud Service têm vários pods que gravam declarações de log neles. Como várias instâncias AEM gravam no mesmo arquivo de log, é importante entender como analisar e reduzir o ruído durante a depuração. Para explicar, o seguinte trecho de `aemerror` log será usado:

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

Usando as IDs do pod, o ponto de dados após a data e a hora, os registros podem ser agrupados por pod ou por instância AEM dentro do serviço, facilitando o rastreamento e a compreensão da execução do código.

__Pod cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-author-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## Níveis de registro recomendados{#log-levels}

Adobe como níveis de log por ambiente:

+ Desenvolvimento local (SDK AEM): `DEBUG`
+ Desenvolvimento: `DEBUG`
+ Estágio: `WARN`
+ Produção: `ERROR`

Definir o nível de log mais apropriado para cada tipo de ambiente é com AEM como Cloud Service, os níveis de log são mantidos no código

+ As configurações de log Java são mantidas em configurações OSGi
+ Níveis de log do Apache Web Server e do Dispatcher no projeto do dispatcher

...e, portanto, exigir uma implantação para mudar.

### Variáveis específicas do ambiente para definir níveis de log do Java

Uma alternativa para configurar níveis estáticos de log Java conhecidos para cada ambiente é usar AEM como variáveis [específicas do Cloud Service para parametrizar os níveis de log, permitindo que os valores sejam alterados dinamicamente por meio da CLI de E/S do](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) Adobe com o plug-in [](#aio-cli)do Cloud Manager.

Isso requer a atualização das configurações de registro OSGi para usar os espaços reservados para variáveis específicas do ambiente. [Os valores](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) padrão para níveis de log devem ser definidos de acordo com as recomendações [de](#log-levels)Adobe. Por exemplo:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
}
```

Esta abordagem tem desvantagens que devem ser tidas em conta:

+ [Um número limitado de variáveis de ambiente é permitido](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)e a criação de uma variável para gerenciar o nível de log usará uma.
+ As variáveis de ambiente só podem ser gerenciadas de forma programática por meio da CLI [de E/S do](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) Adobe ou das APIs [HTTP do](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties)Cloud Manager.
+ As alterações nas variáveis do ambiente devem ser redefinidas manualmente por uma ferramenta compatível. Esquecer de redefinir um ambiente de tráfego alto, como Produção, para um nível de log menos detalhado pode inundar os registros e afetar o desempenho AEM.

_As variáveis específicas do ambiente não funcionam para o servidor Web Apache ou para as configurações de log do Dispatcher, pois não estão configuradas pela configuração OSGi._