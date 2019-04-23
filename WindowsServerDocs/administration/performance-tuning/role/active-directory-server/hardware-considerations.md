---
title: Considerazioni relative all'hardware nell'ottimizzazione delle prestazioni di AD
description: Considerazioni relative all'hardware nell'ottimizzazione delle prestazioni di AD
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 0f1aa1e3c07c5cb9238a332156abfec248e74176
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866092"
---
# <a name="hardware-considerations-in-adds-performance-tuning"></a>Considerazioni relative all'hardware nell'ottimizzazione delle prestazioni di ADDS 

>[!Important]
> Di seguito è riportato un riepilogo dei principali requisiti e considerazioni per ottimizzare l'hardware del server per i carichi di lavoro di Active Directory trattati in modo più approfondito nel [pianificazione della capacità per Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) articolo. I lettori sono altamente consigliabile rivedere [pianificazione della capacità per Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) per una maggiore conoscenza tecnica e le implicazioni di queste indicazioni.

## <a name="avoid-going-to-disk"></a>Evitare di entrare al disco

Active Directory memorizza nella cache la maggior parte del database, poiché consente di memoria. Durante il recupero le pagine dalla memoria sono di gran lunga più veloce rispetto all'uso a supporto fisico, se il supporto è spindle o basato su unità SSD. Aggiungere ulteriore memoria per ridurre al minimo i/o disco.

