---
title: Servidores MCP no AEM
description: Saiba como usar os servidores AEM Model Context Protocol (MCP) do IDE alimentado por IA ou aplicativos baseados em chat preferidos para simplificar e acelerar o trabalho com conteúdo do AEM.
version: Experience Manager as a Cloud Service
role: Leader, User, Developer
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-03-04T00:00:00Z
jira: KT-20473
source-git-commit: c5f1c7f57181b1e9de6dd91aa2428f2fe1a04893
workflow-type: tm+mt
source-wordcount: '881'
ht-degree: 0%

---

# Servidores MCP no AEM

Saiba como usar os _Servidores MCP (Model Context Protocol)_ do AEM a partir de seus aplicativos IDE ou baseados em Chat preferidos alimentados por IA para simplificar e acelerar o trabalho com conteúdo do AEM. Você descreve o que deseja em uma linguagem natural, em vez de escrever código de API de baixo nível ou navegar pela interface do usuário do AEM.

## Lista de servidores MCP do AEM

Todos os Servidores MCP do AEM estão disponíveis em `https://mcp.adobeaemcloud.com/adobe/mcp/`. Consulte [Usando MCP com AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/using-mcp-with-aem-as-a-cloud-service) para obter mais informações.

- **Conteúdo** (`/content`) — Acesso total para criar, ler, atualizar e excluir páginas, fragmentos e ativos.
- **Conteúdo (somente leitura)** (`/content-readonly`) — Somente leitura para listar e obter páginas, fragmentos e ativos (sem alterações).
- **Cloud Manager** (`/cloudmanager`) — Para gerenciar programas, ambientes, repositórios e pipelines do Adobe Cloud Manager.

>[!TIP]
>
>As ferramentas que cada servidor expõe podem mudar com o tempo. Para ver o que está disponível agora, peça à IA para listar todas as ferramentas MCP do AEM (por exemplo, `List all AEM MCP tools available from this server and describe what they do`) ou digite o prompt `tools/list` no IDE.

## Padrões de Uso para o Servidor MCP

Antes de começar a usar os Servidores MCP AEM, vamos entender os dois principais padrões de uso para os Servidores MCP:

- **Centrado no ser humano** — Você está no lugar do condutor. Você pergunta, a IA sugere ou executa ferramentas no IDE.
- **Agente** — Um aplicativo agêntico (agente ou subagente) chama o servidor por conta própria, escolhendo ferramentas e trabalhando em uma meta com pouca intervenção humana.

Veja como esses dois padrões de uso se comparam:

| Aspecto | Centrado no ser humano | Agentic |
| ------ | ------------- | ------- |
| **Quem impulsiona as ações** | Você. <br> A IA sugere ou executa ferramentas para você no IDE ou no aplicativo baseado em Chat. | A IA. <br> Ele escolhe quais ferramentas utilizar e continua com orientação mínima. |
| **Autoridade de decisão** | Você fica no controle. Você aprova ou aciona cada etapa. | A IA tem mais liberdade. As ações de alto impacto podem precisar de medidas de proteção ou aprovações. |
| **Padrão de uso típico** | **Por desenvolvedor**, você o usa de seu próprio aplicativo baseado no IDE ou no Chat, um desenvolvedor por sessão, ideal para o trabalho diário de desenvolvimento. | **Compartilhado** por meio de um aplicativo de agente, como serviços compartilhados e gateways para muitos usuários ou agentes. |
| **Mais adequado para** | Revisar conteúdo, fazer atualizações guiadas, explorar ou repetir tarefas enquanto permanece no loop. | Fluxos de trabalho de agente, processos em lote, pipelines e metas em que o sistema deve ser executado com intervenção mínima. |

### Ao usar o MCP em sistemas de agentes

Os Servidores MCP foram projetados para **Clientes MCP operados por humanos** com UX interativo e supervisão humana. A especificação Ferramentas MCP recomenda _um humano no loop_ que possa aprovar ou negar invocações de ferramentas.

Se você usar Servidores MCP em um sistema agêntico ou autônomo, trate-o como um nível de compatibilidade separado. Não codifique **os nomes da ferramenta** em _prompts_, _incluis na lista de permissões_ ou _lógica de roteamento_. No MCP, o _nome da ferramenta_ é um identificador programático, a _descrição_ é a dica voltada para o modelo do LLM. Prefira o recurso ou a descrição com base no prompt e na seleção.

Implemente a descoberta em tempo de execução via `tools/list`, manipule alterações da lista de ferramentas (`notifications/tools/list_changed`) e alinhe-se com o provedor de servidor MCP na integração e no controle de versão se precisar de garantias de estabilidade além da linha de base do protocolo.

