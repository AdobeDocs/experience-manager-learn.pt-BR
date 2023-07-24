---
title: Personalização em escala
description: Torne as experiências personalizadas parte de cada momento.
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---


# Personalização em escala

No cenário altamente competitivo e orientado por tecnologias digitais atual, os clientes acabaram esperando experiências personalizadas de acordo com suas preferências e necessidades exclusivas. Aproveitar os recursos do Adobe Experience Cloud nos permite coletar e analisar dados abrangentes do cliente, fornecendo insights inestimáveis sobre comportamentos, interesses e preferências. Essa compreensão profunda facilita a entrega de experiências personalizadas em vários pontos de contato, garantindo interações significativas e envolventes. Aproveitar o potencial do Adobe Experience Cloud libera todo o potencial de personalização, promovendo conexões mais fortes com o cliente, incentivando a fidelidade e impulsionando o crescimento dos negócios.

<table>
  <thead>
    <tr>
      <th>Caso de uso de integração</th>
      <th>Descrição da integração</th>
      <th>Exemplos</th>
      <th>aplicativos Experience Cloud</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Criar documentos personalizados do PDF</strong></td>
      <td>
        Gerar documentos de comunicação para assinatura com base nas seleções/preferências do usuário.
      </td>
      <td>
        <ul>
          <li>
            Apresentar um NDA gerado dinamicamente com base nos dados de um envio do AEM Forms para assinatura
          </li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/experience-manager/experience-manager-acrobat-sign.md"
          target="_blank"
          rel="noopener noreferrer"
          >AEM Forms e Sign</a
        >
      </td>
    </tr>
    <tr>
      <td rowspan="2"><strong>Análise de dados e relatórios</strong></td>
      <td>
        Analisar dados comportamentais de experiências digitais <br />Use os dados comportamentais do Adobe Analytics no Analysis Workspace no Customer Journey Analytics.
      </td>
      <td>
        <ul>
          <li>Analisar caminhos de conversão de cima/baixo</li>
          <li>Analisar o engajamento e a conversão do canal</li>
          <li>Entender o conteúdo visualizado principal</li>
          <li>Entender as principais categorias e produtos</li>
          <li>
            Realizar análise de uso da ferramenta para otimizar as experiências de autoatendimento
          </li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/analytics/analytics-customer-journey-analytics.md"
          target="_blank"
          rel="noopener noreferrer"
          >Analytics e Customer Journey Analytics</a
        >
      </td>
    </tr>
    <tr>
      <td>
        Relatórios para atividades de personalização<br />Analise os resultados do teste de otimização, incluindo o teste A/B, utilizando o Adobe Target e gerando relatórios abrangentes por meio do Adobe Analytics.
      </td>
      <td>
        <ul>
          <li>Mostrar resultados do teste A/B em relatórios de análise avançada</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/analytics/analytics-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >Analytics e Target</a
        >
      </td>
    </tr>
    <tr>
      <td><strong>Personalizar entregas de email</strong></td>
      <td>
        Personalize deliveries de email com conteúdo dinâmico aproveitando os recursos do Adobe Target.
      </td>
      <td>
        <ul>
          <li>Adicionar ofertas personalizadas a emails de clientes</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/campaign//campaign-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >Campaign e Target</a
        >
      </td>
    </tr>
    <tr>
      <td rowspan="2">
        <strong>Expandir públicos para plataformas de personalização e publicidade</strong>
      </td>
      <td>
        Use segmentos Audience Manager para criar públicos-alvo no Real-Time CDP para usar em táticas de personalização e remarketing.
      </td>
      <td>
        <ul>
          <li>
            Realizar direcionamento e personalização de público-alvo digital anônimo no site, no aplicativo móvel ou nos canais de publicidade compatíveis
          </li>
          <li>
            Otimizar experiências de página de aterrissagem e pré-autenticação com base em características de dispositivo e comportamento conhecidas
          </li>
          <li>
            Aproveite a rede de dados de terceiros do Audience Manager para refinar e expandir ainda mais seus públicos-alvo para direcionamento
          </li>
          <li>Compartilhar segmentos de Audience Manager para a RTCDP</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/aam/aam-rtcdp.md"
          target="_blank"
          rel="noopener noreferrer"
          >AUDIENCE MANAGER e REAL-TIME CUSTOMER DATA PLATFORM</a
        >
      </td>
    </tr>
    <tr>
      <td>
        Use os dados do Analytics para criar públicos-alvo que serão usados em táticas de personalização ou remarketing.
      </td>
      <td>
        <ul>
          <li>
            Realize o direcionamento e a personalização do público-alvo digital em dispositivos ou canais de publicidade compatíveis.
          </li>
          <li>
            Otimize páginas de aterrissagem conhecidas do cliente e experiências anônimas com base em atributos de dispositivo e de comportamento.
          </li>
          <li>Ative públicos para canais conhecidos, como email e SMS.</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/analytics/analytics-customer-journey-analytics.md"
          target="_blank"
          rel="noopener noreferrer"
          >Analytics e Real-time Customer Data Platform</a
        >
      </td>
    </tr>
    <tr>
      <td rowspan="2"><strong>Personalizar experiências da Web</strong></td>
      <td>
        Personalize experiências de aplicativos de página única (SPA) utilizando efetivamente o AEM Headless em conjunto com o Adobe Target.
      </td>
      <td>
        <ul>
          <li>Personalização de SPA e aplicativo móvel</li>
          <li>Respostas da API personalizadas.</li>
          <li>Entrega de conteúdo direcionada.</li>
          <li>Variações do teste A/B.</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/experience-manager/experience-manager-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >AEM Headless e Target</a
        >
      </td>
    </tr>
    <tr>
      <td>
        Ofereça experiências de site personalizadas utilizando efetivamente o AEM Sites e o Adobe Target para personalização.
      </td>
      <td>
        <ul>
          <li>Personalização do site do AEM.</li>
          <li>Personalizar o conteúdo do site.</li>
          <li>Otimize as experiências do usuário.</li>
          <li>Variações do teste A/B.</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/experience-manager/experience-manager-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >AEM Sites e Target</a
        >
      </td>
    </tr>
    <tr>
      <td><strong>Personalizar experiências digitais</strong></td>
      <td>
        Use Perfis de clientes em tempo real e segmentos da Platform gerenciados centralmente para personalizar mensagens na Web, dispositivos móveis e outros canais digitais
      </td>
      <td>
        <ul>
          <li>Personalização de conteúdo para visitantes conhecidos</li>
          <li>Aumentar a inscrição e a participação no programa de fidelidade</li>
          <li>Identificar e envolver clientes em risco de churn</li>
          <li>Personalização de oferta em tempo real</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/rtcdp/rtcdp-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >Real-time Customer Data Platform e Target</a
        >
      </td>
    </tr>
    <tr>
      <td><strong>Aprimorar a geração de clientes potenciais</strong></td>
      <td>
        Use Perfis de clientes em tempo real e segmentos da Platform gerenciados centralmente para personalizar mensagens na Web, dispositivos móveis e outros canais digitais
      </td>
      <td>
        <ul>
          <li>Personalização de conteúdo para visitantes conhecidos</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/rtcdp/rtcdp-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >Real-time Customer Data Platform e Target</a
        >
      </td>
    </tr>
  </tbody>
</table>
