---
title: Prestazioni dei / o di archiviazione di Hyper-V
description: Considerazioni sulle prestazioni dei / o di archiviazione nell'ottimizzazione delle prestazioni di Hyper-V
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fedc23083914bcf97a8cde12b78c0b174143de25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831312"
---
# <a name="hyper-v-storage-io-performance"></a>Prestazioni dei / o di archiviazione di Hyper-V

Questa sezione descrive diverse opzioni e considerazioni per l'ottimizzazione delle prestazioni dei / o in una macchina virtuale di archiviazione. Il percorso dei / o di archiviazione si estende dal guest sullo stack di archiviazione, a livello di virtualizzazione dell'host, lo stack di archiviazione host, quindi al disco fisico. Di seguito sono le spiegazioni su come le ottimizzazioni sono possibili in ognuna di queste fasi.

## <a name="virtual-controllers"></a>Controller virtuali

Hyper-V offre tre tipi di controller virtuali: IDE, SCSI e virtuali, schede bus host (HBA).

## <a name="ide"></a>IDE

I controller IDE espongono i dischi IDE per la macchina virtuale. Il controller IDE viene emulato, ed è l'unico controller che è disponibile per le macchine virtuali guest in esecuzione la versione precedente di Windows senza i servizi di integrazione di macchina virtuale. Disco i/o che viene eseguita usando il driver del filtro dell'IDE che viene fornito con i servizi di integrazione di macchina virtuale è significativamente migliori rispetto al disco delle prestazioni dei / o che viene fornito con il controller IDE emulato. È consigliabile che i dischi IDE essere usato solo per i dischi del sistema operativo perché presentano le limitazioni delle prestazioni a causa della dimensione dei / o massima che può essere emesso per questi dispositivi.

## <a name="scsi-sas-controller"></a>SCSI (controller di firma di accesso condiviso)

I controller SCSI espongono dischi SCSI nella macchina virtuale e ogni controller SCSI virtuale può supportare fino a 64 dispositivi. Per ottenere prestazioni ottimali, si consiglia di collegare più dischi a un singolo controller SCSI virtuali e creare ulteriori controller solo perché sono richiesti per ridimensionare il numero di dischi connessi alla macchina virtuale. Percorso SCSI non viene emulato che rende il controller preferito per qualsiasi disco diverso da quello del disco del sistema operativo. Con le macchine virtuali di generazione 2, in realtà è l'unico tipo di controller possibili. Introdotta in Windows Server 2012 R2, questo controller è segnalato come firma di accesso condiviso per il supporto VHDX condiviso.

## <a name="virtual-fibre-channel-hbas"></a>Nelle schede HBA Fibre Channel virtuale

Nelle schede HBA Fibre Channel virtuale può essere configurato per consentire l'accesso diretto per le macchine virtuali per Fibre Channel e Fibre Channel tramite LUN Ethernet (FCoE). I dischi virtuali Fibre Channel ignorano il file system NTFS nella partizione radice, che riduce l'utilizzo della CPU dei / o archiviazione.

Le unità di dati di grandi dimensioni e le unità che vengono condivisi tra più macchine virtuali (per gli scenari di clustering dei guest) sono i candidati ideali per i dischi virtuali Fibre Channel.

I dischi virtuali Fibre Channel richiedono uno o più host schede bus Fibre Channel (HBA) da installare nell'host. È necessario usare un driver HBA che supporta le funzionalità di Windows Server 2016 Virtual Fibre Channel/NPIV ogni host HBA. L'infrastruttura SAN deve supportare NPIV e le porte HBA usate per Fibre Channel virtuale devono essere configurate in una topologia Fibre Channel che supporti NPIV.

Per ottimizzare la velocità effettiva in host tramite cui vengono installati con più di una scheda HBA, si consiglia di configurare più schede HBA virtuale all'interno della macchina virtuale Hyper-V (fino a quattro schede HBA può essere configurata per ogni macchina virtuale). Hyper-V verranno automaticamente modo migliore per bilanciare le HBA virtuali per HBA host che accedono alla stessa SAN virtuale.

