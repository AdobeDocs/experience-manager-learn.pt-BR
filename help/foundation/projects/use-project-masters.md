---
title: Como usar o Project Masters no AEM
description: Projetos Mestres simplifica muito o gerenciamento de usuários e equipes com projetos AEM.
version: 6.4, 6.5, Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
kt: 256
thumbnail: 17740.jpg
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# Usar projetos mestre

O Project Masters simplifica muito o gerenciamento de usuários e equipes com [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740?quality=12&learn=on)

Agora, os administradores podem criar uma **[!DNL Master Project]** e atribuir usuários a funções/permissões como parte de uma Equipe do projeto. Os projetos podem ser criados a partir de um Projeto Principal e herdam automaticamente a associação à Equipe. Isso oferece várias vantagens:

* Reutilizar equipes existentes em vários projetos
* Acelera a criação de projetos, já que as equipes não precisam ser recriadas manualmente
* Gerenciar associação de equipe de um local central e quaisquer atualizações para Equipes são herdadas automaticamente pelos Projetos
* evita a criação de ACLs duplicadas que podem causar problemas de desempenho

[!DNL Master Projects] pode ser criado no [!UICONTROL Mestre] pasta em [!UICONTROL Projetos AEM]. Depois que um projeto Principal é criado, ele é exibido como uma opção junto com os modelos disponíveis no assistente quando novos projetos são criados.

[!DNL Project Masters] URL (instância local do autor do AEM): [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## Excluir [!DNL Project Masters]

Excluir um projeto principal resulta em projetos derivados inutilizáveis.

Antes de excluir um projeto principal, verifique se todos os projetos derivados foram concluídos e removidos do AEM. Salve todos os dados necessários do projeto antes de remover os projetos derivados. Depois que todos os projetos derivados forem removidos do AEM, o projeto principal poderá ser excluído com segurança.

## Marcar [!DNL Project Masters] como inativo

Ao alterar o status do projeto principal para inativo nas propriedades do projeto, os projetos principais inativos desaparecem da lista de projetos principais.

Para mostrar projetos principais inativos, alterne o botão de filtro &quot;mostrar ativos&quot; na barra superior (ao lado do botão de exibição da lista). Para tornar o projeto inativo novamente ativo, basta selecionar o projeto principal inativo, editar as propriedades do projeto e configurá-lo novamente para ficar ativo.

## Compreender [!DNL Project Masters]

![Exibição técnica de mestres de projeto](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] trabalhe definindo um conjunto de grupos de usuários do AEM (proprietários, editor e observador) e permitindo que os projetos derivados referenciem e reutilizem esses grupos de usuários definidos centralmente.

Isso reduz o número geral de grupos de usuários necessários no AEM. Antes [!DNL Project Masters], cada projeto criou três grupos de usuários com as ACEs associadas para aplicar a permissão, portanto, 100 projetos geraram 300 grupos de usuários. Projetos mestre permite que qualquer número de projetos reutilize os mesmos três grupos, supondo que a associação compartilhada esteja alinhada aos requisitos de negócios no projeto.
