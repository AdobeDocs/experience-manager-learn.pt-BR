---
title: Fragmentos de conteúdo e fragmentos de experiência
description: Saiba mais sobre as semelhanças e diferenças entre Fragmentos de conteúdo e Fragmentos de experiência, e quando e como usar cada tipo.
sub-product: Experience Manager Assets, Experience Manager Sites
feature: Content Fragments, Experience Fragments
topics: headless
version: 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: User
level: Beginner
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
source-git-commit: 84fdbaa173a929ae7467aecd031cacc4ce73538a
workflow-type: tm+mt
source-wordcount: '1044'
ht-degree: 4%

---

# Fragmentos de conteúdo e fragmentos de experiência

Os Fragmentos de conteúdo e Fragmentos de experiência do Adobe Experience Manager podem parecer semelhantes na superfície, mas cada um desempenha funções principais em diferentes casos de uso. Saiba como Fragmentos de conteúdo e Fragmentos de experiência são semelhantes, diferentes e quando e como usá-los.

## Comparação

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Fragmentos de conteúdo (CF)</strong></td>
<td><strong>Fragmentos de experiência (XF)</strong></td>
</tr><tr><td><strong>Definição</strong></td>
<td><ul>
<li>Reutilizável, agnóstico de apresentação <strong>conteúdo</strong>, composto por elementos de dados estruturados (texto, datas, referências, etc.)</li>
</ul>
</td>
<td><ul>
<li>Um composto reutilizável de um ou mais AEM Componentes que definem o conteúdo e a apresentação que forma um <strong>experiência</strong> que faz sentido por si só</li>
</ul>
</td>
</tr><tr><td><strong>Locatários principais</strong></td>
<td><ul>
<li>Centralizado no conteúdo</li>
<li>Definido por um <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">modelo de dados estruturado, baseado em formulário.</a></li>
<li>Design e layout agnósticos.</li>
<li>O canal é proprietário da apresentação do conteúdo do Fragmento de conteúdo (layout e design)</li>
</ul>
</td>
<td><ul>
<li>Centrado na apresentação</li>
<li>Definido por composição não estruturada de Componentes AEM</li>
<li>Define o design e o layout do conteúdo</li>
<li>Usado "no estado em que se encontra" em canais</li>
</ul>
</td>
</tr><tr><td><strong>Detalhes técnicos</strong></td>
<td><ul>
<li>Implementado como um <strong>dam:Asset</strong></li>
<li>Definido por um <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">Modelo de fragmento de conteúdo</a></li>
</ul>
</td>
<td><ul>
<li>Implementado como um <strong>cq:Page</strong></li>
<li>Definido por modelos editáveis</li>
<li>Representação nativa de HTML</li>
</ul>
</td>
</tr><tr><td><strong>Variações</strong></td>
<td><ul>
<li>A variação Principal é a variação canônica</li>
<li>As variações são específicas para casos de uso, que podem ser alinhadas com canais.</li>
</ul>
</td>
<td><ul>
<li>As variações são específicas de canal ou contexto</li>
<li>As variações são mantidas sincronizadas por meio AEM Live Copy</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">Blocos de construção</a> permitir reutilização de conteúdo em variações</li>
</ul>
</td>
</tr><tr><td><strong>Recursos</strong></td>
<td><ul>
<li>Variações</li>
<li>Versões</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank">Sincronização</a> de conteúdo em variações</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">Diferencial visual</a> das versões do Fragmento de conteúdo</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank">Anotações</a> de elementos de texto de várias linhas</li>
<li>Inteligente <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">resumo</a> de elementos de texto de várias linhas.</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=en" target="_blank">Tradução/localização</a></li>
</ul>
</td>
<td><ul>
<li>Variações</li>
<li>Variações como Live Copies</li>
<li>Versões</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en#building-blocks" target="_blank">Blocos de construção</a></li>
<li>Anotações</li>
<li>Layout responsivo e visualização</li>
<li>Tradução/localização</li>
<li>Modelo de dados complexo por meio de referências de fragmento de conteúdo</li>
<li>Visualização no aplicativo</li>
</ul>
</td>
</tr><tr><td><strong>Utilização</strong></td>
<td><ul>
<li>Exportação JSON via <a href="https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=pt-BR">APIs do GraphQL AEM Headless</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=pt-BR" target="_blank">Componente Fragmento de conteúdo dos componentes principais de AEM</a> para uso no AEM Sites, AEM Screens ou em Fragmentos de experiência.</li>
<li>Exportação JSON via <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">AEM Content Services</a> para consumo de terceiros</li>
<li>Exportação JSON para o Adobe Target para ofertas direcionadas</li>
<li>JSON por meio AEM APIs de ativos HTTP para consumo de terceiros</li>
</ul>
</td>
<td><ul>
<li>AEM componente Fragmento de experiência para uso no AEM Sites, AEM Screens ou outros Fragmentos de experiência.</li>
<li>Exportar como <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">HTML simples</a> para uso por sistemas de terceiros</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank">Exportação do HTML para o Adobe Target</a> para ofertas direcionadas</li>
<li>Exportação JSON para o Adobe Target para ofertas direcionadas</li>
</ul>
</td>
</tr><tr><td><strong>Casos de uso comuns</strong></td>
<td><ul>
<li>Fortalecendo casos de uso sem interface em relação ao GraphQL</li>
<li>Entrada de dados estruturada/conteúdo baseado em formulário</li>
<li>Conteúdo editorial de forma longa (elemento de várias linhas)</li>
<li>Conteúdo gerenciado fora do ciclo de vida dos canais que o entregam</li>
</ul>
</td>
<td><ul>
<li>Gerenciamento centralizado de materiais promocionais de vários canais usando variações por canal.</li>
<li>Conteúdo reutilizado em várias páginas em um site.</li>
<li>Chrome do site (ex.: cabeçalho e rodapé)</li>
<li>Uma experiência gerenciada fora do ciclo de vida dos canais que a entregam</li>
</ul>
</td>
</tr><tr><td><strong>Documentação</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Guia do usuário de Fragmentos de conteúdo do AEM</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en" target="_blank">Uso de fragmentos de conteúdo em AEM</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Documentação do Adobe sobre fragmentos de experiência</a></li>
</ul>
</td>
</tr></tbody></table>

