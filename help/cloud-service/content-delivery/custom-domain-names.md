---
title: Opções de nome de domínio personalizado
description: Saiba como gerenciar e implementar nomes de domínio personalizados para seu site hospedado na AEM as a Cloud Service.
version: Cloud Service
feature: Cloud Manager, Custom Domain Names
topic: Architecture, Migration
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 130
last-substantial-update: 2024-08-09T00:00:00Z
jira: KT-15946
thumbnail: KT-15946.jpeg
source-git-commit: 07225f1ae4455e2fa69c8e488851361c725fe9e8
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 1%

---

# Opções de nome de domínio personalizado

Saiba como gerenciar e implementar nomes de domínio para seu site hospedado na AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3432632?quality=12&learn=on)

## Antes de começar

Antes de começar a implementar nomes de domínio personalizados, compreenda os seguintes conceitos:

### O que é um nome de domínio

Um nome de domínio é o nome amigável do nome do site, como adobe.com, que aponta para um local específico (endereço IP como 170.2.14.16) na Internet.

### Nomes de domínio padrão no AEM as a Cloud Service

Por padrão, o AEM as a Cloud Service é provisionado com um nome de domínio padrão, terminando em `*.adobeaemcloud.com`. O certificado SSL curinga emitido em `*.adobeaemcloud.com` é aplicado automaticamente a todos os ambientes e esse certificado curinga é responsabilidade do Adobe.

Os nomes de domínio padrão estão no formato `https://<SERVICE-TYPE>-p<PROGRAM-ID>-e<ENVIRONMENT-ID>.adobeaemcloud.com`.

- O `<SERVICE-TYPE>` pode ser **autor**, **publicação** ou **visualização**.
- O `<PROGRAM-ID>` é o identificador exclusivo do programa. Uma organização pode ter vários programas.
- O `<ENVIRONMENT-ID>` é o identificador exclusivo do ambiente e cada programa contém quatro ambientes: **Desenvolvimento Rápido (RDE)**, **dev**, **stage** e **prod**. Cada ambiente contém os três tipos de serviço mencionados acima, exceto **RDE** que não tem um ambiente de visualização.

Em resumo, depois que todos os ambientes do AEM as a Cloud Service forem provisionados, você terá URLs exclusivas **11** (o RDE não tem um ambiente de visualização) combinadas com o nome de domínio padrão.

### CDN gerenciada por Adobe vs. CDN gerenciada pelo cliente

Para reduzir a latência e melhorar o desempenho do site, o AEM as a Cloud Service é integrado a uma Rede de entrega de conteúdo (CDN) gerenciada por Adobe. A CDN gerenciada por Adobe é ativada automaticamente para todos os ambientes. Consulte [cache do AEM as a Cloud Service](../caching/overview.md) para obter mais detalhes.

No entanto, os clientes também podem usar sua própria CDN, chamada de **CDN gerenciada pelo cliente**. Não é necessário, mas poucos clientes o usam para políticas corporativas ou outros motivos. Nesse caso, o cliente é responsável por gerenciar as configurações e os ajustes da CDN.

### Nomes de domínio personalizados

Os nomes de domínio personalizados são sempre preferidos em relação ao nome de domínio padrão para fins de desenvolvimento de marca, autenticidade e negócios. No entanto, eles só podem ser aplicados aos tipos de serviço **publicar** e **visualizar**, e não ao **autor**.

Ao adicionar nomes de domínio personalizados, você deve fornecer um certificado SSL válido para o domínio personalizado fornecido. O certificado SSL deve ser um certificado válido assinado por uma CA confiável.

Normalmente, os clientes usam um nome de domínio personalizado para ambientes de produção (site do AEM as a Cloud Service) e, às vezes, para ambientes inferiores como **stage** ou **dev**.

| Tipo de serviço AEM | Domínio personalizado aceito? |
|---------------------|:-----------------------:|
| Autor | ✘ |
| Visualização | ✔ |
| Publicação | ✔ |

## Implementar nomes de domínio

Para implementar nomes de domínio usando CDN gerenciado por Adobe ou CDN gerenciado pelo cliente, o fluxograma a seguir o orienta pelo processo:

![Fluxograma de Gerenciamento de Nomes de Domínio](./assets/domain-name-management-flowchart.png){width="800" zoomable="yes"}

Além disso, a tabela a seguir orienta onde gerenciar as configurações específicas:

| Nome de domínio personalizado com | Adicionar certificado SSL a | Adicionar nome de domínio a | Configurar registros DNS em | É necessária a regra CDN de validação do cabeçalho HTTP? |
|---------------------|:-----------------------:|-----------------------:|-----------------------:|-----------------------:|
| CDN gerenciada por Adobe | Adobe Cloud Manager | Adobe Cloud Manager | Serviço de hospedagem DNS | ✘ |
| CDN gerenciada pelo cliente | Fornecedor de CDN | Fornecedor de CDN | Serviço de hospedagem DNS | ✔ |

### Tutoriais passo a passo

Agora que você entende o processo de gerenciamento de nomes de domínio, pode implementar nomes de domínio personalizados para seu site do AEM as a Cloud Service seguindo os tutoriais abaixo:

**[Nomes de domínio personalizados com CDN gerenciado por Adobe](./custom-domain-name-with-adobe-managed-cdn.md)**: neste tutorial, você aprenderá a adicionar um nome de domínio personalizado a um **site da AEM as a Cloud Service com CDN gerenciado por Adobe**.
**[Nomes de domínio personalizados com CDN gerenciada pelo cliente](./custom-domain-names-with-customer-managed-cdn.md)**: neste tutorial, você aprenderá a adicionar um nome de domínio personalizado a um **site da AEM as a Cloud Service com CDN gerenciada pelo cliente**.

