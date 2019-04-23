---
title: Ottimizzazione delle prestazioni delle schede di rete
description: Questo argomento fa parte della Guida all'ottimizzazione delle prestazioni del sottosistema di rete di Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0b9b0f80-415c-4f5e-8377-c09b51d9c5dd
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d6d54f33108d1cdb936b02fc556acca1e5518b9b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861532"
---
# <a name="performance-tuning-network-adapters"></a>Ottimizzazione delle prestazioni delle schede di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile usare questo argomento per schede di rete di ottimizzazione delle prestazioni che vengono installati nei computer che eseguono Windows Server 2016.

La determinazione delle impostazioni di ottimizzazione corrette per la scheda di rete dipende dalle variabili seguenti:

- La scheda di rete e il relativo set di funzionalità  

- Il tipo di carico di lavoro eseguito dal server  

- Le risorse hardware e software del server  

- Gli obiettivi in termini di prestazioni del server  

Se la scheda di rete offre opzioni di ottimizzazione, è possibile ottimizzare la velocità effettiva della rete e l'uso delle risorse per raggiungere una velocità effettiva ottimale sulla base dei parametri descritti in precedenza.  

Nelle sezioni seguenti vengono illustrate alcune delle opzioni di ottimizzazione disponibili.  

##  <a name="bkmk_offload"></a> Abilitazione delle funzionalità di Offload

Di norma è utile attivare l'offload della scheda di rete. Talvolta, tuttavia, la scheda di rete non è sufficientemente potente da gestire le funzionalità di offload con una velocità effettiva elevata.

>[!IMPORTANT]
>Non usare le funzionalità di offload **Offload attività IPsec** oppure **TCP Chimney Offload**. Queste tecnologie sono deprecate in Windows Server 2016 e potrebbero influire negativamente sulle server e le prestazioni della rete. Inoltre, queste tecnologie potrebbero non essere supportate da Microsoft in futuro.

Ad esempio, specificando l'offload di segmentazione è possibile che in alcune schede di rete la velocità effettiva massima sostenibile si riduca a causa delle risorse hardware limitate. Se tuttavia la velocità effettiva ridotta non viene considerata una limitazione, è opportuno abilitare le funzionalità di offload anche per schede di rete di questo tipo.

>[!NOTE]
> Per alcune schede di rete è necessario abilitare le funzionalità di offload in modo indipendente per percorsi di invio e ricezione.

##  <a name="bkmk_rss_web"></a> Abilitazione di Receive Side Scaling (RSS) per i server Web

Con RSS è possibile migliorare scalabilità e prestazioni Web se il server dispone di un numero di schede di rete inferiore al numero di processori logici. Quando l'intero traffico Web passa attraverso le schede di rete che supportano RSS, le richieste Web in arrivo da diverse connessioni possono essere elaborate simultaneamente da diverse CPU.

È importante notare che a causa della logica di RSS e Hypertext Transfer Protocol \(HTTP\) per la distribuzione del carico, le prestazioni potrebbero essere gravemente ridotte se una scheda di rete che non supporta RSS accetta traffico web in un server che dispone di uno o più schede di rete che supportano RSS. In questo caso, sarà necessario usare schede di rete che supportano RSS oppure disabilitare RSS nella scheda **Proprietà avanzate** delle proprietà della scheda di rete. Per stabilire se una scheda di rete supporta RSS, è possibile visualizzare le informazioni su RSS nella scheda **Proprietà avanzate** delle proprietà della scheda di rete.

### <a name="rss-profiles-and-rss-queues"></a>Profili RSS e code RSS

Il profilo RSS predefiniti predefinito è NUMA statico, che modifica il comportamento predefinito rispetto alle versioni precedenti del sistema operativo. Per iniziare a usare i profili RSS, è possibile esaminare i profili disponibili per capire quando sono vantaggiosi e come si applicano all'ambiente di rete e all'hardware.

Se ad esempio si apre Gestione attività, si esaminano i processori logici nel server e questi sembrano sottoutilizzati per la ricezione del traffico, è possibile aumentare il numero di code RSS dal numero predefinito di 2 al massimo supportato dalla scheda di rete. Nella scheda di rete, le opzioni per modificare il numero di code RSS potrebbero essere parte del driver.

