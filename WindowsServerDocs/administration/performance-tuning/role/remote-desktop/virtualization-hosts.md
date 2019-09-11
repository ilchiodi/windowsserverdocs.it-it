---
title: Ottimizzazione delle prestazioni Desktop remoto host di virtualizzazione
description: Ottimizzazione delle prestazioni per Desktop remoto host di virtualizzazione
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 24e3243d4e9791c8941729d396e0a96cd8b11a7d
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866436"
---
# <a name="performance-tuning-remote-desktop-virtualization-hosts"></a>Ottimizzazione delle prestazioni Desktop remoto host di virtualizzazione


Host di virtualizzazione Desktop remoto (host di virtualizzazione Desktop remoto) è un servizio ruolo che supporta scenari VDI (Virtual Desktop Infrastructure) e consente a più utenti simultanei di eseguire applicazioni basate su Windows in macchine virtuali ospitate in un server che esegue Windows Server 2016 e Hyper-V.

Windows Server 2016 supporta due tipi di desktop virtuali, desktop virtuali personali e desktop virtuali in pool.

**Contenuto dell'argomento:**

-   [Considerazioni generali](#general-considerations)

-   [Ottimizzazioni delle prestazioni](#performance-optimizations)

## <a name="general-considerations"></a>Considerazioni generali


### <a name="storage"></a>Archiviazione

L'archiviazione è il collo di bottiglia delle prestazioni più probabile ed è importante ridimensionare lo spazio di archiviazione per gestire correttamente il carico di I/O generato dalle modifiche dello stato della macchina virtuale. Se un progetto pilota o una simulazione non è fattibile, è opportuno eseguire il provisioning di un mandrino del disco per quattro macchine virtuali attive. Usare le configurazioni del disco con prestazioni di scrittura ottimali, ad esempio RAID 1 + 0.

Quando appropriato, usare la deduplicazione del disco e la memorizzazione nella cache per ridurre il carico di lettura del disco e consentire alla soluzione di archiviazione di velocizzare le prestazioni memorizzando nella cache una parte significativa dell'immagine.

### <a name="data-deduplication-and-vdi"></a>Deduplicazione dati e VDI

Introdotta in Windows Server 2012 R2, deduplicazione dati supporta l'ottimizzazione dei file aperti. Per utilizzare le macchine virtuali in esecuzione in un volume deduplicato, è necessario archiviare i file della macchina virtuale in un host separato dall'host Hyper-V. Se Hyper-V e la deduplicazione sono in esecuzione nello stesso computer, le due funzionalità si contendono per le risorse di sistema e influiscono negativamente sulle prestazioni complessive.

Il volume deve essere configurato anche per usare il tipo di ottimizzazione della deduplicazione "Virtual Desktop Infrastructure (VDI)". Per configurarlo, è possibile usare Server Manager ( **deduplicazione**(**file and Storage Services**  - &gt; &gt; **Volumes**  - Settings) o usando il comando di Windows PowerShell seguente:

``` syntax
Enable-DedupVolume <volume> -UsageType HyperV
```

> [!NOTE]
> L'ottimizzazione della deduplicazione dei file aperti è supportata solo per scenari VDI con Hyper-V usando l'archiviazione remota tramite SMB 3,0.

### <a name="memory"></a>Memoria

L'utilizzo della memoria del server è determinato da tre fattori principali:

-   Overhead del sistema operativo

-   Overhead servizio Hyper-V per macchina virtuale

-   Memoria allocata a ogni macchina virtuale

Per un carico di lavoro tipico knowledge worker, le macchine virtuali guest che eseguono x86 Window 8 o Windows 8.1 devono essere concesse ~ 512 MB di memoria come base. Tuttavia, memoria dinamica aumenterà probabilmente la memoria della macchina virtuale Guest a circa 800 MB, a seconda del carico di lavoro. Per x64, si verificano circa 800 MB di avvio, aumentando a 1024 MB.

Pertanto, è importante fornire memoria del server sufficiente per soddisfare la memoria richiesta dal numero previsto di macchine virtuali guest, oltre a consentire una quantità sufficiente di memoria per il server.

### <a name="cpu"></a>CPU

Quando si pianifica la capacità del server per un server host di virtualizzazione Desktop remoto, il numero di macchine virtuali per core fisico dipende dalla natura del carico di lavoro. Come punto di partenza, è ragionevole pianificare 12 macchine virtuali per core fisico e quindi eseguire gli scenari appropriati per convalidare le prestazioni e la densità. Una densità più elevata può essere realizzabile a seconda delle specifiche del carico di lavoro.

È consigliabile abilitare la tecnologia Hyper-Threading, ma assicurarsi di calcolare il rapporto di oversubscription in base al numero di core fisici e non al numero di processori logici. In questo modo si garantisce il livello di prestazioni previsto in base alla CPU.

### <a name="virtual-gpu"></a>GPU virtuale

Microsoft RemoteFX per l'host di virtualizzazione Desktop remoto offre un'esperienza grafica avanzata per Virtual Desktop Infrastructure (VDI) tramite la comunicazione remota lato host, una pipeline di rendering-acquisizione-codifica, una codifica basata su GPU altamente efficiente e una limitazione basata sul client e una GPU virtuale abilitata per DirectX. RemoteFX per Host di virtualizzazione Desktop remoto aggiorna la GPU virtuale da DirectX9 a DirectX11. Consente inoltre di migliorare l'esperienza utente supportando più monitoraggi con risoluzioni più elevate.

L'esperienza DirectX11 di RemoteFX è disponibile senza una GPU hardware, tramite un driver emulato da software. Anche se questa GPU software offre un'esperienza ottimale, l'unità di elaborazione grafica virtuale RemoteFX (VGPU) aggiunge un'esperienza accelerata hardware ai desktop virtuali.

Per sfruttare i vantaggi dell'esperienza RemoteFX VGPU in un server che esegue Windows Server 2016, è necessario un driver GPU, ad esempio DirectX 11.1 o WDDM 1,2, nel server host. Per ulteriori informazioni sulle offerte GPU da utilizzare con RemoteFX per Host di virtualizzazione Desktop remoto, contattare il provider GPU.

Se si usa la GPU virtuale RemoteFX nella distribuzione VDI, la capacità di distribuzione varia in base agli scenari di utilizzo e alla configurazione hardware. Quando si pianifica la distribuzione, tenere presente quanto segue:

-   Numero di GPU nel sistema

-   Capacità di memoria video sulle GPU

-   Risorse del processore e hardware del sistema

### <a name="remotefx-server-system-memory"></a>Memoria di sistema del server RemoteFX

Per ogni desktop virtuale abilitato con una GPU virtuale, RemoteFX usa la memoria di sistema nel sistema operativo guest e nel server abilitato per RemoteFX. L'hypervisor garantisce la disponibilità della memoria di sistema per un sistema operativo guest. Sul server, ogni desktop virtuale abilitato alla GPU virtuale deve annunciare il requisito relativo alla memoria di sistema per l'hypervisor. Quando viene avviato il desktop virtuale abilitato alla GPU virtuale, l'hypervisor riserva memoria di sistema aggiuntiva nel server abilitato per RemoteFX per il desktop virtuale abilitato per VGPU.

Il requisito di memoria per il server abilitato per RemoteFX è dinamico perché la quantità di memoria utilizzata nel server abilitato per RemoteFX dipende dal numero di monitoraggi associati ai desktop virtuali abilitati per VGPU e dalla risoluzione massima per questi monitoraggi.

### <a name="remotefx-server-gpu-video-memory"></a>Memoria video GPU del server RemoteFX

Ogni desktop virtuale abilitato alla GPU virtuale usa la memoria video nell'hardware della GPU sul server host per eseguire il rendering del desktop. Oltre al rendering, la memoria video viene utilizzata da un codec per comprimere la schermata di cui è stato eseguito il rendering. La quantità di memoria necessaria è direttamente basata sulla quantità di monitoraggi di cui è stato effettuato il provisioning nella macchina virtuale.

La memoria video riservata varia a seconda del numero di monitoraggi e della risoluzione dello schermo di sistema. Alcuni utenti potrebbero richiedere una risoluzione dello schermo superiore per le attività specifiche. Se tutte le altre impostazioni rimangono costanti, esiste una maggiore scalabilità con impostazioni di risoluzione inferiori.

### <a name="remotefx-processor"></a>Processore RemoteFX

L'hypervisor Pianifica il server abilitato per RemoteFX e i desktop virtuali abilitati per la GPU virtuale sulla CPU. A differenza della memoria di sistema, non sono disponibili informazioni correlate a risorse aggiuntive che RemoteFX deve condividere con l'hypervisor. Il sovraccarico della CPU aggiuntivo che RemoteFX porta al desktop virtuale abilitato per GPU è correlato all'esecuzione del driver della GPU virtuale e di uno stack di Remote Desktop Protocol in modalità utente.

Sul server abilitato per RemoteFX, l'overhead è aumentato, perché il sistema esegue un processo aggiuntivo (Rdvgm. exe) per ogni desktop virtuale abilitato per la GPU virtuale. Questo processo usa il driver di dispositivo grafico per eseguire i comandi nella GPU. Il codec usa anche le CPU per comprimere i dati della schermata che devono essere inviati al client.

Un maggior numero di processori virtuali significa un'esperienza utente migliore. Si consiglia di allocare almeno due CPU virtuali per desktop virtuale virtuale abilitato per GPU. Si consiglia inoltre di utilizzare l'architettura x64 per i desktop virtuali virtuali abilitati per GPU, perché le prestazioni sulle macchine virtuali x64 sono migliori rispetto alle macchine virtuali x86.

### <a name="remotefx-gpu-processing-power"></a>Potenza di elaborazione GPU RemoteFX

Per ogni desktop virtuale abilitato per GPU virtuale è presente un processo DirectX corrispondente in esecuzione nel server abilitato per RemoteFX. Questo processo riproduce tutti i comandi di grafica ricevuti dal desktop virtuale RemoteFX sulla GPU fisica. Per la GPU fisica, è equivalente all'esecuzione simultanea di più applicazioni DirectX.

In genere, i dispositivi e i driver grafici sono ottimizzati per eseguire alcune applicazioni sul desktop. RemoteFX estende le GPU da usare in modo univoco. Per misurare la modalità di esecuzione della GPU in un server RemoteFX, sono stati aggiunti contatori delle prestazioni per misurare la risposta GPU alle richieste RemoteFX.

In genere, quando una risorsa GPU è insufficiente, le operazioni di lettura e scrittura sulla GPU richiede molto tempo. Utilizzando i contatori delle prestazioni, gli amministratori possono intraprendere azioni preventive, eliminando la possibilità di tempi di inattività per gli utenti finali.

I contatori delle prestazioni seguenti sono disponibili nel server RemoteFX per misurare le prestazioni della GPU virtuale:

**Grafica di RemoteFX**

-   **Frame ignorati/secondo-risorse client insufficienti** Numero di frame ignorati al secondo a causa di risorse client insufficienti

-   **Rapporto di compressione grafica** Rapporto tra il numero di byte codificato e il numero di byte di input

**Gestione GPU radice RemoteFX**

-   **Risorse TDRS in GPU** server numero totale di volte in cui il TDR scade nella GPU sul server

-   **Risorse Macchine virtuali che eseguono** il RemoteFX numero totale di macchine virtuali in cui è installata la scheda video RemoteFX 3D

-   **VRAM MB disponibili per GPU** quantità di memoria video dedicata non utilizzata

-   **VRAM Percentuale riservata di memoria** video riservata per GPU, riservata per RemoteFX

**Software RemoteFX**

-   **Velocità di acquisizione per il monitoraggio** \[1-4\] Visualizza la velocità di acquisizione RemoteFX per i monitoraggi 1-4

-   **Rapporto di compressione** Deprecato in Windows 8 e sostituito dal **rapporto di compressione della grafica**

-   **Frame ritardati/sec** Numero di fotogrammi al secondo in cui i dati grafici non sono stati inviati entro un determinato periodo di tempo

-   **Tempo di risposta GPU dall'acquisizione** Latenza misurata all'interno dell'acquisizione RemoteFX (in microsecondi) per il completamento delle operazioni GPU

-   **Tempo di risposta GPU dal rendering** Latenza misurata entro il rendering di RemoteFX (in microsecondi) per il completamento delle operazioni GPU

-   **Byte di output** Numero totale di byte di output RemoteFX

-   **In attesa del numero di client/sec** Deprecato in Windows 8 e sostituito da **frame ignorati/secondo-risorse client insufficienti**

**Gestione RemoteFX vGPU**

-   **Risorse TDRS da locale a macchina** virtuale numero totale di TDRS che si sono verificati in questa macchina virtuale (TDRS che il server propagato alle macchine virtuali non è incluso)

-   **Risorse TDRS propagata in** base al numero totale di TDRS del server che si sono verificati nel server e che sono stati propagati alla macchina virtuale

**Prestazioni vGPU della macchina virtuale RemoteFX**

-   **Dati Numero totale di operazioni** richiamate/sec (in secondi) per il rendering sul desktop della macchina virtuale al secondo

-   **Dati Numero totale di operazioni correnti** inviate dalla macchina virtuale alla GPU del server al secondo in uscita/sec

-   **Dati Byte letti/sec** numero totale di byte letti dal server abilitato per RemoteFX al secondo

-   **Dati**  Numero totale di byte inviati alla GPU del server abilitata per RemoteFX al secondo

-   **DMA Latenza media buffer di comunicazione (sec** ) tempo medio (in secondi) impiegato nei buffer di comunicazione

-   **DMA Latenza buffer DMA (sec** ) di tempo (in secondi) da quando il DMA viene inviato fino al completamento

-   **DMA Lunghezza coda** DMA lunghezza coda per una scheda video RemoteFX 3D

-   **Risorse Timeout di TDR per numero** di GPU di timeout di TDR che si sono verificati per GPU nella macchina virtuale

-   **Risorse Timeout TDR per numero di motori** GPU di timeout TDR verificatisi per ogni motore GPU nella macchina virtuale

Oltre ai contatori delle prestazioni della GPU virtuale RemoteFX, è anche possibile misurare l'utilizzo della GPU usando Esplora processi, che mostra l'utilizzo della memoria video e l'utilizzo della GPU.

## <a name="performance-optimizations"></a>Ottimizzazioni delle prestazioni

### <a name="dynamic-memory"></a>Memoria dinamica

Memoria dinamica consente un utilizzo più efficiente delle risorse di memoria del server che esegue Hyper-V, bilanciando il modo in cui viene distribuita la memoria tra le macchine virtuali in esecuzione. La memoria può essere riallocata dinamicamente tra macchine virtuali in risposta ai carichi di lavoro in corso di modifica.

Memoria dinamica consente di aumentare la densità della macchina virtuale con le risorse già disponibili senza sacrificare le prestazioni o la scalabilità. Il risultato è un uso più efficiente delle risorse hardware del server costose, che possono tradursi in una gestione più semplice e in costi ridotti.

Nei sistemi operativi guest che eseguono Windows 8 e versioni successive con processori virtuali che si estendono su più processori logici, valutare il compromesso tra l'esecuzione con memoria dinamica per ridurre al minimo l'utilizzo della memoria e la disabilitazione memoria dinamica per migliorare le prestazioni di un'applicazione compatibile con la topologia del computer. Tale applicazione può sfruttare le informazioni sulla topologia per prendere decisioni relative alla pianificazione e all'allocazione della memoria.

### <a name="tiered-storage"></a>Archiviazione a livelli

Host di virtualizzazione Desktop remoto supporta l'archiviazione a livelli per i pool di desktop virtuali. Il computer fisico condiviso da tutti i desktop virtuali in pool in una raccolta può utilizzare una soluzione di archiviazione a prestazioni elevate di dimensioni ridotte, ad esempio un'unità SSD (Solid-State Drive) con mirroring. I desktop virtuali in pool possono essere posizionati in una risorsa di archiviazione tradizionale meno costosa, ad esempio RAID 1 + 0.

Il computer fisico deve essere inserito in un'unità SSD perché la maggior parte delle letture i/o da desktop virtuali in pool passa al sistema operativo di gestione. Pertanto, lo spazio di archiviazione utilizzato dal computer fisico deve sostenere un numero maggiore di operazioni di I/o di lettura al secondo.

Questa configurazione di distribuzione assicura prestazioni convenienti in cui le prestazioni sono necessarie. L'unità SSD fornisce prestazioni più elevate su un disco di dimensioni inferiori (circa 20 GB per raccolta, a seconda della configurazione). L'archiviazione tradizionale per i desktop virtuali in pool (RAID 1 + 0) USA circa 3 GB per macchina virtuale.

### <a name="csv-cache"></a>Cache CSV

Il clustering di failover in Windows Server 2012 e versioni successive consente di memorizzare nella cache i volumi condivisi del cluster (CSV). Questa operazione è estremamente vantaggiosa per gli insiemi di desktop virtuali in pool in cui la maggior parte delle i/o di lettura proviene dal sistema operativo di gestione. La cache CSV fornisce prestazioni più elevate in base a diversi ordini di grandezza, perché memorizza nella cache i blocchi che vengono letti più di una volta e li recapita dalla memoria di sistema, riducendo l'i/O. Per ulteriori informazioni sulla cache CSV, vedere [come abilitare la cache CSV](http://blogs.msdn.com/b/clustering/archive/2012/03/22/10286676.aspx).

### <a name="pooled-virtual-desktops"></a>Desktop virtuali in pool

Per impostazione predefinita, i desktop virtuali in pool vengono ripristinati allo stato incontaminato dopo che un utente si è disconnesso, quindi le modifiche apportate al sistema operativo Windows dall'ultimo accesso dell'utente vengono interrotte.

Sebbene sia possibile disabilitare il rollback, si tratta comunque di una condizione temporanea poiché in genere un insieme di desktop virtuali in pool viene ricreato a causa di vari aggiornamenti del modello di desktop virtuale.

È opportuno disattivare le funzionalità e i servizi di Windows che dipendono dallo stato persistente. Inoltre, è opportuno disabilitare i servizi che sono principalmente per gli scenari non aziendali.

Ogni servizio specifico deve essere valutato in modo appropriato prima di qualsiasi distribuzione estesa. Di seguito sono riportate alcune considerazioni iniziali da considerare:

| Service                                      | Perché?                                                                                                                                                                                                      |
|----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Aggiornamento automatico                                  | I desktop virtuali in pool vengono aggiornati creando nuovamente il modello di desktop virtuale.                                                                                                                          |
| File offline                                | I desktop virtuali sono sempre online e connessi da un punto di vista della rete.                                                                                                                         |
| Defrag in background                            | Le modifiche del file System vengono ignorate dopo l'accesso dell'utente (a causa di un rollback allo stato incontaminato o la ricreazione del modello di desktop virtuale, il che comporta la ricreazione di tutti i desktop virtuali in pool). |
| Ibernazione o sospensione                           | Nessun concetto di questo tipo per VDI                                                                                                                                                                                   |
| Dump memoria controllo bug                        | Nessun concetto di questo tipo per i desktop virtuali in pool. Uno stato incontaminato per il controllo dei bug.                                                                                       |
| Configurazione automatica WLAN                              | Non è disponibile alcuna interfaccia del dispositivo WiFi per VDI                                                                                                                                                                 |
| Servizio di condivisione di rete Media Player Windows | Servizio incentrato sul consumer                                                                                                                                                                                  |
| Provider Gruppo Home                          | Servizio incentrato sul consumer                                                                                                                                                                                  |
| Condivisione connessione Internet                  | Servizio incentrato sul consumer                                                                                                                                                                                  |
| Servizi estesi di Media Center               | Servizio incentrato sul consumer                                                                                                                                                                                  |
> [!NOTE]
> Questo elenco non è destinato a essere un elenco completo, perché le modifiche avranno effetto sugli obiettivi e sugli scenari previsti. Per altre informazioni, vedere la pagina relativa alle [pressioni per le presse, ottenere ora, lo script di ottimizzazione di Windows 8 VDI, cortesia di PFE!](http://blogs.technet.com/b/jeff_stokes/archive/2013/04/09/hot-off-the-presses-get-it-now-the-windows-8-vdi-optimization-script-courtesy-of-pfe.aspx).

 
> [!NOTE]
> SuperFetch in Windows 8 è abilitato per impostazione predefinita. È compatibile con VDI e non deve essere disabilitato. SuperFetch può ridurre ulteriormente l'utilizzo di memoria tramite la condivisione di pagine di memoria, che risulta utile per VDI. I desktop virtuali in pool che eseguono Windows 7, SuperFetch devono essere disabilitati, ma per i desktop virtuali personali che eseguono Windows 7 deve essere lasciato acceso.

 