## Entidades MCP e seu mapeamento

O MCP é construído em torno de três entidades, **host**, **cliente** e **servidor**. A [especificação do MCP](https://modelcontextprotocol.io/docs/getting-started/intro) define-as formalmente. No entanto, a tabela abaixo explica cada um em termos simples e seu mapeamento ao usar os servidores MCP do AEM.

| Componente | Definição padrão | Ao usar servidores MCP do AEM |
| --------- | ------------------- | ---------------- |
| **Host** | O aplicativo que executa tudo, reúne o contexto, conversa com a IA, lida com permissões e cria clientes. | Seu **IDE** (Cursor) ou aplicativo baseado em Chat é o host. Ele executa o cliente MCP e decide quais ferramentas e servidores sua sessão pode usar. |
| **Cliente** | Uma única conexão do host para um servidor. Ele transmite mensagens para frente e para trás e mantém o acesso desse servidor separado de outros. | O **cliente MCP** reside em seu aplicativo baseado no IDE ou Chat. Quando você adiciona o servidor MCP de conteúdo do AEM nas configurações, o IDE ou aplicativo baseado em Chat cria um cliente que se comunica com esse servidor. Seus prompts e chamadas de ferramenta passam por este cliente. |
| **Servidor** | Um serviço que expõe ferramentas, dados e prompts no MCP. Ele pode ser executado na sua máquina ou remotamente. | Os **Servidores MCP AEM** hospedados pela Adobe oferecem ferramentas para criar, ler, atualizar e excluir páginas, fragmentos de conteúdo e ativos, de modo que a IA no IDE ou no aplicativo baseado em Chat possa funcionar com o ambiente do AEM. |

Simplificando, o **Host** é o seu aplicativo baseado no IDE ou no Chat, o **Cliente** é a conexão do IDE ou do aplicativo baseado no Chat com o AEM, o **Servidor** são os Servidores MCP do AEM hospedados pela Adobe que fazem o trabalho.

## Configurar

Os servidores MCP AEM foram projetados para funcionar com um conjunto definido de aplicativos compatíveis com MCP. Os seguintes aplicativos são oficialmente compatíveis:

- [Claude Antrópico](https://claude.com/product/overview)
- [Cursor](https://www.cursor.com/)
- [OpenAI ChatGPT](https://chatgpt.com/)
- [Microsoft Copilot Studio](https://www.microsoft.com/en-us/microsoft-365-copilot/microsoft-copilot-studio)

Consulte [Visão Geral da Instalação](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/using-mcp-with-aem-as-a-cloud-service#setup-overview) para obter mais informações.

## Casos de uso

<!-- CARDS
{target = _self}

* ./accelerate-content-operations-with-aem-mcp-server.md    
  {title = Accelerate Content Operations with AEM MCP Server}
  {description = Learn how to use the AEM Content MCP Server from Cursor IDE to streamline and accelerate your AEM content work.}
  {image = ../assets/content-mcp-server/update-adventure-price-prompt-response.png}
  {cta = Learn Content MCP Server}

* ./cloud-manager.md
  {title = Cloud Manager MCP Server}
  {description = Learn how to use the AEM Cloud Manager MCP Server from Cursor IDE to streamline and accelerate your AEM cloud manager work.}
  {image = ../assets/cm-mcp-server/start-pipeline.png}
  {cta = Learn Cloud Manager MCP Server}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Accelerate Content Operations with AEM MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./accelerate-content-operations-with-aem-mcp-server.md" title="Acelere as operações de conteúdo com o servidor MCP do AEM" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/content-mcp-server/update-adventure-price-prompt-response.png" alt="Acelere as operações de conteúdo com o servidor MCP do AEM"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" title="Acelere as operações de conteúdo com o servidor MCP do AEM">Acelerar as operações de conteúdo com o servidor MCP do AEM</a>
                    </p>
                    <p class="is-size-6">Saiba como usar o AEM Content MCP Server a partir do Cursor IDE para simplificar e acelerar seu trabalho de conteúdo no AEM.</p>
                </div>
                <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais sobre o Servidor MCP de Conteúdo</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud Manager MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./cloud-manager.md" title="Cloud Manager MCP Server" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/cm-mcp-server/start-pipeline.png" alt="Cloud Manager MCP Server"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./cloud-manager.md" target="_self" rel="referrer" title="Cloud Manager MCP Server">Cloud Manager MCP Server</a>
                    </p>
                    <p class="is-size-6">Saiba como usar o servidor MCP do AEM Cloud Manager no IDE de cursor para simplificar e acelerar seu trabalho no AEM Cloud Manager.</p>
                </div>
                <a href="./cloud-manager.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais sobre o Cloud Manager MCP Server</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