##  <a name="bkmk_resources"></a> Aumentare le risorse della scheda di rete

Nelle schede di rete che consentono la configurazione manuale delle risorse, come buffer di ricezione e di invio, è necessario aumentare le risorse allocate. 

Alcune schede di rete impostano i buffer di ricezione su un valore basso per risparmiare memoria allocata dall'host. In presenza di un valore basso, alcuni pacchetti vengono scartati e le prestazioni calano. Di conseguenza, in caso di carichi di lavoro con attività di ricezione intensiva, è consigliabile portare al massimo il valore del buffer di ricezione.

>[!NOTE]
>Se una scheda di rete non consente la configurazione manuale delle risorse, tale configurazione viene effettuata in modo dinamico oppure per le risorse viene impostato un valore fisso che non può essere modificato.

### <a name="enabling-interrupt-moderation"></a>Abilitazione della regolazione di interrupt

Per controllare la regolazione di interrupt, alcune schede di rete espongono diversi livelli di regolazione di interrupt o parametri di aggregazione dei buffer (a volte separatamente per buffer di invio e di ricezione) entrambi.

È opportuno prendere in considerazione la regolazione di interrupt per carichi di lavoro basati su CPU e il compromesso tra risparmio e latenza della CPU host da un lato e maggiore risparmio della CPU host dovuto a un maggior numero di interrupt e una latenza inferiore. Se la scheda di rete non esegue la regolazione di interrupt, ma espone l'aggregazione dei buffer, l'aumento dei buffer aggregati equivale a un maggior numero di buffer per invio o ricezione e a un miglioramento delle prestazioni.

##  <a name="bkmk_low"></a> Ottimizzazione delle prestazioni per l'elaborazione dei pacchetti a bassa latenza

Molte schede di rete offrono opzioni di ottimizzazione della latenza indotta dal sistema operativo. La latenza è il tempo trascorso tra l'elaborazione di un pacchetto in arrivo da parte dell'unità di rete e la restituzione del pacchetto da parte dell'unità di rete. Questo intervallo di tempo è normalmente misurato in microsecondi. Per il confronto, il tempo di trasmissione per trasmissione di pacchetti su lunghe distanze viene in genere misurato in millisecondi \(un ordine di grandezza maggiore\). Questa ottimizzazione non accelererà il trasferimento dei pacchetti.

Seguono alcuni suggerimenti utili per l'ottimizzazione delle prestazioni di schede che rilevano i microsecondi.

