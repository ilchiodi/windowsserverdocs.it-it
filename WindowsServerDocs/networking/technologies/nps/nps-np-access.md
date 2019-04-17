---
title: Autorizzazione di accesso
description: In questo argomento viene fornita una panoramica dell'autorizzazione di accesso di criteri di rete per Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d6d1ca5e-bde0-4509-9e14-dc3fa9ff447e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: aeacfaeb159598d2e53bac29fb09ffc3e7739476
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="access-permission"></a>Autorizzazione di accesso

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Autorizzazione di accesso viene configurata sul **Panoramica** scheda ogni dei criteri di rete in Server dei criteri di rete (NPS). 

Questa impostazione consente di configurare i criteri per concedere o negare l'accesso agli utenti se sono soddisfatte le condizioni e i vincoli di criteri di rete per la richiesta di connessione. 

Impostazioni autorizzazione di accesso effetti:

- **Concedere l'accesso**. Accesso viene concesso se la richiesta di connessione corrispondente le condizioni e vincoli configurati nei criteri.
- **Negare l'accesso**. Se la richiesta di connessione soddisfa le condizioni e vincoli configurati nei criteri di accesso negato.

Autorizzazione di accesso concesso o negato anche in base alla configurazione delle proprietà dial-in di ogni account utente.

>[!NOTE]
>Gli account utente e le relative proprietà, ad esempio delle proprietà di connessione remota, sono configurate in uno dei due utenti di Active Directory e i computer o il utenti locali e gruppi di Microsoft Management Console \(MMC\) snap-in, a seconda che tu abbia Active Directory&reg; servizi di dominio (AD DS) installato.

L'impostazione dell'account utente **autorizzazione di accesso alla rete**, che è configurato nelle proprietà dial-in di account utente, sostituzioni impostazione di autorizzazione di accesso i criteri di rete. Quando un account utente di autorizzazione di accesso rete è impostato sul **controllare l'accesso tramite criteri di rete NPS** opzione, l'impostazione di autorizzazione accesso criteri di rete determina se l'utente viene concesso o negato l'accesso.

>[!NOTE]
>In Windows Server 2016, il valore predefinito di **autorizzazione di accesso alla rete** in servizi di dominio Active Directory dial-in di proprietà dell'account utente è **controllare l'accesso tramite criteri di rete NPS**.

Quando Criteri di rete valuta le richieste di connessione da criteri di rete configurate, esegue le azioni seguenti:

- Se le condizioni del primo criterio non corrispondono, dei criteri di rete valuta i criteri avanti e continua questo processo fino a quando non viene trovata una corrispondenza o tutti i criteri sono stati valutati per una corrispondenza.
- Se sono soddisfatte le condizioni e limitazioni di un criterio, dei criteri di rete concede o nega l'accesso, a seconda del valore dell'impostazione nel criterio di autorizzazione di accesso.
- Se le condizioni di un criterio corrispondente, ma i vincoli nei criteri non corrispondono, dei criteri di rete rifiuta la richiesta di connessione.
- Se le condizioni di tutti i criteri non corrispondono, dei criteri di rete rifiuta la richiesta di connessione.

## <a name="ignore-user-account-dial-in-properties"></a>Ignorare dial-in di proprietà dell'account utente

È possibile configurare criteri di rete dei criteri di rete per ignorare la proprietà di connessione remota degli account utente selezionando o deselezionando la **dial-in di proprietà dell'account utente di ignorare** casella di controllo di **Panoramica** scheda di un criterio di rete. 

In genere quando criteri di rete esegue l'autorizzazione di una richiesta di connessione, controlla la proprietà di connessione remota dell'account utente, in cui l'autorizzazione di accesso di rete impostazione valore può influire sulla se l'utente è autorizzato a connettersi alla rete. Quando si configura criteri di rete per ignorare la proprietà di connessione remota degli account utente durante l'autorizzazione, le impostazioni di criteri di rete è possibile determinare se l'utente viene concesso l'accesso alla rete.

La proprietà di connessione remota degli account utente contenere quanto segue:

- Autorizzazione di accesso alla rete
- ID chiamante
- Opzioni di richiamata
- Indirizzo IP statico
- Route statiche

Per supportare più tipi di connessioni per il quale dei criteri di rete fornisce l'autenticazione e autorizzazione, potrebbe essere necessario disabilitare l'elaborazione delle proprietà di connessione account utente. Questa operazione può essere eseguita per supportare scenari in cui non sono necessari specifiche proprietà di connessione remota.

Ad esempio, l'ID chiamante, callback, indirizzo IP statico e route statiche proprietà sono progettate per un client che si connette a un server di accesso di rete \(NAS\), non per i client che si connettono all'accesso wireless punti. Un punto di accesso wireless che riceve queste impostazioni in un messaggio RADIUS da server dei criteri potrebbe non essere in grado di elaborare, che può provocare il client wireless deve essere disconnessa.

Quando Criteri di rete fornisce l'autenticazione e autorizzazione per gli utenti con accesso remoto e l'accesso di rete dell'organizzazione tramite punti di accesso wireless, è necessario configurare le proprietà di connessione remota per il supporto di entrambe le connessioni in ingresso \ (per impostazione chiamate in ingresso properties\) o connessioni wireless \ (impostando non properties\ dial-in).

È possibile utilizzare criteri di rete per abilitare la proprietà di connessione remota per l'account utente in alcuni scenari di elaborazione \ (ad esempio connessioni attive) e disabilitare una proprietà di connessione remota in altri scenari di elaborazione \ (ad esempio 802.1 X wireless e di autenticazione switch\).

È inoltre possibile utilizzare **dial-in di proprietà dell'account utente di ignorare** per gestire il controllo di accesso di rete tramite gruppi e le autorizzazioni di accesso nei criteri di rete. Quando si seleziona il **dial-in di proprietà dell'account utente di ignorare** casella di controllo autorizzazione di accesso di rete per l'account utente viene ignorato.

L'unico svantaggio di questa configurazione è che non è possibile utilizzare altre proprietà dell'account utente dial-in di ID chiamante, callback, indirizzo IP statico e route statiche.

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).
