---
ms.openlocfilehash: ee9addcbcf58fa971b3e67fb3d84cfb36bf275dd
ms.sourcegitcommit: 523e64e972f247e2df57cd8de949ac7bd1b8b047
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "48823282"
---
<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você usará o [Kit de ferramentas do Microsoft Graph](https://docs.microsoft.com/graph/toolkit/overview) para substituir a lista simples de eventos pela interface do usuário avançada.

O kit de ferramentas fornece um [componente de agenda](https://docs.microsoft.com/graph/toolkit/components/agenda), que é bem adequado para renderizar nossa lista de eventos.

## <a name="update-the-web-part"></a>Atualizar a Web Part

1. Abra **./src/WebParts/graphTutorial/GraphTutorialWebPart.Module.SCSS**. Altere o valor do `background-color` atributo na `.row` entrada para `$ms-color-white` .

    :::code language="css" source="../demo/graph-tutorial/src/webparts/graphTutorial/GraphTutorialWebPart.module.scss" id="rowScssSnippet" highlight="4":::

1. Adicione a seguinte entrada dentro da `.graphTutorial` entrada.

    :::code language="css" source="../demo/graph-tutorial/src/webparts/graphTutorial/GraphTutorialWebPart.module.scss" id="addSocialBtnSnippet":::

1. Abra **./src/WebParts/graphTutorial/GraphTutorialWebPart.TS** e adicione a instrução a seguir `import` na parte superior do arquivo.

    ```typescript
    import { Providers, SharePointProvider, MgtAgenda } from '@microsoft/mgt';
    ```

1. Adicione a função a seguir à classe **GraphTutorialWebPart** para inicializar o kit de ferramentas.

    :::code language="typescript" source="../demo/graph-tutorial/src/webparts/graphTutorial/GraphTutorialWebPart.ts" id="onInitSnippet":::

1. Substitua a função `renderCalendarView` existente pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/webparts/graphTutorial/GraphTutorialWebPart.ts" id="renderCalendarViewSnippet":::

    Isso substitui a lista básica pelo componente de **agenda** do kit de ferramentas.

1. Criar, empacotar e recarregar a Web Part e, em seguida, atualizar a página em que você está testando.

    ![Uma captura de tela da Web Part com o componente de agenda](images/mgt-agenda.png)

## <a name="an-alternate-approach"></a>Uma abordagem alternativa

Neste ponto, você tem um código que:

- Usa o **MSGraphClient** para obter o modo de exibição de calendário do usuário para a semana atual do Microsoft Graph.
- Adicione esses eventos ao componente de **agenda** do kit de ferramentas do Microsoft Graph.

Com essa abordagem, você tem controle total sobre a chamada de API de gráfico e pode fazer qualquer processamento dos eventos antes da renderização que você deseja. No entanto, se isso não for necessário, você pode simplificar, permitindo que o componente de **agenda** faça o trabalho para você.

Todos os componentes do Microsoft Graph Toolkit são capazes de fazer todas as chamadas de API relevantes para o Microsoft Graph. Por exemplo, basta adicionar o componente de **agenda** à Web Part e não definir nenhuma propriedade, a Web Part usaria suas configurações padrão para obter eventos para o dia atual. Vamos ver como podemos obter os mesmos resultados que temos atualmente (eventos para a semana atual).

1. Substitua o `render` método existente pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/webparts/graphTutorial/GraphTutorialWebPart.ts" id="alternateRenderSnippet":::

    Agora, em vez de fazer uma chamada de API no `render` , basta adicionar um `mgt-agenda` elemento diretamente ao HTML. Configurando `date` o início da semana e `days` para 7, o componente fará com que a mesma chamada de API seja `render` feita.

1. Adicione a seguinte função vazia à classe **GraphTutorialWebPart** .

    ```typescript
    private async addSocialToCalendar() {}
    ```

    > [!NOTE]
    > Também adicionamos um botão **Add Team social** à Web Part e adicionamos o `addSocialToCalendar` método como um ouvinte de eventos.  Você implementará o código por trás da próxima seção. Por enquanto, só queremos que o código seja compilado.

1. Criar, empacotar e recarregar a Web Part e, em seguida, atualizar a página em que você está testando. O modo de exibição deve ser igual ao teste anterior.

### <a name="using-the-toolkit-vs-making-api-calls"></a>Usando o kit de ferramentas vs fazendo chamadas de API

Neste ponto, você pode estar se perguntando por que deu o problema de usar o **MSGraphClient** , quando o kit de ferramentas faz o trabalho para você. O kit de ferramentas foi projetado para renderizar os resultados consultados do Microsoft Graph, como uma lista de eventos. No entanto, há situações em que é necessário fazer chamadas de API por conta própria.

- Qualquer chamada de API que não seja uma `GET` solicitação. Por exemplo, criar um novo evento no calendário ou atualizar o número de telefone de um usuário.
- Chamadas de API para obter os dados que devem ser usados "nos bastidores" e não são renderizados diretamente.
