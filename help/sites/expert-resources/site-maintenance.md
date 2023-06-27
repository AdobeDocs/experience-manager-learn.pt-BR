---
title: Guia de manutenção de seu site de rotina
description: Seja você um administrador, autor ou desenvolvedor, a manutenção do site abrange todos os aspectos da instância do AEM Sites. Use este guia para garantir que sua estratégia esteja configurada para ser bem-sucedida.
role: Admin
level: Beginner, Intermediate
topic: Administration
audience: author, marketer, developer
feature: Learn From Your Peers
exl-id: 37ee3234-f91c-4f0a-b0b7-b9167e7847a9
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '1084'
ht-degree: 5%

---

# Dicas e truques de manutenção do site

Há três opções quando se trata de instalar e manter uma instância de AEM

* AEMaaCS (Cloud Service) - o sistema está sempre ativo, atualizado e dimensionado dinamicamente conforme você precisa
* Adobe Managed Services, onde os engenheiros de atendimento ao cliente do Adobe fazem toda a manutenção diária/semanal/mensal e garantem que todos os service packs sejam instalados e que o sistema esteja sempre seguro e funcionando sem problemas
* executá-lo no local, onde você precisa cuidar de todo o sistema, incluindo backups, atualizações e segurança.

Se você optar por implementar seu próprio sistema localmente, lembre-se de algumas coisas para garantir que você tenha um sistema seguro e eficiente. Além dos itens &quot;cuidado e alimentação&quot;, este documento também apontará vários itens que os desenvolvedores de AEM devem ter em mente para ajudar a manter o sistema funcionando bem.

## Administradores

Backups - verifique se você tem backups completos e/ou parciais com uma programação frequente:

* diariamente
* semanalmente
* mensal

Muitos clientes fazem backups de snapshot, o que leva apenas alguns minutos, supondo que o sistema operacional subjacente ofereça suporte a esses backups. Verifique se esses backups estão armazenados corretamente (fora do sistema AEM). Verifique se os backups estão funcionando e podem ser usados para recriar um sistema em funcionamento periodicamente - não há nada pior do que o travamento do sistema e seus backups estão corrompidos por algum motivo!

Há vários itens que você precisa monitorar para garantir uma operação sem problemas:

### Manutenção de rotina

#### [manutenção de índice](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=pt-BR)

Os índices permitem que as consultas sejam executadas o mais rápido possível, liberando recursos para outras operações. Verifique se os índices estão na forma de topo de ponta! O AEM cancela consultas que viajam em vez de usar um índice para impedir que uma consulta ruim afete o desempenho geral do AEM.

#### [Limpeza de compactação/ revisão de TAR](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

Cada atualização no repositório cria uma nova revisão de conteúdo. Como resultado, a cada atualização o tamanho do repositório aumenta. Para evitar o crescimento descontrolado do repositório, as revisões antigas precisam ser removidas para liberar recursos de disco.

#### [Limpeza de binários do Lucene](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-dashboard.html#automated-maintenance-tasks)

Remova os binários do lucene e reduza o requisito de tamanho do armazenamento de dados em execução.

#### [Lixo do armazenamento de dados](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/data-store-garbage-collection.html)

Quando um ativo no AEM é excluído, a referência ao registro de armazenamento de dados subjacente pode ser removida da hierarquia do nó, mas o próprio registro de armazenamento de dados permanece. Esse registro de armazenamento de dados sem referência torna-se &quot;lixo&quot; que não precisa ser retido. Nos casos em que existem vários ativos sem referência, é útil eliminá-los para preservar espaço, otimizar o backup e o desempenho de manutenção do sistema de arquivos.

#### [Remoção do fluxo de trabalho](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/workflows-administering.html?lang=pt-BR)

Minimizar o número de instâncias de fluxo de trabalho aumenta o desempenho do motor de workflow. Portanto, você pode remover regularmente do repositório as instâncias de fluxo de trabalho concluídas ou em execução.

