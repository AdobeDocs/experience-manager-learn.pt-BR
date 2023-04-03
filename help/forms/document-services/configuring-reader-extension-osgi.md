---
title: Configuração de extensões Reader no AEM Forms OSGi
description: Adicionar credencial de extensões Reader ao armazenamento de confiança no AEM Forms OSGi
feature: Reader Extensions
audience: developer
type: Tutorial
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Adicionar credencial de extensões Reader{#configuring-reader-extension-osgi}

O serviço DocAssurance pode aplicar direitos de utilização a documentos PDF. Para aplicar direitos de uso a documentos do PDF, configure os certificados.

## Criar keystore para o usuário do serviço fd

A credencial de extensões do leitor está associada ao usuário fd-service. Para adicionar a credencial ao usuário do fd-service, siga as etapas a seguir. Se você já criou o repositório de chaves para o usuário do serviço de fd, ignore esta seção

* Faça logon na instância do autor do AEM como um administrador
* Acesse Ferramentas-Segurança-Usuários
* Role para baixo na lista de usuários até encontrar a conta de usuário do fd-service
* Clique no usuário fd-service
* Clique na guia keystore
* Clique em Criar KeyStore
* Defina a Senha de Acesso do KeyStore e salve as configurações para criar a senha do KeyStore

### Adicionar credencial ao repositório de chaves do usuário do fd-service

Siga o vídeo para adicionar as credenciais ao usuário do fd-service

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=12&learn=on)


O comando para listar os detalhes do arquivo pfx é. O comando a seguir assume que você está no mesmo diretório que o arquivo pfx .

**keytool -v -list -storetype pkcs12 -keystore &lt;name of=&quot;&quot; your=&quot;&quot; pfx=&quot;&quot; file=&quot;&quot;>**

Por exemplo keytool -v -list -storetype pkcs12 -keystore 1005566.pfx onde 1005566.pfx é o nome do meu arquivo pfx
