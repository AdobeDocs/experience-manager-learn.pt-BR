---
title: Redirecionamentos de URL
description: Saiba mais sobre as várias opções para executar o redirecionamento de URL no AEM.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
kt: 11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
source-git-commit: 00ea3a8e6b69cd99cf293093d38b59df51f6a26d
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 2%

---


# Redirecionamentos de URL

O redirecionamento de URL é um aspecto comum como parte da operação do site. Arquitetos e administradores são desafiados a encontrar a melhor solução sobre como e onde gerenciar os redirecionamentos de URL que fornecem flexibilidade e tempo rápido de implantação de redirecionamento.

Familiarize-se com o [AEM (6.x) também conhecido como AEM Classic](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) e [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) infraestrutura. As principais diferenças são:

1. AEM as a Cloud Service [CDN integrado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html), no entanto, os clientes podem fornecer um CDN (BYOCDN) na frente da CDN gerenciada AEM.
1. AEM 6.x se o Adobe Managed Services (AMS) no local não inclui uma CDN gerenciada por AEM e os clientes devem trazer seus próprios.

Os outros serviços de AEM (Autor/Publicação do AEM e Dispatcher) são conceitualmente semelhantes entre AEM 6.x e AEM as a Cloud Service.

AEM soluções de redirecionamento de URL são as seguintes:

|  | Gerenciado e implantado como AEM código de projeto | Capacidade de alteração por equipe de marketing/conteúdo | AEM como compatível com Cloud Service | Onde a execução de redirecionamento acontece |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [No Edge, traga seu próprio CDN](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN |
| [Apache `mod_rewrite` regras como configuração do Dispatcher ](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons - Gerenciador de mapa de redirecionamento](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS Commons - Gerenciador de redirecionamento](#redirect-manager) | ✘ | ✔ | ✔ | AEM |
| [O `Redirect` propriedade da página](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## Opções de solução

Veja a seguir as opções de solução na ordem em que estão mais próximas do navegador do visitante do site.

### No Edge, traga seu próprio CDN

Alguns serviços de CDN fornecem soluções de redirecionamento no nível do Edge, reduzindo assim as viagens de ida e volta à origem. Consulte [Redirecionador do Akamai Edge](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [Funções do AWS CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Consulte seu provedor de serviços CDN para obter o recurso de redirecionamento no nível da borda.

O gerenciamento de redirecionamentos no nível de Edge ou CDN apresenta vantagens de desempenho, no entanto, eles não são gerenciados como parte de AEM, mas como projetos discretos. Um processo bem pensado para gerenciar e implantar regras de redirecionamento é crucial para evitar problemas.


### Apache `mod_rewrite` módulo

Uma solução comum usa [Apache Module mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). O [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype) fornece uma estrutura de projeto do Dispatcher para ambos [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) e [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) projeto. As regras padrão (imutáveis) e de reescrita personalizadas são definidas na variável `conf.d/rewrites` e o mecanismo de regravação é ativado para `virtualhosts` que escuta na porta `80` via `conf.d/dispatcher_vhost.conf` arquivo. Um exemplo de implementação está disponível na variável [AEM Projeto de Sites WKND](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

No AEM as a Cloud Service, essas regras de redirecionamento são gerenciadas como parte AEM código e implantadas por meio do Cloud Manager [pipeline de configuração de camada da Web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) ou [pipeline de pilha completa](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline). Assim, o processo específico do AEM projeto está em andamento para gerenciar, implantar e rastrear as regras de redirecionamento.

A maioria dos serviços CDN armazena em cache os redirecionamentos HTTP 301 e 302, dependendo de seus `Cache-Control` ou `Expires` cabeçalhos. Isso ajuda a evitar o percurso de ida e volta após o redirecionamento inicial originado no Apache/Dispatcher.


### ACS AEM Commons

Há dois recursos disponíveis no [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) para gerenciar redirecionamentos de URL. Observe que ACS AEM Commons é um projeto aberto operado pela comunidade e não suportado pelo Adobe.

#### Gerenciador de mapa de redirecionamento

[Gerenciador de mapa de redirecionamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) permite que os administradores do AEM 6.x mantenham e publiquem facilmente [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) arquivos sem acessar diretamente o servidor Web Apache ou exigir uma reinicialização do servidor Web Apache. Esse recurso permite que os usuários das permissões criem, atualizem e excluam regras de redirecionamento de um console no AEM, sem a ajuda da equipe de desenvolvimento ou de uma implantação AEM. O Gerenciador do mapa de redirecionamento é **NÃO AEM compatível as a Cloud Service**.

#### Gerenciador de redirecionamento

[Gerenciador de redirecionamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) O permite que os usuários do AEM mantenham e publiquem facilmente redirecionamentos do AEM. A implementação é baseada no filtro de servlet Java™, portanto, o consumo típico de recursos da JVM. Esse recurso também elimina a dependência da equipe de desenvolvimento AEM e das implantações de AEM. O Gerenciador de redirecionamento é **AEM as a Cloud Service** e **AEM 6.x** compatível. Embora a solicitação redirecionada inicial tenha de apertar o serviço de Publicação do AEM para gerar o cache 301/302 (mais) das CDNs 301/302 por padrão, permitindo que as solicitações subsequentes sejam redirecionadas na borda/CDN.

### O `Redirect` propriedade da página

O pronto para uso (OOTB) `Redirect` propriedade de página do [Guia Avançado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/fundamentals/page-properties.html#advanced) permite que os autores de conteúdo definam o local de redirecionamento da página atual. Essa solução é mais adequada para cenários de redirecionamento por página e não tem um local central para visualizar e gerenciar os redirecionamentos da página.

## Qual solução é correta para a implementação

Abaixo estão alguns critérios para determinar a solução correta. Além disso, o processo de TI e marketing de sua organização deve ajudar a escolher a solução correta.

1. Permitindo que a equipe ou os superusuários de marketing gerenciem regras de redirecionamento sem a equipe de desenvolvimento de AEM e as implantações de AEM.
1. O Processo para gerenciar, verificar, rastrear e reverter as alterações ou a mitigação do risco.
1. Disponibilidade de _Experiência em matéria de assunto_ para **No Edge via Serviço CDN** solução.

