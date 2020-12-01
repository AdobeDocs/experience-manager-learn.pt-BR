---
title: Usar mestres do projeto em AEM
description: O Project Masters simplifica bastante o gerenciamento de usuários e de equipes com AEM Projetos.
version: 6.4, 6.5
feature: projects, users-and-groups
topics: administration, collaboration, performance
activity: use
audience: administrator, implementer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---


# Usar páginas-mestre do projeto

O Project Masters simplifica consideravelmente o gerenciamento de usuários e equipes com [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740/?quality=9&learn=on)

Agora, os administradores podem criar um **[!DNL Master Project]** e atribuir usuários a funções/permissões como parte de uma Equipe do projeto. Os projetos podem ser criados a partir de um Projeto Principal e herdarão automaticamente a associação à Equipe. Isso oferta várias vantagens:

* Reutilizar equipes existentes em vários projetos
* Acelera a criação de projetos, pois as equipes não precisam ser recriadas manualmente
* Gerenciar associação de equipe a partir de um local central e quaisquer atualizações para equipes são automaticamente herdadas pelos Projetos
* evita a criação de ACLs de duplicado que podem causar problemas de desempenho

[!DNL Master Projects] pode ser criado na   pasta Mestre em  [!UICONTROL AEM Projetos]. Depois que um [!DNL Master Project] for criado, ele será exibido como uma opção junto aos modelos disponíveis no assistente quando novos Projetos forem criados.

[!DNL Project Masters] URL (instância local do autor de AEM):  [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Exclua [!DNL Project Masters]

Excluir um projeto principal resulta em projetos derivados inutilizáveis.

Antes de excluir um projeto principal, verifique se todos os projetos derivados foram concluídos e removidos da AEM. Certifique-se de salvar todos os dados necessários do projeto antes de remover os projetos derivados. Quando todos os projetos derivados forem removidos do AEM, o projeto principal poderá ser excluído com segurança.

## Marcar [!DNL Project Masters] como Inativo

Ao alterar o status do projeto principal para inativo nas propriedades do projeto, os projetos principais inativos desaparecem da lista de projetos principais.

Para mostrar projetos principais inativos, alterne o botão de filtro &quot;mostrar ativos&quot; na barra superior (ao lado da alternância de exibição de lista). Para tornar o projeto inativo ativo novamente, basta selecionar o projeto principal inativo, editar as propriedades do projeto e configurá-lo novamente para estar ativo.

## Entenda [!DNL Project Masters]

![Visualização técnica dos projetos](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] trabalhe definindo um conjunto de grupos de usuários AEM (proprietários, editor e observador) e permitindo que os Projetos derivados façam referência e reutilizem esses grupos de usuários definidos centralmente.

Isso reduz o número geral de grupos de usuários necessários no AEM. Antes de [!DNL Project Masters], cada projeto criava 3 grupos de usuários com os ACEs acompanhantes para aplicar o controle de permissões, portanto 100 projetos resultavam em 300 grupos de usuários. O Project Masters permite que qualquer número de Projetos reutilize os mesmos três grupos, assumindo que a associação compartilhada se alinha às necessidades de negócios do projeto.
