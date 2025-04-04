---
title: Instale as bibliotecas de reação de formulário adaptáveis necessárias
description: Adicione as dependências necessárias ao projeto do react
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: 0ed44016-d52a-4980-a0b1-06da149c3cb1
duration: 48
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# Instalando as dependências necessárias

Para começar a usar formulários adaptáveis headless no projeto do react, instale as seguintes dependências no projeto do react

* @aemforms/af-response-components
* @aemforms/af-response-renderer

Atualize o package.json para incluir as dependências a seguir. No momento da escrita 0.22.41 era a versão atual

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

>[!NOTE]
>
>A lista suspensa e o layout do cartão neste tutorial foram criados usando a [Biblioteca de Interface do Usuário do Material](https://mui.com/). Você precisará baixar os pacotes de Material UI apropriados para que o código funcione em seu sistema.

## Configurar proxy

O Compartilhamento de recursos entre origens (CORS) é um mecanismo de segurança que impede que os navegadores da Web façam solicitações para um domínio diferente daquele em que o aplicativo está hospedado. Os erros do CORS podem ocorrer ao tentar buscar dados de uma API hospedada em um domínio diferente. Ao configurar um proxy, você pode ignorar as restrições do CORS e fazer solicitações à API usando seu aplicativo React. Eu usei o seguinte código em um arquivo chamado setUpProxy.js na pasta src. **Altere o destino para apontar para sua instância de publicação.**

```
const { createProxyMiddleware } = require('http-proxy-middleware');
const proxy = {
    target: 'https://mypublishinstance:4503/',
    changeOrigin: true
}
module.exports = function(app) {
  app.use(
    '/adobe',
    createProxyMiddleware(proxy)
  ),
  app.use(
    '/content',
    createProxyMiddleware(proxy)
  );
};
```

Você também precisará instalar e adicionar o módulo **http-proxy-middleware** ao seu projeto.

## Próximas etapas

[Buscar o formulário para incorporar](./fetch-the-form.md)
