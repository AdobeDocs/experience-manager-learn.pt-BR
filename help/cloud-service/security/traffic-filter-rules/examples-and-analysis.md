---
title: Exemplos e análise de resultados de regras de Filtro de tráfego incluindo regras WAF
description: Saiba mais sobre as várias regras de Filtro de tráfego, incluindo exemplos de regras do WAF. Além disso, veja como analisar os resultados usando logs de CDN do AEM as a Cloud Service (AEMCS).
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 49becbcb-7965-4378-bb8e-b662fda716b7
source-git-commit: c32497a7fdcf144d30bb8c0e58527013b66013b0
workflow-type: tm+mt
source-wordcount: '1512'
ht-degree: 0%

---

# Exemplos e análise de resultados de regras de filtro de tráfego, incluindo regras WAF

Saiba como declarar vários tipos de regras de filtro de tráfego e analisar os resultados usando logs de CDN e ferramentas de painel do Adobe Experience Manager as a Cloud Service (AEMCS).

Nesta seção, você explorará exemplos práticos de regras de filtro de tráfego, incluindo regras WAF. Você aprenderá a registrar, permitir e bloquear solicitações com base no URI (ou caminho), endereço IP, o número de solicitações e diferentes tipos de ataque usando o [Projeto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

Além disso, você descobrirá como usar ferramentas de painel que assimilam logs de CDN do AEM CS para visualizar métricas essenciais por meio de painéis de amostra fornecidos pelo Adobe.

Para se alinhar aos seus requisitos específicos, você pode aprimorar e criar painéis personalizados, obtendo assim insights mais profundos e otimizando as configurações de regras para seus sites de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3425404?quality=12&learn=on)

## Exemplos

