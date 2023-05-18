---
title: Instalar as bibliotecas de reação de formulário adaptável necessárias
description: Adicione as dependências necessárias ao seu projeto de reação
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


# Instalação das dependências necessárias

Para começar a usar formulários adaptáveis sem periféricos em seu projeto de reação, instale as seguintes dependências em seu projeto de reação

* @aemforms/af-response-components
* @aemforms/af-response-renderer

Atualize o package.json para incluir as seguintes dependências. No momento da escrita 0.22.41 era a versão atual

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

## Configurar Proxy

O CORS (Cross-Origin Resource Sharing) é um mecanismo de segurança que impede os navegadores da Web de fazer solicitações a um domínio diferente daquele em que o aplicativo está hospedado. Podem ocorrer erros de CORS ao tentar buscar dados de uma API hospedada em um domínio diferente. Ao configurar um proxy, é possível ignorar as restrições de CORS e fazer solicitações à API pelo aplicativo React. Eu usei o seguinte código em um arquivo chamado setUpProxy.js na pasta src . **Certifique-se de alterar o target para apontar para a instância de publicação.**

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

Você também precisará instalar e adicionar o **http-proxy-middleware** ao seu projeto.

## Próximas etapas

[Buscar o formulário a ser incorporado](./fetch-the-form.md)