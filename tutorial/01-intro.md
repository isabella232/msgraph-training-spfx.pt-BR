---
ms.openlocfilehash: bb55b2bea2497628ceb2e09c889ce62e773d8e71
ms.sourcegitcommit: 523e64e972f247e2df57cd8de949ac7bd1b8b047
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "48823268"
---
<!-- markdownlint-disable MD002 MD041 -->

Este tutorial ensina como criar uma [Web Part do lado do cliente do SharePoint](https://docs.microsoft.com/sharepoint/dev/spfx/web-parts/overview-client-side-web-parts) que usa a API do Microsoft Graph para recuperar informações de calendário de um usuário.

> [!TIP]
> Se preferir baixar o tutorial concluído, você poderá baixar ou clonar o repositório do [GitHub](https://github.com/microsoftgraph/msgraph-training-spfx).

## <a name="prerequisites"></a>Pré-requisitos

Antes de iniciar este tutorial, você deve ter as seguintes ferramentas instaladas em sua máquina de desenvolvimento.

- [Node.js](https://nodejs.org/en/download/releases/)
- [Yeoman](https://yeoman.io/)
- [Gulp](https://gulpjs.com/)
- [Gerador Yeoman do SharePoint](https://docs.microsoft.com/sharepoint/dev/spfx/toolchain/scaffolding-projects-using-yeoman-sharepoint-generator)

Você pode encontrar mais detalhes sobre os requisitos para o desenvolvimento da estrutura do SharePoint em [configurar seu ambiente de desenvolvimento do SharePoint Framework](https://docs.microsoft.com/sharepoint/dev/spfx/set-up-your-development-environment).

> [!IMPORTANT]
> A estrutura do SharePoint requer o Node.js versão 10. x. Certifique-se de instalar a versão correta.

Você também deve ter uma conta corporativa ou de estudante da Microsoft, com acesso a uma conta de administrador global na mesma organização. Se você não tiver uma conta da Microsoft, poderá [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.

O Microsoft 365 locatário deve ser [configurado para o desenvolvimento da estrutura do SharePoint](https://docs.microsoft.com/sharepoint/dev/spfx/set-up-your-developer-tenant), com um catálogo de aplicativos e um site de teste criados antes de iniciar este tutorial.

> [!NOTE]
> Este tutorial foi escrito com as seguintes versões das ferramentas acima. As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.
>
> - Node.js 10.22.0
> - Yeoman 3.1.1
> - Gulp 4.0.2
> - Yeoman do gerador do SharePoint 1.11.0

## <a name="feedback"></a>Comentários

Forneça comentários sobre este tutorial no [repositório do GitHub](https://github.com/microsoftgraph/msgraph-training-spfx).