## <a name="virtual-disks"></a>Dischi virtuali

I dischi possono essere esposti a macchine virtuali tramite il controller virtuali. Questi dischi potrebbe essere dischi rigidi virtuali che sono astrazioni di file di un disco o un disco pass-through sull'host.

## <a name="virtual-hard-disks"></a>Dischi rigidi virtuali

Esistono due formati di disco rigido virtuale, VHD e VHDX. Ognuno di questi formati supporta tre tipi di file di disco rigido virtuale.

## <a name="vhd-format"></a>Formato di disco rigido virtuale

Il formato di disco rigido virtuale è stato il formato di disco rigido virtuale solo che era supportato da Hyper-V in versioni precedenti. Introdotta in Windows Server 2012, il formato di disco rigido virtuale è stato modificato per consentire di migliorare l'allineamento, con conseguente miglioramento significativo delle prestazioni su nuovi dischi con settori di grandi dimensioni.

Qualsiasi nuovo disco rigido virtuale che viene creato in un Windows Server 2012 o versione successiva è l'allineamento di 4 KB ottimale. Questo formato allineato è completamente compatibile con sistemi operativi Windows Server precedenti. Tuttavia, la proprietà di allineamento è interrotta per le nuove allocazioni dal parser che sono compatibili con l'allineamento, ad esempio (un parser di disco rigido virtuale da una versione precedente di Windows Server) o un parser non Microsoft non 4 KB.

Qualsiasi disco rigido virtuale che viene spostato da una versione precedente non ottenere convertito automaticamente in questo nuovo formato di disco rigido virtuale migliorato.

Per convertire in nuovo formato di disco rigido virtuale, eseguire il comando Windows PowerShell seguente:

``` syntax
Convert-VHD –Path E:\vms\testvhd\test.vhd –DestinationPath E:\vms\testvhd\test-converted.vhd
```

È possibile controllare la proprietà di allineamento per tutti i dischi rigidi virtuali nel sistema e deve essere convertito l'allineamento di 4 KB ottimale. Creare un nuovo disco rigido virtuale con i dati dal disco rigido virtuale originale usando il **Create dall'origine** opzione.

Per verificare l'allineamento mediante Windows Powershell, esaminare la riga di allineamento, come illustrato di seguito:

``` syntax
Get-VHD –Path E:\vms\testvhd\test.vhd

Path                    : E:\vms\testvhd\test.vhd
VhdFormat               : VHD
VhdType                 : Dynamic
FileSize                : 69245440
Size                    : 10737418240
MinimumSize             : 10735321088
LogicalSectorSize       : 512
PhysicalSectorSize      : 512
BlockSize               : 2097152
ParentPath              :
FragmentationPercentage : 10
Alignment               : 0
Attached                : False
DiskNumber              :
IsDeleted               : False
Number                  :
```

Per verificare l'allineamento mediante Windows PowerShell, esaminare la riga di allineamento, come illustrato di seguito:

``` syntax
Get-VHD –Path E:\vms\testvhd\test-converted.vhd

Path                    : E:\vms\testvhd\test-converted.vhd
VhdFormat               : VHD
VhdType                 : Dynamic
FileSize                : 69369856
Size                    : 10737418240
MinimumSize             : 10735321088
LogicalSectorSize       : 512
PhysicalSectorSize      : 512
BlockSize               : 2097152
ParentPath              :
FragmentationPercentage : 0
Alignment               : 1
Attached                : False
DiskNumber              :
IsDeleted               : False
Number                  :
```

## <a name="vhdx-format"></a>Formato VHDX

VHDX è un nuovo formato di disco rigido virtuale introdotto in Windows Server 2012, che consente di creare resilienti ad alte prestazioni per i dischi virtuali, fino a 64 terabyte. Vantaggi di questo formato:

-   Supporto per la capacità di archiviazione di disco rigido virtuale di fino a 64 terabyte.

-   Protezione contro il danneggiamento dei dati in caso di interruzioni dell'alimentazione grazie alla registrazione degli aggiornamenti nelle strutture dei metadati VHDX.

