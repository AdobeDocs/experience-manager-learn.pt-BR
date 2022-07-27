---
title: Implantação de SPA para AEM GraphQL
description: Saiba mais sobre SPA opções de implantação em relação a AEM GraphQL, Headless.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: c7d2e69a9039cfbaa43d6d9b65b9fa6f69378716
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---


# Implantação de um SPA

Nesta seção, vamos analisar uma abordagem de implantação de SPA (React, Vue, Angular etc.) que invoque AEM APIs GraphQL para carregar os dados, no entanto, antes de entender os artefatos de alto nível que devem ser implantados.

**SPA Criar Artefatos Do Aplicativo:**

Os arquivos produzidos pela estrutura do SPA, normalmente **HTML, CSS, JS**, também conhecido como artefatos de construção estática. No caso do aplicativo React, os artefatos do `build` e para artefatos Vue do `dist` diretório.
A solicitação para o seu SPA (por exemplo, https://HOST/my-aem-spa.html) será veiculada usando esses artefatos de construção.

**AEM APIs GraphQL:**

Claramente, esse ponto de extremidade da API GraphQL (`/graphql/execute.json/<PROJECT-CONFIG>/<PERSISTED-QUERY-NAME>`) deve ser hospedado no domínio AEM.

Em resumo, SPA arquitetura de implantação tem duas partes *1. SPA 2. Camada de API GraphQL da AEM*, portanto, vamos analisar as opções de implantação para essas duas partes.


## Opções de implantação

| Opção de implantação | URL SPA | URL da API GraphQL AEM | Configuração do CORS necessária? |
| ---------|---------- | ---------|---------- |
| **Mesmo domínio** | https://**HOST**/my-aem-spa.html | https://**HOST**/graphql/execute.json/.. | ✘ |
| **Domínio diferente** | https://**SPA-HOST**/my-aem-spa.html | https://**AEM-HOST**/graphql/execute.json/.. | ✔ |

**Mesmo domínio:**\
Ambos *Camada de API GraphQL SPA &amp; AEM* nessa opção são implantadas no **Mesmo domínio**. Solicitação de significado para SPA URI `/my-aem-spa.html` Camada da API &amp; GraphQL `/graphql/execute.json/` são servidas exatamente do mesmo domínio.

**Domínio diferente:**\
Ambos *Camada de API GraphQL SPA &amp; AEM* nesta opção, são implantadas em **Domínio diferente**. Solicitação de significado para SPA URI `/my-aem-spa.html` é servido de **domínio diferente** em relação à camada GraphQL da API `/graphql/execute.json/` solicitações. Observe que, como parte dessa opção de implantação, você precisa [configurar CORS](cors.md) na instância de AEM.

>[!NOTE]
>
>É NECESSÁRIO configurar corretamente o CORS AEM instância, [veja as etapas aqui](cors.md).

### Implantação no mesmo domínio

Ao implantar no mesmo domínio, o domínio pode ser um **domínio AEM primário** (No domínio AEM) ou **domínio SPA primário** (Sem AEM domínio) e SPA recebidos, AEM solicitações de API GraphQL podem ser divididas em qualquer um dos componentes de implantação, como CDN ([Rápido](https://docs.fastly.com/en/guides/routing-assets-to-different-origins), Akamai, [CloudFront](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-distribution-serve-content/)), [HTTPD c/ Proxy reverso](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html). Em outras palavras, você ainda implanta os artefatos de criação de SPA e AEM APIs GraphQL para diferentes servidores, mas para usuários finais, eles são entregues de um único domínio e atrás da cena roteada para um destino ou servidores de origem diferente.

Além disso, é possível hospedar artefatos SPA build em AEM *mas não são recomendadas.*

| Implantação do mesmo domínio | Divisão de CDN | HTTPD + Proxy reverso | AEM Artefatos SPA Hospedados |
| ---------|---------- | ---------|---------- |
| **Domínio AEM** | ✔ | ✔ | ✔ |
| **Domínio OFF AEM** | ✔ | ✔ | **N/A** |


**HTTPD + Proxy reverso**

Uma configuração de exemplo terá a aparência abaixo

>[!TIP]
>
> As configurações a seguir são exemplos. Ajuste-os para alinhar-se às necessidades do seu projeto.

NO DOMÍNIO AEM

    &quot;
    ProxyPass &quot;/${YOUR-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    ProxyPassReverse &quot;/${YOUR-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    &quot;

Domínio OFF AEM

    &quot;
    ProxyPass &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    ProxyPassReverse &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    &quot;




### Implantação em domínio diferente

Nesse cenário, SPA artefatos de build são implantados em um domínio diferente do domínio AEM APIs GraphQL e, para usuários finais, serão entregues em dois domínios separados. [Configuração do CORS](cors.md) MUST em AEM.

**SPA solicitações de aplicativo por meio de domínios diferentes**

![Entrega de SPA de domínio diferente](assets/spa/different-domain-spa-delivery.png)


**Cabeçalho de resposta do CORS na API GraphQL AEM**

![Cabeçalho de resposta do CORS AEM API GraphQL](assets/spa/CORS-response-header-aem-graphql-api.png)


