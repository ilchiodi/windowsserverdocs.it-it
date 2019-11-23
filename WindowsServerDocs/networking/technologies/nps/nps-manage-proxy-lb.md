---
title: Bilanciamento del carico del server proxy NPS
description: È possibile utilizzare questo argomento per informazioni sulle funzionalità e le funzionalità VPN di Windows Server 2016 e Windows 10.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2d138b9891fb3cfa8e15060be312ff945942c660
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405422"
---
# <a name="nps-proxy-server-load-balancing"></a>Bilanciamento del carico del server proxy NPS

Si applica a: Windows Server 2016

I client Remote Authentication Dial-In User Service (RADIUS), che sono server di accesso alla rete, ad esempio server VPN (Virtual Private Network) e punti di accesso wireless, creano richieste di connessione e le inviano a server RADIUS come NPS. In alcuni casi, un server dei criteri di accesso può ricevere troppe richieste di connessione contemporaneamente, causando un peggioramento delle prestazioni o un sovraccarico. Quando si esegue l'overload di un server dei criteri di rete, è consigliabile aggiungere altri NPSs alla rete e configurare il bilanciamento del carico. Quando si distribuiscono uniformemente le richieste di connessione in ingresso tra più NPSs per impedire l'overload di uno o più NPSs, viene chiamato bilanciamento del carico.

Il bilanciamento del carico è particolarmente utile per:

- Le organizzazioni che usano Extensible Authentication Protocol-Transport Layer Security \(EAP-TLS\) o Protected Extensible Authentication Protocol \(PEAP\)-TLS per l'autenticazione. Poiché questi metodi di autenticazione usano i certificati per l'autenticazione server e per l'autenticazione di computer client o utente, il carico su proxy e server RADIUS è più pesante rispetto a quando vengono usati metodi di autenticazione basati su password.
- Organizzazioni che devono sostenere la disponibilità continua del servizio.
- Provider di servizi Internet \(provider di servizi Internet\) che esternalizzano l'accesso VPN per altre organizzazioni. I servizi VPN in outsourcing possono generare un volume elevato di traffico di autenticazione.

Esistono due metodi che è possibile usare per bilanciare il carico delle richieste di connessione inviate al NPSs:

- Configurare i server di accesso alla rete per l'invio di richieste di connessione a più server RADIUS. Se, ad esempio, si dispone di 20 punti di accesso wireless e due server RADIUS, configurare ogni punto di accesso per l'invio di richieste di connessione a entrambi i server RADIUS. È possibile bilanciare il carico e fornire il failover in ogni server di accesso alla rete configurando il server di accesso per l'invio di richieste di connessione a più server RADIUS in base a un ordine di priorità specificato. Questo metodo di bilanciamento del carico è in genere ideale per le organizzazioni di piccole dimensioni che non distribuiscono un numero elevato di client RADIUS.
- Usare NPS configurato come proxy RADIUS per bilanciare il carico delle richieste di connessione tra più NPSs o altri server RADIUS. Ad esempio, se si dispone di 100 punti di accesso wireless, un proxy server dei criteri di rete e tre server RADIUS, è possibile configurare i punti di accesso per inviare tutto il traffico al proxy server dei criteri di rete. Nel proxy NPS, configurare il bilanciamento del carico in modo che il proxy distribuisca uniformemente le richieste di connessione tra i tre server RADIUS. Questo metodo di bilanciamento del carico è ideale per le organizzazioni di medie e grandi dimensioni con numerosi client e server RADIUS.

In molti casi, l'approccio migliore al bilanciamento del carico consiste nel configurare i client RADIUS per l'invio di richieste di connessione a due server proxy server dei criteri di accesso e quindi configurare i proxy NPS per il bilanciamento del carico tra server RADIUS. Questo approccio fornisce il failover e il bilanciamento del carico per i proxy server dei criteri di server e i server RADIUS.

## <a name="radius-server-priority-and-weight"></a>Priorità e peso del server RADIUS

Durante il processo di configurazione del proxy NPS, è possibile creare gruppi di server RADIUS remoti e quindi aggiungere server RADIUS a ogni gruppo. Per configurare il bilanciamento del carico, è necessario disporre di più di un server RADIUS per ogni gruppo di server RADIUS remoti. Durante l'aggiunta di membri del gruppo o dopo la creazione di un server RADIUS come membro del gruppo, è possibile accedere alla finestra di dialogo Aggiungi server RADIUS per configurare gli elementi seguenti nella scheda bilanciamento del carico:

