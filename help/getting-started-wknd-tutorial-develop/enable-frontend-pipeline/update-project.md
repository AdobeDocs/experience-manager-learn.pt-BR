---
title: Atualizar projeto AEM de pilha completa para usar pipeline de front-end
description: Saiba como atualizar o projeto de AEM de pilha completa para habilitá-lo para o pipeline de front-end, de modo que ele apenas crie e implante os artefatos de front-end.
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
exl-id: c4a961fb-e440-4f78-b40d-e8049078b3c0
duration: 340
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 0%

---

# Atualizar projeto AEM de pilha completa para usar pipeline de front-end {#update-project-enable-frontend-pipeline}

Neste capítulo, fazemos alterações de configuração no __Projeto do WKND Sites__ usar o pipeline de front-end para implantar JavaScript e CSS, em vez de exigir uma execução completa de pipeline de pilha completa. Isso dissocia o ciclo de vida de desenvolvimento e implantação dos artefatos de front-end e back-end, permitindo um processo de desenvolvimento mais rápido e iterativo em geral.

## Objetivos {#objectives}

* Atualizar projeto de pilha completa para usar o pipeline de front-end

## Visão geral das alterações de configuração no projeto AEM de pilha completa

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## Pré-requisitos {#prerequisites}

Este é um tutorial em várias partes e presume-se que você tenha revisado a [Módulo &quot;ui.frontend&quot;](./review-uifrontend-module.md).


## Alterações no projeto AEM de pilha completa

Há três alterações de configuração relacionadas ao projeto e uma alteração de estilo a ser implantada para uma execução de teste, ou seja, um total de quatro alterações específicas no projeto WKND para habilitá-lo para o contrato de pipeline de front-end.

1. Remova o `ui.frontend` módulo do ciclo de compilação de pilha completa

   * No, a raiz do projeto do WKND Sites `pom.xml` comente o `<module>ui.frontend</module>` entrada do submódulo.

   ```xml
       ...
       <modules>
       <module>all</module>
       <module>core</module>
       <!--
       <module>ui.frontend</module>
       -->                
       <module>ui.apps</module>
       ...
   ```

   * E comentar a dependência relacionada ao `ui.apps/pom.xml`

   ```xml
       ...
       <!-- ====================================================================== -->
       <!-- D E P E N D E N C I E S                                                -->
       <!-- ====================================================================== -->
           ...
       <!--
           <dependency>
               <groupId>com.adobe.aem.guides</groupId>
               <artifactId>aem-guides-wknd.ui.frontend</artifactId>
               <version>${project.version}</version>
               <type>zip</type>
           </dependency>
       -->    
       ...
   ```

1. Prepare o `ui.frontend` módulo para o contrato de pipeline de front-end adicionando dois novos arquivos de configuração de webpack.

   * Copiar o existente `webpack.common.js` as `webpack.theme.common.js`e alterar `output` propriedade e `MiniCssExtractPlugin`, `CopyWebpackPlugin` parâmetros de configuração do plug-in conforme abaixo:

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'theme/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * Copiar o existente `webpack.prod.js` as `webpack.theme.prod.js`e altere o `common` local da variável para o arquivo acima como

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >As duas alterações de configuração de &quot;webpack&quot; acima devem ter nomes diferentes de arquivos de saída e pastas, para que possamos diferenciar facilmente entre artefatos de front-end de pipeline de clientlib (pilha completa) e de tema gerado (front-end).
   >
   >Como você imaginou, as alterações acima podem ser ignoradas para usar também as configurações de webpack existentes, mas as alterações abaixo são necessárias.
   >
   >Depende de você como quer nomeá-los ou organizá-los.


   * No `package.json` arquivo, verifique se, a variável  `name` o valor da propriedade é igual ao nome do site do `/conf` nó. E sob o `scripts` propriedade, uma `build` script que instrui como criar os arquivos front-end a partir deste módulo.

   ```javascript
       {
       "name": "wknd",
       "version": "1.0.0",
       ...
   
       "scripts": {
           "build": "webpack --config ./webpack.theme.prod.js"
       }
   
       ...
       }
   ```

1. Prepare o `ui.content` para o pipeline de front-end, adicionando duas configurações do Sling.

   * Criar um arquivo em `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig` - isso inclui todos os arquivos front-end que o `ui.frontend` O módulo gera no `dist` pasta usando o processo de compilação do webpack.

   ```xml
   ...
       <css
       jcr:primaryType="nt:unstructured"
       element="link"
       location="header">
       <attributes
           jcr:primaryType="nt:unstructured">
           <as
               jcr:primaryType="nt:unstructured"
               name="as"
               value="style"/>
           <href
               jcr:primaryType="nt:unstructured"
               name="href"
               value="/theme/site.css"/>
   ...
   ```

   >[!TIP]
   >
   >    Veja a íntegra [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) no __Projeto AEM WKND Sites__.


   * Segundo o `com.adobe.aem.wcm.site.manager.config.SiteConfig` com o `themePackageName` sendo o mesmo que a variável `package.json` e `name` valor da propriedade e `siteTemplatePath` apontando para um `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` valor do caminho de stub.

   ```xml
   ...
       <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
               jcr:primaryType="nt:unstructured"
               siteTemplatePath="/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0"
               themePackageName="wknd">
       </jcr:root>
   ...
   ```

   >[!TIP]
   >
   >    Veja, a versão completa [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) no __Projeto AEM WKND Sites__.

1. Estamos alterando uma mudança de tema ou estilos para implantar via pipeline de front-end para uma execução de teste `text-color` para Adobe vermelho (ou você pode escolher o seu próprio), atualizando o `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

Por fim, envie essas alterações para o repositório Git do Adobe do seu programa.


>[!AVAILABILITY]
>
> Essas alterações estão disponíveis no GitHub dentro do [__pipeline de front-end__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) ramificação da __Projeto AEM WKND Sites__.


## Cuidado - _Ativar pipeline de front-end_ botão

A variável [Seletor de painéis](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) do [Site](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) mostra a variável **Ativar pipeline de front-end** botão ao selecionar a raiz ou a página do site. Clicando **Ativar pipeline de front-end** O botão substituirá o anterior **Configurações do Sling**, verifique se **você não clicar** Clique neste botão após implantar as alterações acima por meio da execução do pipeline do Cloud Manager.

![Botão Ativar pipeline de front-end](assets/enable-front-end-Pipeline-button.png)

Se for clicado por engano, é necessário executar novamente os pipelines para garantir que o contrato e as alterações do pipeline de front-end sejam restaurados.

## Parabéns. {#congratulations}

Parabéns, você atualizou o projeto WKND Sites para habilitá-lo para o contrato de pipeline de front-end.

## Próximas etapas {#next-steps}

No próximo capítulo, [Implantar usando o pipeline de front-end](create-frontend-pipeline.md), você criará e executará um pipeline de front-end e verificará como __mudou-se__ no delivery de recursos de front-end baseado em &#39;/etc.clientlibs&#39;.
