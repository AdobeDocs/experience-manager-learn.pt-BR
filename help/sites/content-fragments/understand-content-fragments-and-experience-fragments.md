---
title: Fragmentos de conteúdo e Fragmentos de experiência
description: Saiba mais sobre as semelhanças e diferenças entre os Fragmentos de conteúdo e Fragmentos de experiência, e quando e como usar cada tipo.
sub-product: Experience Manager Assets, Experience Manager Sites
feature: Content Fragments, Experience Fragments
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Article
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
duration: 168
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 2%

---

# Fragmentos de conteúdo e Fragmentos de experiência

Os fragmentos de conteúdo e fragmentos de experiência do Adobe Experience Manager podem parecer semelhantes na superfície, mas cada um desempenha funções principais em casos de uso diferentes. Saiba como os Fragmentos de conteúdo e os Fragmentos de experiência são semelhantes, diferentes e quando e como usar cada um deles.

## Comparação

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Fragmentos de conteúdo (CF)</strong></td>
<td><strong>Fragmentos de experiência (XF)</strong></td>
</tr><tr><td><strong>Definição</strong></td>
<td><ul>
<li><strong>conteúdo</strong> reutilizável e independente de apresentação, composto de elementos de dados estruturados (texto, datas, referências, etc.)</li>
</ul>
</td>
<td><ul>
<li>Um componente reutilizável, composto de um ou mais Componentes do AEM que definem o conteúdo e a apresentação que forma uma <strong>experiência</strong> que faz sentido por si só</li>
</ul>
</td>
</tr><tr><td><strong>Princípios principais</strong></td>
<td><ul>
<li>Centrado no conteúdo</li>
<li>Definido por um <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=pt-BR" target="_blank">modelo de dados estruturado, baseado em formulário.</a></li>
<li>Design e layout independentes.</li>
<li>O canal é responsável pela apresentação do conteúdo do fragmento de conteúdo (layout e design)</li>
</ul>
</td>
<td><ul>
<li>Centrado na apresentação</li>
<li>Definido pela composição não estruturada de componentes do AEM</li>
<li>Define o design e o layout do conteúdo</li>
<li>Usado "como está" em canais</li>
</ul>
</td>
</tr><tr><td><strong>Detalhes técnicos</strong></td>
<td><ul>
<li>Implementado como um <strong>dam:Asset</strong></li>
<li>Definido por um <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=pt-BR" target="_blank">Modelo de fragmento de conteúdo</a></li>
</ul>
</td>
<td><ul>
<li>Implementado como uma <strong>cq:Page</strong></li>
<li>Definido por Modelos editáveis</li>
<li>Representação nativa do HTML</li>
</ul>
</td>
</tr><tr><td><strong>Variações</strong></td>
<td><ul>
<li>A variação principal é a variação canônica</li>
<li>As variações são específicas de caso de uso, que podem se alinhar a canais.</li>
</ul>
</td>
<td><ul>
<li>As variações são específicas de canal ou contexto</li>
<li>As variações são mantidas em sincronia por meio da AEM Live Copy</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=pt-BR" target="_blank">Os blocos de construção</a> permitem a reutilização de conteúdo em variações</li>
</ul>
</td>
</tr><tr><td><strong>Recursos</strong></td>
<td><ul>
<li>Variações</li>
<li>Versões</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=pt-BR#synchronizing-with-master" target="_blank">Sincronização</a> de conteúdo entre variações</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=pt-BR#comparing-fragment-versions" target="_blank">Diferença visual</a> de versões de Fragmento de conteúdo</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=pt-BR#annotating-a-content-fragment" target="_blank">Anotações</a> de elementos de texto multilinha</li>
<li>Resumo <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=pt-BR#summarizing-text" target="_blank">inteligente</a> de elementos de texto multilinha.</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=pt-BR" target="_blank">Tradução/localização</a></li>
</ul>
</td>
<td><ul>
<li>Variações</li>
<li>Variações como Live Copies</li>
<li>Versões</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=pt-BR#building-blocks" target="_blank">Blocos de construção</a></li>
<li>Anotações</li>
<li>Layout e visualização responsivos</li>
<li>Tradução/localização</li>
<li>Modelo de dados complexo por meio de referências de fragmento de conteúdo</li>
<li>Visualização no aplicativo</li>
</ul>
</td>
</tr><tr><td><strong>Utilização</strong></td>
<td><ul>
<li>Exportação JSON via <a href="https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=pt-BR">APIs do AEM Headless GraphQL</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=pt-BR" target="_blank">Componente Fragmento de Conteúdo dos Componentes Principais do AEM</a> para uso no AEM Sites, AEM Screens ou em Fragmentos de Experiência.</li>
<li>Exportação JSON via <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=pt-BR" target="_blank">AEM Content Services</a> para consumo de terceiros</li>
<li>Exportação JSON para o Adobe Target para ofertas direcionadas</li>
<li>JSON por meio de APIs AEM HTTP Assets para consumo de terceiros</li>
</ul>
</td>
<td><ul>
<li>Componente de Fragmento de experiência do AEM para uso no AEM Sites, AEM Screens ou outros Fragmentos de experiência.</li>
<li>Exportar como <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=pt-BR" target="_blank">HTML simples</a> para uso por sistemas de terceiros</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=pt-BR" target="_blank">Exportação do HTML para o Adobe Target</a> para ofertas direcionadas</li>
<li>Exportação JSON para o Adobe Target para ofertas direcionadas</li>
</ul>
</td>
</tr><tr><td><strong>Casos de uso comuns</strong></td>
<td><ul>
<li>Capacitando casos de uso headless com o GraphQL</li>
<li>Conteúdo estruturado baseado em formulário/entrada de dados</li>
<li>Conteúdo editorial de longo formato (elemento multilinha)</li>
<li>Conteúdo gerenciado fora do ciclo de vida dos canais que o fornecem</li>
</ul>
</td>
<td><ul>
<li>Gerenciamento centralizado de material promocional multicanal usando variações por canal.</li>
<li>Conteúdo reutilizado em várias páginas de um site.</li>
<li>Cromo do site da Web (ex. cabeçalho e rodapé)</li>
<li>Uma experiência gerenciada fora do ciclo de vida dos canais que a fornecem</li>
</ul>
</td>
</tr><tr><td><strong>Documentação</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=pt-BR&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Guia do usuário de fragmentos de conteúdo do AEM</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=pt-BR" target="_blank">Uso de fragmentos de conteúdo no AEM</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=pt-BR" target="_blank">Documentação do Adobe sobre Fragmentos de experiência</a></li>
</ul>
</td>
</tr></tbody></table>

