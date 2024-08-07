---
title: O que é "A Dispatcher"
description: Entender o que é uma Dispatcher.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
duration: 73
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---

# O que é &quot;A Dispatcher&quot;

[Índice](./overview.md)

Começando pela descrição básica do que implica um Dispatcher AEM.

## Apache Web Server

Comece com uma instalação básica do Apache Web Server em um servidor Linux.

Explicação básica sobre o que um servidor Apache faz:

- Segue regras simples para servir arquivos por protocolos HTTP(s) de seu diretório de documentos estáticos (`DocumentRoot`)
- Os arquivos armazenados em um local padrão (`/var/www/html`) são correspondidos em solicitações e renderizados no navegador do cliente solicitante




## Arquivo de módulo específico de AEM (`mod_dispatcher.so`)

Em seguida, adicione um plug-in ao Apache Web Server chamado de módulo Dispatcher

Explicação básica sobre o que o módulo Adobe AEM Dispatcher faz:

- Aumenta o manipulador de arquivos padrão
- Filtra solicitações incorretas/Protege a barriga macia/pontos finais do AEM
- Balanceamentos de carga se mais de um renderizador estiver presente
- Permite um diretório de cache dinâmico / Suporta a liberação de arquivos estagnados
- É a porta de entrada para todas as instalações do AMS e fornece sites e ativos ao navegador do cliente
- Ele armazena solicitações em cache para serem atendidas em um ritmo muito mais rápido do que um servidor AEM poderia realizar sozinho
- Mais...

## Fluxo de trabalho de tráfego da Web

Entender que partes são instaladas juntas para criar um servidor Dispatcher básico nos leva a entender o fluxo de trabalho básico do tráfego da Web para uma configuração dos Serviços do gerenciador de Adobe.
Isso deve ajudá-lo a entender o papel que ele desempenha na cadeia de sistemas que fornecem conteúdo aos visitantes do seu conteúdo AEM.

<b>Vendo conteúdo já armazenado em cache</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>Veiculação de conteúdo novo do AEM</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if NOT found 
                → requests content from publisher 
                    → publisher sends content 
                        → Dispatcher adds content to cache and replies 
                            → End User
```

<b>Publicação/alterações de conteúdo</b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[Próximo -> Layout básico do arquivo](./basic-file-layout.md)
