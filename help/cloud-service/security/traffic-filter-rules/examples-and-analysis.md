---
title: Exemplos e análise de resultados das regras de filtro de tráfego, incluindo regras do WAF
description: Saiba mais sobre as várias regras de filtro de tráfego, incluindo exemplos de regras do WAF. Além disso, saiba como analisar os resultados por meio de logs de CDN do AEM as a Cloud Service (AEMCS).
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 49becbcb-7965-4378-bb8e-b662fda716b7
duration: 532
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1472'
ht-degree: 100%

---

# Exemplos e análise de resultados das regras de filtro de tráfego, incluindo regras do WAF

Saiba como declarar vários tipos de regra de filtro de tráfego e analisar os resultados por meio de logs de CDN e das ferramentas do painel do Adobe Experience Manager as a Cloud Service (AEMCS).

Nesta seção, você verá exemplos práticos de regras de filtro de tráfego, incluindo regras do WAF. Você aprenderá fazer registros em log, permitir e bloquear solicitações com base no URI (ou caminho), endereço IP, número de solicitações e diferentes tipos de ataque por meio do [Projeto do site da WKND no AEM](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

Além disso, você descobrirá como usar as ferramentas do painel de controle que assimilam logs de CDN do AEMCS para visualizar métricas essenciais por meio de painéis de amostra fornecidos pela Adobe.

Para satisfazer os seus requisitos específicos, você pode aprimorar e criar painéis personalizados, obtendo, assim, insights mais profundos e otimizando as configurações das regras para os seus sites do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3425404?quality=12&learn=on)

## Exemplos

