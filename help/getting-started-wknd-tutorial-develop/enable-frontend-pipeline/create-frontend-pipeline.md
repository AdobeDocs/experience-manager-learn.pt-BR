---
title: Implantar usando o pipeline de front-end
description: Saiba como criar e executar um pipeline de front-end que cria recursos de front-end e implanta no CDN integrado no AEM as a Cloud Service.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: d6da05e4-bd65-4625-b9a4-cad8eae3c9d7
duration: 225
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 0%

---

# Implantar usando o pipeline de front-end

Neste capítulo, criamos e executamos um pipeline de front-end no Adobe Cloud Manager. Ele só cria os arquivos de `ui.frontend` e os implanta no CDN incorporado no AEM as a Cloud Service. Assim, afastando-se da  `/etc.clientlibs` com base na entrega de recursos de front-end.


## Objetivos {#objectives}

* Crie e execute um pipeline de front-end.
* Verificar se os recursos de front-end NÃO são entregues pelo `/etc.clientlibs` mas de um novo nome de host que começa com `https://static-`

## Uso do pipeline de front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409420?quality=12&learn=on)

## Pré-requisitos {#prerequisites}

Este é um tutorial em várias partes e presume-se que as etapas descritas no [Atualizar projeto AEM padrão](./update-project.md) foram concluídas.

Verifique se você tem [privilégios para criar e implantar pipelines no Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions) e [acesso a um ambiente as a Cloud Service do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html).

## Renomear pipeline existente

Renomear o pipeline existente de __Implantar para desenvolvimento__ para  __Implantação do FullStack WKND no Desenvolvimento__ acessando o __Configuração__ da guia __Nome do pipeline de não produção__ campo. Isso é para tornar explícito se um pipeline é de pilha completa ou front-end apenas observando seu nome.

![Renomear pipeline](assets/fullstack-wknd-deploy-dev-pipeline.png)


Também no __Código-fonte__ , verifique se os valores de campo Repositório e Ramificação Git estão corretos e se a ramificação tem suas alterações de contrato de pipeline de front-end.

![Pipeline de configuração do código-fonte](assets/fullstack-wknd-source-code-config.png)


## Criar um pipeline de front-end

Para __SOMENTE__ criar e implantar os recursos de front-end a partir do `ui.frontend` , execute as seguintes etapas:

1. Na interface do usuário do Cloud Manager, no __Pipelines__ clique em __Adicionar__ e selecione __Adicionar pipeline de não produção__ (ou __Adicionar pipeline de produção__) com base no ambiente as a Cloud Service AEM no qual você deseja implantar.

1. No __Adicionar pipeline de não produção__ como parte da __Configuração__ , selecione a __Pipeline de implantação__ opção, nomeie-a como __Implantação WKND de FrontEnd para Desenvolvimento__ e clique em __Continuar__

![Criar configurações de pipeline front-end](assets/create-frontend-pipeline-configs.png)

1. Como parte da __Código-fonte__ , selecione a __Código de front-end__ e escolha o ambiente em __Ambientes de implantação qualificados__. No __Código-fonte__ seção verifique se os valores de campo Repositório e Ramificação Git estão corretos e se a ramificação tem suas alterações de contrato de pipeline de front-end.
E __mais importante__ para o __Localização do código__ o valor é `/ui.frontend` e, por fim, clique em __Salvar__.

![Criar código-fonte do pipeline de front-end](assets/create-frontend-pipeline-source-code.png)


## Sequência de implantação

* Primeiro execute o recém-renomeado __Implantação do FullStack WKND no Desenvolvimento__ pipeline para remover os arquivos clientlib WKND do repositório AEM. E, o mais importante, prepare o AEM para o contrato de pipeline de front-end adicionando __Configuração do Sling__ arquivos (`SiteConfig`, `HtmlPageItemsConfig`).

![Site WKND sem estilo](assets/unstyled-wknd-site.png)

>[!WARNING]
>
>Depois, a variável __Implantação do FullStack WKND no Desenvolvimento__ conclusão do pipeline, você terá um __sem estilo__ Site da WKND, que pode parecer quebrado. Planeje uma interrupção ou implante durante horas ímpares. Essa é uma interrupção única que você precisa planejar durante o switch inicial, desde usar um pipeline de pilha completa única até o pipeline de front-end.


* Por fim, execute o __Implantação WKND de FrontEnd para Desenvolvimento__ pipeline somente para compilação `ui.frontend` e implante os recursos de front-end diretamente na CDN.

>[!IMPORTANT]
>
>Você percebe que a variável __sem estilo__ O site da WKND voltou ao normal e desta vez __FrontEnd__ a execução do pipeline foi muito mais rápida do que o pipeline de pilha completa.

## Verificar alterações de estilo e novo paradigma de entrega

* Abra qualquer página do site WKND e você poderá ver o texto e colorir __Adobe Vermelho__ e os arquivos de recursos de front-end (CSS, JS) são entregues pela CDN. O nome de host da solicitação de recurso começa com `https://static-pXX-eYY.p123-e456.adobeaemcloud.com/$HASH_VALUE$/theme/site.css` e também o site.js ou qualquer outro recurso estático que você referenciou na variável `HtmlPageItemsConfig` arquivo.


![Site WKND recém-estilizado](assets/newly-styled-wknd-site.png)



>[!TIP]
>
>A variável `$HASH_VALUE$` aqui é o mesmo que você vê no __Implantação WKND de FrontEnd para Desenvolvimento__  do pipeline __HASH DE CONTEÚDO__ campo. O AEM é notificado do URL CDN do recurso front-end, o valor é armazenado em `/conf/wknd/sling:configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/jcr:content` em __prefixPath__ propriedade.


![Correlação do valor de hash](assets/hash-value-correlartion.png)



## Parabéns. {#congratulations}

Parabéns, você criou, executou e verificou o pipeline de front-end que só cria e implanta o módulo &quot;ui.frontend&quot; do projeto do WKND Sites. Agora, sua equipe de front-end pode iterar rapidamente sobre o design do site e o comportamento de front-end, fora do ciclo de vida completo do projeto AEM.

## Próximas etapas {#next-steps}

No próximo capítulo, [Considerações](considerations.md), você analisará o impacto no processo de desenvolvimento front-end e back-end.
