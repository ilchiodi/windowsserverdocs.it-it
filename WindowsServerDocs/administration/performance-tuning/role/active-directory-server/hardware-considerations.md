---
title: Considerazioni sull'hardware nell'ottimizzazione delle prestazioni di Active Directory
description: Considerazioni sull'hardware nell'ottimizzazione delle prestazioni di Active Directory
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: timwi; chrisrob; herbertm; kenbrumf;  mleary; shawnrab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: c40faca06668adf6fd29a5e4e753e5790b8104b7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851914"
---
# <a name="hardware-considerations-in-adds-performance-tuning"></a>Considerazioni sull'hardware in aggiunta dell'ottimizzazione delle prestazioni 

>[!Important]
> Di seguito è riportato un riepilogo delle indicazioni e delle considerazioni principali per ottimizzare l'hardware del server per i carichi di lavoro Active Directory analizzati in modo più approfondito nella [pianificazione della capacità per Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) articolo. I lettori sono vivamente invitati a esaminare la [pianificazione della capacità per Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) per una maggiore comprensione tecnica e le implicazioni di questi consigli.

## <a name="avoid-going-to-disk"></a>Evitare di passare al disco

Active Directory memorizza nella cache la maggior parte del database consentito dalla memoria. Il recupero di pagine dalla memoria è più veloce di un ordine di grandezza rispetto al supporto fisico, indipendentemente dal fatto che il supporto sia basato su mandrino o unità SSD. Aggiungere ulteriore memoria per ridurre al minimo l'I/O del disco.

