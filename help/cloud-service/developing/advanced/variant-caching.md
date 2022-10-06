---
title: Armazenamento em cache de variantes de página com AEM as a Cloud Service
description: Saiba como configurar e usar o AEM as a cloud service para suportar variantes de página em cache.
role: Architect, Developer
topic: Development
feature: CDN Cache, Dispatcher
exl-id: fdf62074-1a16-437b-b5dc-5fb4e11f1355
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 1%

---

# Armazenamento de variantes de página em cache

Saiba como configurar e usar o AEM as a cloud service para suportar variantes de página em cache.

## Exemplo de casos de uso

+ Qualquer provedor de serviços que ofereça um conjunto diferente de ofertas de serviço e opções de preço correspondentes com base na localização geográfica do usuário e no cache de páginas com conteúdo dinâmico deve ser gerenciado no CDN e no Dispatcher.

+ Um cliente de varejo tem lojas em todo o país e cada loja tem ofertas diferentes com base em onde está localizada e o cache de páginas com conteúdo dinâmico deve ser gerenciado no CDN e no Dispatcher.

## Visão geral da solução

+ Identifique a chave da variante e o número de valores que ela pode ter. No nosso exemplo, variamos de acordo com o estado dos EUA, portanto, o número máximo é 50. Isso é pequeno o suficiente para não causar problemas nos limites da variante na CDN. [Seção Analisar limitações de variante](#variant-limitations).

+ O código AEM deve definir o cookie __&quot;x-aem-variant&quot;__ para o estado preferencial do visitante (por exemplo, `Set-Cookie: x-aem-variant=NY`) na resposta HTTP correspondente da solicitação HTTP inicial.

+ Solicitações subsequentes do visitante enviam esse cookie (por exemplo, `"Cookie: x-aem-variant=NY"`) e o cookie é transformado no nível da CDN em um cabeçalho predefinido (ou seja, `x-aem-variant:NY`) que é transmitido ao dispatcher.

+ Uma regra de reescrita do Apache modifica o caminho da solicitação para incluir o valor de cabeçalho no URL da página como um Seletor do Apache Sling (por exemplo, `/page.variant=NY.html`). Isso permite que o AEM Publish forneça conteúdo diferente com base no seletor e no dispatcher armazene uma página em cache por variante.

+ A resposta enviada pelo AEM Dispatcher deve conter um cabeçalho de resposta HTTP `Vary: x-aem-variant`. Isso instrui a CDN a armazenar diferentes cópias de cache para diferentes valores de cabeçalho.

>[!TIP]
>
>Sempre que um cookie é definido (por exemplo, Set-Cookie: x-aem-variant=NY) a resposta não deve ser armazenada em cache (deve ter Cache-Control: private ou Cache-Control: no-cache)

## Fluxo de solicitação HTTP

![Fluxo de Solicitação de Cache Variável](./assets/variant-cache-request-flow.png)

>[!NOTE]
>
>O fluxo de solicitação HTTP inicial acima deve ocorrer antes que qualquer conteúdo seja solicitado que use variantes.

## Uso

1. Para demonstrar o recurso, usaremos [WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=pt-BR)Como exemplo, a implementação da .

1. Implementar um [SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) em AEM para definir `x-aem-variant` na resposta HTTP, com um valor de variante.

1. AEM CDN transforma automaticamente `x-aem-variant` em um cabeçalho HTTP com o mesmo nome.

1. Adicionar uma regra mod_rewrite do servidor Apache Web `dispatcher` projeto, que modifica o caminho da solicitação para incluir o seletor de variante.

1. Implante o filtro e reescreva as regras usando o Cloud Manager.

1. Teste o fluxo de solicitação geral.

## Amostras de código

+ Exemplo de SlingServletFilter para definir `x-aem-variant` com um valor em AEM.

   ```
   package com.adobe.aem.guides.wknd.core.servlets.filters;
   
   import javax.servlet.*;
   import java.io.IOException;
   
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.api.SlingHttpServletResponse;
   import org.apache.sling.servlets.annotations.SlingServletFilter;
   import org.apache.sling.servlets.annotations.SlingServletFilterScope;
   import org.osgi.service.component.annotations.Component;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   
   
   // Invoke filter on  HTTP GET /content/wknd.*.foo|bar.html|json requests.
   // This code and scope is for example purposes only, and will not interfere with other requests.
   @Component
   @SlingServletFilter(scope = {SlingServletFilterScope.REQUEST},
           resourceTypes = {"cq:Page"},
           pattern = "/content/wknd/.*",
           extensions = {"html", "json"},
           methods = {"GET"})
   public class PageVariantFilter implements Filter {
       private static final Logger log = LoggerFactory.getLogger(PageVariantFilter.class);
       private static final String VARIANT_COOKIE_NAME = "x-aem-variant";
   
       @Override
       public void init(FilterConfig filterConfig) throws ServletException { }
   
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           SlingHttpServletResponse slingResponse = (SlingHttpServletResponse) servletResponse;
           SlingHttpServletRequest slingRequest = (SlingHttpServletRequest) servletRequest;
   
           // Check is the variant was previously set
           final String existingVariant = slingRequest.getCookie(VARIANT_COOKIE_NAME).getValue();
   
           if (existingVariant == null) {
               // Variant has not been set, so set it now
               String newVariant = "NY"; // Hard coding as an example, but should be a calculated value
               slingResponse.setHeader("Set-Cookie", VARIANT_COOKIE_NAME + "=" + newVariant + "; Path=/; HttpOnly; Secure; SameSite=Strict");
               log.debug("x-aem-variant cookie is set with the value {}", newVariant);
           } else {
               log.debug("x-aem-variant previously set with value {}", existingVariant);
           }
   
           filterChain.doFilter(servletRequest, slingResponse);
       }
   
       @Override
       public void destroy() { }
   }
   ```

+ Regra de reescrita de exemplo no __dispatcher/src/conf.d/rewrite.rules__ arquivo que é gerenciado como código fonte no Git e implantado usando o Cloud Manager.

   ```
   ...
   
   RewriteCond %{REQUEST_URI} ^/us/.*  
   RewriteCond %{HTTP:x-aem-variant} ^.*$  
   RewriteRule ^([^?]+)\.(html.*)$ /content/wknd$1.variant=%{HTTP:x-aem-variant}.$2 [PT,L] 
   
   ...
   ```

## Limitações de variáveis

+ AEM CDN pode gerenciar até 200 variações. Isso significa que o `x-aem-variant` pode ter até 200 valores exclusivos. Para obter mais informações, consulte o [Limites de configuração de CDN](https://docs.fastly.com/en/guides/resource-limits).

+ É necessário tomar cuidado para garantir que a chave de variante escolhida nunca exceda esse número.  Por exemplo, uma ID de usuário não é uma boa chave, pois excederia facilmente 200 valores para a maioria dos sites, enquanto os estados/territórios em um país são mais adequados se houver menos de 200 estados nesse país.

>[!NOTE]
>
>Quando as variantes excederem 200, a CDN responderá com &quot;Muitas variantes&quot; em vez do conteúdo da página.
