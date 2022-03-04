---
user-guide-title: Tutoriais do Adobe Experience Manager as a Cloud Service
user-guide-description: Uma coleção de tutoriais do Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutoriais do AEM as a Cloud Service
sub-product: cloud-service
team: TM
source-git-commit: 5da75b172a7dda29452954990f6ab2374e7698d9
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 23%

---


# Tutoriais do Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Visão geral](./overview.md)
+ Introdução ao AEM as a Cloud Service{#introduction}
   + [O que é AEM as a Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
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
   + Ops de desenvolvimento{#devops}
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
   + Noções básicas de desenvolvimento{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [Ambiente de desenvolvimento local](./developing/basics/local-development-environment.md)
      + [Arquétipo de projeto do AEM](./developing/basics/aem-project-archetype.md)
      + [Estrutura de projetos do AEM](./developing/basics/project-structure.md)
      + [Conteúdo variável vs. imutável](./developing/basics/mutable-immutable.md)
      + [Pacote de estrutura do repositório](./developing/basics/repository-structure-package.md)
      + [Publicação de conteúdo](./developing/basics/content-publishing.md)
      + [Configurações do OSGi](./developing/basics/osgi-configurations.md)
      + [Migração de configuração do Dispatcher](./developing/basics/dispatcher-configuration.md)
   + Projetos AEM{#aem-projects}
      + [Projeto AEM Maven](./developing/projects/maven-project-structure.md)
      + [Limpando um projeto AEM Maven](./developing/projects/remove-samples.md)
   + Serviços OSGi{#osgi-services}
      + [Noções básicas do serviço OSGi](./developing/osgi-services/basics.md)
      + [Ciclo de vida do componente OSGi](./developing/osgi-services/lifecycle.md)
      + [Noções básicas sobre configurações do OSGi](./developing/osgi-services/configurations.md)
      + [Configurações OSGi usando OCD](./developing/osgi-services/configurations-ocd.md)
   + Avançado {#advanced}
      + [Usuários do Serviço](./developing/advanced/service-users.md)
   + [JavaDocs da API do SDK AEM](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ Depuração de AEM{#debugging}
   + Depuração do SDK do AEM{#debugging-aem-sdk}
      + [Visão geral](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Logs](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Depuração remota](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Console da Web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Ferramentas do Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Outras ferramentas](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Depuração AEM as a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Visão geral](./debugging/cloud-service/overview.md)
      + [Logs](./debugging/cloud-service/logs.md)
      + [Criar e implantar](./debugging/cloud-service/build-and-deployment.md)
      + [Console do desenvolvedor](./debugging/cloud-service/developer-console.md)
      + [CRXDE Lite](./debugging/cloud-service/crxde-lite.md)
+ Acessar AEM{#accessing}
   + [Visão geral](./accessing/overview.md)
   + [Usuários do Adobe IMS](./accessing/adobe-ims-users.md)
   + [Grupos de usuários do Adobe IMS](./accessing/adobe-ims-user-groups.md)
   + [Perfis de produto do Adobe IMS](./accessing/adobe-ims-product-profiles.md)
   + [AEM usuários, grupos e permissões](./accessing/aem-users-groups-and-permissions.md)
   + [Configuração do acesso ao AEM](./accessing/walk-through.md)
+ Rede avançada{#networking}
   + [Visão geral](./networking/advanced-networking.md)
   + [Saída flexível da porta](./networking/flexible-port-egress.md)
   + [Endereço IP de saída dedicado](./networking/dedicated-egress-ip-address.md)
   + [Rede privada virtual](./networking/vpn.md)
   + Exemplos de código{#examples}
      + [HTTP/HTTPS em portas não padrão para saída de porta flexível](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [HTTP/HTTPS em portas não padrão para endereço IP de saída dedicado/VPN](./networking/examples/http-on-non-standard-ports.md)
      + [Conexões SQL usando DataSourcePool](./networking/examples/sql-datasourcepool.md)
      + [Conexões SQL usando APIs Java SQL](./networking/examples/sql-java-apis.md)
      + [Serviço de email](./networking/examples/email-service.md)
+ Migração {#migration}
   + [Ferramenta Transferência de conteúdo](./migration/content-transfer-tool.md)
   + [Importação em massa de ativos](./migration/bulk-import.md)

   + Migração para o AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [Introdução](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [Integração](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA e CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [Ferramentas de modernização AEM](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [Modernização do Repositório](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [Microsserviços Asset compute](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [Pesquisa e indexação](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + Migração de conteúdo {#content-migration}
         + [Serviço de importação em massa](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [Ferramenta Transferência de conteúdo](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
      + [Resolução de problemas](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Forms as a Cloud Service {#aem-forms}
         + [Introdução](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [Inscrição digital](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [Comunicações](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [Introdução](./migration/cloud-acceleration-manager/introduction.md)
      + [Analisador de Práticas recomendadas e Práticas Recomendadas](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [Fase de implementação](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [Ferramenta Transferência de conteúdo](./migration/cloud-acceleration-manager/content-transfer-tool.md)
      + [Ferramentas de refatoração de código](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [Modernizador do Repositório de Código](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Conversor do Dispatcher Converter](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [Conversor de índice](./migration/cloud-acceleration-manager/index-converter.md)
      + [Ferramenta Migração de fluxo de trabalho de ativos](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [Navegação pelo Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [Uso do Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ Forms{#forms}

   + Desenvolvimento para o Forms as a Cloud Service{#developing-for-cloud-service}
      + [Introdução](./forms/developing-for-cloud-service/getting-started.md)
      + [Instalar o IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [Configurar Git](./forms/developing-for-cloud-service/setup-git.md)
      + [Sincronizar o IntelliJ com o AEM](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [Criar um formulário](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [Incluir Cloud Services e FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [Encaminhar para o Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [Implantar no ambiente de desenvolvimento](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [Atualização do arquétipo maven](./forms/developing-for-cloud-service/updating-project-archetype.md)
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
   + Geração de documento no AEM Forms CS{#doc-gen-formscs}
      + [Introdução](./forms/doc-gen-forms-cs/introduction.md)
      + [Criar Credenciais de Serviço](./forms/doc-gen-forms-cs/service-credentials.md)
      + [Criar token JWT](./forms/doc-gen-forms-cs/create-jwt.md)
      + [Criar Token de Acesso](./forms/doc-gen-forms-cs/create-access-token.md)
      + [Mesclar dados com modelo](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [Testar a solução](./forms/doc-gen-forms-cs/test.md)
      + [Desafio](./forms/doc-gen-forms-cs/challenge.md)
   + Geração de documento usando API em lote{#formscs-batch-api}
      + [Introdução](./forms/formscs-batch-api/introduction.md)
      + [Configurar o Armazenamento do Azure](./forms/formscs-batch-api/configure-azure-storage.md)
      + [Criar configuração em lote do USC](./forms/formscs-batch-api/configure-usc-batch.md)
      + [Criar configuração em lote](./forms/formscs-batch-api/create-batch-config.md)
      + [Executar lote](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Armazenamento do Portal do Azure{#forms-cs-azure-portal}
      + [Introdução](./forms/forms-cs-azure-portal/introduction.md)
      + [Criar modelo de dados do formulário](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Armazenar dados de formulário no Armazenamento do Azure](./forms/forms-cs-azure-portal/create-af.md)
      + [Preencher formulário](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [Envio de consultas](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + Criar fluxo de trabalho de revisão{#create-aem-workflow}
      + [Exteriorização do armazenamento de workflow](./forms/create-aem-workflow/externalize-workflow.md)
      + [Criar modelo de fluxo de trabalho](./forms/create-aem-workflow/create-workflow.md)
      + [Acionar fluxo de trabalho](./forms/create-aem-workflow/configure-af.md)
   + Adobe Sign com AEM Forms{#forms-and-sign}
      + [Introdução](./forms/forms-and-sign/introduction.md)
      + [Aplicativo de API do Adobe Sign](./forms/forms-and-sign/create-sign-api-application.md)
      + [Configuração de nuvem do Adobe Sign](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [Criar formulário adaptável](./forms/forms-and-sign/create-adaptive-form.md)
      + [Configurar para preenchimento e sinal](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + Integração com o Microsoft Dynamics{#formscs-dynamics-crm}
      + [Criar aplicativo dinâmico](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [Configurar fonte de dados](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [Criar modelo de dados do formulário](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [Criar formulário adaptável](./forms/formscs-dynamics-crm/create-adaptive-form.md)
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
   + [Resolução de problemas](./asset-compute/troubleshooting.md)
+ Cloud 5{#cloud-5}
   + [Introdução](./cloud-5/cloud5-introduction.md)
   + [Temporada 1](./cloud-5/cloud5-season-1.md)
   + [AEM CDN Parte 1](./cloud-5/cloud5-aem-cdn-part1.md)
+ [Série AEM especialistas](./aem-experts-series.md)
+ Tutorials de várias etapas{#multi-step-tutorials}
   + [Desenvolvimento do AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=pt-BR)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [Editor de SPA (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [Editor de SPA (Angular)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [AEM Sites e Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [Autenticação por token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
