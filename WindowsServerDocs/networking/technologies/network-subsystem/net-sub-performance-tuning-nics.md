---
title: Schede di rete di ottimizzazione delle prestazioni
description: Questo argomento fa parte della Guida di ottimizzazione delle prestazioni del sottosistema di rete per Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0b9b0f80-415c-4f5e-8377-c09b51d9c5dd
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1c6d36966ce6e2d407b6568e16946745256ee69b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="performance-tuning-network-adapters"></a>Schede di rete di ottimizzazione delle prestazioni

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per schede di rete consente di ottimizzare le prestazioni sono installati in computer che eseguono Windows Server 2016.

Determinare le impostazioni di ottimizzazione corrette per la scheda di rete dipendono le seguenti variabili:

- La scheda di rete e la relativa serie di funzionalità  

- Il tipo di carico di lavoro eseguito dal server  

- Le risorse hardware e software del server  

- Gli obiettivi di prestazioni per il server  

Se la scheda di rete offre opzioni di ottimizzazione, è possibile ottimizzare l'utilizzo della velocità effettiva e risorse di rete per ottenere una velocità effettiva ottimale sulla base dei parametri descritti in precedenza.  

Le sezioni seguenti vengono descritte alcune delle opzioni di ottimizzazione.  

##  <a name="bkmk_offload"></a>Abilitare le funzionalità di Offload

Attivando la funzionalità di offload scheda di rete è in genere utile. In alcuni casi, tuttavia, la scheda di rete non è sufficientemente potente da gestire le funzionalità di offload con una velocità effettiva elevata.

>[!IMPORTANT]
>Non usare le funzionalità di offload **Offload attività IPsec** o **TCP Chimney Offload **. Queste tecnologie sono deprecate in Windows Server 2016 e potrebbero influire negativamente server e le prestazioni della rete. Inoltre, queste tecnologie potrebbero non essere supportate da Microsoft in futuro.

Ad esempio, l'offload di segmentazione abilitazione può ridurre la velocità effettiva massima sostenibile in alcune schede di rete a causa di risorse hardware limitate. Tuttavia, se la velocità effettiva ridotta non è prevista una limitazione, è necessario abilitare funzionalità di offload anche per questo tipo di scheda di rete.

>[!NOTE]
> Alcune schede di rete richiedono funzionalità di offload in modo indipendente da abilitare per inviare e ricevere i percorsi.

##  <a name="bkmk_rss_web"></a>Abilitazione di Receive Side Scaling (RSS) per i server Web

RSS è possibile migliorare scalabilità e prestazioni web quando sono presenti un minor numero di schede di rete processori logici nel server. Quando tutto il traffico web passa attraverso le schede di rete che supportano RSS, richieste web provenienti da diverse connessioni possono essere elaborate simultaneamente diverse CPU.

È importante notare che a causa della logica di RSS e Hypertext Transfer Protocol \(HTTP\) per la distribuzione del carico, le prestazioni potrebbero essere gravemente danneggiata se una scheda di rete che non supporta RSS accetta traffico web in un server che dispone di uno o più schede di rete che supportano RSS. In questo caso, sarà necessario utilizzare schede di rete che supportano RSS o disabilitare RSS nella finestra delle proprietà della scheda di rete **proprietà avanzate** scheda. Per determinare se una scheda di rete è in grado di supportare RSS, è possibile visualizzare le informazioni di RSS in proprietà della scheda di rete **proprietà avanzate** scheda.

### <a name="rss-profiles-and-rss-queues"></a>Profili RSS e code RSS

Il profilo RSS predefiniti predefinito è NUMA statico e modifica il comportamento predefinito rispetto alle versioni precedenti del sistema operativo. Per iniziare con i profili RSS, è possibile esaminare i profili disponibili per capire quando sono vantaggiosi e come si applicano all'ambiente di rete e hardware.

Ad esempio, se si apre Gestione attività e si esaminano i processori logici nel server e questi sembrano sottoutilizzati per la ricezione del traffico, è possibile provare ad aumentare il numero di code RSS dal valore predefinito di 2 al massimo supportato dalla scheda di rete. Opzioni per modificare il numero di code RSS come parte del driver potrebbe essere la scheda di rete.

