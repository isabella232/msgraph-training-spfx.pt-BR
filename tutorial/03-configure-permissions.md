---
ms.openlocfilehash: 19bd57df9f349d67e9f10e7dd70f02231950b010
ms.sourcegitcommit: 523e64e972f247e2df57cd8de949ac7bd1b8b047
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "48823280"
---
<!-- markdownlint-disable MD002 MD041 -->

A estrutura do SharePoint elimina a necessidade de registrar um aplicativo no Azure AD para obter tokens de acesso para acessar o Microsoft Graph. Ele trata da autenticação do usuário que está conectado ao SharePoint, permitindo que a Web Part obtenha tokens de usuário. A Web Part precisa indicar quais [escopos de permissão de gráfico](https://docs.microsoft.com/graph/permissions-reference) são necessários e um administrador de locatários pode aprovar essas permissões durante a instalação.

## <a name="configure-permissions"></a>Configurar permissões

1. Abrir **./config/package-solution.jsem**.

1. Adicione o seguinte código à `solution` propriedade.

    ```json
    "webApiPermissionRequests": [
      {
        "resource": "Microsoft Graph",
        "scope": "Calendars.ReadWrite"
      },
      {
        "resource": "Microsoft Graph",
        "scope": "User.ReadBasic.All"
      },
      {
        "resource": "Microsoft Graph",
        "scope": "Contacts.Read"
      },
      {
        "resource": "Microsoft Graph",
        "scope": "People.Read"
      }
    ]
    ```

A `Calendars.ReadWrite` permissão permite que a Web Part recupere o calendário do usuário e adicione eventos usando o Microsoft Graph. As outras permissões são usadas por componentes no Microsoft Graph Toolkit para renderizar informações sobre participantes e organizadores de eventos.

## <a name="optional-test-token-acquisition"></a>Opcional: aquisição de token de teste

> [!NOTE]
> O restante das etapas nesta página são opcionais. Se preferir acessar a codificação do Microsoft Graph imediatamente, você pode prosseguir para [obter um modo de exibição de calendário](/graph/tutorials/spfx?tutorial-step=3).

Vamos adicionar um código temporário à Web Part para testar a aquisição de tokens.

1. Abra **./src/WebParts/graphTutorial/GraphTutorialWebPart.TS** e adicione a instrução a seguir `import` na parte superior do arquivo.

    ```typescript
    import { AadTokenProvider } from '@microsoft/sp-http';
    ```

1. Substitua a função `render` existente pelo seguinte.

    ```typescript
    public render(): void {
    this.context.aadTokenProviderFactory
      .getTokenProvider()
      .then((provider: AadTokenProvider)=> {
      provider
        .getToken('https://graph.microsoft.com')
        .then((token: string) => {
          this.domElement.innerHTML = `
          <div class="${ styles.graphTutorial }">
            <div class="${ styles.container }">
              <div class="${ styles.row }">
                <div class="${ styles.column }">
                  <span class="${ styles.title }">Welcome to SharePoint!</span>
                  <p><code style="word-break: break-all;">${ token }</code></p>
                </div>
              </div>
            </div>
          </div>`;
        });
      });
    }
    ```

### <a name="deploy-the-web-part"></a>Implantar a Web Part

1. Execute os dois comandos a seguir em sua CLI para criar e empacotar a Web Part.

    ```Shell
    gulp bundle --ship
    gulp package-solution --ship
    ```

1. Abra o navegador e vá para o catálogo de aplicativos do SharePoint do locatário. Selecione o item **de menu aplicativos para SharePoint** no lado esquerdo.

1. Carregar o arquivo **./SharePoint/Solution/Graph-tutorial.sppkg** .

1. No prompt **confiar...** , confirme se o prompt lista as 4 permissões do Microsoft Graph definidas na **package-solution.jsem** arquivo. Selecione **tornar esta solução disponível para todos os sites na organização** e selecione **implantar**.

1. Vá para o [centro de administração do SharePoint](https://admin.microsoft.com/sharepoint?page=classicfeatures&modern=true) usando um administrador de locatários.

1. No menu à esquerda, selecione **avançado** e, em seguida, **acesso à API**.

1. Selecione cada uma das solicitações pendentes do pacote **gráfico-tutorial-cliente-solução** e escolha **aprovar**.

    ![Uma captura de tela da página de acesso à API do centro de administração do SharePoint](images/api-access.png)

### <a name="test-the-web-part"></a>Testar a Web Part

1. Vá para um site do SharePoint onde você deseja testar a Web Part. Crie uma nova página para testar a Web Part.

1. Use o seletor de Web Part para localizar a Web Part **GraphTutorial** e adicioná-la à página.

    ![Uma captura de tela da Web Part GraphTutorial no seletor de Web Part](images/add-web-part.png)

1. O token de acesso é impresso abaixo do **Bem-vindo ao SharePoint!** mensagem na Web Part. Você pode copiar esse token e analisá-lo [https://jwt.ms/](https://jwt.ms/) para confirmar se ele contém os escopos de permissão exigidos pela Web Part.

    ![Uma captura de tela da Web Part exibindo um token de acesso](images/access-token.png)
