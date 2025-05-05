---
title: Redirecionamentos de URL
description: Saiba mais sobre as várias opções para executar o redirecionamento de URL no AEM.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
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
source-git-commit: 62887c6251b09ac22664cfeb9c5513363efb555e
workflow-type: tm+mt
source-wordcount: '961'
ht-degree: 0%

---

# Redirecionamentos de URL

O redirecionamento de URL é um aspecto comum como parte da operação do site. Os arquitetos e administradores são desafiados a encontrar a melhor solução sobre como e onde gerenciar os redirecionamentos de URL que fornecem flexibilidade e tempo de implantação de redirecionamento rápido.

Familiarize-se com a infraestrutura do [AEM (6.x) também conhecida como AEM Classic](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/dispatcher-tutorial/chapter-2) e [AEM as a Cloud Service](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/overview/architecture). As principais diferenças são:

1. O AEM as a Cloud Service tem [CDN interna](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn), no entanto, os clientes podem fornecer um CDN (BYOCDN) na frente do CDN gerenciado pela AEM.
1. O AEM 6.x, no local ou no Adobe Managed Services (AMS), não inclui uma CDN gerenciada pela AEM, e os clientes devem trazer a sua própria CDN.

Os outros serviços do AEM (AEM Author/Publish e Dispatcher) são conceitualmente semelhantes entre o AEM 6.x e o AEM as a Cloud Service.

As soluções de redirecionamento de URL da AEM são as seguintes:

