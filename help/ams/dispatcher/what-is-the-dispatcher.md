---
title: O que é "A Dispatcher"
description: Entender o que é uma Dispatcher.
version: Experience Manager 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
duration: 73
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---

# O que é &quot;A Dispatcher&quot;

[Índice](./overview.md)

Começando com a descrição básica do que implica uma AEM Dispatcher.

## Apache Web Server

Comece com uma instalação básica do Apache Web Server em um servidor Linux.

Explicação básica sobre o que um servidor Apache faz:

- Segue regras simples para servir arquivos por protocolos HTTP(s) de seu diretório de documentos estáticos (`DocumentRoot`)
- Os arquivos armazenados em um local padrão (`/var/www/html`) são correspondidos em solicitações e renderizados no navegador do cliente solicitante




## Arquivo de módulo específico do AEM (`mod_dispatcher.so`)

Em seguida, adicione um plug-in ao Apache Web Server chamado de módulo Dispatcher

Explicação básica sobre o que o módulo Adobe AEM Dispatcher faz:

- Aumenta o manipulador de arquivos padrão
- Filtra solicitações incorretas/Protege os pontos de extremidade/barriga macia do AEM
- Balanceamentos de carga se mais de um renderizador estiver presente
- Permite um diretório de cache dinâmico / Suporta a liberação de arquivos estagnados
- É a porta de entrada para todas as instalações do AMS e fornece sites e ativos ao navegador do cliente
- Ele armazena solicitações em cache para fazer a reatendimento a uma taxa muito mais rápida do que um servidor AEM poderia fazer sozinho
- Mais...

## Fluxo de trabalho de tráfego da Web

Entender quais partes são instaladas juntas para criar um servidor básico do Dispatcher nos leva a entender o fluxo de trabalho básico do tráfego da Web para uma configuração dos Serviços do Adobe Manager.
Isso deve ajudá-lo a entender qual papel ele desempenha na cadeia de sistemas que fornecem conteúdo aos visitantes do seu conteúdo do AEM.

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