Vamos explorar vários exemplos de regras de filtro de tráfego, incluindo regras WAF. Verifique se você concluiu o processo de configuração necessário, conforme descrito na seção anterior [como configurar](./how-to-setup.md) capítulo e que você clonou o [Projeto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

### Registrando solicitações

Começar por **solicitações de logon dos caminhos de logon e logout da WKND** contra o serviço AEM Publish.

- Adicione a seguinte regra ao do projeto WKND `/config/cdn.yaml` arquivo.

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

- Confirme e envie as alterações para o repositório Git do Cloud Manager.

- Implante as alterações no ambiente de desenvolvimento do AEM usando o Cloud Manager `Dev-Config` pipeline de configuração [criado anteriormente](how-to-setup.md#deploy-rules-through-cloud-manager).

  ![Pipeline de configuração do Cloud Manager](./assets/cloud-manager-config-pipeline.png)

- Teste a regra fazendo logon e logout do site WKND do programa no serviço de Publicação (por exemplo, `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`). Você pode usar `asmith/asmith` como nome de usuário e senha.

  ![Logon do WKND](./assets/wknd-login.png)

#### Analisando{#analyzing}

Vamos analisar os resultados da `publish-auth-requests` regra ao baixar os logs de CDN do AEM CS no Cloud Manager e usar o [ferramentas do painel](how-to-setup.md#analyze-results-using-elk-dashboard-tool), que você configurou no capítulo anterior.

- De [Cloud Manager](https://my.cloudmanager.adobe.com/)do **Ambientes** , baixe o AEMCS **Publish** logs de CDN do serviço.

  ![Downloads de logs do CDN do Cloud Manager](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    Pode levar até 5 minutos para que as novas solicitações apareçam nos logs de CDN.

- Copie o arquivo de log baixado (por exemplo, `publish_cdn_2023-10-24.log` na captura de tela abaixo) na janela `logs/dev` pasta do projeto da ferramenta Elastic dashboard.

  ![Pasta de registros da ferramenta ELK](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- Atualize a página da ferramenta Elastic dashboard.
   - No topo **Filtro global** editar a `aem_env_name.keyword` e selecione o `dev` ambiente.

     ![Filtro global da ferramenta ELK](./assets/elk-tool-global-filter.png)

   - Para alterar o intervalo de tempo, clique no ícone de calendário no canto superior direito e selecione o intervalo de tempo desejado.

     ![Intervalo de tempo da ferramenta ELK](./assets/elk-tool-time-interval.png)

- Revise o do painel atualizado  **Solicitações analisadas**, **Solicitações sinalizadas**, e **Detalhes das solicitações sinalizadas** painéis. Para entradas de log CDN correspondentes, ele deve mostrar os valores de IP do cliente (cli_ip), host, url, ação (waf_action) e nome da regra (waf_match) de cada entrada.

  ![Painel de ferramentas ELK](./assets/elk-tool-dashboard.png)


### Bloquear solicitações

Neste exemplo, vamos adicionar uma página em uma _interno_ pasta no caminho `/content/wknd/internal` no projeto WKND implantado. Em seguida, declare uma regra de filtro de tráfego que **bloqueia o tráfego** para subpáginas de qualquer lugar diferente de um endereço IP especificado que corresponda à sua organização (por exemplo, uma VPN corporativa).

Você pode criar sua própria página interna (por exemplo, `demo-page.html`) ou use o [pacote anexado](./assets/demo-internal-pages-package.zip).

- Adicione a seguinte regra no do projeto WKND `/config/cdn.yaml` arquivo:

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

- Confirme e envie as alterações para o repositório Git do Cloud Manager.

- Implante as alterações no ambiente de desenvolvimento do AEM usando o [criado anteriormente](how-to-setup.md#deploy-rules-through-cloud-manager) `Dev-Config` pipeline de configuração no Cloud Manager.

- Teste a regra acessando a página interna do site WKND, por exemplo `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` ou usando o comando CURL abaixo:

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Repita a etapa acima a partir do endereço IP usado na regra e de um endereço IP diferente (por exemplo, usando seu celular).

#### Analisando

Para analisar os resultados do `block-internal-paths` siga as mesmas etapas descritas na seção [exemplo anterior](#analyzing).

No entanto, desta vez, você deverá ver o **Solicitações bloqueadas** e os valores correspondentes nas colunas client IP (cli_ip), host, URL, action (waf_action) e rule-name (waf_match).

![Solicitação de painel de ferramentas ELK bloqueada](./assets/elk-tool-dashboard-blocked.png)


### Impedir ataques de DoS

Vamos **impedir ataques de DoS** bloqueando solicitações de um endereço IP que faz 100 solicitações por segundo, fazendo com que ele seja bloqueado por 5 minutos.

- Adicione o seguinte [regra de filtro de tráfego de limite de taxa](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#ratelimit-structure) no do projeto WKND `/config/cdn.yaml` arquivo.

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
>Para seu ambiente de produção, colabore com sua equipe de Segurança da Web para determinar os valores apropriados para `rateLimit`,

- Confirmar, enviar e implantar alterações, conforme mencionado na [exemplos anteriores](#logging-requests).

- Para simular o ataque de DoS, use o seguinte [Vegeta](https://github.com/tsenart/vegeta) comando.

  ```shell
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=5s | vegeta report
  ```

  Esse comando faz 120 solicitações por 5 segundos e gera um relatório. Como você pode ver, a taxa de sucesso é de 32,5%; um código de resposta HTTP 406 é recebido para o restante, demonstrando que o tráfego foi bloqueado.

  ![Ataque De DoS Vegeta](./assets/vegeta-dos-attack.png)

#### Analisando

Para analisar os resultados do `prevent-dos-attacks` siga as mesmas etapas descritas na seção [exemplo anterior](#analyzing).

Desta vez, você deverá ver muitos **Solicitações bloqueadas** e os valores correspondentes nas colunas client IP (cli_ip), host, url, action (waf_action) e rule-name (waf_match).

![Solicitação de DoS do painel da ferramenta ELK](./assets/elk-tool-dashboard-dos.png)

Além disso, a variável **Os 100 principais ataques por IP do cliente, país e agente do usuário** Os painéis do mostram detalhes adicionais, que podem ser usados para otimizar ainda mais a configuração de regras.

![As 100 principais solicitações do DoS do painel da ferramenta ELK](./assets/elk-tool-dashboard-dos-top-100.png)

### Regras do WAF

Os exemplos de regra de filtro de tráfego até o momento podem ser configurados por todos os clientes do Sites e do Forms.

A seguir, vamos explorar a experiência de um cliente que adquiriu uma licença Enhanced Security ou WAF-DDoS Protection, que permite configurar regras avançadas para proteger sites de ataques mais sofisticados.

Antes de continuar, ative a Proteção WAF-DDoS para o seu programa, conforme descrito na documentação das regras de filtro de tráfego [etapas de configuração](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en#setup).

#### Sem WAFFlags

Vamos ver a experiência antes mesmo de as Regras do WAF serem declaradas. Quando o WAF-DDoS está ativado em seu programa, os registros da CDN registram, por padrão, quaisquer correspondências de tráfego mal-intencionado, para que você tenha as informações corretas para criar as regras apropriadas.

Vamos começar atacando o site WKND sem adicionar uma regra WAF (ou usar o `wafFlags` propriedade ) e analisar os resultados.

- Para simular um ataque, use o [Nikto](https://github.com/sullo/nikto) abaixo, que envia cerca de 700 solicitações mal-intencionadas em 6 minutos.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![Simulação de Ataque Nikto](./assets/nikto-attack.png)

  Para saber mais sobre a simulação de ataques, reveja o [Nikto - Ajuste de Digitalização](https://github.com/sullo/nikto/wiki/Scan-Tuning) documentação, que informa como especificar o tipo de ataques de teste a serem incluídos ou excluídos.

##### Analisando

Para analisar os resultados da simulação de ataque, siga as mesmas etapas descritas em [exemplo anterior](#analyzing).

No entanto, desta vez, você deverá ver o **Solicitações sinalizadas** e os valores correspondentes nas colunas client IP (cli_ip), host, url, action (waf_action) e rule-name (waf_match). Essas informações permitem analisar os resultados e otimizar a configuração da regra.

![Solicitação sinalizada WAF do painel de ferramentas ELK](./assets/elk-tool-dashboard-waf-flagged.png)

Observe como **Distribuição de sinalizadores do WAF** e **Principais ataques** Os painéis do mostram detalhes adicionais, que podem ser usados para otimizar ainda mais a configuração da regra.

![Solicitação de Ataques de Sinalizadores WAF do Painel de Ferramentas ELK](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![Solicitação de Ataques principais de WAF do painel de ferramentas ELK](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### Com WAFFlags

Agora vamos adicionar uma regra WAF que contém `wafFlags` propriedade como parte da `action` propriedade e **bloquear as solicitações de ataque simuladas**.

De uma perspectiva de sintaxe, as regras do WAF são semelhantes às vistas anteriormente, no entanto, as `action` a propriedade faz referência a um ou mais `wafFlags` valores. Para saber mais sobre o `wafFlags`, revise a [Lista de Sinalizadores do WAF](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#waf-flags-list) seção.

- Adicione a seguinte regra no do projeto WKND `/config/cdn.yaml` arquivo. Observe como `block-waf-flags` A regra inclui alguns dos wafFlags que apareceram nas ferramentas de painel quando atacados com tráfego mal-intencionado simulado. Na verdade, é uma boa prática ao longo do tempo analisar registros para determinar quais novas regras devem ser declaradas, à medida que o cenário de ameaças evolui.

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

- Confirmar, enviar e implantar alterações, conforme mencionado na [exemplos anteriores](#logging-requests).

- Para simular um ataque, use o mesmo [Nikto](https://github.com/sullo/nikto) como antes.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### Analisando

Repita as mesmas etapas descritas no [exemplo anterior](#analyzing).

Desta vez, você deve ver entradas em **Solicitações bloqueadas** e os valores correspondentes nas colunas client IP (cli_ip), host, url, action (waf_action) e rule-name (waf_match).

![Solicitação de WAF bloqueada do painel da ferramenta ELK](./assets/elk-tool-dashboard-waf-blocked.png)

Além disso, a variável **Distribuição de sinalizadores do WAF** e **Principais ataques** Os painéis do mostram detalhes adicionais.

![Solicitação de Ataques de Sinalizadores WAF do Painel de Ferramentas ELK](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![Solicitação de Ataques principais de WAF do painel de ferramentas ELK](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### Análise abrangente

No quadro acima _análise_ seções, você aprendeu a analisar os resultados de regras específicas usando a ferramenta dashboard. Você pode explorar ainda mais a análise dos resultados usando outros painéis de painel, incluindo:


- Solicitações analisadas, sinalizadas e bloqueadas
- Distribuição de sinalizadores do WAF ao longo do tempo
- Regras de filtro de tráfego acionadas ao longo do tempo
- Principais ataques por ID de sinalizador do WAF
- Filtro de tráfego mais acionado
- Os 100 principais invasores por IP de cliente, país e agente-usuário

![Análise abrangente do painel da ferramenta ELK](./assets/elk-tool-dashboard-comprehensive-analysis-1.png)

![Análise abrangente do painel da ferramenta ELK](./assets/elk-tool-dashboard-comprehensive-analysis-2.png)


## Próxima etapa

Familiarize-se com o recomendado [práticas recomendadas](./best-practices.md) reduzir o risco de violações da segurança.

## Recursos adicionais

[Sintaxe das regras de filtro de tráfego](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#rules-syntax)

[Formato de Log da CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#cdn-log-format)
