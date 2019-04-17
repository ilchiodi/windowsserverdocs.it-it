---
title: Configurare i criteri di richiesta di connessione
description: In questo argomento fornisce informazioni su come configurare criteri richiesta di connessione nel Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9677e147bdaea4de71a054cd6c52d81126e005d1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-connection-request-policies"></a>Configurare i criteri di richiesta di connessione

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per creare e configurare criteri di richiesta di connessione che indicano se il server dei criteri di rete locale elabora le richieste di connessione o li inoltra al server RADIUS remoti per l'elaborazione.

Criteri di richiesta di connessione sono insiemi di condizioni e le impostazioni che consentono agli amministratori di rete specificare quali server RADIUS Remote Authentication Dial-In User Service () eseguire l'autenticazione e autorizzazione delle richieste di connessione che il server che esegue Server dei criteri di rete \(NPS\) riceve da client RADIUS.

Il criterio di richiesta di connessione predefinito utilizza criteri di rete come server RADIUS ed elabora localmente tutte le richieste di autenticazione.

Per configurare un server dei criteri di rete come proxy RADIUS e le richieste di connessione inoltrare ad altri server dei criteri di rete o RADIUS, è necessario configurare un gruppo di server RADIUS remoto oltre ad aggiungere un nuovo criterio di richiesta di connessione che specifica le condizioni e le impostazioni che devono soddisfare le richieste di connessione.

Durante la creazione di un nuovo criterio di richiesta di connessione con la creazione guidata nuovo criterio di richiesta di connessione, è possibile creare un nuovo gruppo di server RADIUS remoto.

Se non vuoi il server dei criteri di rete per fungere da una connessione di processo e server RADIUS richieste in locale, è possibile eliminare il criterio di richiesta di connessione predefinito.

Se si desidera che il server dei criteri di rete per fungere sia da server RADIUS, l'elaborazione delle richieste di connessione in locale e come proxy RADIUS, inoltro alcune richieste di connessione a un gruppo di server RADIUS remoto, aggiungere un nuovo criterio utilizzando la procedura seguente e quindi verificare che il criterio di richiesta di connessione predefinito è l'ultimo criterio elaborato posizionandolo ultima nell'elenco dei criteri.

## <a name="add-a-connection-request-policy"></a>Aggiungere un criterio di richiesta di connessione

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-add-a-new-connection-request-policy"></a>Per aggiungere un nuovo criterio di richiesta di connessione 

1. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete** per aprire la console dei criteri di rete. 
2. Nell'albero della console, fare doppio clic su **criteri**.
3. Fare doppio clic su **criteri richiesta di connessione**, quindi fare clic su **nuovo criterio di richiesta connessione**.
4. Utilizzare Creazione guidata nuovo criterio richiesta di connessione per configurare la connessione criterio di richiesta e, se non configurati in precedenza, un gruppo di server RADIUS remoto.


Per ulteriori informazioni sulla gestione dei criteri di rete, vedere [gestire Server dei criteri di rete](nps-manage-top.md).

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).