##  <a name="bkmk_resources"></a>Aumentare le risorse della scheda di rete

Per le schede di rete che consentono la configurazione manuale delle risorse, ad esempio riceveranno e inviano i buffer, è necessario aumentare le risorse allocate. 

Alcune schede di rete impostano i buffer di ricezione basso per risparmiare memoria allocata dall'host. Un valore basso comporta pacchetti ignorati e una riduzione delle prestazioni. Pertanto, per la ricezione intensiva, è consigliabile aumentare il valore del buffer di ricezione al massimo.

>[!NOTE]
>Se una scheda di rete non espone la configurazione manuale delle risorse, effettuata in modo dinamico le risorse o le risorse viene impostato su un valore fisso che non può essere modificato.

### <a name="enabling-interrupt-moderation"></a>Abilitazione di regolazione di Interrupt

Per controllare la regolazione di interrupt, alcune schede di rete espongono livelli di regolazione di interrupt diversi, buffer parametri di aggregazione (a volte separatamente per inviare e ricevere buffer), o entrambi.

Prendere in considerazione la regolazione di interrupt per carichi di lavoro associato alla CPU e valutare il compromesso tra risparmio della CPU host e latenza maggiore risparmio della CPU host a causa di maggior numero di interrupt e una latenza inferiore. Se la scheda di rete non esegue la regolazione di interrupt, ma espone l'aggregazione dei buffer, aumentando il numero di buffer fuse consente più buffer per invio o ricezione, che migliora le prestazioni.

##  <a name="bkmk_low"></a>Ottimizzazione delle prestazioni per l'elaborazione di pacchetti a bassa latenza

Molte schede di rete offrono opzioni di ottimizzazione della latenza indotta dal sistema operativo. La latenza è il tempo trascorso tra il driver di rete l'elaborazione di un pacchetto in ingresso e il driver di rete restituzione del pacchetto. Questo tempo è normalmente misurato in microsecondi. Per il confronto, il tempo di trasmissione per trasmissione di pacchetti a grande distanza viene in genere misurato in millisecondi \ (un ordine di grandezza larger\). Questa ottimizzazione non consente di ridurre il tempo impiegato per un pacchetto in transito.

Ecco alcuni suggerimenti per microsecondi l'ottimizzazione delle prestazioni.

