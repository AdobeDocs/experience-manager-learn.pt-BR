---
title: Implantar usando o pipeline de front-end
description: Saiba como criar e executar um pipeline de front-end que cria recursos de front-end e implanta no CDN integrado no AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
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
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 0%

---

# Implantar usando o pipeline de front-end

Neste capítulo, criamos e executamos um pipeline de front-end no Adobe Cloud Manager. Ele só compila os arquivos do módulo `ui.frontend` e os implanta no CDN incorporado no AEM as a Cloud Service. Afastando-se assim da entrega de recursos de front-end baseada em `/etc.clientlibs`.


## Objetivos {#objectives}

* Crie e execute um pipeline de front-end.
* Verifique se os recursos de front-end NÃO são entregues a partir de `/etc.clientlibs`, mas de um novo nome de host que comece com `https://static-`

## Uso do pipeline de front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409420?quality=12&learn=on)

## Pré-requisitos {#prerequisites}

Este é um tutorial em várias partes e presume-se que as etapas descritas no [Projeto de Atualização de AEM Padrão](./update-project.md) foram concluídas.

Verifique se você tem [privilégios para criar e implantar pipelines no Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=pt-BR#role-definitions) e [acesso a um ambiente do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=pt-BR).

## Renomear pipeline existente

Renomeie o pipeline existente de __Implantar em Desenvolvimento__ para __Implantação WKND de Empilhamento Completo em Desenvolvimento__ indo para o campo __Nome do Pipeline de Não Produção__ da guia __Configuração__. Isso é para tornar explícito se um pipeline é de pilha completa ou front-end apenas observando seu nome.

![Renomear Pipeline](assets/fullstack-wknd-deploy-dev-pipeline.png)


Além disso, na guia __Código Source__, verifique se os valores de campo do Repositório e da Ramificação Git estão corretos e se a ramificação tem suas alterações de contrato de pipeline de front-end.

![Pipeline de configuração de código do Source](assets/fullstack-wknd-source-code-config.png)


## Criar um pipeline de front-end

Para __SOMENTE__ compilar e implantar os recursos de front-end do módulo `ui.frontend`, execute as seguintes etapas:

1. Na interface do usuário do Cloud Manager, na seção __Pipelines__, clique no botão __Adicionar__ e selecione __Adicionar pipeline de não produção__ (ou __Adicionar pipeline de produção__) com base no ambiente do AEM as a Cloud Service no qual você deseja implantar.

1. Na caixa de diálogo __Adicionar Pipeline de Não Produção__, como parte das etapas __Configuração__, selecione a opção __Pipeline de Implantação__, nomeie-a como __Implantação WKND FrontEnd para Desenvolvimento__ e clique em __Continuar__

![Criar Configurações De Pipeline De Front-End](assets/create-frontend-pipeline-configs.png)

1. Como parte das etapas __Código Source__, selecione a opção __Código de front-end__ e escolha o ambiente em __Ambientes de implantação qualificados__. Na seção __Código Source__, verifique se os valores dos campos Repositório e Ramificação Git estão corretos e se a ramificação tem suas alterações no contrato do pipeline de front-end.
E __o mais importante__ para o campo __Localização do Código__, o valor é `/ui.frontend` e, por fim, clique em __Salvar__.

![Criar código Source do pipeline de front-end](assets/create-frontend-pipeline-source-code.png)


## Sequência de implantação

* Primeiro execute o pipeline __FullStack WKND Deploy para Dev__ recém-renomeado para remover os arquivos clientlib WKND do repositório do AEM. E, o mais importante, prepare o AEM para o contrato de pipeline de front-end adicionando __arquivos de configuração do Sling__ (`SiteConfig`, `HtmlPageItemsConfig`).

![Site WKND Sem Estilo](assets/unstyled-wknd-site.png)

>[!WARNING]
>
>Após a conclusão do pipeline __FullStack WKND Dev__, você terá um Site WKND __sem estilo__, que pode parecer quebrado. Planeje uma interrupção ou implante durante horas ímpares. Essa é uma interrupção única que você precisa planejar durante o switch inicial, desde usar um pipeline de pilha completa única até o pipeline de front-end.


* Finalmente, execute a Implantação do WKND de __FrontEnd no pipeline de Desenvolvimento__ para compilar somente o módulo `ui.frontend` e implantar os recursos de front-end diretamente no CDN.

>[!IMPORTANT]
>
>Você percebe que o site WKND __sem estilo__ voltou ao normal e, desta vez, a execução do pipeline __FrontEnd__ foi muito mais rápida do que o pipeline de pilha completa.

## Verificar alterações de estilo e novo paradigma de entrega

* Abra qualquer página do Site WKND e você poderá ver a cor do texto usando o __Adobe Red__, e os arquivos de recursos de front-end (CSS, JS) serão entregues pela CDN. O nome de host da solicitação de recurso começa com `https://static-pXX-eYY.p123-e456.adobeaemcloud.com/$HASH_VALUE$/theme/site.css` e também com o site.js ou qualquer outro recurso estático que você referenciou no arquivo `HtmlPageItemsConfig`.


![Site WKND Recém-Estilizado](assets/newly-styled-wknd-site.png)



>[!TIP]
>
>O `$HASH_VALUE$` aqui é o mesmo que você vê no campo __CONTENT HASH__ da __Implantação WKND de FrontEnd no pipeline de Desenvolvimento__. O AEM é notificado da URL CDN do recurso front-end; o valor é armazenado em `/conf/wknd/sling:configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/jcr:content` na propriedade __prefixPath__.


![Correlação de Valor de Hash](assets/hash-value-correlartion.png)



## Parabéns! {#congratulations}

Parabéns, você criou, executou e verificou o pipeline de front-end que só cria e implanta o módulo &quot;ui.frontend&quot; do projeto do WKND Sites. Agora, sua equipe de front-end pode iterar rapidamente sobre o design do site e o comportamento do front-end, fora do ciclo de vida completo do projeto do AEM.

## Próximas etapas {#next-steps}

No próximo capítulo, [Considerações](considerations.md), você analisará o impacto no processo de desenvolvimento front-end e back-end.
