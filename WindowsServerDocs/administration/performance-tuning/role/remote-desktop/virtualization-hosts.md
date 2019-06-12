---
title: Gli host di virtualizzazione Desktop remoto l'ottimizzazione delle prestazioni
description: Ottimizzazione delle prestazioni per gli host di virtualizzazione Desktop remoto
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: da528a742a7f49513c50b22a25970d65b9e1885f
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811377"
---
# <a name="performance-tuning-remote-desktop-virtualization-hosts"></a>Gli host di virtualizzazione Desktop remoto l'ottimizzazione delle prestazioni


Host di virtualizzazione Desktop remoto (Host di virtualizzazione Desktop remoto) è un servizio di ruolo che supporta scenari di Virtual Desktop Infrastructure (VDI) e consente più utenti simultanei di eseguire applicazioni basate su Windows in macchine virtuali ospitate in un server che esegue Windows Server 2016 e Hyper-V.

Windows Server 2016 supporta due tipi di desktop virtuali, i desktop virtuali personali e desktop virtuali in pool.

**In questo argomento:**

-   [Considerazioni generali](#general-considerations)

-   [Ottimizzazioni delle prestazioni](#performance-optimizations)

## <a name="general-considerations"></a>Considerazioni generali


### <a name="storage"></a>Archiviazione

L'archiviazione è un collo di bottiglia più probabile ed è importante impostare le dimensioni di archiviazione per gestire correttamente il carico dei / o generata da modifiche apportate allo stato di macchina virtuale. Se un progetto pilota o una simulazione non è fattibile, è una buona indicazione per effettuare il provisioning di un asse del disco per quattro macchine virtuali attive. Usare le configurazioni del disco con buone prestazioni in scrittura (ad esempio RAID 1 + 0).

Quando appropriato, usare la deduplicazione dei dischi e la memorizzazione nella cache per ridurre il carico di lettura del disco e abilitare la soluzione di archiviazione velocizzare le prestazioni memorizzando nella cache una parte significativa dell'immagine.

### <a name="data-deduplication-and-vdi"></a>L'infrastruttura VDI e la deduplicazione dei dati

Introdotta in Windows Server 2012 R2, la deduplicazione dati supporta l'ottimizzazione dei file aperti. Per usare macchine virtuali in esecuzione su un volume deduplicato, i file di macchina virtuale devono essere archiviati in un host separato dall'host Hyper-V. Se Hyper-V e la deduplicazione siano in esecuzione nello stesso computer, le due funzionalità verranno si contendono le risorse di sistema e influire negativamente sulle prestazioni complessive.

Il volume deve essere configurato anche per usare il tipo di ottimizzazione di deduplicazione "Virtual Desktop Infrastructure (VDI)". È possibile configurare questa impostazione tramite Server Manager (**servizi File e archiviazione**  - &gt; **volumi**  - &gt; **impostazionidideduplicazione**) In alternativa, con il comando Windows PowerShell seguente comando:

``` syntax
Enable-DedupVolume <volume> -UsageType HyperV
```

> [!NOTE]
> Ottimizzazione della deduplicazione dei dati di file aperti è supportata solo per scenari VDI con Hyper-V usando l'archiviazione remota tramite SMB 3.0.

### <a name="memory"></a>Memoria

Utilizzo della memoria server dipende dai tre fattori principali:

-   Overhead del sistema operativo

-   Servizio Hyper-V overhead per ogni macchina virtuale

-   Memoria allocata a ogni macchina virtuale

Per un carico di lavoro tipico knowledge worker, guest virtuale macchine in esecuzione x86 Windows 8 o Windows 8.1 è necessario assegnare ~ 512 MB di memoria come linea di base. Tuttavia, la memoria dinamica probabilmente aumenterà la memoria della macchina virtuale guest per circa 800 MB, a seconda del carico di lavoro. Per x64, noteremo sull'avvio di 800 MB, aumento su 1024 MB.

Pertanto, è importante fornire memoria sufficiente server per soddisfare la memoria necessaria per il numero previsto di macchine virtuali guest, oltre a consentire una quantità di memoria sufficiente per il server.

### <a name="cpu"></a>CPU

Quando si pianifica la capacità del server per un server Host di virtualizzazione Desktop remoto, il numero di macchine virtuali per ogni core fisico dipenderà dalla natura del carico di lavoro. Come punto di partenza, è ragionevole per piano di 12 le macchine virtuali per ogni core fisico e quindi eseguire gli scenari appropriati per convalidare le prestazioni e la densità. Un aumento della densità potrebbe essere raggiungibile in base alle specifiche del carico di lavoro.

Si consiglia di abilitare hyper-threading, ma assicurarsi di calcolare il rapporto di oversubscription in base al numero di core fisici e non il numero di processori logici. In questo modo il livello di prestazioni in base a ogni CPU previsto.

### <a name="virtual-gpu"></a>GPU virtuale

Microsoft RemoteFX per Host di virtualizzazione Desktop remoto offre un'esperienza grafica avanzata per codificare Virtual Desktop Infrastructure (VDI) tramite comunicazione remota lato host, una pipeline di rendering / acquisizione / codifica, altamente efficienti basate su GPU, la limitazione delle richieste basata su client attività e una GPU virtuale abilitata per DirectX. RemoteFX per Host di virtualizzazione Desktop remoto consente di aggiornare la GPU virtuale da DirectX9 per DirectX11. Inoltre, migliora l'esperienza utente grazie al supporto di più monitor con risoluzioni più elevate.

L'esperienza RemoteFX DirectX11 è disponibile senza una hardware GPU, tramite un driver emulati software. Sebbene questo software GPU offre un'esperienza ottimale, l'unità di elaborazione grafica virtuale RemoteFX (VGPU) aggiunge un'esperienza con accelerata hardware per i desktop virtuali.

Per sfruttare i vantaggi dell'esperienza di RemoteFX VGPU in un server che esegue Windows Server 2016, è necessario un driver di GPU nel server host (ad esempio DirectX11.1 o WDDM 1.2). Per altre informazioni sulle offerte GPU da usare con RemoteFX per Host di virtualizzazione Desktop remoto, contattare il provider GPU.

Se si usa la GPU virtuale RemoteFX nella distribuzione VDI, la capacità di distribuzione possono variare in base gli scenari di utilizzo e sulla configurazione hardware. Quando si pianifica la distribuzione, considerare quanto segue:

-   Numero di GPU nel sistema

-   Capacità di memoria video con la GPU

-   Risorse hardware e del processore nel sistema

### <a name="remotefx-server-system-memory"></a>Memoria di sistema server RemoteFX

Per ogni desktop virtuale abilitato con una GPU virtuale RemoteFX Usa la memoria di sistema in sistema operativo guest e il server abilitate per RemoteFX. L'hypervisor garantisce la disponibilità di memoria di sistema per un sistema operativo guest. Nel server, ogni virtual desktop virtuali abilitate per GPU deve annunciare il requisito di memoria di sistema nell'hypervisor. All'avvio abilitate per GPU virtuali desktop virtuale, l'hypervisor di riserva della memoria di sistema aggiuntivi nel server abilitate per RemoteFX per desktop virtuali basate su GPU Virtualizzata.

I requisiti di memoria per il server abilitate per RemoteFX sono dinamico, poiché è dipendente dal numero di monitor che sono associati i desktop virtuali basate su GPU Virtualizzata e la risoluzione massima per la quantità di memoria utilizzata nel server abilitate per RemoteFX tali monitoraggi.

### <a name="remotefx-server-gpu-video-memory"></a>Server RemoteFX memoria video GPU

Ciascun virtual desktop virtuali abilitate per GPU Usa la memoria video nell'hardware di GPU nel server host per eseguire il rendering sul desktop. Oltre il rendering, la memoria video viene utilizzata da un codec per comprimere lo schermo sottoposta a rendering. La quantità di memoria necessaria è direttamente basata sulla quantità di monitoraggi sottoposte a provisioning per la macchina virtuale.

La memoria video riservata varia in base al numero di monitor e la risoluzione dello schermo di sistema. Alcuni utenti possono richiedere una risoluzione superiore dello schermo per attività specifiche. È maggiore scalabilità con le impostazioni di risoluzione inferiore se tutte le altre impostazioni rimangono costanti.

### <a name="remotefx-processor"></a>Processore RemoteFX

L'hypervisor pianifica il server abilitate per RemoteFX e il supporto per GPU virtuali desktop virtuali sulla CPU. A differenza di memoria di sistema, non c'è informazioni relative a risorse aggiuntive che dovrà condividere con l'hypervisor RemoteFX. Overhead la CPU aggiuntiva che consente la abilitate per GPU virtuali desktop virtuale RemoteFX è correlata all'esecuzione di uno stack di Remote Desktop Protocol in modalità utente e il driver GPU virtuale.

Nel server abilitate per RemoteFX, il sovraccarico è maggiore in quanto il sistema viene eseguito un processo aggiuntivo (rdvgm.exe) per desktop virtuali abilitate per GPU virtuale. Questo processo Usa il driver di dispositivo di grafica per eseguire i comandi nella GPU. Il codec Usa anche le CPU per comprimere i dati della schermata che devono essere inviato al client.

Più processori virtuali significa che una migliore esperienza utente. È consigliabile allocare almeno due CPU virtuali per desktop virtuali abilitate per GPU virtuale. È anche consigliabile usare x64 architettura per i desktop virtuali abilitate per GPU virtuali perché le prestazioni su x64 x86 meglio confrontato con le macchine virtuali delle macchine virtuali.

### <a name="remotefx-gpu-processing-power"></a>Potenza di elaborazione RemoteFX GPU

Per ogni abilitate per GPU virtuali desktop virtuale, è disponibile un processo di DirectX corrispondente in esecuzione nel server abilitate per RemoteFX. Questo processo riesegue tutti i comandi di grafica che riceve dal desktop virtuale RemoteFX alla GPU fisico. Per la GPU fisica, è equivalente a in esecuzione simultanea di più applicazioni DirectX.

In genere, i driver e i dispositivi di grafica sono ottimizzati per l'esecuzione di alcune applicazioni sul desktop. RemoteFX adatta le GPU da utilizzare in modo univoco. Per misurare le prestazioni della GPU in un server RemoteFX, sono stati aggiunti i contatori delle prestazioni per misurare la risposta GPU per le richieste di RemoteFX.

In genere quando una risorsa GPU non ha risorse sufficienti, leggere e scrivere operazioni di richiede GPU molto tempo per il completamento. Usando i contatori delle prestazioni, gli amministratori possono adottare misure di prevenzione, eliminando la possibilità di tempi di inattività per gli utenti finali.

Contatori delle prestazioni seguenti sono disponibili nel server RemoteFX per misurare le prestazioni della GPU virtuale:

**RemoteFX grafica**

-   **Fotogrammi ignorati al secondo - risorse insufficienti Client** numero di fotogrammi ignorati al secondo a causa di risorse insufficienti client

-   **Rapporto di compressione grafica** rapporto tra il numero di byte con codificata per il numero di byte di input

**RemoteFX root management GPU**

-   **Risorse: TDR in GPU Server** numero totale di volte in cui scade il TDR in GPU nel server

-   **Risorse: Macchine virtuali in esecuzione RemoteFX** numero totale di macchine virtuali con la scheda Video RemoteFX 3D installato

-   **VRAM: MB disponibili per ogni GPU** quantità di memoria video dedicata che non è in uso

-   **VRAM: % Riservato per GPU** percentuale di memoria video dedicata, che è stata riservata per RemoteFX

**Software di RemoteFX**

-   **Acquisizione di velocità per il monitoraggio** \[1-4\] Visualizza la frequenza di acquisizione di RemoteFX per i monitoraggi 1-4

-   **Rapporto di compressione** deprecate in Windows 8 e sostituito dal **rapporto di compressione della grafica**

-   **Fotogrammi al secondo di ritardo** numero di fotogrammi al secondo in cui i dati di grafica non è stati inviati all'interno di un determinato periodo di tempo

-   **Tempo di risposta GPU di acquisizione** misurata la latenza all'interno di acquisizione RemoteFX (in microsecondi) GPU completamento delle operazioni

-   **Tempo di risposta GPU dal rendering** misurata la latenza all'interno di RemoteFX rendering (in microsecondi) GPU completamento delle operazioni

-   **Byte di output** numero totale di RemoteFX byte di output

-   **In attesa di conteggio di client/sec** deprecate in Windows 8 e sostituito dal **fotogrammi ignorati al secondo - risorse insufficienti Client**

**Gestione di GPU virtualizzata RemoteFX**

-   **Risorse: TDR locale alle macchine virtuali** numero totale di TDR che si sono verificati in questa macchina virtuale (TDR che il server di propagazione per le macchine virtuali non sono incluse)

-   **Risorse: TDR propagati dal Server** numero totale di TDR che si è verificato sul server e che siano state propagate alla macchina virtuale

**Prestazioni di GPU virtualizzata RemoteFX macchina virtuale**

-   **Dati: Richiamato/sec Visualizza il numero** totale (in secondi) di presente operazioni da eseguire il rendering sul desktop della macchina virtuale al secondo

-   **Dati: In uscita/sec Visualizza il numero** numero totale di operazioni presente inviato dalla macchina virtuale al server GPU al secondo

-   **Dati: Byte letti/sec** numero totale di byte letti dal server abilitate per RemoteFX al secondo

-   **Dati: Invio byte/sec** numero totale di byte inviati al server abilitate per RemoteFX GPU al secondo

-   **DMA: Buffer di comunicazione della latenza (sec) di Media** quantità media di tempo (in secondi) impiegato per i buffer di comunicazione

-   **DMA: Latenza di buffer DMA (sec)** quantità di tempo (in secondi) da quando viene inviata DMA fino al completamento

-   **DMA: Lunghezza coda** lunghezza della coda DMA per una scheda Video RemoteFX 3D

-   **Risorse: Timeout TDR al GPU** conteggio di TDR timeout che si sono verificati per GPU nella macchina virtuale

-   **Risorse: Timeout TDR al motore GPU** conteggio di TDR timeout che si sono verificati per GPU del motore nella macchina virtuale

Oltre ai contatori di prestazioni RemoteFX virtuali GPU, è anche possibile misurare l'utilizzo della GPU usando Process Explorer, che mostra l'utilizzo della memoria video e l'utilizzo della GPU.

## <a name="performance-optimizations"></a>Ottimizzazioni delle prestazioni

### <a name="dynamic-memory"></a>Memoria dinamica

Memoria dinamica Abilita in modo più efficiente utilizzo delle risorse di memoria del server che esegue Hyper-V dal bilanciamento del carico di modalità di distribuzione della memoria tra macchine virtuali in esecuzione. Memoria può essere riallocata in modo dinamico tra le macchine virtuali in risposta alla loro modifica dei carichi di lavoro.

Memoria dinamica consente di aumentare la densità di macchine virtuali con le risorse che hai già senza sacrificare le prestazioni o della scalabilità. Il risultato è un uso più efficiente delle risorse hardware server costose, che può convertire in una gestione più semplice e Riduci i costi.

Nei sistemi operativi guest che eseguono Windows 8 e versioni successive con processori virtuali che si estendono su più processori logici, valutare il compromesso tra l'esecuzione con la memoria dinamica per ridurre al minimo dell'utilizzo della memoria e la disabilitazione della memoria dinamica per migliorare le prestazioni di un'applicazione che è compatibile con computer-topologia. Questo tipo di applicazione può sfruttare le informazioni sulla topologia per prendere decisioni di allocazione di memoria e pianificazione.

### <a name="tiered-storage"></a>Archiviazione a livelli

Host di virtualizzazione Desktop remoto supporta l'archiviazione a livelli per i pool di desktop virtuali. Computer fisico in cui è condiviso da tutti i pool dei desktop virtuali in una raccolta è possibile usare una soluzione di archiviazione di piccole dimensioni e a elevate prestazioni, ad esempio un'unità SSD con mirroring (SSD). I desktop virtuali in pool possono essere inseriti in archiviazione meno costosa e tradizionali, ad esempio RAID 1 + 0.

Il computer fisico deve essere posizionato su un'unità SSD, infatti la maggior parte di lettura-ho/sistema operativo dal desktop virtuali in pool passare al sistema operativo di gestione. Pertanto, lo spazio di archiviazione utilizzato dal computer fisico deve sostenere più elevato di lettura i/o al secondo.

Questa configurazione di distribuzione garantisce prestazioni economicamente conveniente in cui le prestazioni sono necessarie. L'unità SSD offre prestazioni più elevate su un disco di dimensioni più piccolo (circa 20 GB per ogni raccolta, a seconda della configurazione). Archiviazione tradizionale per desktop virtuali in pool (RAID 1 + 0) usa circa 3 GB per ogni macchina virtuale.

### <a name="csv-cache"></a>Cache CSV

Clustering di failover in Windows Server 2012 e versioni successive offre la memorizzazione nella cache in volumi condivisi Cluster (CSV). Questo è particolarmente vantaggioso per insiemi di desktop virtuali in pool in cui la maggior parte della lettura i/o forniti dal sistema operativo di gestione. La cache CSV offre prestazioni più elevate per diversi ordini di grandezza poiché vengono memorizzati nella cache i blocchi che vengono letti più volte e li recapita dalla memoria di sistema, riducendo il / o. Per altre informazioni sulla cache CSV, vedere [come abilitare la Cache CSV](http://blogs.msdn.com/b/clustering/archive/2012/03/22/10286676.aspx).

### <a name="pooled-virtual-desktops"></a>Desktop virtuali in pool

Per impostazione predefinita, i desktop virtuali in pool rollback allo stato originario dopo che un utente si disconnette, pertanto tutte le modifiche apportate al sistema operativo Windows dopo l'ultimo accesso dell'utente venga abbandonato.

Sebbene sia possibile disabilitare il rollback, è comunque una condizione temporanea perché in genere un insieme di desktop virtuali in pool è stato nuovamente creato a causa di vari aggiornamenti al modello di desktop virtuale.

È opportuno disattivare la funzionalità di Windows e i servizi che dipendono da uno stato persistente. Inoltre, è opportuno disattivare i servizi che fanno principalmente per gli scenari non aziendali.

Ogni specifico servizio deve essere valutato in modo appropriato prima di eventuali distribuzioni di grandi dimensioni. Di seguito sono alcuni aspetti iniziali da prendere in considerazione:

| Servizio                                      | Perché?                                                                                                                                                                                                      |
|----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Aggiornamento automatico                                  | Desktop virtuali in pool vengono aggiornate per ricreare il modello di desktop virtuale.                                                                                                                          |
| File offline                                | Desktop virtuali sono sempre online e connessi da un rete punto di vista.                                                                                                                         |
| Deframmentazione in background                            | Le modifiche del file system vengono eliminate dopo che un utente esegue l'accesso (a causa di un rollback per lo stato originario o ricreare il modello di desktop virtuale, con conseguente ricreazione in tutti i pool di desktop virtuali). |
| Stato di ibernazione o va in sospensione                           | Nessun tale concetto per VDI                                                                                                                                                                                   |
| Immagine della memoria verifica bug                        | Nessun tale concetto per desktop virtuali in pool. Verrà avviato un desktop virtuale in pool controllo bug dallo stato originario.                                                                                       |
| Configurazione automatica WLAN                              | Non è disponibile alcuna interfaccia di dispositivo Wi-Fi per l'infrastruttura VDI                                                                                                                                                                 |
| Servizio di condivisione in rete Windows Media Player | Servizi centrati sul consumer                                                                                                                                                                                  |
| Provider gruppo home                          | Servizi centrati sul consumer                                                                                                                                                                                  |
| Condivisione connessione Internet                  | Servizi centrati sul consumer                                                                                                                                                                                  |
| Media Center extended services               | Servizi centrati sul consumer                                                                                                                                                                                  |
> [!NOTE]
> Questo elenco non deve essere un elenco completo, in quanto tutte le modifiche riguardano gli obiettivi previsti e scenari. Per altre informazioni, vedi [frequente off pressioni, scaricarlo ora, lo script di ottimizzazione di Windows 8 VDI, per conoscenza del PFE!](http://blogs.technet.com/b/jeff_stokes/archive/2013/04/09/hot-off-the-presses-get-it-now-the-windows-8-vdi-optimization-script-courtesy-of-pfe.aspx).

 
> [!NOTE]
> SuperFetch in Windows 8 è abilitato per impostazione predefinita. È in grado di riconoscere un'infrastruttura VDI e non deve essere disabilitata. SuperFetch è possibile ridurre ulteriormente il consumo di memoria tramite la condivisione delle pagine di memoria, che è utile per l'infrastruttura VDI. Desktop virtuali in pool che eseguono Windows 7, SuperFetch deve essere disabilitata, ma per desktop personali virtuali che eseguono Windows 7, deve essere lasciato connesso.

 
