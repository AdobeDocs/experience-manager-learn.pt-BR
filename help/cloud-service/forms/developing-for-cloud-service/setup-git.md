---
title: Instalar e configurar o Git
description: Inicializar o repositório Git local
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
duration: 48
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# Instalar o Git


[Instalar Git](https://git-scm.com/downloads). Você pode selecionar as configurações padrão e concluir o processo de instalação.
Ir para o prompt de comando
Navegue até c:\cloudmanager\aem-banking-app
digite git — version. Você deve ver a versão do GIT instalada em seu sistema

## Inicializar repositório Git local

Verifique se você está na pasta c:\cloudmanager\aem-banking-app

```
git init
```

O comando acima inicializará o projeto como um repositório local do Git

```
git add .
```

Isso adiciona todos os arquivos do projeto ao repositório Git prontos para serem confirmados no repositório Git

```
git commit -m "initial commit"
```

Isso confirma os arquivos no repositório Git



## Registrar o repositório do Cloud Manager com nosso repositório Git local

Acesse seu repositório do Cloud Manager
![acesse as informações do representante](assets/cloud-manager-repo.png)
Obter as credenciais do repositório do Cloud Manager
![obter-credenciais](assets/cloud-manager-repo1.png)

Salvar o nome de usuário no arquivo de configuração

```java
git config --global credential.username "gbedekar-adobe-com"
```

salvar a senha no arquivo de configuração

```java
git config --global user.password "XXXX"
```

(A senha é sua senha do repositório Git do cloud manager)

Registre o repositório Git do cloud manager com seu repositório Git local. O comando abaixo associa **bankingapp** ao repositório Git do gerenciador de nuvem remoto. Você poderia ter usado qualquer nome em vez de **bankingapp**


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

(Certifique-se de usar o URL do repositório)

Verificar se o repositório remoto está registrado

```java
git remote -v
```

## Próximas etapas

[Sincronizar o AEM com o projeto AEM no IntelliJ](./intellij-and-aem-sync.md)
