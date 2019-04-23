---
title: Criteri di rete
description: In questo argomento offre una panoramica di criteri di rete per Server dei criteri di rete in Windows Server 2016 e include collegamenti a indicazioni aggiuntive dei criteri di rete.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 60ab80bb6cf26578430b76806405a65a0f596ef2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848952"
---
# <a name="network-policies"></a>Criteri di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per una panoramica di criteri di rete in Criteri di rete.

>[!NOTE]
>Oltre a questo argomento, la documentazione di criteri di rete seguente è disponibile.
> - [Autorizzazione di accesso](nps-np-access.md)
> - [Configurare i criteri di rete](nps-np-configure.md)

I criteri di rete sono set di condizioni, vincoli e le impostazioni che consentono di definire chi è autorizzato a connettersi alla rete e le circostanze in cui possono o non è possibile connettersi.

Quando si elaborano le richieste di connessione come server RADIUS Remote Authentication Dial-In User Service (), dei criteri di rete esegue l'autenticazione e autorizzazione per la richiesta di connessione. Durante il processo di autenticazione dei criteri di rete consente di verificare l'identità dell'utente o del computer che si connette alla rete. Durante il processo di autorizzazione, dei criteri di rete determina se l'utente o il computer è consentito accedere alla rete.

Per rendere queste decisioni, dei criteri di rete Usa i criteri di rete configurati nella console Criteri di rete. NPS esamina anche le proprietà di connessione remota dell'account utente in Active Directory&reg; servizi di dominio \(Active Directory Domain Services\) per eseguire l'autorizzazione.

## <a name="network-policies---an-ordered-set-of-rules"></a>Criteri di rete - un Set ordinato di regole

I criteri di rete possono essere visualizzati sotto forma di regole. Ogni regola ha un set di condizioni e le impostazioni. Criteri di rete confronta le condizioni della regola per le proprietà delle richieste di connessione. Se viene rilevata una corrispondenza tra la regola e la richiesta di connessione, vengono applicate le impostazioni definite nella regola per la connessione.

Quando sono configurati più criteri di rete in Criteri di rete, sono un set ordinato di regole. NPS controlla ogni richiesta di connessione con la prima regola nell'elenco, quindi il secondo e così via, fino a quando non viene trovata una corrispondenza.

I criteri di rete includono una **lo stato dei criteri** impostazione che consente di abilitare o disabilitare i criteri. Quando si disabilita un criterio di rete, NPS non valuta i criteri quando si autorizza le richieste di connessione.

>[!NOTE]
>Se si desidera che Criteri di rete per valutare i criteri di rete quando si esegue l'autorizzazione per le richieste di connessione, è necessario configurare il **lo stato dei criteri** impostazione selezionando i criteri abilitati casella di controllo.

## <a name="network-policy-properties"></a>Proprietà dei criteri di rete

Esistono quattro categorie di proprietà per ogni criterio di rete:

### <a name="overview"></a>Panoramica

 Queste proprietà consentono di specificare se il criterio è abilitato, se il criterio concede o nega l'accesso e se un metodo di connessione di rete specifico o un tipo di server di accesso di rete (NA), è necessaria per le richieste di connessione. Proprietà Panoramica consentono anche di specificare se la proprietà di connessione remota degli account utente in Active Directory Domain Services vengono ignorate. Se si seleziona questa opzione, vengono utilizzate solo le impostazioni nei criteri di rete da NPS per determinare se la connessione è autorizzata.


### <a name="conditions"></a>Condizioni

 Queste proprietà consentono di specificare le condizioni che deve avere la richiesta di connessione per la corrispondenza con i criteri di rete. Se le condizioni configurate nei criteri di corrispondano la richiesta di connessione, dei criteri di rete applica le impostazioni indicate nei criteri di rete per la connessione. Ad esempio, se si specifica l'indirizzo IPv4 NAS come condizione per i criteri di rete e dei criteri di rete riceve una richiesta di connessione da un server NAS con l'indirizzo IP specificato, la condizione nei criteri corrisponde alla richiesta di connessione. 


### <a name="constraints"></a>Vincoli

 I vincoli sono parametri aggiuntivi dei criteri di rete necessarie in base alla richiesta di connessione. Se la richiesta di connessione non corrisponde un vincolo, dei criteri di rete Rifiuta automaticamente la richiesta. A differenza di risposta dei criteri di rete alle condizioni di mancata corrispondenza nei criteri di rete, se un vincolo non viene trovata una corrispondenza, NPS Nega la richiesta di connessione senza valutare i criteri di rete aggiuntiva.

### <a name="settings"></a>Impostazioni

 Queste proprietà consentono di specificare le impostazioni dei criteri di rete viene applicato alla richiesta di connessione se tutte le condizioni di criteri di rete per i criteri vengono soddisfatti.

Quando si aggiunge un nuovo criterio di rete tramite la console Criteri di rete, è necessario utilizzare la creazione guidata nuovo criterio di rete. Dopo aver creato un criterio di rete tramite la procedura guidata, è possibile personalizzare il criterio facendo doppio clic sul criterio nella console Criteri di rete per ottenere le proprietà dei criteri.

Per esempi di caratteri jolly per specificare gli attributi di criteri di rete, vedere [utilizzo delle espressioni regolari in Criteri di rete](nps-crp-reg-expressions.md).

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).