-   Active Directory procedure consigliate consiglia di inserire RAM sufficiente per caricare l'intero DIT in memoria, oltre a contenere il sistema operativo e altre applicazioni installate, ad esempio antivirus, software di backup, monitoraggio e così via.

    -   Per le limitazioni delle piattaforme legacy, vedere [utilizzo della memoria da parte del processo Lsass. exe nei controller di dominio che eseguono Windows server 2003 o windows 2000 Server](https://support.microsoft.com/kb/308356).

    -   Usare la memoria\\durata media della cache di standby a lungo termine &gt; contatore delle prestazioni di 30 minuti.

-   Inserire il sistema operativo, i log e il database in volumi separati. Se tutte o la maggior parte del DIT possono essere memorizzate nella cache, una volta che la cache viene riscaldata e in uno stato stabile, questo diventa meno pertinente e offre una maggiore flessibilità nel layout di archiviazione. Negli scenari in cui l'intero DIT non può essere memorizzato nella cache, l'importanza di suddividere il sistema operativo, i log e il database su volumi distinti diventa più importante.

-   In genere, I rapporti di I/O per il DIT sono circa il 90% di lettura e il 10% di scrittura. Gli scenari in cui I volumi di I/O di scrittura superano significativamente il 10%-20% sono considerati pesanti da scrittura. Gli scenari con gravi Scritture non traggono vantaggio dalla cache Active Directory. Per garantire la durabilità delle transazioni dei dati scritti nella directory, Active Directory non esegue la memorizzazione nella cache di scrittura del disco. Al contrario, esegue il commit di tutte le operazioni di scrittura sul disco prima di restituire uno stato di completamento corretto per un'operazione, a meno che non esista una richiesta esplicita di non eseguire questa operazione. Pertanto, le operazioni di I/O su disco veloci sono importanti per le prestazioni delle operazioni di scrittura in Active Directory. Di seguito sono riportate le indicazioni hardware che potrebbero migliorare le prestazioni di questi scenari:

    -   Controller RAID hardware

    -   Aumentare il numero di dischi a bassa latenza e ad alta RPM che ospitano i file di log e DIT

    -   Caching in scrittura nel controller

-   Esaminare le prestazioni del sottosistema del disco singolarmente per ogni volume. La maggior parte degli scenari di Active Directory è prevalentemente basata su Read, quindi le statistiche sul volume che ospita il DIT sono le più importanti da ispezionare. Tuttavia, non trascurare il monitoraggio delle altre unità, incluse le unità del sistema operativo e dei file di log. Per determinare se il controller di dominio è configurato in modo appropriato per evitare che lo spazio di archiviazione sia il collo di bottiglia per le prestazioni, fare riferimento alla sezione sui sottosistemi di archiviazione per i consigli di archiviazione standard. In molti ambienti, la filosofia consiste nel garantire che lo spazio disponibile sia sufficiente per gestire picchi o picchi di carico. Queste soglie sono soglie di avviso in cui la stanza principale per gestire i picchi o i picchi di carico diventa vincolata e la velocità di risposta del client peggiora. In breve, il superamento di queste soglie non è valido a breve termine (da 5 a 15 minuti per alcune volte al giorno), tuttavia un sistema in esecuzione con questi tipi di statistiche non memorizza nella cache completamente il database e può essere sottoposto a tassazione e deve essere analizzato.

    -   Database = = istanze&gt; (Lsass/NTDSa)\\latenza media delle letture del database di I/O &lt; 15ms

    -   Database = = istanze&gt; (Lsass/NTDSa)\\letture database I/O/sec &lt; 10

    -   Database = = istanze&gt; (Lsass/NTDSa)\\la latenza media delle Scritture del log di I/O &lt; 10 ms

    -   Database = = istanze&gt; (Lsass/NTDSa)\\Scritture log I/O/sec-solo informativo.

        Per mantenere la coerenza dei dati, tutte le modifiche devono essere scritte nel log. Qui non esiste alcun numero valido o non valido, ma solo una misura del supporto per l'archiviazione.

-   Pianificare I carichi di I/O su disco non core, ad esempio analisi di backup e antivirus, per i periodi di carico non di punta. Usare anche soluzioni di backup e antivirus che supportano la funzionalità di I/O con priorità bassa introdotta in Windows Server 2008 per ridurre la concorrenza con le esigenze di I/O dei Active Directory.

## <a name="dont-over-tax-the-processors"></a>Non sovratassare i processori

I processori che non dispongono di un numero sufficiente di cicli disponibili possono causare tempi di attesa lunghi per il recupero dei thread al processore per l'esecuzione. In molti ambienti, la filosofia consiste nel garantire che lo spazio disponibile sia sufficiente per gestire picchi o picchi di carico per ridurre al minimo l'effetto sulla velocità di risposta dei client in questi scenari. In breve, il superamento delle soglie indicate di seguito non è valido a breve termine (da 5 a 15 minuti per alcune volte al giorno), tuttavia un sistema in esecuzione con questi tipi di statistiche non fornisce alcuna stanza principale per gestire carichi anomali e può essere facilmente inserito in uno scenario con tassazione eccessiva. Per la riduzione dei carichi del processore, è necessario esaminare i sistemi che passano oltre le soglie.

-   Per ulteriori informazioni su come selezionare un processore, vedere [ottimizzazione delle prestazioni per l'hardware del server](../../hardware/index.md).

-   Aggiungere hardware, ottimizzare il carico, indirizzare i client altrove o rimuovere il carico dall'ambiente per ridurre il carico della CPU.

-   Usare le informazioni sul processore (\_Total)\\% di utilizzo del processore &lt; 60% del contatore delle prestazioni.

## <a name="avoid-overloading-the-network-adapter"></a>Evitare l'overload della scheda di rete

Analogamente a quanto avviene con i processori, un utilizzo eccessivo delle schede di rete provocherà tempi di attesa lunghi per il traffico in uscita verso la rete. Active Directory tende a avere richieste in ingresso piccole e relativamente a quantità significativamente maggiori di dati restituiti ai sistemi client. I dati inviati superano i dati ricevuti. In molti ambienti, la filosofia consiste nel garantire che lo spazio disponibile sia sufficiente per gestire picchi o picchi di carico. Questa soglia è una soglia di avviso in cui la stanza principale per gestire i picchi o i picchi di carico diventa vincolata e la velocità di risposta del client diminuisce. In breve, il superamento di queste soglie non è valido a breve termine (da 5 a 15 minuti per alcune volte al giorno), tuttavia un sistema in esecuzione con questi tipi di statistiche viene sottoposto a tassazione ed è necessario esaminarlo.

-   Per ulteriori informazioni su come ottimizzare il sottosistema di rete, vedere [ottimizzazione delle prestazioni per i sottosistemi di rete](../../../../networking/technologies/network-subsystem/net-sub-performance-top.md).

-   Usare il contatore delle prestazioni confronta interfaccia (\*)\\byte inviati/sec con interfaccia (\*)\\larghezza di banda corrente. Il rapporto deve essere inferiore al 60% utilizzato.

## <a name="see-also"></a>Vedere anche
- [Ottimizzazione delle prestazioni di Active Directory Server](index.md)
- [Considerazioni relative a LDAP](ldap-considerations.md)
- [Esatto posizionamento dei controller di dominio e considerazioni sul sito](site-definition-considerations.md)
- [Risoluzione dei problemi delle prestazioni di Active Directory Domain Services](troubleshoot.md) 
- [Pianificazione della capacità per Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566)