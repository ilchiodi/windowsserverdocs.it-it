---
title: Bilanciamento del carico Server Proxy dei criteri di rete
description: È possibile utilizzare questo argomento per informazioni sulle caratteristiche e funzionalità VPN di Windows 10 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9f00eb6575284a69358c58b36d08672cdd69dc18
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="nps-proxy-server-load-balancing"></a>Bilanciamento del carico Server Proxy dei criteri di rete

Si applica a: Windows Server 2016

Remote Authentication Dial-In servizio client RADIUS (User), che sono server di accesso di rete come server di rete privata virtuale (VPN) e punti di accesso wireless, creare le richieste di connessione e inviarli a server RADIUS, ad esempio criteri di rete. In alcuni casi, un server dei criteri di rete potrebbe ricevere un numero eccessivo di richieste di connessione in una sola volta, conseguente riduzione delle prestazioni o un overload. Quando un server dei criteri di rete è in overload, è consigliabile aggiungere più server dei criteri di rete alla rete e per configurare il bilanciamento del carico. Quando si distribuiscono uniformemente le richieste di connessione in ingresso tra più server dei criteri di rete per impedire l'overload di uno o più server dei criteri di rete, viene chiamato il bilanciamento del carico.

Il bilanciamento del carico è particolarmente utile per:

- Le organizzazioni che usano \(EAP-TLS\) Extensible Authentication Protocol-Transport Layer Security o Protected Extensible Authentication Protocol \ (PEAP\)-TLS per l'autenticazione. Poiché questi metodi di autenticazione utilizzano i certificati per l'autenticazione server e per utente o autenticazione del computer client, il carico sul server e proxy RADIUS è maggiore quando si usano i metodi di autenticazione basata su password.
- Organizzazioni che necessitano di sostenere disponibilità continua del servizio.
- \(ISPs\) provider del servizio Internet che forniscono l'accesso VPN per altre organizzazioni. I servizi VPN in outsourcing possono generare un grande volume di traffico di autenticazione.

Esistono due metodi che è possibile utilizzare per bilanciare il carico delle richieste di connessione inviate ai server dei criteri di rete:

- Configurare i server di accesso di rete per inviare le richieste di connessione a più server RADIUS. Ad esempio, se si dispone di 20 punti di accesso wireless e due server RADIUS, configurare ogni punto di accesso per inviare le richieste di connessione a entrambi i server RADIUS. È possibile il bilanciamento del carico e fornire un failover in ogni server di accesso di rete configurando il server di accesso per inviare le richieste di connessione a più server RADIUS in un ordine di priorità specificato. Questo metodo di bilanciamento del carico è in genere consigliabile per le organizzazioni di piccole dimensioni che non distribuiscono un numero elevato di client RADIUS.
- Utilizzare criteri di rete configurato come proxy RADIUS per inoltrare le richieste di connessione tra più server dei criteri di rete o da altri server RADIUS. Ad esempio, se si dispone di 100 punti di accesso wireless, un proxy dei criteri di rete e tre i server RADIUS, è possibile configurare i punti di accesso per l'invio di tutto il traffico per il proxy di criteri di rete. Sul proxy di criteri di rete, configurare il bilanciamento del carico in modo che il proxy distribuisce uniformemente le richieste di connessione tra i tre server RADIUS. Questo metodo di bilanciamento del carico è ideale per le organizzazioni di medie e grandi dimensioni che dispongono di molti client e server RADIUS.

In molti casi, l'approccio migliore per il bilanciamento del carico consiste nel configurare client RADIUS per inviare le richieste di connessione a due server proxy dei criteri di rete e quindi configurare il proxy di criteri di rete per bilanciare il carico tra i server RADIUS. Questo approccio fornisce il failover e bilanciamento del carico per il proxy di criteri di rete e server RADIUS.

## <a name="radius-server-priority-and-weight"></a>Priorità di server RADIUS e il peso

Durante il processo di configurazione proxy dei criteri di rete, è possibile creare gruppi di server RADIUS remoti e quindi aggiungere i server RADIUS per ogni gruppo. Per configurare il bilanciamento del carico, è necessario disporre di più di un server RADIUS per ogni gruppo di server RADIUS remoti. Durante l'aggiunta di membri del gruppo o dopo la creazione di un server RADIUS come un membro del gruppo, è possibile accedere la finestra di dialogo Aggiungi RADIUS server per configurare gli elementi seguenti nella scheda bilanciamento del carico:

- **Priorità**. Priorità specifica l'ordine di priorità del server RADIUS per il server proxy dei criteri di rete. Livello di priorità deve essere assegnato un valore che è un numero intero, ad esempio 1, 2 o 3. Tanto minore è il numero, la priorità più alta il proxy di criteri di rete offre al server RADIUS. Ad esempio, se il server RADIUS viene assegnato la priorità più alta di 1, il proxy di criteri di rete invia le richieste di connessione al server RADIUS per primo. Se non sono disponibili server con priorità 1, dei criteri di rete invia le richieste di connessione al server RADIUS con priorità 2 e così via. Puoi assegnare la stessa priorità a più server RADIUS e quindi utilizzare l'impostazione del peso per bilanciare il carico tra loro.

- **Peso**. Usa Criteri di rete questa impostazione di peso per determinare quanti connessione le richieste di inviare a ciascun gruppo membro quando i membri del gruppo hanno lo stesso livello di priorità. Impostazione del peso deve essere assegnato un valore compreso tra 1 e 100 e il valore rappresenta una percentuale del 100%. Ad esempio, se il gruppo di server RADIUS remoto contiene due membri che dispongono di un livello di priorità pari a 1 e una classificazione peso pari a 50, il proxy di criteri di rete inoltra al 50% delle richieste di connessione a ogni server RADIUS.

- **Impostazioni avanzate**. Queste impostazioni di failover forniscono un modo per criteri di rete determinare se il server RADIUS remoto non è disponibile. Se Criteri di rete determina che un server RADIUS non è disponibile, è possibile avviare l'invio di richieste di connessione a altri membri del gruppo. Con queste impostazioni è possibile configurare il numero di secondi che il proxy di criteri di rete attende una risposta dal server RADIUS prima considera la richiesta ICMP. il numero massimo di richieste annullate prima il proxy di criteri di rete identifica il server RADIUS come non disponibile. e il numero di secondi che possono trascorrere tra le richieste prima il proxy di criteri di rete identifica il server RADIUS come non disponibile.

## <a name="configure-nps-proxy-load-balancing"></a>Configurare il bilanciamento del carico proxy di criteri di rete

Prima di configurare il bilanciamento del carico, creare un piano di distribuzione che include il numero RADIUS gruppi di server remoti sono necessari, i server sono membri del gruppo e l'impostazione di priorità e peso per ogni server.

>[!NOTE]
>I passaggi seguenti presuppongono che hanno già distribuito e configurato il server RADIUS.

Per configurare criteri di rete per agire come un server proxy e le richieste di connessione diretta dai client RADIUS in server RADIUS remoti, è necessario eseguire le operazioni seguenti:

1. Distribuire i client RADIUS \ (server VPN, server di connessione remota, il server Gateway Servizi Terminal, commutatori di autenticazione 802.1 X e 802.1 X wireless access points\) e configurarli per inviare le richieste di connessione per i server proxy dei criteri di rete.

2. Sul proxy di criteri di rete, configurare i server di accesso di rete come client RADIUS. Per ulteriori informazioni, vedere [configurare client RADIUS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure).

3. Sul proxy di criteri di rete, creare uno o più gruppi di server RADIUS remoti. Durante questo processo, aggiungere i server RADIUS per i gruppi di server RADIUS remoti. Per ulteriori informazioni, vedere [configurare gruppi di Server RADIUS remoti](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure).

4. Il proxy dei criteri di rete, per ogni server RADIUS aggiunti a un gruppo di server RADIUS remoto, fare clic sul server RADIUS **il bilanciamento del carico** scheda e quindi configurare **priorità**, **peso**, e **impostazioni avanzate**.

5. Sul proxy di criteri di rete, configurare criteri di richiesta di connessione per l'inoltro di richieste di autenticazione e accounting per gruppi di server RADIUS remoti. È necessario creare un criterio di richiesta di connessione per ogni gruppo di server RADIUS remoti. Per ulteriori informazioni, vedere [configurare criteri di richiesta di connessione](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure).


