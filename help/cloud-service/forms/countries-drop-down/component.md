---
title: Criar a estrutura para o componente de países
description: Criar a estrutura do componente no crx
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 2%

---

# Criar a estrutura para o componente de países

Faça logon na instância do AEM Forms e siga estas etapas para criar um novo componente com base no componente suspenso pronto para uso:

1. Navegue até &#39;/apps/&lt;yourproject>/components/adaptiveForm/dropdown&#39; no CRXDE Lite.
2. Copie o componente suspenso e cole-o no mesmo nível de diretório.
3. Renomeie o componente copiado para os países.
4. Atualize a propriedade jcr:title do nó cq:template para Países.
5. Salve as alterações.

Agora você tem um novo componente chamado Países, que é uma réplica exata do componente suspenso predefinido. Isso serve como base para personalização adicional.

## Criar o arquivo HTL

Para criar o arquivo HTL para o componente Países:

1. Navegue até a pasta de países no repositório crx
2. Crie um novo arquivo chamado países.html.
3. Abra o arquivo /apps/core/fd/components/form/dropdown/v1/dropdown/dropdown.html no repositório crx e copie seu conteúdo.
4. Cole o conteúdo copiado em countries.html.
5. Altere o código para usar o novo modelo do sling, como mostrado na captura de tela
6. Salve as alterações.

![modelo-sling](assets/countriesdropdown.png)

Por fim, sincronize seu projeto com essas atualizações para garantir que as alterações no repositório do CRX sejam refletidas em seu projeto AEM.


## Próximas etapas

[Criar cq-dialog](./dialog.md)
