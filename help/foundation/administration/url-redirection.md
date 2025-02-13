---
title: Redirecionamentos de URL
description: Saiba mais sobre as várias opções para executar o redirecionamento de URL no AEM.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
jira: KT-11466
last-substantial-update: 2024-10-22T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
duration: 164
source-git-commit: bc2f4655631f28323a39ed5b4c7878613296a0ba
workflow-type: tm+mt
source-wordcount: '961'
ht-degree: 0%

---

# Redirecionamentos de URL

O redirecionamento de URL é um aspecto comum como parte da operação do site. Os arquitetos e administradores são desafiados a encontrar a melhor solução sobre como e onde gerenciar os redirecionamentos de URL que fornecem flexibilidade e tempo de implantação de redirecionamento rápido.

Familiarize-se com a infraestrutura do [AEM (6.x) também conhecida como AEM Classic](https://experienceleague.adobe.com/en/docs/experience-manager-learn/dispatcher-tutorial/chapter-2) e [AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture). As principais diferenças são:

1. O AEM as a Cloud Service tem [CDN interna](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn), no entanto, os clientes podem fornecer um CDN (BYOCDN) na frente do CDN gerenciado pela AEM.
1. AEM 6.x se a Managed Services local ou Adobe Systems (AMS) não inclui um CDN gerenciado AEM e os clientes devem trazer os seus próprios clientes.

Os outros serviços da AEM (AEM Autor/Publish e Dispatcher) são conceitualmente semelhantes entre AEM 6.x e AEM como Cloud Service.

As soluções redirecionar URL da AEM são as seguintes:

|                                                   | Gerenciado e implantado como AEM código do projeto | Capacidade de alteração por equipe/marketing/conteúdo | Compatível com AEM as Cloud Service | Onde ocorre a execução do redirecionamento |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [No Edge via CDN gerenciada pela AEM](#at-edge-via-aem-managed-cdn) | ✔ | ✘ | ✔ | Edge/CDN (incorporado) |
| [No Edge, traga sua própria CDN (BYOCDN)](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN (BYOCDN) |
| [Regras do Apache `mod_rewrite` como configuração do Dispatcher](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons - Gerenciador de Mapa de Redirecionamento](#redirect-map-manager) | ✘ | ✔ | ✔ | Dispatcher |
| [ACS Commons - Gerenciador de redirecionamento](#redirect-manager) | ✘ | ✔ | ✔ | AEM/Dispatcher |
| [A propriedade da página `Redirect`](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## Opções de solução

A seguir estão opções de solução na ordem em que estão mais perto do navegador do visitante do site.

### No Edge, por meio da CDN gerenciada pela AEM {#at-edge-via-aem-managed-cdn}

Essa opção só está disponível para AEM como um cliente Cloud Service.

A [CDN gerenciada AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn) oferece uma solução de redirecionamento no nível do Edge, reduzindo assim as viagens de ida e volta para o origem. O [recurso Redirecionamentos](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors) do lado do cliente permite configurar as regras de redirecionar no código de projeto AEM e implantar usando o [pipeline](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager) de configuração. O tamanho CDN arquivo de configuração (`cdn.yaml`) não deve exceder 100 KB.

O gerenciamento de redirecionamentos no nível da Edge ou CDN tem vantagens de desempenho.

### Na Edge, por meio do, traga seu próprio CDN

Alguns serviços de CDN fornecem soluções de redirecionamento no nível da Edge, reduzindo assim as viagens de ida e volta à origem. Consulte [Redirecionador Akamai Edge](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [Funções do AWS CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Consulte seu provedor de serviços CDN para obter recursos de redirecionamento de nível de Edge.

O gerenciamento de redirecionamentos no nível da Edge ou CDN tem vantagens de desempenho, no entanto, não são gerenciados como parte do AEM, mas como projetos discretos. Um processo bem definido para gerenciar e implantar regras de redirecionamento é fundamental para evitar problemas.


### Apache `mod_rewrite` módulo

Uma solução comum usa o [apache Module mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). O [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) fornece uma estrutura de projeto Dispatcher para as [versões AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) e [AEM como um projeto Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) . As regras de reescrita padrão (imutável) e personalizadas são definidas na `conf.d/rewrites` pasta e o mecanismo de reescrita é ativado para `virtualhosts` que o ouvinte seja ativado porta `80` via `conf.d/dispatcher_vhost.conf` arquivo. Um exemplo implementação está disponível na [AEM WKND Sites Project](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

No AEM como um Cloud Service, essas regras redirecionar são gerenciadas como parte do código AEM e implantadas por meio do pipeline](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) de configuração do Cloud Manager [Web Tier ou do [pipeline](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) de pilha cheia. Dessa forma, AEM processo específico do projeto está em andamento para gerenciar, implantar e rastrear as regras redirecionar.

A maioria dos serviços CDN armazena em cache os redirecionamentos HTTP 301 e 302, dependendo de seus cabeçalhos `Cache-Control` ou `Expires`. Ajuda a evitar a viagem de ida e volta após o redirecionamento inicial originado no Apache/Dispatcher.


### ACS AEM Commons

Há dois recursos disponíveis no [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) para gerenciar redirecionamentos de URL. Observe que o ACS AEM Commons é um projeto operado pela comunidade e de código aberto, e não é suportado pela Adobe.

#### Gerenciador do Mapa de Redirecionamento

O [Redirect Map Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) ajuda os administradores do AEM a manter e publicar facilmente os arquivos do [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) sem acessar diretamente o Apache Web Server ou exigir a reinicialização do Apache Web Server. Esse recurso permite que os usuários de permissões criem, atualizem e excluam regras de redirecionamento de um console no AEM, sem a ajuda da equipe de desenvolvimento ou de uma implantação do AEM. O Gerenciador do Mapa de Redirecionamento é compatível com o **AEM as a Cloud Service** (consulte a estratégia [Redirecionamentos de URL sem pipeline](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) e o [tutorial](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/administration/implementing-pipeline-free-url-redirects#acs-commons---redirect-map-manager) relacionado) e o **AEM 6.x**.

#### Gerenciador de redirecionamento

[O Gerenciador de redirecionamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) permite que os usuários em AEM mantenham e publicar redirecionamentos de AEM. A implementação é baseada no filtro de servlet Java™, portanto, o consumo típico de recursos da JVM. Esse recurso também elimina a dependência do AEM equipe de desenvolvimento e das implantações AEM. O Gerenciador de redirecionamento é **AEM como um Cloud Service** e **AEM compatível com o 6.x** . Embora a solicitação inicial redirecionada hit o AEM Publish serviço para gerar o cache 301/302 (mais) das CDNs 301/302 por padrão, permitindo que solicitações subsequentes sejam redirecionadas na borda/CDN.

[O Gerenciador de redirecionamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) também oferece [suporte à grátis pipeline-URL estratégia de redirecionamentos](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) para **AEM como Cloud Service [** compilando redirecionamentos em um arquivo](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/subpages/rewritemap.html) de texto para [o Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html), de modo que permita a atualização de redirecionamentos usados no servidor Web Apache sem acessá-lo diretamente ou exigir sua reinicialização. Consulte o [tutorial](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/administration/implementing-pipeline-free-url-redirects#acs-commons---redirect-manager) para obter mais detalhes. Nesse cenário, o redirecionar inicial solicitação atinge o servidor Web Apache e não o AEM serviço Publish.

### O `Redirect` propriedade do página

A página inicial (OOTB) `Redirect` propriedade do [Avançado guia](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties.html) permite que conteúdo autores definam o local redirecionar para o página atual. Essa solução é melhor para cenários redirecionar por página e não tem uma localização central para visualização e gerenciar os redirecionamentos página.

## Qual solução é adequada para implementação

Abaixo estão alguns critérios para determinar a solução correta. Além disso, o processo de TI e marketing de sua organização deve ajudar a escolher a solução certa.

1. Permitir que a equipe de marketing ou os superusuários gerenciem regras de redirecionamento sem a equipe de desenvolvimento do AEM e as implantações do AEM.
1. O processo para gerenciar, verificar, controlar e reverter as alterações ou a mitigação de riscos.
1. Disponibilidade de _Experiência no Assunto_ para **Na Edge por meio da solução CDN Service**.
