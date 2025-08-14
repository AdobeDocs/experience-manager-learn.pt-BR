---
title: Visualizar uma extensão do editor universal
description: Saiba como visualizar uma extensão do editor universal executado localmente durante o desenvolvimento.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner, Intermediate, Experienced
doc-type: Tutorial
jira: KT-18658
source-git-commit: f0ad5d66549970337118220156d7a6b0fd30fd57
workflow-type: ht
source-wordcount: '306'
ht-degree: 100%

---


# Visualizar uma extensão local do editor universal

>[!TIP]
> Saiba como [criar uma extensão do editor universal](https://developer.adobe.com/uix/docs/services/aem-universal-editor/).

Para visualizar uma extensão do editor universal durante o desenvolvimento, é necessário:

1. Executar a extensão localmente.
2. Aceitar o certificado autoassinado.
3. Abrir uma página no editor universal.
4. Atualizar o URL da localização para carregar a extensão local.

## Executar a extensão localmente

Isto pressupõe que você já tenha criado uma [extensão do editor universal](https://developer.adobe.com/uix/docs/services/aem-universal-editor/) e deseja visualizá-la durante os testes e o desenvolvimento local.

Inicie a sua extensão do editor universal com:

```bash
$ aio app run
```

Você verá resultados como:

```
To view your local application:
  -> https://localhost:9080
To view your deployed application in the Experience Cloud shell:
  -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
```

Por padrão, esta opção executa a sua extensão em `https://localhost:9080`.


## Aceitar o certificado autoassinado

O editor universal requer HTTPS para carregar extensões. Como o desenvolvimento local usa um certificado autoassinado, o seu navegador precisar confiar nele explicitamente.

Abra uma nova guia do navegador e navegue até a saída do URL da extensão local por meio do comando `aio app run`:

```
https://localhost:9080
```

O seu navegador mostrará um aviso de certificado. Aceite o certificado para continuar.

![Aceitar o certificado autoassinado](./assets/local-extension-preview/accept-certificate.png)

Depois de aceito, você verá a página de espaço reservado da extensão local:

![A extensão está acessível](./assets/local-extension-preview/extension-accessible.png)


## Abrir uma página no editor universal

Abra o editor universal por meio do [console do editor universal](https://experience.adobe.com/#/@myOrg/aem/editor/canvas/) ou da edição de uma página no AEM Sites que utilize o editor universal:

![Abrir uma página no editor universal](./assets/local-extension-preview/open-page-in-ue.png)


## Carregar a extensão

No editor universal, procure o campo **Localização** na parte superior central da interface. Expanda e atualize o **URL no campo “Localização”**, **não a barra de endereço do navegador**.

Anexe os seguintes parâmetros de consulta:

* `devMode=true`: habilita o modo de desenvolvimento para o editor universal.
* `ext=https://localhost:9080`: carrega a extensão executada localmente.

Exemplo:

```
https://author-pXXX-eXXX.adobeaemcloud.com/content/aem-ue-wknd/index.html?devMode=true&ext=https://localhost:9080
```

![Atualizar o URL da localização do editor universal](./assets/local-extension-preview/update-location-url.png)


## Visualizar a extensão

Execute um **carregamento forçado** do navegador para garantir que o URL atualizado seja usado.

O editor universal carregará a sua extensão local agora, somente na sua sessão do navegador.

Quaisquer alterações feitas localmente no código serão refletidas imediatamente.

![Extensão local carregada](./assets/local-extension-preview/extension-loaded.png)

