---
title: Atualizar projeto de pilha completa do AEM para usar pipeline de front-end
description: Saiba como atualizar o projeto de pilha completa do AEM para habilitá-lo para o pipeline de front-end, de modo que ele apenas crie e implante os artefatos de front-end.
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
exl-id: c4a961fb-e440-4f78-b40d-e8049078b3c0
duration: 307
source-git-commit: b395b3b84e63fe6c24e597d1628f4aed5ba47469
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 0%

---

# Atualizar projeto de pilha completa do AEM para usar pipeline de front-end {#update-project-enable-frontend-pipeline}

Neste capítulo, fazemos alterações de configuração no __projeto do WKND Sites__ para usar o pipeline de front-end para implantar JavaScript e CSS, em vez de exigir uma execução completa de pipeline de pilha completa. Isso dissocia o ciclo de vida de desenvolvimento e implantação dos artefatos de front-end e back-end, permitindo um processo de desenvolvimento mais rápido e iterativo em geral.

## Objetivos {#objectives}

* Atualizar projeto de pilha completa para usar o pipeline de front-end

## Visão geral das alterações de configuração no projeto de pilha completa do AEM

>[!VIDEO](https://video.tv.adobe.com/v/3453614?quality=12&learn=on&captions=por_br)

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e presume-se que você tenha revisado o [ Módulo&#39;ui.frontend&#39;](./review-uifrontend-module.md).


## Alterações no projeto de pilha completa do AEM

Há três alterações de configuração relacionadas ao projeto e uma alteração de estilo a ser implantada para uma execução de teste, ou seja, um total de quatro alterações específicas no projeto WKND para habilitá-lo para o contrato de pipeline de front-end.

1. Remover o módulo `ui.frontend` do ciclo de compilação de pilha completa

   * No, a raiz `pom.xml` do Projeto do Sites WKND comenta a entrada do submódulo `<module>ui.frontend</module>`.

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

   * E comentar a dependência relacionada do `ui.apps/pom.xml`

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

1. Prepare o módulo `ui.frontend` para o contrato de pipeline de front-end adicionando dois novos arquivos de configuração de webpack.

   * Copie o `webpack.common.js` existente como `webpack.theme.common.js` e altere a propriedade `output` e os parâmetros de configuração de plug-in `MiniCssExtractPlugin`, `CopyWebpackPlugin` conforme abaixo:

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
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './theme' }
           ]
       })
   ...
   ```

   * Copie o `webpack.prod.js` existente como `webpack.theme.prod.js` e altere o local da variável `common` para o arquivo acima como

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


   * No arquivo `package.json`, verifique se o valor da propriedade `name` é igual ao nome do site do nó `/conf`. E na propriedade `scripts`, um script `build` instruindo como criar os arquivos front-end a partir deste módulo.

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

1. Prepare o módulo `ui.content` para o pipeline de front-end adicionando duas configurações do Sling.

   * Criar um arquivo em `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig` - isso inclui todos os arquivos de front-end que o módulo `ui.frontend` gera na pasta `dist` usando o processo de compilação do webpack.

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
   >    Consulte o [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) completo no __projeto do AEM WKND Sites__.


   * Segundo o `com.adobe.aem.wcm.site.manager.config.SiteConfig` com o valor `themePackageName` igual ao valor de propriedade `package.json` e `name` e `siteTemplatePath` apontando para um valor de caminho de stub `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0`.

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
   >    Consulte a [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) concluída no __projeto do AEM WKND Sites__.

1. Se um tema ou estilos forem alterados para implantação por meio do pipeline de front-end para uma execução de teste, estamos alterando `text-color` para vermelho do Adobe (ou você pode escolher o seu) ao atualizar o `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

Por fim, envie essas alterações para o repositório Git do Adobe do seu programa.


>[!AVAILABILITY]
>
> Estas alterações estão disponíveis no GitHub na ramificação [__front-end do pipeline__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) do __projeto do AEM WKND Sites__.


## Cuidado - _Botão Habilitar pipeline de front-end_

A opção [Site](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html?lang=pt-BR) do [Seletor de Painel](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html?lang=pt-BR) mostra o botão **Habilitar Pipeline de Front-End** ao selecionar a raiz ou a página do site. Clicar no botão **Habilitar Pipeline de Front-End** substituirá as **configurações do Sling** acima. Verifique se **você não clicou** nesse botão depois de implantar as alterações acima por meio da execução do pipeline do Cloud Manager.

![Botão Habilitar pipeline de front-end](assets/enable-front-end-Pipeline-button.png)

Se for clicado por engano, é necessário executar novamente os pipelines para garantir que o contrato e as alterações do pipeline de front-end sejam restaurados.

## Parabéns. {#congratulations}

Parabéns, você atualizou o projeto WKND Sites para habilitá-lo para o contrato de pipeline de front-end.

## Próximas etapas {#next-steps}

No próximo capítulo, [Implantar usando o Pipeline de Front-End](create-frontend-pipeline.md), você criará e executará um pipeline de front-end e verificará como nós __saímos__ da entrega de recursos de front-end baseada em &#39;/etc.clientlibs&#39;.