- Impostare il BIOS del computer **ad alte prestazioni**, con C-state disattivati. Si noti tuttavia che si tratta di sistema e dipende dal BIOS e alcuni sistemi forniranno prestazioni più elevate se il sistema operativo controlla risparmio energia. È possibile controllare e modificare le impostazioni di risparmio energia da **impostazioni** o tramite il **powercfg** comando. Per ulteriori informazioni, vedere [opzioni della riga di comando di Powercfg](https://technet.microsoft.com/library/cc748940.aspx)

- Impostare il profilo del risparmio energia del sistema operativo **sistema a prestazioni elevate**. Si noti che questo non funzionerà correttamente se il BIOS di sistema è stato impostato per disabilitare il controllo del sistema operativo di gestione dell'alimentazione.

- Abilitare offload statici, ad esempio, checksum UDP, checksum e inviare Offload grandi dimensioni (LSO).

- Abilitare RSS se il traffico è multiflusso, ad esempio ricezione multicast a volume elevato.

-   Disabilitare il **regolazione di Interrupt** impostazione per i driver di scheda di rete che richiedono la minor latenza possibile. Tieni presente che questo può utilizzare più tempo della CPU e rappresenta un buon compromesso.

- Gestire interrupt di schede di rete e le DPC su un processore core che condivide cache CPU con il core usato dal programma (thread utente) che gestisce il pacchetto. Ottimizzazione dell'affinità CPU può essere utilizzato per indirizzare un processo verso determinati processori logici in combinazione con una configurazione di RSS per questo scopo. Usando lo stesso core per il thread modalità utente interrupt e DPC le prestazioni peggiorano con aumento del carico, perché il ISR DPC e thread competono per l'uso del core.

##  <a name="bkmk_smi"></a>System Management interrupt

Molti sistemi hardware usano System Management interrupt \(SMI\) per un'ampia gamma di funzioni di manutenzione, tra cui segnalazione degli errori di memoria \(ECC\) di codice di errore correzione, risparmio energia controllato dal BIOS, il controllo della ventola e compatibilità USB legacy. 

SMI è l'interrupt con la priorità più alta nel sistema e posiziona la CPU in modalità di gestione, anticipando così tutte le altre attività mentre esegue una routine di servizio interrupt, normalmente contenuta nel BIOS.

Sfortunatamente, questo può comportare picchi di latenza di 100 microsecondi o più. 

Se devi latenza più bassa, è consigliabile richiedere una versione del BIOS del fornitore dell'hardware che riduca gli SMI il livello minimo possibile. Questi sono spesso detta "BIOS a bassa latenza" o "SMI free BIOS". In alcuni casi, non è possibile per una piattaforma hardware eliminare completamente l'attività SMI perché viene usata per controllare funzioni essenziali (ad esempio ventole di raffreddamento).

>[!NOTE]
>Il sistema operativo non può esercitare alcun controllo sugli SMI, perché il processore logico è in esecuzione in una speciale modalità di manutenzione che impedisce l'intervento del sistema operativo.

##  <a name="bkmk_tcp"></a>Ottimizzazione delle prestazioni TCP

 È possibile ottimizzare le prestazioni TCP usando gli elementi seguenti.

###  <a name="bkmk_tcp_params"></a>Ottimizzazione automatica della finestra di ricezione TCP

Prima di Windows Server 2008, lo stack di rete utilizza una finestra del receive-side (65.535 byte) di dimensioni fisse limitata la velocità effettiva potenziale complessiva per le connessioni. Una delle modifiche più significative allo stack TCP è TCP ricevere ottimizzazione automatica della finestra. 

È possibile calcolare la velocità effettiva totale di una singola connessione quando si usa una dimensione fissa come finestra di ricezione TCP:

**Velocità effettiva totale raggiungibile in byte = TCP ricevere dimensioni della finestra in byte \ * (1 / latenza della connessione in secondi)**

Ad esempio, la velocità effettiva totale raggiungibile è solo 51 Mbps in una connessione con latenza di 10 ms \ (un valore ragionevole per infrastructure\ una rete aziendale di grandi dimensioni). 

Con ottimizzazione automatica, tuttavia, la finestra di receive-side è regolabile, e può aumentare da soddisfare la domanda del mittente. È possibile per una connessione a raggiungere una velocità completa di una connessione di 1 Gbps. Scenari di utilizzo della rete che potrebbero sono stati limitati in passato tramite la velocità effettiva totale raggiungibile delle connessioni TCP possono ora utilizzare completamente la rete.

#### <a name="deprecated-tcp-parameters"></a>Parametri TCP deprecati

Le seguenti impostazioni del Registro di sistema da Windows Server 2003 non sono più supportate e vengono ignorate nelle versioni successive.

Tutte queste impostazioni erano disponibili nel percorso del Registro di sistema seguente:

    ```  
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters  
    ```  

- TcpWindowSize

- NumTcbTablePartitions  

- MaxHashTableSize  



###  <a name="bkmk_wfp"></a>Piattaforma filtro Windows

Il Windows Filtering Platform (WFP) introdotta in Windows Vista e Windows Server 2008 offre API a fornitori non Microsoft di software indipendenti (ISV) per creare pacchetti filtri di elaborazione. Esempi di software antivirus e firewall.

>[!NOTE]
>Un filtro piattaforma filtro Windows non scritto può ridurre notevolmente le prestazioni di rete del server. Per ulteriori informazioni, vedere [-elaborazione di conversione dei pacchetti driver e App alla piattaforma filtro Windows](https://msdn.microsoft.com/windows/hardware/gg463267.aspx) in Windows Dev Center.


Per i collegamenti a tutti gli argomenti in questa Guida, vedere [ottimizzazione delle prestazioni del sottosistema di rete](net-sub-performance-top.md).
