---
title: Monitoramento do usuário real (RUM)
description: Saiba mais sobre o Monitoramento de usuário real (RUM) no site as a Cloud Service do AEM.
version: Cloud Service
feature: Operations
topic: Performance
role: Admin, Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 0
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-03-18T00:00:00Z
source-git-commit: 7c80bb25b79a77c4a0bb2bbedf8a7c338177b857
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 0%

---


# Monitoramento do usuário real (RUM)

Saiba mais sobre o Monitoramento de usuário real (RUM) no site as a Cloud Service do AEM. Entenda como habilitar o RUM, quais dados são coletados e como usar os dados de RUM para otimizar a experiência do usuário em seu site.

## Visão geral

O Monitoramento de usuário real (RUM) é um método usado para _coletar, medir e analisar interações e experiências do usuário_ em tempo real. Ele fornece insights sobre como os visitantes do site estão interagindo com o site, incluindo o comportamento, o desempenho e a experiência geral. Isso é feito injetando uma pequena parte do código JavaScript nas páginas do site.

Usando o código JavaScript, o RUM captura dados diretamente do navegador do usuário, à medida que eles interagem com seu site. Esses dados podem ser usados para identificar e diagnosticar problemas de desempenho, otimizar a experiência do usuário e melhorar os resultados dos negócios.

O recurso RUM no AEM as a Cloud Service fornece uma visão abrangente da experiência do usuário do seu site. Ele captura as seguintes métricas principais para cada página (URL) visitada pelo usuário:

- [Maior pintura de conteúdo (LCP)](https://web.dev/articles/lcp) - mede o desempenho do carregamento.
- [Mudança de layout cumulativa (CLS)](https://web.dev/articles/cls) - mede a estabilidade visual.
- [Interação com o Próximo Pintura (INP)](https://web.dev/articles/inp) - mede a interatividade.
- Visualizações de página - mede o número de vezes que uma página é visualizada.

Além disso, captura os erros 404 e os gráficos de visualização de página do site.

As métricas de LCP, CLS e INP fazem parte do [Vídeos principais da Web](https://web.dev/articles/vitals) que são um conjunto de métricas relacionadas à velocidade, capacidade de resposta e estabilidade visual de um site. Essas métricas são usadas pelo Google para medir a experiência do usuário em um site e são importantes para a classificação do mecanismo de pesquisa.

## Ativar RUM

Para habilitar o RUM para o site do AEM, consulte [Como Configurar o Serviço de Monitoramento do Usuário Real](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#how-to-set-up-the-rum-service).

Os principais detalhes do RUM no AEM CS são:

- O RUM só é aplicável ao serviço de publicação do AEMCS, o que significa que o código JavaScript é inserido somente no ambiente de publicação.
- A variável `com.adobe.granite.webvitals.WebVitalsConfig` A configuração do OSGi controla os caminhos de inclusão e exclusão, que são caminhos de repositório e não caminhos de URL.
- Por padrão `/content` o caminho está incluído.
- Para excluir caminhos, adicione o `AEM_WEBVITALS_EXCLUDE` Variável de ambiente do Cloud Manager, consulte [Adição de variáveis de ambiente](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables#add-variables). Os caminhos são separados por vírgula.
- O código OOTB é responsável por inserir o código JavaScript nas páginas.

### Verificação

Para verificar se o RUM está ativado para o seu site, exiba a origem de HTML da página publicada e procure os seguintes blocos de script:

```html
...

<!-- Added before the closing </head> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('top');
    window.addEventListener('load', () => sampleRUM('load'));
    document.addEventListener('click', () => sampleRUM('click'));
</script>

...

<!-- Added before the closing </body> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('lazy');
    sampleRUM('cwv');
</script>
```

## Coleta de dados RUM

- Os dados do RUM são coletados usando `sampleRUM()` transmitindo o nome do ponto de verificação. No exemplo acima, os pontos de verificação são `top`, `load`, `click`, `lazy`, e `cwv`.
- Um ponto de verificação é um evento nomeado na sequência de carregamento da página e interação com ela.

Consulte também [Serviço de monitoramento e privacidade do usuário real](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#rum-service-and-privacy) e [Quais dados estão sendo coletados](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#what-data-is-being-collected) para obter mais detalhes.

## Exibição de dados do RUM

Para exibir os dados de RUM necessários `domainkey`, o Adobe o fornece como parte da configuração do RUM. O painel RUM está disponível em [https://data.aem.live/](https://data.aem.live/) e você pode acessá-lo usando a chave de domínio e o url.

Por exemplo, a captura de tela abaixo mostra o painel RUM do site da WKND do AEM.

![Painel de RUM](./assets/rum/RUM-Dashboard-WKND.png)

O painel de RUM fornece os seguintes insights principais:

- **Métricas de desempenho** - LCP, CLS, INP e Pageviews.
- **Métricas de erro** - erros 404.
- **Gráficos de visualização de página** - Número de visualizações de página ao longo do tempo.

## Como usar os dados de RUM

Usando os insights acima, você pode otimizar a experiência do usuário em seu site. Por exemplo:

- Reduza LCP, CLS e INP para melhorar o desempenho e a interatividade do carregamento da página. Consulte [Como melhorar o LCP](https://web.dev/articles/lcp#improve-lcp), [Como melhorar o CLS](https://web.dev/articles/cls#improve-cls) e [Como melhorar o INP](https://web.dev/articles/inp#improve-inp)para obter mais detalhes.
- Para melhorar a experiência do usuário, corrija os erros 404.
- Para entender o comportamento do usuário e otimizar o conteúdo, analise os gráficos de visualização de página.

A Adobe recomenda a revisão regular do painel de RUM e, especialmente, após a liberação maior ou menor.

Consulte também [Quem pode se beneficiar do serviço de monitoramento de usuários reais](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#who-can-benefit-from-rum-service) para obter mais detalhes.