-   Possibilità di archiviare metadati personalizzati su un file, che un utente può decidere di registrare, come versione del sistema operativo o patch applicate.

Il formato VHDX offre anche i vantaggi delle prestazioni seguenti:

-   Migliore allineamento del formato di disco rigido virtuale per assicurare risultati migliori sui dischi con settori di grandi dimensioni.

-   Blocchi di dimensioni maggiori per i dischi dinamici e backup differenziali, che consente a tali dischi per l'adattamento alle esigenze del carico di lavoro.

-   4 KB disco con settore logico virtuale che consente di migliorare le prestazioni quando viene utilizzato dalle applicazioni e carichi di lavoro progettati per settori di 4 KB.

-   Efficienza nella rappresentazione di dati, che genera file di dimensioni inferiori e consente al dispositivo di archiviazione fisica sottostante recuperare lo spazio inutilizzato. Per trim è necessario pass-through o dischi SCSI e hardware siano compatibili.

Quando esegue l'aggiornamento a Windows Server 2016, è consigliabile convertire tutti i file di disco rigido virtuale in formato VHDX a causa di questi vantaggi. L'unico scenario in cui è opportuno conservare i file in formato VHD è quando una macchina virtuale ha la possibilità di essere spostato in una versione precedente di Hyper-V che non supporta il formato VHDX.

## <a name="types-of-virtual-hard-disk-files"></a>Tipi di file di disco rigido virtuale

Esistono tre tipi di file di disco rigido virtuale. Le sezioni seguenti sono le caratteristiche di prestazioni e i compromessi tra i tipi.

I consigli seguenti devono essere prese in considerazione per quanto riguarda la selezione di un tipo di file di disco rigido virtuale:

-   Quando si usa il formato di disco rigido virtuale, è consigliabile usare il tipo predefinito perché ha una migliore resilienza e le caratteristiche di prestazioni rispetto ad altri tipi di file di disco rigido virtuale.

-   Quando si usa il formato VHDX, è consigliabile usare il tipo dinamico perché offre garanzie di resilienza oltre al risparmio di spazio che derivano dall'allocazione di spazio solo quando è necessario eseguire questa operazione.

-   Il tipo predefinito è inoltre consigliabile, indipendentemente dal formato, quando lo spazio di archiviazione sul volume di hosting non viene monitorato attivamente per garantire che sia presente quando si espande il file di disco rigido virtuale in fase di esecuzione spazio su disco sufficiente.

-   Gli snapshot di una macchina virtuale crea una differenziazione disco rigido virtuale per archiviare alla scrittura dei dischi. Con solo alcuni snapshot possono elevare non notevolmente l'utilizzo della CPU di spazio di archiviazione i/o, ma è possibile influire sulle prestazioni, ad eccezione dei carichi di lavoro server altamente I/O intensivo. Tuttavia, disponibilità di una catena di snapshot influiscono notevolmente sulle prestazioni poiché la lettura dal disco rigido virtuale può richiedere la verifica dei blocchi necessari in molti dischi rigidi virtuali differenze. Mantenere brevi le catene dello snapshot è importante per la gestione delle prestazioni dei / o disco valida.

## <a name="fixed-virtual-hard-disk-type"></a>Tipo di disco rigido virtuale fisso

Lo spazio per il disco rigido virtuale prima di tutto viene allocato quando viene creato il file di disco rigido virtuale. Questo tipo di file di disco rigido virtuale è meno probabile che la frammentazione, che riduce la velocità effettiva dei / o quando un / o singola è suddiviso in diversi i/o. Include il sovraccarico della CPU più basso tra i tre tipi di file di disco rigido virtuale perché le letture e scritture non sono necessario ricercare il mapping del blocco.

## <a name="dynamic-virtual-hard-disk-type"></a>Tipo di disco rigido virtuale dinamico

Lo spazio per il disco rigido virtuale è allocato su richiesta. I blocchi nel disco avviare come blocchi non allocati e non sono supportati da qualsiasi spazio effettivo nel file. Quando a un blocco viene scritto, lo stack di virtualizzazione deve allocare spazio all'interno del file di disco rigido virtuale per il blocco e quindi aggiornare i metadati. Ciò aumenta il numero dei / o del disco necessari per la scrittura e aumenta l'utilizzo della CPU. Letture e scritture a blocchi esistenti non comportano un sovraccarico della CPU e dei dischi durante la ricerca di mapping dei blocchi nei metadati.

