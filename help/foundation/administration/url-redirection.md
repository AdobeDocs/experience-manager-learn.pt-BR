---
title: Redirecionamentos de URL
description: Saiba mais sobre as várias opções para executar o redirecionamento de URL no AEM.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
jira: KT-11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
duration: 164
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '781'
ht-degree: 0%

---

# Redirecionamentos de URL

O redirecionamento de URL é um aspecto comum como parte da operação do site. Os arquitetos e administradores são desafiados a encontrar a melhor solução sobre como e onde gerenciar os redirecionamentos de URL que fornecem flexibilidade e tempo de implantação de redirecionamento rápido.

Familiarize-se com o [AEM (6.x) também conhecido como AEM Classic](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) e [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) infraestrutura. As principais diferenças são:

1. O AEM AS A CLOUD SERVICE [CDN integrada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html)No entanto, os clientes podem fornecer um CDN (BYOCDN) na frente de um CDN gerenciado por AEM.
1. O AEM 6.x, no local ou o Adobe Managed Services (AMS), não inclui uma CDN gerenciada pelo AEM, e os clientes devem trazer a sua própria CDN.

Os outros serviços de AEM (AEM Author/Publish e Dispatcher) são conceitualmente semelhantes entre o AEM AEM 6.x e o as a Cloud Service.

As soluções de redirecionamento de URL do AEM são as seguintes:

|                                                   | Gerenciado e implantado como código de projeto AEM | Capacidade de alteração pela equipe de marketing/conteúdo | Compatível com AEM as Cloud Service | Onde ocorre a execução do redirecionamento |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [No Edge por meio do, traga seu próprio CDN](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN |
| [Apache `mod_rewrite` regras como configuração do Dispatcher](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons - Redirect Map Manager](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS Commons - Gerenciador de redirecionamento](#redirect-manager) | ✘ | ✔ | ✔ | AEM |
| [A variável `Redirect` propriedade da página](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## Opções de solução

A seguir estão opções de solução na ordem em que estão mais perto do navegador do visitante do site.

### No Edge por meio do, traga seu próprio CDN

Alguns serviços de CDN fornecem soluções de redirecionamento no nível da borda, reduzindo assim as viagens de ida e volta à origem. Consulte [Redirecionador de borda Akamai](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [Funções do AWS CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Consulte seu provedor de serviços CDN para obter o recurso de redirecionamento de nível de borda.

O gerenciamento de redirecionamentos no nível da borda ou CDN tem vantagens de desempenho, no entanto, não são gerenciados como parte do AEM, mas sim projetos discretos. Um processo bem pensado para gerenciar e implantar regras de redirecionamento é crucial para evitar problemas.


### Apache `mod_rewrite` módulo

Uma solução comum usa [Apache Module mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). A variável [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype) O fornece uma estrutura de projeto do Dispatcher para ambos [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) e [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) projeto. As regras de regravação padrão (imutáveis) e personalizadas são definidas no `conf.d/rewrites` e o mecanismo de regravação fica ATIVADO por `virtualhosts` que escuta na porta `80` via `conf.d/dispatcher_vhost.conf` arquivo. Um exemplo de implementação está disponível na [Projeto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

No AEM as a Cloud Service, essas regras de redirecionamento são gerenciadas como parte do código do AEM e implantadas pelo Cloud Manager [Pipeline de configuração no nível da Web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) ou [Pipeline de pilha completa](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline). Assim, o processo específico do projeto AEM está em andamento para gerenciar, implantar e rastrear as regras de redirecionamento.

A maioria dos serviços CDN armazena em cache os redirecionamentos HTTP 301 e 302, dependendo de seus `Cache-Control` ou `Expires` cabeçalhos. Isso ajuda a evitar a viagem de ida e volta após o redirecionamento inicial originado no Apache/Dispatcher.


### ACS AEM Commons

Há dois recursos disponíveis no [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) para gerenciar redirecionamentos de URL. Por favor, note que o ACS AEM Commons é um projeto operado pela comunidade, de código aberto e não apoiado pela Adobe.

#### Gerenciador do Mapa de Redirecionamento

[Gerenciador do Mapa de Redirecionamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) permite que administradores do AEM 6.x façam a manutenção e a publicação com facilidade [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) arquivos sem acessar diretamente o Apache Web Server ou exigir uma reinicialização do Apache Web Server. Esse recurso permite que os usuários de permissões criem, atualizem e excluam regras de redirecionamento de um console no AEM, sem a ajuda da equipe de desenvolvimento ou de uma implantação do AEM. O Gerenciador do Mapa de Redirecionamento está **NÃO compatível com AEM as a Cloud Service**.

#### Gerenciador de redirecionamento

[Gerenciador de redirecionamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) O permite que os usuários no AEM mantenham e publiquem facilmente redirecionamentos do AEM. A implementação é baseada no filtro de servlet Java™, portanto, no consumo típico de recursos JVM. Esse recurso também elimina a dependência da equipe de desenvolvimento do AEM e das implantações do AEM. O Gerenciador de redirecionamento é ambos **AEM as a Cloud Service** e **AEM 6.x** compatível. Embora a solicitação redirecionada inicial deva atingir o serviço de publicação do AEM para gerar o cache 301/302 (a maioria) dos CDNs 301/302 por padrão, permitindo que as solicitações subsequentes sejam redirecionadas na borda/CDN.

### A variável `Redirect` propriedade da página

O pacote pronto para uso (OOTB) `Redirect` propriedade de página do [Guia Avançado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/fundamentals/page-properties.html#advanced) permite que os autores de conteúdo definam o local de redirecionamento para a página atual. Essa solução é mais adequada para cenários de redirecionamento por página e não tem um local central para exibir e gerenciar os redirecionamentos de página.

## Qual é a solução certa para implementação

Abaixo estão alguns critérios para determinar a solução correta. Além disso, o processo de TI e marketing de sua organização deve ajudar a escolher a solução certa.

1. Permitir que a equipe de marketing ou os superusuários gerenciem regras de redirecionamento sem a equipe de desenvolvimento do AEM e as implantações do AEM.
1. O processo para gerenciar, verificar, controlar e reverter as alterações ou a mitigação de riscos.
1. Disponibilidade de _Expertise no assunto_ para **No Edge pelo serviço CDN** solução.
