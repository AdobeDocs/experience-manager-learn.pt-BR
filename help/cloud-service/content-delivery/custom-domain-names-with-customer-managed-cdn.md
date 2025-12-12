---
title: Nome de domínio personalizado com CDN gerenciado pelo cliente
description: Saiba como implementar um nome de domínio personalizado no site da AEM as a Cloud Service que usa um CDN gerenciado pelo cliente.
version: Experience Manager as a Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-06-21T00:00:00Z
jira: KT-15945
thumbnail: KT-15945.jpeg
exl-id: fa9ee14f-130e-491b-91b6-594ba47a7278
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# Nome de domínio personalizado com CDN gerenciado pelo cliente

Saiba como adicionar um nome de domínio personalizado a um site do AEM as a Cloud Service que usa uma **CDN gerenciada pelo cliente**.

Neste tutorial, a identidade visual do site [AEM WKND](https://github.com/adobe/aem-guides-wknd) de exemplo é aprimorada com a adição de um nome de domínio personalizado endereçável por HTTPS `wkndviaawscdn.enablementadobe.com` com TLS (Transport Layer Security) usando uma CDN gerenciada pelo cliente. Neste tutorial, o AWS CloudFront é usado como a CDN gerenciada pelo cliente, no entanto, qualquer provedor de CDN deve ser compatível com o AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3432561?quality=12&learn=on)

As etapas de alto nível são:

![Nome de domínio personalizado com CDN do cliente](./assets/add-custom-domain-name-with-customer-CDN.png){width="800" zoomable="yes"}

## Pré-requisitos

>[!VIDEO](https://video.tv.adobe.com/v/3432562?quality=12&learn=on)

- O [OpenSSL](https://www.openssl.org/) e o [dig](https://www.isc.org/blogs/dns-checker/) estão instalados no computador local.
- Acesso a serviços de terceiros:
   - Autoridade de Certificação (CA) - para solicitar o certificado assinado para o domínio do site, como [DigitCert](https://www.digicert.com/)
   - CDN do cliente - para configurar o CDN do cliente e adicionar certificados SSL e detalhes de domínio, como AWS CloudFront, CDN do Azure ou Akamai.
   - Serviço de hospedagem de DNS (Sistema de Nomes de Domínio) - para adicionar registros DNS ao seu domínio personalizado, como Azure DNS ou AWS Route 53.
- Acesso ao [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/) para implantar a regra CDN de validação do Cabeçalho HTTP no ambiente do AEM as a Cloud Service.
- O site [AEM WKND](https://github.com/adobe/aem-guides-wknd) de exemplo está implantado no ambiente AEM as a Cloud Service do tipo [programa de produção](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs).

Se você não tiver acesso a serviços de terceiros, _colabore com a equipe de segurança ou hospedagem para concluir as etapas_.

## Gerar certificado SSL

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

Você tem duas opções:

1. Usando a ferramenta de linha de comando `openssl` - você pode gerar uma chave privada e uma Solicitação de Assinatura de Certificado (CSR) para o domínio do site. Para solicitar um certificado assinado, envie a CSR a uma CA (Autoridade de Certificação).
1. Sua equipe de hospedagem fornece a chave privada necessária e o certificado assinado para o site.

Vamos analisar as etapas da primeira opção.

Para gerar uma chave privada e uma CSR, execute os seguintes comandos e forneça as informações necessárias quando solicitado:

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

Para solicitar um certificado assinado, forneça a CSR gerada à CA seguindo a documentação. Depois que a CA assinar a CSR, você receberá o arquivo de certificado assinado.

### Revisar certificado assinado

Revisar o certificado assinado antes de adicioná-lo à Cloud Manager é uma boa prática. Você pode revisar os detalhes do certificado usando o seguinte comando:

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

O certificado assinado pode conter a cadeia de certificados, que inclui os certificados raiz e intermediário junto com o certificado da entidade final.

O Adobe Cloud Manager aceita o certificado de entidade final e a cadeia de certificados _em campos de formulário separados_, portanto, você deve extrair o certificado de entidade final e a cadeia de certificados do certificado assinado.

Neste tutorial, o certificado assinado [DigitCert](https://www.digicert.com/) emitido para o domínio `*.enablementadobe.com` é usado como exemplo. A entidade final e a cadeia de certificados são extraídas abrindo o certificado assinado em um editor de texto e copiando o conteúdo entre os marcadores `-----BEGIN CERTIFICATE-----` e `-----END CERTIFICATE-----`.

## Configurar a CDN gerenciada pelo cliente

>[!VIDEO](https://video.tv.adobe.com/v/3432563?quality=12&learn=on)

Configure o CDN do cliente, como AWS CloudFront, Azure CDN ou Akamai, e adicione o certificado SSL e os detalhes do domínio. Neste tutorial, o AWS CloudFront é usado como exemplo. No entanto, dependendo do fornecedor de CDN, as etapas podem variar. As principais chamadas de retorno são:

- Adicione o certificado SSL ao CDN.
- Adicione o nome de domínio personalizado à CDN.
- Configure a CDN para armazenar o conteúdo em cache, como imagens, CSS e arquivos JavaScript.
- Adicione o cabeçalho HTTP `X-Forwarded-Host` às configurações de CDN para que sua CDN inclua esse cabeçalho em todas as solicitações enviadas à origem do AEMCD.
- Verifique se o valor do cabeçalho `Host` está definido como o domínio padrão do AEM as a Cloud Service que contém a ID do programa e do ambiente e que termina com `adobeaemcloud.com`. O valor do cabeçalho do host HTTP passado do CDN do cliente para o CDN da Adobe deve ser o domínio padrão do AEM as a Cloud Service. Qualquer outro valor deve resultar em um estado de erro.

## Configurar registros DNS

>[!VIDEO](https://video.tv.adobe.com/v/3432564?quality=12&learn=on)

Para configurar o registro DNS para seu domínio personalizado, siga estas etapas,

1. Adicione um registro CNAME para o domínio personalizado que aponta para o nome de domínio CDN.

Este tutorial adiciona um registro CNAME ao DNS do Azure para o domínio personalizado `wkndviaawscdn.enablementadobe.com` e o aponta para o nome do domínio de distribuição do AWS CloudFront.

### Verificação do site

Verifique o nome de domínio personalizado acessando o site com o nome de domínio personalizado.
Pode ou não funcionar, dependendo da configuração do vhhost no ambiente do AEM as a Cloud Service.

Uma etapa de segurança crucial é implantar a regra CDN de validação de cabeçalho HTTP no ambiente do AEM as a Cloud Service. A regra garante que a solicitação vem da CDN do cliente e não de qualquer outra fonte.

## Estado de trabalho atual sem a regra CDN de validação do Cabeçalho HTTP

>[!VIDEO](https://video.tv.adobe.com/v/3432565?quality=12&learn=on)

Sem a regra CDN de validação do Cabeçalho HTTP, o valor do cabeçalho `Host` é definido como o domínio padrão do AEM as a Cloud Service que contém a ID do programa e do ambiente e termina com `adobeaemcloud.com`. O Adobe CDN transforma o valor do cabeçalho `Host` no valor de `X-Forwarded-Host` recebido do CDN do cliente somente se a regra CDN de validação do Cabeçalho HTTP for implantada. Caso contrário, o valor do cabeçalho `Host` será passado como está para o ambiente AEM as a Cloud Service e o cabeçalho `X-Forwarded-Host` não será usado.

### Exemplo de código de servlet para imprimir o valor do cabeçalho do Host

O código de servlet a seguir imprime os valores do cabeçalho HTTP `Host`, `X-Forwarded-*`, `Referer` e `Via` na resposta JSON.

```java
package com.adobe.aem.guides.wknd.core.servlets;

import java.io.IOException;
import java.util.Enumeration;

import javax.servlet.Servlet;
import javax.servlet.ServletException;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.ServletResolverConstants;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component(service = Servlet.class, property = {
        ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/verify-headers",
        ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
})
public class VerifyHeadersServlet extends SlingSafeMethodsServlet {

    @Reference
    private ResourceResolverFactory resourceResolverFactory;

    @Override
    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");

        // Create JSON response
        StringBuilder jsonResponse = new StringBuilder();
        jsonResponse.append("{");

        Enumeration<String> headerNames = request.getHeaderNames();
        boolean firstHeader = true;

        while (headerNames.hasMoreElements()) {
            String headerName = headerNames.nextElement();

            if (headerName.startsWith("X-Forwarded-") || headerName.startsWith("Host")
                    || headerName.startsWith("Referer") || headerName.startsWith("Via")) {
                if (!firstHeader) {
                    jsonResponse.append(",");
                }
                jsonResponse.append("\"").append(headerName).append("\": \"").append(request.getHeader(headerName))
                        .append("\"");
                firstHeader = false;
            }
        }

        jsonResponse.append("}");

        response.getWriter().write(jsonResponse.toString());
    }
}
```

Para testar o servlet, atualize o arquivo `../dispatcher/src/conf.dispatcher.d/filters/filters.any` com a seguinte configuração. Verifique também se o CDN está configurado para **NÃO armazenar em cache** o caminho `/bin/*`.

```plaintext
# Testing purpose bin
/0300 { /type "allow" /extension "json" /path "/bin/*"}
/0301 { /type "allow" /path "/bin/*"}
/0302 { /type "allow" /url "/bin/*"}
```

## Configurar e implantar a regra CDN de validação do Cabeçalho HTTP

>[!VIDEO](https://video.tv.adobe.com/v/3432566?quality=12&learn=on)

Para configurar e implantar a regra CDN de validação do Cabeçalho HTTP, siga estas etapas:

- Adicione a regra CDN de validação de Cabeçalho HTTP ao arquivo `cdn.yaml`. Um exemplo é fornecido abaixo.

  ```yaml
  kind: "CDN"
  version: "1"
  metadata:
    envTypes: ["prod"]
  data:
    authentication:
      authenticators:
        - name: edge-auth
          type: edge
          edgeKey1: ${{CDN_EDGEKEY_080124}}
          edgeKey2: ${{CDN_EDGEKEY_110124}}
      rules:
        - name: edge-auth-rule
          when: { reqProperty: tier, equals: "publish" }
          action:
            type: authenticate
            authenticator: edge-auth
  ```

- Crie variáveis de ambiente do tipo secreto (CDN_EDGEKEY_080124, CDN_EDGEKEY_110124) usando a interface do Cloud Manager.
- Implante a regra CDN de validação de cabeçalho HTTP no ambiente do AEM as a Cloud Service usando o pipeline do Cloud Manager.

## Passar segredo no cabeçalho HTTP X-AEM-Edge-Key

>[!VIDEO](https://video.tv.adobe.com/v/3432567?quality=12&learn=on)

Atualize o CDN do cliente para transmitir o segredo no Cabeçalho HTTP `X-AEM-Edge-Key`. O segredo é usado pela CDN da Adobe para validar se a solicitação vem da CDN do cliente e transformar o valor do cabeçalho `Host` no valor da `X-Forwarded-Host` recebida da CDN do cliente.

## Vídeo completo

Você também pode assistir ao vídeo completo que demonstra as etapas acima para adicionar um nome de domínio personalizado com um CDN gerenciado pelo cliente a um site hospedado pela AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3432568?quality=12&learn=on)