## <a name="differencing-virtual-hard-disk-type"></a>Tipo di disco rigido virtuale differenze

Il disco rigido virtuale punta a un file di disco rigido virtuale padre. Tutte le scritture a blocchi non scritti come risultato lo spazio viene allocato nel file di disco rigido virtuale, come con un VHD a espansione dinamica. Le letture vengono servite dal file di disco rigido virtuale se il blocco è stato scritto. In caso contrario, che siano gestite dal file di disco rigido virtuale padre. In entrambi i casi, i metadati vengono letti per determinare il mapping del blocco. Letture e scritture su disco rigido virtuale possono utilizzare più CPU e comportare più i/o rispetto a un file di disco rigido virtuale fisso.

## <a name="block-size-considerations"></a>Considerazioni relative alle dimensioni di blocco

Dimensione del blocco può influire significativamente sulle prestazioni. È ottimale in base alle dimensioni di blocco per i modelli di allocazione del carico di lavoro che usa il disco. Ad esempio, se un'applicazione sta allocando in blocchi da 16 MB, sarebbe ottimale per avere una dimensione del blocco di disco rigido virtuale di 16 MB. Dimensione del blocco di &gt;solo in dischi rigidi virtuali con il formato VHDX è 2 MB. Presenza di blocchi di dimensioni maggiori del criterio di allocazione per un carico di lavoro dei / o casuali aumenterà in modo significativo l'utilizzo dello spazio nell'host.

## <a name="sector-size-implications"></a>Implicazioni relative alla dimensione di settore

La maggior parte del settore del software ha dipendono i settori del disco di 512 byte, ma lo standard viene spostata nei settori del disco da 4 KB. Per ridurre i problemi di compatibilità che potrebbero verificarsi da una modifica nelle dimensioni di settore, i fornitori di disco rigido di introdurre una dimensione transitoria definita come unità di emulazione 512 (512e).

Queste unità emulazione offrono alcuni dei vantaggi offerti da 4 KB settore native unità disco, ad esempio l'efficienza di formato migliorato e uno schema ottimizzato per i codici di correzione errore (ECC). Vengono forniti con meno problemi di compatibilità che si verificano esponendo una dimensione di settore 4 KB all'interfaccia del disco.

## <a name="support-for-512e-disks"></a>Supporto per i dischi 512e

Un disco 512e può eseguire un'operazione di scrittura solo in termini di settore fisico, vale a dire, non è possibile scrivere direttamente un settore da 512 byte rilasciato ad esso. Il processo interno nel disco che rende possibili i diritti appropriati a questi passaggi:

-   Il disco legge il settore fisico a 4 KB per la cache interna, che contiene il settore logico a 512 byte a cui l'operazione di scrittura.

-   I dati nel buffer a 4 KB vengono modificati in modo da includere il settore a 512 byte aggiornato.

-   Il disco esegue un'operazione di scrittura del buffer da 4 KB aggiornato al proprio settore fisico sul disco.

Questo processo è detto read-modify-write (RMW). L'impatto sulle prestazioni complessive del processo di RMW varia in base i carichi di lavoro. Il processo di RMW provoca una riduzione delle prestazioni in dischi rigidi virtuali per i motivi seguenti:

-   Dischi rigidi virtuali dinamici e differenze sono una bitmap settore a 512 byte prima del payload dati. Inoltre, un piè di pagina, intestazione e i localizzatori padre Allinea in un settore da 512 byte. È comune per il driver del disco rigido virtuale per eseguire i comandi di scrittura a 512 byte per aggiornare queste strutture, causando il processo RMW descritto in precedenza.

