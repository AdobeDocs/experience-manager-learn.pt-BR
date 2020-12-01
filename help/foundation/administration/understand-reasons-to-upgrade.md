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

+ **Suporte** ao Java 11 (mantendo o suporte ao Java 8).

### Criação e gerenciamento de sites

A AEM Sites apresenta vários recursos projetados para acelerar a criação e o desenvolvimento de sites:

+ **SPA** Editorsupport permite a criação completa de SPA (aplicativos de página única) em AEM, com suporte para uma experiência de criação avançada e amigável para o profissional de marketing.
+_ **SDKs JavaScript**, um Kit de Start do SPA Project e ferramentas de criação de suporte, permitem que desenvolvedores front-end desenvolvam aplicativos de página única compatíveis com SPA Editor independentemente do AEM.
+ **Os componentes principais** acrescentam uma grande variedade de novos componentes, uma  **Biblioteca de** componentes e diversas melhorias aos componentes principais existentes.
+ Outras **Traduções** otimizam a tradução do AEM Sites.

### Experiências fluidas

AEM continua a abraçar as Experiências Fluidas com ferramentas novas e melhoradas que facilitam o uso de conteúdo fora da AEM.

+ **Fragmentos de conteúdo** suportam Comparação/Diferença de versão e anotações.
+ **AEM Assets HTTP** APIs suporta a exposição  **de** Fragmentos de conteúdo diretamente no DAM como  **JSON**.
   **Os** Fragmentos de experiência suportam  **a** pesquisa de texto completo  **AEM a** invalidação do cache do Dispatcher para referência a  **páginas**.

### Gerenciamento de ativos

A AEM Assets continua desenvolvendo seu avançado conjunto de recursos de gerenciamento de ativos para melhorar o uso, o gerenciamento e a compreensão do DAM. AEM 6.5 continua a melhorar a integração entre a Adobe Creative Cloud e workflows criativos.

+ **Adobe Asset** Linkconecta criativos diretamente ao AEM Assets a partir de ferramentas Adobe Creative Cloud.
+ **A integração do Adobe** Stockintegration permite acesso direto às imagens do Adobe Stock diretamente da experiência do AEM Assets, criando uma experiência contínua de descoberta de conteúdo.
+ **AEM Desktop** Appreleases versão 2.0 e re-visionando a si mesmo, melhorando o desempenho e a estabilidade.
+ **Os** ativos conectados oferecem suporte a instâncias discretas do AEM Sites para acessar e usar perfeitamente os ativos de uma instância diferente do AEM Assets.
+ Atualização do suporte a vídeo em **Dynamic Media**, incluindo **360 Video** e **Miniaturas de vídeo personalizadas**.

### Inteligência de conteúdo

AEM continua a construir sua integração com tecnologias inteligentes, aproveitando o aprendizado de máquinas e inteligência artificial para melhorar todas as experiências.

+ **Adobe Asset** Linkadd Pesquisa **de semelhança** visual, permitindo que imagens semelhantes sejam facilmente descobertas e usadas dentro das ferramentas **do** Adobe Creative Cloud.

### Integrações

AEM aumenta sua capacidade de integração com outros serviços de Adobe:

+ **A Experience** Fragmentsaprofunda sua integração com o  **Adobe** Targetao oferecer suporte ao  **Export as** JSONto Adobe Target e a capacidade de  **excluir** ofertas baseadas em Fragmentos de experiência da  **Adobe Target**.

### Gerenciador da AMS Cloud

