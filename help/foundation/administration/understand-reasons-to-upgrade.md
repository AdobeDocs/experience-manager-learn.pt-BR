---
title: Entenda os motivos para atualizar
description: Um detalhamento de alto nível dos principais recursos para clientes que consideram atualizar para a versão mais recente do Adobe Experience Manager.
version: 6.5
sub-product: ativos, gerenciador de nuvem, comércio, serviços de conteúdo, mídia dinâmica, formulários, fundação, telas, sites
topics: best-practices, upgrade
audience: all
activity: understand
doc-type: article
translation-type: tm+mt
source-git-commit: 1519856731758ece2860615c06fc0d64edb104a5
workflow-type: tm+mt
source-wordcount: '3540'
ht-degree: 2%

---


# Entendendo os motivos para atualizar

Um detalhamento de alto nível dos principais recursos para clientes que consideram atualizar para a versão mais recente do Adobe Experience Manager.

## Principais recursos para atualizar para o AEM 6.5

+ [Notas de versão do Adobe Experience Manager 6.5](https://helpx.adobe.com/experience-manager/6-5/release-notes.html)

### Melhorias no alicerce

A Adobe Experience Manager 6.5 continua a melhorar a estabilidade, o desempenho e a capacidade de suporte do sistema através de:

+ **Suporte ao Java 11** (mantendo o suporte ao Java 8).

### Criação e gerenciamento de sites

A AEM Sites apresenta vários recursos projetados para acelerar a criação e o desenvolvimento de sites:

+ **O suporte ao Editor** SPA permite que o SPA (aplicativos de página única) seja totalmente criado no AEM, com suporte para uma experiência de criação avançada e amigável para o profissional de marketing.
+_ **JavaScript SDKs**, um Kit de Start do SPA Project e ferramentas de criação de suporte, permitem que desenvolvedores front-end desenvolvam aplicativos de página única compatíveis com o Editor SPA independentemente da AEM.
+ **Os Componentes** principais adicionam uma grande variedade de novos componentes, uma Biblioteca **de** componentes e diversas melhorias aos Componentes principais existentes.
+ Outras **traduções** otimizam a tradução do AEM Sites.

### Experiências fluidas

AEM continua a abraçar as Experiências Fluidas com ferramentas novas e melhoradas que facilitam o uso de conteúdo fora da AEM.

+ **Fragmentos** de conteúdo oferecem suporte para Comparação/Diferença de versão e Anotações.
+ **AEM Assets HTTP API** suporta a exposição de Fragmentos **de** conteúdo diretamente no DAM como **JSON**.
   **Fragmentos** de experiência oferecem suporte à Pesquisa **em texto** completo e à Invalidação **do Cache do Dispatcher** AEM referência a **Páginas**.

### Gerenciamento de ativos

A AEM Assets continua desenvolvendo seu avançado conjunto de recursos de gerenciamento de ativos para melhorar o uso, o gerenciamento e a compreensão do DAM. AEM 6.5 continua a melhorar a integração entre a Adobe Creative Cloud e workflows criativos.

+ **O Adobe Asset Link** conecta criativos diretamente ao AEM Assets a partir das ferramentas Adobe Creative Cloud.
+ **A integração com o Adobe Stock** permite acesso direto às imagens do Adobe Stock diretamente da experiência da AEM Assets, criando uma experiência contínua de descoberta de conteúdo.
+ **O aplicativo** para desktop AEM versão 2.0 e nova visualização ao mesmo tempo em que melhora o desempenho e a estabilidade.
+ **Os ativos** conectados oferecem suporte a instâncias discretas do AEM Sites para acessar e usar perfeitamente os ativos de uma instância diferente do AEM Assets.
+ Atualização do suporte a vídeo no **Dynamic Media**, incluindo **360 Video** e **Custom Video Thumbnails**.

### Inteligência de conteúdo

AEM continua a construir sua integração com tecnologias inteligentes, aproveitando o aprendizado de máquinas e inteligência artificial para melhorar todas as experiências.

+ **O Link** do ativo Adobe adiciona Pesquisa **de semelhança** visual, permitindo que imagens semelhantes sejam facilmente descobertas e usadas dentro das ferramentas **do** Adobe Creative Cloud.

### Integrações

AEM aumenta sua capacidade de integração com outros serviços de Adobe:

+ **Os Fragmentos** de experiência aprofundam sua integração com o **Adobe Target** ao suportar **Exportar como JSON** para Adobe Target e a capacidade de **excluir ofertas** baseadas em Fragmentos de experiência da **Adobe Target**.

### Gerenciador da AMS Cloud

[O Cloud Manager](https://adobe.ly/2HODmsv), exclusivo para clientes do Adobe Managed Services (AMS), oferta os seguintes recursos:

+ O Cloud Manager oferece suporte AEM implantação da AEM Sites para a **AEM Assets**, incluindo testes de desempenho **automatizados do processamento** de ativos.
+ **O dimensionamento** automático da camada de publicação de AEM em limites predefinidos garante uma experiência ideal para o usuário final.
+ **Os pipelines** de não produção permitem que as equipes de desenvolvimento aproveitem o Cloud Manager para verificar continuamente a qualidade do código e implantar em ambientes menores (Desenvolvimento e QA).
+ **As APIs** de pipeline de CI/CD permitem que os clientes interajam programaticamente com o Cloud Manager, aprofundando as possibilidades de integração com a infraestrutura de desenvolvimento local.

## Recursos básicos

Abaixo está uma matriz dos principais recursos básicos oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias incrementais de versões anteriores adicionadas em cada versão.

+ [Notas de versão do AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup>melhorias significativas no recurso nesta versão.***

***✔<sup>SP</sup>indica que o recurso está disponível via Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso básico</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <strong>Suporte para Java 11:</strong> AEM suporta Java 11 (bem como Java 8).
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Repositório</a>de conteúdo Oak:</strong> Oferece desempenho e escalabilidade muito maiores do que o antecessor Jackrabbit 2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">suporte</a>a índice oak-run.jar:</strong> Melhoria na reindexação/coleta de estatísticas e verificação de consistência de índices Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Índices</a>de pesquisa personalizados: </strong>
                Capacidade de adicionar definições de índice personalizadas para otimizar o desempenho do query e a relevância da pesquisa.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Limpeza</a>de revisão online:</strong>
                Execute a manutenção do repositório sem tempo de inatividade do servidor.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Armazenamento</a>do repositório TarMK ou MongoMK:</strong>
                <br> Opções para usar armazenamentos simples e eficientes baseados em arquivos do TarMK (versão da próxima geração do TarPM) <br> ou dimensionar horizontalmente com um repositório com backup MongoDB com MongoMK.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Desempenho e estabilidade</a>do MongoMK:</strong>
            Foram feitos aprimoramentos contínuos ao MongoMK desde a sua introdução com o AEM 6.0.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>:</strong>
            Aproveite a solução de armazenamento em nuvem expansível para armazenar ativos binários.</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Paridade do recurso da interface de toque:</strong>
                Aprimoramentos contínuos na interface de criação para obter velocidade com maior produtividade e paridade de recursos com a interface clássica.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnisearch:</strong>
                Pesquise e navegue rapidamente AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Painel</a>de operações:</strong>
 Execute manutenção, monitore a integridade do servidor e analise o desempenho de dentro do AEM.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Melhorias</a>na atualização:</strong>
            As melhorias de atualização permitem atualizações de AEM mais fáceis e rápidas.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/htl/using/overview.html" target="_blank">Idioma</a>do modelo HTL:</strong>
            Um mecanismo moderno de modelagem que separa a apresentação da lógica. Reduz significativamente o tempo de desenvolvimento do componente. Recursos incrementais adicionados a cada versão.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Modelos</a>Sling:</strong>
            Uma estrutura flexível para modelar os recursos do JCR em objetos de negócios e lógica. Recursos incrementais adicionados a cada versão.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Gerenciador</a>de nuvem: </strong>
                Exclusivo para clientes do Adobe Managed Services (AMS), o Cloud Manager acelera o desenvolvimento e a implantação por meio de um pipeline de CI/CD avançado.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Recursos de segurança

Abaixo está uma matriz dos principais recursos de segurança oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias incrementais de versões anteriores adicionadas em cada versão.

+ [Notas de versão de segurança](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***Isso denota melhorias significativas no recurso nesta versão.***

***✔<sup>+</sup>indica que o recurso está disponível por meio de um Service Pack ou de um Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso de segurança</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Usuários</a></strong><br> do serviçoCompartimentaliza permissões para evitar o uso desnecessário dos privilégios de administrador.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Gerenciamento</a></strong>de armazenamento de chave <br> Armazenamento de confiança global, certificados e chaves todos gerenciados no repositório.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>Proteção</strong>CSRF<strong></strong></a>proteção <br> contra solicitações entre sites proteção imediata.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>O CORS</strong><strong>oferece suporte</strong></a><br> ao Compartilhamento de recursos entre Origens para maior flexibilidade do aplicativo.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/security/saml-2-0-authenticationhandler.html" target="_blank">Suporte</a><br>de autenticação SAML aprimorado Redirecionamento SAML </strong>aprimorado, informações de grupo otimizadas e problemas de criptografia de chave resolvidos.
            <br>
        </td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP como uma configuração</a><br>OSGi </strong>Simplifica o gerenciamento e as atualizações da autenticação LDAP.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>O suporte à Criptografia OSGi para senhas<br>de texto simples e </strong>senhas e outros valores confidenciais podem ser salvos em forma criptografada e descriptografados automaticamente.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">As melhorias</a><br>de CUG na implementação do Grupo de usuários </strong>fechados foram reescritas para resolver problemas de desempenho e escalabilidade.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">Assistente</a></strong><br> SSL para simplificar a configuração e o gerenciamento do SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Suporte</a></strong>a token encapsulado <br> Não é mais necessário para sessões "aderentes" para suportar a autenticação horizontal em instâncias de publicação.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Suporte</a><br>de autenticação do Adobe IMS </strong>exclusivo para serviços gerenciados da Adobe (AMS), gerencie centralmente o acesso às instâncias de autor do AEM por meio do Adobe IMS (Identity Management System).</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
    </tr>
</tbody>
</table>

## Recursos do Sites

Abaixo está uma matriz dos principais recursos do Sites oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias incrementais de versões anteriores adicionadas em cada versão.

+ [Notas de versão do AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup>melhorias significativas no recurso nesta versão.***

***✔<sup>SP</sup>indica que o recurso está disponível via Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td><strong>Recurso Sites</strong></td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Criação</a>de página otimizada ao toque:</strong>
            Permite que editores aproveitem tablets e computadores com telas de toque.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Criação</a>responsiva de site:</strong>
                O modo de layout permite que os editores redimensionem componentes com base nas larguras do dispositivo para sites responsivos.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Modelos</a>editáveis:</strong>
            Permitir que autores especializados criem e editem modelos de página.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">Componentes</a>principais:</strong>
            Acelere o desenvolvimento do site. Disponível no GitHub para agendamento e flexibilidade de versões frequentes.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">Editor</a>SPA:</strong>
            Crie experiências criáveis e interessantes da Web usando estruturas SPA (Single Page Application) criadas em React ou Angular.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">Sistema</a>de estilo:</strong>
            Aumente AEM reutilização do componente definindo sua aparência visual usando o sistema de estilo no contexto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">MSM (Multi-Site Manager)</a>:</strong>
            Gerencie vários sites que compartilham conteúdo comum (ou seja, várias marcas multilíngues).</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Tradução</a>de conteúdo:</strong>
            A estrutura Plug and Play integra-se aos serviços de tradução de terceiros líderes do setor.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>
            Estrutura de contexto do cliente da próxima geração para personalização do conteúdo.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Inicializações</a>:</strong>
            Desenvolver conteúdo para uma versão futura sem interromper a criação diária.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">Fragmentos</a>de conteúdo:</strong>
            Crie e prepare conteúdo editorial dissociado da apresentação para fácil reutilização.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Fragmentos</a>de experiência:</strong>
            Crie experiências e variações reutilizáveis otimizadas para desktop, dispositivos móveis e canais sociais.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">Serviços</a>de conteúdo:</strong>
            Exporte conteúdo de AEM como JSON para consumo em dispositivos e aplicativos.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Integração da Adobe Analytics e insights de conteúdo:</strong>
                Integração fácil do Adobe Analytics e do DTM. Exibir informações de desempenho no ambiente do autor.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Integração</a>Adobe Target:</strong>
            Assistente passo a passo para criar experiências direcionadas, criar bibliotecas de ofertas reutilizáveis.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Integração</a>Adobe Campaign:</strong>
            Integração fácil com a solução de campanha de email da próxima geração.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Integração</a>do Adobe Launch:</strong>
            Integrar ao serviço de gerenciamento de tags da próxima geração do Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">Telas</a>:</strong>
            Gerencie experiências para placas digitais e quiosques.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">eCommerce</a>:</strong>
            Forneça experiências de compras personalizadas e com marca em pontos de contato da Web, móveis e sociais.
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Comunidades</a>:</strong>
            Fóruns, comentários encadeados, calendários de eventos e muitos outros recursos permitem um envolvimento profundo com visitantes do site.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Recursos de ativos

Abaixo está uma matriz dos principais recursos do Assets oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias incrementais de versões anteriores adicionadas em cada versão.

+ [Notas de versão do AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***Isso denota melhorias significativas no recurso nesta versão.***

***✔<sup>+</sup>indica que o recurso está disponível por meio de um Service Pack ou de um Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso Ativos</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Interface otimizada para toque</a>:</strong>
            Gerencie ativos em um computador desktop ou em dispositivos habilitados para toque.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Gerenciamento</a>avançado de metadados:</strong>
            Modelos de metadados, Editor de Schemas de metadados e Edição de metadados em massa.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Gerenciamento de tarefa</a> e <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank">fluxo de trabalho</a> :</strong>
            Workflows e tarefas pré-criados para revisão e aprovação de ativos digitais que aproveitam AEM projetos.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Escalabilidade e desempenho:</strong>
            Suporte aprimorado para ingestão, carregamento e armazenamento em escala.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API</a>HTTP de ativos:</strong>
            Interagir programaticamente com ativos por HTTP e JSON.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Compartilhamento</a>de links:</strong>
            Compartilhamento ad hoc simples de ativos digitais sem a necessidade de fazer logon.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Portal</a>da marca:</strong>
            Solução SAAS de serviço em nuvem para compartilhamento e distribuição ininterrupta de ativos digitais.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Ativos</a>conectados:</strong>
            As instâncias do AEM Sites podem acessar e usar facilmente ativos de uma instância diferente do AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Insights</a>de ativos:</strong>
            Aproveite a Adobe Analytics para capturar a interação do cliente com ativos digitais e visualizações em AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Ativos</a>multilíngues:</strong>
            Suporte de tradução para metadados de ativos automaticamente com raízes de idioma.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Tags inteligentes e moderação</a>:</strong>
            Utilize o Adobe Sensei para marcar imagens automaticamente com metadados úteis.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Pesquisa</a>de tradução inteligente:</strong>
            Traduza automaticamente os termos de pesquisa ao pesquisar pelo AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Integração</a>Adobe InDesign Server:</strong>
            Gerar catálogos de produtos. Crie folhetos, folhetos e anúncios impressos com base em modelos de InDesigns.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">Aplicativo</a>para desktop AEM:</strong>
            Sincronize os ativos com a área de trabalho local para edição com produtos Creative Suite.
            </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Biblioteca</a>de imagens de Adobe:</strong>
                <br> Bibliotecas PDF Photoshop e Acrobat usadas para manipulação de arquivos de alta qualidade.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/br/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Link</a>do ativo Adobe:</strong>
            Acesse o AEM Assets diretamente de aplicativos da Adobe Create Cloud.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Integração</a>Adobe Stock:</strong>
            Acesse e use imagens do Adobe Stock sem problemas diretamente do AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***✔<sup>+</sup>melhorias significativas no recurso nesta versão.***

***✔<sup>SP</sup>indica que o recurso está disponível via Service Pack ou Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Recurso Dynamic Media</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6,3 +FP</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Imagem</a>:</strong>
            Forneça imagens de diferentes tamanhos e formatos dinamicamente, incluindo Recorte inteligente.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Vídeo</a>:</strong>
            Codificação avançada de vídeo e streaming de vídeo adaptável</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Mídia</a>interativa:</strong>
            Crie banners interativos, vídeos com conteúdo clicável para mostrar ofertas principais.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Conjuntos (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Imagem</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Giro</a>, Mídia <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">mista</a>):</strong>
            Permita que os usuários aumentem, deslocem, girem e simulem uma experiência de visualização de 360 graus.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/content/dynamic-media/viewer-presets.html" target="_blank">Visualizadores</a>:</strong>
            Leitores de mídia avançada personalizados e predefinições com suporte para diferentes telas/dispositivos.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Delivery</a>:</strong>
            Opções flexíveis para vinculação ou incorporação de conteúdo e delivery do Dynamic Media ao protocolo HTTP/2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Atualize do Scene7 para o Dynamic Media:</strong>
            Capacidade de migrar ativos principais e continuar usando URLs S7 existentes.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Recursos do Forms

Abaixo está uma matriz dos principais recursos do AEM Forms Add-on oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias incrementais de versões anteriores adicionadas em cada versão.

+ [Notas de versão do AEM Forms](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***✔<sup>+</sup>melhorias significativas no recurso nesta versão.***

***✔<sup>SP</sup>indica que o recurso está disponível via Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso Forms</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Editor</a>adaptável Forms:</strong>
            Crie formulários envolventes, responsivos e adaptáveis com base nas configurações do dispositivo e do navegador.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Documento do registro</a>:</strong>
            Crie um documento para garantir o armazenamento de longo prazo de uma experiência de captura de dados ou versão pronta para impressão.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">Editor</a>de Temas:</strong>
            Crie temas reutilizáveis para estilizar componentes e painéis de um formulário.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Editor</a>de modelos:</strong>
            Padronize e implemente as práticas recomendadas para formulários adaptáveis.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Integração</a>Adobe Sign:</strong>
            Permitir a implantação de cenários de assinatura baseados em formulários integrados da Adobe Sign.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Gerenciamento</a>de correspondência:</strong>
            Com a AEM Forms, você pode criar, gerenciar e fornecer correspondências personalizadas e interativas do cliente.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Integração</a>de dados de terceiros:</strong>
            Usando a integração de dados, os dados são obtidos de fontes de dados diferentes com base em entradas de usuários em um formulário. No envio do formulário, os dados capturados são gravados de volta nas fontes de dados.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Fluxo de trabalho (no OSGi) para processamento</a>Forms:</strong>
            Implantação simplificada de processos de aprovação de formulários.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Integração com o Marketing Cloud</a>:</strong>
            Integração com a Adobe Analytics e a Adobe Target para aprimorar e avaliar as experiências dos clientes.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Gerenciador</a>de formulários:</strong>
            Local único para gerenciar todos os formulários/documentos/correspondências, como habilitar análises, traduções, testes A/B, revisões e publicações.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">Aplicativo</a>AEM Forms:</strong>
            Permita o processamento de formulários online/offline em um aplicativo no iOS, Android ou Windows.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Comunicações</a>interativas:</strong>
            Crie comunicações avançadas, como declarações direcionadas com elementos interativos, como gráficos (anteriormente conhecidos como Documentos adaptáveis).</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">Fluxo de trabalho (J2EE) para processamento</a>Forms:</strong>
            Crie formulários complexos/workflows centrados no documento usando um IDE intuitivo.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">Segurança</a>do Documento AEM Forms:</strong>
            Acesso e autorização seguros de documentos PDF e do Office.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Estruturas</a>de teste:</strong>
            Use a estrutura Calvin e o plug-in Chrome para suportar e depurar formulários adaptáveis.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Recursos das comunidades

Abaixo está uma matriz dos principais recursos do AEM Communities Add-on oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias incrementais de versões anteriores adicionadas em cada versão.

+ [Resumo dos novos recursos do AEM Communities](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***✔<sup>+</sup>melhorias significativas no recurso nesta versão.***

***✔<sup>SP</sup>indica que o recurso está disponível via Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>Recurso Comunidades</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">Funções das comunidades</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Fóruns</a>:</strong> (Estrutura de componentes sociais) Crie novos tópicos, ou visualização, siga, pesquise e mova tópicos existentes.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">QnA</a>:</strong>
                Pergunte, visualização e responda perguntas.</p>
            </td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">Blogs</a>:</strong>
                Crie artigos e comentários em blogs no lado da publicação.
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Ideação</a>:</strong>
                Crie e compartilhe ideias com a comunidade, ou visualização, siga e comente ideias existentes.
            </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">Calendário</a>:</strong>
                (Estrutura de componentes sociais) Forneça informações de eventos da comunidade para visitantes do site.
            </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Biblioteca</a>de arquivos:</strong>
                Carregue, gerencie e baixe arquivos no site da comunidade.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Grupos</a>de usuários:
            </strong>Um conjunto de usuários pode pertencer a grupos de membros e pode receber funções atribuídas coletivamente.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Atribuição</a>:</strong>
            Crie e atribua recursos de aprendizado a membros da comunidade.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Ativação</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">Gerenciamento</a> de Catálogo e <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Recursos</a>:</strong>
            Acesse os recursos de ativação do catálogo.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Gerenciamento</a>de caminhos de aprendizado:</strong>
            Gerencie cursos ou grupos de recursos de ativação.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Relatórios</a>de ativação:</strong>
            Relatórios nos recursos de ativação e caminhos de aprendizado.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Participação na ativação</a>:</strong>
            Adicione comentários sobre os recursos de ativação.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Ativação do Analytics</a>:</strong>
            Análise de vídeo, Relatórios de andamento e Relatórios de atribuição</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">Comuns</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">Comentários</a> e anexos:</strong>
            (Estrutura de componentes sociais) Como membro da comunidade, compartilham opiniões e conhecimentos sobre conteúdo no site Comunidades.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Conversão de fragmento de conteúdo:</strong>
            Converter contribuições UGC em fragmentos de conteúdo.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Revisões</a>:</strong>
                (Estrutura de componentes sociais) Como um membro da comunidade revisa um conteúdo usando uma combinação de comentários e funções de classificação.</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">Classificações</a>:/strong&gt; (Social Component Framework) Como um membro da comunidade classifica um conteúdo.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">Votos</a>:</strong>
                (Estrutura de componentes sociais) Como membro da comunidade, votar a favor ou contra um conteúdo.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">Tags</a>:</strong>
            Anexe tags (palavras-chave ou rótulos) ao conteúdo para localizar rapidamente o conteúdo.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Pesquisar</a>:</strong>
            Buscas preditivas e sugestivas.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">Tradução</a>:</strong>
            Tradução automática de conteúdo gerado pelo usuário.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="10">Administração</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">Gerenciamento</a>do site:</strong>
            Criação de sites com funções de comunidades.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Modelos</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Modelos de site</a> e <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank">grupo</a> para a criação baseada em assistente de sites comunitários totalmente funcionais.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Modelos editáveis:</strong>
            Capacite os administradores da comunidade a criar experiências avançadas usando modelos editáveis AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Grupos ou subcomunidades</a>:</strong>
            Crie subcomunidades dinamicamente em sites de comunidades.
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">Moderação</a>:</strong>
            Moderação de conteúdo gerado pelo usuário.
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Moderação</a>em massa:</strong>
            Console de moderação para gerenciar em massa o conteúdo gerado pelo usuário.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Detecção de spam e filtros</a>de lucratividade:</strong>
            Detecção automática de spam.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">Gerenciamento</a>de membros:</strong>
            Gerencie perfis e grupos de usuários da área de gerenciamento de membros.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Design</a>responsivo:</strong>
            Os sites da AEM Communities respondem.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a>:</strong>
            Integre-se à Adobe Analytics para obter informações importantes sobre o uso de sites das Comunidades.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">Membros</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Pontuação e marcação</a>:</strong>
            (Pontuação avançada capacitada pela Adobe Sensei) Identifique os membros da comunidade como especialistas e recompense-os.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank">Atividades</a> e <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">notificações</a>:</strong>
            Visualização do fluxo de atividades recentes e receba notificações sobre eventos de interesse.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">Mensagens</a>:</strong>
            Mensagens diretas para usuários e grupos.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Logons</a>sociais:</strong>
            Faça logon com sua conta do Facebook ou Twitter.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Plataforma</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (Armazenamento Mongo)</a>:</strong>
            O conteúdo gerado pelo usuário (UGC) é persistente diretamente em uma instância MongoDB local</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (Armazenamento de banco de dados)</a>:</strong>
            O conteúdo gerado pelo usuário (UGC) é persistente diretamente em uma instância de banco de dados MySQL local.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (Armazenamento em nuvem)</a>:</strong>
                O conteúdo gerado pelo usuário (UGC) é persistente remotamente em um serviço de nuvem hospedado e gerenciado pelo Adobe.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                O conteúdo da comunidade é armazenado no JCR e o UGC pode ser acessado da instância do autor (ou da publicação) na qual foi publicado.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Sincronização</a>de usuários e grupos:</strong>
            Sincronize usuários e grupos em instâncias de publicação ao usar uma topologia de farm de publicação.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

A AEM Communities acrescenta [aprimoramentos](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html) através de lançamentos para permitir que as organizações interajam e habilitem seus usuários, ao:

+ **@menção** suporte em conteúdo gerado pelo usuário.
+ Aprimoramentos de acessibilidade por meio da Navegação **do** teclado nos componentes **Ativação** .
+ Moderação **em** massa aprimorada usando Filtros **** personalizados.
+ **Modelos** editáveis para capacitar os administradores da comunidade a criar experiências ricas da comunidade em AEM.
+ Os usuários agora podem enviar mensagens **diretas em massa** para todos os membros de um grupo.