-   Le applicazioni comunemente problema legge e scrive in multipli di 4 KB dimensione (la dimensione del cluster del file system NTFS). Perché è una bitmap settore a 512 byte prima il blocco del payload dati dinamico e dischi rigidi virtuali differenze, i blocchi di 4 KB non allineati al limite di 4 KB fisico. La figura seguente mostra un disco rigido virtuale 4 KB blocco (evidenziato) vale a dire non allineato con limite di 4 KB fisico.

![blocchi di 4 kb di disco rigido virtuale](../../media/perftune-guide-vhd-4kb-block.png)

Ogni comando di scrittura 4 KB che viene generato dal parser corrente per aggiornare i dati di payload comporta due operazioni di lettura per due blocchi sul disco, che vengono quindi aggiornati e successivamente scritti in due blocchi del disco. Hyper-V in Windows Server 2016 consente di ridurre alcuni degli effetti sulle prestazioni su dischi 512e sullo stack di disco rigido virtuale preparandone le strutture menzionate in precedenza per l'allineamento ai limiti di 4 KB in formato VHD. Questo evita l'effetto RMW durante l'accesso a dati all'interno del file di disco rigido virtuale e quando si aggiornano le strutture di metadati del disco rigido virtuale.

Come accennato in precedenza, i dischi rigidi virtuali copiati da versioni precedenti di Windows Server non verranno automaticamente allineati a 4 KB. È possibile convertirli manualmente per allineare in modo ottimale tramite il **copia dall'origine** opzione disponibile nelle interfacce di disco rigido virtuale del disco.

Per impostazione predefinita, i dischi rigidi virtuali vengono esposte con dimensioni fisiche di settore di 512 byte. Questa operazione viene eseguita per garantire che le applicazioni dipendenti di dimensioni fisiche di settore non sono interessate quando l'applicazione e i dischi rigidi virtuali vengono spostati da una versione precedente di Windows Server.

Per impostazione predefinita, i dischi con il formato VHDX vengono creati con le dimensioni fisiche di settore di 4 KB per ottimizzare i dischi dei profili regolari delle prestazioni e i dischi con settori di grandi dimensioni. Per le funzionalità complete dei settori di 4 KB è consigliabile per usare il formato VHDX.

## <a name="support-for-native-4kb-disks"></a>Supporto per dischi a 4 KB nativo

Hyper-V in Windows Server 2012 R2 e versioni successive supporta i dischi nativi di 4 KB. Ma è comunque possibile per l'archiviazione disco rigido virtuale su disco di 4 KB nativo. Questa operazione viene eseguita mediante l'implementazione di un software RMW algoritmo nel livello dello stack di archiviazione virtuale che converte le richieste di accesso e l'aggiornamento a 512 byte corrispondenti a 4 KB accede e degli aggiornamenti.

Perché file di disco rigido virtuale può solo esposte come dischi di dimensioni di settore logico a 512 byte, è molto probabile che siano presenti applicazioni che inviano richieste dei / o a 512 byte. In questi casi, il livello RMW verrà soddisfare queste richieste e provocare una riduzione delle prestazioni. Questo vale anche per un disco che viene formattato con VHDX che ha una dimensione di settore logico di 512 byte.

È possibile configurare un file VHDX deve essere esposta come un disco di dimensioni di settore logico di 4 KB, e si tratterà di una configurazione ottimale per le prestazioni, quando il disco è ospitato in un dispositivo fisico native da 4 KB. Dovrebbe prestare attenzione per assicurarsi che il guest e l'applicazione che usa il disco virtuale sono supportati per le dimensioni di settore logico di 4 KB. Il file VHDX formattazione funzioneranno correttamente in un dispositivo di dimensioni con settore logico di 4 KB.

## <a name="pass-through-disks"></a>Dischi pass-through

Il disco rigido virtuale in una macchina virtuale può eseguire il mapping direttamente a un disco fisico o il numero di unità logica (LUN), anziché in un file di disco rigido virtuale. Il vantaggio è che questa configurazione consente di ignorare il file system NTFS nella partizione radice, che riduce l'utilizzo della CPU dei / o archiviazione. Il rischio è che i LUN o dischi fisici possono essere più difficili per spostarsi tra i computer rispetto ai file di disco rigido virtuale.

