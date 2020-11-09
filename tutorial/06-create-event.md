---
ms.openlocfilehash: 0d7c9cc909e2f848cc9760d5862bb1863501f02b
ms.sourcegitcommit: 523e64e972f247e2df57cd8de949ac7bd1b8b047
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "48823258"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="8fd52-101">Nesta seção, você atualizará a Web Part para permitir que o usuário adicione um evento ao seu calendário para a hora social semanal da equipe.</span><span class="sxs-lookup"><span data-stu-id="8fd52-101">In this section you'll update the web part to allow the user to add an event to their calendar for the team's weekly social hour.</span></span> <span data-ttu-id="8fd52-102">Neste cenário, a equipe tem um horário social semanal em 4 PM na sexta-feira.</span><span class="sxs-lookup"><span data-stu-id="8fd52-102">In this scenario, the team has a weekly social hour at 4 PM on Friday.</span></span>

1. <span data-ttu-id="8fd52-103">Abra **./src/WebParts/graphTutorial/GraphTutorialWebPart.TS** e substitua o `addSocialToCalendar()` método existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="8fd52-103">Open **./src/webparts/graphTutorial/GraphTutorialWebPart.ts** and replace the existing `addSocialToCalendar()` method with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/webparts/graphTutorial/GraphTutorialWebPart.ts" id="addSocialToCalendarSnippet":::

    <span data-ttu-id="8fd52-104">Considere o que esse código faz.</span><span class="sxs-lookup"><span data-stu-id="8fd52-104">Consider what this code does.</span></span>

    - <span data-ttu-id="8fd52-105">Ele determina a próxima sexta-feira seguinte e cria uma **Data** para 4 p.m. nesse dia.</span><span class="sxs-lookup"><span data-stu-id="8fd52-105">It determines the next upcoming Friday and constructs a **Date** for 4 PM on that day.</span></span>
    - <span data-ttu-id="8fd52-106">Ele cria um novo objeto **MicrosoftGraph. Event** , definindo o início para o valor da **Data** e o final de uma hora mais tarde.</span><span class="sxs-lookup"><span data-stu-id="8fd52-106">It constructs a new **MicrosoftGraph.Event** object, setting the start to the value of the **Date** , and the end for one hour later.</span></span>
    - <span data-ttu-id="8fd52-107">Ele usa o **MSGraphClient** para postar o novo evento no `/me/events` ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="8fd52-107">It uses the **MSGraphClient** to POST the new event to the `/me/events` endpoint.</span></span>
    - <span data-ttu-id="8fd52-108">Ele renderiza novamente a Web Part para que o modo de exibição seja atualizado com o novo evento.</span><span class="sxs-lookup"><span data-stu-id="8fd52-108">It re-renders the web part so the view is updated with the new event.</span></span>

1. <span data-ttu-id="8fd52-109">Criar, empacotar e recarregar a Web Part e, em seguida, atualizar a página em que você está testando.</span><span class="sxs-lookup"><span data-stu-id="8fd52-109">Build, package, and re-upload the web part, then refresh the page where you are testing it.</span></span>

1. <span data-ttu-id="8fd52-110">Clique no botão **Adicionar equipe social** .</span><span class="sxs-lookup"><span data-stu-id="8fd52-110">Click the **Add team social** button.</span></span> <span data-ttu-id="8fd52-111">Depois que a página for atualizada, role para baixo até sexta-feira e localize o novo evento.</span><span class="sxs-lookup"><span data-stu-id="8fd52-111">Once the page refreshes, scroll down to Friday and find the new event.</span></span>

    ![Uma captura de tela do evento recém-criado renderizado na Web Part](images/new-event.png)
