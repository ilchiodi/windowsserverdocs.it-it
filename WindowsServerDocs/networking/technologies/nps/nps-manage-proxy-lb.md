---
title: Bilanciamento del carico Server Proxy dei criteri di rete
description: È possibile utilizzare questo argomento per apprendere le funzionalità VPN di Windows 10 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 04f466f7646fc5109af7379cab1240d8cd6829ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874402"
---
# <a name="nps-proxy-server-load-balancing"></a>Bilanciamento del carico Server Proxy dei criteri di rete

Si applica a: Windows Server 2016

Remote Authentication Dial-In utente client RADIUS (Service), che sono server di accesso di rete come server di rete privata virtuale (VPN) e punti di accesso wireless, creare richieste di connessione e inviarli al server RADIUS, ad esempio criteri di rete. In alcuni casi, un NPS potrebbe ricevere un numero eccessivo di richieste di connessione in una sola volta, conseguente riduzione delle prestazioni o da un overload. Quando è sottoposto a overload un NPS, è consigliabile aggiungere altre NPSs alla rete e configurare il bilanciamento del carico. Quando si distribuiscono in modo uniforme le richieste di connessione in ingresso tra più NPSs per impedire il sovraccarico di NPSs uno o più, viene chiamato il bilanciamento del carico.

Il bilanciamento del carico è particolarmente utile per:

- Le organizzazioni che usano Extensible Authentication Protocol-Transport Layer Security \(EAP-TLS\) o Protected Extensible Authentication Protocol \(PEAP\)- TLS per l'autenticazione. Poiché questi metodi di autenticazione usano certificati per l'autenticazione server e per utente o l'autenticazione del computer client, il carico sul server e proxy RADIUS è più pesante rispetto a quando si utilizzano i metodi di autenticazione basato su password.
- Organizzazioni che devono sostenere disponibilità continua del servizio.
- Provider di servizi Internet \(ISP\) che outsourcing accesso VPN per altre organizzazioni. I servizi VPN in outsourcing possono generare un grosso volume di traffico di autenticazione.

Esistono due metodi che è possibile usare per bilanciare il carico delle richieste di connessione inviata per il NPSs:

- Configurare i server di accesso di rete per inviare le richieste di connessione al server RADIUS multipli. Ad esempio, se si dispone di due server RADIUS e 20 punti di accesso wireless, configurare ogni punto di accesso per l'invio di richieste di connessione a entrambi i server RADIUS. È possibile bilanciare il carico e assicurare il failover in ogni server di accesso di rete configurando il server di accesso per inviare le richieste di connessione al server RADIUS multipli in un ordine di priorità specificato. Questo metodo di bilanciamento del carico è in genere consigliabile per le organizzazioni di piccole dimensioni che non distribuiscono un numero elevato di client RADIUS.
- Usare criteri di rete configurato come proxy RADIUS per inoltrare le richieste di connessione tra più NPSs o altri server RADIUS. Ad esempio, se si dispone di 100 punti di accesso wireless, un unico proxy dei criteri di rete e tre i server RADIUS, è possibile configurare i punti di accesso per l'invio di tutto il traffico al proxy di criteri di rete. Sul proxy di criteri di rete, configurare il bilanciamento del carico in modo che il proxy distribuisce uniformemente le richieste di connessione tra i tre server RADIUS. Questo metodo di bilanciamento del carico è ideale per le organizzazioni di medie e grandi dimensioni che hanno molti client e server RADIUS.

In molti casi, l'approccio migliore per il bilanciamento del carico consiste nel configurare i client RADIUS per inviare le richieste di connessione a due server proxy dei criteri di rete e quindi configurare il proxy di criteri di rete per bilanciare il carico tra i server RADIUS. Questo approccio fornisce il failover e bilanciamento del carico per i proxy dei criteri di rete e server RADIUS.

## <a name="radius-server-priority-and-weight"></a>Priorità di server RADIUS e il peso

Durante il processo di configurazione dei criteri di rete proxy, è possibile creare gruppi di server RADIUS remoti e quindi aggiungere i server RADIUS per ogni gruppo. Per configurare il bilanciamento del carico, è necessario disporre di più di un server RADIUS per ogni gruppo di server RADIUS remoto. Durante l'aggiunta di membri del gruppo o dopo la creazione di un server RADIUS come membro del gruppo, è possibile accedere la finestra di dialogo server RADIUS aggiungere per configurare gli elementi seguenti nella scheda il bilanciamento del carico:

- **priorità**. Priorità specifica l'ordine di importanza del server RADIUS al server proxy dei criteri di rete. Livello di priorità deve essere assegnato un valore che è un numero intero, ad esempio 1, 2 o 3. Più basso il numero, la priorità più alta il proxy di criteri di rete offre al server RADIUS. Ad esempio, se il server RADIUS viene assegnato la priorità più alta di 1, il proxy NPS invia le richieste di connessione al server RADIUS per primo. Se non sono disponibili server con priorità 1, NPS invia le richieste di connessione al server RADIUS con priorità 2 e così via. È possibile assegnare la stessa priorità a più server RADIUS e quindi usare l'impostazione del peso per bilanciare il carico tra loro.

- **Peso**. Utilizza criteri di rete questa impostazione del peso per determinare quanti connessione richieste per inviare a ciascun gruppo membro quando i membri del gruppo hanno lo stesso livello di priorità. Impostazione del peso deve essere assegnato un valore compreso tra 1 e 100 e il valore rappresenta una percentuale pari al 100 percento. Ad esempio, se il gruppo di server RADIUS remoto contiene due membri che hanno un livello di priorità pari a 1 e una classificazione di peso di 50, il proxy di criteri di rete inoltra il 50% delle richieste di connessione per ogni server RADIUS.

- **Impostazioni avanzate**. Queste impostazioni di failover offrono un modo per criteri di rete determinare se il server RADIUS remoto non è disponibile. Se Criteri di rete determina che non è disponibile un server RADIUS, può iniziare a inviare le richieste di connessione ad altri membri del gruppo. Con queste impostazioni è possibile configurare il numero di secondi che il proxy NPS attende una risposta dal server RADIUS prima che venga stabilita la richiesta ICMP. il numero massimo di richieste annullate prima il proxy NPS identifica il server RADIUS come non disponibili; e il numero di secondi che può trascorrere tra le richieste prima il proxy NPS che identifica il server RADIUS come non disponibile.

## <a name="configure-nps-proxy-load-balancing"></a>Configurare il bilanciamento del carico proxy di criteri di rete

Prima di configurare il bilanciamento del carico, creare un piano di distribuzione che include quanti RADIUS gruppi di server remoti sono necessari, i server che sono membri di ogni gruppo specifico e l'impostazione di parametri Priority e Weight per ogni server.

>[!NOTE]
>La procedura seguente si supponga di avere già distribuito e configurato i server RADIUS.

Per configurare criteri di rete per agire come un server proxy e le richieste di connessione diretta da client RADIUS per i server RADIUS remoti, è necessario eseguire le azioni seguenti:

1. Distribuire i client RADIUS \(server VPN, server remoto, i server Gateway Servizi Terminal, commutatori di autenticazione 802.1X e 802.1 X wireless access point\) e configurarli per l'invio di richieste di connessione per il proxy di criteri di rete Server.

2. Sul proxy di criteri di rete, configurare i server di accesso di rete come client RADIUS. Per altre informazioni, vedere [configurare i client RADIUS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure).

3. Sul proxy di criteri di rete, creare uno o più gruppi di server RADIUS remoti. Durante questo processo, aggiungere i server RADIUS per i gruppi di server RADIUS remoti. Per altre informazioni, vedere [configurare gruppi di Server RADIUS remoti](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure).

4. Sul proxy e dei criteri di rete per ogni server RADIUS che aggiungono a un gruppo di server RADIUS remoti, fare clic su server RADIUS **bilanciamento del carico** scheda e quindi configurare **priorità**, **peso** , e **impostazioni avanzate**.

5. Sul proxy di criteri di rete, configurare criteri di richiesta di connessione per l'inoltro delle richieste di autenticazione e accounting per gruppi di server RADIUS remoti. È necessario creare un criterio di richiesta di connessione per ogni gruppo server RADIUS remoti. Per altre informazioni, vedere [configurare criteri di richiesta di connessione](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure).


