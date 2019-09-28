---
title: Autorizzazione di accesso
description: Questo argomento fornisce una panoramica delle autorizzazioni di accesso ai criteri di rete per server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d6d1ca5e-bde0-4509-9e14-dc3fa9ff447e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8a63bf7d277108a528a3ab1c20263e31b59c1547
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405360"
---
# <a name="access-permission"></a>Autorizzazione di accesso

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

L'autorizzazione di accesso viene configurata nella scheda **Panoramica** di ogni criterio di rete in server dei criteri di rete. 

Questa impostazione consente di configurare i criteri per concedere o negare l'accesso agli utenti se le condizioni e i vincoli dei criteri di rete corrispondono alla richiesta di connessione. 

Le impostazioni delle autorizzazioni di accesso hanno l'effetto seguente:

- **Concessione dell'accesso**. L'accesso viene concesso se la richiesta di connessione soddisfa le condizioni e i vincoli configurati nei criteri.
- **Nega accesso**. L'accesso viene negato se la richiesta di connessione soddisfa le condizioni e i vincoli configurati nei criteri.

Viene inoltre concessa o negata l'autorizzazione di accesso in base alla configurazione delle proprietà di connessione remota di ogni account utente.

>[!NOTE]
>Gli account utente e le relative proprietà, ad esempio le proprietà di connessione, vengono configurati in Active Directory utenti e computer oppure nello snap-in utenti e gruppi locali di Microsoft Management Console \(MMC @ no__t-1, a seconda che sia attivo o meno Directory @ no__t-2 Domain Services (AD DS) installata.

L'impostazione dell'account utente **autorizzazione di accesso alla rete**, che è configurata nelle proprietà di connessione degli account utente, sostituisce l'impostazione di autorizzazione di accesso ai criteri di rete. Quando l'autorizzazione di accesso alla rete per un account utente è impostata sull'opzione **Controlla l'accesso tramite criteri di rete NPS** , l'impostazione delle autorizzazioni di accesso ai criteri di rete determina se all'utente viene concesso o negato l'accesso.

>[!NOTE]
>In Windows Server 2016, il valore predefinito delle **autorizzazioni di accesso alla rete** nelle proprietà di connessione remota dell'account utente di servizi di dominio Active Directory è **Controlla l'accesso tramite criteri di rete NPS**.

Quando NPS valuta le richieste di connessione in base ai criteri di rete configurati, esegue le azioni seguenti:

- Se non vengono soddisfatte le condizioni del primo criterio, NPS valuta i criteri successivi e continua il processo fino a quando non viene trovata una corrispondenza o tutti i criteri sono stati valutati per una corrispondenza.
- Se vengono soddisfatte le condizioni e i vincoli di un criterio, NPS concede o nega l'accesso, a seconda del valore dell'impostazione delle autorizzazioni di accesso nei criteri.
- Se le condizioni di un criterio corrispondono ma i vincoli nei criteri non corrispondono, NPS rifiuta la richiesta di connessione.
- Se le condizioni di tutti i criteri non corrispondono, NPS rifiuterà la richiesta di connessione.

## <a name="ignore-user-account-dial-in-properties"></a>Ignorare le proprietà di connessione remota dell'account utente

È possibile configurare i criteri di rete NPS per ignorare le proprietà di accesso remoto degli account utente selezionando o deselezionando la casella di controllo **Ignora proprietà di connessione dell'account utente** nella scheda **Panoramica** di un criterio di rete. 

In genere, quando server dei criteri di rete esegue l'autorizzazione di una richiesta di connessione, verifica le proprietà del dial-in dell'account utente, in cui il valore dell'impostazione delle autorizzazioni di accesso alla rete può influire sul fatto che l'utente sia autorizzato a connettersi alla rete. Quando si configura il server dei criteri di rete per ignorare le proprietà di connessione degli account utente durante l'autorizzazione, le impostazioni dei criteri di rete determinano se l'utente è autorizzato ad accedere alla rete.

Le proprietà di connessione degli account utente contengono quanto segue:

- Autorizzazione di accesso alla rete
- ID chiamante
- Opzioni di callback
- Indirizzo IP statico
- Route statiche

Per supportare più tipi di connessioni per le quali NPS fornisce l'autenticazione e l'autorizzazione, potrebbe essere necessario disabilitare l'elaborazione delle proprietà di connessione remota dell'account utente. Questa operazione può essere eseguita per supportare scenari in cui non sono necessarie proprietà remote specifiche.

Ad esempio, le proprietà ID chiamante, callback, indirizzo IP statico e route statiche sono progettate per un client che esegue la connessione a un server di accesso alla rete \(NAS @ no__t-1, non per i client che si connettono ai punti di accesso wireless. Un punto di accesso wireless che riceve queste impostazioni in un messaggio RADIUS da server dei criteri di rete potrebbe non essere in grado di elaborarle, il che può causare la disconnessione del client wireless.

Quando il server dei criteri di rete fornisce l'autenticazione e l'autorizzazione per gli utenti che eseguono la connessione e accedono alla rete aziendale tramite punti di accesso wireless, è necessario configurare le proprietà di connessione in modo che supportino le connessioni remote @no__t impostazione di 0BY Proprietà di connessione @ no__t-1 o connessioni wireless \(da non imposta le proprietà della connessione remota @ no__t-3.

È possibile utilizzare server dei criteri di rete per abilitare l'elaborazione delle proprietà di connessione per l'account utente in alcuni scenari \(such come dial-in @ no__t-1 e per disabilitare l'elaborazione delle proprietà Remote in altri scenari \(Such come 802.1 X wireless e l'opzione di autenticazione @ no__t-3.

Per gestire il controllo di accesso alla rete tramite i gruppi e l'impostazione delle autorizzazioni di accesso per i criteri di rete, è anche possibile usare le **proprietà della connessione remota dell'account utente** . Quando si seleziona la casella **di controllo Ignora proprietà della connessione all'account utente** , l'autorizzazione di accesso alla rete per l'account utente viene ignorata.

L'unico svantaggio di questa configurazione è che non è possibile usare le proprietà di connessione remota dell'account utente aggiuntive di ID chiamante, callback, indirizzo IP statico e route statiche.

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).
