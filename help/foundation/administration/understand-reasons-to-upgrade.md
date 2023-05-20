---
title: Entender os motivos para atualização
description: Um detalhamento de alto nível de recursos principais para clientes que consideram atualizar para a versão mais recente do Adobe Experience Manager 6.
version: 6.5
topic: Upgrade
role: Leader, Architect, Developer, Admin, User
level: Beginner
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '3460'
ht-degree: 3%

---

# Entendendo os motivos para atualização

Um detalhamento de alto nível de recursos principais para clientes que consideram atualizar para a versão mais recente do Adobe Experience Manager 6.

## Principais recursos para atualizar para o AEM 6.5

+ [Notas de versão do Adobe Experience Manager 6.5](https://helpx.adobe.com/experience-manager/6-5/release-notes.html)

### Melhorias na base

O Adobe Experience Manager 6.5 continua a aprimorar a estabilidade, o desempenho e a capacidade de suporte do sistema através:

+ **Java 11** (mantendo o suporte ao Java 8).

### Criação e gerenciamento de sites

O AEM Sites apresenta vários recursos projetados para acelerar a criação e a criação de sites:

+ **Editor de SPA** O suporte do permite que o SPA (aplicativos de página única) seja totalmente criado no AEM, oferecendo suporte a uma experiência de criação avançada e fácil de usar pelo profissional de marketing.
+_ **SDKs JavaScript**, um kit de início de projeto SPA e ferramentas de build de suporte, permitem que desenvolvedores de front-end desenvolvam aplicativos de página única compatíveis com o SPA, independentemente do AEM.
+ **Componentes principais** adiciona uma variedade de novos componentes, uma **Biblioteca de componentes** bem como uma variedade de melhorias nos Componentes principais existentes.
+ Mais **Traduções** As melhorias simplificam a tradução do AEM Sites.

### Experiências fluídas

O AEM continua a adotar Experiências fluídas com ferramentas novas e aprimoradas que facilitam o uso de conteúdo fora do AEM.

+ **Fragmentos de conteúdo** suportam Comparação/comparação de versão e anotações.
+ **API HTTP de ativos AEM** suporta exposição **Fragmentos de conteúdo** diretamente no DAM como **JSON**.
   **Fragmentos de experiência** suporte **Pesquisa de texto completo** e **Invalidação do cache do Dispatcher do AEM** para referência **Páginas**.

### Gerenciamento de ativos

A AEM Assets continua desenvolvendo seu conjunto avançado de recursos de gerenciamento de ativos para melhorar o uso, o gerenciamento e a compreensão do DAM. O AEM 6.5 continua a melhorar a integração entre o Adobe Creative Cloud e os fluxos de trabalho criativos.

+ **Adobe Asset Link** conecta criações diretamente ao AEM Assets a partir de ferramentas do Adobe Creative Cloud.
+ **Adobe Stock** A integração do permite acesso direto às imagens do Adobe Stock diretamente da experiência do AEM Assets, criando uma experiência perfeita de descoberta de conteúdo.
+ **Aplicativo de desktop AEM** O lança a versão 2.0 e se revê enquanto melhora o desempenho e a estabilidade.
+ **Connected Assets** O oferece suporte a instâncias discretas do AEM Sites para acessar e usar facilmente os ativos de uma instância diferente do AEM Assets.
+ Suporte de vídeo atualizado no **Dynamic Media**, incluindo **Vídeo 360** e **Miniaturas de vídeo personalizadas**.

### Inteligência de conteúdo

A AEM continua a construir sua integração com tecnologias inteligentes, aproveitando o aprendizado de máquina e a inteligência artificial para melhorar todas as experiências.

+ **Adobe Asset Link** adiciona **Pesquisa visual por semelhança**, permitindo que imagens semelhantes sejam facilmente descobertas e usadas no **Ferramentas do Adobe Creative Cloud**.

### Integrações

O AEM aumenta sua capacidade de integração com outros serviços da Adobe:

+ **Fragmentos de experiência** aprofunda a sua integração com a **Adobe Target** apoiando **Exportar como JSON** à Adobe Target e à capacidade de **excluir ofertas baseadas em fragmento de experiência** de **Adobe Target**.

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv), uma exclusividade dos clientes do Adobe Managed Services (AMS), oferece os seguintes recursos:

