---
user-guide-title: Tutoriais do Adobe Experience Manager as a Cloud Service
user-guide-description: Uma coleção de tutoriais do Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutoriais do AEM as a Cloud Service
sub-product: cloud-service
team: TM
translation-type: tm+mt
source-git-commit: cb4f678be79ad39110cc199b8c66349f311a431d
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 22%

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
   + [Tempo de Execução do AEM Local](./local-development-environment/aem-runtime.md)
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
   + [JavaDocs da API do SDK AEM](https://docs.adobe.com/content/help/en/experience-manager-cloud-service-javadoc/)
+ Depuração de AEM{#debugging}
   + Depuração do SDK do AEM{#debugging-aem-sdk}
      + [Visão geral](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Logs](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Depuração remota](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Console da Web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Ferramentas do Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Outras ferramentas](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Depuração de AEM como um Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Visão geral](./debugging/cloud-service/overview.md)
      + [Logs](./debugging/cloud-service/logs.md)
      + [Criar e implantar](./debugging/cloud-service/build-and-deployment.md)
      + [Console do desenvolvedor](./debugging/cloud-service/developer-console.md)
      + [CRXDE Lite](./debugging/cloud-service/crxde-lite.md)
+ Acessar AEM{#accessing}
   + [Visão geral](./accessing/overview.md)
   + [Usuários de Adobe IMS](./accessing/adobe-ims-users.md)
   + [Grupos de usuários do Adobe IMS](./accessing/adobe-ims-user-groups.md)
   + [Perfis de produto Adobe IMS](./accessing/adobe-ims-product-profiles.md)
   + [AEM usuários, grupos e permissões](./accessing/aem-users-groups-and-permissions.md)
   + [Configuração do acesso ao AEM](./accessing/walk-through.md)
+ Migração {#migration}
   + [Ferramenta Transferência de conteúdo](./migration/content-transfer-tool.md)
   + [Importação em massa de ativos](./migration/bulk-import.md)
+ Forms{#forms}
   + Criar formulário adaptável{#create-first-af}
      + [Introdução](./forms/create-first-af/introduction.md)
      + [Criar tema](./forms/create-first-af/create-theme.md)
      + [Criar modelo](./forms/create-first-af/create-template.md)
      + [Criar fragmento](./forms/create-first-af/create-fragments.md)
      + [Criar formulário](./forms/create-first-af/create-af.md)
      + [Configurar painel raiz](./forms/create-first-af/configure-root-panel.md)
      + [Configurar painel de pessoas](./forms/create-first-af/configure-people-panel.md)
      + [Configurar painel de renda](./forms/create-first-af/configure-income-panel.md)
      + [Configurar painel de ativos](./forms/create-first-af/configure-assets-panel.md)
      + [Configurar o painel de início](./forms/create-first-af/configure-start-panel.md)
      + [Adicionar e configurar a barra de ferramentas](./forms/create-first-af/add-configure-toolbar.md)
   + Criar Fluxo de Trabalho de Revisão{#create-aem-workflow}
      + [Criar modelo de fluxo de trabalho](./forms/create-aem-workflow/create-workflow.md)
      + [Acionar fluxo de trabalho](./forms/create-aem-workflow/configure-af.md)
   + Adobe Sign com AEM Forms{#forms-and-sign}
      + [Aplicativo de API do Adobe Sign](./forms/forms-and-sign/create-sign-api-application.md)
      + [Configuração de nuvem do Adobe Sign](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [Criar formulário adaptável](./forms/forms-and-sign/create-adaptive-form.md)
      + [Configurar para preenchimento e sinal](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + Integrar com o Salesforce{#integrate-with-salesforce}
      + [Introdução](./forms/integrate-with-salesforce/introduction.md)
      + [Criar aplicativo conectado](./forms/integrate-with-salesforce/create-connected-app.md)
      + [Criar arquivo swagger](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [Criar fonte de dados](./forms/integrate-with-salesforce/create-data-source.md)
      + [Criar modelo de dados de formulário](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [Apresentação do formulário de teste](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [Testar evento de clique](./forms/integrate-with-salesforce/create-lead-click-event.md)
+ Extensibilidade do Asset compute{#asset-compute}
   + [Visão geral](./asset-compute/overview.md)
   + Configurar{#set-up}
      + [Provisionamento de conta e serviços](./asset-compute/set-up/accounts-and-services.md)
      + [Ambiente de desenvolvimento local](./asset-compute/set-up/development-environment.md)
      + [Adobe Project Firefly](./asset-compute/set-up/firefly.md)
   + Desenvolver{#develop}
      + [Criar um projeto do Asset compute](./asset-compute/develop/project.md)
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
   + [Desenvolvimento do AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/develop-wknd-tutorial.html)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [Editor de SPA (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [Editor de SPA (Angular)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [AEM Sites e Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [Autenticação por token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