#### [Manutenção do Log de Auditoria](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-audit-log.html

Eventos AEM qualificados para registro de auditoria geram muitos dados arquivados. Esses dados podem crescer rapidamente com o tempo devido a replicações, uploads de ativos e outras atividades do sistema.

#### [Segurança](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=pt-BR)

Certifique-se de que as práticas recomendadas da Lista de verificação de segurança sejam seguidas cuidadosamente para garantir a instância mais segura do AEM.

#### Espaço em disco

Monitore o espaço em disco para garantir que você tenha espaço suficiente para o Repositório JCR, além de aproximadamente metade do espaço; a compactação tar usa espaço extra quando é executada. A falta de espaço em disco é o principal motivo para a corrupção do JCR!

## Desenvolvedor

Tentar não usar componentes personalizados - use [componentes principais](https://www.aemcomponents.dev/). Sua meta deve ser usar os componentes principais 80 a 90% do tempo e componentes personalizados com moderação. Isso geralmente requer uma nova maneira de observar os componentes em uma página. Você deve perceber que os componentes podem ser reestilizados facilmente por um desenvolvedor de front-end usando CSS. Lembre-se também de que esses componentes principais podem ser incorporados entre si para alcançar resultados bastante complexos. Seja criativo!

### [Sistemas de estilo](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

Os sistemas de estilo permitem que os componentes principais e até mesmo os componentes personalizados tenham sua aparência e comportamento alterados a critério dos Autores para criar componentes com aparência completamente nova. Essas mudanças estilísticas geralmente envolvem apenas um designer de front-end e um autor experiente (geralmente chamado de &quot;Super Autor&quot;)

### [Lançamentos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

Os lançamentos permitem que o trabalho seja concluído para uma nova promoção, venda ou implantação de site sem afetar as páginas implantadas atualmente. Além disso, eles podem ser programados para serem ativados automaticamente, sem participação ou supervisão, permitindo que os Autores façam o trabalho da próxima semana (ou do próximo trimestre) hoje e não entrem rapidamente no desenvolvimento da página no dia anterior ao lançamento - é realmente o presente do TIME!)

### [Fragmentos de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments.html)

Fragmentos de conteúdo são &quot;blocos&quot; personalizáveis de informações que podem ser facilmente reutilizadas em todo o site. Se você precisar de uma alteração, basta alterar o bloco original e a atualização é vista em todos os lugares em que é usada - imediatamente!

### [Fragmentos de experiência](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

Ao soar quase idêntico aos Fragmentos de conteúdo, os Fragmentos de experiência são pequenas partes visíveis de uma página. Eles também podem ser amplamente reutilizados em todo o site e mantidos em um local central no AEM para facilitar a tarefa de fazer alterações potencialmente globais no site em segundos, em vez de dias ou semanas.

Pense antes de ver o que pode ser reutilizado. Um rodapé? Um aviso? Um Cabeçalho? Certos tipos de conteúdo? Tudo isso pode ser compartilhado em todo o site, mantendo a manutenção no mínimo. Precisa atualizar uma data em um aviso de isenção de responsabilidade, mas ela está em 1.000 páginas no seu site? É uma operação de 5 segundos se você usou um Fragmento de experiência!

## Geral

Mantenha-se informado sobre as mudanças ocorridas no AEM através da aprendizagem contínua - não fique preso no passado. Uso [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) e [Serviços de aprendizado digital Adobe (ADLS)](https://learning.adobe.com/) para aprimorar suas habilidades.

## Conclusão

O AEM pode ser um sistema de grande porte, e são necessários muitos tipos de pessoas para fazê-lo &quot;cantar&quot;. De administradores a desenvolvedores (desenvolvedores de front-end e hardcore Java) a autores - há algo para todos! E se você não tem vontade de lidar com a administração cotidiana, sempre há AMS e AEM as a Cloud Service.
