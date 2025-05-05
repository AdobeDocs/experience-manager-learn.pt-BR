---
title: Configuração de extensões do Reader no OSGi do AEM Forms
description: Adicionar a credencial Reader Extensions ao armazenamento de confiança no OSGi do AEM Forms
feature: Reader Extensions
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
duration: 308
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Adicionar credencial de extensões do Reader{#configuring-reader-extension-osgi}

O serviço DocAssurance pode aplicar direitos de uso a documentos do PDF. Para aplicar direitos de uso a documentos do PDF, configure os certificados.

## Criar armazenamento de chaves para o usuário do serviço fd

A credencial de extensões do leitor está associada ao usuário do serviço de fd. Para adicionar a credencial ao usuário do serviço fd, siga as etapas a seguir. Se você já criou o armazenamento de chaves para o usuário do serviço fd, ignore esta seção

* Faça logon na instância do autor do AEM como administrador
* Vá para Ferramentas-Segurança-Usuários
* Role a lista de usuários até encontrar a conta de usuário do serviço fd
* Clique no usuário do serviço fd
* Clique na guia Armazenamento de chaves
* Clique em Criar KeyStore
* Defina a Senha de acesso do KeyStore e salve suas configurações para criar a senha do KeyStore

### Adicionar credencial ao armazenamento de chaves do usuário do serviço de fd

Siga o vídeo para adicionar as credenciais ao usuário do serviço fd

>[!VIDEO](https://video.tv.adobe.com/v/3447292?quality=12&learn=on&captions=por_br)


O comando para listar os detalhes do arquivo pfx é. O comando a seguir presume que você esteja no mesmo diretório do arquivo pfx.

**keytool -v -list -storetype pkcs12 -keystore &lt;nome do arquivo .pfx>**

Por exemplo keytool -v -list -storetype pkcs12 -keystore 1005566.pfx onde 1005566.pfx é o nome do meu arquivo pfx
