---
title: Entender os motivos para atualização
description: Um detalhamento de alto nível de recursos principais para clientes que consideram atualizar para a versão mais recente do Adobe Experience Manager 6.
version: Experience Manager 6.5
topic: Upgrade
feature: Release Information
role: Leader, Architect, Developer, Admin, User
level: Beginner
doc-type: Article
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
duration: 538
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '2576'
ht-degree: 1%

---

# Entendendo os motivos para atualização

Um detalhamento de alto nível de recursos principais para clientes que consideram atualizar para a versão mais recente do Adobe Experience Manager 6.

## Principais recursos para atualizar para o AEM 6.5

+ [Notas de versão do Adobe Experience Manager 6.5](https://helpx.adobe.com/br/experience-manager/6-5/release-notes.html)

### Melhorias na base

O Adobe Experience Manager 6.5 continua a aprimorar a estabilidade, o desempenho e a capacidade de suporte do sistema através:

+ Suporte ao **Java 11** (mantendo o suporte ao Java 8).

### Criação e gerenciamento de sites

O AEM Sites apresenta vários recursos projetados para acelerar a criação e a criação de sites:

+ O suporte ao **Editor de SPA** permite que o SPA (aplicativos de página única) seja totalmente criado no AEM, oferecendo suporte a uma experiência de criação avançada e fácil de usar pelo profissional de marketing.
+_ **O JavaScript SDK**, um kit de início de projeto de SPA e ferramentas de compilação de suporte, permite que desenvolvedores de front-end desenvolvam aplicativos de página única compatíveis com o Editor de SPA, independentemente do AEM.
+ **Componentes Principais** adiciona uma variedade de novos componentes, uma **Biblioteca de Componentes**, bem como uma variedade de melhorias aos Componentes Principais existentes.
+ Outras **Traduções** aprimoramentos simplificam a tradução do AEM Sites.

### Experiências fluídas

A AEM continua adotando o Fluid Experience com ferramentas novas e aprimoradas que facilitam o uso de conteúdo fora do AEM.

+ **Fragmentos de conteúdo** oferecem suporte a Comparação/comparação de versão e anotações.
+ A **API HTTP do Assets** da AEM oferece suporte à exposição de **Fragmentos de conteúdo** diretamente no DAM como **JSON**.
  **Fragmentos de experiência** oferecem suporte à **Pesquisa de texto completo** e à **Invalidação do cache do AEM Dispatcher** para fazer referência às **Páginas**.

### Gerenciamento de ativos

A AEM Assets continua desenvolvendo seu conjunto avançado de recursos de gerenciamento de ativos para melhorar o uso, o gerenciamento e a compreensão do DAM. O AEM 6.5 continua a melhorar a integração entre o Adobe Creative Cloud e os fluxos de trabalho criativos.

+ O **Adobe Asset Link** conecta criações diretamente ao AEM Assets a partir de ferramentas Adobe Creative Cloud.
+ A integração do **Adobe Stock** permite acesso direto às imagens do Adobe Stock diretamente da experiência do AEM Assets, criando uma experiência de descoberta de conteúdo contínua.
+ O **AEM Desktop App** lança a versão 2.0 e se revê enquanto melhora o desempenho e a estabilidade.
+ **O Connected Assets** oferece suporte a instâncias distintas do AEM Sites para acessar e usar facilmente os ativos de uma instância diferente do AEM Assets.
+ Suporte de vídeo atualizado no **Dynamic Media**, incluindo **Vídeo 360** e **Miniaturas de vídeo personalizadas**.

### Inteligência de conteúdo

A AEM continua a desenvolver sua integração com tecnologias inteligentes, aproveitando o aprendizado de máquina e a inteligência artificial para melhorar todas as experiências.

+ O **Adobe Asset Link** adiciona a **Pesquisa de Semelhança Visual**, permitindo que imagens semelhantes sejam facilmente descobertas e usadas nas **ferramentas do Adobe Creative Cloud**.

### Integrações

A AEM aumenta sua capacidade de integração com outros serviços da Adobe:

+ Os **Fragmentos de experiência** aprofundam sua integração com o **Adobe Target**, oferecendo suporte à **Exportação como JSON** para a Adobe Target e possibilitando a **exclusão de ofertas baseadas em Fragmento de experiência** da **Adobe Target**.

### AMS CLOUD MANAGER

O [Cloud Manager](https://adobe.ly/2HODmsv), uma exclusividade dos clientes do Adobe Managed Services (AMS), oferece os seguintes recursos:

+ O Cloud Manager oferece suporte ao estende o suporte à implantação do AEM do AEM Sites para o **AEM Assets**, incluindo o **teste de desempenho automatizado do processamento de ativos**.
+ **Dimensionamento automático** da camada de Publicação do AEM em limites predefinidos, garanta uma experiência ideal para o usuário final.
+ **Pipelines de não produção** permitem que as equipes de desenvolvimento usem o Cloud Manager para verificar continuamente a qualidade do código e implantar em ambientes inferiores (Desenvolvimento e controle de qualidade).
+ As **APIs do pipeline de CI/CD** permitem que os clientes se envolvam de forma programática com o Cloud Manager, aprofundando as possibilidades de integração com a infraestrutura de desenvolvimento local.

## Recursos do Foundation

Veja abaixo uma matriz dos principais recursos básicos oferecidos pelo AEM. Alguns desses recursos foram introduzidos em versões anteriores e melhorias incrementais foram adicionadas em cada versão.

+ [Notas de versão do AEM Foundation](https://helpx.adobe.com/br/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> aprimoramentos significativos no recurso nesta versão.***

***✔<sup>SP</sup> indica que o recurso está disponível por meio de um Service Pack ou de um Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso do Foundation</td>
            <td>5.6.x</td>
            <td>6,0</td>
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
                <strong>Suporte ao Java 11:</strong> o AEM oferece suporte ao Java 11 (bem como ao Java 8).
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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Repositório de Conteúdo do Oak</a>:</strong> Fornece desempenho e escalabilidade muito maiores do que o antecessor Jackrabbit 2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">Suporte a Índice oak-run.jar</a>:</strong> Reindexação, coleta de estatísticas e verificação de consistência aprimoradas dos índices Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Índices de Pesquisa Personalizados</a>: </strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Limpeza de Revisão Online</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Armazenamento de repositório TarMK ou MongoMK</a>:</strong>
                <br> Opções para usar o armazenamento de TarMK (versão de última geração do TarPM) baseado em arquivos simples e com bom desempenho
                <br> ou dimensione horizontalmente com um repositório com suporte de MongoDB com MongoMK.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Desempenho e estabilidade do MongoMK</a>:</strong>
            Aprimoramentos contínuos foram feitos no MongoMK desde sua introdução ao AEM 6.0.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">DataStore do Amazon S3</a>:</strong>
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
            <td><strong>Paridade do Recurso da Interface para Toque:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Painel de Operações</a>:</strong>
 Faça manutenção, monitore a integridade do servidor e analise o desempenho no AEM.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Melhorias de Atualização</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/htl/using/overview.html" target="_blank">Linguagem de Modelo HTL</a>:</strong>
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

Veja abaixo uma matriz dos principais recursos de segurança oferecidos pelo AEM. Alguns desses recursos foram introduzidos em versões anteriores e melhorias incrementais foram adicionadas em cada versão.

+ [Notas de versão de segurança](https://helpx.adobe.com/br/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔indica que foram feitos aprimoramentos significativos no recurso nesta versão.***

***✔<sup>+</sup> indica que o recurso está disponível por meio de um Service Pack ou de um Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso de segurança</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Usuários de Serviço</a></strong>
            <br> Compartimentaliza as permissões para evitar o uso desnecessário de privilégios de Administrador.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Gerenciamento de Repositório de Chaves</a></strong>
            <br> Repositório de confiança global, certificados e chaves, todos gerenciados no repositório.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong>proteção</strong></a>
            <br> Proteção contra falsificação de solicitação entre sites pronta para uso.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>suporte</strong></a>
            <br> Suporte ao Compartilhamento de Recursos entre Origens para maior flexibilidade dos aplicativos.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/pt-br/docs" target="_blank">Suporte aprimorado à autenticação SAML</a><br>
 </strong>Redirecionamento SAML aprimorado, informações de grupo otimizadas e problemas de criptografia de chave resolvidos.
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
        <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP como uma Configuração OSGi</a><br>
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
        <td><strong>Suporte de criptografia OSGi para senhas de texto sem formatação<br>
 </strong>Senhas e outros valores confidenciais podem ser salvos em formato criptografado e descriptografados automaticamente.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">Melhorias do CUG</a><br>
 </strong>A implementação do Grupo fechado de usuários foi regravada para solucionar problemas de desempenho e escalabilidade.</td>
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
            Interface do usuário do <br> para simplificar a configuração e o gerenciamento do SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Suporte a Token Encapsulado</a></strong>
            <br> Não é mais necessário que as sessões "adesivas" suportem autenticação horizontal em instâncias de publicação.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Suporte à autenticação do Adobe IMS</a><br>
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

+ [notas de versão do AEM Sites](https://helpx.adobe.com/br/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> aprimoramentos significativos no recurso nesta versão.***

***✔<sup>SP</sup> indica que o recurso está disponível por meio de um Service Pack ou de um Feature Pack.***

<table>
    <thead>
        <tr>
            <td><strong>Recurso do Sites</strong></td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Criação de Página Otimizada para Toque</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Criação Responsiva de Site</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Modelos Editáveis</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/core-components/user-guide.html" target="_blank">Componentes Principais</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">Editor de SPA</a>:</strong>
            Crie experiências da Web atraentes e autorais usando estruturas de Aplicativo de página única (SPA) criadas no React.</td>
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
            Aumente a reutilização do componente do AEM definindo sua aparência visual usando o sistema de estilo em contexto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Gerenciador de Vários Sites (MSM)</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Conversão de Conteúdo</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Inicializações</a>:</strong>
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
            <td><strong>Insights de Integração e Conteúdo do Adobe Analytics:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Integração do Adobe Target</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Integração do Adobe Campaign</a>:</strong>
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
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=pt-BR" target="_blank">Marcas na Integração do Adobe Experience Platform</a>:</strong>
            Integre com o serviço de nuvem de gerenciamento de tags de próxima geração da Adobe.</td>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">comércio eletrônico</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/communities/using/overview.html" target="_blank">Comunidades</a>:</strong>
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

## Recursos do Assets

Abaixo está uma matriz dos principais recursos do Assets oferecidos pelo AEM. Alguns desses recursos foram introduzidos em versões anteriores e melhorias incrementais foram adicionadas em cada versão.

+ [notas de versão do AEM Assets](https://helpx.adobe.com/br/experience-manager/6-5/release-notes/assets.html)

***✔indica que foram feitos aprimoramentos significativos no recurso nesta versão.***

***✔<sup>+</sup> indica que o recurso está disponível por meio de um Service Pack ou de um Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso do Assets</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Interface Otimizada para Toque</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/metadata.html" target="_blank">Gerenciamento avançado de metadados</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Tarefa</a> e Gerenciamento de Fluxo de Trabalho:</strong>
            Fluxos de trabalho e tarefas pré-criados para revisão e aprovação de ativos digitais que usam os projetos do AEM.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Escalabilidade e Desempenho:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API HTTP do Assets</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Compartilhamento de links</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Assets Conectado</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Insights de ativos</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Assets Multilíngue</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Marcas Inteligentes e Moderação</a>:</strong>
            Aproveite a IA do Adobe para marcar imagens automaticamente com metadados úteis.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Pesquisa de Conversão Inteligente</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/indesign.html" target="_blank">Integração do Adobe InDesign Server</a>:</strong>
            Gerar catálogos de produtos. Crie folhetos, folhetos e anúncios impressos com base em modelos do InDesign.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=pt-BR" target="_blank">Aplicativo de Desktop AEM</a>:</strong>
            Sincronize ativos com o desktop local para edição com produtos do Creative Suite.
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Biblioteca de Imagens do Adobe</a>:</strong>
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
            Acesse o AEM Assets diretamente dos aplicativos Create Cloud da Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Integração do Adobe Stock</a>:</strong>
            Acesse e use imagens do Adobe Stock diretamente do AEM de maneira contínua.</td>
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

***✔<sup>+</sup> aprimoramentos significativos no recurso nesta versão.***

***✔<sup>SP</sup> indica que o recurso está disponível por meio de um Service Pack ou de um Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Recurso do Dynamic Media</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3 +FP</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Imagens</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Vídeo</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Mídia interativa</a>:</strong>
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
            <td><strong>Conjuntos (<a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Imagem</a>, <a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Rotação</a>, <a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">Mídia Mista</a>):</strong>
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
            <td><strong><a href="https://experienceleague.adobe.com/pt-br/docs" target="_blank">Visualizadores</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Entrega</a>:</strong>
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

***✔<sup>+</sup> aprimoramentos significativos no recurso nesta versão.***

***✔<sup>SP</sup> indica que o recurso está disponível por meio de um Service Pack ou de um Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso do Forms</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Editor Adaptável do Forms</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Documento de Registro</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/forms/using/themes.html" target="_blank">Editor de Temas</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Editor de Modelos</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Integração do Acrobat Sign</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Gerenciamento de Correspondências</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Integração de Dados de Terceiros</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Fluxo de trabalho (no OSGi) para Processamento do Forms</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Gerenciador de Formulários</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">Aplicativo AEM Forms</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/aem-forms/6-5/adaptive-document.html" target="_blank">Comunicações Interativas</a>:</strong>
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
            <td><strong>Fluxo de trabalho (J2EE) para processamento do Forms:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">Segurança de documentos do AEM Forms</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/br/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Testando Estruturas</a>:</strong>
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

