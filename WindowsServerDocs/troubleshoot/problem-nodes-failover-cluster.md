---
title: Un problema relativo all'eliminazione di un nodo
description: In questo articolo vengono descritti i problemi riscontrati durante la rimozione di nodi dall'appartenenza al cluster di failover Active.
ms.prod: windows-server
ms.technology: server-general
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 30714e8a9c5ca4ad13ed6757c6ce1da211b279ac
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2020
ms.locfileid: "84150229"
---
# <a name="having-a-problem-with-nodes-being-removed-from-active-failover-cluster-membership"></a>Problemi con i nodi rimossi dall'appartenenza al cluster di failover attivo

Questo articolo illustra come risolvere i problemi in cui i nodi vengono rimossi in modo casuale dall'appartenenza al cluster di failover attivo.

## <a name="symptoms"></a>Sintomi

Quando si verifica il problema, vengono visualizzati eventi simili a quelli registrati nel registro eventi di sistema:

![Evento 1135](media/problem-nodes-failover-cluster/1135-1.png)

Questo evento verrà registrato in tutti i nodi del cluster, ad eccezione del nodo che è stato rimosso. Il motivo di questo evento è dovuto al fatto che uno dei nodi del cluster ha contrassegnato il nodo come inattivo. Invia quindi una notifica a tutti gli altri nodi dell'evento. Quando i nodi vengono informati, interrompono e arrestano le connessioni heartbeat al nodo inattivo.

## <a name="what-caused-the-node-to-be-marked-down"></a>Cosa ha causato la demarcazione del nodo

Tutti i nodi in un cluster di failover di Windows 2008 o 2008 R2 comunicano tra loro attraverso le reti impostate per consentire le comunicazioni di rete del cluster su questa rete. I nodi invieranno i pacchetti heartbeat attraverso tali reti a tutti gli altri nodi. Questi pacchetti dovrebbero essere ricevuti dagli altri nodi e quindi viene restituita una risposta. Ogni nodo del cluster ha i propri heartbeat che verranno monitorati per assicurarsi che la rete sia attiva e che gli altri nodi si trovino. L'esempio seguente consente di chiarire questo problema:

![Nodi](media/problem-nodes-failover-cluster/Node2.png)

Se uno di questi pacchetti non viene restituito, l'heartbeat specifico viene considerato non riuscito. Ad esempio, W2K8-R2-NODE2 invia una richiesta e riceve una risposta da W2K8-R2-NODE1 a un pacchetto heartbeat in modo che determini la rete e il nodo sia attivo.  Se W2K8-R2-NODE1 invia una richiesta a W2K8-R2-NODE2 e W2K8-R2-NODE1 non ottiene la risposta, viene considerato un heartbeat perso e W2K8-R2-NODE1 ne tiene traccia.  Questa risposta mancante può avere W2K8-R2-NODE1 per mostrare la rete come inattivo fino a quando non viene ricevuta un'altra richiesta di heartbeat.

Per impostazione predefinita, i nodi del cluster hanno un limite di cinque errori in 5 secondi prima che la connessione venga contrassegnata come inattiva. Quindi, se W2K8-R2-NODE1 non riceve la risposta cinque volte nel periodo di tempo, considera che la route specifica a W2K8-R2-NODE2 non sia attiva. Se altre route sono ancora considerate attive, W2K8-R2-NODE2 rimarrà come membro attivo.

Se tutte le route sono contrassegnate per W2K8-R2-NODE2, vengono rimosse dall'appartenenza al cluster di failover attivo e viene registrato l'evento 1135 visualizzato nella prima sezione. In W2K8-R2-NODE2 il servizio cluster viene interrotto e quindi riavviato in modo che possa provare a partecipare nuovamente al cluster.

Per altre informazioni sul modo in cui si gestiscono route specifiche che si arrestano con tre o più nodi, vedere il Blog ["partizionato" di reti cluster](/archive/blogs/askcore/partitioned-cluster-networks) scritto da Jeff Hughes.

## <a name="now-that-we-know-how-the-heartbeat-process-works-what-are-some-of-the-known-causes-for-the-process-to-fail"></a>Ora che sappiamo come funziona il processo di heartbeat, quali sono alcune delle cause note che il processo ha esito negativo

1. Errori hardware di rete effettivi. Se il pacchetto viene perso in transito in un punto qualsiasi tra i nodi, gli heartbeat avranno esito negativo. Una traccia di rete da entrambi i nodi interessati lo rivelerà.