## Arquitetura dos fragmentos de conteúdo

O diagrama a seguir ilustra a arquitetura geral dos Fragmentos de conteúdo AEM

![Arquitetura dos fragmentos de conteúdo](./assets/content-fragments-architecture.png)

+ **Modelos de fragmentos do conteúdo** defina os elementos (ou campos) que definem o conteúdo que o Fragmento de conteúdo pode capturar e expor.
+ O **Fragmento de conteúdo** é uma instância de um Modelo de fragmento de conteúdo que representa uma entidade de conteúdo lógico.
+ Fragmento de conteúdo **variações** No entanto, siga o Modelo do fragmento de conteúdo , que tem variações no conteúdo.
+ Fragmentos de conteúdo podem ser expostos/consumidos por:
   + Uso de fragmentos de conteúdo em **AEM Sites** (ou AEM Screens) por meio do componente Fragmento de conteúdo dos componentes principais do WCM AEM.
   + Consumir **Fragmento de conteúdo** de aplicativos sem cabeçalho usando APIs AEM Headless GraphQL.
   + Expor um fragmento de conteúdo altera o conteúdo como JSON via **AEM Content Services** Páginas de API para casos de uso somente leitura.
   + Exposição direta do conteúdo do fragmento de conteúdo (todas as variações) como JSON por meio de chamadas diretas ao AEM Assets por meio da **API HTTP AEM Assets** para casos de uso de CRUD.

## Arquitetura dos fragmentos de experiência

![Arquitetura dos fragmentos de experiência](./assets/experience-fragments-architecture.png)

+ **Modelos editáveis** que, por sua vez, são definidas por **Tipos de modelo editáveis** e um **Implementação do componente Página AEM**, defina os Componentes de AEM permitidos que podem ser usados para compor um Fragmento de experiência.
+ O **Fragmento de experiência** é uma instância de um Modelo editável que representa uma experiência lógica.
+ Fragmento de experiência **variações** No entanto, siga o Modelo editável com variações de experiência (conteúdo e design).
+ Fragmentos de experiência podem ser expostos/consumidos por:
   + Uso de fragmentos de experiência no AEM Sites (ou AEM Screens) por meio do componente Fragmento de experiência AEM.
   + Exposição de um fragmento de experiência altera o conteúdo como JSON (com HTML incorporado) via **AEM Content Services** e páginas de API.
   + Expor diretamente uma variação do Fragmento de experiência como **&quot;HTML simples&quot;**.
   + Exportar fragmentos de experiência para **Adobe Target** como HTML ou ofertas JSON.
   + A AEM Sites oferece suporte nativo a ofertas de HTML, no entanto, as ofertas JSON exigem desenvolvimento personalizado.

## Recurso de suporte para fragmentos de conteúdo

+ [Guia do usuário de Fragmentos de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Introdução ao Adobe Experience Manager as a Headless CMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/introduction.html?lang=pt-BR)
+ [Uso de fragmentos de conteúdo em AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [AEM componente Fragmento de conteúdo dos componentes principais do WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=pt-BR)
+ [Uso de fragmentos de conteúdo e AEM headless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=en)
+ [Introdução aos serviços de conteúdo AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## Recurso de suporte para Fragmentos de experiência

+ [Documentação do Adobe sobre fragmentos de experiência](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [Compreensão AEM fragmentos de experiência](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Uso AEM fragmentos de experiência](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Uso AEM fragmentos de experiência com o Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
