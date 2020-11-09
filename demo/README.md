---
ms.openlocfilehash: ca72a0d6047d3cd275d34e2d0b4e3c54dbab5375
ms.sourcegitcommit: 523e64e972f247e2df57cd8de949ac7bd1b8b047
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "48831259"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="a3d13-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="a3d13-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a3d13-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="a3d13-102">Prerequisites</span></span>

<span data-ttu-id="a3d13-103">Para executar o projeto concluído nessa pasta, você precisará do seguinte:</span><span class="sxs-lookup"><span data-stu-id="a3d13-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="a3d13-104">[Node.js](https://nodejs.org/en/download/releases/) versão 10. x</span><span class="sxs-lookup"><span data-stu-id="a3d13-104">[Node.js](https://nodejs.org/en/download/releases/) version 10.x</span></span>
- [<span data-ttu-id="a3d13-105">Gulp</span><span class="sxs-lookup"><span data-stu-id="a3d13-105">Gulp</span></span>](https://gulpjs.com/)
- <span data-ttu-id="a3d13-106">Uma conta corporativa ou de estudante da Microsoft, com acesso a uma conta de administrador global na mesma organização.</span><span class="sxs-lookup"><span data-stu-id="a3d13-106">A Microsoft work or school account, with access to a global administrator account in the same organization.</span></span> <span data-ttu-id="a3d13-107">Se você não tiver uma conta da Microsoft, poderá [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="a3d13-107">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>
- <span data-ttu-id="a3d13-108">O Microsoft 365 locatário deve ser [configurado para o desenvolvimento da estrutura do SharePoint](https://docs.microsoft.com/sharepoint/dev/spfx/set-up-your-developer-tenant), com um catálogo de aplicativos e um site de teste criados antes de iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="a3d13-108">Your Microsoft 365 tenant should be [setup for SharePoint Framework development](https://docs.microsoft.com/sharepoint/dev/spfx/set-up-your-developer-tenant), with an app catalog and testing site created before you start this tutorial.</span></span>

### <a name="deploy-the-web-part"></a><span data-ttu-id="a3d13-109">Implantar a Web Part</span><span class="sxs-lookup"><span data-stu-id="a3d13-109">Deploy the web part</span></span>

1. <span data-ttu-id="a3d13-110">Execute os dois comandos a seguir em sua CLI para criar e empacotar a Web Part.</span><span class="sxs-lookup"><span data-stu-id="a3d13-110">Run the following two commands in your CLI to build and package your web part.</span></span>

    ```Shell
    gulp bundle --ship
    gulp package-solution --ship
    ```

1. <span data-ttu-id="a3d13-111">Abra o navegador e vá para o catálogo de aplicativos do SharePoint do locatário.</span><span class="sxs-lookup"><span data-stu-id="a3d13-111">Open your browser and go to your tenant's SharePoint App Catalog.</span></span> <span data-ttu-id="a3d13-112">Selecione o item **de menu aplicativos para SharePoint** no lado esquerdo.</span><span class="sxs-lookup"><span data-stu-id="a3d13-112">Select the **Apps for SharePoint** menu item on the left-hand side.</span></span>

1. <span data-ttu-id="a3d13-113">Carregar o arquivo **./SharePoint/Solution/Graph-tutorial.sppkg** .</span><span class="sxs-lookup"><span data-stu-id="a3d13-113">Upload the **./sharepoint/solution/graph-tutorial.sppkg** file.</span></span>

1. <span data-ttu-id="a3d13-114">No prompt **confiar...** , confirme se o prompt lista as 4 permissões do Microsoft Graph definidas na **package-solution.jsem** arquivo.</span><span class="sxs-lookup"><span data-stu-id="a3d13-114">In the **Do you trust...** prompt, confirm that the prompt lists the 4 Microsoft Graph permissions you set in the **package-solution.json** file.</span></span> <span data-ttu-id="a3d13-115">Selecione **tornar esta solução disponível para todos os sites na organização** e selecione **implantar**.</span><span class="sxs-lookup"><span data-stu-id="a3d13-115">Select **Make this solution available to all sites in the organization** , then select **Deploy**.</span></span>

1. <span data-ttu-id="a3d13-116">Vá para o [centro de administração do SharePoint](https://admin.microsoft.com/sharepoint?page=classicfeatures&modern=true) usando um administrador de locatários.</span><span class="sxs-lookup"><span data-stu-id="a3d13-116">Go to the [SharePoint admin center](https://admin.microsoft.com/sharepoint?page=classicfeatures&modern=true) using a tenant administrator.</span></span>

1. <span data-ttu-id="a3d13-117">No menu à esquerda, selecione **avançado** e, em seguida, **acesso à API**.</span><span class="sxs-lookup"><span data-stu-id="a3d13-117">In the left-hand menu, select **Advanced** , then **API access**.</span></span>

1. <span data-ttu-id="a3d13-118">Selecione cada uma das solicitações pendentes do pacote **gráfico-tutorial-cliente-solução** e escolha **aprovar**.</span><span class="sxs-lookup"><span data-stu-id="a3d13-118">Select each of the pending requests from the **graph-tutorial-client-side-solution** package and choose **Approve**.</span></span>

    ![Uma captura de tela da página de acesso à API do centro de administração do SharePoint](../tutorial/images/api-access.png)

### <a name="test-the-web-part"></a><span data-ttu-id="a3d13-120">Testar a Web Part</span><span class="sxs-lookup"><span data-stu-id="a3d13-120">Test the web part</span></span>

1. <span data-ttu-id="a3d13-121">Vá para um site do SharePoint onde você deseja testar a Web Part.</span><span class="sxs-lookup"><span data-stu-id="a3d13-121">Go to a SharePoint site where you want to test the web part.</span></span> <span data-ttu-id="a3d13-122">Crie uma nova página para testar a Web Part.</span><span class="sxs-lookup"><span data-stu-id="a3d13-122">Create a new page to test the web part on.</span></span>

1. <span data-ttu-id="a3d13-123">Use o seletor de Web Part para localizar a Web Part **GraphTutorial** e adicioná-la à página.</span><span class="sxs-lookup"><span data-stu-id="a3d13-123">Use the web part picker to find the **GraphTutorial** web part and add it to the page.</span></span>

    ![Uma captura de tela da Web Part GraphTutorial no seletor de Web Part](../tutorial/images/add-web-part.png)
