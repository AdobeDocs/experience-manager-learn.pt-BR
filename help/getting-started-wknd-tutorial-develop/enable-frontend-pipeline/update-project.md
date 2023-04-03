---
title: Atualizar projeto de AEM de pilha completa para usar o pipeline de front-end
description: Saiba como atualizar AEM projeto de pilha completa para habilitá-lo para o pipeline front-end, de modo que ele apenas crie e implante os artefatos front-end.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---


# Atualizar projeto de AEM de pilha completa para usar o pipeline de front-end {#update-project-enable-frontend-pipeline}

Neste capítulo, fazemos alterações de configuração no __Projeto WKND Sites__ para usar o pipeline front-end para implantar JavaScript e CSS, em vez de exigir uma execução completa de pipeline de pilha. Isso dissocia o desenvolvimento e o ciclo de vida da implantação de artefatos front-end e back-end, permitindo um processo de desenvolvimento mais rápido e iterativo em geral.

## Objetivos {#objectives}

* Atualizar projeto de pilha completa para usar o pipeline front-end

## Visão geral das alterações de configuração no projeto de AEM de pilha completa

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e presume-se que você tenha revisado o [Módulo &quot;ui.frontend&quot;](./review-uifrontend-module.md).


## Alterações no projeto de AEM de pilha completa

Há três alterações de configuração relacionadas ao projeto e uma alteração de estilo para implantar em uma execução de teste, portanto, no total quatro alterações específicas no projeto WKND para habilitá-lo para o contrato de pipeline front-end.

1. Remova o `ui.frontend` módulo do ciclo de compilação de pilha completa

   * Na, a raiz do Projeto de sites da WKND `pom.xml` comente o `<module>ui.frontend</module>` entrada do submódulo.

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

   * E dependência relacionada a comentários da `ui.apps/pom.xml`

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

1. Prepare o `ui.frontend` módulo para o contrato de pipeline front-end adicionando dois novos arquivos de configuração do webpack.

   * Copie as `webpack.common.js` as `webpack.theme.common.js`e alterar `output` propriedade e `MiniCssExtractPlugin`, `CopyWebpackPlugin` parâmetros de configuração do plug-in como abaixo:

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

   * Copie as `webpack.prod.js` as `webpack.theme.prod.js`e altere o `common` a localização da variável para o arquivo acima como

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >As duas alterações de configuração do &quot;webpack&quot; acima são para ter arquivos de saída e nomes de pastas diferentes, para que possamos diferenciar facilmente entre artefatos front-end do pipeline clientlib (Full-stack) e gerado por tema (front-end).
   >
   >Como você adivinhou, as alterações acima podem ser ignoradas para usar as configurações existentes do webpack também, mas as alterações abaixo são necessárias.
   >
   >Cabe a você nomear ou organizá-los.


   * No `package.json` , certifique-se de que o  `name` o valor da propriedade é igual ao nome do site da variável `/conf` nó . E sob o `scripts` propriedade, um `build` script que instrui como criar os arquivos front-end a partir desse módulo.

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

1. Prepare o `ui.content` para o pipeline front-end adicionando duas configurações do Sling.

   * Crie um arquivo em `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig` - inclui todos os arquivos front-end que a variável `ui.frontend` O módulo gera sob o `dist` pasta usando processo de build do webpack.

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
   >    Veja a conclusão [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) no __AEM projeto de Sites WKND__.


   * Segundo o `com.adobe.aem.wcm.site.manager.config.SiteConfig` com o `themePackageName` sendo o mesmo que `package.json` e `name` valor da propriedade e `siteTemplatePath` apontando para um `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` valor do caminho do stub.

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
   >    Veja, o [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) no __AEM projeto de Sites WKND__.

1. Um tema ou estilos são alterados para serem implantados por meio de pipeline front-end para uma execução de teste. `text-color` para Adobe vermelho (ou você pode escolher o seu), atualizando o `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

Por fim, envie essas alterações para o repositório Adobe git do seu programa.


>[!AVAILABILITY]
>
> Essas alterações estão disponíveis no GitHub na seção [__pipeline front-end__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) ramo do __AEM projeto de Sites WKND__.


## Precaução - _Ativar pipeline de front-end_ botão

O [Seletor de painéis](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) &#39;s [Site](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) mostra a **Ativar pipeline de front-end** ao selecionar a raiz do site ou a página do site. Clicar **Ativar pipeline de front-end** O botão substituirá o acima **Configurações do Sling**, certifique-se de **você não clicar em** esse botão após implantar as alterações acima por meio da execução do pipeline do Cloud Manager.

![Botão Ativar pipeline de front-end](assets/enable-front-end-Pipeline-button.png)

Se for clicado por engano, é necessário executar novamente os pipelines para garantir que o contrato do pipeline front-end e as alterações sejam restaurados.

## Parabéns! {#congratulations}

Parabéns, você atualizou o projeto WKND Sites para habilitá-lo para o contrato de pipeline front-end.

## Próximas etapas {#next-steps}

No próximo capítulo, [Implantar usando o pipeline front-end](create-frontend-pipeline.md), você criará e executará um pipeline front-end e verificará como nós __afastado__ na entrega de recursos front-end baseados em &quot;/etc.clientlibs&quot;.
