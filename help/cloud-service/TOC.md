---
user-guide-title: Tutoriais do Adobe Experience Manager as a Cloud Service
user-guide-description: Uma coleção de tutoriais do Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutorials do AEM as a Cloud Service
sub-product: Experience Manager as a Cloud Service
version: Cloud Service
team: TM
source-git-commit: 9917b16248ef1f0a9c86f03a024c634636b2304e
workflow-type: tm+mt
source-wordcount: '913'
ht-degree: 20%

---


# Tutoriais do Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Visão geral](./overview.md)
+ Introdução ao AEM as a Cloud Service{#introduction}
   + [O que é AEM as a Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Evolução](./introduction/evolution.md)
   + [Arquitetura](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + Estratégia e liderança de pensamento{#strategy}
      + [Experience Manager - Governança e modelos e arquétipos de pessoal](./introduction/experience-manager-governance-and-staffing-models.md)
      + [Como aumentar a velocidade do conteúdo com o Adobe Experience Manager](./introduction/drive-content-velocity-for-sites.md)
      + [Acelere a velocidade do conteúdo com sistemas de estilo AEM](./introduction/accelerate-content-velocity-aem.md)
+ [Integrações da Experience Cloud](./experience-cloud/integrations.md)
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
   + Extensibilidade{#extensibility}
      + App Builder{#app-builder}
         + [Gerar token de acesso](./developing/extensibility/app-builder/jwt-auth.md)
      + Console do Fragmento de conteúdo{#content-fragments}
         + [Visão geral](./developing/extensibility/content-fragments/overview.md)
         + [Projeto do Adobe Developer Console](./developing/extensibility/content-fragments/adobe-developer-console-project.md)
         + [Inicialização do aplicativo](./developing/extensibility/content-fragments/app-initialization.md)
         + [Registro de extensão](./developing/extensibility/content-fragments/extension-registration.md)
         + [Menu Cabeçalho](./developing/extensibility/content-fragments/header-menu.md)
         + [Barra de ação](./developing/extensibility/content-fragments/action-bar.md)
         + [Modal](./developing/extensibility/content-fragments/modal.md)
         + [Ação do Adobe I/O Runtime](./developing/extensibility/content-fragments/runtime-action.md)
         + [Testar](./developing/extensibility/content-fragments/test.md)
         + [Implantar](./developing/extensibility/content-fragments/deploy.md)
         + Exemplo de extensões{#example-extensions}
            + [Atualização de propriedade em massa](./developing/extensibility/content-fragments/example-extensions/bulk-property-update.md)
            + [AEM geração de ativos de imagem usando o OpenAI](./developing/extensibility/content-fragments/example-extensions/image-generation-and-image-upload.md)
   + Conceitos básicos de desenvolvimento{#basics}
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
      + [APIs de imagem otimizadas para a Web](./developing/advanced/web-optimized-image-delivery-java-apis.md)
      + [Usuários do Serviço](./developing/advanced/service-users.md)
      + [Namespaces personalizados](./developing/advanced/custom-namespaces.md)
      + [Armazenamento em cache de variáveis de página](./developing/advanced/variant-caching.md)
   + Ambiente de desenvolvimento rápido{#rde}
      + [Visão geral](./developing/rde/overview.md)
      + [Como configurar](./developing/rde/how-to-setup.md)
      + [Como usar](./developing/rde/how-to-use.md)
      + [Ciclo de vida do desenvolvimento](./developing/rde/development-life-cycle.md)
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
      + [Navegador de repositório](./debugging/cloud-service/repository-browser.md)
      + Riscos{#risks}
         + [Avisos transversais](./debugging/cloud-service/risks/traversals.md)
+ Entrega de conteúdo{#content-delivery}
   + [Redirecionamentos de URL](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html)
+ Acessar AEM{#accessing}
   + [Visão geral](./accessing/overview.md)
   + [Usuários do Adobe IMS](./accessing/adobe-ims-users.md)
   + [Grupos de usuários do Adobe IMS](./accessing/adobe-ims-user-groups.md)
   + [Perfis de produto do Adobe IMS](./accessing/adobe-ims-product-profiles.md)
   + [AEM usuários, grupos e permissões](./accessing/aem-users-groups-and-permissions.md)
   + [Configuração do acesso ao AEM](./accessing/walk-through.md)
+ Autenticação{#authentication}
   + [Visão geral](./authentication/authentication.md)
   + [SAML 2.0](./authentication/saml-2-0.md)
+ Rede avançada{#networking}
   + [Visão geral](./networking/advanced-networking.md)
   + [Saída flexível da porta](./networking/flexible-port-egress.md)
   + [Endereço IP de saída dedicado](./networking/dedicated-egress-ip-address.md)
   + [Rede privada virtual](./networking/vpn.md)
   + Exemplos de código{#examples}
      + [HTTP/HTTPS em portas não padrão para saída de porta flexível](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [HTTP/HTTPS para endereço IP de saída dedicado/VPN](./networking/examples/http-dedicated-egress-ip-vpn.md)
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
         + [Perguntas frequentes](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
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
      + [Ativar componentes do Forms Portal](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [Incluir Cloud Services e FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [Configuração da nuvem sensível ao contexto](./forms/developing-for-cloud-service/context-aware-fdm.md)
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
   + AEM Forms e Analytics{#forms-and-analytics}
      + [Introdução](./forms/form-data-analytics/introduction.md)
      + [Criar elementos de dados](./forms/form-data-analytics/data-elements.md)
      + [Criar regras](./forms/form-data-analytics/rules.md)
      + [Testar a solução](./forms/form-data-analytics/test.md)
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
   + Manipulação de PDF no Forms CS{#forms-cs-assembler}
      + [Introdução](./forms/forms-cs-assembler/introduction.md)
      + [Criar Credenciais de Serviço](./forms/forms-cs-assembler/service-credentials.md)
      + [Criar token JWT](./forms/forms-cs-assembler/create-jwt.md)
      + [Criar Token de Acesso](./forms/forms-cs-assembler/create-access-token.md)
      + [Compilar arquivos PDF](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [Utilitários PDF/A](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [Testar a solução](./forms/forms-cs-assembler/test.md)
      + [Desafio](./forms/forms-cs-assembler/challenge.md)
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
   + Acrobat Sign com AEM Forms{#forms-and-sign}
      + [Introdução](./forms/forms-and-sign/introduction.md)
      + [Aplicativo de API do Acrobat Sign](./forms/forms-and-sign/create-sign-api-application.md)
      + [Configuração da Acrobat Sign Cloud](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [Criar formulário adaptável](./forms/forms-and-sign/create-adaptive-form.md)
      + [Configurar para preenchimento e sinal](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + Integração com o Microsoft Power Automate{#forms-cs-and-power-automate}
      + [Configurar a integração](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [Analisar dados de formulário enviados](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [Enviar DoR como anexo de email](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [Extrair anexos de formulário de dados enviados](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
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
      + [App Builder](./asset-compute/set-up/app-builder.md)
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
   + [Temporada 2](./cloud-5/cloud5-season-2.md)
   + [AEM CDN Parte 1](./cloud-5/cloud5-aem-cdn-part1.md)
   + [CDN AEM Parte 2](./cloud-5/cloud5-aem-cdn-part2.md)
   + [Arquivos de registro de AEM](./cloud-5/cloud5-aem-log-files.md)
   + [Tokens de logon](./cloud-5/cloud5-getting-login-token-integrations.md)
   + [Dispatcher na nuvem](./cloud-5/cloud5-aem-dispatcher-cloud.md)
   + [Migração 1](./cloud-5/cloud5-aem-content-migration-part-1.md)
   + [Migração 2](./cloud-5/cloud5-aem-content-migration-part-2.md)
   + [Validador do Dispatcher](./cloud-5/cloud5-aem-dispatcher-validator.md)
   + [Pesquisa e indexação](./cloud-5/cloud5-aem-search-and-indexing.md)
   + [Adobe App Builder](./cloud-5/cloud5-adobe-app-builder.md)
   + Temporada 2{#season-2}
      + [Fragmentos](./cloud-5/season-2/cloud5-experience-v-content-fragments.md)
      + [Repo Modernizer](./cloud-5/season-2/cloud5-repo-modernizer.md)
      + [Admin Console](./cloud-5/season-2/cloud5-admin-console.md)
      + [REPOSITAR](./cloud-5/season-2/cloud5-repoinit.md)
      + [Agendador de Tarefas do Sling](./cloud-5/season-2/cloud5-sling-job-scheduler.md)
      + [Corrija seu cache](./cloud-5/season-2/cloud5-fix-your-cache.md)
      + [Corrigir suas regravações](./cloud-5/season-2/cloud5-fix-your-rewrites.md)
      + [Cloud Manager - Auditoria de experiência](./cloud-5/season-2/cloud5-mocm-experience-audit.md)
      + [Cloud Manager - Testes de unidade](./cloud-5/season-2/cloud5-mocm-unit-tests.md)
      + [Cloud Manager - Testes funcionais](./cloud-5/season-2/cloud5-mocm-functional-tests.md)
+ [Série AEM especialistas](./aem-experts-series.md)
+ Tutorials de várias etapas{#multi-step-tutorials}
   + [Desenvolvimento do AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=pt-BR)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=pt-BR)
   + [Editor de SPA (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [AEM Sites e Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [Autenticação por token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
