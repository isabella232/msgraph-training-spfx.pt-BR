---
ms.openlocfilehash: 5164c68a78837c179636f685d28ddf61f93f8415
ms.sourcegitcommit: 523e64e972f247e2df57cd8de949ac7bd1b8b047
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "48823265"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ed723-101">Neste tutorial, você criará uma [Web Part do lado do cliente do SharePoint](https://docs.microsoft.com/sharepoint/dev/spfx/web-parts/overview-client-side-web-parts) que usará o Microsoft Graph para obter o calendário do usuário para a semana atual e permitir que o usuário adicione um evento ao seu calendário.</span><span class="sxs-lookup"><span data-stu-id="ed723-101">In this tutorial, you'll create a [SharePoint client-side web part](https://docs.microsoft.com/sharepoint/dev/spfx/web-parts/overview-client-side-web-parts) that will use Microsoft Graph to get the user's calendar for the current week and allow the user to add an event to their calendar.</span></span>

## <a name="create-a-web-part-project"></a><span data-ttu-id="ed723-102">Criar um projeto de Web Part</span><span class="sxs-lookup"><span data-stu-id="ed723-102">Create a web part project</span></span>

1. <span data-ttu-id="ed723-103">Abra a interface de linha de comando (CLI) em um diretório vazio onde você deseja criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="ed723-103">Open your command-line interface (CLI) in an empty directory where you want to create the project.</span></span> <span data-ttu-id="ed723-104">Execute o seguinte comando para iniciar o gerador do SharePoint do Yeoman.</span><span class="sxs-lookup"><span data-stu-id="ed723-104">Run the following command to start the Yeoman SharePoint generator.</span></span>

    ```Shell
    yo @microsoft/sharepoint
    ```

1. <span data-ttu-id="ed723-105">Responda aos prompts da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="ed723-105">Respond to the prompts as follows.</span></span>

    - <span data-ttu-id="ed723-106">**Qual é o nome da sua solução?**</span><span class="sxs-lookup"><span data-stu-id="ed723-106">**What is your solution name?**</span></span> `graph-tutorial`
    - <span data-ttu-id="ed723-107">**Quais pacotes de linha de base deseja direcionar para seus componentes?**</span><span class="sxs-lookup"><span data-stu-id="ed723-107">**Which baseline packages do you want to target for your component(s)?**</span></span> `SharePoint Online only (latest)`
    - <span data-ttu-id="ed723-108">**Onde você deseja colocar os arquivos?**</span><span class="sxs-lookup"><span data-stu-id="ed723-108">**Where do you want to place the files?**</span></span> `Use the current folder`
    - <span data-ttu-id="ed723-109">**Você deseja permitir que o administrador de locatários possa implantar a solução para todos os sites imediatamente sem executar qualquer implantação de recurso ou adicionar aplicativos aos sites?**</span><span class="sxs-lookup"><span data-stu-id="ed723-109">**Do you want to allow the tenant admin the choice of being able to deploy the solution to all sites immediately without running any feature deployment or adding apps in sites?**</span></span> `Yes`
    - <span data-ttu-id="ed723-110">**Os componentes da solução exigem permissões para acessar as APIs da Web que são exclusivas e não são compartilhadas com outros componentes nos dez Ants?**</span><span class="sxs-lookup"><span data-stu-id="ed723-110">**Will the components in the solution require permissions to access web APIs that are unique and not shared with other components in the ten ant?**</span></span> `No`
    - <span data-ttu-id="ed723-111">**Que tipo de componente do cliente a ser criado?**</span><span class="sxs-lookup"><span data-stu-id="ed723-111">**Which type of client-side component to create?**</span></span> `WebPart`
    - <span data-ttu-id="ed723-112">**Qual é o nome da Web Part?**</span><span class="sxs-lookup"><span data-stu-id="ed723-112">**What is your Web part name?**</span></span> `GraphTutorial`
    - <span data-ttu-id="ed723-113">**Qual é a descrição da Web Part?**</span><span class="sxs-lookup"><span data-stu-id="ed723-113">**What is your Web part description?**</span></span> `GraphTutorial description`
    - <span data-ttu-id="ed723-114">**Qual estrutura você deseja usar?**</span><span class="sxs-lookup"><span data-stu-id="ed723-114">**Which framework would you like to use?**</span></span> `No JavaScript framework`

1. <span data-ttu-id="ed723-115">Execute o seguinte comando para atualizar a versão TypeScript no projeto para 3,7.</span><span class="sxs-lookup"><span data-stu-id="ed723-115">Run the following command to update the TypeScript version in the project to 3.7.</span></span>

    ```Shell
    npm install @microsoft/rush-stack-compiler-3.7 --save-dev
    ```

1. <span data-ttu-id="ed723-116">Abrir **./tsconfig.js** e substituir `rush-stack-compiler-3.3` por `rush-stack-compiler-3.7` .</span><span class="sxs-lookup"><span data-stu-id="ed723-116">Open **./tsconfig.json** and replace `rush-stack-compiler-3.3` with `rush-stack-compiler-3.7`.</span></span>

1. <span data-ttu-id="ed723-117">Abra **./tslint.js** e remova a `"no-use-before-declare": true,` linha.</span><span class="sxs-lookup"><span data-stu-id="ed723-117">Open **./tslint.json** and remove the `"no-use-before-declare": true,` line.</span></span> <span data-ttu-id="ed723-118">A `no-use-before-declare` regra foi preterida e causará um erro durante o processo de compilação.</span><span class="sxs-lookup"><span data-stu-id="ed723-118">The `no-use-before-declare` rule is deprecated and will cause an error during the build process.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="ed723-119">Instalar dependências</span><span class="sxs-lookup"><span data-stu-id="ed723-119">Install dependencies</span></span>

<span data-ttu-id="ed723-120">Antes de prosseguir, instale alguns pacotes adicionais do NPM que serão usados posteriormente.</span><span class="sxs-lookup"><span data-stu-id="ed723-120">Before moving on, install some additional NPM packages that you will use later.</span></span>

- <span data-ttu-id="ed723-121">[Definições do TypeScript do Microsoft Graph](https://github.com/microsoftgraph/msgraph-typescript-typings) para fornecer IntelliSense para objetos do Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="ed723-121">[Microsoft Graph TypeScript definitions](https://github.com/microsoftgraph/msgraph-typescript-typings) to provide Intellisense for Microsoft Graph objects.</span></span>
- <span data-ttu-id="ed723-122">[Kit de ferramentas do Microsoft Graph](https://docs.microsoft.com/graph/toolkit/overview) para fornecer componentes da interface do usuário para a Web Part.</span><span class="sxs-lookup"><span data-stu-id="ed723-122">[Microsoft Graph Toolkit](https://docs.microsoft.com/graph/toolkit/overview) to provide UI components for the web part.</span></span>
- <span data-ttu-id="ed723-123">[Data-FNS](https://date-fns.org/) para funções úteis para trabalhar com datas.</span><span class="sxs-lookup"><span data-stu-id="ed723-123">[date-fns](https://date-fns.org/) for helpful functions for working with dates.</span></span>

```Shell
npm install @microsoft/microsoft-graph-types@1.17.0 --save-dev
npm install @microsoft/mgt@1.3.4 date-fns @2.16.0
```
