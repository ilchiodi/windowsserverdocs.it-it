---
title: Criteri di rete
description: In questo argomento viene fornita una panoramica dei criteri di rete per server dei criteri di rete in Windows Server 2016 e sono inclusi collegamenti a indicazioni aggiuntive su server dei criteri di rete.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c241191f54080ed92a1f1a274c0b2aff0b8c564c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405351"
---
# <a name="network-policies"></a>Criteri di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per una panoramica dei criteri di rete nel server dei criteri di rete.

>[!NOTE]
>Oltre a questo argomento, è disponibile la documentazione seguente relativa ai criteri di rete.
> - [Autorizzazione di accesso](nps-np-access.md)
> - [Configurare i criteri di rete](nps-np-configure.md)

I criteri di rete sono set di condizioni, vincoli e impostazioni che consentono di designare gli utenti autorizzati a connettersi alla rete e le circostanze in cui possono o non possono connettersi.

Quando si elaborano le richieste di connessione come server Remote Authentication Dial-In User Service (RADIUS), NPS esegue sia l'autenticazione che l'autorizzazione per la richiesta di connessione. Durante il processo di autenticazione, il server dei criteri di rete verifica l'identità dell'utente o del computer che si connette alla rete. Durante il processo di autorizzazione, NPS determina se l'utente o il computer è autorizzato ad accedere alla rete.

Per eseguire queste decisioni, NPS usa i criteri di rete configurati nella console server dei criteri di rete. NPS esamina anche le proprietà di connessione remota dell'account utente in Active Directory servizi di dominio&reg; \(servizi di dominio Active Directory\) per eseguire l'autorizzazione.

## <a name="network-policies---an-ordered-set-of-rules"></a>Criteri di rete-un set ordinato di regole

I criteri di rete possono essere visualizzati come regole. Ogni regola ha un set di condizioni e impostazioni. NPS Confronta le condizioni della regola con le proprietà delle richieste di connessione. Se viene stabilita una corrispondenza tra la regola e la richiesta di connessione, le impostazioni definite nella regola verranno applicate alla connessione.

Quando nel server dei criteri di rete sono configurati più criteri di rete, si tratta di un set ordinato di regole. NPS controlla ogni richiesta di connessione rispetto alla prima regola nell'elenco, quindi alla seconda e così via fino a quando non viene trovata una corrispondenza.

Ogni criterio di rete ha un'impostazione **dello stato dei criteri** che consente di abilitare o disabilitare i criteri. Quando si disabilita un criterio di rete, NPS non valuta i criteri durante l'autorizzazione delle richieste di connessione.

>[!NOTE]
>Se si desidera che NPS valuti i criteri di rete quando si esegue l'autorizzazione per le richieste di connessione, è necessario configurare l'impostazione **dello stato dei criteri** selezionando la casella di controllo criteri abilitati.

## <a name="network-policy-properties"></a>Proprietà dei criteri di rete

Esistono quattro categorie di proprietà per ogni criterio di rete:

### <a name="overview"></a>Panoramica

 Queste proprietà consentono di specificare se i criteri sono abilitati, se il criterio concede o nega l'accesso e se è necessario un metodo di connessione di rete specifico o un tipo di server di accesso alla rete (NAS) per le richieste di connessione. Le proprietà di panoramica consentono inoltre di specificare se le proprietà di accesso remoto degli account utente in servizi di dominio Active Directory vengono ignorate. Se si seleziona questa opzione, solo le impostazioni nei criteri di rete vengono utilizzate da server dei criteri di rete per determinare se la connessione è autorizzata.


### <a name="conditions"></a>Condizioni

 Queste proprietà consentono di specificare le condizioni che la richiesta di connessione deve avere per corrispondere ai criteri di rete. Se le condizioni configurate nei criteri corrispondono alla richiesta di connessione, server dei criteri di rete applica le impostazioni specificate nei criteri di rete alla connessione. Ad esempio, se si specifica l'indirizzo IPv4 del NAS come condizione dei criteri di rete e NPS riceve una richiesta di connessione da un NAS con l'indirizzo IP specificato, la condizione nei criteri corrisponde alla richiesta di connessione. 


### <a name="constraints"></a>Vincoli

 I vincoli sono parametri aggiuntivi dei criteri di rete necessari per soddisfare la richiesta di connessione. Se un vincolo non corrisponde alla richiesta di connessione, il server dei criteri di ricerca rifiuterà automaticamente la richiesta. A differenza della risposta server dei criteri di rete a condizioni non corrispondenti nei criteri di rete, se non è presente un vincolo, NPS nega la richiesta di connessione senza valutare altri criteri di rete.

### <a name="settings"></a>Impostazioni

 Queste proprietà consentono di specificare le impostazioni che server dei criteri di rete applica alla richiesta di connessione in caso di corrispondenza di tutte le condizioni dei criteri di rete per il criterio.

Quando si aggiunge un nuovo criterio di rete utilizzando la console server dei criteri di rete, è necessario utilizzare la creazione guidata nuovo criterio di rete. Dopo aver creato un criterio di rete tramite la procedura guidata, è possibile personalizzare il criterio facendo doppio clic sul criterio nella console server dei criteri di rete per ottenere le proprietà dei criteri.

Per esempi di sintassi di corrispondenza dei modelli per specificare gli attributi dei criteri di rete, vedere [usare le espressioni regolari in NPS](nps-crp-reg-expressions.md).

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).