## Arquitetura de fragmentos de conteúdo

O diagrama a seguir ilustra a arquitetura geral dos fragmentos de conteúdo do AEM

![Arquitetura de fragmentos de conteúdo](./assets/content-fragments-architecture.png)

+ **Os modelos de fragmento de conteúdo** definem os elementos (ou campos) que definem qual conteúdo o fragmento de conteúdo pode capturar e expor.
+ O **Fragmento de Conteúdo** é uma instância de um Modelo de Fragmento de Conteúdo que representa uma entidade de conteúdo lógica.
+ As **variações** do fragmento de conteúdo seguem o modelo de fragmento de conteúdo, no entanto, apresentam variações no conteúdo.
+ Os fragmentos de conteúdo podem ser expostos/consumidos por:
   + Uso de Fragmentos de conteúdo no **AEM Sites** (ou AEM Screens) por meio do componente Fragmento de conteúdo dos Componentes Principais do AEM WCM.
   + Consumir **Fragmento de conteúdo** de aplicativos headless usando APIs AEM Headless GraphQL.
   + Expor um conteúdo de variações de Fragmento de conteúdo como JSON via **AEM Content Services** e Páginas de API para casos de uso somente leitura.
   + Expondo diretamente o conteúdo do Fragmento de conteúdo (todas as variações) como JSON por meio de chamadas diretas ao AEM Assets por meio da **API HTTP do AEM Assets** para casos de uso de CRUD.

## Arquitetura de fragmentos de experiência

![Arquitetura de Fragmentos de experiência](./assets/experience-fragments-architecture.png)

+ **Modelos editáveis**, que por sua vez são definidos pelos **Tipos de Modelo Editáveis** e pela **implementação do componente de Página do AEM**, definem os Componentes do AEM permitidos que podem ser usados para compor um Fragmento de experiência.
+ O **Fragmento de Experiência** é uma instância de um Modelo Editável que representa uma experiência lógica.
+ As **variações** do fragmento de experiência seguem o modelo editável, no entanto, têm variações na experiência (conteúdo e design).
+ Os fragmentos de experiência podem ser expostos/consumidos por:
   + Uso de fragmentos de experiência no AEM Sites (ou AEM Screens) por meio do componente de Fragmento de experiência do AEM.
   + Expor um conteúdo de variações de Fragmento de experiência como JSON (com HTML inserido) via **Serviços de conteúdo do AEM** e Páginas de API.
   + Expondo diretamente uma variação de Fragmento de experiência como **&quot;Plain HTML&quot;**.
   + Exportar Fragmentos de experiência para **Adobe Target** como ofertas HTML ou JSON.
   + O AEM Sites é compatível nativamente com as ofertas da HTML, no entanto, as ofertas JSON exigem desenvolvimento personalizado.

## Recurso de suporte para fragmentos de conteúdo

+ [Guia do Usuário de Fragmentos de Conteúdo](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=pt-BR&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Introdução ao Adobe Experience Manager as a Headless CMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/introduction.html?lang=pt-BR)
+ [Usando fragmentos de conteúdo no AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=pt-BR)
+ [Componente Fragmento de Conteúdo dos Componentes Principais do AEM WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=pt-BR)
+ [Usando fragmentos de conteúdo e o AEM Headless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=pt-BR)
+ [Introdução aos Serviços de Conteúdo da AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=pt-BR)

## Recurso de suporte para Fragmentos de experiência

+ [Documentação do Adobe sobre Fragmentos de experiência](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=pt-BR)
+ [Noções básicas sobre os Fragmentos de experiência do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=pt-BR)
+ [Usando Fragmentos de experiência do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=pt-BR)
+ [Usando Fragmentos de experiência do AEM com o Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
