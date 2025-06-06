---
title: Como usar Projetos mestres no AEM
description: Projetos mestre simplifica muito o gerenciamento de usuários e equipes com projetos AEM.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
jira: KT-256
thumbnail: 17740.jpg
doc-type: Feature Video
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
duration: 260
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# Usar projetos mestre

Os Projetos Mestres simplificam muito o gerenciamento de usuários e equipes com o [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/3418847?quality=12&learn=on&captions=por_br)

Agora, os administradores podem criar um **[!DNL Master Project]** e atribuir usuários a funções/permissões como parte de uma Equipe do Projeto. Os projetos podem ser criados a partir de um projeto mestre e herdam automaticamente a associação Equipe. Isso oferece várias vantagens:

* Reutilizar equipes existentes em vários projetos
* Acelera a criação de projetos, já que as equipes não precisam ser recriadas manualmente
* Gerenciar associação de equipe de um local central e quaisquer atualizações para Equipes são herdadas automaticamente pelos Projetos
* evita a criação de ACLs duplicadas que podem causar problemas de desempenho

[!DNL Master Projects] pode ser criado na pasta [!UICONTROL Mestres] em [!UICONTROL Projetos AEM]. Depois que um projeto mestre é criado, ele é exibido como uma opção junto com os modelos disponíveis no assistente quando novos projetos são criados.

URL [!DNL Project Masters] (instância de Autor do AEM local): [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Excluir [!DNL Project Masters]

A exclusão de um projeto principal resulta em projetos derivados inutilizáveis.

Antes de excluir um projeto principal, verifique se todos os projetos derivados foram concluídos e removidos do AEM. Salve todos os dados necessários do projeto antes de remover os projetos derivados. Depois que todos os projetos derivados forem removidos do AEM, o projeto principal poderá ser excluído com segurança.

## Marcar [!DNL Project Masters] como Inativo

Ao alterar o status do projeto mestre para inativo nas propriedades do projeto, os projetos mestre inativos desaparecem da lista de projetos mestre.

Para mostrar projetos mestres inativos, alterne o botão de filtro &quot;mostrar ativos&quot; na barra superior (ao lado do botão de exibição da lista). Para tornar o projeto inativo novamente ativo, basta selecionar o projeto mestre inativo, editar as propriedades do projeto e configurá-lo novamente para ficar ativo.

## Entender [!DNL Project Masters]

![Modo de exibição técnico de mestres de projeto](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] trabalha definindo um conjunto de grupos de usuários do AEM (proprietários, editor e observador) e permitindo que os projetos derivados referenciem e reutilizem esses grupos de usuários definidos centralmente.

Isso reduz o número geral de grupos de usuários necessários no AEM. Antes de [!DNL Project Masters], cada projeto criava três grupos de usuários com as ACEs associadas para aplicar a permissão. Assim, 100 projetos geravam 300 grupos de usuários. Projetos mestre permite que qualquer número de projetos reutilize os mesmos três grupos, supondo que a associação compartilhada esteja alinhada aos requisitos de negócios no projeto.
