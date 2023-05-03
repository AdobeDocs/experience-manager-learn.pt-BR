---
title: Guia de manutenção do site de rotina
seo-title: Your Routine Site Maintenance Guide
description: Seja administrador, autor ou desenvolvedor, a manutenção do site toca cada aspecto da instância do AEM Sites. Use este guia para garantir que sua estratégia seja configurada para ser bem-sucedida.
seo-description: Whether you're an admin, author, or developer, site maintenance touches every aspect of your AEM Sites instance. Use this guide to ensure your strategy is set up for success.
audience: author, marketer, developer
exl-id: 37ee3234-f91c-4f0a-b0b7-b9167e7847a9
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '1084'
ht-degree: 5%

---

# Dicas e truques de manutenção do site

Há três opções quando se trata de instalar e manter uma instância AEM

* AEMaaCS (serviço em nuvem) - o sistema está sempre ativado, atualizado e dimensionado dinamicamente conforme sua necessidade
* Serviços gerenciados da Adobe, onde os engenheiros de atendimento ao cliente do Adobe fazem toda a manutenção diária/semanal/mensal e garantem que todos os service packs sejam instalados e o sistema esteja sempre seguro e funcionando sem problemas
* executando-o no local, onde é necessário cuidar de todo o sistema, incluindo backups, atualizações e segurança.

Se você optar por implementar seu próprio sistema no local, veja algumas coisas que você deve ter em mente para garantir que tenha um sistema seguro e eficiente. Além dos itens de &quot;cuidado e alimentação&quot;, este documento também destacará vários itens AEM os desenvolvedores devem ter em mente para ajudar a manter o sistema funcionando bem.

## Administradores

Backups - certifique-se de ter backups completos e/ou parciais em uma programação frequente:

* diariamente
* semanalmente
* mensal

Muitos clientes fazem backups de snapshot, que levam apenas alguns minutos, supondo que o sistema operacional subjacente ofereça suporte a esses backups. Certifique-se de que esses backups sejam armazenados corretamente (fora do sistema de AEM). Verifique se os backups estão funcionando e podem ser usados para recriar um sistema de trabalho periodicamente - não há nada pior do que ter a falha do sistema e seus backups estão corrompidos por algum motivo!

Há vários itens que você precisa monitorar para garantir a operação sem problemas:

### Manutenção de rotina

#### [manutenção de índice](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=pt-BR)

Os índices permitem que as consultas sejam executadas o mais rápido possível, liberando recursos para outras operações. Certifique-se de que os índices estão na forma de ponta! AEM cancela queries que perdem em vez de usar um índice para impedir que uma consulta ruim afete o desempenho geral AEM.

#### [Compactação de Tar/Limpeza de revisão](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

Cada atualização no repositório cria uma nova revisão de conteúdo. Como resultado, a cada atualização o tamanho do repositório aumenta. Para evitar o crescimento descontrolado do repositório, é necessário limpar revisões antigas para liberar recursos de disco.

#### [Limpeza de binários do Lucene](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-dashboard.html#automated-maintenance-tasks)

Limpe os binários do lucene e reduza o requisito de tamanho do armazenamento de dados em execução.

#### [Lixo do armazenamento de dados](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/data-store-garbage-collection.html)

Quando um ativo no AEM é excluído, a referência ao registro do armazenamento de dados subjacente pode ser removida da hierarquia do nó, mas o próprio registro do armazenamento de dados permanece. Esse registro de armazenamento de dados não referenciado se torna &quot;lixo&quot; que não precisa ser retido. Nos casos em que existem vários ativos não referenciados, é útil livrá-los deles, preservar espaço, otimizar o backup e o desempenho de manutenção do sistema de arquivos.

#### [Remoção do fluxo de trabalho](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/workflows-administering.html?lang=pt-BR)