[O Cloud Manager](https://adobe.ly/2HODmsv), exclusivo para clientes do Adobe Managed Services (AMS), oferta os seguintes recursos:

+ O Cloud Manager oferece suporte AEM implantação da AEM Sites para **AEM Assets**, incluindo **teste automático de desempenho do processamento de ativos**.
+ **A** dimensionamento automático da camada de publicação de AEM em limites predefinidos garante uma experiência ideal para o usuário final.
+ **Os** pipelinesde não produção permitem que as equipes de desenvolvimento aproveitem o Cloud Manager para verificar continuamente a qualidade do código e implantar em ambientes mais baixos (Desenvolvimento e QA).
+ **API do pipeline de CI/CDlos clientes para se envolverem programaticamente com o Cloud Manager, aprofundando as possibilidades de integração com a infraestrutura de desenvolvimento local.** 

## Recursos básicos

Abaixo está uma matriz dos principais recursos básicos oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias incrementais de versões anteriores adicionadas em cada versão.

+ [Notas de versão do AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔ <sup>+melhorias </sup> significativas no recurso nesta versão.***

***✔ <sup></sup> SPdenota que o recurso está disponível via Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso básico</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <strong>Suporte a Java 11:</strong> AEM suporta Java 11 (bem como Java 8).
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Repositório</a> de conteúdo Oak:</strong> fornece desempenho e escalabilidade muito maiores do que o antecessor Jackrabbit 2.</td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">suporte</a> a índice oak-run.jar:</strong> reindexação aprimorada, coleta de estatísticas e verificação de consistência de índices Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Índices</a> de pesquisa personalizados:  </strong>
                Capacidade de adicionar definições de índice personalizadas para otimizar o desempenho do query e a relevância da pesquisa.</td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Limpeza</a> de revisão on-line: </strong>
                realize a manutenção do repositório sem tempo ocioso do servidor.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Armazenamento</a> do repositório TarMK ou MongoMK:</strong>
                <br> opções para usar armazenamentos simples e eficientes baseados em arquivos do TarMK (versão da próxima geração do TarPM) 
                <br> ou dimensionar horizontalmente com um repositório com backup MongoDB com MongoMK.</td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Desempenho e estabilidade</a> do MongoMK: </strong>
            aprimoramentos contínuos foram feitos ao MongoMK desde a sua introdução ao AEM 6.0.</td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>:</strong>
            aproveite a solução de armazenamento em nuvem expansível para armazenar ativos binários.</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong>Paridade de recursos da interface do usuário de toque: melhorias </strong>
                contínuas na interface do usuário de criação para aumentar a produtividade e a paridade de recursos com a interface clássica.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong>Omnisearch:pesquisa e navegue </strong>
                rapidamente AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Painel</a> de operações: </strong>
 realize manutenção, monitore a integridade do servidor e analise o desempenho de dentro do AEM.</td>
            <td></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Melhorias</a> na atualização: </strong>
            as melhorias na atualização permitem atualizações de AEM mais fáceis e rápidas.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/htl/using/overview.html" target="_blank">Linguagem</a> de modelo HTL:</strong>
            um mecanismo moderno de modelagem que separa a apresentação da lógica. Reduz significativamente o tempo de desenvolvimento do componente. Recursos incrementais adicionados a cada versão.</td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Modelos</a> Sling:</strong>
            uma estrutura flexível para modelar os recursos do JCR em objetos de negócios e lógica. Recursos incrementais adicionados a cada versão.
            </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Gerenciador</a> de nuvem:  </strong>
                Exclusivo para clientes do Adobe Managed Services (AMS), o Cloud Manager acelera o desenvolvimento e a implantação por meio de um pipeline de CI/CD avançado.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Recursos de segurança

Abaixo está uma matriz dos principais recursos de segurança oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias incrementais de versões anteriores adicionadas em cada versão.

+ [Notas de versão de segurança](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***Isso denota melhorias significativas no recurso nesta versão.***

***✔ <sup>+</sup> indica que o recurso está disponível por meio de um Service Pack ou de um Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso de segurança</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Serviços </a></strong>
            <br> UsuáriosCompartimentaliza permissões para evitar o uso desnecessário de privilégios de Administrador.</td>
        <td></td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Gerenciamento </a></strong>
            <br> de armazenamento de chaveArmazenamento de confiança global, certificados e chaves gerenciados no repositório.</td>
        <td></td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>Proteção </strong> <strong></strong></a>
            <br> CSRFprotectionProteção contra falsificações entre sites pronta para uso.</td>
        <td></td>
        <td></td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>Suporte a </strong> <strong></strong></a>
            <br> CORSsupportCompartilhamento de recursos entre Origens para maior flexibilidade do aplicativo.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
    </tr>
    <tr>
        <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/security/saml-2-0-authenticationhandler.html" target="_blank">Aprimoramento do </a><br>
 </strong>suporte de autenticação SAMLredirecionamento SAML aprimorado, informações de grupo otimizadas e problemas de criptografia de chave resolvidos. 
            <br>
        </td>
        <td></td>
        <td></td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP como uma </a><br>
 </strong>configuração OSGiSimplifica o gerenciamento e as atualizações da autenticação LDAP.</td>
        <td></td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
    </tr>
    <tr>
        <td><strong>Suporte de criptografia OSGi para <br>
 </strong>senhas de texto simplesAs senhas e outros valores confidenciais podem ser salvos em forma criptografada e descriptografados automaticamente.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">Aprimoramentos </a><br>
 </strong>de CUG A implementação do Grupo de usuários fechado foi reescrita para resolver problemas de desempenho e escalabilidade.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>Can</td>
        <td><sup>+</sup></td>
        <td>Can</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL </a></strong>
            <br> WizardUI para simplificar a configuração e o gerenciamento do SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Suporte </a></strong>
            <br> a token encapsuladoNão é mais necessário para sessões "aderentes" para suportar a autenticação horizontal em instâncias de publicação.</td>
        <td> </td>
        <td> </td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
        <td>Can</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe IMS Authentication </a><br>
 </strong>SupportExclusivo para Adobe Managed Services (AMS), gerencia centralmente o acesso às instâncias de autor de AEM por meio do Adobe IMS (Identity Management System).</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>Can</td>
        <td>Can</td>
    </tr>
</tbody>
</table>

## Recursos do Sites

Abaixo está uma matriz dos principais recursos do Sites oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias incrementais de versões anteriores adicionadas em cada versão.

+ [Notas de versão do AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔ <sup>+melhorias </sup> significativas no recurso nesta versão.***

***✔ <sup></sup> SPdenota que o recurso está disponível via Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td><strong>Recurso Sites</strong></td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Criação</a> de página otimizada ao toque:</strong>
            permite que editores aproveitem tablets e computadores com telas de toque.</td>
            <td></td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Criação</a> de sites responsivos:</strong>
                o modo de layout permite que os editores redimensionem componentes com base nas larguras de dispositivos para sites responsivos.</td>
            <td></td>
            <td></td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Modelos</a> editáveis:</strong>
            permite que autores especializados criem e editem modelos de página.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">Componentes</a> principais:</strong>
            acelere o desenvolvimento do site. Disponível no GitHub para agendamento e flexibilidade de versões frequentes.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">Editor</a> de SPA:</strong>
            crie experiências web criáveis e interessantes usando estruturas de aplicativo de página única (SPA) criadas em React ou Angular.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">Sistema</a> de estilo: </strong>
            aumente AEM reutilização do componente definindo sua aparência visual usando o sistema de estilo no contexto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>SP</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">MSM (Multi-Site Manager)</a>:</strong>
            gerencie vários sites que compartilham conteúdo comum (ou seja, várias marcas multilíngues).</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Tradução</a> de conteúdo: a estrutura </strong>
            Plug and Play integra-se aos principais serviços de tradução de terceiros do setor.</td>
            <td></td>
            <td></td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:estrutura de contexto de cliente da </strong>
            próxima geração para personalização do conteúdo.</td>
            <td></td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Inicializações</a>:</strong>
            desenvolva conteúdo para uma versão futura sem interromper a criação diária.</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">Fragmentos</a> de conteúdo:</strong>
            crie e prepare conteúdo editorial dissociado da apresentação para fácil reutilização.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Fragmentos</a> de experiência:</strong>
            crie experiências e variações reutilizáveis otimizadas para canais de desktop, móveis e sociais.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">Serviços</a> de conteúdo:</strong>
            exporte conteúdo de AEM como JSON para consumo em dispositivos e aplicativos.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>SP</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong>Integração da Adobe Analytics e insights de conteúdo:integração </strong>
                fácil da Adobe Analytics e do DTM. Exibir informações de desempenho no ambiente do autor.</td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Integração</a> do Adobe Target:assistente </strong>
            passo a passo para criar experiências direcionadas, criar bibliotecas de ofertas reutilizáveis.</td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Integração</a> Adobe Campaign:integração </strong>
            fácil com a solução de campanha de e-mail da próxima geração.</td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Integração</a> do Adobe Launch: </strong>
            integre com a nuvem de gerenciamento de tags da próxima geração do serviço Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">Telas</a>:</strong>
            gerencie experiências para placas digitais e quiosques.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">eCommerce</a>:</strong>
            forneça experiências de compras personalizadas e com marca em pontos de contato da Web, móveis e sociais.
            </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Comunidades</a>:</strong>
            Fóruns, comentários encadeados, calendários de eventos e muitos outros recursos permitem um envolvimento profundo com visitantes do site.</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Recursos de ativos

Abaixo está uma matriz dos principais recursos do Assets oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias incrementais de versões anteriores adicionadas em cada versão.

+ [Notas de versão do AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***Isso denota melhorias significativas no recurso nesta versão.***

***✔ <sup>+</sup> indica que o recurso está disponível por meio de um Service Pack ou de um Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso Ativos</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Interface otimizada para toque</a>:</strong>
            gerencie ativos em um computador desktop ou em dispositivos habilitados para toque.</td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Gerenciamento</a> avançado de metadados: modelos </strong>
            de metadados, editor de Schemas de metadados e edição de metadados em massa.</td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Gerenciamento de </a> tarefas e  <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank"></a> fluxo de trabalho:tarefas e workflows </strong>
            pré-criados para revisão e aprovação de ativos digitais que aproveitam AEM projetos.</td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong>Escalabilidade e desempenho: suporte </strong>
            aprimorado para ingestão, upload e armazenamento em escala.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API</a> HTTP de ativos:interage </strong>
            programaticamente com ativos por HTTP e JSON.</td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Compartilhamento</a> de links: compartilhamento ad-hoc </strong>
            simples de ativos digitais sem precisar fazer logon.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Portal</a> da marca:solução SAAS de serviço da </strong>
            Cloud para compartilhamento e distribuição ininterruptas de ativos digitais.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Ativos</a> conectados: as instâncias do </strong>
            AEM Sites podem acessar e usar facilmente ativos de uma instância diferente do AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Insights</a> de ativos:</strong>
            aproveite a Adobe Analytics para capturar a interação dos clientes com ativos digitais e visualizações em AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Ativos</a> multilíngues:suporte de </strong>
            tradução de metadados de ativos automaticamente com raízes de idioma.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Tags inteligentes e moderação</a>:</strong>
            aproveite o Adobe Sensei para marcar imagens automaticamente com metadados úteis.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Pesquisa</a> de Tradução Inteligente: Traduz </strong>
            automaticamente termos de pesquisa ao pesquisar pelo AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Integração</a> Adobe InDesign Server:</strong>
            Gerar catálogos de produtos. Crie folhetos, folhetos e anúncios impressos com base em modelos de InDesigns.</td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">Aplicativo</a> para desktop AEM:</strong>
            sincronize os ativos na área de trabalho local para edição com produtos de Creative Suite.
            </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Biblioteca</a> de imagens de Adobe:bibliotecas PDF de </strong>
                <br> Photoshop e Acrobat usadas para manipulação de arquivos de alta qualidade.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/br/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Link</a> do ativo do Adobe:</strong>
            acesse o AEM Assets diretamente do Adobe Create Cloud Applications.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Integração</a> com o Adobe Stock:acesse </strong>
            e use imagens do Adobe Stock sem problemas diretamente da AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>SP</sup></td>
            <td>Can</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***✔ <sup>+melhorias </sup> significativas no recurso nesta versão.***

***✔ <sup></sup> SPdenota que o recurso está disponível via Service Pack ou Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Recurso Dynamic Media</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3 +FP</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Imagem</a>:</strong>
            forneça imagens de diferentes tamanhos e formatos dinamicamente, incluindo Recorte inteligente.</td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Vídeo</a>:codificação de vídeo </strong>
            avançada e streaming de vídeo adaptável</td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Mídia</a> interativa: </strong>
            crie banners interativos, vídeos com conteúdo clicável para mostrar ofertas principais.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong>Conjuntos (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Imagem</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Giro</a>, Mídia <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank"> </a>mista):</strong>
            Permite que os usuários ampliem, deslocem, girem e simulem uma experiência de visualização de 360 graus.</td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/content/dynamic-media/viewer-presets.html" target="_blank">Visualizadores</a>:players de mídia avançada </strong>
            personalizados e predefinições com suporte para diferentes telas/dispositivos.</td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Delivery</a>: opções </strong>
            flexíveis para vinculação ou incorporação de conteúdo e delivery do Dynamic Media ao protocolo HTTP/2.</td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong>Atualização do Scene7 para o Dynamic Media:</strong>
            capacidade de migrar ativos principais e continuar usando URLs S7 existentes.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
    </tbody>
</table>

## Recursos do Forms

Abaixo está uma matriz dos principais recursos do AEM Forms Add-on oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias incrementais de versões anteriores adicionadas em cada versão.

+ [Notas de versão do AEM Forms](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***✔ <sup>+melhorias </sup> significativas no recurso nesta versão.***

***✔ <sup></sup> SPdenota que o recurso está disponível via Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso Forms</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Editor</a> adaptável Forms:</strong>
            crie formulários envolventes, responsivos e adaptáveis com base nas configurações do dispositivo e do navegador.</td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Documento de registro</a>:</strong>
            crie um documento para garantir o armazenamento de longo prazo de uma experiência de captura de dados ou versão pronta para impressão.</td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">Editor</a> de Temas:</strong>
            Crie temas reutilizáveis para criar o estilo de componentes e painéis de um formulário.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Editor</a> de modelos:</strong>
            padronize e implemente práticas recomendadas para formulários adaptáveis.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Integração</a> Adobe Sign:</strong>
            permita a implantação de cenários de assinatura baseados em formulários integrados Adobe Sign.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Gerenciamento</a> de correspondência: </strong>
            com a AEM Forms, você pode criar, gerenciar e fornecer correspondências personalizadas e interativas do cliente.
            </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Integração</a> de dados de terceiros:</strong>
            usando a integração de dados, os dados são obtidos de fontes de dados diferentes com base em entradas de usuários em um formulário. No envio do formulário, os dados capturados são gravados de volta nas fontes de dados.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Fluxo de trabalho (no OSGi) para processamento</a> Forms:implantação </strong>
            simplificada de processos de aprovação de formulários.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Integração com o Marketing Cloud</a>:</strong>
            integração com a Adobe Analytics e a Adobe Target para aprimorar e avaliar as experiências dos clientes.</td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Gerenciador</a> de formulários:local </strong>
            único para gerenciar todos os formulários/documentos/correspondências, como ativar análises, traduções, testes A/B, revisões e publicações.
            </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">Aplicativo</a> AEM Forms: </strong>
            permite o processamento de formulários online/offline em um aplicativo no iOS, Android ou Windows.</td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Comunicações</a> interativas: </strong>
            crie comunicações avançadas, como declarações direcionadas com elementos interativos, como gráficos (anteriormente conhecidos como Documentos adaptativos).</td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">Fluxo de trabalho (J2EE) para processamento</a> Forms:</strong>
            crie formulários complexos/workflows centrados no documento usando um IDE intuitivo.</td>
            <td></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms Documento Security</a>: acesso </strong>
            seguro e autorização de documentos PDF e do Office.
            </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Estruturas</a> de teste:</strong>
            Use a estrutura Calvin e o plug-in Chrome para suportar e depurar formulários adaptáveis.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
    </tbody>
</table>

## Recursos das comunidades

Abaixo está uma matriz dos principais recursos do AEM Communities Add-on oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias incrementais de versões anteriores adicionadas em cada versão.

+ [Resumo dos novos recursos do AEM Communities](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***✔ <sup>+melhorias </sup> significativas no recurso nesta versão.***

***✔ <sup></sup> SPdenota que o recurso está disponível via Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>Recurso Comunidades</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">Funções das comunidades</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Fóruns</a>:</strong> (Estrutura de componentes sociais) Crie novos tópicos ou visualização, siga, pesquise e mova tópicos existentes.</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">QnA</a>:</strong>
                Perguntar, visualização e responder perguntas.</p>
            </td>
            <td></td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">Blogs</a>:</strong>
                Crie artigos e comentários em blogs no lado da publicação.
            </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Ideação</a>:</strong>
                Crie e compartilhe ideias com a comunidade, ou visualização, siga e comente ideias existentes.
            </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">Calendário</a>:</strong>
                (Estrutura de componentes sociais) Fornecer informações de eventos da comunidade aos visitantes do site.
            </td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Biblioteca</a> de arquivos:</strong>
                faça upload, gerencie e baixe arquivos no site da comunidade.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Grupos</a> de usuários: 
            </strong>Um conjunto de usuários pode pertencer a grupos de membros e pode receber funções atribuídas coletivamente.</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Atribuição</a>:</strong>
            crie e atribua recursos de aprendizado a membros da comunidade.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td rowspan="5">Ativação</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">Gerenciamento</a>  de  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">catálogos e </a>recursos:</strong>
            acesse os recursos de ativação do catálogo.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Gerenciamento</a> de caminhos de aprendizado:</strong>
            gerencie cursos ou grupos de recursos de ativação.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Relatórios</a> de ativação:</strong>
            Relatórios sobre recursos de ativação e caminhos de aprendizado.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Envolvimento na ativação</a>:</strong>
            adicione comentários aos recursos de ativação.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Análise</a> de ativação:Análise de </strong>
            vídeo, Relatórios de progresso e Relatórios de atribuição</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td rowspan="8">Comuns</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank"></a> Comentários e anexos:</strong>
            (Estrutura do componente social) Como membro da comunidade, compartilham opiniões e conhecimentos sobre conteúdo no site Comunidades.</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong>Conversão de fragmento de conteúdo:</strong>
            Converter contribuições UGC em fragmentos de conteúdo.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Revisões</a>:</strong>
                (Social Component Framework) Como um membro da comunidade revisa um conteúdo usando uma combinação de comentários e funções de classificação.</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">Classificações</a>:/strong&gt; (Social Component Framework) Como um membro da comunidade classifica um conteúdo.</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">Votos</a>:</strong>
                (Estrutura de Componente Social) Como membro da comunidade, votar a favor ou a favor de um conteúdo.</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">Tags</a>:</strong>
            anexe tags (palavras-chave ou rótulos) ao conteúdo para localizar rapidamente o conteúdo.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Pesquisa</a>: </strong>
            buscas preditivas e sugestivas.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">Tradução</a>:</strong>
            tradução automática de conteúdo gerado pelo usuário.</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td rowspan="10">Administração</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">Gerenciamento</a> do site:</strong>
            Criar sites com funções de comunidades.</td>
            <td> </td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Modelos</a>:modelos de </strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank"></a> site e  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> grupo para a criação baseada em assistente de sites da Comunidade totalmente funcionais.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong>Modelos editáveis:</strong>
            capacite os administradores da comunidade a criar experiências avançadas usando modelos editáveis AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Grupos ou Subcomunidades</a>:Criar </strong>
            dinamicamente subcomunidades em sites de comunidades.
            </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">Moderação</a>:</strong>
            moderação de conteúdo gerado pelo usuário.
            </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Moderação</a> em massa:console </strong>
            de moderação para gerenciar em massa o conteúdo gerado pelo usuário.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Detecção de spam e filtros</a> de lucratividade:detecção </strong>
            automática de spam.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">Gerenciamento</a> de membros: </strong>
            gerencie perfis de usuários e grupos da área de gerenciamento de membros.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Design</a> responsivo: os sites da </strong>
            AEM Communities respondem.
            </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a>:</strong>
            integre-se à Adobe Analytics para obter informações importantes sobre o uso dos sites das Comunidades.</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td rowspan="4">Membros</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Pontuação e marcação</a>:</strong>
            (Pontuação avançada capacitada pela Adobe Sensei) Identifique os membros da comunidade como especialistas e recompense-os.</td>
            <td> </td>
            <td> </td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank"></a> Atividades e  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">Notificações</a>:</strong>
            Visualização de fluxo de atividades recentes e receber notificações sobre eventos de interesse.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">Mensagens</a>:Mensagens </strong>
            diretas para usuários e grupos.</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Logons</a> sociais:</strong>
            faça logon com sua conta do Facebook ou Twitter.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td rowspan="5">Plataforma</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (Armazenamento Mongo)</a>:o conteúdo gerado pelo </strong>
            usuário (UGC) é persistente diretamente em uma instância MongoDB local</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (Armazenamento de Banco de Dados)</a>:o conteúdo gerado pelo </strong>
            usuário (UGC) é persistente diretamente em uma instância de banco de dados MySQL local.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (Armazenamento em nuvem)</a>: o conteúdo gerado pelo </strong>
                usuário (UGC) é persistente remotamente em um serviço em nuvem hospedado e gerenciado pelo Adobe.</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>: o conteúdo </strong>
                da comunidade é armazenado no JCR e o UGC pode ser acessado a partir da instância do autor (ou publicação) na qual foi publicado.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Sincronização</a> de usuários e grupos:</strong>
            sincronize usuários e grupos em instâncias de publicação ao usar uma topologia de farm de publicação.</td>
            <td><sup>+</sup></td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
            <td>Can</td>
        </tr>
    </tbody>
</table>

A AEM Communities adiciona [melhorias](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html) através de versões para permitir que as organizações se engajem e habilitem seus usuários, ao:

+ **@** mentionsupport em conteúdo gerado pelo usuário.
+ Aprimoramentos de acessibilidade por meio de **Navegação do teclado** nos componentes **Ativação**.
+ Melhoria na **Moderação em massa** usando **Filtros personalizados**.
+ **Modelos editáveis** para capacitar os administradores da comunidade a criar experiências ricas da comunidade em AEM.
+ Os usuários agora podem enviar **mensagens diretas em massa** para todos os membros de um grupo.