-   Procedure consigliate di Active Directory è consigliabile inserire RAM sufficiente per caricare l'intero DIT in memoria, oltre a soddisfare le esigenze del sistema operativo e altre applicazioni installate, ad esempio software antivirus, backup, monitoraggio e così via.

    -   Per le limitazioni delle piattaforme legacy, vedere [utilizzo di memoria di processo Lsass.exe nei controller di dominio che eseguono Windows Server 2003 o Windows 2000 Server](https://support.microsoft.com/kb/308356).

    -   Usare la memoria\\Long-Term durata media di Standby della Cache (s) &gt; contatore delle prestazioni di 30 minuti.

-   Inserire il sistema operativo, i log e il database su volumi separati. Se tutti o la maggior parte del DIT può essere memorizzati nella cache, dopo che la cache viene preparata e in uno stato stabile, questo diventa meno rilevante e offre una maggiore flessibilità nel layout di archiviazione. Negli scenari in cui non può essere memorizzato nella cache l'intero DIT, diventa più importante l'importanza di suddividere il sistema operativo, i log e database su volumi separati.

-   Rapporti dei / o per il DIT in genere, sono circa 90% di lettura e scrittura del 10%. Gli scenari in cui i volumi i/o scrittura superano significativamente 10-20% vengono considerati con intensa attività di scrittura. Gli scenari con intensa attività di scrittura non trarre vantaggio dalla cache Active Directory. Per garantire la durabilità delle transazioni di dati che viene scritto nella directory, Active Directory non esegue la memorizzazione nella cache di scrittura del disco. Al contrario, esegue il commit tutte le operazioni di scrittura sul disco prima che venga restituito uno stato di completamento corretto per un'operazione, a meno che non vi è una richiesta esplicita di non eseguire questa operazione. I/o veloci disco sono pertanto importanti per le prestazioni delle operazioni di scrittura Active Directory. Di seguito sono i requisiti hardware che potrebbero migliorare le prestazioni per questi scenari:

    -   Controller RAID hardware

    -   Aumentare il numero di bassa latenza/elevata-da RPM dischi che ospitano i file DIT e log

    -   Scrittura nella cache nel controller

-   Esaminare le prestazioni del sottosistema disco singolarmente per ogni volume. La maggior parte degli scenari di Active Directory sono prevalentemente basati su lettura, pertanto le statistiche sul volume che ospita il DIT sono quelli più importanti da esaminare. Tuttavia, non trascurare il resto delle unità, tra cui il sistema operativo, di monitoraggio e le unità di file di log. Per determinare se il controller di dominio è configurato correttamente per evitare l'archiviazione in corso il collo di bottiglia delle prestazioni, fare riferimento la sezione su sottosistemi di archiviazione, per indicazioni sull'archiviazione standard. In molti ambienti, la filosofia consiste nell'assicurarsi che vi sia spazio sufficiente head per adeguarla crescite o picchi di carico. Queste soglie vengono avviso diventa vincolata soglie in cui di spazio disponibile per adeguarla crescite o picchi di carico e comporta una riduzione della velocità di risposta di client. In breve, che superano queste soglie non è valido a breve termine (5-15 minuti più volte al giorno), tuttavia un sistema che esegue sostenuta questi tipi di statistiche è non completamente la memorizzazione nella cache del database e può essere rispetto a tassare e dovrebbero essere esaminati.

    -   Database = =&gt; Instances(lsass/NTDSA)\\latenza media di letture i/o Database &lt; gt;15 ms

    -   Database = =&gt; Instances(lsass/NTDSA)\\letture/sec i/o Database &lt; 10

    -   Database = =&gt; Instances(lsass/NTDSA)\\latenza media di scritture nei Log dei / o &lt; 10 ms

    -   Database = =&gt; Instances(lsass/NTDSA)\\Log dei / o. scritture/sec: informativo solo.

        Per mantenere la coerenza dei dati, tutte le modifiche devono essere scritte nel log. Nessun numero valido o non valido in questo caso, è solo una misura della quantità di archiviazione è stato raggiunto.

-   Pianificare i carichi dei / o disco non core, ad esempio le analisi antivirus e backup, per i periodi di carico non di punta. Inoltre, usare soluzioni di backup e antivirus che supportano la funzionalità dei / o con priorità bassa introdotta in Windows Server 2008 per ridurre la concorrenza con esigenze dei / o di Active Directory.

## <a name="dont-over-tax-the-processors"></a>Non imposta più processori

Processori che non dispone di cicli insufficienti possono causare tempi di attesa prolungati all'acquisizione di thread al processore per l'esecuzione. In molti ambienti, la filosofia consiste nell'assicurarsi che vi sia spazio sufficiente head per gestire picchi o picchi di carico per ridurre al minimo impatto sulla velocità di risposta di client in questi scenari. In breve, che supera di sotto delle soglie non è valido a breve termine (5-15 minuti più volte al giorno), tuttavia non fornisce un sistema che esegue sostenuta questi tipi di statistiche qualsiasi intestazione di spazio sufficiente per soddisfare carichi anomale e possono essere impostati facilmente in un failover tassare s cenario. I sistemi spesa periodi prolungati sopra alle soglie devono essere analizzati come ridurre il carico del processore.

-   Per altre informazioni su come selezionare un processore, vedi [Performance Tuning for Server Hardware](../../hardware/index.md).

-   Aggiungere hardware, ottimizzare il carico, indirizzare i client in altre posizioni o rimuovere carico dall'ambiente per ridurre il carico della CPU.

-   Usare le informazioni del processore (\_totale)\\% utilizzo processore &lt; contatore delle prestazioni del 60%.

## <a name="avoid-overloading-the-network-adapter"></a>Evitare di sovraccaricare la scheda di rete

Proprio come con i processori, utilizzo della scheda di rete è eccessiva causerà tempi di attesa per il traffico in uscita accedere alla rete. Active Directory che ne derivano sono delle piccole richieste in ingresso e relativamente a notevolmente più grandi quantità di dati restituiti per i sistemi client. Dati inviati di là dei dati ricevuti. In molti ambienti, la filosofia consiste nell'assicurarsi che vi sia spazio sufficiente head per adeguarla crescite o picchi di carico. Questa soglia è un valore di soglia di avviso in cui di spazio disponibile per adeguarla crescite o picchi di carico diventa vincolata e comporta una riduzione della velocità di risposta di client. In breve, che superano queste soglie non è valido a breve termine (5-15 minuti più volte al giorno), tuttavia un sistema che esegue sostenuta questi tipi di statistiche tramite tassare e dovrebbero essere esaminati.

-   Per altre informazioni su come ottimizzare il sottosistema di rete, vedere [ottimizzazione delle prestazioni per i sottosistemi di rete](../../../../networking/technologies/network-subsystem/net-sub-performance-top.md).

-   Usare l'interfaccia di rete confronta (\*)\\byte inviati/Sec con l'interfaccia di rete (\*)\\contatore delle prestazioni di larghezza di banda corrente. Il rapporto deve essere inferiore al 60% utilizzato.

## <a name="see-also"></a>Vedere anche
- [Server Active Directory l'ottimizzazione delle prestazioni](index.md)
- [Considerazioni di LDAP](ldap-considerations.md)
- [Esatto posizionamento dei controller di dominio e considerazioni sul sito](site-definition-considerations.md)
- [Risoluzione dei problemi delle prestazioni di ADDS](troubleshoot.md) 
- [Pianificazione della capacità per servizi di dominio Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566)