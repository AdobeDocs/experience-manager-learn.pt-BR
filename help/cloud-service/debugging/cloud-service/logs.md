---
title: Logs
description: Os registros atuam como linha de frente para depurar aplicativos do AEM no AEM as a Cloud Service, mas dependem do logon adequado no aplicativo AEM implantado.
feature: Ferramentas do desenvolvedor
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: Desenvolvimento
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '995'
ht-degree: 3%

---


# Depuração do AEM as a Cloud Service usando logs

Os registros atuam como linha de frente para depurar aplicativos do AEM no AEM as a Cloud Service, mas dependem do logon adequado no aplicativo AEM implantado.

Todas as atividades de log para um determinado serviço AEM do ambiente (Autor, Publicação/Publicação do Dispatcher) são consolidadas em um único arquivo de log, mesmo se diferentes pods nesse serviço gerarem as declarações de log.

As IDs de pod são fornecidas em cada instrução de log e permitem a filtragem ou a coleta de declarações de log. As IDs de pod estão no formato de:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Exemplo: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Arquivos de log personalizados

O AEM as a Cloud Services não oferece suporte a arquivos de log personalizados, no entanto, ele oferece suporte ao logon personalizado.

Para que os logs Java estejam disponíveis no AEM as a Cloud Service (via [Cloud Manager](#cloud-manager) ou [Adobe I/O CLI](#aio)), as instruções de log personalizadas devem ser gravadas no `error.log`. Os registros gravados em logs nomeados personalizados, como `example.log`, não serão acessíveis pelo AEM as a Cloud Service.

## Logs do serviço de Autor e Publicação do AEM

Os serviços de Autor e Publicação do AEM fornecem registros do servidor de tempo de execução do AEM:

+ `aemerror` é o log de erros do Java (encontrado  `/crx-quickstart/error.log` no AEM SDK local quickstart). Veja a seguir os [níveis de log recomendados](#log-levels) para registradores personalizados por tipo de ambiente:
   + Desenvolvimento: `DEBUG`
   + Estágio: `WARN`
   + Produção: `ERROR`
+ `aemaccess` lista solicitações HTTP para o serviço AEM com detalhes
+ `aemrequest` lista solicitações HTTP feitas ao serviço AEM e sua resposta HTTP correspondente

## Publicação do AEM Logs do Dispatcher

Somente o AEM Publish Dispatcher fornece registros de servidor da Web Apache e Dispatcher, pois esses aspectos só existem no nível de Publicação AEM e não no nível de Autor do AEM.

+ `httpdaccess` lista solicitações HTTP feitas no servidor da Web Apache/Dispatcher do serviço AEM.
+ `httperror`  lista mensagens de log do servidor da Web Apache e ajuda na depuração de módulos Apache suportados, como  `mod_rewrite`.
   + Desenvolvimento: `DEBUG`
   + Estágio: `WARN`
   + Produção: `ERROR`
+ `aemdispatcher` lista mensagens de log dos módulos do Dispatcher, incluindo filtragem e veiculação de mensagens de cache.
   + Desenvolvimento: `DEBUG`
   + Estágio: `WARN`
   + Produção: `ERROR`

## Cloud Manager{#cloud-manager}

O Adobe Cloud Manager permite o download de logs, por dia, por meio de uma ação de Baixar logs do ambiente.

![Cloud Manager - Logs de download](./assets/logs/download-logs.png)

Esses logs podem ser baixados e inspecionados por meio de qualquer ferramenta de análise de log.

## CLI do Adobe I/O com o plug-in do Cloud Manager{#aio}

O Adobe Cloud Manager oferece suporte ao acesso a logs do AEM as a Cloud Service por meio da [Adobe I/O CLI](https://github.com/adobe/aio-cli) com o [plug-in do Cloud Manager para a Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager).

Primeiro, [configure o Adobe I/O com o plug-in do Cloud Manager](../../local-development-environment/development-tools.md#aio-cli).

Verifique se a ID de programa e a ID de ambiente relevantes foram identificadas e use [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) para listar as opções de log usadas para [tail](#aio-cli-tail-logs) ou [download](#aio-cli-download-logs) logs.

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

A CLI do Adobe I/O fornece a capacidade de rastrear logs em tempo real do AEM as a Cloud Service usando o comando [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name). A divisão é útil para assistir à atividade de log em tempo real, pois as ações são executadas no ambiente do AEM as a Cloud Service.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Outras ferramentas de linha de comando, como `grep`, podem ser usadas em conjunto com `tail-logs` para ajudar a isolar declarações de interesse de log, por exemplo:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... exibe somente instruções de log geradas a partir de `com.example.MySlingModel` ou contêm essa string neles.

### Fazendo download de logs{#aio-cli-download-logs}

A CLI do Adobe I/O fornece a capacidade de baixar logs do AEM as a Cloud Service usando o comando [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)). Isso fornece o mesmo resultado final que o download dos logs da interface do usuário da Web do Cloud Manager, com a diferença sendo que o comando `download-logs` consolida os logs ao longo dos dias, com base no número de dias de logs solicitados.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Como entender os logs

Os logs no AEM as a Cloud Service têm vários pods gravando instruções de log neles. Como várias instâncias do AEM são gravadas no mesmo arquivo de log, é importante entender como analisar e reduzir o ruído durante a depuração. Para explicar, o seguinte trecho de log `aemerror` será usado:

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

Usando as IDs de pod, o ponto de dados após a data e a hora, os logs podem ser coletados por Pod ou por uma instância do AEM dentro do serviço, facilitando o rastreamento e o entendimento da execução do código.

__Pod cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-author-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## Níveis de log recomendados{#log-levels}

As orientações gerais da Adobe sobre níveis de log por ambiente do AEM as a Cloud Service são:

+ Desenvolvimento local (SDK do AEM): `DEBUG`
+ Desenvolvimento: `DEBUG`
+ Estágio: `WARN`
+ Produção: `ERROR`

Definir o nível de log mais apropriado para cada tipo de ambiente é com o AEM as a Cloud Service, os níveis de log são mantidos no código

+ As configurações de log do Java são mantidas nas configurações do OSGi
+ Níveis de log do Apache Web Server e Dispatcher no projeto do dispatcher

...e, portanto, requer uma implantação para alteração.

### Variáveis específicas do ambiente para definir níveis de log do Java

Uma alternativa para definir níveis de log Java estáticos e bem conhecidos para cada ambiente é usar as [variáveis específicas do ambiente do AEM as Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) para parametrizar os níveis de log, permitindo que os valores sejam alterados dinamicamente por meio da [CLI do Adobe I/O com o plug-in do Cloud Manager](#aio-cli).

Isso requer a atualização das configurações de log do OSGi para usar os espaços reservados da variável específica do ambiente. [Os ](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) valores padrão para níveis de log devem ser definidos de acordo com as recomendações [ da ](#log-levels)Adobe. Por exemplo:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
}
```

Esta abordagem tem desvantagens que devem ser tidas em conta:

+ [Um número limitado de variáveis de ambiente é permitido](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables), e a criação de uma variável para gerenciar o nível de log usará uma.
+ As variáveis de ambiente só podem ser gerenciadas programaticamente via [Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) ou [APIs HTTP do Cloud Manager](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties).
+ As alterações nas variáveis de ambiente devem ser redefinidas manualmente por uma ferramenta compatível. Esquecer de redefinir um ambiente de alto tráfego, como Produção, para um nível de log menos detalhado pode inundar os logs e afetar o desempenho do AEM.

_As variáveis específicas do ambiente não funcionam para as configurações de log do Apache Web Server ou Dispatcher, pois não são configuradas por meio da configuração OSGi._