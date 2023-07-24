---
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# Integrar o Adobe Analytics com o Customer Journey Analytics

{{analytics-description}}

{{customer-journey-analytics-description}}

A integração do Adobe Analytics com o Customer Journey Analytics oferece os principais benefícios:

+ **Insights abrangentes** em comportamentos e preferências do cliente.
+ **Rastreamento contínuo entre canais** para obter uma visão holística.
+ **Dados e relatórios unificados** para uma análise precisa.
+ **Personalização aprimorada** e maior engajamento do cliente.
+ **Insights de dados em tempo real** para uma tomada de decisões ágil.

## Integrações comuns

<table>
    <thead>
        <tr>
            <td>aplicativos Experience Cloud</td>
            <td>Integra-se usando o</td>
            <td>Quando usar</td>
            <td>Casos de uso comuns</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><a href="../../integrations/tutorials/analytics-customer-journey-analytics/experience-platform-source-connector.md" target="_blank" rel="noreferrer">Analytics com Customer Journey Analytics</a></td>
            <td>conector de origem do Experience Platform</td>
            <td>
                <ul>
                    <li>TODO: use essa integração para assimilar dados do Analytics no Experience Platform de seus conjuntos de relatórios.</li>
                    <li>Quando a disponibilidade dos dados para o Perfil do cliente puder estar entre 2 e 30 minutos a partir do momento da coleta de dados, e a disponibilidade para o Data Lake for de até 90 minutos.</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Fluxo de trabalho direto, iniciado pela interface do usuário.</li>
                    <li>Mapeamento da interface do usuário para copiar props e eVars do Analytics para novos campos XDM.</li>
                    <li>A maneira mais rápida de obter valor do Perfil do cliente em tempo real e do Customer Journey Analytics.</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://www.adobe.com/" target="_blank" rel="noreferrer">LINK PARA FAZER: Analytics com Customer Journey Analytics</a></td>
            <td>Experience Platform Edge</td>
            <td>
                <ul>
                    <li>Quando você deseja implementar uma estratégia de longo prazo. Isso envia dados diretamente de um dispositivo para o Experience Platform usando o SDK da Web da AEP, o SDK móvel da AEP ou a API do servidor da rede de borda.</li>
                    <li>Quando você é um cliente novo ou existente, que precisa da disponibilidade de dados do Analytics para o Perfil do cliente para oferecer suporte a casos de uso de personalização da mesma e da próxima página.</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Fornece o mais alto nível de controle para os dados coletados a serem usados no suporte aos seus casos de uso.</li>
                    <li>Os dados do lado do cliente são facilmente mapeados para campos XDM.</li>
                    <li>Disponibilidade de dados mais rápida para o Perfil do cliente em tempo real.</li>
                </ul>
            </td>
        </tr>  
    </tbody>          
</table>
