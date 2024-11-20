---
user-guide-title: Tutorials do Adobe Experience Manager as a Cloud Service
user-guide-description: Uma coleção de tutoriais do Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutorials do AEM as a Cloud Service
solution: Experience Manager, Experience Manager as a Cloud Service
sub-product: Experience Manager as a Cloud Service
version: Cloud Service
team: TM
source-git-commit: 6b8a8dc5cdcddfa2d8572bfd195bc67906882f67
workflow-type: tm+mt
source-wordcount: '1335'
ht-degree: 15%

---


# Tutorials do Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Visão geral](./overview.md)
+ Testes de AEM {#aem-trials}
   + [Imagens](./aem-trials/images.md)
+ Listas de Reprodução{#playlists}
   + [Desenvolvimento do AEM](./playlists/development.md)
+ Introdução ao AEM as a Cloud Service{#introduction}
   + [O que é o AEM as a Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Arquitetura](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + Estratégia e liderança de pensamento{#strategy}
      + [Experience Manager - Modelos e arquétipos de governança e equipe](./introduction/experience-manager-governance-and-staffing-models.md)
      + [Como impulsionar a velocidade do conteúdo com o Adobe Experience Manager](./introduction/drive-content-velocity-for-sites.md)
+ Integrações Experience Cloud{#integrations}
   + [Integrações](./integrations/experience-cloud.md)
   + [Adobe Target](./integrations/target.md)
+ Tecnologia Subjacente {#underlying-technology}
   + [Arquitetura do AEM](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Repositório de conteúdo Java](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [Serviços de Autor e Publish](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Edge Delivery Services {#edge-delivery-services}
   + [Plug-in Sidekick do AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/edge-delivery-services/sidekick-plugin.html){target=_blank}
+ Cloud Manager {#cloud-manager}
   + [Programas](./cloud-manager/programs.md)
   + [Ambientes](./cloud-manager/environments.md)
   + [Uso de um repositório GitHub](./cloud-manager/byogithub.md)
   + [Pipeline de produção CI/CD](./cloud-manager/cicd-production-pipeline.md)
   + [Pipeline CI/CD de não produção](./cloud-manager/cicd-non-production-pipeline.md)
   + [Atividade](./cloud-manager/activity.md)
   + [Nomes de Domínio Personalizados](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names){target=_blank}
   + Operações de Desenvolvimento{#devops}
      + [Implantação de código](./cloud-manager/devops/deploy-code.md)
      + [Mesclar projetos](./cloud-manager/devops/merge-projects.md)
      + [Configurar pipelines](./cloud-manager/devops/configure-pipelines.md)
      + [Integração contínua](./cloud-manager/devops/continuous-integration.md)
      + [Analisar resultados do teste](./cloud-manager/devops/analyze-test-results.md)
      + [Configurações do Dispatcher](./cloud-manager/devops/dispatcher-configurations.md)
      + [Análise de log da CDN](./cloud-manager/devops/cdn-log-analysis.md)
+ Configuração do Ambiente de Desenvolvimento Local {#local-development-environment-set-up}
   + [Visão geral](./local-development-environment/overview.md)
   + [Ferramentas de desenvolvimento](./local-development-environment/development-tools.md)
   + [SDK AEM local](./local-development-environment/aem-runtime.md)
   + [Ferramentas locais do Dispatcher](./local-development-environment/dispatcher-tools.md)
+ Desenvolvendo{#developing}
   + Extensibilidade{#extensibility}
      + App Builder{#app-builder}
         + [Gerar token de acesso JWT](./developing/extensibility/app-builder/jwt-auth.md)
         + [Gerar token de acesso de servidor para servidor](./developing/extensibility/app-builder/server-to-server-auth.md)
         + [Verificação de webhook do Github](./developing/extensibility/app-builder/github-webhook-verification.md)
      + Extensibilidade da Interface do Usuário{#ui}
         + [Visão geral](./developing/extensibility/ui/overview.md)
         + [Projeto do Adobe Developer Console](./developing/extensibility/ui/adobe-developer-console-project.md)
         + [Inicializar aplicativo](./developing/extensibility/ui/app-initialization.md)
         + [Registrar extensão](./developing/extensibility/ui/extension-registration.md)
         + [Modal](./developing/extensibility/ui/modal.md)
         + [Ação do Adobe I/O Runtime](./developing/extensibility/ui/runtime-action.md)
         + [Verificar](./developing/extensibility/ui/verify.md)
         + [Implantar](./developing/extensibility/ui/deploy.md)
         + Fragmentos de conteúdo{#content-fragments}
            + [Visão geral](./developing/extensibility/ui/content-fragments/overview.md)
            + Exemplos{#examples}
               + [Geração de imagem de IA](./developing/extensibility/ui/content-fragments/examples/console-image-generation-and-image-upload.md)
               + [Atualização de propriedade em massa](./developing/extensibility/ui/content-fragments/examples/console-bulk-property-update.md)
               + [Colunas de Grade Personalizadas](./developing/extensibility/ui/content-fragments/examples/custom-grid-columns.md)
               + [Exportar como XML](./developing/extensibility/ui/content-fragments/examples/editor-export-to-xml.md)
               + [Botão da barra de ferramentas RTE](./developing/extensibility/ui/content-fragments/examples/editor-rte-toolbar.md)
               + [Widgets do RTE](./developing/extensibility/ui/content-fragments/examples/editor-rte-widget.md)
               + [Medalhas RTE](./developing/extensibility/ui/content-fragments/examples/editor-rte-badges.md)
               + [Campos personalizados](./developing/extensibility/ui/content-fragments/examples/editor-custom-field.md)
   + Conceitos básicos de desenvolvimento{#basics}
      + [SDK do AEM](./developing/basics/aem-sdk.md)
      + [Ambiente de desenvolvimento local](./developing/basics/local-development-environment.md)
      + [Arquétipo de projeto do AEM](./developing/basics/aem-project-archetype.md)
      + [Estrutura de projetos do AEM](./developing/basics/project-structure.md)
      + [Conteúdo mutável vs. imutável](./developing/basics/mutable-immutable.md)
      + [Pacote de estrutura do repositório](./developing/basics/repository-structure-package.md)
      + [Publicação de conteúdo](./developing/basics/content-publishing.md)
      + [Configurações do OSGi](./developing/basics/osgi-configurations.md)
      + [Migração da configuração do Dispatcher](./developing/basics/dispatcher-configuration.md)
   + Projetos AEM{#aem-projects}
      + [Projeto AEM Maven](./developing/projects/maven-project-structure.md)
      + [Limpando um projeto AEM Maven](./developing/projects/remove-samples.md)
   + Serviços OSGi{#osgi-services}
      + [Noções básicas do serviço OSGi](./developing/osgi-services/basics.md)
      + [Ciclo de vida do componente OSGi](./developing/osgi-services/lifecycle.md)
      + [Noções básicas de configurações do OSGi](./developing/osgi-services/configurations.md)
      + [Configurações do OSGi usando o OCD](./developing/osgi-services/configurations-ocd.md)
   + Avançado{#advanced}
      + [Armazenando Variantes de Página em Cache](./developing/advanced/variant-caching.md)
      + [Proteção CSRF](./developing/advanced/csrf-protection.md)
      + [Namespaces personalizados](./developing/advanced/custom-namespaces.md)
      + [Parametrizar modelos do Sling a partir do HTL](./developing/advanced/sling-model-parameters.md)
      + [Segredos](./developing/advanced/secrets.md)
      + [Usuários de serviço](./developing/advanced/service-users.md)
      + [APIs de imagem otimizadas para a Web](./developing/advanced/web-optimized-image-delivery-java-apis.md)
      + [Executar trabalho na instância líder no Autor AEM](./developing/advanced/run-job-on-leader-instance-in-aem-author.md)
   + Ambiente de desenvolvimento rápido{#rde}
      + [Visão geral](./developing/rde/overview.md)
      + [Como configurar](./developing/rde/how-to-setup.md)
      + [Como usar](./developing/rde/how-to-use.md)
      + [Ciclo de vida de desenvolvimento](./developing/rde/development-life-cycle.md)
   + Editor Universal{#universal-editor}
      + Edição do aplicativo React{#react-app-editing}
         + [Visão geral](./developing/universal-editor/react-app/overview.md)
         + [Configuração de desenvolvimento local](./developing/universal-editor/react-app/local-development-setup.md)
         + [Aplicativo React do instrumento](./developing/universal-editor/react-app/instrument-to-edit-content.md)
   + [JavaDocs da API do SDK do AEM](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html){target=_blank}
+ Depurando AEM{#debugging}
   + Depurando o SDK do AEM{#debugging-aem-sdk}
      + [Visão geral](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Logs](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Depuração remota](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Console da Web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Ferramentas do Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Outras ferramentas](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Depurando o AEM as a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Visão geral](./debugging/cloud-service/overview.md)
      + [Logs](./debugging/cloud-service/logs.md)
      + [Build e implantação](./debugging/cloud-service/build-and-deployment.md)
      + [Console do desenvolvedor](./debugging/cloud-service/developer-console.md)
      + [Navegador de repositório](./debugging/cloud-service/repository-browser.md)
      + Riscos{#risks}
         + [Avisos transversais](./debugging/cloud-service/risks/traversals.md)
+ APIs AEM{#aem-apis}
   + [Visão geral](./apis/overview.md)
   + [Chamar APIs AEM baseadas em OpenAPI](./apis/invoke-openapi-based-aem-apis.md)
+ Entrega de conteúdo{#content-delivery}
   + [Nome de domínio personalizado](./content-delivery/custom-domain-names.md)
   + [Nome de domínio personalizado com CDN gerenciado por Adobe](./content-delivery/custom-domain-name-with-adobe-managed-cdn.md)
   + [Nome de domínio personalizado com CDN do cliente](./content-delivery/custom-domain-names-with-customer-managed-cdn.md)
   + [Armazenamento em cache](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/overview){target=_blank}
   + [CDN de Adobe - além do cache](./content-delivery/adobe-cdn-beyond-caching.md)
   + [Páginas de erro personalizadas](./content-delivery/custom-error-pages.md)
   + [Redirecionamentos de URL](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html){target=_blank}
+ Armazenando em cache{#caching}
   + [Visão geral](./caching/overview.md)
   + [Serviço de Publish para AEM](./caching/publish.md)
   + [Serviço de Autor do AEM](./caching/author.md)
   + [Análise de Taxa de Acertos do Cache CDN](./caching/cdn-cache-hit-ratio-analysis.md)
   + Como{#how-to}
      + [Ativar armazenamento em cache](./caching/how-to/enable-caching.md)
      + [Desativar armazenamento em cache](./caching/how-to/disable-caching.md)
      + [Limpar cache](./caching/how-to/purge-cache.md)
+ Acessando AEM{#accessing}
   + [Visão geral](./accessing/overview.md)
   + [Usuários do Adobe IMS](./accessing/adobe-ims-users.md)
   + [Grupos de usuários do Adobe IMS](./accessing/adobe-ims-user-groups.md)
   + [Perfis de produtos do Adobe IMS](./accessing/adobe-ims-product-profiles.md)
   + [Usuários, grupos e permissões do AEM](./accessing/aem-users-groups-and-permissions.md)
   + [Configuração do acesso ao AEM](./accessing/walk-through.md)
+ Autenticação{#authentication}
   + [Visão geral](./authentication/authentication.md)
   + [SAML 2.0](./authentication/saml-2-0.md)
+ Rede avançada{#networking}
   + [Visão geral](./networking/advanced-networking.md)
   + [Saída de porta flexível](./networking/flexible-port-egress.md)
   + [Endereço IP de saída exclusivo](./networking/dedicated-egress-ip-address.md)
   + [Rede virtual privada](./networking/vpn.md)
   + Exemplos de código{#examples}
      + [HTTP/HTTPS em portas fora do padrão para saída de porta flexível](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [HTTP/HTTPS para endereço IP de saída dedicado/VPN](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [Conexões SQL usando DataSourcePool](./networking/examples/sql-datasourcepool.md)
      + [Conexões SQL usando APIs Java SQL](./networking/examples/sql-java-apis.md)
      + [Serviço de e-mail](./networking/examples/email-service.md)
+ Segurança {#security}
   + [Bloqueio de ataques de DoS/DDoS usando regras de filtro de tráfego](./security/blocking-dos-attack-using-traffic-filter-rules.md)
   + Regras de Filtro de Tráfego incluindo regras WAF{#traffic-filter-and-waf-rules}
      + [Visão geral](./security/traffic-filter-rules/overview.md)
      + [Como configurar](./security/traffic-filter-rules/how-to-setup.md)
      + [Exemplos e análise de resultados](./security/traffic-filter-rules/examples-and-analysis.md)
      + [Práticas recomendadas](./security/traffic-filter-rules/best-practices.md)
+ Evento AEM{#aem-eventing}
   + [Visão geral](./eventing/overview.md)
   + Exemplos{#examples}
      + [Webhook - Receber eventos de AEM](./eventing/examples/webhook.md)
      + [Registro em log - Carregar eventos AEM](./eventing/examples/journaling.md)
      + [Ação do Adobe I/O Runtime - Receber eventos AEM](./eventing/examples/runtime-action.md)
      + [Ação do Adobe I/O Runtime - Processar eventos AEM](./eventing/examples/event-processing-using-runtime-action.md)
+ Migração {#migration}
   + [Ferramenta Transferência de conteúdo](./migration/content-transfer-tool.md)
   + [Importação em massa de ativos](./migration/bulk-import.md)
   + Migração para o AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [Introdução](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [Integração](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA e CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [Ferramentas de Modernização do AEM](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [Modernização do repositório](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [Microsserviços do Asset Compute](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
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
      + [Analisador de práticas recomendadas e disponibilidade](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [Fase de implementação](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [Ferramentas de refatoração de código](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [Modernizador do repositório de código](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher Converter](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [Conversor de índice](./migration/cloud-acceleration-manager/index-converter.md)
      + [Ferramenta Migração de fluxo de trabalho de ativos](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [Navegação no Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [Uso do Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ [Fragmentos de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-learn/content-fragments-console/overview.html){target=_blank}
+ Forms{#forms}
   + Desenvolvimento do Forms as a Cloud Service{#developing-for-cloud-service}
      + [1 - Introdução](./forms/developing-for-cloud-service/getting-started.md)
      + [2 - Instalar IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [3 - Configuração do Git](./forms/developing-for-cloud-service/setup-git.md)
      + [4 - Sincronizar IntelliJ com AEM](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [5 - Criar um formulário](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [6 - Manipulador de envio personalizado](./forms/developing-for-cloud-service/custom-submit-to-servlet.md)
      + [7 - Registrar servlet usando tipo de recurso](./forms/developing-for-cloud-service/registering-servlet-using-resourcetype.md)
      + [8 - Ativar os componentes do portal do Forms](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [9 - Incluir Cloud Service e FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [10 - Configuração em nuvem com reconhecimento de contexto](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [11 - Encaminhar para o Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [12 - Implantar no ambiente de desenvolvimento](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [13 - Atualização do arquétipo maven](./forms/developing-for-cloud-service/updating-project-archetype.md)
   + Criar Formulário Adaptável{#create-first-af}
      + [Introdução](./forms/create-first-af/introduction.md)
      + [Criar tema](./forms/create-first-af/create-theme.md)
      + [Criar modelo](./forms/create-first-af/create-template.md)
      + [Criar fragmento](./forms/create-first-af/create-fragments.md)
      + [Criar formulário](./forms/create-first-af/create-af.md)
      + [Configurar painel raiz](./forms/create-first-af/configure-root-panel.md)
      + [Configurar painel de pessoas](./forms/create-first-af/configure-people-panel.md)
      + [Configurar painel de receita](./forms/create-first-af/configure-income-panel.md)
      + [Configurar o painel de ativos](./forms/create-first-af/configure-assets-panel.md)
      + [Configurar o painel inicial](./forms/create-first-af/configure-start-panel.md)
      + [Adicionar e configurar a barra de ferramentas](./forms/create-first-af/add-configure-toolbar.md)
   + Serviço de envio personalizado com formulário headless{#custom-submit-headless-forms}
      + [1 - Introdução](./forms/custom-submit-headless-forms/introduction.md)
      + [2 - Criar serviço de envio personalizado](./forms/custom-submit-headless-forms/custom-submit-service.md)
      + [3 - Exibir a resposta](./forms/custom-submit-headless-forms/handle-response-react-app.md)
   + Criar componente de bloco de endereço{#create-address-block}
      + [1 - Introdução](./forms/create-address-block-component/introduction.md)
      + [2 - Configuração](./forms/create-address-block-component/set-up.md)
      + [3 - Criar componente](./forms/create-address-block-component/creating-address-component.md)
      + [4 - Implantar componente](./forms/create-address-block-component/deploy-your-project.md)
   + Criar componente de imagem clicável{#clickable-image-component}
      + [1 - Introdução](./forms/clickable-image-component/introduction.md)
      + [2 - Criar componente](./forms/clickable-image-component/create-component.md)
      + [3 - Manipular evento de clique](./forms/clickable-image-component/handle-click-event.md)
   + AEM Forms e Analytics{#forms-and-analytics}
      + [Introdução](./forms/form-data-analytics/introduction.md)
      + [Criar elementos de dados](./forms/form-data-analytics/data-elements.md)
      + [Criar regras](./forms/form-data-analytics/rules.md)
      + [Testar a solução](./forms/form-data-analytics/test.md)
   + Criando Variações de Botão{#style-system}
      + [Introdução](./forms/style-system/introduction.md)
      + [Definir política](./forms/style-system/style-policy.md)
      + [Definir variações](./forms/style-system/create-variations.md)
      + [Testar variações](./forms/style-system/build.md)
   + Usando guias verticais{#using-vertical-tabs}
      + [1. Introdução](./forms/using-vertical-tabs/introduction.md)
      + [2. Criar formulário](./forms/using-vertical-tabs/create-af.md)
      + [3. Navegação](./forms/using-vertical-tabs/navigation.md)
      + [4. Adicionar ícones](./forms/using-vertical-tabs/icons.md)
   + Usando o serviço de saída e formulários {#forms-cs-output-and-forms-service}
      + [Gerar PDF](./forms/forms-cs-output-and-forms-service/outputservice.md)
   + Geração de documentos no AEM Forms CS{#doc-gen-formscs}
      + [Introdução](./forms/doc-gen-forms-cs/introduction.md)
      + [Criar credenciais de serviço](./forms/doc-gen-forms-cs/service-credentials.md)
      + [Criar token JWT](./forms/doc-gen-forms-cs/create-jwt.md)
      + [Criar token de acesso](./forms/doc-gen-forms-cs/create-access-token.md)
      + [Mesclar dados com modelo](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [Testar a solução](./forms/doc-gen-forms-cs/test.md)
      + [Desafio](./forms/doc-gen-forms-cs/challenge.md)
   + Usando a API DocAssurance{#doc-assurance-api}
      + [Exemplos de snippets de código](./forms/doc-assurance-api/using-doc-assurance-api.md)
   + Geração de documentos usando a API em lote {#formscs-batch-api}
      + [Introdução](./forms/formscs-batch-api/introduction.md)
      + [Configurar armazenamento do Azure](./forms/formscs-batch-api/configure-azure-storage.md)
      + [Criar configuração em lote do USC](./forms/formscs-batch-api/configure-usc-batch.md)
      + [Criar configuração em lote](./forms/formscs-batch-api/create-batch-config.md)
      + [Executar lote](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Manipulação de PDF no Forms CS{#forms-cs-assembler}
      + [Introdução](./forms/forms-cs-assembler/introduction.md)
      + [Criar credenciais de serviço](./forms/forms-cs-assembler/service-credentials.md)
      + [Criar token JWT](./forms/forms-cs-assembler/create-jwt.md)
      + [Criar token de acesso](./forms/forms-cs-assembler/create-access-token.md)
      + [Montar arquivos PDF](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [Utilitários PDF/A](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [Testar a solução](./forms/forms-cs-assembler/test.md)
      + [Desafio](./forms/forms-cs-assembler/challenge.md)
   + Integrar ao Marketo{#froms-cs-with-marketo}
      + [Introdução](./forms/forms-cs-with-marketo/part1.md)
      + [Criar Source de dados](./forms/forms-cs-with-marketo/part2.md)
      + [Criar modelo de dados do formulário](./forms/forms-cs-with-marketo/part3.md)
   + Armazenar Envios de Formulário com Marcas de Índice Blob{#store-submiited-data-with-metadata-tags}
      + [Introdução](./forms/store-submiited-data-with-metadata-tags/introduction.md)
      + [Estender componente do grupo de opções](./forms/store-submiited-data-with-metadata-tags/extend-choice-group-components.md)
      + [Criar configuração OSGi](./forms/store-submiited-data-with-metadata-tags/create-osgi-configuration.md)
      + [Criar tags de índice](./forms/store-submiited-data-with-metadata-tags/create-blob-index-tags.md)
      + [Criar envio personalizado](./forms/store-submiited-data-with-metadata-tags/create-custom-submit.md)
   + Preencher formulário baseado no componente principal{#prefill-core-component-based-form}
      + [Introdução](./forms/prefill-core-component-form/introduction.md)
      + [Serviço de preenchimento prévio de gravação](./forms/prefill-core-component-form/pre-fill-service.md)
      + [Testar a solução](./forms/prefill-core-component-form/test-solution.md)
   + Armazenamento do Portal do Azure{#forms-cs-azure-portal}
      + [Introdução](./forms/forms-cs-azure-portal/introduction.md)
      + [Criar modelo de dados do formulário](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Armazenar dados de formulário no Armazenamento do Azure](./forms/forms-cs-azure-portal/create-af.md)
      + [Preencher formulário previamente](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [Envios de consultas](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + Salvar e Retomar preenchimento de formulário{#prefill-azure-storage}
      + [1- Introdução](./forms/prefill-azure-storage/introduction.md)
      + [2- Criar componente de página](./forms/prefill-azure-storage/page-component.md)
      + [3- Criar modelo de formulário adaptável](./forms/prefill-azure-storage/associate-page-component.md)
      + [4- Criar integração do Armazenamento do Azure](./forms/prefill-azure-storage/create-fdm.md)
      + [5 - Criar integração do SendGrid](./forms/prefill-azure-storage/send-grid-fdm.md)
      + [6 - Criar o formulário adaptável](./forms/prefill-azure-storage/create-af.md)
      + [7 - Implantar os ativos de amostra](./forms/prefill-azure-storage/deploy-sample-assets.md)

   + Criar Fluxo de Trabalho de Revisão{#create-aem-workflow}
      + [Externalizar o armazenamento do fluxo de trabalho](./forms/create-aem-workflow/externalize-workflow.md)
      + [Criar modelo de fluxo de trabalho](./forms/create-aem-workflow/create-workflow.md)
      + [Acionar fluxo de trabalho](./forms/create-aem-workflow/configure-af.md)
   + Acrobat Sign com AEM Forms{#forms-and-sign}
      + [Introdução](./forms/forms-and-sign/introduction.md)
      + [Aplicativo de API do Acrobat Sign](./forms/forms-and-sign/create-sign-api-application.md)
      + [Configuração da nuvem do Acrobat Sign](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [Criar formulário adaptável](./forms/forms-and-sign/create-adaptive-form.md)
      + [Configurar para preenchimento e assinatura](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + Integração com o Microsoft Power Automate{#forms-cs-and-power-automate}
      + [Configurar a integração](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [Analisar dados de formulário enviados](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [Enviar DoR como um anexo de email](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [Extrair anexos de formulário de dados enviados](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + Integrar ao Microsoft Dynamics{#formscs-dynamics-crm}
      + [Criar Aplicativo do Dynamics](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [Configurar o Data Source](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [Criar modelo de dados do formulário](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [Criar formulário adaptável](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + Integrar ao Salesforce{#integrate-with-salesforce}
      + [Introdução](./forms/integrate-with-salesforce/introduction.md)
      + [Criar aplicativo conectado](./forms/integrate-with-salesforce/create-connected-app.md)
      + [Criar arquivo do Swagger](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [Criar fonte de dados](./forms/integrate-with-salesforce/create-data-source.md)
      + [Criar modelo de dados de formulário](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [Envio de formulário de teste](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [Evento de clique de teste](./forms/integrate-with-salesforce/create-lead-click-event.md)
   + Armazenar envios de formulários em uma unidade e no SharePoint{#one-drive}
      + [Armazenar dados de formulário em uma unidade](./forms/forms-cs-one-drive/store-form-submission-one-drive.md)
      + [Armazenar dados de formulário no sharepoint](./forms/forms-cs-sharepoint/store-form-submission-in-sharepoint.md)
      + [Preencher formulário com dados da lista do SharePoint](./forms/forms-cs-sharepoint/prefill-data-from-sharepoint-list.md)
      + [Inserir dados na lista do SharePoint usando o fluxo de trabalho](./forms/forms-cs-sharepoint/submit-data-sharepoint-list-workflow.md)
+ Extensibilidade de asset compute{#asset-compute}
   + [Visão geral](./asset-compute/overview.md)
   + Configurar{#set-up}
      + [Provisionamento de conta e serviço](./asset-compute/set-up/accounts-and-services.md)
      + [Ambiente de desenvolvimento local](./asset-compute/set-up/development-environment.md)
      + [App Builder](./asset-compute/set-up/app-builder.md)
   + Desenvolver{#develop}
      + [Criar um projeto do Asset Compute](./asset-compute/develop/project.md)
      + [Configurar variáveis de ambiente](./asset-compute/develop/environment-variables.md)
      + [Configurar o manifest.yml](./asset-compute/develop/manifest.md)
      + [Desenvolver um trabalhador](./asset-compute/develop/worker.md)
      + [Usar a Ferramenta de desenvolvimento](./asset-compute/develop/development-tool.md)
   + Teste e Depuração{#test-debug}
      + [Testar um trabalhador](./asset-compute/test-debug/test.md)
      + [Depurar um trabalhador](./asset-compute/test-debug/debug.md)
   + Implantar{#deploy}
      + [Implantar no Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [Integrar ao AEM](./asset-compute/deploy/processing-profiles.md)
   + Avançado{#advanced}
      + [Trabalhadores de metadados](./asset-compute/advanced/metadata.md)
   + [Resolução de problemas](./asset-compute/troubleshooting.md)

+ Tutorials{#multi-step-tutorials} em várias etapas
   + [Desenvolvimento do AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=pt-BR){target=_blank}
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=pt-BR){target=_blank}
   + [Editor de SPA (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html){target=_blank}
   + [AEM Sites e Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html){target=_blank}
   + [Autenticação baseada em token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html){target=_blank}
+ Recursos de especialistas {#expert-resources}
   + Campeões do AEM {#aem-champions}
      + [Manual de integração do Cloud Manager](./expert-resources/aem-champions/onboarding-playbook.md)
      + [Tipos de ambiente do Cloud Manager](./expert-resources/aem-champions/environment-types.md)
      + [Interface do usuário do Cloud Manager](./expert-resources/aem-champions/cloud-manager-ui.md)
   + [Série para especialistas em AEM](./expert-resources/expert-series/aem-experts-series.md)
   + Nuvem 5{#cloud-5}
      + [Introdução](./expert-resources/cloud-5/cloud5-introduction.md)
      + [Temporada 1](./expert-resources/cloud-5/cloud5-season-1.md)
      + [Temporada 2](./expert-resources/cloud-5/cloud5-season-2.md)
      + [Temporada 3](./expert-resources/cloud-5/cloud5-season-3.md)
      + [CDN AEM Parte 1](./expert-resources/cloud-5/cloud5-aem-cdn-part1.md)
      + [CDN AEM Parte 2](./expert-resources/cloud-5/cloud5-aem-cdn-part2.md)
      + [Arquivos de registro do AEM](./expert-resources/cloud-5/cloud5-aem-log-files.md)
      + [Tokens de logon](./expert-resources/cloud-5/cloud5-getting-login-token-integrations.md)
      + [Cloud Dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-cloud.md)
      + [Migração 1](./expert-resources/cloud-5/cloud5-aem-content-migration-part-1.md)
      + [Dispatcher Validator](./expert-resources/cloud-5/cloud5-aem-dispatcher-validator.md)
      + [Pesquisa e indexação](./expert-resources/cloud-5/cloud5-aem-search-and-indexing.md)
      + [Adobe App Builder](./expert-resources/cloud-5/cloud5-adobe-app-builder.md)
      + Temporada 2{#season-2}
         + [Fragmentos](./expert-resources/cloud-5/season-2/cloud5-experience-v-content-fragments.md)
         + [Modernizador do repositório](./expert-resources/cloud-5/season-2/cloud5-repo-modernizer.md)
         + [Admin Console](./expert-resources/cloud-5/season-2/cloud5-admin-console.md)
         + [REPOINIT](./expert-resources/cloud-5/season-2/cloud5-repoinit.md)
         + [Sling Job Scheduler](./expert-resources/cloud-5/season-2/cloud5-sling-job-scheduler.md)
         + [Corrigir o cache](./expert-resources/cloud-5/season-2/cloud5-fix-your-cache.md)
         + [Corrija suas substituições](./expert-resources/cloud-5/season-2/cloud5-fix-your-rewrites.md)
         + [Cloud Manager - Auditoria de experiência](./expert-resources/cloud-5/season-2/cloud5-mocm-experience-audit.md)
         + [Cloud Manager - Testes de unidade](./expert-resources/cloud-5/season-2/cloud5-mocm-unit-tests.md)
         + [Cloud Manager - Testes funcionais](./expert-resources/cloud-5/season-2/cloud5-mocm-functional-tests.md)
      + Temporada 3{#season-3}
         + [Pesquisa de terceiros](./expert-resources/cloud-5/season-3/cloud5-3rd-party-search.md)
         + [Monitoramento do usuário real (RUM)](./expert-resources/cloud-5/season-3/cloud5-rum.md)
         + [Trabalhadores da Edge](./expert-resources/cloud-5/season-3/cloud5-edge-workers.md)
         + [Publish, cancelar publicação de eventos no Edge Delivery Services](./expert-resources/cloud-5/season-3/cloud5-publish-events.md)
         + [Índices de consulta e fórmulas do Excel](./expert-resources/cloud-5/season-3/cloud5-query-indexes.md)
         + [Traga sua própria CDN do Cloud Flare](./expert-resources/cloud-5/season-3/cloud5-byo-cloudflare-cdn.md)
         + [Integrar o AEM Assets](./expert-resources/cloud-5/season-3/cloud5-integrate-assets.md)
         + [IA gerativa para o AEM Sites](./expert-resources/cloud-5/season-3/cloud5-generative-ai-for-aem-sites.md)
         + [Explorar editor universal](./expert-resources/cloud-5/season-3/cloud5-exploring-universal-editor.md)
         + [Importar sites](./expert-resources/cloud-5/season-3/cloud5-import-sites-to-edge-delivery-services.md)
         + [Uso da API de administração](./expert-resources/cloud-5/season-3/cloud5-using-admin-api.md)
         + [Otimização de pontuação do farol - Parte1](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part1.md)
         + [Otimização de pontuação do farol - Parte2](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part2.md)
         + [Otimização de pontuação do farol - Parte3](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part3.md)

