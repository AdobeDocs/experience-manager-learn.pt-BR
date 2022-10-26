---
title: Analise o módulo ui.frontend do projeto de pilha completa
description: Revise o desenvolvimento de front-end, a implantação e o ciclo de vida do delivery de um projeto AEM Sites de pilha completa baseado em Maven.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 4%

---


# Revise o módulo &quot;ui.frontend&quot; do projeto de pilha completa AEM {#aem-full-stack-ui-frontent}

No, este capítulo analisa o desenvolvimento, a implantação e a entrega de artefatos front-end de um projeto de AEM de pilha completa, concentrando-se no módulo &quot;ui.frontend&quot; da __Projeto WKND Sites__.


## Objetivos {#objective}

* Entenda o fluxo de criação e implantação de artefatos front-end em um projeto de pilha completa AEM
* Revise o AEM do projeto de pilha completa `ui.frontend` do módulo [webpack](https://webpack.js.org/) configurações
* AEM processo de geração da biblioteca do cliente (também conhecida como clientlibs)

## Fluxo de implantação front-end para projetos de pilha completa e criação rápida de sites AEM

>[!IMPORTANT]
>
>Este vídeo explica e demonstra o fluxo de front-end para ambos **Instalação completa e criação rápida de site** projetos para destacar a sutil diferença no modelo de disponibilização, implantação e criação de recursos de front-end.

>[!VIDEO](https://video.tv.adobe.com/v/3409344/)

## Pré-requisitos {#prerequisites}


* Clonar o [AEM projeto de Sites WKND](https://github.com/adobe/aem-guides-wknd)
* Criado e implantado o projeto AEM sites WKND clonados para AEM as a Cloud Service.

Consulte o projeto do site AEM WKND [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) para obter mais detalhes.

## Fluxo de artefato front-end do projeto de pilha completa AEM {#flow-of-frontend-artifacts}

Abaixo está uma representação de alto nível da variável __desenvolvimento, implantação e delivery__ fluxo dos artefatos front-end em um projeto de AEM de pilha completa.

![Desenvolvimento, implantação e entrega de artefatos de front-end](assets/Dev-Deploy-Delivery-AEM-Project.png)


Durante a fase de desenvolvimento, as alterações de front-end como estilo e rebranding são realizadas ao atualizar os arquivos CSS e JS da `ui.frontend/src/main/webpack` pasta. Em seguida, durante o tempo de criação, a variável [webpack](https://webpack.js.org/) module-bundler e maven plugin tornam esses arquivos em clientlibs AEM otimizadas no `ui.apps` módulo.

As alterações de front-end são implantadas em AEM ambiente as a Cloud Service ao executar o [__Pilha completa__ pipeline no Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html).

Os recursos de front-end são fornecidos aos navegadores da Web por caminhos de URI que começam com `/etc.clientlibs/`e normalmente são armazenadas em cache AEM Dispatcher e CDN.


>[!NOTE]
>
> Da mesma forma, no __AEM Jornada de criação rápida de site__, o [alterações de front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html) são implantados em AEM ambiente as a Cloud Service ao executar o __Front-End__ pipeline, consulte [Configurar o pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### Revisar configurações do webpack no projeto WKND Sites {#development-frontend-webpack-clientlib}

* Há três __webpack__ arquivos de configuração usados para agrupar os recursos de front-end dos sites WKND.

   1. `webpack.common` - Contém o __frequentes__ configuração para instruir o agrupamento e a otimização de recursos WKND. O __output__ informa onde emitir os arquivos consolidados (também conhecidos como pacotes JavaScript, mas não para ser confundido com pacotes OSGi AEM) que cria. O nome padrão está definido como `clientlib-site/js/[name].bundle.js`.

   ```javascript
       ...
       output: {
               filename: 'clientlib-site/js/[name].bundle.js',
               path: path.resolve(__dirname, 'dist')
           }
       ...    
   ```

   1. `webpack.dev.js` contém a variável __desenvolvimento__ configuração do webpack-dev-serve e aponta para o modelo HTML a ser usado. Também contém uma configuração de proxy para uma instância do AEM em execução em `localhost:4502`.

   ```javascript
       ...
       devServer: {
           proxy: [{
               context: ['/content', '/etc.clientlibs', '/libs'],
               target: 'http://localhost:4502',
           }],
       ...    
   ```

   1. `webpack.prod.js` contém a variável __produção__ e usa os plug-ins para transformar os arquivos de desenvolvimento em pacotes otimizados.

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


* Os recursos agrupados são movidos para a variável `ui.apps` módulo usando [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) usando a configuração gerenciada na `clientlib.config.js` arquivo.

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

* O __frontend-maven-plugin__ from `ui.frontend/pom.xml` orquestra o agrupamento de webpack e a geração clientlib durante AEM criação do projeto.

`$ mvn clean install -PautoInstallSinglePackage`

### Implantação para AEM as a Cloud Service {#deployment-frontend-aemaacs}

O [__Pilha completa__ pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline) O implanta essas alterações em um ambiente AEM as a Cloud Service.


### Entrega AEM as a Cloud Service {#delivery-frontend-aemaacs}

Os recursos de front-end implantados por meio do pipeline de pilha completa são fornecidos do AEM Site para navegadores da Web como `/etc.clientlibs` arquivos. Você pode verificar isso acessando o [site WKND hospedado publicamente](https://wknd.site/content/wknd/us/en.html) e na fonte de visualização da página da Web.

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## Parabéns.  {#congratulations}

Parabéns, você revisou o módulo ui.frontend do projeto de pilha completa

## Próximas etapas {#next-steps}

No próximo capítulo, [Atualizar projeto para usar o pipeline front-end](update-project.md), você atualizará o AEM Projeto de sites da WKND para habilitá-lo para o contrato de pipeline de front-end.
