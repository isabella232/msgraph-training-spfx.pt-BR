---
ms.openlocfilehash: 5164c68a78837c179636f685d28ddf61f93f8415
ms.sourcegitcommit: 523e64e972f247e2df57cd8de949ac7bd1b8b047
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "48823265"
---
<!-- markdownlint-disable MD002 MD041 -->

Neste tutorial, você criará uma [Web Part do lado do cliente do SharePoint](https://docs.microsoft.com/sharepoint/dev/spfx/web-parts/overview-client-side-web-parts) que usará o Microsoft Graph para obter o calendário do usuário para a semana atual e permitir que o usuário adicione um evento ao seu calendário.

## <a name="create-a-web-part-project"></a>Criar um projeto de Web Part

1. Abra a interface de linha de comando (CLI) em um diretório vazio onde você deseja criar o projeto. Execute o seguinte comando para iniciar o gerador do SharePoint do Yeoman.

    ```Shell
    yo @microsoft/sharepoint
    ```

1. Responda aos prompts da seguinte maneira.

    - **Qual é o nome da sua solução?** `graph-tutorial`
    - **Quais pacotes de linha de base deseja direcionar para seus componentes?** `SharePoint Online only (latest)`
    - **Onde você deseja colocar os arquivos?** `Use the current folder`
    - **Você deseja permitir que o administrador de locatários possa implantar a solução para todos os sites imediatamente sem executar qualquer implantação de recurso ou adicionar aplicativos aos sites?** `Yes`
    - **Os componentes da solução exigem permissões para acessar as APIs da Web que são exclusivas e não são compartilhadas com outros componentes nos dez Ants?** `No`
    - **Que tipo de componente do cliente a ser criado?** `WebPart`
    - **Qual é o nome da Web Part?** `GraphTutorial`
    - **Qual é a descrição da Web Part?** `GraphTutorial description`
    - **Qual estrutura você deseja usar?** `No JavaScript framework`

1. Execute o seguinte comando para atualizar a versão TypeScript no projeto para 3,7.

    ```Shell
    npm install @microsoft/rush-stack-compiler-3.7 --save-dev
    ```

1. Abrir **./tsconfig.js** e substituir `rush-stack-compiler-3.3` por `rush-stack-compiler-3.7` .

1. Abra **./tslint.js** e remova a `"no-use-before-declare": true,` linha. A `no-use-before-declare` regra foi preterida e causará um erro durante o processo de compilação.

## <a name="install-dependencies"></a>Instalar dependências

Antes de prosseguir, instale alguns pacotes adicionais do NPM que serão usados posteriormente.

- [Definições do TypeScript do Microsoft Graph](https://github.com/microsoftgraph/msgraph-typescript-typings) para fornecer IntelliSense para objetos do Microsoft Graph.
- [Kit de ferramentas do Microsoft Graph](https://docs.microsoft.com/graph/toolkit/overview) para fornecer componentes da interface do usuário para a Web Part.
- [Data-FNS](https://date-fns.org/) para funções úteis para trabalhar com datas.

```Shell
npm install @microsoft/microsoft-graph-types@1.17.0 --save-dev
npm install @microsoft/mgt@1.3.4 date-fns @2.16.0
```
