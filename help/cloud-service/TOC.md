---
user-guide-title: Tutoriais do Adobe Experience Manager as a Cloud Service
user-guide-description: Uma coleção de tutoriais do Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutoriais do AEM as a Cloud Service
sub-product: serviço em nuvem
team: TM
translation-type: tm+mt
source-git-commit: 91399ff4ab26655de27bbecab4ff773f6666d86a
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 25%

---


# Tutoriais do Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Visão geral](./overview.md)
+ Introdução ao AEM as a Cloud Service{#introduction}
   + [O que é AEM como Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Evolução](./introduction/evolution.md)
   + [Arquitetura](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
+ Tecnologia subjacente {#underlying-technology}
   + [Arquitetura AEM](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Repositório de conteúdo Java](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [Serviços de autor e publicação](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Cloud Manager {#cloud-manager}
   + [Programas](./cloud-manager/programs.md)
   + [Ambientes](./cloud-manager/environments.md)
   + [Pipeline de produção CI/CD](./cloud-manager/cicd-production-pipeline.md)
   + [Pipeline de não produção CI/CD](./cloud-manager/cicd-non-production-pipeline.md)
   + [Atividade](./cloud-manager/activity.md)
   + Dev Ops{#devops}
      + [Implantação de código](./cloud-manager/devops/deploy-code.md)
      + [Mesclar projetos](./cloud-manager/devops/merge-projects.md)
      + [Configurar Pipelines](./cloud-manager/devops/configure-pipelines.md)
      + [Integração contínua](./cloud-manager/devops/continuous-integration.md)
      + [Analisar resultados do teste](./cloud-manager/devops/analyze-test-results.md)
      + [Configurações do Dispatcher](./cloud-manager/devops/dispatcher-configurations.md)
      + [APIs do Cloud Manager](./cloud-manager/devops/cloud-manager-apis.md)
+ Configuração do Ambiente de desenvolvimento local {#local-development-environment-set-up}
   + [Visão geral](./local-development-environment/overview.md)
   + [Ferramentas de desenvolvimento](./local-development-environment/development-tools.md)
   + [Tempo de execução de AEM local](./local-development-environment/aem-runtime.md)
   + [Ferramentas do Dispatcher Local](./local-development-environment/dispatcher-tools.md)
+ Desenvolvimento{#developing}
   + Conceitos básicos de desenvolvimento{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [Ambiente de desenvolvimento local](./developing/basics/local-development-environment.md)
      + [Arquétipo de projeto do AEM](./developing/basics/aem-project-archetype.md)
      + [Estrutura de projetos do AEM](./developing/basics/project-structure.md)
      + [Conteúdo mutável vs. imutável](./developing/basics/mutable-immutable.md)
      + [Pacote de estrutura do repositório](./developing/basics/repository-structure-package.md)
      + [Publicação de conteúdo](./developing/basics/content-publishing.md)
      + [Configurações do OSGi](./developing/basics/osgi-configurations.md)
      + [Migração de Configuração do Dispatcher](./developing/basics/dispatcher-configuration.md)
+ Depuração AEM{#debugging}
   + Depuração do SDK AEM{#debugging-aem-sdk}
      + [Visão geral](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Logs](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Depuração remota](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Console da Web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Ferramentas do Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Outras ferramentas](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Depuração de AEM como Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Visão geral](./debugging/cloud-service/overview.md)
      + [Logs](./debugging/cloud-service/logs.md)
      + [Criação e implantação](./debugging/cloud-service/build-and-deployment.md)
      + [Console do desenvolvedor](./debugging/cloud-service/developer-console.md)
      + [CRXDE Lite](./debugging/cloud-service/crxde-lite.md)
+ Acessar AEM{#accessing}
   + [Visão geral](./accessing/overview.md)
   + [Usuários de Adobe IMS](./accessing/adobe-ims-users.md)
   + [Grupos de usuários de Adobe IMS](./accessing/adobe-ims-user-groups.md)
   + [Perfis de produtos Adobe IMS](./accessing/adobe-ims-product-profiles.md)
   + [AEM usuários, grupos e permissões](./accessing/aem-users-groups-and-permissions.md)
   + [Configuração do acesso a AEM de navegação](./accessing/walk-through.md)
+ Migração {#migration}
   + [Ferramenta Transferência de conteúdo](./migration/content-transfer-tool.md)
   + [Importação em massa de ativos](./migration/bulk-import.md)
+ Extensibilidade do asset compute{#asset-compute}
   + [Visão geral](./asset-compute/overview.md)
   + Configurar{#set-up}
      + [Provisionamento de conta e serviços](./asset-compute/set-up/accounts-and-services.md)
      + [Ambiente de desenvolvimento local](./asset-compute/set-up/development-environment.md)
      + [Adobe Project Firefly](./asset-compute/set-up/firefly.md)
   + Desenvolver{#develop}
      + [Criar um projeto de Asset compute](./asset-compute/develop/project.md)
      + [Configurar variáveis de ambiente](./asset-compute/develop/environment-variables.md)
      + [Configurar o manifest.yml](./asset-compute/develop/manifest.md)
      + [Desenvolver um trabalhador](./asset-compute/develop/worker.md)
      + [Usar a ferramenta de desenvolvimento](./asset-compute/develop/development-tool.md)
   + Testar e depurar{#test-debug}
      + [Testar um trabalhador](./asset-compute/test-debug/test.md)
      + [Depurar um trabalhador](./asset-compute/test-debug/debug.md)
   + Implantar{#deploy}
      + [Implantar no Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [Integrar com AEM](./asset-compute/deploy/processing-profiles.md)
   + Avançado {#advanced}
      + [Trabalhadores de metadados](./asset-compute/advanced/metadata.md)
   + [Resolução de Problemas](./asset-compute/troubleshooting.md)
+ Tutorials de várias etapas{#multi-step-tutorials}
   + [Desenvolvimento AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/develop-wknd-tutorial.html)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [Editor de SPA (Reagir)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [Editor de SPA (Angular)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [AEM Sites e Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [Autenticação baseada em token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)

