---
title: Modelli Server dei criteri di rete
description: In questo argomento viene fornita una panoramica dei modelli di Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0647dbf0f99a01e32ba68475b439501e2dbeebfe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823312"
---
# <a name="nps-templates"></a>Modelli Server dei criteri di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Server dei criteri di rete \(NPS\) modelli consentono di creare elementi di configurazione, ad esempio Remote Authentication Dial-In User Service \(RADIUS\) client o i segreti condivisi, che è possibile riutilizzare in locale Criteri di rete e l'esportazione per l'utilizzo in altri NPSs.

Modelli di criteri di rete sono progettati per ridurre la quantità di tempo e i costi necessari per configurare criteri di rete in uno o più server. I tipi di modello dei criteri di rete seguenti sono disponibili per la configurazione in Gestione modelli:

- Segreti condivisi
- Client RADIUS
- Server RADIUS remoti
- Filtri IP
- Gruppi di Server di monitoraggio e aggiornamento

Configurazione di un modello è diversa rispetto alla configurazione direttamente i criteri di rete. Creazione di un modello non incide sulla funzionalità di NPS. È solo quando si seleziona il modello nella posizione appropriata nella console Criteri di rete che il modello influisce sulla funzionalità dei criteri di rete. 

Ad esempio, se si configura un client RADIUS nella console Criteri di rete nel client e server RADIUS, si hanno modificato la configurazione dei criteri di rete ed eseguito un passaggio della configurazione dei criteri di rete per comunicare con uno dei server di accesso di rete \(del NAS\) . \(Il passaggio successivo consisterà nel configurare il server NAS per comunicare con criteri di rete.\) Tuttavia, se si configura un modello di client RADIUS nuovo nella console Criteri di rete in **modelli di gestione** invece di creare un nuovo client RADIUS in **client e server RADIUS**, aver creato un modello, ma non abbiano alterato le funzionalità dei criteri di rete ancora. Per modificare la funzionalità dei criteri di rete, è necessario selezionare il modello dal percorso corretto nella console Criteri di rete.

## <a name="creating-templates"></a>Creazione di modelli

Per creare un modello, aprire la console Criteri di rete, fare doppio clic su un tipo di modello, ad esempio **i filtri IP**, quindi fare clic su **New**. Viene visualizzata una nuova finestra di dialogo proprietà di modello che consente di configurare il modello.

## <a name="using-templates-locally"></a>Usando i modelli in locale

È possibile usare un modello che sono stati creati in **modelli di gestione** passando a un percorso nella console di NPS in cui può essere applicato il modello. Ad esempio, se si crea un nuovo modello dei segreti condivisi che si desidera applicare a una configurazione di client RADIUS, in **client e server RADIUS** e **client RADIUS**, aprire le proprietà di client RADIUS. Nelle **selezionare un modello esistente di segreti condivisi**, selezionare il modello creato in precedenza dall'elenco dei modelli disponibili.

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).
