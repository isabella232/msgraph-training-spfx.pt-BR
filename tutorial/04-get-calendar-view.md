---
ms.openlocfilehash: c239c1ea6daae99cc5a65b7b95508ccb2a1bf014
ms.sourcegitcommit: 523e64e972f247e2df57cd8de949ac7bd1b8b047
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "48823276"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="bd49b-101">A estrutura do SharePoint fornece o [MSGraphClient](https://docs.microsoft.com/javascript/api/sp-http/msgraphclient?view=sp-typescript-latest) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="bd49b-101">The SharePoint Framework provides the [MSGraphClient](https://docs.microsoft.com/javascript/api/sp-http/msgraphclient?view=sp-typescript-latest) for making calls to Microsoft Graph.</span></span> <span data-ttu-id="bd49b-102">Essa classe envolve a [biblioteca de cliente JavaScript do Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-javascript), Autenticando-a com o usuário conectado no momento.</span><span class="sxs-lookup"><span data-stu-id="bd49b-102">This class wraps the [Microsoft Graph JavaScript Client Library](https://github.com/microsoftgraph/msgraph-sdk-javascript), pre-authenticating it with the current logged on user.</span></span>

<span data-ttu-id="bd49b-103">Como ela quebra a biblioteca JavaScript existente, seu uso é o mesmo e é totalmente compatível com as definições do TypeScript do Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="bd49b-103">Because it wraps the existing JavaScript library, its usage is the same, and it's fully compatible with the Microsoft Graph TypeScript definitions.</span></span>

## <a name="get-the-users-calendar"></a><span data-ttu-id="bd49b-104">Obter o calendário do usuário</span><span class="sxs-lookup"><span data-stu-id="bd49b-104">Get the user's calendar</span></span>

1. <span data-ttu-id="bd49b-105">Abra **./src/WebParts/graphTutorial/GraphTutorialWebPart.TS** e adicione as seguintes `import` instruções na parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="bd49b-105">Open **./src/webparts/graphTutorial/GraphTutorialWebPart.ts** and add the following `import` statements at the top of the file.</span></span>

    ```typescript
    import { MSGraphClient } from '@microsoft/sp-http';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    import { startOfWeek, endOfWeek, setDay, set } from 'date-fns';
    ```

1. <span data-ttu-id="bd49b-106">Adicione a função a seguir à classe **GraphTutorialWebPart** para renderizar um erro.</span><span class="sxs-lookup"><span data-stu-id="bd49b-106">Add the following function to the **GraphTutorialWebPart** class to render an error.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/webparts/graphTutorial/GraphTutorialWebPart.ts" id="renderGraphErrorSnippet":::

1. <span data-ttu-id="bd49b-107">Adicione a função a seguir para imprimir os eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="bd49b-107">Add the following function to print out the events in the user's calendar.</span></span>

    ```typescript
    private renderCalendarView(events: MicrosoftGraph.Event[]) : void {
      const viewContainer = this.domElement.querySelector('#calendarView');
      let html = '';

      // Temporary: print events as a list
      for(const event of events) {
        html += `
          <p class="${ styles.description }">Subject: ${event.subject}</p>
          <p class="${ styles.description }">Organizer: ${event.organizer.emailAddress.name}</p>
          <p class="${ styles.description }">Start: ${event.start.dateTime}</p>
          <p class="${ styles.description }">End: ${event.end.dateTime}</p>
          `;
      }

      viewContainer.innerHTML = html;
    }
    ```

1. <span data-ttu-id="bd49b-108">Substitua a função `render` existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="bd49b-108">Replace the existing `render` function with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/webparts/graphTutorial/GraphTutorialWebPart.ts" id="renderSnippet":::

    <span data-ttu-id="bd49b-109">Observe o que esse código faz.</span><span class="sxs-lookup"><span data-stu-id="bd49b-109">Notice what this code does.</span></span>

    - <span data-ttu-id="bd49b-110">Ele usa `this.context.msGraphClientFactory.getClient` para obter um objeto **MSGraphClient** autenticado.</span><span class="sxs-lookup"><span data-stu-id="bd49b-110">It uses `this.context.msGraphClientFactory.getClient` to get an authenticated **MSGraphClient** object.</span></span>
    - <span data-ttu-id="bd49b-111">Ele chama o `/me/calendarView` ponto de extremidade, definindo os `startDateTime` `endDateTime` parâmetros de consulta e para o início e o fim da semana atual.</span><span class="sxs-lookup"><span data-stu-id="bd49b-111">It calls the `/me/calendarView` endpoint, setting the `startDateTime` and `endDateTime` query parameters to the start and end of the current week.</span></span>
    - <span data-ttu-id="bd49b-112">Ele usa `select` para limitar quais campos são retornados, solicitando somente os campos que o aplicativo usa.</span><span class="sxs-lookup"><span data-stu-id="bd49b-112">It uses `select` to limit which fields are returned, requesting only the fields the app uses.</span></span>
    - <span data-ttu-id="bd49b-113">Ele usa `orderby` para classificar os eventos pelo horário de início.</span><span class="sxs-lookup"><span data-stu-id="bd49b-113">It uses `orderby` to sort the events by their start time.</span></span>
    - <span data-ttu-id="bd49b-114">Ele usa `top` para limitar os resultados a 25 eventos.</span><span class="sxs-lookup"><span data-stu-id="bd49b-114">It uses `top` to limit the results to 25 events.</span></span>

## <a name="deploy-the-web-part"></a><span data-ttu-id="bd49b-115">Implantar a Web Part</span><span class="sxs-lookup"><span data-stu-id="bd49b-115">Deploy the web part</span></span>

1. <span data-ttu-id="bd49b-116">Execute os dois comandos a seguir em sua CLI para criar e empacotar a Web Part.</span><span class="sxs-lookup"><span data-stu-id="bd49b-116">Run the following two commands in your CLI to build and package your web part.</span></span>

    ```Shell
    gulp bundle --ship
    gulp package-solution --ship
    ```

1. <span data-ttu-id="bd49b-117">Abra o navegador e vá para o catálogo de aplicativos do SharePoint do locatário.</span><span class="sxs-lookup"><span data-stu-id="bd49b-117">Open your browser and go to your tenant's SharePoint App Catalog.</span></span> <span data-ttu-id="bd49b-118">Selecione o item **de menu aplicativos para SharePoint** no lado esquerdo.</span><span class="sxs-lookup"><span data-stu-id="bd49b-118">Select the **Apps for SharePoint** menu item on the left-hand side.</span></span>

1. <span data-ttu-id="bd49b-119">Carregar o arquivo **./SharePoint/Solution/Graph-tutorial.sppkg** .</span><span class="sxs-lookup"><span data-stu-id="bd49b-119">Upload the **./sharepoint/solution/graph-tutorial.sppkg** file.</span></span>

1. <span data-ttu-id="bd49b-120">No prompt **confiar...** , confirme se o prompt lista as 4 permissões do Microsoft Graph definidas na **package-solution.jsem** arquivo.</span><span class="sxs-lookup"><span data-stu-id="bd49b-120">In the **Do you trust...** prompt, confirm that the prompt lists the 4 Microsoft Graph permissions you set in the **package-solution.json** file.</span></span> <span data-ttu-id="bd49b-121">Selecione **tornar esta solução disponível para todos os sites na organização** e selecione **implantar**.</span><span class="sxs-lookup"><span data-stu-id="bd49b-121">Select **Make this solution available to all sites in the organization** , then select **Deploy**.</span></span>

1. <span data-ttu-id="bd49b-122">Se você ainda não aprovou as permissões de gráfico para a Web Part, faça isso agora.</span><span class="sxs-lookup"><span data-stu-id="bd49b-122">If you have not already approved the Graph permissions for your web part, do that now.</span></span>

    1. <span data-ttu-id="bd49b-123">Vá para o [centro de administração do SharePoint](https://admin.microsoft.com/sharepoint?page=classicfeatures&modern=true) usando um administrador de locatários.</span><span class="sxs-lookup"><span data-stu-id="bd49b-123">Go to the [SharePoint admin center](https://admin.microsoft.com/sharepoint?page=classicfeatures&modern=true) using a tenant administrator.</span></span>

    1. <span data-ttu-id="bd49b-124">No menu à esquerda, selecione **avançado** e, em seguida, **acesso à API**.</span><span class="sxs-lookup"><span data-stu-id="bd49b-124">In the left-hand menu, select **Advanced** , then **API access**.</span></span>

    1. <span data-ttu-id="bd49b-125">Selecione cada uma das solicitações pendentes do pacote **gráfico-tutorial-cliente-solução** e escolha **aprovar**.</span><span class="sxs-lookup"><span data-stu-id="bd49b-125">Select each of the pending requests from the **graph-tutorial-client-side-solution** package and choose **Approve**.</span></span>

        ![Uma captura de tela da página de acesso à API do centro de administração do SharePoint](images/api-access.png)

## <a name="test-the-web-part"></a><span data-ttu-id="bd49b-127">Testar a Web Part</span><span class="sxs-lookup"><span data-stu-id="bd49b-127">Test the web part</span></span>

1. <span data-ttu-id="bd49b-128">Vá para um site do SharePoint onde você deseja testar a Web Part.</span><span class="sxs-lookup"><span data-stu-id="bd49b-128">Go to a SharePoint site where you want to test the web part.</span></span> <span data-ttu-id="bd49b-129">Crie uma nova página para testar a Web Part.</span><span class="sxs-lookup"><span data-stu-id="bd49b-129">Create a new page to test the web part on.</span></span>

1. <span data-ttu-id="bd49b-130">Use o seletor de Web Part para localizar a Web Part **GraphTutorial** e adicioná-la à página.</span><span class="sxs-lookup"><span data-stu-id="bd49b-130">Use the web part picker to find the **GraphTutorial** web part and add it to the page.</span></span>

    ![Uma captura de tela da Web Part GraphTutorial no seletor de Web Part](images/add-web-part.png)

1. <span data-ttu-id="bd49b-132">Uma lista de eventos da semana atual é impressa na Web Part.</span><span class="sxs-lookup"><span data-stu-id="bd49b-132">A list of events for the current week are printed in the web part.</span></span>

    ![Uma captura de tela da Web Part exibindo uma lista de eventos](images/calendar-list.png)