Evitare di dischi pass-through causa sono le limitazioni introdotte con scenari di migrazione di macchine virtuali.

## <a name="advanced-storage-features"></a>Funzionalità avanzate di archiviazione

### <a name="storage-quality-of-service-qos"></a>QoS di archiviazione

A partire da Windows Server 2012 R2, Hyper-V include la possibilità di impostare determinati parametri di qualità del servizio (QoS) per l'archiviazione nelle macchine virtuali. QoS di archiviazione consente di isolare le prestazioni di archiviazione in un ambiente multitenant e fornisce meccanismi di notifica per le situazioni in cui le prestazioni di I/O di archiviazione non soddisfano la soglia definita per garantire un'esecuzione efficiente dei carichi di lavoro delle macchine virtuali.

QoS di archiviazione consente di specificare un valore massimo per le operazioni di input/output al secondo (IOPS) per un disco rigido virtuale. Un amministratore può limitare l'I/O di archiviazione per impedire a un tenant di continuare a consumare una quantità di risorse di archiviazione eccessiva, con possibili ripercussioni negative su un altro tenant.

È anche possibile impostare un valore minimo di IOPS. L'amministratore riceverà una notifica quando il valore di IOPS per un determinato disco rigido virtuale scende sotto la soglia necessaria per assicurare prestazioni ottimali.

È stata aggiornata anche l'infrastruttura metrica delle macchine virtuali, con parametri di archiviazione che consentono all'amministratore di monitorare le prestazioni ed eseguire il chargeback dei parametri correlati.

Valori massimi e minimi sono specificati in termini di IOPS normalizzato, dove ogni 8 KB di dati viene conteggiato come un / o.

Alcune delle limitazioni sono i seguenti:

-   Solo per i dischi virtuali

-   Disco differenze non può avere del disco virtuale principale su un volume diverso

-   Replica - QoS per il sito di replica configurato separatamente dal sito primario

-   VHDX condiviso non è supportato

