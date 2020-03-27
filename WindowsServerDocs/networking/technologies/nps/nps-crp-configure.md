---
title: Configurare i criteri di richiesta di connessione
description: Questo argomento fornisce informazioni su come configurare i criteri di richiesta di connessione in server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7b42dd9470b44b0f1c7d25627d491cd6f2a2dfae
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316286"
---
# <a name="configure-connection-request-policies"></a>Configurare i criteri di richiesta di connessione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per creare e configurare i criteri di richiesta di connessione che definiscono se il server dei criteri di server locale elabora le richieste di connessione o le invia al server RADIUS remoto per l'elaborazione.

I criteri di richiesta di connessione sono set di condizioni e impostazioni che consentono agli amministratori di rete di designare i server Remote Authentication Dial-In User Service (RADIUS) che eseguono l'autenticazione e l'autorizzazione delle richieste di connessione che il server che esegue Server dei criteri di rete \(NPS\) riceve dai client RADIUS.

Il criterio di richiesta di connessione predefinito usa NPS come server RADIUS ed elabora tutte le richieste di autenticazione in locale.

Per configurare un server che esegue Server dei criteri di gruppo in modo che funga da proxy RADIUS e inoltri richieste di connessione ad altri server NPS o RADIUS, è necessario configurare un gruppo di server RADIUS remoti, oltre ad aggiungere un nuovo criterio di richiesta di connessione che specifichi le condizioni e le impostazioni che le richieste di connessione devono corrispondere.

È possibile creare un nuovo gruppo di server RADIUS remoto durante la creazione di un nuovo criterio di richiesta di connessione con la creazione guidata nuovo criterio di richiesta di connessione.

Se non si desidera che il server dei criteri di Server funga da server RADIUS ed elabora le richieste di connessione localmente, è possibile eliminare il criterio di richiesta di connessione predefinito.

Se si desidera che il server dei criteri di gruppo funga da server RADIUS, elabora le richieste di connessione localmente e come proxy RADIUS, inviando alcune richieste di connessione a un gruppo di server RADIUS remoti, aggiungere un nuovo criterio usando la procedura seguente e quindi verificare che il valore predefinito il criterio di richiesta di connessione è l'ultimo criterio elaborato inserendolo per ultimo nell'elenco dei criteri.

## <a name="add-a-connection-request-policy"></a>Aggiungere un criterio di richiesta di connessione

L'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-add-a-new-connection-request-policy"></a>Per aggiungere un nuovo criterio di richiesta di connessione 

1. In Server Manager fare clic su **strumenti**e quindi su **Server dei criteri di rete** per aprire la console server dei criteri di rete. 
2. Nell'albero della console fare doppio clic su **criteri**.
3. Fare clic con il pulsante destro del mouse su **criteri di richiesta di connessione**, quindi scegliere **nuovo criterio richiesta di connessione**.
4. Utilizzare la creazione guidata nuovo criterio di richiesta di connessione per configurare i criteri di richiesta di connessione e, se non configurati in precedenza, un gruppo di server RADIUS remoti.


Per ulteriori informazioni sulla gestione dei server dei criteri di rete, vedere [Manage Network Policy Server](nps-manage-top.md).

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).

