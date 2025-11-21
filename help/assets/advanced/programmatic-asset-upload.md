---
title: Upload de ativo programático no AEM as a Cloud Service
description: Saiba como fazer upload de ativos para o AEM as a Cloud Service usando a biblioteca @adobe/aem-upload Node.js.
version: Experience Manager as a Cloud Service
topic: Development, Content Management
feature: Asset Management
role: Developer, Architect
level: Intermediate
last-substantial-update: 2025-11-14T00:00:00Z
doc-type: Tutorial
jira: KT-19571
thumbnail: KT-19571.png
source-git-commit: 151a5220ee842ee77ae27e99ded62f8d3dae4612
workflow-type: tm+mt
source-wordcount: '1585'
ht-degree: 1%

---


# Upload de ativo programático no AEM as a Cloud Service

Saiba como carregar ativos para o ambiente do AEM as a Cloud Service usando o aplicativo cliente que usa a biblioteca Node.js [aem-upload](https://github.com/adobe/aem-upload).


>[!VIDEO](https://video.tv.adobe.com/v/3476957?captions=por_br&quality=12&learn=on)


## O que você aprenderá

Neste tutorial, você aprenderá:

+ Como usar a abordagem de _upload binário direto_ para carregar ativos para o ambiente do AEM as a Cloud Service (RDE, Desenvolvimento, Preparo, Produção) usando a biblioteca Node.js [aem-upload](https://github.com/adobe/aem-upload).
+ Como configurar e executar o aplicativo [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) para carregar ativos para o ambiente do AEM as a Cloud Service.
+ Revise o código do aplicativo de amostra e entenda os detalhes de implementação.
+ Entenda as práticas recomendadas para o upload de ativos programático no ambiente do AEM as a Cloud Service.

## Entendendo a abordagem _upload binário direto_

A abordagem de _carregamento binário direto_ permite carregar arquivos do sistema de origem _diretamente para o armazenamento na nuvem_ no ambiente do AEM as a Cloud Service usando uma _URL pré-assinada_. Ele elimina a necessidade de rotear dados binários por meio dos processos Java da AEM, resultando em uploads mais rápidos e carga reduzida do servidor.

Antes de executar o aplicativo de amostra, vamos entender o fluxo de upload binário direto.

No fluxo de upload binário direto, os dados binários são carregados diretamente no armazenamento em nuvem com URLs pré-assinados. O AEM as a Cloud Service é responsável pelo processamento leve, como gerar os URLs pré-assinados e notificar o AEM Asset Compute Service sobre a conclusão do upload. O diagrama de fluxo lógico a seguir ilustra o fluxo de upload binário direto.

![Fluxo de carregamento binário direto](./assets/programmatic-asset-upload/direct-binary-asset-upload-flow.png)

### A biblioteca de upload do aem

A biblioteca Node.js [aem-upload](https://github.com/adobe/aem-upload) abstrai os detalhes de implementação da abordagem de _upload binário direto_. Ela fornece duas classes para orquestrar o processo de upload:

+ **FileSystemUpload** - Use-o ao carregar arquivos do sistema de arquivos local, incluindo suporte para estruturas de diretório
+ **DirectBinaryUpload** - Use-o para obter um controle mais polido sobre o processo de carregamento binário, como o carregamento de fluxos ou buffers

>[!CAUTION]
>
>NÃO há equivalente da biblioteca [aem-upload](https://github.com/adobe/aem-upload) em Java. O aplicativo cliente deve ser gravado em Node.js para usar a abordagem _upload binário direto_. Para obter informações adicionais, consulte a página [APIs e operações do Experience Manager Assets](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/assets/admin/developer-reference-material-apis#use-cases-and-apis).

## Aplicativo de amostra

Use o aplicativo [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) para saber mais sobre o processo de carregamento de ativos programático. O aplicativo de amostra demonstra o uso das classes `FileSystemUpload` e `DirectBinaryUpload` da biblioteca [aem-upload](https://github.com/adobe/aem-upload).

### Pré-requisitos

Antes de executar o aplicativo de amostra, verifique os seguintes pré-requisitos:

+ Ambiente de criação do AEM as a Cloud Service, como RDE (Rapid Development Environment, ambiente de desenvolvimento rápido), ambiente de desenvolvimento etc.
+ Node.js (versão LTS mais recente)
+ Noções básicas sobre Node.js e npm

>[!CAUTION]
>
> VOCÊ NÃO pode usar o AEM as a Cloud Service SDK (também conhecido como instância local do AEM) para testar o processo de upload de ativos programático. Você deve usar um ambiente AEM as a Cloud Service, como o RDE (Rapid Development Environment, ambiente de desenvolvimento rápido), o Dev environment etc.

### Baixar o aplicativo de amostra

1. Baixe o arquivo zip do aplicativo [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) e extraia-o.

   ```bash
   $ unzip aem-asset-upload-sample.zip
   ```

1. Abra a pasta extraída no editor de código favorito.

   ```bash
   $ cd aem-asset-upload-sample
   $ code .
   ```

1. Usando o terminal do editor de código, instale as dependências.

   ```bash
   $ npm install
   ```

   ![Aplicativo de exemplo](./assets/programmatic-asset-upload/install-dependencies.png)

### Configurar o aplicativo de amostra

Antes de executar o aplicativo de amostra, você deve configurá-lo com os detalhes necessários do ambiente do AEM as a Cloud Service, como a URL do autor do AEM, o _método de autenticação_ e o caminho da pasta de ativos.

Há _vários métodos de autenticação_ compatíveis com a biblioteca Node.js [aem-upload](https://github.com/adobe/aem-upload). A tabela a seguir resume os _métodos de autenticação_ com suporte e sua finalidade.

| | Autenticação básica | [Token de desenvolvimento local](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token) | [Credenciais de serviço](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials) | [S2S do OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/) | [Aplicativo Web OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-web-app-credential) | [SPA do OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-single-page-app-credential) |
|---|---|---|---|---|---|---|
| É compatível? | &verificar; | &verificar; | &verificar; | &cruz; | &cruz; | &cruz; |
| Propósito | Desenvolvimento local | Desenvolvimento local | Produção | N/A | N/A | N/A |

Para configurar o aplicativo de amostra, siga as etapas abaixo:

1. Copie o arquivo `env.example` para o arquivo `.env`.

   ```bash
   $ cp env.example .env
   ```

1. Abra o arquivo `.env` e atualize a variável de ambiente `AEM_URL` com a URL do autor do AEM as a Cloud Service.

1. Escolha o método de autenticação nas opções a seguir e atualize as variáveis de ambiente correspondentes.

>[!BEGINTABS]

>[!TAB Autenticação básica]

Para usar a autenticação básica, é necessário criar um usuário no ambiente do AEM as a Cloud Service.

1. Faça logon no ambiente do AEM as a Cloud Service.

1. Navegue até **Ferramentas** > **Segurança** > **Usuários** e clique no botão **Criar**.

   ![Criar usuário](./assets/programmatic-asset-upload/create-user.png)

1. Insira os detalhes do usuário

   ![Detalhes do usuário](./assets/programmatic-asset-upload/user-details.png)

1. Na guia **Grupos**, adicione o grupo **Usuários DAM**. Clique no botão **Salvar e fechar**.

   ![Adicionar grupo de usuários DAM](./assets/programmatic-asset-upload/add-dam-users-group.png)

1. Atualize as variáveis de ambiente `AEM_USERNAME` e `AEM_PASSWORD` com o nome de usuário e a senha do usuário criado.

>[!TAB Token de desenvolvimento local]

Para obter o token de desenvolvimento local, é necessário usar o Developer Console **AEM**. O token gerado é do tipo JSON Web Token (JWT).

1. Faça logon no [Adobe Cloud Manager](https://experience.adobe.com/#/@aem/cloud-manager) e navegue até a página de detalhes do **Ambiente** desejado. Clique no **&quot;...&quot;** e selecione **Developer Console**.

   ![Console do desenvolvedor](./assets/programmatic-asset-upload/developer-console.png)

1. Faça logon no AEM Developer Console e use o botão _Novo Console_ para alternar para o console mais recente.

1. Na seção **Ferramentas**, selecione **Integrações** e clique no botão **Obter token local**.

   ![Obter token local](./assets/programmatic-asset-upload/get-local-token.png)

1. Copie o valor do token e atualize a variável de ambiente `AEM_BEARER_TOKEN` com o valor do token.

Observe que o token de desenvolvimento local é válido por 24 horas e é emitido para o usuário que gerou o token.

>[!TAB Credenciais de serviço]

Para obter as credenciais do serviço, é necessário usar o Developer Console **AEM**. Ele é usado para gerar o token do tipo JSON Web Token (JWT) usando o módulo [jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm.

1. Faça logon no [Adobe Cloud Manager](https://experience.adobe.com/#/@aem/cloud-manager) e navegue até a página de detalhes do **Ambiente** desejado. Clique no **&quot;...&quot;** e selecione **Developer Console**.

   ![Console do desenvolvedor](./assets/programmatic-asset-upload/developer-console.png)

1. Faça logon no AEM Developer Console e use o botão _Novo Console_ para alternar para o console mais recente.

1. Na seção **Ferramentas**, selecione **Integrações** e clique no botão **Criar nova conta técnica**.

   ![Obter credenciais de serviço](./assets/programmatic-asset-upload/get-service-credentials.png)

1. Clique na opção **Exibir** para copiar as credenciais de serviço JSON.

   ![Credenciais de serviço](./assets/programmatic-asset-upload/service-credentials.png)

1. Crie um arquivo `service-credentials.json` na raiz do aplicativo de amostra e cole as credenciais de serviço JSON no arquivo.

1. Atualize a variável de ambiente `AEM_SERVICE_CREDENTIALS_FILE` com o caminho para o arquivo service-credentials.json.

1. Verifique se o usuário da credencial de serviço tem as permissões necessárias para carregar ativos no ambiente do AEM as a Cloud Service. Para obter mais informações, consulte [Configurar acesso na página AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials#configure-access-in-aem).

>[!ENDTABS]

Aqui está o arquivo de amostra `.env` completo com todos os três métodos de autenticação configurados.

```
# AEM Environment Configuration
# Copy this file to .env and fill in your AEM as a Cloud Service details

# AEM as a Cloud Service Author URL (without trailing slash)
# Example: https://author-p12345-e67890.adobeaemcloud.com
AEM_URL=https://author-p63947-e1733365.adobeaemcloud.com

# Upload Configuration
# Target folder in AEM DAM where assets will be uploaded
TARGET_FOLDER=/content/dam

# DirectBinaryUpload Remote URLs (required for DirectBinaryUpload example)
# URLs for remote files to upload in the DirectBinaryUpload example
# These demonstrate uploading from remote sources (URLs, CDNs, APIs)
REMOTE_FILE_URL_1=https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets

################################################################
# Authentication - Choose one of the following methods:
################################################################

# Method 1: Service Credentials (RECOMMENDED for production)
# Download service credentials JSON from AEM Developer Console and save it locally
# Then provide the path to the file here
AEM_SERVICE_CREDENTIALS_FILE=./service-credentials.json

# Method 2: Bearer Token Authentication (for manual testing)
AEM_BEARER_TOKEN=eyJhbGciOiJSUzI1NiIsIng1dSI6Imltc19uYTEta2V5LWF0LTEuY2VyIiwia2lkIjoiaW1zX25hM....fsdf-Rgt5hm_8FHutTyNQnkj1x1SUs5OkqUfJaGBaKBKdqQ

# Method 3: Basic Authentication (for development/testing only)
AEM_USERNAME=asset-uploader-local-user
AEM_PASSWORD=asset-uploader-local-user

# Optional: Enable detailed logging
DEBUG=false
```

### Execute o aplicativo de amostra

O aplicativo de amostra mostra três maneiras diferentes de fazer upload de ativos de amostra para o ambiente do AEM as a Cloud Service.

1. **FileSystemUpload** - Fazer upload de arquivos de um sistema de arquivos local com suporte à estrutura de diretórios e criação automática de pastas
1. **DirectBinaryUpload** - Carrega um [arquivo remoto](https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets). O binário do arquivo é armazenado em buffer na memória antes de ser carregado no ambiente do AEM as a Cloud Service.
1. **Carregamento em lote** - Faz upload de vários arquivos de um sistema de arquivos local em lotes com lógica de repetição automática e recuperação de erros. Em segundo plano, ele usa a classe `FileSystemUpload` para carregar arquivos do sistema de arquivos local.

Os ativos a serem carregados estão localizados na pasta `sample-assets` e contêm subpastas `img`, `video` e `doc`, cada uma contendo alguns ativos de amostra.

1. Para executar o aplicativo de amostra, use o seguinte comando:

```bash
$ npm start
```

1. Insira a opção desejada _número_ entre as seguintes opções:

```
╔════════════════════════════════════════════════════════════╗
║      AEM Asset Upload Sample Application                   ║
║      Demonstrating @adobe/aem-upload library               ║
╚════════════════════════════════════════════════════════════╝

Choose an upload method:

1. FileSystemUpload - Upload files from local filesystem with auto-folder creation
2. DirectBinaryUpload - Upload from remote URLs/streams to AEM
3. Batch Upload - Upload multiple files in batches with retry logic
4. Exit
```

As guias a seguir mostram a execução do aplicativo de amostra, sua saída e os ativos carregados no ambiente do AEM as a Cloud Service para cada método de upload.

>[!BEGINTABS]

>[!TAB UploadDoSistemaDeArquivos]

1. A saída do aplicativo de exemplo para a opção `FileSystemUpload`:

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 2.67s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. Assets carregado usando a opção `FileSystemUpload` no ambiente AEM as a Cloud Service:

   ![Ativos carregados no ambiente AEM as a Cloud Service usando a classe FileSystemUpload](./assets/programmatic-asset-upload/uploaded-assets-aem-file-system-upload.png)

>[!TAB DirectBinaryUpload]

1. A saída do aplicativo de exemplo para a opção `DirectBinaryUpload`:

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 1
Successful: 1
Total time: 561ms
──────────────────────────────────────────────────

✅ Successfully uploaded to AEM: https://author-p63947-e1733365.adobeaemcloud.com/ui#/aem/assets.html/content/dam?appId=aemshell
  → remote-file-1.png
    Source: https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets
✓ 
All files uploaded successfully!
```

1. Assets carregado usando a opção `DirectBinaryUpload` no ambiente AEM as a Cloud Service:

![Ativos carregados no ambiente AEM as a Cloud Service usando a classe DirectBinaryUpload](./assets/programmatic-asset-upload/uploaded-assets-aem-direct-binary-upload.png)

>[!TAB Carregamento em lote]

1. A saída do aplicativo de exemplo para a opção `Batch Upload`:

```bash
...
ℹ Found 4 item(s) to upload in batches (directories + files)
ℹ Batch size: 2 (small for demo, use 10-50 for production)

...

✓ Batch 2 completed in 2.79s

Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 4.50s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. Assets carregado usando a opção `Batch Upload` no ambiente AEM as a Cloud Service:

![Ativos carregados no ambiente AEM as a Cloud Service usando a classe BatchUpload](./assets/programmatic-asset-upload/uploaded-assets-aem-batch-upload.png)

>[!ENDTABS]

## Revisar o código do aplicativo de exemplo

O principal ponto de entrada do aplicativo de amostra é o arquivo `index.js`. Ele contém a função `promptUser` que solicita uma escolha ao usuário e executa o exemplo selecionado.

```javascript
/**
 * Prompts user for choice and executes the selected example
 */
function promptUser() {
  rl.question(chalk.bold('Enter your choice (1-4): '), async (answer) => {
    console.log('');

    try {
      switch (answer.trim()) {
        case '1':
          console.log(chalk.bold.green('\n▶ Running FileSystemUpload Example...\n'));
          await filesystemUpload.main();
          break;

        case '2':
          console.log(chalk.bold.green('\n▶ Running DirectBinaryUpload Example...\n'));
          await directBinaryUpload.main();
          break;

        case '3':
          console.log(chalk.bold.green('\n▶ Running Batch Upload Example...\n'));
          await batchUpload.main();
          break;

        case '4':
          rl.close();
          return;

        default:
          console.log(chalk.red('\n✗ Invalid choice. Please enter 1, 2, 3, or 4.\n'));
      }

      // After example completes, ask if user wants to continue
      rl.question(chalk.bold('\nPress Enter to return to menu or Ctrl+C to exit...'), () => {
        displayMenu();
        promptUser();
      });

    } catch (error) {
      console.error(chalk.red('\n✗ Error:'), error.message);
      rl.question(chalk.bold('\nPress Enter to return to menu...'), () => {
        displayMenu();
        promptUser();
      });
    }
  });
}
```

Para obter o código completo, consulte o arquivo `index.js` do aplicativo de amostra.

As guias a seguir mostram os detalhes de implementação de cada método de upload.

>[!BEGINTABS]

>[!TAB UploadDoSistemaDeArquivos]

A classe `FileSystemUpload` é usada para carregar arquivos do sistema de arquivos local com suporte à estrutura de diretório e criação automática de pastas.

```javascript
...
// Initialize FileSystemUpload
const upload = new FileSystemUpload();

const startTime = Date.now();
const spinner = createSpinner('Preparing upload...');

// Upload options for this specific upload
// For FileSystemUpload, the url should include the target folder path
const fullUrl = `${options.url}${targetFolder}`;

const uploadOptions = new FileSystemUploadOptions()
  .withUrl(fullUrl)
  .withDeepUpload(true);  // Enable recursive upload of subdirectories

// Add HTTP options including headers (auth is already in headers from config)
uploadOptions.withHttpOptions({
  headers: {
    ...options.headers,
    'X-Upload-Source': 'FileSystemUpload-Example'
  }
});

spinner.stop();

// Attach progress event handlers to the upload instance
handleUploadProgress(upload);

// Perform the upload and wait for completion
// Upload the contents (subdirectories and files) not the parent folder
const uploadResult = await upload.upload(uploadOptions, uploadPaths);
const totalTime = Date.now() - startTime;

// Analyze results using shared function
const analysis = analyzeUploadResult(uploadResult);

// Display summary
displayUploadSummary(analysis, totalTime);
...
```

Para obter o código completo, consulte o arquivo `examples/filesystem-upload.js` do aplicativo de amostra.

>[!TAB DirectBinaryUpload]

A classe `DirectBinaryUpload` é usada para carregar um arquivo remoto para o ambiente AEM as a Cloud Service.

```javascript
...
/**
 * Creates upload file objects for DirectBinaryUpload from remote URLs
 * @param {Array<Object>} remoteFiles - Array of objects with url, fileName, targetFolder
 * @returns {Array<Object>} Array of upload file objects
 */
async function createUploadFilesFromUrls(remoteFiles) {
  const uploadFiles = [];
  
  for (const remoteFile of remoteFiles) {
    logInfo(`Fetching: ${remoteFile.fileName} from ${remoteFile.url}`);
    try {
      const fileBuffer = await fetchRemoteFile(remoteFile.url);
      uploadFiles.push({
        fileName: remoteFile.fileName,
        fileSize: fileBuffer.length,
        blob: fileBuffer,  // DirectBinaryUpload uses 'blob' for buffers
        targetFolder: remoteFile.targetFolder,
        targetFile: `${remoteFile.targetFolder}/${remoteFile.fileName}`,
        sourceUrl: remoteFile.url  // Track source URL for display in summary
      });
      logSuccess(`Downloaded: ${remoteFile.fileName} (${formatBytes(fileBuffer.length)})`);
    } catch (error) {
      logError(`Failed to fetch ${remoteFile.fileName}: ${error.message}`);
    }
  }
  
  return uploadFiles;
}

...

    // Initialize DirectBinaryUpload
    const upload = new DirectBinaryUpload();

    // Fetch remote files and create upload objects
    const uploadFiles = await createUploadFilesFromUrls(remoteFiles);

...    

    // Upload options for each file
    const uploadOptions = new DirectBinaryUploadOptions()
        .withUrl(fullUrl)
        .withUploadFiles([uploadFile]);
    
    // Add HTTP options (auth is already in headers from config)
    uploadOptions
        .withHttpOptions({
        headers: {
            ...options.headers,
            'X-Upload-Source': 'DirectBinaryUpload-Example'
        }
        })
        .withMaxConcurrent(5);

    // Upload individual file and wait for completion
    const uploadResult = await upload.uploadFiles(uploadOptions);
```

Para obter o código completo, consulte o arquivo `examples/direct-binary-upload.js` do aplicativo de amostra.

>[!TAB Carregamento em lote]

Ele divide os arquivos em lotes e os carrega em lotes com lógica de repetição automática e recuperação de erros. Em segundo plano, ele usa a classe `FileSystemUpload` para carregar arquivos do sistema de arquivos local.

```javascript
...
async function uploadInBatches(paths, options, targetFolder, batchSize = 2) {
  const allResults = [];
  const totalPaths = paths.length;
  const totalBatches = Math.ceil(totalPaths / batchSize);

  logInfo(`Processing ${totalPaths} item(s) in ${totalBatches} batch(es)`);

  for (let i = 0; i < totalPaths; i += batchSize) {
    const batchNumber = Math.floor(i / batchSize) + 1;
    const batch = paths.slice(i, i + batchSize);
    
    console.log(`\n${'='.repeat(50)}`);
    logInfo(`Batch ${batchNumber}/${totalBatches} - Uploading ${batch.length} item(s)`);
    console.log('='.repeat(50));

    const batchStartTime = Date.now();
    let retryCount = 0;
    const maxRetries = 3;
    let batchResults = null;

    // Retry logic for failed batches
    while (retryCount <= maxRetries) {
      try {
        // Create a fresh upload instance for each retry to avoid duplicate event listeners
        const upload = new FileSystemUpload();
        
        const fullUrl = `${options.url}${targetFolder}`;
        
        const uploadOptions = new FileSystemUploadOptions()
          .withUrl(fullUrl)
          .withDeepUpload(true);  // Enable recursive upload of subdirectories
        
        // Add HTTP options including headers (auth is already in headers from config)
        uploadOptions.withHttpOptions({
          headers: {
            ...options.headers,
            'X-Upload-Source': 'Batch-Upload-Example',
            'X-Batch-Number': batchNumber
          }
        });

        // Track progress - attach listeners to upload instance
        upload.on('foldercreated', (data) => {
          logSuccess(`Created folder: ${data.folderName} at ${data.targetFolder}`);
        });
        
        let currentFile = '';
        upload.on('filestart', (data) => {
          currentFile = data.fileName;
          logInfo(`Starting: ${currentFile}`);
        });

        upload.on('fileprogress', (data) => {
          const percentage = ((data.transferred / data.fileSize) * 100).toFixed(1);
          process.stdout.write(
            `\r  Progress: ${percentage}% - ${formatBytes(data.transferred)}/${formatBytes(data.fileSize)}`
          );
        });

        upload.on('fileend', (data) => {
          process.stdout.write('\n');
          logSuccess(`Completed: ${data.fileName}`);
        });

        upload.on('fileerror', (data) => {
          // Only show in DEBUG mode (may be retries)
          if (process.env.DEBUG === 'true') {
            process.stdout.write('\n');
            const errorMsg = data.error?.message || data.message || 'Unknown error';
            logWarning(`Error (may retry): ${data.fileName} - ${errorMsg}`);
          }
        });

        // Perform upload and wait for batch completion
        const uploadResult = await upload.upload(uploadOptions, batch);
        
        const batchEndTime = Date.now();
        const batchTime = batchEndTime - batchStartTime;
        
        logSuccess(`Batch ${batchNumber} completed in ${formatTime(batchTime)}`);
        
        // Extract detailed results from the upload result
        batchResults = uploadResult.detailedResult || [];
        break; // Success, exit retry loop

      } catch (error) {
        retryCount++;
        if (retryCount <= maxRetries) {
          logWarning(`Batch ${batchNumber} failed. Retry ${retryCount}/${maxRetries}...`);
          await new Promise(resolve => setTimeout(resolve, 2000 * retryCount)); // Exponential backoff
        } else {
          logError(`Batch ${batchNumber} failed after ${maxRetries} retries: ${error.message}`);
          // Mark all files in batch as failed
          batchResults = batch.map(file => ({
            fileName: path.basename(file),
            error: error,
            success: false
          }));
        }
      }
    }

    if (batchResults) {
      allResults.push(...batchResults);
    }
  }

  return allResults;
}
```

Para obter o código completo, consulte o arquivo `examples/batch-upload.js` do aplicativo de amostra.

>[!ENDTABS]

Além disso, o arquivo `README.md` do aplicativo de amostra contém a documentação detalhada para o aplicativo de amostra.

## Práticas recomendadas

1. **Escolha o método de autenticação correto:**
Use credenciais de serviço para ambientes de produção, token de desenvolvimento local e autenticação básica apenas para desenvolvimento/teste. Verifique se o usuário da credencial de serviço tem as permissões necessárias para carregar ativos no ambiente do AEM as a Cloud Service.

1. **Escolha o método de carregamento correto:**
Use FileSystemUpload para arquivos locais com criação automática de pasta, DirectBinaryUpload para fluxos/buffers/URLs remotos com controle refinado e padrão de Upload em lote para ambientes de produção com mais de 1000 arquivos que exigem lógica de repetição.

1. **Objetos de arquivo DirectBinaryUpload de estrutura corretamente**
Use a propriedade blob (não buffer) com os campos obrigatórios: { fileName, fileSize, blob: buffer, targetFolder } e lembre-se de que DirectBinaryUpload NÃO cria pastas automaticamente.

1. **Aplicativo de exemplo como referência:**
O aplicativo de amostra é uma boa referência para os detalhes de implementação do processo de upload de ativos programáticos. Você pode usá-lo como ponto de partida para sua própria implementação do.
