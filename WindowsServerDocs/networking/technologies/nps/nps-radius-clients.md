---
title: Client RADIUS
description: In questo argomento viene fornita una panoramica di client RADIUS per il Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fdca45237d971c1b2e08443112b63d3ce77e4a2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874312"
---
# <a name="radius-clients"></a>Client RADIUS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Un server di accesso di rete \(NAS\) è un dispositivo che fornisce un livello di accesso a una rete più estesa. Un server NAS usando un'infrastruttura RADIUS è anche un client RADIUS, l'invio di messaggi di accounting e le richieste di connessione a un server RADIUS per autenticazione, autorizzazione e accounting.

>[!NOTE]
>I computer client, ad esempio computer portatili e altri computer che eseguono sistemi operativi client, non sono client RADIUS. I client RADIUS sono server di accesso di rete - ad esempio punti di accesso wireless, commutatori di autenticazione 802.1X, rete privata virtuale \(VPN\) server e dei server Accesso remoto - perché usano il protocollo RADIUS per comunicare con RADIUS i server come Server dei criteri di rete \(NPS\) server.

Per distribuire criteri di rete come server RADIUS o un proxy RADIUS, è necessario configurare i client RADIUS in Criteri di rete.

## <a name="radius-client-examples"></a>Esempi di client RADIUS

Esempi di server di accesso alla rete sono:

- Server di accesso di rete che forniscono la connettività di accesso remoto alla rete di un'organizzazione o a Internet. Un esempio è un computer che esegue il sistema operativo Windows Server 2016 e il servizio di accesso remoto che fornisce una tradizionale remoto o servizi di accesso remoto di rete privata virtuale (VPN) da una intranet dell'organizzazione.
- Access point wireless che forniscono accesso fisico a una rete dell'organizzazione usando tecnologie di trasmissione e ricezione senza fili.
- Commutatori che forniscono accesso fisico alla rete dell'organizzazione, che usa tecnologie LAN tradizionali, ad esempio Ethernet.
- Proxy RADIUS che inoltra le richieste di connessione al server RADIUS che sono membri di un gruppo di server RADIUS remoto configurato per il proxy RADIUS.

## <a name="radius-access-request-messages"></a>Messaggi di richiesta di accesso RADIUS

I client RADIUS creano messaggi di richiesta di accesso RADIUS e inoltrano a un server RADIUS o il proxy RADIUS, oppure eseguono l'inoltro dei messaggi di richiesta di accesso a un server RADIUS che sono stati ricevuti da un altro client RADIUS, ma non hanno creato.

I client RADIUS non elaborano i messaggi di richiesta di accesso per eseguire autenticazione, autorizzazione e accounting. Solo i server RADIUS eseguono queste funzioni.

Criteri di rete, tuttavia, possono essere configurate come proxy RADIUS e un server RADIUS contemporaneamente, in modo che lo elabora alcuni messaggi di richiesta di accesso e lo inoltra gli altri messaggi.

## <a name="nps-as-a-radius-client"></a>NPS come client RADIUS

NPS agisce come client RADIUS è configurato come proxy RADIUS per inoltrare i messaggi di richiesta di accesso ad altri server RADIUS per l'elaborazione. Quando si usa criteri di rete come proxy RADIUS, sono necessari i passaggi di configurazione generali seguenti:

1. I server di accesso di rete, ad esempio punti di accesso wireless e server VPN, vengono configurati con l'indirizzo IP del proxy come il server RADIUS designato o il server che esegue l'autenticazione dei criteri di rete. In questo modo i server di accesso di rete, quale creano messaggi di richiesta di accesso in base alle informazioni ricevute dal client di accesso, inoltrare messaggi al proxy di criteri di rete.

2. Il proxy di criteri di rete è configurato, aggiungere ogni server di accesso di rete come client RADIUS. Questo passaggio di configurazione consente il proxy NPS per ricevere messaggi dai server di accesso di rete e di comunicare con essi in tutta l'autenticazione. Inoltre, criteri di richiesta di connessione del proxy di criteri di rete sono configurati per specificare quali messaggi di richiesta di accesso per l'inoltro a uno o più server RADIUS. Questi criteri sono configurati anche con un gruppo di server RADIUS remoto, che indica dove inviare i messaggi ricevuti dai server di accesso di rete dei criteri di rete.

3. I criteri di rete o da altri server RADIUS che sono membri del gruppo di server RADIUS remoto nel proxy dei criteri di rete sono configurati per ricevere messaggi dal proxy di criteri di rete. Questa operazione viene eseguita tramite la configurazione del proxy di criteri di rete come client RADIUS.

## <a name="radius-client-properties"></a>Proprietà di client RADIUS

Quando si aggiunge un client RADIUS per la configurazione dei criteri di rete tramite la console Criteri di rete oppure tramite i comandi netsh per criteri di rete o i comandi di Windows PowerShell, che si sta configurando Criteri di rete per ricevere messaggi di richiesta di accesso RADIUS da un server di accesso rete o un oggetto Proxy RADIUS.

Quando si configura un client RADIUS in Criteri di rete, è possibile definire le proprietà seguenti:

### <a name="client-name"></a>Nome del client

 Nome descrittivo per il client RADIUS, rendendo più semplice identificare quando si usano i comandi di snap-in o netsh NPS per criteri di rete.

### <a name="ip-address"></a>L'indirizzo IP

Il protocollo Internet versione 4 \(IPv4\) indirizzo o Domain Name System \(DNS\) nome del client RADIUS.

### <a name="client-vendor"></a>Client-Vendor

Il fornitore della libreria del client RADIUS. In caso contrario, è possibile utilizzare il valore standard RADIUS per il fornitore di Client.

### <a name="shared-secret"></a>Segreto condiviso

Una stringa di testo che viene usata come una password tra i client RADIUS, i server RADIUS e i proxy RADIUS. Quando viene usato l'attributo autenticatore messaggio, il segreto condiviso viene anche utilizzato come chiave per crittografare i messaggi RADIUS. Questa stringa deve essere configurata nel client RADIUS e nello snap-in NPS.

### <a name="message-authenticator-attribute"></a>Attributo autenticatore messaggio

Descritto in RFC 2869 "Estensioni" RADIUS", una Message Digest 5 \(MD5\) hash dell'intero messaggio RADIUS. Se è presente l'attributo autenticatore del messaggio, viene verificato. In caso di errore verifica, viene eliminato il messaggio RADIUS. Se le impostazioni client richiedono l'attributo autenticatore messaggio e non è presente, viene eliminato il messaggio RADIUS. È consigliabile usare l'attributo autenticatore messaggio.

>[!NOTE]
>L'attributo autenticatore messaggio è necessaria e abilitato per impostazione predefinita quando si utilizza Extensible Authentication Protocol \(EAP\) l'autenticazione. 

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).

