---
title: Adobe CDN - Recursos avançados além do armazenamento em cache
description: Saiba mais sobre os recursos avançados do Adobe CDN além do armazenamento em cache, como configuração de tráfego no CDN, configuração de tokens e credenciais, páginas de erro de CDN e muito mais.
version: Experience Manager as a Cloud Service
feature: Website Performance, CDN Cache
topic: Architecture, Performance, Content Management
role: Developer, User, Leader
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-08-21T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
exl-id: 8948a900-01e9-49ed-9ce5-3a057f5077e4
source-git-commit: 7b29187ef84bebebd4586374abb09ced947dff28
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 0%

---

# Adobe CDN - Recursos avançados além do armazenamento em cache

Saiba mais sobre os recursos avançados da Rede de entrega de conteúdo (CDN) da Adobe além do armazenamento em cache, como configuração de tráfego na CDN, configuração de tokens e credenciais, páginas de erro de CDN e muito mais.

Além do armazenamento em cache do conteúdo, o Adobe CDN oferece vários recursos avançados que podem ajudar a otimizar o desempenho do seu site. Esses recursos incluem:

- Configuração do tráfego no CDN
- Configuração de credenciais e autenticação da CDN
- Páginas de erro da CDN

Estes recursos são do **autoatendimento**. Configurado no arquivo `cdn.yaml` do seu projeto do AEM e implantado usando o pipeline de configuração do Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3433104?quality=12&learn=on)

## Configuração do tráfego no CDN

Vamos entender os principais recursos relacionados à _Configuração do tráfego na CDN_:

- **Prevenção de ataques de DoS:** o Adobe CDN absorve ataques de DoS na camada de rede, impedindo-os de alcançar seu servidor de origem.
- **Limitação de taxa:** para proteger seu servidor de origem de ser sobrecarregado com muitas solicitações, você pode configurar a limitação de taxa no CDN.
- **Firewall do Aplicativo Web (WAF):** O WAF protege seu site contra vulnerabilidades comuns de aplicativos Web, como injeção de SQL, script entre sites e muito mais. Para usar esse recurso, é necessária a licença de Segurança estendida (antes chamada de Proteção WAF-DDoS) ou Segurança estendida para a área de saúde (antes chamada de Segurança aprimorada).
- **Transformação de solicitação:** modifique solicitações de entrada, como definir ou remover cabeçalhos, modificar parâmetros de consulta, cookies e muito mais.
- **Transformação de resposta:** modifique as respostas de saída, como definir ou remover cabeçalhos.
- **Seleção de origem:** roteia o tráfego para servidores de origem diferentes (Adobe e não Adobe) com base na URL da solicitação.
- **Redirecionamento de URL:** Redirecione solicitações (HTTP 301/302) para uma URL absoluta ou relativa diferente.

## Configuração de credenciais e autenticação da CDN

Vamos entender os principais recursos relacionados a _Configuração de credenciais e autenticação de CDN_:

- **Token da API de Limpeza**: permite que você crie sua própria chave de limpeza para limpar um único grupo ou todos os recursos do cache.
- **Autenticação Básica**: um mecanismo de autenticação leve quando você deseja restringir o acesso ao seu site ou a uma parte dele. Necessário principalmente como parte de vários processos de revisão antes de entrar em funcionamento.
- **Validação do cabeçalho HTTP**: usada quando uma CDN gerenciada pelo cliente está roteando o tráfego para a CDN da Adobe. A CDN do Adobe valida a solicitação de entrada com base no valor do cabeçalho `X-AEM-Edge-Key`. Permite criar seu próprio valor para o cabeçalho `X-AEM-Edge-Key`.

## Páginas de erro da CDN

Vamos entender os principais recursos relacionados às _páginas de erro da CDN_:

- **Páginas de erro com marca**: exiba uma página de erro com marca para seus usuários no _cenário improvável_ quando a CDN da Adobe não conseguir acessar seu servidor de origem.

## Como implementar o

A implementação desses recursos avançados envolve duas etapas:

1. **Atualizar arquivo de configuração da CDN**: atualize o arquivo `cdn.yaml` em seu projeto do AEM com as configurações necessárias. As configurações são adicionadas como regras e seguem uma sintaxe de regra. A regra tem três componentes principais: `name`, `when` e `action`.

2. **Implantar arquivo de configuração CDN**: implante o arquivo `cdn.yaml` atualizado usando o pipeline de configuração do Cloud Manager. Para obter mais informações, consulte [Implantar regras por meio do Cloud Manager](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).

### Exemplo

No exemplo abaixo, o site WKND de exemplo está configurado para redirecionar a URL `/top3` para `/us/en/top3.html`.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  redirects:
    rules:
      - name: redirect-top3-adventures
        when: { reqProperty: path, equals: "/top3" }
        action:
          type: redirect
          status: 302
          location: /us/en/top3.html
```

## Tutoriais relacionados

[Protegendo sites com regras de filtro de tráfego](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/overview)

[Configurar e implantar a regra CDN de validação do Cabeçalho HTTP](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names-with-customer-managed-cdn#configure-and-deploy-http-header-validation-cdn-rule)

[Como limpar o cache da CDN](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/how-to/purge-cache)

[Configurando Páginas de Erro da CDN](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-error-pages#cdn-error-pages)

[Configurando o tráfego na CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors)

[Configurando Credenciais e Autenticação da CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-credentials-authentication)

