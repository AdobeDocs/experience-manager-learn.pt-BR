---
title: Instale as bibliotecas de reação de formulário adaptáveis necessárias
description: Adicione as dependências necessárias ao projeto do react
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: c6e83a627743c40355559d9cdbca2b70db7f23ed
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 1%

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

## Configurar proxy

O Compartilhamento de recursos entre origens (CORS) é um mecanismo de segurança que impede que os navegadores da Web façam solicitações para um domínio diferente daquele em que o aplicativo está hospedado. Os erros do CORS podem ocorrer ao tentar buscar dados de uma API hospedada em um domínio diferente. Ao configurar um proxy, você pode ignorar as restrições do CORS e fazer solicitações à API usando seu aplicativo React. Eu usei o seguinte código em um arquivo chamado setUpProxy.js na pasta src. **Altere o destino para apontar para a instância de publicação.**

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

Será necessário também instalar e adicionar o **http-proxy-middleware** para o seu projeto.

## Próximas etapas

[Buscar o formulário para incorporar](./fetch-the-form.md)