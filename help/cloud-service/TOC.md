---
user-guide-title: Tutoriais do Adobe Experience Manager as a Cloud Service
user-guide-description: Uma coleção de tutoriais do Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutoriais do AEM as a Cloud Service
sub-product: cloud-service
team: TM
translation-type: tm+mt
source-git-commit: 59b786d95d1428916adad37ceca4412b93463e9b
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 25%

---


# Tutoriais do Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Visão geral](./overview.md)
+ Introdução ao AEM as a Cloud Service{#introduction}
   + [O que é o AEM as a Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Evolução](./introduction/evolution.md)
   + [Arquitetura](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
+ Tecnologia subjacente {#underlying-technology}
   + [Arquitetura do AEM](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Repositório de conteúdo Java](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [Serviços de criação e publicação](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Cloud Manager {#cloud-manager}
   + [Programas](./cloud-manager/programs.md)
   + [Ambientes](./cloud-manager/environments.md)
   + [Pipeline de produção de CI/CD](./cloud-manager/cicd-production-pipeline.md)
   + [Pipeline de não produção de CI/CD](./cloud-manager/cicd-non-production-pipeline.md)
   + [Atividade](./cloud-manager/activity.md)
   + Dev Ops{#devops}
      + [Implantação do código](./cloud-manager/devops/deploy-code.md)
      + [Mesclar projetos](./cloud-manager/devops/merge-projects.md)
      + [Configurar pipeline](./cloud-manager/devops/configure-pipelines.md)
      + [Integração contínua](./cloud-manager/devops/continuous-integration.md)
      + [Analisar resultados de teste](./cloud-manager/devops/analyze-test-results.md)
      + [Configurações do Dispatcher](./cloud-manager/devops/dispatcher-configurations.md)
      + [APIs do Cloud Manager](./cloud-manager/devops/cloud-manager-apis.md)
+ Configuração do Ambiente de Desenvolvimento Local {#local-development-environment-set-up}
   + [Visão geral](./local-development-environment/overview.md)
   + [Ferramentas de desenvolvimento](./local-development-environment/development-tools.md)
   + [Tempo de execução local do AEM](./local-development-environment/aem-runtime.md)
   + [Ferramentas locais do Dispatcher](./local-development-environment/dispatcher-tools.md)
+ Desenvolvimento{#developing}
   + Noções básicas sobre desenvolvimento{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [Ambiente de desenvolvimento local](./developing/basics/local-development-environment.md)
      + [Arquétipo de projeto do AEM](./developing/basics/aem-project-archetype.md)
      + [Estrutura de projetos do AEM](./developing/basics/project-structure.md)
      + [Conteúdo variável vs. imutável](./developing/basics/mutable-immutable.md)
      + [Pacote de estrutura do repositório](./developing/basics/repository-structure-package.md)
      + [Publicação de conteúdo](./developing/basics/content-publishing.md)
      + [Configurações do OSGi](./developing/basics/osgi-configurations.md)
      + [Migração de configuração do Dispatcher](./developing/basics/dispatcher-configuration.md)
   + [JavaDocs da API do SDK do AEM](https://docs.adobe.com/content/help/en/experience-manager-cloud-service-javadoc/)
+ Depuração do AEM{#debugging}
   + Depuração do SDK do AEM{#debugging-aem-sdk}
      + [Visão geral](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Logs](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Depuração remota](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Console da Web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Ferramentas do Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Outras ferramentas](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Depuração do AEM as a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Visão geral](./debugging/cloud-service/overview.md)
      + [Logs](./debugging/cloud-service/logs.md)
      + [Criar e implantar](./debugging/cloud-service/build-and-deployment.md)
      + [Console do desenvolvedor](./debugging/cloud-service/developer-console.md)
      + [CRXDE Lite](./debugging/cloud-service/crxde-lite.md)
+ Acesso ao AEM{#accessing}
   + [Visão geral](./accessing/overview.md)
   + [Usuários do Adobe IMS](./accessing/adobe-ims-users.md)
   + [Grupos de usuários do Adobe IMS](./accessing/adobe-ims-user-groups.md)
   + [Perfis de produto do Adobe IMS](./accessing/adobe-ims-product-profiles.md)
   + [Usuários, grupos e permissões do AEM](./accessing/aem-users-groups-and-permissions.md)
   + [Configuração do acesso ao AEM](./accessing/walk-through.md)
+ Migração {#migration}
   + [Ferramenta Transferência de conteúdo](./migration/content-transfer-tool.md)
   + [Importação em massa de ativos](./migration/bulk-import.md)
+ Extensibilidade do Asset Compute{#asset-compute}
   + [Visão geral](./asset-compute/overview.md)
   + Configurar{#set-up}
      + [Provisionamento de conta e serviços](./asset-compute/set-up/accounts-and-services.md)
      + [Ambiente de desenvolvimento local](./asset-compute/set-up/development-environment.md)
      + [Adobe Project Firefly](./asset-compute/set-up/firefly.md)
   + Desenvolver{#develop}
      + [Criar um projeto do Asset Compute](./asset-compute/develop/project.md)
      + [Configurar variáveis de ambiente](./asset-compute/develop/environment-variables.md)
      + [Configurar o manifest.yml](./asset-compute/develop/manifest.md)
      + [Desenvolver um trabalhador](./asset-compute/develop/worker.md)
      + [Usar a ferramenta de desenvolvimento](./asset-compute/develop/development-tool.md)
   + Testar e depurar{#test-debug}
      + [Testar um trabalhador](./asset-compute/test-debug/test.md)
      + [Depurar um trabalhador](./asset-compute/test-debug/debug.md)
   + Implantar{#deploy}
      + [Implantar no Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [Integrar ao AEM](./asset-compute/deploy/processing-profiles.md)
   + Avançado {#advanced}
      + [Trabalhadores de metadados](./asset-compute/advanced/metadata.md)
   + [Resolução de Problemas](./asset-compute/troubleshooting.md)
+ Tutoriais em várias etapas{#multi-step-tutorials}
   + [Desenvolvimento do AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/develop-wknd-tutorial.html)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [Editor de SPA (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [Editor SPA (Angular)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [AEM Sites e Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [Autenticação por token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)

