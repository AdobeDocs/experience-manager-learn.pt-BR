---
title: O que é "O Dispatcher"
description: Entenda o que é realmente um Dispatcher.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 829ad9733b4326c79b9b574b13b1d4c691abf877
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---


# O que é &quot;O Dispatcher&quot;

[Índice](./overview.md)

Começando com a descrição básica do que implica um Dispatcher AEM.

## Apache Web Server

Comece com uma instalação básica do Apache Web Server em um servidor Linux.

Explicação básica do que um servidor Apache faz:

- Segue regras simples para veicular arquivos através dos protocolos HTTP(s) a partir de seu diretório de documento estático (`DocumentRoot`)
- Arquivos armazenados em um local padrão (`/var/www/html`) são correspondidos nas solicitações e renderizadas no navegador do cliente solicitante




## AEM arquivo de módulo específico (`mod_dispatcher.so`)

Em seguida, adicione um plug-in ao Apache Web Server chamado o módulo Dispatcher

Explicação básica do que o módulo Dispatcher do Adobe AEM faz:

- Aumenta o manipulador de arquivos padrão
- Filtra solicitações incorretas / Protege AEM barriga suave/pontos finais
- Balanceamento de carga se houver mais de um renderizador
- Permite um diretório de cache dinâmico / Suporta a liberação de arquivos estagnados
- É a porta de entrada de todas as instalações do AMS e fornece sites e ativos ao navegador do cliente
- Armazena em cache as solicitações para reservir a uma taxa muito mais rápida do que um servidor AEM poderia realizar por conta própria
- Muito mais...

## Fluxo de trabalho do tráfego da Web

Entender quais partes são instaladas juntas para criar um servidor básico do Dispatcher faz com que você entenda o fluxo de trabalho básico do tráfego na Web para uma configuração dos Serviços do Adobe Manager.
Isso deve ajudá-lo a entender qual função ele desempenha na cadeia de sistemas que veiculam conteúdo para os visitantes de seu conteúdo AEM.

<b>Veiculação de conteúdo já armazenado em cache</b>

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