Minimizar o número de instâncias de fluxo de trabalho aumenta o desempenho do motor de workflow. Portanto, você pode remover regularmente do repositório as instâncias de fluxo de trabalho concluídas ou em execução.

#### [Manutenção do log de auditoria](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-audit-log.html)

AEM eventos qualificados para registro de auditoria geram muitos dados arquivados. Esses dados podem crescer rapidamente ao longo do tempo devido a replicações, uploads de ativos e outras atividades do sistema.

#### [Segurança](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=pt-BR)

Verifique se as práticas recomendadas da Lista de verificação de segurança são seguidas de perto para garantir a instância mais segura do AEM.

#### Diskspace

Monitore o espaço em disco para garantir que você tenha o suficiente para o Repositório JCR, além de cerca da metade novamente da quantidade de espaço - a compactação tar usa espaço extra quando é executada. A falta de espaço em disco é o principal motivo para a corrupção do JCR!

## Desenvolvedor

Tente não usar componentes personalizados - use [componentes principais](https://www.aemcomponents.dev/). Sua meta deve ser usar os componentes principais de 80 a 90% do tempo e os componentes personalizados apenas com moderação. Isso geralmente requer uma nova maneira de ver os componentes em uma página. Você deve perceber que os componentes podem ser reprogramados facilmente por um desenvolvedor front-end usando CSS. Lembre-se também de que esses componentes principais podem ser inseridos uns nos outros para alcançar resultados bastante complexos. Crie!

### [Sistemas de estilos](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

Os sistemas de estilos permitem que os componentes principais e até mesmo os componentes personalizados tenham sua aparência alterada ao critério dos Autores para criar componentes com aparência completamente nova. Essas mudanças estilísticas geralmente envolvem apenas um designer front-end e um autor experiente (geralmente chamado de &quot;Super Autor&quot;)

### [Lançamentos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

Lançamentos permitem que o trabalho seja concluído para uma nova promoção, venda ou implantação de site sem afetar as páginas implantadas no momento. Além disso, eles podem ser programados para ficarem ativos automaticamente, sem participação ou supervisão, permitindo que os Autores façam o trabalho da próxima semana (ou do próximo trimestre) hoje e não corram para o desenvolvimento da página no dia anterior ao lançamento - é realmente a dádiva do TEMPO!)

### [Fragmentos de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments.html)

Fragmentos de conteúdo são &quot;partes&quot; personalizáveis de informações que podem ser facilmente reutilizadas em todo o site. Se você precisar de uma alteração, basta alterar o pedaço original e a atualização é vista em todos os lugares em que é usada - imediatamente!

### [Fragmentos de experiência](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

Embora pareça quase idêntico aos Fragmentos de conteúdo, os Fragmentos de experiência são pequenos, visíveis, partes de uma página. Eles também podem ser amplamente reutilizados em todo o site e mantidos em um local central dentro do AEM para facilitar a tarefa de fazer alterações potencialmente globais no site em segundos, não em dias ou semanas.

Pense em frente e veja o que pode ser reutilizado. Um rodapé? Um aviso? Um Cabeçalho? Certos tipos de conteúdo? Todos esses itens podem ser compartilhados em um site inteiro, mantendo a manutenção no mínimo. Precisa atualizar uma data em um aviso, mas está em 1.000 páginas do seu site? É uma operação de 5 segundos se você usou um Fragmento de experiência!

## Geral

Fique atento às mudanças AEM pelo aprendizado contínuo - não fique preso no passado. Use [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) e [Serviços de aprendizado digital do Adobe (ADLS)](https://learning.adobe.com/) para melhorar suas habilidades.

## Conclusão

AEM pode ser um sistema grande, e são necessários muitos tipos de pessoas para fazê-lo &quot;cantar&quot;. De administradores a desenvolvedores (desenvolvedores Java de front-end e de hardware) a autores - há algo para todos! E se você não tem vontade de lidar com a Administração cotidiana, sempre há AMS e AEM as a Cloud Service.
