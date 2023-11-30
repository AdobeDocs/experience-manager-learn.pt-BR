---
title: Suporte à substituição de configuração sensível ao contexto para o modelo de dados de formulário
description: Configurar modelos de dados de formulário para se comunicar com diferentes pontos de extremidade com base em ambientes.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-10423
exl-id: 2ce0c07b-1316-4170-a84d-23430437a9cc
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 11%

---

# Configurações da nuvem com reconhecimento de contexto

Ao criar a configuração de nuvem no ambiente local e após um teste bem-sucedido, você desejaria usar a mesma configuração de nuvem nos ambientes upstream, mas sem precisar alterar o endpoint, a chave/senha secreta e/ou o nome de usuário. Para obter esse caso de uso, o AEM Forms no Cloud Service introduziu a capacidade de definir configurações de nuvem com reconhecimento de contexto.
Por exemplo, a configuração da nuvem da conta de armazenamento do Azure pode ser reutilizada em ambientes de desenvolvimento, preparo e produção usando diferentes cadeias de conexão e chaves para.

As etapas a seguir são necessárias para criar a configuração da nuvem com reconhecimento de contexto

## Criar variáveis de Ambiente

As variáveis de ambiente padrão podem ser configuradas e gerenciadas pelo Cloud Manager. Elas são fornecidas para o ambiente de tempo de execução e podem ser usados nas configurações do OSGi. [As variáveis de ambiente podem ser valores específicos ou segredos do ambiente, com base no que está sendo alterado.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=en)



A captura de tela a seguir mostra as variáveis de ambiente azure_key e azure_connection_string definidas
![environment_variables](assets/environment-variables.png)

Essas variáveis de ambiente podem ser especificadas nos arquivos de configuração a serem usados no ambiente apropriado. Por exemplo, se você quiser que todas as instâncias do autor usem essas variáveis de ambiente, você definirá o arquivo de configuração na pasta config.author conforme especificado abaixo

## Criar arquivo de configuração

Abra o projeto no IntelliJ. Navegue até config.author e crie um arquivo chamado

```java
org.apache.sling.caconfig.impl.override.OsgiConfigurationOverrideProvider-integrationTest.cfg.json
```

![config.author](assets/config-author.png)

Copie o texto a seguir no arquivo criado na etapa anterior. O código neste arquivo está substituindo o valor das propriedades accountName e accountKey pelas variáveis de ambiente **azure_connection_string** e **azure_key**.

```json
{
  "enabled":true,
  "description":"dermisITOverrideConfig",
  "overrides":[
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountName=\"$[env:azure_connection_string]\"",
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountKey=\"$[secret:azure_key]\""

  ]
}
```

>[!NOTE]
>
>Essa configuração se aplicará a todos os ambientes de autor na instância do Cloud Service. Para aplicar a configuração a ambientes de publicação, você terá que colocar o mesmo arquivo de configuração na pasta config.publish do projeto intelliJ
>[!NOTE]
> Certifique-se de que a propriedade que está sendo substituída é uma propriedade válida da configuração de nuvem. Navegue até a configuração da nuvem para localizar a propriedade que você deseja substituir, conforme mostrado abaixo.

![cloud-config-property](assets/cloud-config-properties.png)

Para configurações de nuvem baseadas em REST com autenticação básica, normalmente é desejável criar variáveis de ambiente para propriedades serviceEndPoint, userName e password.

## Próximas etapas

[Envie seu projeto AEM para o Cloud Manager](./push-project-to-cloud-manager-git.md)