- Impostare il BIOS del computer su **Prestazioni elevate**, con i C-state disattivati. Si noti tuttavia che questa operazione dipende dal sistema e dal BIOS e che alcuni sistemi forniranno prestazioni più elevate se il sistema operativo controlla il risparmio energia. È possibile controllare e modificare le impostazioni di risparmio energia da **le impostazioni** oppure usando la **powercfg** comando. Per altre informazioni, vedere [opzioni della riga di comando Powercfg](https://technet.microsoft.com/library/cc748940.aspx)

- Impostare il profilo del risparmio energia del sistema operativo su **Sistema a prestazioni elevate**. Si noti che questa opzione non funzionerà correttamente se le impostazioni del BIOS del sistema prevedono la disattivazione del controllo del risparmio energia da parte del sistema operativo.

- Abilitare offload statici, ad esempio checksum UDP, checksum e Offload invio di grandi dimensioni (LSO).

- Abilitare RSS se il traffico è multiflusso, come nel caso di ricezione multicast a volume elevato.

-   Disabilitare l'impostazione **Regolazione di interrupt** per driver di schede di rete che richiedono la minor latenza possibile. È importante ricordare che questo può comportare un maggiore utilizzo di tempo di CPU e rappresenta una soluzione di compromesso.

- Gestire interrupt di schede di rete e chiamate di procedura differita su un processore core che condivide cache CPU con il core usato dal programma (thread utente) che gestisce il pacchetto. A tale scopo, è possibile usare l'ottimizzazione dell'affinità CPU per indirizzare un processo verso determinati processori logici insieme alla configurazione di RSS. Usando lo stesso core per l'interrupt, chiamata di procedura differita e thread modalità utente thread le prestazioni peggiorano con l'aumentare del carico, perché IRS, chiamata di procedura differita e thread competono per l'uso del core.

##  <a name="bkmk_smi"></a> System Management interrupt

Molti sistemi hardware usano System Management interrupt \(SMI\) per un'ampia gamma di funzioni di manutenzione, tra cui la segnalazione di codice di errore correzione \(ECC\) gli errori di memoria, compatibilità USB legacy, ventola risparmio energia controllato dal controllo e BIOS. 

SMI è l'interrupt con la priorità più elevata nel sistema e posiziona la CPU in modalità di gestione, anticipando così tutte le altre attività mentre esegue una routine di servizio interrupt, normalmente contenuta nel BIOS.

In questo modo si possono però verificare picchi di latenza di 100 microsecondi o più. 

Se è necessario ridurre al minimo la latenza, è opportuno richiedere al provider hardware una versione BIOS che riduca gli SMI il più possibile. Spesso sono definiti "BIOS a bassa latenza" o "SMI free BIOS". In alcuni casi, non è possibile per una piattaforma hardware eliminare del tutto l'attività SMI, che viene usata per controllare funzioni essenziali (come le ventole di raffreddamento).

>[!NOTE]
>Il sistema operativo non può esercitare alcun controllo sugli SMI, perché il processore logico viene eseguito in una speciale modalità di manutenzione che impedisce l'intervento del sistema operativo.

##  <a name="bkmk_tcp"></a> Ottimizzazione delle prestazioni TCP

 È possibile ottimizzare le prestazioni TCP usando gli elementi seguenti.

###  <a name="bkmk_tcp_params"></a>  Ottimizzazione automatica della finestra di ricezione TCP

Prima di Windows Server 2008, lo stack di rete utilizzata una finestra di ricezione di dimensione fissa (65.535 byte) che limitava la velocità effettiva potenziale complessiva per le connessioni. Uno dei cambiamenti più significativi apportati allo stack TCP consiste nell'ottimizzazione automatica della finestra di ricezione TCP. 

È possibile calcolare la velocità effettiva totale di una singola connessione quando si usa una dimensione fissa come finestra di ricezione TCP:

**Velocità effettiva totale raggiungibile in byte = TCP dimensioni della finestra di ricezione in byte \* (1 / latenza della connessione in secondi)**

Ad esempio, la velocità effettiva totale raggiungibile è solo 51 Mbps in una connessione con latenza di 10 ms \(un valore ragionevole per un'infrastruttura di rete aziendali di grandi dimensioni\). 

L'ottimizzazione automatica consente tuttavia la regolazione della finestra di ricezione, che può raggiungere dimensioni tali da soddisfare la domanda del mittente. È possibile che una connessione ottenere una velocità completa di una connessione di 1 Gbps. Gli scenari di utilizzo della rete che presentavano in passato limiti dovuti alla velocità effettiva totale raggiungibile delle connessioni TCP sono stati superati.

#### <a name="deprecated-tcp-parameters"></a>Deprecati i parametri TCP

Le seguenti impostazioni del Registro di sistema da Windows Server 2003 non sono più supportate e vengono ignorate nelle versioni successive.

Tutte queste impostazioni ha il percorso del Registro di sistema seguente:

    ```  
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters  
    ```  

- TcpWindowSize

- NumTcbTablePartitions  

- MaxHashTableSize  



###  <a name="bkmk_wfp"></a> Piattaforma filtro Windows

Il Windows Filtering Platform (WFP) introdotta in Windows Vista e Windows Server 2008 offre API a fornitori non Microsoft di software indipendenti (ISV) per creare filtri di elaborazione dei pacchetti. Ne sono esempi i software per firewall e antivirus.

>[!NOTE]
>Un filtro Piattaforma filtro Windows non scritto in modo corretto può ridurre in misura significativa le prestazioni di rete di un server. Per altre informazioni, vedere [l'elaborazione di conversione dei pacchetti driver e le App per piattaforma filtro Windows](https://msdn.microsoft.com/windows/hardware/gg463267.aspx) in Windows Dev Center.


Per collegamenti a tutti gli argomenti in questa Guida, vedere [ottimizzazione delle prestazioni di rete sottosistema](net-sub-performance-top.md).
