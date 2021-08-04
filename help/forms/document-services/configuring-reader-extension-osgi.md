---
title: Configuração de extensões Reader no AEM Forms OSGi
description: Adicionar credencial de extensões Reader ao armazenamento de confiança no AEM Forms OSGi
feature: Extensões Reader
feature-set: Reader Extensions
topics: development
audience: developer
doc-type: Tutorial
activity: implement
version: 6.4,6.5
topic: Administração
role: Admin
level: Beginner
source-git-commit: 2fc4f748fd3b8f820d1451d08c5fe01d11892029
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 0%

---


# Adicionar credencial de extensões Reader{#configuring-reader-extension-osgi}

O serviço DocAssurance pode aplicar direitos de uso a documentos PDF. Para aplicar direitos de uso a documentos PDF, configure os certificados.

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

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=9&learn=on)


O comando para listar os detalhes do arquivo pfx é. O comando a seguir assume que você está no mesmo diretório que o arquivo pfx .

**keytool -v -list -storetype pkcs12 -keystore  &lt;name of=&quot;&quot; your=&quot;&quot;>**

Por exemplo keytool -v -list -storetype pkcs12 -keystore 1005566.pfx onde 1005566.pfx é o nome do meu arquivo pfx