2. Il profilo per le connessioni di rete potrebbe essere rimbalzato da un dominio a un dominio pubblico e viceversa. Durante la transizione di queste modifiche, l'I/O di rete può essere bloccato. È possibile verificare se questo è il caso esaminando il log operativo del profilo di rete. È possibile trovare questo log aprendo il Visualizzatore eventi e passando a: applicazioni e servizi Logs\Microsoft\Windows\NetworkProfile\Operational. Esaminare gli eventi in questo log sul nodo indicato nell'ID evento: 1135 e verificare se il profilo è stato modificato in questo momento. In tal caso, consultare l'articolo della Knowledge Base relativo al [profilo del percorso di rete modificato da "dominio" a "pubblico" in Windows 7 o in Windows Server 2008 R2](https://support.microsoft.com/help/2524478/the-network-location-profile-changes-from-domain-to-public-in-windows).

3. Per i server è abilitato IPv6, ma le due regole seguenti sono disabilitate per il traffico in ingresso e in uscita nella Windows Firewall:

    - Rete principale-annuncio di individuazione router adiacenti
    - Rete principale-richiesta di individuazione vicina

4. Il software antivirus potrebbe interferire anche con questo processo. Se si sospetta questo problema, eseguire il test disabilitando o disinstallando il software. Eseguire questa operazione a proprio rischio, perché a questo punto l'utente non sarà protetto da virus.

5. Anche la latenza sulla rete potrebbe causare questo problema. È possibile che i pacchetti non vadano perduti tra i nodi, ma potrebbero non raggiungere i nodi in modo sufficientemente veloce prima della scadenza del periodo di timeout.

6. IPv6 è il protocollo predefinito che verrà utilizzato dal clustering di failover per gli heartbeat. L'heartbeat è un pacchetto di rete unicast UDP che comunica sulla porta 3343. Se sono presenti opzioni, firewall o router non configurati correttamente per consentire il traffico attraverso, è possibile riscontrare problemi di questo tipo.

7. Gli aggiornamenti dei criteri di sicurezza IPsec possono anche causare questo problema. Il problema specifico è che, durante un aggiornamento di criteri di gruppo IPSec, tutte le associazioni di sicurezza IPsec vengono eliminate da Windows Firewall con sicurezza avanzata (WFAS). Quando si verifica questa situazione, viene bloccata tutta la connettività di rete. Quando si rinegoziano le associazioni di sicurezza in caso di ritardi nell'esecuzione dell'autenticazione con Active Directory, questi ritardi (in cui tutte le comunicazioni di rete sono bloccate) bloccano anche gli heartbeat del cluster e provocano il monitoraggio dell'integrità del cluster per rilevare i nodi come inattivi se non rispondono entro la soglia di 5 secondi.

8. Driver della scheda di rete obsoleti o obsoleti o firmware.  In alcuni casi, una semplice configurazione errata della scheda di rete o del Commuter può causare la perdita di heartbeat.

9. Le schede di rete moderne e le schede di rete virtuali potrebbero riscontrare perdite di pacchetti.  Questa operazione può essere rilevata aprendo performance monitor e aggiungendo il contatore "Network Interface\Packets received".  Questo contatore è cumulativo e aumenta solo fino a quando il server non viene riavviato.  La visualizzazione di un numero elevato di pacchetti rilasciati qui potrebbe essere un segno che i buffer di ricezione nella scheda di rete sono impostati su un valore troppo basso o che il server sta eseguendo lentamente e non è in grado di gestire il traffico in ingresso.  Ogni produttore di schede di rete sceglie se esporre queste impostazioni nelle proprietà della scheda di rete, pertanto è necessario fare riferimento al sito Web del produttore per apprendere come aumentare questi valori e usare i valori consigliati.  Se si esegue in VMware, il blog seguente illustra questo aspetto in modo più dettagliato, tra cui come stabilire se questo è il problema, oltre a fare riferimento all'articolo VMWare sulle impostazioni da modificare.

    [Nodi rimossi dall'appartenenza al cluster di failover in VMWare ESX](/archive/blogs/askcore/nodes-being-removed-from-failover-cluster-membership-on-vmware-esx)

Questi sono i motivi più comuni per cui questi eventi vengono registrati, ma potrebbero anche esserci altri motivi. Il punto di questo blog è stato quello di fornire informazioni approfondite sul processo e fornire anche idee su cosa cercare. Alcuni generano i valori seguenti ai valori massimi per tentare di arrestare il problema.

|Parametro|Predefinito|Range|
|---|---|---|
|**SameSubnetDelay**|1000 millisecondi|250-2000 millisecondi|
|**CrossSubnetDelay**|1000 millisecondi|250-4000 millisecondi|
|**SameSubnetThreshold**|5|3-10|
|**CrossSubnetThreshold**|5|3-10|
||||

Se si aumentano questi valori al massimo, è possibile che l'evento e la rimozione del nodo vengano rilasciati, ma è sufficiente mascherare il problema. Non corregge alcun problema. La cosa migliore da fare è individuare la causa principale degli errori di heartbeat e risolverla. L'unica necessità reale di aumentare questi valori è in uno scenario multisito in cui i nodi si trovano in posizioni diverse e la latenza di rete non può essere superata.
