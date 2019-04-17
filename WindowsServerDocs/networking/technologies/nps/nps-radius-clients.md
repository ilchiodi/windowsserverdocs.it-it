---
title: Client RADIUS
description: In questo argomento viene fornita una panoramica di client RADIUS per Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a970dabcc6f3f4fc4d8ed5a0dedd01b5a7dee71a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="radius-clients"></a>Client RADIUS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Un server di accesso di rete \(NAS\) è un dispositivo che fornisce un certo livello di accesso a una rete più estesa. Un server NAS utilizzando un'infrastruttura RADIUS è anche un client RADIUS, invia le richieste di connessione e messaggi di accounting a un server RADIUS per l'autenticazione, autorizzazione e accounting.

>[!NOTE]
>I computer client, ad esempio computer portatili e altri computer che eseguono sistemi operativi client, non sono client RADIUS. I client RADIUS sono server di accesso di rete - ad esempio punti di accesso wireless, commutatori di autenticazione 802.1 X, rete privata virtuale \(VPN\) Server e server di connessione remota - perché usano il protocollo RADIUS per comunicare con server RADIUS, ad esempio i server \(NPS\) Server dei criteri di rete.

Per distribuire i criteri di rete come server RADIUS o come proxy RADIUS, è necessario configurare client RADIUS in Criteri di rete.

## <a name="radius-client-examples"></a>Esempi di client RADIUS

Esempi di server di accesso alla rete sono:

- Server di accesso di rete che fornisce la connettività di accesso remoto alla rete di un'organizzazione o a Internet. Un esempio è un computer che esegue il sistema operativo Windows Server 2016 e il servizio di accesso remoto che fornisce sia remota tradizionali o servizi di accesso remoto di rete privata virtuale (VPN) a una intranet dell'organizzazione.
- Punti di accesso wireless che forniscono l'accesso fisico alla rete di un'organizzazione utilizzando tecnologie di trasmissione e la ricezione wireless.
- Commutatori che forniscono l'accesso fisico alla rete di un'organizzazione, utilizzando tecnologie LAN tradizionali, ad esempio Ethernet.
- Proxy RADIUS che inoltra le richieste di connessione al server RADIUS che sono membri di un gruppo di server RADIUS remoto configurati sul proxy RADIUS.

## <a name="radius-access-request-messages"></a>Messaggi di richiesta di accesso RADIUS

Client RADIUS creare messaggi di richiesta di accesso RADIUS e inoltrarli a un proxy RADIUS o di un server RADIUS oppure inoltrano messaggi di richiesta di accesso a un server RADIUS che hanno ricevuto da un altro client RADIUS, ma non hanno creato.

Client RADIUS non elaborano i messaggi di richiesta di accesso per eseguire autenticazione, autorizzazione e accounting. Queste funzioni solo i server RADIUS.

Criteri di rete, tuttavia, possono essere configurati come proxy RADIUS e un server RADIUS contemporaneamente, in modo che elabora alcuni messaggi di richiesta di accesso e inoltra altri messaggi.

## <a name="nps-as-a-radius-client"></a>Criteri di rete come client RADIUS

Criteri di rete funge da client RADIUS è configurato come proxy RADIUS per l'inoltro dei messaggi di richiesta di accesso ad altri server RADIUS per l'elaborazione. Quando si utilizza criteri di rete come proxy RADIUS, sono necessari i seguenti passaggi di configurazione generale:

1. Server di accesso di rete, ad esempio punti di accesso wireless e server VPN, vengono configurati con l'indirizzo IP del proxy di criteri di rete come server RADIUS designato o server di autenticazione. In questo modo i server di accesso di rete, quale creano messaggi di richiesta di accesso in base alle informazioni ricevuti dai client di accesso, per l'inoltro dei messaggi al proxy di criteri di rete.

2. Il proxy di criteri di rete viene configurato tramite l'aggiunta di ogni server di accesso di rete come client RADIUS. Questo passaggio di configurazione consente il proxy di criteri di rete per ricevere i messaggi dal server di accesso di rete e di comunicare con essi in tutta l'autenticazione. Inoltre, criteri di richiesta di connessione sul proxy di criteri di rete sono configurati per specificare i messaggi di richiesta di accesso da inoltrare a uno o più server RADIUS. Questi criteri sono configurati anche con un gruppo di server RADIUS remoto, che indica dove inviare i messaggi ricevuti dal server di accesso di rete di criteri di rete.

3. I criteri di rete o da altri server RADIUS che sono membri del gruppo di server RADIUS remoto sul proxy di criteri di rete sono configurati per ricevere i messaggi dal proxy dei criteri di rete. Questa operazione viene eseguita tramite la configurazione del proxy dei criteri di rete come client RADIUS.

## <a name="radius-client-properties"></a>Proprietà di client RADIUS

Quando si aggiunge un client RADIUS per la configurazione dei criteri di rete tramite la console dei criteri di rete o tramite i comandi di Windows PowerShell o i comandi netsh per criteri di rete, la configurazione dei criteri di rete per ricevere i messaggi di richiesta di accesso RADIUS da un server di accesso di rete o un proxy RADIUS.

Quando si configura un client RADIUS in Criteri di rete, è possibile designare le proprietà seguenti:

### <a name="client-name"></a>Nome del client

 Un nome descrittivo per il client RADIUS, che rende più semplice identificare quando si utilizza i NPS snap-in o i comandi netsh per criteri di rete.

### <a name="ip-address"></a>Indirizzo IP

L'indirizzo di protocollo Internet versione 4 \(IPv4\) o il nome Domain Name System \(DNS\) del client RADIUS.

### <a name="client-vendor"></a>Client-fornitore

Il fornitore del client RADIUS. In caso contrario, è possibile utilizzare il valore del raggio standard per il fornitore di Client.

### <a name="shared-secret"></a>Segreto condiviso

Una stringa di testo che viene utilizzata come password tra i client RADIUS e server RADIUS, proxy RADIUS. Quando si utilizza l'attributo autenticatore messaggio, il segreto condiviso viene utilizzato anche come chiave per crittografare i messaggi RADIUS. Questa stringa deve essere configurata nel client RADIUS e nello snap-in Criteri di rete.

### <a name="message-authenticator-attribute"></a>Attributo autenticatore messaggio

Descritto nella RFC 2869, "RADIUS estensioni", un hash Message Digest 5 \(MD5\) dell'intero messaggio RADIUS. Se l'attributo autenticatore del messaggio è presente, viene verificato. In caso contrario la verifica, il messaggio RADIUS viene scartato. Se le impostazioni client richiedono l'attributo autenticatore messaggio e non è presente, il messaggio RADIUS viene scartato. È consigliabile utilizzare l'attributo autenticatore messaggio.

>[!NOTE]
>L'attributo autenticatore messaggio è obbligatorio e abilitato per impostazione predefinita quando si utilizza l'autenticazione Extensible Authentication Protocol \(EAP\). 

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).

