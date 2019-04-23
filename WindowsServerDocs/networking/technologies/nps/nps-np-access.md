---
title: Autorizzazione di accesso
description: In questo argomento viene fornita una panoramica di rete dei criteri di autorizzazione di accesso per il Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d6d1ca5e-bde0-4509-9e14-dc3fa9ff447e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cdec41fb7925061bb8c8402634e1d9b1625bf301
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849492"
---
# <a name="access-permission"></a>Autorizzazione di accesso

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

L'autorizzazione di accesso è configurato sul **Panoramica** scheda ogni dei criteri di rete in Strumentazione gestione Windows (NPS, Network Policy Server). 

Questa impostazione consente di configurare i criteri per concedere o negare l'accesso agli utenti se vengono soddisfatte le condizioni e vincoli dei criteri di rete per la richiesta di connessione. 

Impostazioni delle autorizzazioni di accesso hanno l'effetto seguente:

- **Concedere l'accesso**. L'accesso viene concesso se la richiesta di connessione soddisfa le condizioni e vincoli che sono configurati nel criterio.
- **Negare l'accesso**. Se la richiesta di connessione soddisfa le condizioni e vincoli che sono configurati nel criterio di accesso negato.

L'autorizzazione di accesso viene anche concesso o negato in base alla configurazione delle proprietà dial-in di ogni account utente.

>[!NOTE]
>Gli account utente e le relative proprietà, ad esempio proprietà di connessione remota, sono configurati in Active Directory Users e computer o di utenti locali e i gruppi di Microsoft Management Console \(MMC\) snap-in, a seconda del Active Directory&reg; Domain Services (AD DS) installato.

L'impostazione di account utente **l'autorizzazione di accesso rete**, cui viene configurato nelle proprietà dial-degli account utente, gli override dell'impostazione delle autorizzazioni di accesso i criteri di rete. Quando è impostata l'autorizzazione di accesso di rete in un account utente il **controllare l'accesso tramite criteri di rete NPS** opzione, l'impostazione di autorizzazione l'accesso di rete dei criteri determina se l'utente viene concesso o negato l'accesso.

>[!NOTE]
>In Windows Server 2016, il valore predefinito **l'autorizzazione di accesso di rete** in Active Directory Domain Services dial-in di proprietà dell'account utente viene **controllare l'accesso tramite criteri di rete NPS**.

Quando Criteri di rete valuta le richieste di connessione in base ai criteri di rete configurate, esegue le azioni seguenti:

- Se non sono soddisfatte le condizioni dei criteri prima, dei criteri di rete valuta i criteri successivo e continua questo processo finché non viene trovata una corrispondenza o tutti i criteri sono stati valutati per trovare una corrispondenza.
- Se sono soddisfatte le condizioni e i vincoli di un criterio, NPS concede o nega l'accesso, a seconda del valore dell'impostazione di autorizzazione di accesso nei criteri.
- Se non corrispondono alle condizioni di una corrispondenza di criteri, ma i vincoli nel criterio, NPS rifiuta la richiesta di connessione.
- Se non corrispondono alle condizioni di tutti i criteri, criteri di rete rifiuta la richiesta di connessione.

## <a name="ignore-user-account-dial-in-properties"></a>Ignora dial-in di proprietà dell'account utente

È possibile configurare criteri di rete NPS per ignorare le proprietà di connessione remota degli account utente, selezionando o deselezionando le **dial-in di proprietà dell'account utente di ignorare** casella di controllo la **Panoramica** scheda di rete criteri. 

In genere quando NPS esegue l'autorizzazione di una richiesta di connessione, controlla le proprietà di connessione remota dell'account utente, in cui l'autorizzazione di accesso di rete l'impostazione di valore può influire sulle se l'utente è autorizzato a connettersi alla rete. Quando si configurano criteri di rete per ignorare le proprietà di connessione remota degli account utente durante l'autorizzazione, le impostazioni dei criteri di rete determinano se l'utente viene concesso l'accesso alla rete.

La proprietà di connessione remota degli account utente contenere quanto segue:

- Autorizzazione di accesso di rete
- ID chiamante
- Opzioni di callback
- Indirizzo IP statico
- Route statiche

Per supportare più tipi di connessioni per il quale NPS fornisce autenticazione e autorizzazione, potrebbe essere necessario disabilitare l'elaborazione di chiamate in ingresso proprietà dell'account utente. Questa operazione può essere eseguita per supportare scenari in cui non sono necessarie le proprietà di connessione specifiche.

Ad esempio, l'ID chiamante, il callback, indirizzo IP statico e le proprietà di route statiche sono progettate per un client che effettua chiamate a un server di accesso di rete \(NAS\), non per i client che si connettono ai punti di accesso wireless. Un punto di accesso wireless riceve queste impostazioni in un messaggio RADIUS da NPS potrebbe non essere in grado di elaborarli, che può causare il client senza fili venga considerata disconnessa.

Quando Criteri di rete fornisce autenticazione e autorizzazione per gli utenti accesso remoto e l'accesso di rete dell'organizzazione tramite punti di accesso wireless, è necessario configurare le proprietà di connessione remota per supportare entrambe le connessioni in ingresso \(da impostazione proprietà di connessione remota\) o le connessioni senza fili \(non impostando le proprietà di connessione remota\).

È possibile usare criteri di rete per abilitare la proprietà di connessione remota per l'account utente in alcuni scenari di elaborazione \(ad esempio chiamate in ingresso\) e per disabilitare la proprietà di connessione remota in altri scenari di elaborazione \(ad esempio 802.1x wireless e l'opzione di autenticazione\).

È anche possibile usare **dial-in di proprietà dell'account utente di ignorare** per gestire il controllo di accesso alla rete tramite gruppi e l'impostazione di autorizzazione di accesso nei criteri di rete. Quando si seleziona il **dial-in di proprietà dell'account utente di ignorare** casella di controllo, autorizzazione di accesso di rete per l'account utente viene ignorato.

L'unico svantaggio di questa configurazione è che è possibile utilizzare altre proprietà dell'account utente dial-dell'ID chiamante, il callback, indirizzo IP statico e route statiche.

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).
