---
title: Adicionar nome de domínio personalizado
description: Saiba como adicionar um nome de domínio personalizado ao AEM como um site hospedado pelo Cloud Service.
version: Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: null
last-substantial-update: 2024-03-12T00:00:00Z
jira: KT-15121
thumbnail: KT-15121.jpeg
source-git-commit: 8230991cebf1a9e994f0dfe96c5590d0c19ef887
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---


# Adicionar nome de domínio personalizado

Saiba como adicionar um nome de domínio personalizado ao site do AEM as a Cloud Service.

Neste tutorial, a identidade visual da amostra [WKND AEM](https://github.com/adobe/aem-guides-wknd) site é aprimorado adicionando um nome de domínio personalizado endereçável por HTTPS `wknd.enablementadobe.com` com o protocolo TLS.

>[!VIDEO](https://video.tv.adobe.com/v/3427903?quality=12&learn=on)

As etapas de alto nível são:

![Nome de Domínio Personalizado Alto](./assets/add-custom-domain-name-steps.png){width="800" zoomable="yes"}

## Pré-requisitos

>[!VIDEO](https://video.tv.adobe.com/v/3427909?quality=12&learn=on)

- [OpenSSL](https://www.openssl.org/) e [dig](https://www.isc.org/blogs/dns-checker/) estão instalados no computador local.
- Acesso a serviços de terceiros:
   - Autoridade de Certificação (CA) - para solicitar o certificado assinado para o domínio do site, como [DigitCert](https://www.digicert.com/)
   - Serviço de hospedagem de DNS (Sistema de Nomes de Domínio) - para adicionar registros DNS ao seu domínio personalizado, como Azure DNS ou AWS Route 53.
- Acesso a [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/) como Proprietário da empresa ou Gerente de implantação.
- Amostra [WKND AEM](https://github.com/adobe/aem-guides-wknd) O site é implantado no ambiente AEM de [programa de produção](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs) tipo.

Se você não tiver acesso a serviços de terceiros, _colabore com sua equipe de segurança ou hospedagem para concluir as etapas_.

## Gerar certificado SSL

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

Você tem duas opções:

- Usar `openssl` ferramenta de linha de comando - você pode gerar uma chave privada e uma Solicitação de assinatura de certificado (CSR) para o domínio do site. Para solicitar um certificado assinado, envie a CSR a uma autoridade de certificação (CA).

- Sua equipe de hospedagem fornece a chave privada necessária e o certificado assinado para o site.

Vamos analisar as etapas da primeira opção.

Para gerar uma chave privada e uma CSR, execute os seguintes comandos e forneça as informações necessárias quando solicitado:

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

Para solicitar um certificado assinado, forneça a CSR gerada à CA seguindo a documentação. Depois que a CA assinar a CSR, você receberá o arquivo de certificado assinado.

### Revisar certificado assinado

Revisar o certificado assinado antes de adicioná-lo ao Cloud Manager é uma boa prática. Você pode revisar os detalhes do certificado usando o seguinte comando:

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

O certificado assinado pode conter a cadeia de certificados, que inclui os certificados raiz e intermediário junto com o certificado da entidade final.

O Adobe Cloud Manager aceita o certificado da entidade final e a cadeia de certificados _em campos de formulário separados_, portanto, você deve extrair o certificado de entidade final e a cadeia de certificados do certificado assinado.

Neste tutorial, a variável [DigitCert](https://www.digicert.com/) certificado assinado emitido contra `*.enablementadobe.com` O domínio é usado como exemplo. A entidade final e a cadeia de certificados são extraídas abrindo o certificado assinado em um editor de texto e copiando o conteúdo entre as `-----BEGIN CERTIFICATE-----` e `-----END CERTIFICATE-----` marcadores.

## Adicionar certificado SSL no Cloud Manager

>[!VIDEO](https://video.tv.adobe.com/v/3427906?quality=12&learn=on)

Para adicionar o certificado SSL no Cloud Manager, siga o [Adicionar certificado SSL](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-ssl-certificates/add-ssl-certificate) documentação.

## Verificação do nome de domínio

>[!VIDEO](https://video.tv.adobe.com/v/3427905?quality=12&learn=on)

Para verificar o nome de domínio, siga estas etapas:

- Adicione o nome do domínio no Cloud Manager seguindo o [Adicionar nome de domínio personalizado](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-custom-domain-name) documentação.
- Adicionar uma mensagem específica do AEM [Registro TXT](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-text-record) no serviço de hospedagem DNS.
- Verifique as etapas acima consultando os servidores DNS usando o `dig` comando.

```bash
# General syntax, the `_aemverification` is prefix provided by Adobe
$ dig _aemverification.[YOUR-DOMAIN-NAME] -t txt

# This tutorial specific example, as the subdomain `wknd.enablementadobe.com` is used
$ dig _aemverification.wknd.enablementadobe.com -t txt
```

O exemplo de resposta bem-sucedida tem esta aparência:

```bash
; <<>> DiG 9.10.6 <<>> _aemverification.wknd.enablementadobe.com -t txt
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8636
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1220
;; QUESTION SECTION:
;_aemverification.wknd.enablementadobe.com. IN TXT

;; ANSWER SECTION:
_aemverification.wknd.enablementadobe.com. 3600    IN TXT "adobe-aem-verification=wknd.enablementadobe.com/105881/991000/bef0e843-9280-4385-9984-357ed9a4217b"

;; Query time: 81 msec
;; SERVER: 153.32.14.247#53(153.32.14.247)
;; WHEN: Tue Mar 12 15:54:25 EDT 2024
;; MSG SIZE  rcvd: 181
```

Neste tutorial, o DNS do Azure é usado como exemplo. Para adicionar o registro TXT, você deve seguir a documentação do serviço de hospedagem do DNS.

Revise o [Verificação de status do nome de domínio](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-domain-name-status) se houver um problema.

## Configurar registro DNS

>[!VIDEO](https://video.tv.adobe.com/v/3427907?quality=12&learn=on)

Para configurar o registro DNS para seu domínio personalizado, siga estas etapas,

- Determine o tipo de registro DNS (CNAME ou APEX) com base no tipo de domínio, como domínio raiz (APEX) ou subdomínio (CNAME), e siga o [Definição das configurações de DNS](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/configure-dns-settings) documentação.
- Adicione o registro DNS em seu serviço de hospedagem de DNS.
- Acione a validação do registro DNS seguindo o método [Verificação de status do registro DNS](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-dns-record-status) documentação.

Neste tutorial, as a **subdomínio** `wknd.enablementadobe.com` for usado, o tipo de registro CNAME que aponta para `cdn.adobeaemcloud.com` é adicionado.

No entanto, se você estiver usando o **domínio raiz**, você deve adicionar um tipo de registro APEX (também conhecido como A, ALIAS ou ANAME) que aponte para os endereços IP específicos fornecidos pelo Adobe.

## Verificação do site

>[!VIDEO](https://video.tv.adobe.com/v/3427904?quality=12&learn=on)

Para verificar se o site pode ser acessado usando o nome de domínio personalizado, abra um navegador da Web e navegue até o URL do domínio personalizado. Verifique se o site pode ser acessado e se o navegador exibe uma conexão segura com o ícone de cadeado.

## Vídeo completo

Você também pode assistir ao vídeo completo que demonstra a visão geral, os pré-requisitos e as etapas acima para adicionar um nome de domínio personalizado ao site hospedado pelo AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3427817?quality=12&learn=on)


