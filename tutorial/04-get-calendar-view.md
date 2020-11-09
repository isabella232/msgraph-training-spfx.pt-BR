---
ms.openlocfilehash: c239c1ea6daae99cc5a65b7b95508ccb2a1bf014
ms.sourcegitcommit: 523e64e972f247e2df57cd8de949ac7bd1b8b047
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "48823276"
---
<!-- markdownlint-disable MD002 MD041 -->

A estrutura do SharePoint fornece o [MSGraphClient](https://docs.microsoft.com/javascript/api/sp-http/msgraphclient?view=sp-typescript-latest) para fazer chamadas para o Microsoft Graph. Essa classe envolve a [biblioteca de cliente JavaScript do Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-javascript), Autenticando-a com o usuário conectado no momento.

Como ela quebra a biblioteca JavaScript existente, seu uso é o mesmo e é totalmente compatível com as definições do TypeScript do Microsoft Graph.

## <a name="get-the-users-calendar"></a>Obter o calendário do usuário

1. Abra **./src/WebParts/graphTutorial/GraphTutorialWebPart.TS** e adicione as seguintes `import` instruções na parte superior do arquivo.

    ```typescript
    import { MSGraphClient } from '@microsoft/sp-http';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    import { startOfWeek, endOfWeek, setDay, set } from 'date-fns';
    ```

1. Adicione a função a seguir à classe **GraphTutorialWebPart** para renderizar um erro.

    :::code language="typescript" source="../demo/graph-tutorial/src/webparts/graphTutorial/GraphTutorialWebPart.ts" id="renderGraphErrorSnippet":::

1. Adicione a função a seguir para imprimir os eventos no calendário do usuário.

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

1. Substitua a função `render` existente pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/webparts/graphTutorial/GraphTutorialWebPart.ts" id="renderSnippet":::

    Observe o que esse código faz.

    - Ele usa `this.context.msGraphClientFactory.getClient` para obter um objeto **MSGraphClient** autenticado.
    - Ele chama o `/me/calendarView` ponto de extremidade, definindo os `startDateTime` `endDateTime` parâmetros de consulta e para o início e o fim da semana atual.
    - Ele usa `select` para limitar quais campos são retornados, solicitando somente os campos que o aplicativo usa.
    - Ele usa `orderby` para classificar os eventos pelo horário de início.
    - Ele usa `top` para limitar os resultados a 25 eventos.

## <a name="deploy-the-web-part"></a>Implantar a Web Part

1. Execute os dois comandos a seguir em sua CLI para criar e empacotar a Web Part.

    ```Shell
    gulp bundle --ship
    gulp package-solution --ship
    ```

1. Abra o navegador e vá para o catálogo de aplicativos do SharePoint do locatário. Selecione o item **de menu aplicativos para SharePoint** no lado esquerdo.

1. Carregar o arquivo **./SharePoint/Solution/Graph-tutorial.sppkg** .

1. No prompt **confiar...** , confirme se o prompt lista as 4 permissões do Microsoft Graph definidas na **package-solution.jsem** arquivo. Selecione **tornar esta solução disponível para todos os sites na organização** e selecione **implantar**.

1. Se você ainda não aprovou as permissões de gráfico para a Web Part, faça isso agora.

    1. Vá para o [centro de administração do SharePoint](https://admin.microsoft.com/sharepoint?page=classicfeatures&modern=true) usando um administrador de locatários.

    1. No menu à esquerda, selecione **avançado** e, em seguida, **acesso à API**.

    1. Selecione cada uma das solicitações pendentes do pacote **gráfico-tutorial-cliente-solução** e escolha **aprovar**.

        ![Uma captura de tela da página de acesso à API do centro de administração do SharePoint](images/api-access.png)

## <a name="test-the-web-part"></a>Testar a Web Part

1. Vá para um site do SharePoint onde você deseja testar a Web Part. Crie uma nova página para testar a Web Part.

1. Use o seletor de Web Part para localizar a Web Part **GraphTutorial** e adicioná-la à página.

    ![Uma captura de tela da Web Part GraphTutorial no seletor de Web Part](images/add-web-part.png)

1. Uma lista de eventos da semana atual é impressa na Web Part.

    ![Uma captura de tela da Web Part exibindo uma lista de eventos](images/calendar-list.png)