|                                                   | Gerenciado e implantado como código de projeto do AEM | Capacidade de alteração pela equipe de marketing/conteúdo | Compatível com AEM as Cloud Service | Onde ocorre a execução do redirecionamento |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [No Edge via CDN gerenciada pela AEM](#at-edge-via-aem-managed-cdn) | ✔ | ✘ | ✔ | Edge/CDN (Interno) |
| [Na Edge via, traga sua própria CDN (BYOCDN)](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN (BYOCDN) |
| [Regras do Apache `mod_rewrite` como configuração do Dispatcher](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons - Gerenciador de Mapa de Redirecionamento](#redirect-map-manager) | ✘ | ✔ | ✔ | Dispatcher |
| [ACS Commons - Gerenciador de redirecionamento](#redirect-manager) | ✘ | ✔ | ✔ | AEM/Dispatcher |
| [A propriedade da página `Redirect`](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## Opções de solução

A seguir estão opções de solução na ordem em que estão mais perto do navegador do visitante do site.

### No Edge, por meio da CDN gerenciada pela AEM {#at-edge-via-aem-managed-cdn}

Essa opção está disponível somente para clientes do AEM as a Cloud Service.

A [CDN gerenciada pela AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn) fornece uma solução de redirecionamento no nível da Edge, reduzindo assim as viagens de ida e volta à origem. O recurso [Redirecionamentos do lado do servidor](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#server-side-redirectors) permite configurar as regras de redirecionamento no código de projeto do AEM e implantar usando o [Pipeline de Configuração](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager). O tamanho do arquivo de configuração CDN (`cdn.yaml`) não deve exceder 100 KB.

O gerenciamento de redirecionamentos no nível da Edge ou CDN tem vantagens de desempenho.

### Na Edge, por meio do, traga seu próprio CDN

Alguns serviços de CDN fornecem soluções de redirecionamento no nível da Edge, reduzindo assim as viagens de ida e volta à origem. Consulte [Redirecionador Akamai Edge](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [Funções do AWS CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Consulte seu provedor de serviços CDN para obter recursos de redirecionamento de nível de Edge.

O gerenciamento de redirecionamentos no nível da Edge ou CDN tem vantagens de desempenho, no entanto, não são gerenciados como parte do AEM, mas como projetos discretos. Um processo bem definido para gerenciar e implantar regras de redirecionamento é fundamental para evitar problemas.


### Módulo Apache `mod_rewrite`

Uma solução comum usa o [Apache Module mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). O [Arquétipo de Projeto do AEM](https://github.com/adobe/aem-project-archetype) fornece uma estrutura de projeto do Dispatcher para os projetos [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) e [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure). As regras de regravação padrão (imutáveis) e personalizadas são definidas na pasta `conf.d/rewrites` e o mecanismo de regravação é ATIVADO para `virtualhosts` que escuta na porta `80` via arquivo `conf.d/dispatcher_vhost.conf`. Um exemplo de implementação está disponível no [Projeto do AEM WKND Sites](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

No AEM as a Cloud Service, essas regras de redirecionamento são gerenciadas como parte do código AEM e implantadas pelo [pipeline de configuração da Camada da Web](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) ou [pipeline de pilha completa](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) do Cloud Manager. Assim, seu processo específico de projeto do AEM está em andamento para gerenciar, implantar e rastrear as regras de redirecionamento.

A maioria dos serviços CDN armazena em cache os redirecionamentos HTTP 301 e 302, dependendo de seus cabeçalhos `Cache-Control` ou `Expires`. Ajuda a evitar a viagem de ida e volta após o redirecionamento inicial originado no Apache/Dispatcher.


### ACS AEM Commons

Há dois recursos disponíveis no [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) para gerenciar redirecionamentos de URL. Observe que o ACS AEM Commons é um projeto operado pela comunidade e de código aberto, e não é suportado pela Adobe.

#### Gerenciador do Mapa de Redirecionamento

O [Redirect Map Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) ajuda os administradores do AEM a manter e publicar facilmente os arquivos do [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) sem acessar diretamente o Apache Web Server ou exigir a reinicialização do Apache Web Server. Esse recurso permite que os usuários de permissões criem, atualizem e excluam regras de redirecionamento de um console no AEM, sem a ajuda da equipe de desenvolvimento ou de uma implantação do AEM. O Gerenciador do Mapa de Redirecionamento é compatível com o **AEM as a Cloud Service** (consulte a estratégia [Redirecionamentos de URL sem pipeline](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) e o [tutorial](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/foundation/administration/implementing-pipeline-free-url-redirects#acs-commons---redirect-map-manager) relacionado) e o **AEM 6.x**.

#### Gerenciador de redirecionamento

O [Gerenciador de Redirecionamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) permite que os usuários no AEM mantenham e publiquem facilmente os redirecionamentos do AEM. A implementação é baseada no filtro de servlet Java™, portanto, no consumo típico de recursos JVM. Esse recurso também elimina a dependência da equipe de desenvolvimento do AEM e das implantações do AEM. O Gerenciador de Redirecionamento é compatível com o **AEM as a Cloud Service** e o **AEM 6.x**. Embora a solicitação redirecionada inicial precise fazer com que o serviço de Publicação do AEM gere o cache 301/302 (a maioria) dos CDNs 301/302 por padrão, permitindo que as solicitações subsequentes sejam redirecionadas na borda/CDN.

O [Gerenciador de Redirecionamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) também oferece suporte à estratégia [Redirecionamentos de URL sem pipeline](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) para **AEM as a Cloud Service** ao [compilar redirecionamentos em um arquivo de texto](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/subpages/rewritemap.html) para [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html), de modo que ele permite a atualização de redirecionamentos usados no Apache Web Server sem acessá-lo diretamente ou exigir sua reinicialização. Consulte o [tutorial](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/foundation/administration/implementing-pipeline-free-url-redirects#acs-commons---redirect-manager) para obter mais detalhes. Nesse cenário, a solicitação de redirecionamento inicial atinge o servidor Web Apache, e não o serviço de Publicação do AEM.

### A propriedade da página `Redirect`

A propriedade de página `Redirect` pronta para uso (OOTB) da [guia Avançado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties.html?lang=pt-BR) permite que os autores de conteúdo definam o local de redirecionamento para a página atual. Essa solução é mais adequada para cenários de redirecionamento por página e não tem um local central para exibir e gerenciar os redirecionamentos de página.

## Qual é a solução certa para implementação

Abaixo estão alguns critérios para determinar a solução correta. Além disso, o processo de TI e marketing de sua organização deve ajudar a escolher a solução certa.

1. Permitir que a equipe de marketing ou os superusuários gerenciem regras de redirecionamento sem a equipe de desenvolvimento do AEM e as implantações do AEM.
1. O processo para gerenciar, verificar, controlar e reverter as alterações ou a mitigação de riscos.
1. Disponibilidade de _Experiência no Assunto_ para **Na Edge por meio da solução CDN Service**.