+ O Cloud Manager oferece suporte à ampliação do suporte à implantação do AEM da AEM Sites para **AEM Assets**, incluindo **teste de desempenho automatizado do processamento de ativos**.
+ **Dimensionamento automático** da camada de Publicação do AEM em limites predefinidos, garante uma experiência ideal para o usuário final.
+ **Pipelines de não produção** permitir que as equipes de desenvolvimento aproveitem o Cloud Manager para verificar continuamente a qualidade do código e implantar em ambientes inferiores (Desenvolvimento e controle de qualidade).
+ **APIs do pipeline de CI/CD** permitir que os clientes se envolvam programaticamente com o Cloud Manager, aprofundando as possibilidades de integração com a infraestrutura de desenvolvimento local.

## Recursos do Foundation

Abaixo está uma matriz dos principais recursos básicos oferecidos pelo AEM. Alguns desses recursos foram introduzidos em versões anteriores e melhorias incrementais foram adicionadas em cada versão.

+ [Notas de versão da Fundação AEM](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> melhorias significativas no recurso nesta versão.***

***✔<sup>SP</sup> indica que o recurso está disponível por meio de um Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso do Foundation</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <strong>Suporte ao Java 11:</strong> O AEM é compatível com o Java 11 (bem como com o Java 8).
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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Repositório de conteúdo do Oak</a>:</strong> Fornece desempenho e escalabilidade muito maiores do que o antecessor Jackrabbit 2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">Suporte ao índice oak-run.jar</a>:</strong> Melhoria na reindexação/indexação, coleta de estatísticas e verificação de consistência de índices do Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Índices de pesquisa personalizados</a>: </strong>
                Capacidade de adicionar definições de índice personalizadas para otimizar o desempenho da consulta e a relevância da pesquisa.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Limpeza de revisão online</a>:</strong>
                Faça a manutenção do repositório sem tempo de inatividade do servidor.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Armazenamento de repositório TarMK ou MongoMK</a>:</strong>
                <br> Opções para usar o armazenamento simples e eficiente baseado em arquivos do TarMK (versão de última geração do TarPM)
                <br> ou dimensione horizontalmente com um repositório MongoDB com suporte com MongoMK.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Desempenho e estabilidade do MongoMK</a>:</strong>
            Aprimoramentos contínuos foram feitos no MongoMK desde sua introdução com AEM 6.0.</td>
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
            <td><strong>Paridade de recursos da interface para toque:</strong>
                Aprimoramentos contínuos na interface do usuário de criação para agilizar com maior produtividade e paridade de recursos com a interface do usuário clássica.</td>
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
                Pesquise e navegue rapidamente pelo AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Painel de operações</a>:</strong>
 Faça manutenção, monitore a integridade do servidor e analise o desempenho de dentro do AEM.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Melhorias de atualização</a>:</strong>
            As melhorias de atualização permitem atualizações mais fáceis e rápidas no local do AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/htl/using/overview.html" target="_blank">Linguagem de modelo HTL</a>:</strong>
            Um mecanismo de modelo moderno que separa a apresentação da lógica. Reduz significativamente o tempo de desenvolvimento de componentes. Recursos incrementais adicionados a cada versão.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Modelos Sling</a>:</strong>
            Uma estrutura flexível para modelar recursos JCR em objetos de negócios e lógica. Recursos incrementais adicionados a cada versão.
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
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>: </strong>
                Exclusivo para clientes do Adobe Managed Services (AMS), o Cloud Manager acelera o desenvolvimento e a implantação por meio de um pipeline de CI/CD de última geração.</td>
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

Abaixo está uma matriz dos principais recursos de segurança oferecidos pelo AEM. Alguns desses recursos foram introduzidos em versões anteriores e melhorias incrementais foram adicionadas em cada versão.

+ [Notas de versão de segurança](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔ indica que melhorias significativas ao recurso nesta versão.***

***✔<sup>+</sup> indica que o recurso está disponível por meio de um Service Pack ou Feature Pack.***

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
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Usuários de serviço</a></strong>
            <br> As permissões compartimentalizadas evitam o uso desnecessário de privilégios de administrador.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Gerenciamento de Armazenamento de Chaves</a></strong>
            <br> Armazenamento global de confiança, certificados e chaves, todos gerenciados no repositório.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong>proteção</strong></a>
            <br> Solicitação entre sites Falsificação de proteção pronta para uso.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>suporte</strong></a>
            <br> Suporte ao compartilhamento de recursos entre origens para maior flexibilidade dos aplicativos.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">Aprimoramento do suporte à autenticação SAML</a><br>
 </strong>Melhorias no redirecionamento de SAML, informações de grupo otimizadas e problemas de criptografia de chave resolvidos.
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP como uma configuração OSGi</a><br>
 </strong>Simplifica o gerenciamento e as atualizações da autenticação LDAP.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>Suporte à criptografia OSGi para senhas de texto simples<br>
 </strong>Senhas e outros valores confidenciais podem ser salvos de forma criptografada e descriptografados automaticamente.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">Melhorias do CUG</a><br>
 </strong>A implementação do grupo fechado de usuários foi regravada para solucionar problemas de desempenho e escalabilidade.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">Assistente de SSL</a></strong>
            <br> para simplificar a configuração e o gerenciamento do SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Suporte a token encapsulado</a></strong>
            <br> Não é mais necessário que as sessões "adesivas" sejam compatíveis com autenticação horizontal em instâncias de publicação.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Suporte à autenticação do Adobe IMS</a><br>
 </strong>Exclusivo do Adobe Managed Services (AMS), gerencie centralmente o acesso às instâncias do AEM Author por meio do Adobe IMS (Identity Management System).</td>
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

Abaixo está uma matriz dos principais recursos do Sites oferecidos pelo AEM. Alguns desses recursos foram introduzidos em versões anteriores e melhorias incrementais foram adicionadas em cada versão.

+ [Notas de versão do AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> melhorias significativas no recurso nesta versão.***

***✔<sup>SP</sup> indica que o recurso está disponível por meio de um Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td><strong>Recurso do Sites</strong></td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Criação de página otimizada para toque</a>:</strong>
            Permite que os editores aproveitem tablets e computadores com telas sensíveis ao toque.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Criação responsiva de site</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Modelos editáveis</a>:</strong>
            Permite que autores especializados criem e editem modelos de página.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">Componentes principais</a>:</strong>
            Acelere o desenvolvimento de sites. Disponível no GitHub para programação de lançamento frequente e flexibilidade.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">Editor de SPA</a>:</strong>
            Crie experiências da Web atraentes e autorais usando estruturas de aplicativo de página única (SPA) criadas no React.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Sistema de Estilos:</strong>
            Aumentar a reutilização do componente AEM definindo sua aparência visual usando o sistema de estilo em contexto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Gerenciador de vários sites (MSM)</a>:</strong>
            Gerenciar vários sites que compartilham conteúdo em comum (ou seja, vários idiomas, várias marcas).</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Tradução de conteúdo</a>:</strong>
            A estrutura Plug and Play integra-se aos principais serviços de tradução de terceiros do setor.</td>
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
            Estrutura de contexto de cliente de última geração para personalização de conteúdo.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Lançamentos</a>:</strong>
            Desenvolva conteúdo para uma versão futura sem interromper a criação diária.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Fragmentos de conteúdo:</strong>
            Crie e prepare conteúdo editorial separado da apresentação para fácil reutilização.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Fragmentos de experiência</a>:</strong>
            Crie experiências reutilizáveis e variações otimizadas para desktop, dispositivos móveis e canais sociais.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Serviços de conteúdo:</strong>
            Exporte conteúdo do AEM como JSON para consumo em dispositivos e aplicativos.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Integração do Adobe Analytics e insights de conteúdo:</strong>
                Fácil integração do Adobe Analytics e do DTM. Exibir informações de desempenho no ambiente do Autor.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Integração do Adobe Target</a>:</strong>
            Assistente passo a passo para criar experiências direcionadas e criar bibliotecas de ofertas reutilizáveis.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Integração do Adobe Campaign</a>:</strong>
            Fácil integração com a solução de campanha de email de última geração.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=pt-BR" target="_blank">Integração do Adobe Launch</a>:</strong>
            Integre o ao serviço de nuvem de gerenciamento de tags de próxima geração do Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Screens:</strong>
            Gerencie experiências para sinalização digital e quiosques.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">comércio eletrônico</a>:</strong>
            Ofereça experiências de compra personalizadas e com marca em pontos de contato da Web, dispositivos móveis e sociais.
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Communities</a>:</strong>
            Fóruns, comentários encadeados, calendários de eventos e muitos outros recursos permitem um engajamento profundo com os visitantes do site.</td>
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

Abaixo está uma matriz dos principais recursos do Assets oferecidos pelo AEM. Alguns desses recursos foram introduzidos em versões anteriores e melhorias incrementais foram adicionadas em cada versão.

+ [Notas de versão do AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔ indica que melhorias significativas ao recurso nesta versão.***

***✔<sup>+</sup> indica que o recurso está disponível por meio de um Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso de ativos</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Interface otimizada para toque</a>:</strong>
            Gerencie ativos em um desktop ou em dispositivos habilitados para toque.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Gerenciamento avançado de metadados</a>:</strong>
            Modelos de metadados, Editor de esquema de metadados e Edição de metadados em massa.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Tarefa</a> e <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank">Fluxo de trabalho</a> Gerenciamento:</strong>
            Fluxos de trabalho e tarefas pré-criados para revisão e aprovação de ativos digitais que usam projetos AEM.</td>
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
            Suporte aprimorado para assimilação, upload e armazenamento em escala.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API HTTP de ativos</a>:</strong>
            Interagir programaticamente com ativos via HTTP e JSON.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Compartilhamento de link</a>:</strong>
            Compartilhamento ad-hoc simples de ativos digitais sem a necessidade de fazer logon.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:</strong>
            Solução SAAS do Cloud Service para compartilhamento e distribuição ininterruptos de ativos digitais.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Connected Assets</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Informações de ativos</a>:</strong>
            Aproveite o Adobe Analytics para capturar a interação do cliente de ativos digitais e visualizar no AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Ativos multilíngues</a>:</strong>
            Compatibilidade de tradução de metadados de ativos automaticamente com raízes de idioma.</td>
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
            Aproveite o Adobe Sensei para marcar imagens automaticamente com metadados úteis.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Pesquisa inteligente de tradução</a>:</strong>
            Traduzir automaticamente termos de pesquisa ao pesquisar pelo AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Integração do Adobe InDesign Server</a>:</strong>
            Gerar catálogos de produtos. Crie folhetos, folhetos e anúncios impressos com base em modelos de InDesign.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=pt-BR" target="_blank">Aplicativo de desktop AEM</a>:</strong>
            Sincronizar ativos ao desktop local para edição com os produtos Creative Suite.
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Biblioteca de imagens Adobe</a>:</strong>
                <br> Bibliotecas de PDF Photoshop e Acrobat usadas para manipulação de arquivos de alta qualidade.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/br/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe Asset Link</a>:</strong>
            Acesse o AEM Assets diretamente dos aplicativos Adobe Create Cloud.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Integração do Adobe Stock</a>:</strong>
            Acesse e use imagens do Adobe Stock diretamente do AEM de forma contínua.</td>
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

***✔<sup>+</sup> melhorias significativas no recurso nesta versão.***

***✔<sup>SP</sup> indica que o recurso está disponível por meio de um Service Pack ou Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Recurso do Dynamic Media</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3 +FP</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Criação de imagens</a>:</strong>
            Fornecer imagens dinamicamente em diferentes tamanhos e formatos, incluindo o Recorte inteligente.</td>
            <td> </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Vídeo</a>:</strong>
            Codificação avançada de vídeo e transmissão adaptável de vídeo</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Mídia interativa</a>:</strong>
            Crie banners interativos e vídeos com conteúdo clicável para exibir as principais ofertas.
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
            <td><strong>Conjuntos (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Imagem</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Rotação</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">Mix de mídia</a>):</strong>
            Permitir que os usuários ampliem, desloquem, girem e simulem uma experiência de visualização de 360 graus.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">Visualizadores</a>:</strong>
            Reprodutores de mídia avançada de marca personalizada e predefinições com suporte para telas/dispositivos diferentes.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Entrega</a>:</strong>
            Opções flexíveis para vinculação ou incorporação de conteúdo e entrega do Dynamic Media pelo protocolo HTTP/2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Atualização do Scene7 para o Dynamic Media:</strong>
            Capacidade de migrar ativos principais e continuar usando URLs do S7 existentes.</td>
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

Abaixo está uma matriz dos principais recursos complementares do AEM Forms oferecidos pelo AEM. Alguns desses recursos foram introduzidos em versões anteriores e melhorias incrementais foram adicionadas em cada versão.

***✔<sup>+</sup> melhorias significativas no recurso nesta versão.***

***✔<sup>SP</sup> indica que o recurso está disponível por meio de um Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso do Forms</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Editor Forms adaptável</a>:</strong>
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
            Crie um documento para garantir o armazenamento a longo prazo de uma experiência de captura de dados ou versão pronta para impressão.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">Editor de temas</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Editor de modelo</a>:</strong>
            Padronizar e implementar práticas recomendadas para formulários adaptáveis.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Integração do Acrobat Sign</a>:</strong>
            Permitir a implantação de cenários de assinatura baseados em formulários integrados do Acrobat Sign.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Gerenciamento de correspondência</a>:</strong>
            Com o AEM Forms, você pode criar, gerenciar e fornecer correspondências personalizadas e interativas para o cliente.
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Integração de dados de terceiros</a>:</strong>
            Com a Integração de dados, os dados são obtidos de diferentes fontes de dados com base nas entradas do usuário em um formulário. No envio do formulário, os dados capturados são gravados nas fontes de dados.
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Fluxo de trabalho (no OSGi) para processamento do Forms</a>:</strong>
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
            Integração com o Adobe Analytics e o Adobe Target para aprimorar e medir as experiências do cliente.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Gerenciador de formulários</a>:</strong>
            Local único para gerenciar todos os formulários/documentos/correspondências, como ativar análises, tradução, testes A/B, revisões e publicações.
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">Aplicativo AEM Forms</a>:</strong>
            Permitir o processamento de formulários online/offline em um aplicativo no iOS, Android ou Windows.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Comunicações interativas</a>:</strong>
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
            <td><strong>Fluxo de trabalho (J2EE) para processamento Forms:</strong>
            Crie formulários complexos/fluxos de trabalho centrados em documentos utilizando um IDE intuitivo.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">Segurança de documentos do AEM Forms</a>:</strong>
            Acesso e autorização seguros de documentos do PDF e do Office.
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Estruturas de teste</a>:</strong>
            Use a estrutura Calvin e o plug-in Chrome para oferecer suporte e depurar formulários adaptáveis.</td>
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

Abaixo está uma matriz dos principais recursos complementares do AEM Communities oferecidos pelo AEM. Alguns desses recursos foram introduzidos em versões anteriores e melhorias incrementais foram adicionadas em cada versão.

***✔<sup>+</sup> melhorias significativas no recurso nesta versão.***

***✔<sup>SP</sup> indica que o recurso está disponível por meio de um Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>Recurso de comunidades</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">Funções das comunidades</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Fóruns</a>:</strong> (Estrutura do componente social) Crie novos tópicos ou exiba, siga, pesquise e mova tópicos existentes.</td>
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
                Faça, visualize e responda perguntas.</p>
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
                Criar artigos e comentários do blog no lado da publicação.
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
                Crie e compartilhe ideias com a comunidade ou visualize, siga e comente ideias existentes.
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
                (Estrutura do componente social) Forneça informações sobre eventos da comunidade para visitantes do site.
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
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Biblioteca de arquivos</a>:</strong>
                Faça upload, gerencie e baixe arquivos no site da comunidade.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Grupos de usuários</a>:
            </strong>Um conjunto de usuários pode pertencer a grupos de membros e podem ser coletivamente atribuídos a funções.</td>
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
            Crie e atribua recursos de aprendizagem a membros da comunidade.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Ativação</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">Catálogo</a> e <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Gerenciamento de recursos</a>:</strong>
            Acessar recursos de ativação do catálogo.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Gerenciamento de caminhos de aprendizagem</a>:</strong>
            Gerenciar cursos ou grupos de recursos de ativação.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Relatórios de ativação</a>:</strong>
            Relatórios sobre recursos de ativação e caminhos de aprendizagem.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Envolvimento na ativação</a>:</strong>
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
            Análise de vídeo, relatórios de progresso e relatórios de atribuição</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">Comentários</a> e anexos:</strong>
            (Estrutura de componente social) Como um membro da comunidade, compartilhe opinião e conhecimento sobre o conteúdo no site Comunidades.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Conversão do fragmento de conteúdo:</strong>
            Converter contribuições UGC em fragmentos de conteúdo.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Resenhas</a>:</strong>
                (Estrutura do componente social) Como um membro da comunidade, revise um conteúdo usando uma combinação de comentários e funções de classificação.</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">Classificações</a>:/strong&gt; (Estrutura de componente social) Como um membro da comunidade, classifique um conteúdo.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">Votos</a>:</strong>
                (Estrutura de componente social) Como um membro da comunidade, vote a favor ou contra um conteúdo.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">Tags</a>:</strong>
            Anexe tags (palavras-chave ou rótulos) com o conteúdo para localizar rapidamente o conteúdo.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Pesquisar</a>:</strong>
            Pesquisas preditivas e sugestivas.</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">Gerenciamento de sites</a>:</strong>
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
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Site</a> e <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank">grupo</a> Modelos para a criação de sites da comunidade totalmente funcionais com base em assistentes.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Modelos editáveis:</strong>
            Capacite os administradores da comunidade para criar experiências avançadas usando modelos editáveis de AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Grupos ou subcomunidades</a>:</strong>
            Criar subcomunidades dinamicamente nos sites das comunidades.
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Moderação em massa</a>:</strong>
            Console de moderação para gerenciar conteúdo gerado pelo usuário em massa.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Detecção de spam e filtros de profanação</a>:</strong>
            Detecção automática de spam.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">Gerenciamento de Membros</a>:</strong>
            Gerenciar perfis de usuário e grupos na área de gerenciamento de membros.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Design responsivo</a>:</strong>
            Os sites do AEM Communities são responsivos.
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
            Integre com o Adobe Analytics para obter os principais insights sobre o uso dos sites das comunidades.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">Membros</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Pontuação e insígnia</a>:</strong>
            (Pontuação avançada fornecida pelo Adobe Sensei) Identifique membros da comunidade como especialistas e recompense-os.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank">Atividades</a> e <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">Notificação</a>:</strong>
            Exiba o fluxo de atividades recentes e seja notificado sobre eventos de interesse.</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Logons sociais</a>:</strong>
            Faça logon com a conta da Facebook ou da Twitter.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Platform</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (Armazenamento Mongo)</a>:</strong>
            O conteúdo gerado pelo usuário (UGC) é mantido diretamente em uma instância local do MongoDB</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (Armazenamento de banco de dados)</a>:</strong>
            O conteúdo gerado pelo usuário (UGC) é mantido diretamente em uma instância do banco de dados MySQL local.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (Cloud Storage, armazenamento em nuvem)</a>:</strong>
                O conteúdo gerado pelo usuário (UGC) é mantido remotamente em um serviço de nuvem hospedado e gerenciado pelo Adobe.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                O conteúdo da comunidade é armazenado no JCR e o UGC pode ser acessado a partir da instância do autor (ou publicação) na qual foi publicado.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Sincronização de usuários e grupos</a>:</strong>
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

A AEM Communities adiciona melhorias por meio de versões para permitir que as organizações envolvam e capacitem seus usuários, ao:

+ **@mention** suporte em conteúdo gerado pelo usuário.
+ Melhorias de acessibilidade por meio do **Navegação do teclado** in **Ativação** componentes.
+ Aprimorado **Moderação em massa** usar **Filtros personalizados**.
+ **Modelos editáveis** para capacitar administradores de comunidade a criar experiências ricas de comunidade no AEM.
+ Os usuários agora podem enviar **mensagens diretas em massa** a todos os membros de um grupo.