Per altre informazioni su QoS di archiviazione, vedi [QoS di archiviazione per Hyper-V](https://technet.microsoft.com/library/dn282281.aspx).

### <a name="numa-io"></a>NUMA I/O

Windows Server 2012 e versioni successive supporta macchine virtuali grandi e qualsiasi configurazione di macchine virtuali di grandi dimensioni (ad esempio, una configurazione con Microsoft SQL Server in esecuzione con 64 processori virtuali) sarà necessario anche la scalabilità in termini di velocità effettiva dei / o.

I seguenti miglioramenti principali introdotti inizialmente nello stack di archiviazione di Windows Server 2012 e Hyper-V forniscono le esigenze di scalabilità dei / o di grandi dimensioni di macchine virtuali:

-   Un aumento del numero di canali di comunicazione tra i dispositivi guest e stack di archiviazione host creato.

-   Un meccanismo di completamento i/o più efficiente che interessa la distribuzione di interruzione tra i processori virtuali per evitare costose interruzioni interprocessor.

Introdotta in Windows Server 2012, sono presenti alcune voci del Registro di sistema, che si trova in HKLM\\System\\CurrentControlSet\\Enum\\VMBUS\\{id dispositivo}\\{id istanza}\\StorChannel, che consentono il numero di canali affinché venga regolato. Vengono inoltre allineati i processori virtuali che gestiscono i completamenti dei / o alle CPU virtuale vengono assegnati per l'applicazione per cui i processori dei / o. Le impostazioni del Registro di sistema sono configurate su una base di ogni scheda per la chiave del dispositivo hardware.

-   **ChannelCount (DWORD)** il numero totale di canali da usare, con un massimo di 16. Per impostazione predefinita un tetto massimo, ovvero il numero di processori virtuali/16.

-   **ChannelMask (QWORD)** l'affinità processori per i canali. Se non è impostata o è impostato su 0, per impostazione predefinita l'algoritmo di distribuzione canale esistente che usa per l'archiviazione normale o per canali di networking. Ciò garantisce che i canali di archiviazione non siano in conflitto con i canali di rete.

### <a name="offloaded-data-transfer-integration"></a>Integrazione di trasferimento di dati ODX

Attività di manutenzione cruciali per i dischi rigidi virtuali, come unione, spostare e compact, variano in copia grandi quantità di dati. Con il metodo corrente il processo di copia implica la lettura e la scrittura dei dati in posizioni diverse e può quindi richiedere molto tempo. Usa inoltre le risorse della CPU e memoria nell'host, che sarebbe stato possibile utilizzare per macchine virtuali del servizio.

I fornitori di reti di archiviazione (SAN, Storage Area Network) sono già al lavoro per offrire operazioni di copia quasi istantanea di grandi quantità di dati. Questa risorsa di archiviazione è progettato per consentire al sistema in cui i dischi di specificare lo spostamento di un set di dati specifico da una posizione a un'altra. Questa funzionalità di hardware è noto come un Offloaded Data Transfer.

Hyper-V in Windows Server 2012 e versioni successive supporta le operazioni di Offloaded Data Transfer () in modo che queste operazioni possono essere passate dal sistema operativo guest per l'hardware host. Ciò garantisce che il carico di lavoro è possibile utilizzare ODX abilitato archiviazione come se fosse in esecuzione in un ambiente non virtualizzato. Lo stack di archiviazione di Hyper-V rilascia anche le operazioni ODX durante operazioni di manutenzione per i dischi rigidi virtuali, ad esempio l'unione di dischi e archiviazione meta-operazioni di migrazione in cui vengono spostate grandi quantità di dati.

### <a name="unmap-integration"></a>Annullare il mapping di integrazione

File disco rigido virtuale sono presenti in un volume di archiviazione che condividono lo spazio disponibile con gli altri file. Poiché le dimensioni di questi file tendono a essere di grandi dimensioni, lo spazio che consumano una quantità può aumentare rapidamente. Richiesta per l'archiviazione fisica aggiuntiva interessa il budget per l'hardware IT. È importante ottimizzare l'uso di archiviazione fisica quanto più possibile.

Prima di Windows Server 2012, quando le applicazioni eliminano contenuto all'interno di un disco rigido virtuale, che ha abbandonato in modo efficiente lo spazio di archiviazione del contenuto, lo stack di archiviazione di Windows nel sistema operativo guest e host Hyper-V con limitazioni che ha impedito a questa informazioni da viene comunicato al disco rigido virtuale e il dispositivo di archiviazione fisica. Questo ha impedito lo stack di archiviazione di Hyper-V ottimizzando l'utilizzo dello spazio dai file di disco virtuale basato su disco rigido virtuale. Inoltre, ha impedito il dispositivo di archiviazione sottostante dal recupero di spazio occupata dai dati eliminati.

A partire da Windows Server 2012, Hyper-V supporta annullare il mapping delle notifiche, che consentono di essere più efficiente nel caso che rappresenta i dati in esso contenuti dei file VHDX. Il risultato è più piccolo delle dimensioni di file e consente al dispositivo di archiviazione fisica sottostante recuperare lo spazio inutilizzato.

Solo SCSI specifici di Hyper-V, abilitate per IDE e Fibre Channel virtuale controller di consentire il comando di annullamento del mapping del guest per raggiungere lo stack di archiviazione virtuale host. Nei dischi rigidi virtuali, solo i dischi virtuali formattati VHDX supporta annullare il mapping di comandi dal guest.

Per questi motivi, è consigliabile usare i file VHDX collegati a un controller SCSI quando non usano dischi virtuali Fibre Channel.

## <a name="see-also"></a>Vedere anche

-   [Terminologia di Hyper-V](terminology.md)

-   [Architettura di Hyper-V](architecture.md)

-   [Configurazione del server Hyper-V](configuration.md)

-   [Prestazioni del processore di Hyper-V](processor-performance.md)

-   [Prestazioni della memoria di Hyper-V](memory-performance.md)

-   [Rete Hyper-V delle prestazioni dei / o](network-io-performance.md)

-   [Rilevamento dei colli di bottiglia in un ambiente virtualizzato](detecting-virtualized-environment-bottlenecks.md)

-   [Macchine virtuali Linux](linux-virtual-machine-considerations.md)
