---
ms.openlocfilehash: bb55b2bea2497628ceb2e09c889ce62e773d8e71
ms.sourcegitcommit: 523e64e972f247e2df57cd8de949ac7bd1b8b047
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "48823268"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a32c9-101">Este tutorial ensina como criar uma [Web Part do lado do cliente do SharePoint](https://docs.microsoft.com/sharepoint/dev/spfx/web-parts/overview-client-side-web-parts) que usa a API do Microsoft Graph para recuperar informações de calendário de um usuário.</span><span class="sxs-lookup"><span data-stu-id="a32c9-101">This tutorial teaches you how to build a [SharePoint client-side web part](https://docs.microsoft.com/sharepoint/dev/spfx/web-parts/overview-client-side-web-parts) that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="a32c9-102">Se preferir baixar o tutorial concluído, você poderá baixar ou clonar o repositório do [GitHub](https://github.com/microsoftgraph/msgraph-training-spfx).</span><span class="sxs-lookup"><span data-stu-id="a32c9-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-spfx).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a32c9-103">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="a32c9-103">Prerequisites</span></span>

<span data-ttu-id="a32c9-104">Antes de iniciar este tutorial, você deve ter as seguintes ferramentas instaladas em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="a32c9-104">Before you start this tutorial, you should have the following tools installed on your development machine.</span></span>

- [<span data-ttu-id="a32c9-105">Node.js</span><span class="sxs-lookup"><span data-stu-id="a32c9-105">Node.js</span></span>](https://nodejs.org/en/download/releases/)
- [<span data-ttu-id="a32c9-106">Yeoman</span><span class="sxs-lookup"><span data-stu-id="a32c9-106">Yeoman</span></span>](https://yeoman.io/)
- [<span data-ttu-id="a32c9-107">Gulp</span><span class="sxs-lookup"><span data-stu-id="a32c9-107">Gulp</span></span>](https://gulpjs.com/)
- [<span data-ttu-id="a32c9-108">Gerador Yeoman do SharePoint</span><span class="sxs-lookup"><span data-stu-id="a32c9-108">Yeoman SharePoint generator</span></span>](https://docs.microsoft.com/sharepoint/dev/spfx/toolchain/scaffolding-projects-using-yeoman-sharepoint-generator)

<span data-ttu-id="a32c9-109">Você pode encontrar mais detalhes sobre os requisitos para o desenvolvimento da estrutura do SharePoint em [configurar seu ambiente de desenvolvimento do SharePoint Framework](https://docs.microsoft.com/sharepoint/dev/spfx/set-up-your-development-environment).</span><span class="sxs-lookup"><span data-stu-id="a32c9-109">You can find more details about requirements for SharePoint Framework development at [Set up your SharePoint Framework development environment](https://docs.microsoft.com/sharepoint/dev/spfx/set-up-your-development-environment).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a32c9-110">A estrutura do SharePoint requer o Node.js versão 10. x.</span><span class="sxs-lookup"><span data-stu-id="a32c9-110">SharePoint Framework requires Node.js version 10.x.</span></span> <span data-ttu-id="a32c9-111">Certifique-se de instalar a versão correta.</span><span class="sxs-lookup"><span data-stu-id="a32c9-111">Be sure to install the correct version.</span></span>

<span data-ttu-id="a32c9-112">Você também deve ter uma conta corporativa ou de estudante da Microsoft, com acesso a uma conta de administrador global na mesma organização.</span><span class="sxs-lookup"><span data-stu-id="a32c9-112">You should also have a Microsoft work or school account, with access to a global administrator account in the same organization.</span></span> <span data-ttu-id="a32c9-113">Se você não tiver uma conta da Microsoft, poderá [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="a32c9-113">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

<span data-ttu-id="a32c9-114">O Microsoft 365 locatário deve ser [configurado para o desenvolvimento da estrutura do SharePoint](https://docs.microsoft.com/sharepoint/dev/spfx/set-up-your-developer-tenant), com um catálogo de aplicativos e um site de teste criados antes de iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="a32c9-114">Your Microsoft 365 tenant should be [setup for SharePoint Framework development](https://docs.microsoft.com/sharepoint/dev/spfx/set-up-your-developer-tenant), with an app catalog and testing site created before you start this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="a32c9-115">Este tutorial foi escrito com as seguintes versões das ferramentas acima.</span><span class="sxs-lookup"><span data-stu-id="a32c9-115">This tutorial was written with the following versions of the above tools.</span></span> <span data-ttu-id="a32c9-116">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="a32c9-116">The steps in this guide may work with other versions, but that has not been tested.</span></span>
>
> - <span data-ttu-id="a32c9-117">Node.js 10.22.0</span><span class="sxs-lookup"><span data-stu-id="a32c9-117">Node.js 10.22.0</span></span>
> - <span data-ttu-id="a32c9-118">Yeoman 3.1.1</span><span class="sxs-lookup"><span data-stu-id="a32c9-118">Yeoman 3.1.1</span></span>
> - <span data-ttu-id="a32c9-119">Gulp 4.0.2</span><span class="sxs-lookup"><span data-stu-id="a32c9-119">Gulp 4.0.2</span></span>
> - <span data-ttu-id="a32c9-120">Yeoman do gerador do SharePoint 1.11.0</span><span class="sxs-lookup"><span data-stu-id="a32c9-120">Yeoman SharePoint generator 1.11.0</span></span>

## <a name="feedback"></a><span data-ttu-id="a32c9-121">Comentários</span><span class="sxs-lookup"><span data-stu-id="a32c9-121">Feedback</span></span>

<span data-ttu-id="a32c9-122">Forneça comentários sobre este tutorial no [repositório do GitHub](https://github.com/microsoftgraph/msgraph-training-spfx).</span><span class="sxs-lookup"><span data-stu-id="a32c9-122">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-spfx).</span></span>
