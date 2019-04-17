---
title: Criteri di rete
description: In questo argomento viene fornita una panoramica dei criteri di rete per Server dei criteri di rete in Windows Server 2016 e include collegamenti a indicazioni aggiuntive sui criteri di rete.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7e1604f4839bd955e5ea10d9eafea5ef0c978977
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="network-policies"></a>Criteri di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per una panoramica dei criteri di rete in Criteri di rete.

>[!NOTE]
>Oltre a questo argomento, è disponibile la seguente documentazione di criteri di rete.
> - [Autorizzazione di accesso](nps-np-access.md)
> - [Configurare i criteri di rete](nps-np-configure.md)

Criteri di rete sono insiemi di condizioni, vincoli e le impostazioni che consentono di definire chi è autorizzato a connettersi alla rete e le circostanze in cui possono o non riesci a connetterti.

Durante l'elaborazione delle richieste di connessione come server RADIUS Remote Authentication Dial-In User Service (), dei criteri di rete esegue sia l'autenticazione e autorizzazione per la richiesta di connessione. Durante il processo di autenticazione, dei criteri di rete verifica l'identità dell'utente o computer che si connette alla rete. Durante il processo di autorizzazione, dei criteri di rete determina se l'utente o il computer può accedere alla rete.

Per apportare queste determinazioni, dei criteri di rete utilizza criteri di rete che sono configurati nella console di criteri di rete. Criteri di rete esamina inoltre le proprietà di connessione dell'account utente in Active Directory&reg; \(AD DS\) servizi di dominio per eseguire l'autorizzazione.

## <a name="network-policies---an-ordered-set-of-rules"></a>Criteri di rete - un Set ordinato di regole

Criteri di rete possono essere visualizzati sotto forma di regole. Ogni regola è un set di condizioni e impostazioni. Criteri di rete confronta le condizioni della regola per le proprietà delle richieste di connessione. Se si verifica una corrispondenza tra la regola e la richiesta di connessione, vengono applicate le impostazioni definite nella regola per la connessione.

Quando sono configurati più criteri di rete in Criteri di rete, sono un set ordinato di regole. Criteri di rete controlla ogni richiesta di connessione con la prima regola nell'elenco, quindi il secondo e così via, fino a quando non viene trovata una corrispondenza.

Ogni criterio di rete ha un **lo stato dei criteri** l'impostazione che consente di abilitare o disabilitare il criterio. Quando si disabilita un criterio di rete, dei criteri di rete non valuta i criteri quando autorizzare le richieste di connessione.

>[!NOTE]
>Se si desidera dei criteri di rete per valutare i criteri di rete durante l'autorizzazione per le richieste di connessione, è necessario configurare il **lo stato dei criteri** impostazione selezionando i criteri è abilitata la casella di controllo.

## <a name="network-policy-properties"></a>Proprietà di criteri di rete

Esistono quattro categorie di proprietà per ogni criterio di rete:

### <a name="overview"></a>Panoramica

 Queste proprietà consentono di specificare se il criterio è abilitato, se il criterio o nega l'accesso e se un metodo di connessione di rete specifico o un tipo di server di accesso di rete (NAS), è necessaria per le richieste di connessione. Proprietà di panoramica consentono inoltre di specificare se la proprietà di connessione remota degli account utente di dominio Active Directory vengono ignorate. Se si seleziona questa opzione, solo le impostazioni nei criteri di rete vengono utilizzate da criteri di rete per determinare se la connessione è autorizzata.


### <a name="conditions"></a>Condizioni

 Queste proprietà consentono di specificare le condizioni che deve avere la richiesta di connessione per la corrispondenza con i criteri di rete. Se le condizioni configurate nel criterio corrispondano alla richiesta di connessione, dei criteri di rete applica le impostazioni indicate nei criteri di rete per la connessione. Ad esempio, se si specifica l'indirizzo IPv4 NAS come condizione per i criteri di rete e dei criteri di rete riceve una richiesta di connessione da un server NAS con l'indirizzo IP specificato, la condizione nel criterio corrisponde la richiesta di connessione. 


### <a name="constraints"></a>Vincoli

 I vincoli sono parametri aggiuntivi dei criteri di rete necessari per trovare la corrispondenza la richiesta di connessione. Se non corrisponde un vincolo per la richiesta di connessione, dei criteri di rete automaticamente rifiuta la richiesta. A differenza di risposta dei criteri di rete di condizioni che non soddisfano i criteri di rete, se un vincolo non corrisponde, dei criteri di rete rifiuta la richiesta di connessione senza valutare ulteriori criteri di rete.

### <a name="settings"></a>Impostazioni

 Queste proprietà consentono di specificare le impostazioni dei criteri di rete si applica alla richiesta di connessione, se sono soddisfatte tutte le condizioni dei criteri di rete per il criterio.

Quando si aggiunge un nuovo criterio di rete tramite la console dei criteri di rete, è necessario utilizzare la creazione guidata criteri di rete. Dopo aver creato un criterio di rete utilizzando la procedura guidata, è possibile personalizzare i criteri facendo doppio clic sul criterio nella console di criteri di rete per ottenere le proprietà del criterio.

Per esempi di caratteri jolly per specificare gli attributi dei criteri di rete, vedere [utilizzare espressioni regolari in Criteri di rete](nps-crp-reg-expressions.md).

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).