- **Priorità**. Priorità specifica l'ordine di importanza del server RADIUS per il server proxy server dei criteri di server. Al livello di priorità deve essere assegnato un valore che corrisponde a un numero intero, ad esempio 1, 2 o 3. Più basso è il numero, la priorità più elevata assegnata dal proxy NPS al server RADIUS. Se ad esempio al server RADIUS viene assegnata la priorità più alta di 1, il proxy NPS invia prima le richieste di connessione al server RADIUS; Se i server con priorità 1 non sono disponibili, NPS Invia le richieste di connessione ai server RADIUS con priorità 2 e così via. È possibile assegnare la stessa priorità a più server RADIUS e quindi usare l'impostazione del peso per bilanciare il carico tra di essi.

- **Peso**. Il server dei criteri di gruppo usa questa impostazione di peso per determinare il numero di richieste di connessione da inviare a ogni membro del gruppo quando i membri del gruppo hanno lo stesso livello di priorità. L'impostazione del peso deve essere assegnata a un valore compreso tra 1 e 100 e il valore rappresenta una percentuale del 100%. Se, ad esempio, il gruppo di server RADIUS remoti contiene due membri che hanno entrambi un livello di priorità pari a 1 e una classificazione ponderata di 50, il proxy NPS inoltrerà il 50% delle richieste di connessione a ogni server RADIUS.

- **Impostazioni avanzate**. Queste impostazioni di failover consentono a NPS di determinare se il server RADIUS remoto non è disponibile. Se NPS rileva che un server RADIUS non è disponibile, può iniziare a inviare richieste di connessione ad altri membri del gruppo. Con queste impostazioni è possibile configurare il numero di secondi durante i quali il proxy NPS attende una risposta dal server RADIUS prima di considerare la richiesta eliminata; il numero massimo di richieste Eliminate prima che il proxy NPS identifichi il server RADIUS come non disponibile; e il numero di secondi che possono trascorrere tra le richieste prima che il proxy NPS identifichi il server RADIUS come non disponibile.

## <a name="configure-nps-proxy-load-balancing"></a>Configurare il bilanciamento del carico del proxy NPS

Prima di configurare il bilanciamento del carico, creare un piano di distribuzione che includa il numero di gruppi di server RADIUS remoti necessari, i server che sono membri di ogni particolare gruppo e le impostazioni di priorità e ponderazione per ogni server.

>[!NOTE]
>I passaggi seguenti presuppongono che siano già stati distribuiti e configurati i server RADIUS.

Per configurare NPS affinché funga da server proxy e inoltro delle richieste di connessione dai client RADIUS ai server RADIUS remoti, è necessario eseguire le operazioni seguenti:

1. Distribuire i client RADIUS \(server VPN, server di connessione remota, server gateway di Servizi terminal, commutatori di autenticazione 802.1 X e punti di accesso wireless 802.1 X\) e configurarli per l'invio di richieste di connessione ai server proxy server dei criteri di rete.

2. Nel proxy server dei criteri di rete configurare i server di accesso alla rete come client RADIUS. Per altre informazioni, vedere [configurare i client RADIUS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure).

3. Nel proxy NPS creare uno o più gruppi di server RADIUS remoti. Durante questo processo, aggiungere i server RADIUS ai gruppi di server RADIUS remoti. Per ulteriori informazioni, vedere [configurare gruppi di server RADIUS remoti](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure).

4. Nel proxy NPS, per ogni server RADIUS aggiunto a un gruppo di server RADIUS remoti, fare clic sulla scheda **bilanciamento del carico** del server RADIUS, quindi configurare **priorità**, **peso**e **Impostazioni avanzate**.

5. Nel proxy NPS configurare i criteri di richiesta di connessione per l'inoltro delle richieste di autenticazione e accounting ai gruppi di server RADIUS remoti. È necessario creare un criterio di richiesta di connessione per ogni gruppo di server RADIUS remoti. Per altre informazioni, vedere [configurare i criteri di richiesta di connessione](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure).


