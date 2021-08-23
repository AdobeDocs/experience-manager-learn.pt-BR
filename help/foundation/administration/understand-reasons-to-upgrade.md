---
title: Entender os motivos para atualizar
description: Um detalhamento de alto nível dos principais recursos para clientes que consideram atualizar para a versão mais recente do Adobe Experience Manager.
version: 6.5
sub-product: ativos, gerenciador de nuvem, comércio, serviços de conteúdo, Dynamic-media, formulários, fundação, telas, sites
topics: best-practices, upgrade
audience: all
activity: understand
doc-type: article
topic: Atualização
role: Leader, Architect, Developer, Admin, User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '3529'
ht-degree: 3%

---


# Entendendo os motivos para atualizar

Um detalhamento de alto nível dos principais recursos para clientes que consideram atualizar para a versão mais recente do Adobe Experience Manager.

## Principais recursos para a atualização para o AEM 6.5

+ [Notas de versão do Adobe Experience Manager 6.5](https://helpx.adobe.com/experience-manager/6-5/release-notes.html)

### Melhorias de base

O Adobe Experience Manager 6.5 continua a melhorar a estabilidade, o desempenho e a capacidade de suporte do sistema por meio de:

+ **Suporte ao Java 11** (mantendo o suporte ao Java 8).

### Criação e gerenciamento de sites

O AEM Sites apresenta vários recursos projetados para acelerar a criação e o desenvolvimento de sites:

+ **SPA** O suporte ao Editor permite que SPA (aplicativos de página única) sejam completamente criados no AEM, o que oferece suporte a uma experiência de criação avançada e compatível com o profissional de marketing.
+_ **SDKs do JavaScript**, um Kit de início de projeto SPA e ferramentas de criação de suporte, permitem que desenvolvedores front-end desenvolvam aplicativos de página única compatíveis com SPA Editor independentemente do AEM.
+ **Os** Componentes principais adicionam uma variedade de novos componentes, uma  **biblioteca de** componentes e uma variedade de aprimoramentos aos componentes principais existentes.
+ Outras melhorias **Traduções** simplificam a tradução do AEM Sites.

### Experiências fluídas

AEM continua a abraçar Experiências fluídas com ferramentas novas e aprimoradas que facilitam o uso de conteúdo fora do AEM.

+ **Os** Fragmentos de conteúdo suportam Comparação/diff e anotações de versão.
+ **A** API HTTP do AEM Assets suporta a exposição  **de** Fragmentos de conteúdo diretamente no DAM como  **JSON**.
   **Os** Fragmentos de experiência oferecem suporte à  **Pesquisa de texto completo e** AEM  **Invalidação do cache do Dispatcher para** páginas de referência ****.

### Gerenciamento de ativos

A AEM Assets continua a aproveitar seu conjunto avançado de recursos de gerenciamento de ativos para melhorar o uso, o gerenciamento e a compreensão do DAM. O AEM 6.5 continua a melhorar a integração entre o Adobe Creative Cloud e os fluxos de trabalho criativos.

+ **O Adobe Asset** Linkconecta criações diretamente ao AEM Assets a partir das ferramentas do Adobe Creative Cloud.
+ **A integração do Adobe** Stockintegration permite acesso direto às imagens do Adobe Stock diretamente da experiência do AEM Assets, criando uma experiência contínua de descoberta de conteúdo.
+ **AEM Desktop** Appreses versão 2.0 e re-visionamento ao mesmo tempo em que melhora o desempenho e a estabilidade.
+ **O Connected** Assets oferece suporte a instâncias discretas do AEM Sites para acessar e usar com facilidade ativos de uma instância diferente do AEM Assets.
+ Atualização do suporte de vídeo em **Dynamic Media**, incluindo **360 Video** e **Miniaturas de vídeo personalizadas**.

### Inteligência de conteúdo

AEM continua a construir sua integração com tecnologias inteligentes, aproveitando o aprendizado de máquina e a inteligência artificial para melhorar todas as experiências.

+ **O Adobe Asset** Linkadiciona a Pesquisa de Similaridade  **Visual**, permitindo que imagens semelhantes sejam facilmente descobertas e usadas nas ferramentas **do** Adobe Creative Cloud.

### Integrações

AEM aumenta sua capacidade de integração com outros serviços de Adobe:

+ **Os** Fragmentos de experiência aprofundam a integração com o  **Adobe** Target, oferecendo suporte à  **exportação como** JSON para Adobe Target e à capacidade de  **excluir** ofertas baseadas em Fragmentos de experiência do  **Adobe Target**.

### AMS Cloud Manager

[O Cloud Manager](https://adobe.ly/2HODmsv), exclusivo dos clientes do Adobe Managed Services (AMS), oferece os seguintes recursos:

+ O Cloud Manager é compatível com o estende AEM suporte de implantação do AEM Sites para **AEM Assets**, incluindo **teste de desempenho automatizado do processamento de ativos**.
+ **O** dimensionamento automático da camada de publicação do AEM em limites predefinidos garante uma experiência ideal para o usuário final.
+ **Os** pipelines não relacionados à produção permitem que as equipes de desenvolvimento aproveitem o Cloud Manager para verificar continuamente a qualidade do código e implantar em ambientes inferiores (desenvolvimento e controle de qualidade).
+ **** API do pipeline de CI/CDlpermite que os clientes interajam programaticamente com o Cloud Manager, aprofundando as possibilidades de integração com a infraestrutura de desenvolvimento local.

## Recursos da base

Abaixo está uma matriz de recursos fundamentais oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias adicionais de versões anteriores, adicionadas em cada versão.

+ [Notas de versão do AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔ <sup>+melhorias </sup> significativas no recurso nesta versão.***

***✔ <sup></sup> SPdenota que o recurso está disponível por meio de um Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso de base</td>
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
                <strong>Suporte ao Java 11:</strong> o AEM suporta o Java 11 (bem como o Java 8).
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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Repositório de conteúdo do Oak</a>: </strong> oferece desempenho e escalabilidade muito maiores do que o antecessor Jackrabbit 2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">Suporte ao índice oak-run.jar</a>: </strong> reindexação aprimorada, coleta de estatísticas e verificação de consistência de índices Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Índices</a> de pesquisa personalizados:  </strong>
                Capacidade de adicionar definições de índice personalizadas para otimizar o desempenho da consulta e a relevância da pesquisa.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Limpeza de Revisão Online</a>:</strong>
                Faça a manutenção do repositório sem tempo de inatividade do servidor.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Armazenamento de repositório TarMK ou MongoMK</a>: </strong>
                <br> opções para usar armazenamento simples, com base em arquivos e desempenho do TarMK (versão de próxima geração do TarPM) 
                <br> ou dimensionar horizontalmente com um repositório com backup do MongoDB com MongoMK.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Desempenho e estabilidade do MongoMK</a>: </strong>
            melhorias contínuas foram feitas ao MongoMK desde a sua introdução com o AEM 6.0.</td>
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
            aproveite a solução de armazenamento em nuvem expansível para armazenar ativos binários.</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Paridade de recursos da interface do usuário de toque:</strong>
                 melhorias contínuas na interface do usuário de criação para agilizar, com maior produtividade e paridade de recursos com a interface do usuário clássica.</td>
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
                pesquise e navegue rapidamente AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Painel de operações</a>: </strong>
 execute manutenção, monitore a integridade do servidor e analise o desempenho de dentro do AEM.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Melhorias na atualização</a>: </strong>
            as melhorias na atualização permitem atualizações de AEM mais fáceis e rápidas no local.</td>
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
            um mecanismo de modelo moderno que separa a apresentação da lógica. Reduz significativamente o tempo de desenvolvimento de componentes. Recursos incrementais adicionados a cada versão.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Modelos do Sling</a>:</strong>
            uma estrutura flexível para modelar recursos do JCR em objetos e lógicas de negócios. Recursos incrementais adicionados a cada versão.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>:  </strong>
                Exclusivo dos clientes do Adobe Managed Services (AMS), o Cloud Manager acelera o desenvolvimento e a implantação por meio de um pipeline de CI/CD avançado.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Recursos de segurança

Abaixo está uma matriz dos principais recursos de segurança oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias adicionais de versões anteriores, adicionadas em cada versão.

+ [Notas de versão de segurança](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***Indica que há melhorias significativas no recurso nesta versão.***

***✔ <sup>+</sup> indica que o recurso está disponível por meio de um Service Pack ou Feature Pack.***

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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Service </a></strong>
            <br> UsersCompartimentaliza permissões para evitar o uso desnecessário de privilégios de Administrador.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Armazenamento </a></strong>
            <br> de chavesArmazenamento de confiança global, certificados e chaves todos gerenciados no repositório.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> Proteção CSRFprotectionProteção de falsificação de solicitação entre sites pronta para uso.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> Suporte ao CORSsupportCompartilhamento de recursos entre origens para maior flexibilidade de aplicativos.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">Aprimoramento do </a><br>
 </strong>suporte de autenticação SAMLredirecionamento SAML aprimorado, informações de grupo otimizadas e problemas de criptografia de chave resolvidos. 
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP como uma </a><br>
 </strong>configuração OSGiSimplifica o gerenciamento e as atualizações da autenticação LDAP.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>O suporte à criptografia OSGi para <br>
 </strong>senhas de texto simples, senhas e outros valores confidenciais pode ser salvo em formato criptografado e descriptografado automaticamente.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUG </a><br>
 </strong>enhancementsClosed-User Group a implementação foi reescrita para solucionar problemas de desempenho e escalabilidade.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td><sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL </a></strong>
            <br> WizardUI para simplificar a configuração e o gerenciamento do SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Encapsulated Token </a></strong>
            <br> SupportNão é mais necessário para sessões "aderentes" para suportar autenticação horizontal em instâncias de publicação.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Suporte </a><br>
 </strong>à autenticação do Adobe IMSExclusivo do Adobe Managed Services (AMS), gerencie centralmente o acesso às instâncias de autor do AEM por meio do Adobe IMS (Identity Management System).</td>
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

## Recursos de sites

Abaixo está uma matriz dos principais recursos do Sites oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias adicionais de versões anteriores, adicionadas em cada versão.

+ [Notas de versão do AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔ <sup>+melhorias </sup> significativas no recurso nesta versão.***

***✔ <sup></sup> SPdenota que o recurso está disponível por meio de um Service Pack ou Feature Pack.***

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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Criação de página otimizada ao toque</a>: </strong>
            permite que os editores aproveitem tablets e computadores com telas de toque.</td>
            <td></td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Criação responsiva do site</a>: </strong>
                o modo de layout permite que os editores redimensionem componentes com base na largura do dispositivo para sites responsivos.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Modelos editáveis</a>:</strong>
            permite que autores especializados criem e editem modelos de página.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">Componentes principais</a>:</strong>
            acelere o desenvolvimento do site. Disponível no GitHub para agendamento de versão e flexibilidade frequentes.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">Editor de SPA</a>:</strong>
            crie experiências da Web autoráveis e envolventes usando estruturas de Aplicativo de página única (SPA) criadas no React ou no Angular.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/br/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">Sistema de estilos</a>: </strong>
            aumente a reutilização AEM componente definindo sua aparência visual usando o sistema de estilo no contexto.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Multi-Site Manager (MSM)</a>:</strong>
            gerencie vários sites que compartilham conteúdo comum (ou seja, várias marcas em várias línguas).</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Tradução de conteúdo</a>: </strong>
            a estrutura Plug and play integra-se aos serviços de tradução de terceiros líderes do setor.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>
            Estrutura de contexto de cliente de próxima geração para personalização de conteúdo.</td>
            <td></td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Lançamentos</a>: </strong>
            desenvolva conteúdo para uma versão futura sem interromper a criação diária.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">Fragmentos de conteúdo</a>: </strong>
            crie e prepare o conteúdo editorial dissociado da apresentação para fácil reutilização.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Fragmentos de experiência</a>: </strong>
            crie experiências e variações reutilizáveis otimizadas para desktops, dispositivos móveis e canais sociais.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">Serviços de conteúdo</a>: </strong>
            exporte conteúdo de AEM como JSON para consumo em dispositivos e aplicativos.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Integração do Adobe Analytics e insights de conteúdo:</strong>
                integração fácil do Adobe Analytics e do DTM. Exiba informações de desempenho no ambiente do autor.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Integração do Adobe Target</a>: </strong>
            assistente passo a passo para criar experiências direcionadas, criar bibliotecas de ofertas reutilizáveis.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Integração do Adobe Campaign</a>:</strong>
            fácil integração com a solução de campanha de email da próxima geração.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Integração</a> do Adobe Launch:</strong>
            integre-se ao serviço em nuvem de gerenciamento de tags de próxima geração do Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">Telas</a>: </strong>
            gerencie experiências para sinalização digital e quiosques.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">eCommerce</a>: </strong>
            forneça experiências de compras personalizadas e com marca em todos os pontos de contato da Web, móveis e sociais.
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Comunidades</a>: </strong>
            Fóruns, comentários segmentados, calendários de eventos e muitos outros recursos permitem um envolvimento profundo com visitantes do site.</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Recursos do Assets

Abaixo está uma matriz dos principais recursos do Assets oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias adicionais de versões anteriores, adicionadas em cada versão.

+ [Notas de versão do AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***Indica que há melhorias significativas no recurso nesta versão.***

***✔ <sup>+</sup> indica que o recurso está disponível por meio de um Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso de ativos</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Interface otimizada para toque</a>: </strong>
            gerencie ativos em um computador de desktop ou em dispositivos habilitados para toque.</td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Gerenciamento avançado de metadados</a>: </strong>
            modelos de metadados, editor de esquema de metadados e edição de metadados em massa.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank"></a> Taskand  <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank"></a> WorkflowManagement:</strong>
            fluxos de trabalho e tarefas pré-criados para revisão e aprovação de ativos digitais que aproveitam projetos AEM.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Escalabilidade e desempenho:</strong>
            suporte aprimorado para assimilação, upload e armazenamento em escala.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API HTTP de ativos</a>:</strong>
            interage programaticamente com ativos por HTTP e JSON.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Compartilhamento de link</a>: </strong>
            Compartilhamento ad hoc simples de ativos digitais sem precisar fazer logon.</td>
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
            solução SAAS de serviço em nuvem para compartilhamento e distribuição ininterruptos de ativos digitais.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Ativos conectados</a>:</strong>
            as instâncias do AEM Sites podem acessar e usar facilmente os ativos de uma instância diferente do AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Asset Insights</a>: </strong>
            Aproveite o Adobe Analytics para capturar a interação dos ativos digitais com o cliente e visualizar no AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Ativos multilíngues</a>:</strong>
            suporte de tradução de metadados de ativos automaticamente com raízes de idioma.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Tags inteligentes e moderação</a>: </strong>
            aproveite o Adobe Sensei para marcar imagens automaticamente com metadados úteis.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Pesquisa de tradução inteligente</a>: </strong>
            traduza automaticamente termos de pesquisa ao pesquisar pelo AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Integração do Adobe InDesign Server</a>: </strong>
            gere catálogos de produtos. Crie brochuras, folhetos e anúncios impressos com base em modelos de InDesign.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=pt-BR" target="_blank">AEM aplicativo de desktop</a>: </strong>
            sincronize ativos com o desktop local para edição com os produtos Creative Suite.
            </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Biblioteca de imagens Adobe</a>:</strong>
                <br> bibliotecas PDF Photoshop e Acrobat usadas para manipulação de arquivos de alta qualidade.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/br/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe Asset Link</a>: </strong>
            acesse o AEM Assets diretamente de aplicativos Adobe-Create Cloud.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Integração do Adobe Stock</a>:</strong>
            acesse e use de forma contínua as imagens do Adobe Stock diretamente da AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>SP</sup></td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***✔ <sup>+melhorias </sup> significativas no recurso nesta versão.***

***✔ <sup></sup> SPdenota que o recurso está disponível por meio de um Service Pack ou Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Recurso do Dynamic Media</td>
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
            forneça imagens de forma dinâmica em diferentes tamanhos e formatos, incluindo o Recorte inteligente.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Vídeo</a>: </strong>
            Codificação de vídeo avançada e transmissão de vídeo adaptável</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Mídia interativa</a>: </strong>
            crie banners interativos, vídeos com conteúdo clicável para mostrar as principais ofertas.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Conjuntos (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Imagem</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Rotação</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">Mídia mista</a>): </strong>
            permite que os usuários ampliem, panoramizem, girem e simulem uma experiência de visualização de 360 graus.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">Visualizadores</a>: </strong>
            players de mídia avançada com marca personalizada e predefinições com suporte para diferentes telas/dispositivos.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Delivery</a>:</strong>
            opções flexíveis para vinculação ou incorporação de conteúdo Dynamic Media e entrega por protocolo HTTP/2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Atualização do Scene7 para o Dynamic Media: </strong>
            capacidade de migrar ativos principais e continuar usando URLs do S7 existentes.</td>
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

Abaixo está uma matriz dos principais recursos do complemento AEM Forms oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias adicionais de versões anteriores, adicionadas em cada versão.

+ [Notas de versão do AEM Forms](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***✔ <sup>+melhorias </sup> significativas no recurso nesta versão.***

***✔ <sup></sup> SPdenota que o recurso está disponível por meio de um Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Recurso do Forms</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Editor Adaptive Forms</a>:</strong>
            crie formulários envolventes, responsivos e adaptáveis com base nas configurações do dispositivo e do navegador.</td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Documento de registro</a>: </strong>
            crie um documento para garantir o armazenamento de longo prazo de uma experiência de captura de dados ou versão pronta para impressão.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">Editor de Temas</a>: </strong>
            Crie temas reutilizáveis para criar componentes e painéis de estilo de um formulário.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Editor de modelo</a>:</strong>
            padronize e implemente práticas recomendadas para formulários adaptáveis.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Integração do Adobe Sign</a>:</strong>
            permite a implantação de cenários de assinatura baseados em formulários integrados do Adobe Sign.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Gerenciamento de correspondência</a>: </strong>
            com o AEM Forms, você pode criar, gerenciar e fornecer correspondências personalizadas e interativas do cliente.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Integração de dados de terceiros</a>:</strong>
            com a integração de dados, os dados são obtidos de fontes de dados diferentes com base em entradas de usuários em um formulário. No envio do formulário, os dados capturados são gravados de volta nas fontes de dados.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Fluxo de trabalho (no OSGi) para processamento do Forms</a>:</strong>
            implantação simplificada de processos de aprovação de formulários.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Integração com o Marketing Cloud</a>:</strong>
            integração com o Adobe Analytics e o Adobe Target para aprimorar e medir as experiências do cliente.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Gerenciador de formulário</a>:</strong>
            local único para gerenciar todos os formulários/documentos/correspondência, como habilitar análises, tradução, testes A/B, revisões e publicação.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">Aplicativo AEM Forms</a>: </strong>
            permite o processamento de formulários online/offline em um aplicativo no iOS, Android ou Windows.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Comunicações interativas</a>:</strong>
            crie comunicações avançadas, como declarações direcionadas com elementos interativos, como gráficos (conhecidos anteriormente como Documentos adaptativos).</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">Fluxo de trabalho (J2EE) para processamento do Forms</a>: </strong>
            crie formulários complexos/fluxos de trabalho centrados em documentos utilizando um IDE intuitivo.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms Document Security</a>: </strong>
            acesso e autorização seguros de documentos PDF e do Office.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Estruturas de teste</a>: </strong>
            use a estrutura Calvin e o plug-in Chrome para suportar e depurar formulários adaptáveis.</td>
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

Abaixo está uma matriz dos principais recursos do complemento AEM Communities oferecidos pela AEM. Alguns desses recursos foram introduzidos em melhorias adicionais de versões anteriores, adicionadas em cada versão.

+ [Resumo do novo recurso do AEM Communities](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***✔ <sup>+melhorias </sup> significativas no recurso nesta versão.***

***✔ <sup></sup> SPdenota que o recurso está disponível por meio de um Service Pack ou Feature Pack.***

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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Fóruns</a>:</strong>  (Estrutura do componente social) Crie novos tópicos ou exiba, siga, pesquise e mova os tópicos existentes.</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">QnA</a>: </strong>
                Faça, exiba e responda perguntas.</p>
            </td>
            <td></td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">Blogs</a>: </strong>
                Crie artigos e comentários no blog do lado da publicação.
            </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Ideação</a>: </strong>
                crie e compartilhe ideias com a comunidade ou exiba, siga e comente ideias existentes.
            </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">Calendário</a>:</strong>
                 (Estrutura do componente social) fornece informações de eventos da comunidade para visitantes do site.
            </td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Biblioteca de arquivos</a>: </strong>
                Faça upload, gerencie e baixe arquivos no site da comunidade.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Grupos</a> de usuários: 
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
            crie e atribua recursos de aprendizagem a membros da comunidade.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Ativação</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank"></a> Catálogo e gerenciamento de  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">recursos</a>:</strong>
            acesse os recursos de ativação do catálogo.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Gerenciamento de caminhos de aprendizado</a>:</strong>
            gerencie cursos ou grupos de recursos de capacitação.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Relatórios de ativação</a>:</strong>
            relatórios sobre recursos de ativação e caminhos de aprendizado.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Envolvimento na ativação</a>: </strong>
            adicione comentários sobre recursos de ativação.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Análise de ativação</a>: </strong>
            análise de vídeo, relatórios de progresso e relatórios de atribuição</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank"></a> Comentários e anexos:</strong>
             (Estrutura do componente social) como membro da comunidade compartilham opinião e conhecimento sobre conteúdo no site Comunidades.</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Conversão do fragmento de conteúdo: </strong>
            converta contribuições UGC para fragmentos de conteúdo.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Revisões</a>:</strong>
                 (Estrutura do componente social) Como um membro da comunidade, revise um conteúdo usando uma combinação de comentários e funções de classificação.</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">Classificações</a>:/strong&gt; (Estrutura do componente social) Como um membro da comunidade classifica uma parte do conteúdo.</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">Votos</a>:</strong>
                 (estrutura de componente social) Como membro da comunidade, votar em cima ou em baixo de um conteúdo.</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">Tags</a>: </strong>
            anexe tags (palavras-chave ou rótulos) ao conteúdo para localizar rapidamente o conteúdo.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Pesquisa</a>: </strong>
            Pesquisas preditivas e sugestivas.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">Tradução</a>:</strong>
            tradução automática do conteúdo gerado pelo usuário.</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="10">Administração</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">Gerenciamento de site</a>:</strong>
            criação de sites com funções de comunidades.</td>
            <td> </td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Modelos</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank"></a> Modelos de site e  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> grupo para a criação baseada em assistente de sites da Comunidade totalmente funcionais.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Modelos editáveis:</strong>
            capacite os administradores da comunidade a criar experiências avançadas usando Modelos editáveis AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Grupos ou subcomunidades</a>:</strong>
            crie subcomunidades dinamicamente nos sites de comunidades.
            </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">Moderação</a>: </strong>
            moderação de conteúdo gerado pelo usuário.
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Moderação em massa</a>: </strong>
            console de moderação para gerenciar o conteúdo gerado pelo usuário em massa.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Detecção de spam e Filtros de lucratividade</a>:</strong>
            Detecção automática de spam.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">Gerenciamento de membros</a>: </strong>
            gerencie perfis de usuários e grupos da área de gerenciamento de membros.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td><sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Design responsivo</a>: </strong>
            os sites do AEM Communities são responsivos.
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
            integre-se ao Adobe Analytics para obter informações importantes sobre o uso dos sites do Communities.</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">Membros</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Pontuação e selo</a>: </strong>
             (Pontuação avançada disponibilizada pela Adobe Sensei) Identifique membros da comunidade como especialistas e premie-os.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank"></a> Atividades e  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">notificações</a>:</strong>
            visualize o fluxo de atividades recentes e receba notificações sobre eventos de interesse.</td>
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
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Logons do Social</a>: </strong>
            faça logon com a conta da Facebook ou Twitter.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Plataforma</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (Mongo Storage)</a>: </strong>
            o conteúdo gerado pelo usuário (UGC) é persistente diretamente em uma instância MongoDB local</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (Database Storage)</a>: </strong>
            o conteúdo gerado pelo usuário (UGC) é mantido diretamente em uma instância de banco de dados MySQL local.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (Cloud Storage)</a>: </strong>
                o conteúdo gerado pelo usuário (UGC) é mantido remotamente em um serviço de nuvem hospedado e gerenciado pelo Adobe.</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                o conteúdo da comunidade é armazenado no JCR e o UGC é acessível a partir da instância do autor (ou publicação) na qual foi publicado.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Sincronização de usuários e grupos</a>: </strong>
            sincronize usuários e grupos em instâncias de publicação ao usar uma topologia de farm de Publicação.</td>
            <td><sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

A AEM Communities adiciona [aprimoramentos](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html) por meio de versões para permitir que as organizações se envolvam e habilitem seus usuários, ao:

+ **@** mentionsupport em conteúdo gerado pelo usuário.
+ Melhorias de acessibilidade por meio da **Navegação do teclado** nos componentes **Ativação**.
+ Melhoria no **Moderação em massa** usando **Filtros personalizados**.
+ **Modelos editáveis** para capacitar os administradores da comunidade a criar experiências de comunidade avançadas em AEM.
+ Agora os usuários podem enviar **mensagens diretas em massa** para todos os membros de um grupo.
