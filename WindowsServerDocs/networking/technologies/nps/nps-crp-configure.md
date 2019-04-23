---
title: Configurare i criteri di richiesta di connessione
description: In questo argomento vengono fornite informazioni su come configurare criteri di richiesta di connessione nel Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4f80f9fb8be0c44cfb5685e5b9cc489282e4961d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884512"
---
# <a name="configure-connection-request-policies"></a>Configurare i criteri di richiesta di connessione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per creare e configurare i criteri di richiesta di connessione che definisce se i criteri di rete locale elabora le richieste di connessione o li inoltra al server RADIUS remoto per l'elaborazione.

I criteri di richiesta di connessione sono set di condizioni e le impostazioni che consentono agli amministratori di rete definire quali server RADIUS Remote Authentication Dial-In User Service () eseguono l'autenticazione e autorizzazione di connessione richiede che la server che esegue Server dei criteri di rete \(NPS\) riceve dai client RADIUS.

Il criterio di richiesta connessione predefinito utilizza criteri di rete come server RADIUS ed elabora tutte le richieste di autenticazione in locale.

Per configurare un server dei criteri di rete da usare come proxy RADIUS e le richieste di connessione diretta ad altri server RADIUS o dei criteri di rete, è necessario configurare un gruppo di server RADIUS remoti oltre ad aggiungere un nuovo criterio richiesta di connessione che specifica le condizioni e le impostazioni che devono corrispondere alle richieste di connessione.

È possibile creare un nuovo gruppo di server RADIUS remoto durante la creazione di un nuovo criterio richiesta di connessione con la creazione guidata nuovo criterio richiesta di connessione.

Se non si desidera NPS di agire come una connessione RADIUS server ed elaborare le richieste in locale, è possibile eliminare il criterio di richiesta connessione predefinito.

Se si desidera che i criteri di rete di funzionare come sia un server RADIUS, l'elaborazione delle richieste di connessione in locale e come proxy RADIUS, alcune richieste di connessione di inoltro a un gruppo di server RADIUS remoti, aggiungere un nuovo criterio utilizzando la procedura seguente e quindi verificare che il valore predefinito criterio di richiesta di connessione è l'ultimo criterio elaborato inserendolo ultimo nell'elenco dei criteri.

## <a name="add-a-connection-request-policy"></a>Aggiungere un criterio di richiesta di connessione

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

### <a name="to-add-a-new-connection-request-policy"></a>Per aggiungere un nuovo criterio richiesta di connessione 

1. In Server Manager fare clic su **degli strumenti**, quindi fare clic su **Server dei criteri di rete** per aprire la console Criteri di rete. 
2. Nell'albero della console, fare doppio clic su **criteri**.
3. Fare doppio clic su **criteri di richiesta di connessione**, quindi fare clic su **nuovo criterio richiesta connessione**.
4. Utilizzare Creazione guidata nuovo criterio richiesta di connessione per configurare la connessione del criterio di richiesta e, se non configurati in precedenza, un gruppo di server RADIUS remoti.


Per altre informazioni sulla gestione dei criteri di rete, vedere [gestire i Server dei criteri di rete](nps-manage-top.md).

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).

