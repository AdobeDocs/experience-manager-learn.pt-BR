---
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 1%

---


# Integrar o Adobe Analytics com o Real-time Customer Data Platform

{{analytics-description}}

{{real-time-cdp-description}}

A integração do Adobe Analytics com o Adobe Real-time Customer Data Platform (RTCDP) pode oferecer vários benefícios para as empresas que buscam aprimorar as experiências dos clientes e os esforços de marketing. Estas são algumas das principais vantagens:

+ **Direcionamento e personalização aprimorados do público**: marketing preciso em dispositivos e canais, mensagens personalizadas para envolvimento otimizado.
+ **Otimização aprimorada da página de aterrissagem**: experiências personalizadas com base em dispositivo e comportamento, melhorando a satisfação e a conversão do usuário.
+ **Ativação contínua do público**: utilize perfis de clientes para um direcionamento eficaz por meio de canais preferenciais, fornecendo mensagens relevantes.

Ao combinar o Adobe Analytics e o Real-Time CDP, as empresas podem elevar seus esforços de marketing a um novo patamar, fornecendo experiências personalizadas, aumentando o engajamento do cliente e otimizando conversões em vários pontos de contato digitais.

<table>
    <thead>
        <tr>
            <th>aplicativos Experience Cloud</th>
            <th>Integra-se usando o</th>
            <th>Quando usar</th>
            <th>Casos de uso comuns</th>
        </tr>
    </thead>
    <tr>
        <td><a href="../../integrations/tutorials/analytics-real-time-cdp/experience-platform-source-connector.md" target="_blank" rel="noreferrer">Analytics com Real-Time CDP</a></td>
        <td>conector de origem do Experience Platform</td>
        <td>
            <ul>
                <li>Quando você deseja assimilar dados do Analytics no Experience Platform a partir de seus conjuntos de relatórios.</li>
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
        <td><a href="https://adobe.com" target="_blank" rel="noreferrer">TODO: Analytics com Real-Time CDP</a></td>
        <td>Experience Platform Edge</td>
        <td>
            <ul>
                <li>Ao implementar uma estratégia de análise de longo prazo.</li>
                <li>Quando você é um novo cliente ou um cliente existente que precisa da disponibilidade de dados do Analytics para o Perfil do cliente para suportar casos de uso de personalização da mesma e da próxima página.</li>
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
</table>
