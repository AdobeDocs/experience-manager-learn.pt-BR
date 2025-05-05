---
title: Revisar módulo ui.frontend do projeto de pilha completa
description: Revise o desenvolvimento front-end, a implantação e o ciclo de vida de entrega de um projeto AEM Sites de pilha completa baseado em Maven.
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
exl-id: 65e8d41e-002a-4d80-a050-5366e9ebbdea
duration: 364
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# Revise o módulo &quot;ui.frontend&quot; do projeto de pilha completa do AEM {#aem-full-stack-ui-frontent}

No, este capítulo analisamos o desenvolvimento, a implantação e a entrega de artefatos de front-end de um projeto AEM de pilha completa, com foco no módulo &quot;ui.frontend&quot; do __projeto WKND Sites__.


## Objetivos {#objective}

* Entender o fluxo de compilação e implantação de artefatos de front-end em um projeto de pilha completa do AEM
* Revise as configurações do [webpack](https://webpack.js.org/) do módulo `ui.frontend` do projeto de pilha completa do AEM
* Processo de geração de bibliotecas de clientes do AEM (também conhecido como clientlibs)

## Fluxo de implantação front-end para projetos de pilha completa e Criação rápida de sites do AEM

>[!IMPORTANT]
>
>Este vídeo explica e demonstra o fluxo de front-end para projetos de **Pilha completa e Criação rápida de sites** para destacar a diferença sutil no modelo de compilação, implantação e entrega de recursos de front-end.

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## Pré-requisitos {#prerequisites}


* Clonar o [projeto do AEM WKND Sites](https://github.com/adobe/aem-guides-wknd)
* Criou e implantou o projeto clonado do AEM WKND Sites no AEM as a Cloud Service.

Consulte o projeto do site WKND do AEM [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) para obter mais detalhes.

## Fluxo de artefatos de front-end do projeto de pilha completa do AEM {#flow-of-frontend-artifacts}

Abaixo está uma representação de alto nível do __fluxo de desenvolvimento, implantação e entrega__ dos artefatos de front-end em um projeto de pilha completa do AEM.

![Desenvolvimento, Implantação e Entrega de Artefatos de Front-End](assets/Dev-Deploy-Delivery-AEM-Project.png)


Durante a fase de desenvolvimento, alterações de front-end, como estilo e rebranding, são realizadas atualizando os arquivos CSS, JS da pasta `ui.frontend/src/main/webpack`. Em seguida, durante o tempo de compilação, o pacote de módulos [webpack](https://webpack.js.org/) e o plug-in maven transformam esses arquivos em clientlibs otimizadas do AEM no módulo `ui.apps`.

As alterações de front-end são implantadas no ambiente do AEM as a Cloud Service ao executar o pipeline [__de pilha completa__ no Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=pt-BR).

Os recursos de front-end são entregues aos navegadores da Web por meio de caminhos URI que começam com `/etc.clientlibs/` e normalmente são armazenados em cache no AEM Dispatcher e CDN.


>[!NOTE]
>
> Da mesma forma, na __Jornada de Criação Rápida de Sites do AEM__, as [alterações de front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html?lang=pt-BR) são implantadas no ambiente do AEM as a Cloud Service executando o pipeline de __Front-End__. Consulte [Configurar o Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html?lang=pt-BR)

### Revisar configurações do webpack no projeto Sites WKND {#development-frontend-webpack-clientlib}

* Há três arquivos de configuração do __webpack__ usados para agrupar os recursos de front-end de sites WKND.

   1. `webpack.common` - Contém a configuração __common__ para instruir o agrupamento e a otimização de recursos WKND. A propriedade __output__ informa onde emitir os arquivos consolidados (também conhecidos como pacotes JavaScript, mas não devem ser confundidos com pacotes OSGi do AEM) que ela cria. O nome padrão está definido como `clientlib-site/js/[name].bundle.js`.

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js` contém a configuração de __desenvolvimento__ para o webpack-dev-serve e aponta para o modelo de HTML a ser usado. Também contém uma configuração de proxy para uma instância do AEM em execução em `localhost:4502`.

  ```javascript
      ...
      devServer: {
          proxy: [{
              context: ['/content', '/etc.clientlibs', '/libs'],
              target: 'http://localhost:4502',
          }],
      ...    
  ```

   1. `webpack.prod.js` contém a configuração __produção__ e usa os plug-ins para transformar os arquivos de desenvolvimento em pacotes otimizados.

  ```javascript
      ...
      module.exports = merge(common, {
          mode: 'production',
          optimization: {
              minimize: true,
              minimizer: [
                  new TerserPlugin(),
                  new CssMinimizerPlugin({ ...})
          }
      ...    
  ```


* Os recursos agrupados são movidos para o módulo `ui.apps` usando o plug-in [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator), com a configuração gerenciada no arquivo `clientlib.config.js`.

```javascript
    ...
    const BUILD_DIR = path.join(__dirname, 'dist');
    const CLIENTLIB_DIR = path.join(
    __dirname,
    '..',
    'ui.apps',
    'src',
    'main',
    'content',
    'jcr_root',
    'apps',
    'wknd',
    'clientlibs'
    );
    ...
```

* O __frontend-maven-plugin__ de `ui.frontend/pom.xml` orquestra o agrupamento de webpack e a geração de clientlib durante a compilação do projeto do AEM.

`$ mvn clean install -PautoInstallSinglePackage`

### Implantação no AEM as a Cloud Service {#deployment-frontend-aemaacs}

O pipeline [__de pilha completa__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=pt-BR&#full-stack-pipeline) implanta essas alterações em um ambiente do AEM as a Cloud Service.


### Entrega do AEM as a Cloud Service {#delivery-frontend-aemaacs}

Os recursos de front-end implantados por meio do pipeline de pilha completa são entregues do site do AEM para navegadores da Web como arquivos `/etc.clientlibs`. Você pode verificar isso acessando o [site da WKND hospedado publicamente](https://wknd.site/content/wknd/us/en.html) e exibindo a fonte da página da Web.

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## Parabéns. {#congratulations}

Parabéns, você revisou o módulo ui.frontend do projeto de pilha completa

## Próximas etapas {#next-steps}

No próximo capítulo, [Atualizar projeto para usar o pipeline de front-end](update-project.md), você atualizará o Projeto de Sites WKND do AEM para habilitá-lo para o contrato de pipeline de front-end.
