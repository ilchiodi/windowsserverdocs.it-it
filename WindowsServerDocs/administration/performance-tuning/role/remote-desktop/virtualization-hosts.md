---
title: Ottimizzazione delle prestazioni Desktop remoto host di virtualizzazione
description: Ottimizzazione delle prestazioni per Desktop remoto host di virtualizzazione
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: hammadbu; vladmis; denisgun
author: phstee
ms.date: 10/22/2019
ms.openlocfilehash: 2a0db4d890a01df13c44a9bb7adfbd13bebbdde0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851704"
---
# <a name="performance-tuning-remote-desktop-virtualization-hosts"></a>Ottimizzazione delle prestazioni Desktop remoto host di virtualizzazione

Host di virtualizzazione Desktop remoto (host di virtualizzazione Desktop remoto) è un servizio ruolo che supporta scenari VDI (Virtual Desktop Infrastructure) e consente a più utenti di eseguire applicazioni basate su Windows in macchine virtuali ospitate in un server che esegue Windows Server e Hyper-V.

Windows Server supporta due tipi di desktop virtuali: desktop virtuali personali e desktop virtuali in pool.

## <a name="general-considerations"></a>Considerazioni generali

### <a name="storage"></a>Archiviazione

L'archiviazione è il collo di bottiglia delle prestazioni più probabile ed è importante ridimensionare lo spazio di archiviazione per gestire correttamente il carico di I/O generato dalle modifiche dello stato della macchina virtuale. Se un progetto pilota o una simulazione non è fattibile, è opportuno eseguire il provisioning di un mandrino del disco per quattro macchine virtuali attive. Usare le configurazioni del disco con prestazioni di scrittura ottimali, ad esempio RAID 1 + 0.

Quando appropriato, usare la deduplicazione del disco e la memorizzazione nella cache per ridurre il carico di lettura del disco e consentire alla soluzione di archiviazione di velocizzare le prestazioni memorizzando nella cache una parte significativa dell'immagine.

### <a name="data-deduplication-and-vdi"></a>Deduplicazione dati e VDI

Introdotta in Windows Server 2012 R2, deduplicazione dati supporta l'ottimizzazione dei file aperti. Per utilizzare le macchine virtuali in esecuzione in un volume deduplicato, è necessario archiviare i file della macchina virtuale in un host separato dall'host Hyper-V. Se Hyper-V e la deduplicazione sono in esecuzione nello stesso computer, le due funzionalità si contendono per le risorse di sistema e influiscono negativamente sulle prestazioni complessive.

Il volume deve essere configurato anche per usare il tipo di ottimizzazione della deduplicazione "Virtual Desktop Infrastructure (VDI)". Per configurarlo, è possibile usare Server Manager (**Servizi file e archiviazione** -&gt; **volumi** -**Impostazioni deduplicazione**&gt;) oppure usando il comando di Windows PowerShell seguente:

``` syntax
Enable-DedupVolume <volume> -UsageType HyperV
```

> [!NOTE]
> L'ottimizzazione della deduplicazione dei file aperti è supportata solo per scenari VDI con Hyper-V usando l'archiviazione remota tramite SMB 3,0.

### <a name="memory"></a>Memoria

L'utilizzo della memoria del server è determinato da tre fattori principali:

- Overhead del sistema operativo

- Overhead servizio Hyper-V per macchina virtuale

- Memoria allocata a ogni macchina virtuale

Per un carico di lavoro tipico knowledge worker, le macchine virtuali guest che eseguono x86 Window 8 o Windows 8.1 devono essere concesse ~ 512 MB di memoria come base. Tuttavia, memoria dinamica aumenterà probabilmente la memoria della macchina virtuale Guest a circa 800 MB, a seconda del carico di lavoro. Per x64, si verificano circa 800 MB di avvio, aumentando a 1024 MB.

Pertanto, è importante fornire memoria del server sufficiente per soddisfare la memoria richiesta dal numero previsto di macchine virtuali guest, oltre a consentire una quantità sufficiente di memoria per il server.

### <a name="cpu"></a>CPU

Quando si pianifica la capacità del server per un server host di virtualizzazione Desktop remoto, il numero di macchine virtuali per core fisico dipende dalla natura del carico di lavoro. Come punto di partenza, è ragionevole pianificare 12 macchine virtuali per core fisico e quindi eseguire gli scenari appropriati per convalidare le prestazioni e la densità. Una densità più elevata può essere realizzabile a seconda delle specifiche del carico di lavoro.

È consigliabile abilitare la tecnologia Hyper-Threading, ma assicurarsi di calcolare il rapporto di oversubscription in base al numero di core fisici e non al numero di processori logici. In questo modo si garantisce il livello di prestazioni previsto in base alla CPU.

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

Il clustering di failover in Windows Server 2012 e versioni successive consente di memorizzare nella cache i volumi condivisi del cluster (CSV). Questa operazione è estremamente vantaggiosa per gli insiemi di desktop virtuali in pool in cui la maggior parte delle i/o di lettura proviene dal sistema operativo di gestione. La cache CSV fornisce prestazioni più elevate in base a diversi ordini di grandezza, perché memorizza nella cache i blocchi che vengono letti più di una volta e li recapita dalla memoria di sistema, riducendo l'i/O. Per ulteriori informazioni sulla cache CSV, vedere [come abilitare la cache CSV](https://blogs.msdn.com/b/clustering/archive/2012/03/22/10286676.aspx).

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
> Questo elenco non è destinato a essere un elenco completo, perché le modifiche avranno effetto sugli obiettivi e sugli scenari previsti. Per altre informazioni, vedere la pagina relativa alle [pressioni per le presse, ottenere ora, lo script di ottimizzazione di Windows 8 VDI, cortesia di PFE!](https://blogs.technet.com/b/jeff_stokes/archive/2013/04/09/hot-off-the-presses-get-it-now-the-windows-8-vdi-optimization-script-courtesy-of-pfe.aspx).


> [!NOTE]
> SuperFetch in Windows 8 è abilitato per impostazione predefinita. È compatibile con VDI e non deve essere disabilitato. SuperFetch può ridurre ulteriormente l'utilizzo di memoria tramite la condivisione di pagine di memoria, che risulta utile per VDI. I desktop virtuali in pool che eseguono Windows 7, SuperFetch devono essere disabilitati, ma per i desktop virtuali personali che eseguono Windows 7 deve essere lasciato acceso.
