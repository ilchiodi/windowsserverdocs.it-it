---
title: Modelli di criteri di rete
description: In questo argomento viene fornita una panoramica dei modelli di Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2835959b8c076ef7b6aeb1fca31a62717ef95037
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="nps-templates"></a>Modelli di criteri di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Modelli \(NPS\) Server dei criteri di rete consentono di creare elementi di configurazione, ad esempio i client di Remote Authentication Dial-In User Service \(RADIUS\) o i segreti condivisi, è possibile riutilizzare sul server dei criteri di rete e l'esportazione per l'utilizzo in altri server dei criteri di rete locale.

Modelli di criteri di rete sono progettati per ridurre la quantità di tempo e i costi necessari per configurare criteri di rete in uno o più server. I tipi di modello dei criteri di rete seguenti sono disponibili per la configurazione in Gestione modelli:

- Segreti condivisi
- Client RADIUS
- Server RADIUS remoti
- Filtri IP
- Gruppi di Server di monitoraggio e aggiornamento

Configurazione di un modello è diversa dalla configurazione direttamente il server dei criteri di rete. Creazione di un modello non altera la funzionalità del server dei criteri di rete. È solo quando si seleziona il modello nella posizione appropriata nella console di criteri di rete che il modello influisce sulla funzionalità server dei criteri di rete. 

Ad esempio, se si configura un client RADIUS nella console dei criteri di rete client e server RADIUS, si hanno modificato la configurazione del server dei criteri di rete e prendere un passaggio della configurazione dei criteri di rete per comunicare con una delle tua rete accesso server \(NAS's\). \ (Il passaggio successivo, è possibile configurare il server NAS per comunicare con criteri di rete. \), tuttavia, se si configura un nuovo modello di client RADIUS nella console di criteri di rete in **Gestione modelli** invece di creare un nuovo client RADIUS in **client e server RADIUS**, è stato creato un modello, ma non è stato modificato ancora la funzionalità del server dei criteri di rete. Per modificare la funzionalità del server dei criteri di rete, è necessario selezionare il modello dal percorso corretto nella console di criteri di rete.

## <a name="creating-templates"></a>Creazione di modelli

Per creare un modello, aprire la console dei criteri di rete, fare doppio clic su un tipo di modello, ad esempio **filtri IP**, quindi fare clic su **New**. Verrà visualizzata una finestra di dialogo di proprietà nuovo modello che consente di configurare il modello.

## <a name="using-templates-locally"></a>Utilizzare i modelli localmente

È possibile utilizzare un modello che hai creato in **Gestione modelli** passando a una posizione nella console di criteri di rete in cui può essere applicato il modello. Ad esempio, se si crea un nuovo modello i segreti condivisi che si desidera applicare a una configurazione di client RADIUS, in **client e server RADIUS** e **client RADIUS**, aprire le proprietà di client RADIUS. In **selezionare un modello esistente segreti condivisi**, selezionare il modello creato in precedenza dall'elenco dei modelli disponibili.

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).