Vamos explorar vários exemplos de regras de filtro de tráfego, incluindo regras do WAF. Certifique-se de ter concluído o processo de configuração necessário, conforme descrito no capítulo [Como configurar](./how-to-setup.md) anterior, e de ter clonado o [Projeto do site da WKND no AEM](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

### Solicitações de registro em log

Comece com as **solicitações de registro em log dos caminhos de logon e logoff da WKND** no serviço do AEM Publish.

- Adicione a regra a seguir ao arquivo `/config/cdn.yaml` do projeto da WKND.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    # On AEM Publish service log WKND Login and Logout requests
      - name: publish-auth-requests
        when:
          allOf:
            - reqProperty: tier
              matches: publish
            - reqProperty: path
              in:
                - /system/sling/login/j_security_check
                - /system/sling/logout
        action: log
```

- Confirme e envie as alterações ao repositório do Git do Cloud Manager.

- Implante as alterações no ambiente de desenvolvimento do AEM, usando o pipeline de configuração `Dev-Config` do Cloud Manager [criado anteriormente](how-to-setup.md#deploy-rules-through-cloud-manager).

  ![Pipeline de configuração do Cloud Manager](./assets/cloud-manager-config-pipeline.png)

- Teste a regra, fazendo logon e logoff do site da WKND do seu programa no serviço do Publish (por exemplo, `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`). Você pode usar `asmith/asmith` como nome de usuário e senha.

  ![Logon na WKND](./assets/wknd-login.png)

#### Análise{#analyzing}

Vamos analisar os resultados da regra `publish-auth-requests`, baixando os logs de CDN do AEMCS do Cloud Manager e usando as [ferramentas do painel](how-to-setup.md#analyze-results-using-elk-dashboard-tool), que você configurou no capítulo anterior.

- No cartão **Ambientes** do [Cloud Manager](https://my.cloudmanager.adobe.com/), baixe os logs de CDN do serviço AEMCS **Publish**.

  ![Downloads de logs da CDN do Cloud Manager](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    Pode levar até 5 minutos para que as novas solicitações apareçam nos logs da CDN.

- Copie o arquivo de log baixado (por exemplo, `publish_cdn_2023-10-24.log` na captura de tela abaixo) para a pasta `logs/dev` do projeto da ferramenta do painel “Elástico”.

  ![Pasta de logs da ferramenta ELK](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- Atualize a página da ferramenta do painel “Elástico”.
   - Na seção superior de **Filtro global**, edite o filtro `aem_env_name.keyword` e selecione o valor de ambiente `dev`.

     ![Filtro global da ferramenta ELK](./assets/elk-tool-global-filter.png)

   - Para alterar o intervalo de tempo, clique no ícone de calendário, no canto superior direito, e selecione o intervalo de tempo desejado.

     ![Intervalo de tempo da ferramenta ELK](./assets/elk-tool-time-interval.png)

- Analise os painéis **Solicitações analisadas**, **Solicitações sinalizadas** e **Detalhes das solicitações sinalizadas** do painel atualizado. Para entradas do log da CDN correspondentes, ele deve mostrar os valores de IP do cliente (cli_ip), host, URL, ação (waf_action) e nome da regra (waf_match) de cada entrada.

  ![Painel da ferramenta ELK](./assets/elk-tool-dashboard.png)


### Bloquear solicitações

Neste exemplo, vamos adicionar uma página em uma pasta _interna_ no caminho `/content/wknd/internal` no projeto da WKND implantado. Em seguida, declare uma regra de filtro de tráfego para **bloquear o tráfego** para subpáginas de qualquer lugar que não seja um endereço IP especificado que corresponda à sua organização (por exemplo, uma VPN corporativa).

Você pode criar a sua própria página interna (por exemplo, `demo-page.html`) ou usar o [pacote anexado](./assets/demo-internal-pages-package.zip).

- Adicione a seguinte regra no arquivo `/config/cdn.yaml` do projeto da WKND:

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...

    # Block requests to (demo) internal only page/s from public IP address but allow from internal IP address.
    # Make sure to replace the IP address with your own IP address.
      - name: block-internal-paths
        when:
          allOf:
            - reqProperty: path
              matches: /content/wknd/internal
            - reqProperty: clientIp
              notIn: [192.150.10.0/24]
        action: block
```

- Confirme e envie as alterações ao repositório do Git do Cloud Manager.

- Implante as alterações no ambiente de desenvolvimento do AEM, usando o pipeline de configuração [criado anteriormente](how-to-setup.md#deploy-rules-through-cloud-manager) `Dev-Config` no Cloud Manager.

- Teste a regra, acessando a página interna do site da WKND, como, por exemplo `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`, ou usando o comando CURL abaixo:

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Repita a etapa acima a partir do endereço IP usado na regra e de um endereço IP diferente (por exemplo, usando o seu celular).

#### Análise

Para analisar os resultados da regra `block-internal-paths`, siga as mesmas etapas descritas no [exemplo anterior](#analyzing).

No entanto, desta vez, você deve ver as **Solicitações bloqueadas** e os valores correspondentes nas colunas de IP do cliente (cli_ip), host, URL, ação (waf_action) e nome da regra (waf_match).

![Solicitação bloqueada do painel da ferramenta ELK](./assets/elk-tool-dashboard-blocked.png)


### Prevenir ataques de DoS

Vamos **prevenir ataques de DoS**, bloqueando solicitações de um endereço IP que envia 100 solicitações por segundo, fazendo com que ele seja bloqueado por 5 minutos.

- Adicione a seguinte [regra de filtro de tráfego de limite de taxa](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=pt-BR#ratelimit-structure) no arquivo `/config/cdn.yaml` do projeto da WKND.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...
    #  Prevent DoS attacks by blocking client for 5 minutes if they make more than 100 requests in 1 second.
      - name: prevent-dos-attacks
        when:
          reqProperty: path
          like: '*'
        rateLimit:
          limit: 100
          window: 1
          penalty: 300
          groupBy:
            - reqProperty: clientIp
        action: block
```

>[!WARNING]
>
>Para o seu ambiente de produção, colabore com a sua equipe de segurança da web para determinar os valores apropriados de `rateLimit`,

- Confirme, envie e implante as alterações conforme mencionado nos [exemplos anteriores](#logging-requests).

- Para simular o ataque de DoS, use o seguinte comando do [Vegeta](https://github.com/tsenart/vegeta).

  ```shell
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=60s | vegeta report
  ```

  Esse comando faz 120 solicitações por 5 segundos e gera um relatório. Como você pode ver, a taxa de sucesso é de 32,5%. Um código de resposta HTTP 406 é recebido para o restante, demonstrando que o tráfego foi bloqueado.

  ![Ataque de DoS do Vegeta](./assets/vegeta-dos-attack.png)

#### Análise

Para analisar os resultados da regra `prevent-dos-attacks`, siga as mesmas etapas descritas no [exemplo anterior](#analyzing).

Desta vez, você deve ver muitas **Solicitações bloqueadas** e os valores correspondentes nas colunas de IP do cliente (cli_ip), host, URL, ação (waf_action) e nome da regra (waf_match).

![Solicitação de DoS do painel da ferramenta ELK](./assets/elk-tool-dashboard-dos.png)

Além disso, os painéis de **Cem principais ataques por IP do cliente, país e agente usuário** mostram detalhes adicionais, que podem ser usados para otimizar ainda mais a configuração de regras.

![Cem principais solicitações de DoS do painel da ferramenta ELK](./assets/elk-tool-dashboard-dos-top-100.png)

Para mais informações sobre como prevenir ataques de DoS e DDoS, consulte o tutorial [Bloquear ataques de DoS e DDoS com regras de filtro de tráfego](../blocking-dos-attack-using-traffic-filter-rules.md).

### Regras do WAF

Os exemplos de regra de filtro de tráfego até o momento podem ser configurados por todos os clientes do Sites e do Forms.

A seguir, vamos explorar a experiência de um cliente que adquiriu uma licença de segurança aprimorada ou proteção WAF-DDoS, que permite configurar regras avançadas para proteger sites contra ataques mais sofisticados.

Antes de continuar, habilite a proteção WAF-DDoS para o seu programa, conforme descrito na documentação de regras de filtro de tráfego [etapas de configuração](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#setup).

#### Sem WAFFlags

Vamos ver a experiência antes mesmo de as regras do WAF serem declaradas. Quando o WAF-DDoS está habilitado no seu programa, a CDN registra, por padrão, quaisquer correspondências de tráfego mal-intencionado, para que você conte com as informações certas para criar as regras apropriadas.

Para começar, vamos atacar o site da WKND sem adicionar uma regra do WAF (ou usando a propriedade `wafFlags`) e analisar os resultados.

- Para simular um ataque, use o comando [Nikto](https://github.com/sullo/nikto) abaixo, que envia cerca de 700 solicitações mal-intencionadas em 6 minutos.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![Simulação de ataque do Nikto](./assets/nikto-attack.png)

  Para saber mais sobre a simulação de ataques, consulte a documentação [Nikto: sintonização da varredura](https://github.com/sullo/nikto/wiki/Scan-Tuning), que informa como especificar os tipos de ataque de teste a serem incluídos ou excluídos.

##### Análise

Para analisar os resultados da simulação de ataque, siga as mesmas etapas descritas no [exemplo anterior](#analyzing).

No entanto, desta vez, você deve ver as **Solicitações sinalizadas** e os valores correspondentes nas colunas de IP do cliente (cli_ip), host, URL, ação (waf_action) e nome da regra (waf_match). Essas informações permitem analisar os resultados e otimizar a configuração da regra.

![Solicitação sinalizada pelo WAF do painel da ferramenta ELK](./assets/elk-tool-dashboard-waf-flagged.png)

Observe como os painéis **Distribuição de sinalizadores do WAF** e **Principais ataques** mostram detalhes adicionais que podem ser usados para otimizar ainda mais a configuração da regra.

![Solicitação de ataques de sinalizadores do WAF do painel da ferramenta ELK](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![Principais solicitações de ataques do WAF do painel da ferramenta ELK](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### Com WAFFlags

Agora, vamos adicionar uma regra do WAF que contém a propriedade `wafFlags` como parte da propriedade `action` e **bloquear as solicitações simuladas de ataque**.

Da perspectiva da sintaxe, as regras do WAF são semelhantes às vistas anteriormente, mas a propriedade `action` faz referência a um ou mais valores `wafFlags`. Para saber mais sobre `wafFlags`, consulte a seção [Lista de sinalizadores do WAF](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=pt-BR#waf-flags-list).

- Adicione a seguinte regra no arquivo `/config/cdn.yaml` do projeto da WKND. Observe que a regra `block-waf-flags` inclui alguns dos wafFlags que apareciam nas ferramentas do painel quando atacados com tráfego mal-intencionado simulado. Na verdade, é uma boa prática ao longo do tempo analisar os logs para determinar quais novas regras devem ser declaradas, à medida que o cenário de ameaças evolui.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...
    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8
```

- Confirme, envie e implante alterações conforme mencionado nos [exemplos anteriores](#logging-requests).

- Para simular um ataque, use o mesmo comando [Nikto](https://github.com/sullo/nikto) de antes.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### Análise

Repita as mesmas etapas descritas no [exemplo anterior](#analyzing).

Desta vez, você deve ver entradas em **Solicitações bloqueadas** e os valores correspondentes nas colunas de IP do cliente (cli_ip), host, URL, ação (waf_action) e nome da regra (waf_match).

![Solicitação bloqueada pelo WAF do painel da ferramenta ELK](./assets/elk-tool-dashboard-waf-blocked.png)

Além disso, os painéis **Distribuição de sinalizadores do WAF** e **Principais ataques** mostram detalhes adicionais.

![Solicitação de ataques dos sinalizadores do WAF do painel da ferramenta ELK](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![Solicitação de ataques principais do WAF do painel da ferramenta ELK](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### Análise abrangente

Nas seções de _análise_ acima, você aprendeu a analisar os resultados de regras específicas com a ferramenta do painel. Você pode explorar ainda mais a análise dos resultados com outros painéis, incluindo:


- Solicitações analisadas, sinalizadas e bloqueadas
- Distribuição dos sinalizadores do WAF ao longo do tempo
- Regras de filtro de tráfego acionadas ao longo do tempo
- Principais ataques por ID do sinalizador do WAF
- Filtro de tráfego mais acionado
- Cem principais invasores por IP do cliente, país e agente usuário

![Análise abrangente do painel da ferramenta ELK](./assets/elk-tool-dashboard-comprehensive-analysis-1.png)

![Análise abrangente do painel da ferramenta ELK](./assets/elk-tool-dashboard-comprehensive-analysis-2.png)


## Próxima etapa

Familiarize-se com as [práticas recomendadas](./best-practices.md) para reduzir o risco de violações de segurança.

## Recursos adicionais

[Sintaxe das regras de filtro de tráfego](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=pt-BR#rules-syntax)

[Formato de log da CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=pt-BR#cdn-log-format)

