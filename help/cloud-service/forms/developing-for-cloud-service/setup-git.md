---
title: Instalar e configurar o Git
description: Inicializar seu repositório Git local
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Instalar Git


[Instalar Git](https://git-scm.com/downloads). Você pode selecionar as configurações padrão e concluir o processo de instalação.
Vá para o prompt de comando Navegue até c:\cloudmanager\aem-banking-app type in git —version. Você deve ver a versão do GIT instalada em seu sistema

## Inicializar Repositório Git Local

Verifique se você está na pasta c:\cloudmanager\aem-banking-app folder

```
git init
```

O comando acima inicializará o projeto como um repositório local Git

```
git add .
```

Isso adiciona todos os arquivos de projeto ao repositório Git pronto para ser confirmado no repositório Git

```
git commit -m "initial commit"
```

Isso confirma os arquivos no repositório Git



## Registre o repositório do cloud manager com nosso repositório Git local

Acesse seu repositório do cloud manager
![acessar as informações do rep](assets/cloud-manager-repo.png)
Obter as credenciais do repo do cloud manager
![get-credentials](assets/cloud-manager-repo1.png)

Salve o nome de usuário no arquivo de configuração

```java
git config --global credential.username "gbedekar-adobe-com"
```

salvar a senha no arquivo de configuração

```java
git config --global user.password "XXXX"
```

(A senha é a senha do repositório git do cloud manager)

Registre o repositório git do cloud manager com seu repositório Git local. O comando abaixo associa **aplicativo bancário** com o repositório git do gerenciador de nuvem remoto. Você poderia ter usado qualquer nome em vez de **aplicativo bancário**


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

(Certifique-se de usar o URL do repositório)

Verifique se o repositório remoto está registrado

```java
git remote -v
```
