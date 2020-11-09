---
ms.openlocfilehash: 0d7c9cc909e2f848cc9760d5862bb1863501f02b
ms.sourcegitcommit: 523e64e972f247e2df57cd8de949ac7bd1b8b047
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "48823258"
---
<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você atualizará a Web Part para permitir que o usuário adicione um evento ao seu calendário para a hora social semanal da equipe. Neste cenário, a equipe tem um horário social semanal em 4 PM na sexta-feira.

1. Abra **./src/WebParts/graphTutorial/GraphTutorialWebPart.TS** e substitua o `addSocialToCalendar()` método existente pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/webparts/graphTutorial/GraphTutorialWebPart.ts" id="addSocialToCalendarSnippet":::

    Considere o que esse código faz.

    - Ele determina a próxima sexta-feira seguinte e cria uma **Data** para 4 p.m. nesse dia.
    - Ele cria um novo objeto **MicrosoftGraph. Event** , definindo o início para o valor da **Data** e o final de uma hora mais tarde.
    - Ele usa o **MSGraphClient** para postar o novo evento no `/me/events` ponto de extremidade.
    - Ele renderiza novamente a Web Part para que o modo de exibição seja atualizado com o novo evento.

1. Criar, empacotar e recarregar a Web Part e, em seguida, atualizar a página em que você está testando.

1. Clique no botão **Adicionar equipe social** . Depois que a página for atualizada, role para baixo até sexta-feira e localize o novo evento.

    ![Uma captura de tela do evento recém-criado renderizado na Web Part](images/new-event.png)
