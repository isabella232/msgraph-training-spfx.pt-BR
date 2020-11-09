---
ms.openlocfilehash: ca72a0d6047d3cd275d34e2d0b4e3c54dbab5375
ms.sourcegitcommit: 523e64e972f247e2df57cd8de949ac7bd1b8b047
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "48831259"
---
# <a name="how-to-run-the-completed-project"></a>Como executar o projeto concluído

## <a name="prerequisites"></a>Pré-requisitos

Para executar o projeto concluído nessa pasta, você precisará do seguinte:

- [Node.js](https://nodejs.org/en/download/releases/) versão 10. x
- [Gulp](https://gulpjs.com/)
- Uma conta corporativa ou de estudante da Microsoft, com acesso a uma conta de administrador global na mesma organização. Se você não tiver uma conta da Microsoft, poderá [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.
- O Microsoft 365 locatário deve ser [configurado para o desenvolvimento da estrutura do SharePoint](https://docs.microsoft.com/sharepoint/dev/spfx/set-up-your-developer-tenant), com um catálogo de aplicativos e um site de teste criados antes de iniciar este tutorial.

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

    ![Uma captura de tela da página de acesso à API do centro de administração do SharePoint](../tutorial/images/api-access.png)

### <a name="test-the-web-part"></a>Testar a Web Part

1. Vá para um site do SharePoint onde você deseja testar a Web Part. Crie uma nova página para testar a Web Part.

1. Use o seletor de Web Part para localizar a Web Part **GraphTutorial** e adicioná-la à página.

    ![Uma captura de tela da Web Part GraphTutorial no seletor de Web Part](../tutorial/images/add-web-part.png)
