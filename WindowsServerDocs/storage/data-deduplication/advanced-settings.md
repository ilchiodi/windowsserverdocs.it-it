---
ms.assetid: 01c8cece-66ce-4a83-a81e-aa6cc98e51fc
title: Impostazioni avanzate di Deduplicazione dati
ms.prod: windows-server
ms.technology: storage-deduplication
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: 1d0677cec134ddeb4c706d0f1231f2c26b39967e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403213"
---
# <a name="advanced-data-deduplication-settings"></a>Impostazioni avanzate di Deduplicazione dati

> Si applica a Windows Server (Canale semestrale), Windows Server 2016

In questo documento viene descritto come modificare le impostazioni di [deduplicazione](overview.md) avanzate. Per i [carichi di lavoro consigliati](install-enable.md#enable-dedup-candidate-workloads), le impostazioni predefinite dovrebbero essere sufficienti. Il motivo principale per cui si modificano queste impostazioni è migliorare le prestazioni della deduplicazione dei dati con altri tipi di carichi di lavoro.

## <a id="modifying-job-schedules"></a>Modifica delle pianificazioni dei processi di deduplicazione dati
Le [pianificazioni predefinite dei processi di deduplicazione dati](understand.md#job-info) sono progettate in modo da funzionare per i carichi di lavoro consigliati ed essere il meno possibile intrusive (escluso il processo *Ottimizzazione della priorità* che è abilitato per il [tipo di utilizzo **Backup**](understand.md#usage-type-backup)). Quando i carichi di lavoro richiedono la disponibilità di un gran numero di risorse, è possibile garantire che i processi vengano eseguiti solo durante le ore di inattività oppure ridurre o aumentare la quantità di risorse di sistema che un processo di deduplicazione dati è autorizzato a consumare.

### <a id="modifying-job-schedules-change-schedule"></a>Modifica della pianificazione della deduplicazione dei dati
I processi di deduplicazione dati vengono pianificati dal servizio Utilità di pianificazione di Windows, che consente di visualizzarli e modificarli nel percorso Microsoft\Windows\Deduplication. La deduplicazione dei dati include diversi cmdlet che semplificano la pianificazione.
* [`Get-DedupSchedule`](https://technet.microsoft.com/library/hh848446.aspx) Mostra i processi pianificati correnti.
* [`New-DedupSchedule`](https://technet.microsoft.com/library/hh848445.aspx) crea un nuovo processo pianificato.
* [`Set-DedupSchedule`](https://technet.microsoft.com/library/hh848447.aspx) modifica un processo pianificato esistente.
* [`Remove-DedupSchedule`](https://technet.microsoft.com/library/hh848451.aspx) rimuove un processo pianificato.

Il motivo più comune per cui si modifica l'orario in cui devono essere eseguiti i processi di deduplicazione dei dati è la necessità di assicurarsi che vengano eseguiti durante le ore non lavorative. Nell'esempio seguente viene illustrato in dettaglio come modificare la pianificazione della deduplicazione dei dati per uno scenario *sunny day*: un host Hyper-V iperconvergente non attivo nei fine settimana e dopo le 19:00 nei giorni lavorativi. Per modificare la pianificazione, eseguire i cmdlet di PowerShell indicati di seguito in un contesto Amministratore.

1. Disabilitare i processi di [ottimizzazione](understand.md#job-info-optimization) con pianificazione oraria.  
    ```PowerShell
    Set-DedupSchedule -Name BackgroundOptimization -Enabled $false
    Set-DedupSchedule -Name PriorityOptimization -Enabled $false
    ```

2. Rimuovere i processi [Garbage Collection](understand.md#job-info-gc) e [Pulitura dell'integrità](understand.md#job-info-scrubbing) attualmente pianificati.
    ```PowerShell
    Get-DedupSchedule -Type GarbageCollection | ForEach-Object { Remove-DedupSchedule -InputObject $_ }
    Get-DedupSchedule -Type Scrubbing | ForEach-Object { Remove-DedupSchedule -InputObject $_ }
    ```

3. Creare un processo di ottimizzazione notturno che venga eseguito alle 19:00 con priorità alta e tutte le CPU e tutta la memoria disponibili nel sistema.
    ```PowerShell
    New-DedupSchedule -Name "NightlyOptimization" -Type Optimization -DurationHours 11 -Memory 100 -Cores 100 -Priority High -Days @(1,2,3,4,5) -Start (Get-Date "2016-08-08 19:00:00")
    ```

    >[!NOTE]  
    > La parte *date* del `System.Datetime` trasmesso a `-Start` è irrilevante (purché sia nel passato), ma la parte *time* specifica quando deve iniziare il processo.
4. Creare un processo di Garbage Collection settimanale che venga eseguito il sabato a partire dalle 7:00 con priorità alta e tutte le CPU e tutta la memoria disponibili nel sistema.
    ```PowerShell
    New-DedupSchedule -Name "WeeklyGarbageCollection" -Type GarbageCollection -DurationHours 23 -Memory 100 -Cores 100 -Priority High -Days @(6) -Start (Get-Date "2016-08-13 07:00:00")
    ```

5. Creare un processo di Pulitura dell'integrità settimanale che venga eseguito la domenica a partire dalle 7:00 con priorità alta e tutte le CPU e tutta la memoria disponibili nel sistema.
    ```PowerShell
    New-DedupSchedule -Name "WeeklyIntegrityScrubbing" -Type Scrubbing -DurationHours 23 -Memory 100 -Cores 100 -Priority High -Days @(0) -Start (Get-Date "2016-08-14 07:00:00")
    ```

### <a id="modifying-job-schedules-available-settings"></a>Impostazioni a livello di processo disponibili
È possibile attivare o disattivare le impostazioni seguenti per i processi di deduplicazione dati nuovi o pianificati:

<table>
    <thead>
        <tr>
            <th style="min-width:125px">Nome parametro</th>
            <th>Definizione</th>
            <th>Valori accettati</th>
            <th>Motivo per cui si vuole impostare questo valore</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Tipo</td>
            <td>Il tipo di processo che deve essere pianificato</td>
            <td>
                <ul>
                    <li>Ottimizzazione</li>
                    <li>GarbageCollection</li>
                    <li>Scrubbing</li>
                </ul>
            </td>
            <td>Questo valore è obbligatorio perché è il tipo di processo che si vuole pianificare. Questo valore non può essere modificato dopo che l'attività è stata pianificata.</td>
        </tr>
        <tr>
            <td>Priority</td>
            <td>Priorità di sistema del processo pianificato</td>
            <td>
                <ul>
                    <li>Alto</li>
                    <li>Medio</li>
                    <li>Bassa</li>
                </ul>
            </td>
            <td>Questo valore consente al sistema di determinare la modalità di allocazione del tempo della CPU. <em>Alto</em> userà più tempo della CPU, <em>Basso</em> ne userà il minimo.</td>
        </tr>
        <tr>
            <td>Giorni</td>
            <td>I giorni in cui il processo è pianificato</td>
            <td>Matrice di numeri interi da 0 a 6 che rappresenta i giorni della settimana:<ul>
                <li>0 = domenica</li>
                <li>1 = lunedì</li>
                <li>2 = martedì</li>
                <li>3= mercoledì</li>
                <li>4 = giovedì</li>
                <li>5 = venerdì</li>
                <li>6 = sabato</li>
            </ul></td>
            <td>Le attività pianificate devono essere eseguite in almeno un giorno.</td>
        </tr>
        <tr>
            <td>Core</td>
            <td>La percentuale di core del sistema che deve usata da un processo</td>
            <td>Numeri interi da 0 a 100 (indica una percentuale)</td>
            <td>Per controllare il livello di impatto di un processo sulle risorse di calcolo del sistema</td>
        </tr>
        <tr>
            <td>DurationHours</td>
            <td>Numero massimo di ore consentite per l'esecuzione di un processo</td>
            <td>Numeri interi positivi</td>
            <td>Per impedire l'esecuzione di un processo in ore&#39;non di inattività del carico di lavoro</td>
        </tr>
        <tr>
            <td>Enabled</td>
            <td>Se il processo verrà eseguito</td>
            <td>True/false</td>
            <td>Per disabilitare un processo senza rimuoverlo</td>
        </tr>
        <tr>
            <td>Completi</td>
            <td>Per la pianificazione di un processo di Garbage Collection completo</td>
            <td>Opzione (true/false)</td>
            <td>Per impostazione predefinita, ogni quarto processo è un processo di Garbage Collection completo. Con questa opzione, è possibile pianificare l'esecuzione di Garbage Collection completa in modo che sia più frequente.</td>
        </tr>
        <tr>
            <td>InputOutputThrottle</td>
            <td>Specifica la quantità di limitazione di input/output applicata al processo</td>
            <td>Numeri interi da 0 a 100 (indica una percentuale)</td>
            <td>La limitazione garantisce che i processi&#39;non interferiscano con altri processi a elevato utilizzo di I/O.</td>
        </tr>
        <tr>
            <td>Memoria</td>
            <td>Percentuale di memoria del sistema che deve usata da un processo</td>
            <td>Numeri interi da 0 a 100 (indica una percentuale)</td>
            <td>Per controllare il livello di impatto del processo sulle risorse di memoria del sistema</td>
        </tr>
        <tr>
            <td>Nome</td>
            <td>Il nome del processo pianificato</td>
            <td>Stringa</td>
            <td>Un processo deve avere un nome identificabile in modo univoco.</td>
        </tr>
        <tr>
            <td>ReadOnly</td>
            <td>Indica che il processo di pulitura elabora e segnala i danneggiamenti che trova, ma non effettua alcuna azione di riparazione</td>
            <td>Opzione (true/false)</td>
            <td>Si vuole ripristinare manualmente i file che risiedono in sezioni danneggiate del disco.</td>
        </tr>
        <tr>
            <td>Inizio</td>
            <td>Specifica l'ora in cui un processo deve iniziare</td>
            <td><code>System.DateTime</code></td>
            <td>La parte relativa alla <em>Data</em> del <code>System.Datetime</code> fornito per l' <em>avvio</em> è irrilevante (purché&#39;sia nel passato), ma la parte relativa all' <em>ora</em> specifica quando avviare il processo.</td>
        </tr>
        <tr>
            <td>StopWhenSystemBusy</td>
            <td>Specifica se la deduplicazione dei dati deve essere interrotta se il sistema è occupato</td>
            <td>Opzione (true/false)</td>
            <td>Questa opzione consente di controllare il comportamento della deduplicazione dei dati. Ciò è particolarmente importante se si vuole eseguire la deduplicazione dei dati mentre il carico di lavoro è attivo.</td>
        </tr>
    </tbody>
</table>

## <a id="modifying-volume-settings"></a>Modifica delle impostazioni a livello di volume Deduplicazione dati
### <a id="modifying-volume-settings-how-to-toggle"></a>Attivazione/disattivizzazione delle impostazioni del volume
È possibile specificare impostazioni predefinite a livello di volume per la deduplicazione dei dati usando il [tipo di utilizzo](understand.md#usage-type) che si seleziona quando si abilita una deduplicazione per un volume. La deduplicazione dei dati include cmdlet che semplificano la modifica delle impostazioni a livello di volume:

* [`Get-DedupVolume`](https://technet.microsoft.com/library/hh848448.aspx)
* [`Set-DedupVolume`](https://technet.microsoft.com/library/hh848438.aspx)

I motivi principali per cui si modificano le impostazioni di volume per il tipo di utilizzo selezionato sono migliorare le prestazioni di lettura per file specifici, ad esempio elementi multimediali o altri tipi di file che sono già compressi, e regolare la deduplicazione dei dati per ottimizzarla al meglio per il carico di lavoro specifico. Nell'esempio seguente viene illustrato come modificare le impostazioni del volume della deduplicazione dei dati per un carico di lavoro che è molto simile a un carico di lavoro generale del file server, ma usa file di grandi dimensioni che cambiano frequentemente.

1. Vedere le impostazioni correnti del volume per il Volume condiviso cluster 1.
    ```PowerShell
    Get-DedupVolume -Volume C:\ClusterStorage\Volume1 | Select *
    ```

2. Abilitare OptimizePartialFiles nel Volume condiviso cluster 1 in modo che il criterio MinimumFileAge venga applicato a sezioni del file anziché all'intero file. Ciò garantisce che la maggior parte del file venga ottimizzata anche se le sezioni del file cambiano regolarmente.
    ```PowerShell
    Set-DedupVolume -Volume C:\ClusterStorage\Volume1 -OptimizePartialFiles
    ```

### <a id="modifying-volume-settings-available-settings"></a>Impostazioni a livello di volume disponibili
<table>
    <thead>
        <tr>
            <th style="min-width:125px">Nome dell'impostazione</th>
            <th>Definizione</th>
            <th>Valori accettati</th>
            <th>Motivo per cui si vuole modificare questo valore</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ChunkRedundancyThreshold</td>
            <td>Numero di volte in cui si fa riferimento a un blocco prima che un blocco venga duplicato nella sezione hotspot dell'Archivio blocchi. Il valore della sezione hotspot è che i cosiddetti &quot;blocchi Hot&quot; a cui si fa riferimento spesso hanno più percorsi di accesso per migliorare i tempi di accesso.</td>
            <td>Numeri interi positivi</td>
            <td>Il motivo principale per cui si modifica questo numero è incrementare la percentuale di riduzione per i volumi con elevata duplicazione. In generale, il valore predefinito (100) rappresenta l'impostazione consigliata e non è&#39;necessario modificarlo.</td>
        </tr>
        <tr>
            <td>ExcludeFileType</td>
            <td>Tipi di file che vengono esclusi dall'ottimizzazione</td>
            <td>Matrice di estensioni file</td>
            <td>Per alcuni tipi di file, in particolare i file multimediali o quelli già compressi, l'ottimizzazione non comporta particolari vantaggi. Questa impostazione consente di configurare quali tipi sono esclusi.</td>
        </tr>
        <tr>
            <td>ExcludeFolder</td>
            <td>Specifica i percorsi delle cartelle che non devono essere considerate per l'ottimizzazione</td>
            <td>Matrice di percorsi di cartelle</td>
            <td>Per migliorare le prestazioni o evitare che il contenuto in determinati percorsi venga ottimizzato, è possibile escludere determinati percorsi nel volume dal processo di ottimizzazione.</td>
        </tr>
        <tr>
            <td>InputOutputScale</td>
            <td>Specifica il livello di parallelizzazione IO (code IO) per la deduplicazione dei dati da usare in un volume durante un processo di post-elaborazione</td>
            <td>Numeri interi positivi da 1 a 36</td>
            <td>Il motivo principale per cui si modifica questo valore è ridurre l'impatto sulle prestazioni di un elevato carico di lavoro IO limitando il numero di code IO che la deduplicazione dei dati può usare in un volume. Si noti che la modifica di questa impostazione dall'impostazione predefinita può causare un&#39;rallentamento dei processi di post-elaborazione di deduplicazione dati.</td>
        </tr>
        <tr>
            <td>MinimumFileAgeDays</td>
            <td>Numero di giorni dopo la creazione del file prima che il file sia considerato conforme ai criteri per l'ottimizzazione.</td>
            <td>Numeri interi positivi (incluso zero)</td>
            <td>I tipi di uso <strong>predefinito</strong> e <strong>HyperV</strong> impostano questo valore su 3 per ottimizzare le prestazioni sui file attivi o creati di recente. È consigliabile modificare il valore per rendere più aggressiva la deduplicazione dei dati o se non è rilevante la latenza aggiuntiva associata alla deduplicazione.</td>
        </tr>
        <tr>
            <td>MinimumFileSize</td>
            <td>Dimensione minima che un file deve avere per essere considerato conforme ai criteri per l'ottimizzazione</td>
            <td>Numeri interi positivi (byte) maggiori di 32 KB</td>
            <td>Il motivo principale della modifica di questo valore è escludere i file di piccole dimensioni che possono avere un valore di ottimizzazione limitato per risparmiare tempo di calcolo.</td>
        </tr>
        <tr>
            <td>NoCompress</td>
            <td>Indica se i blocchi devono essere compressi prima di essere inseriti nell'Archivio blocchi</td>
            <td>True/False</td>
            <td>Alcuni tipi di file, in particolare quelli multimediali e già compressi, non vengono compressi in modo appropriato. Questa impostazione consente di disattivare la compressione per tutti i file nel volume. Questa funzionalità è ideale se si sta ottimizzando un dataset che contiene molti file già compressi.</td>
        </tr>
        <tr>
            <td>NoCompressionFileType</td>
            <td>Tipi di file i cui blocchi non devono essere compresso prima di essere aggiunti all'Archivio blocchi</td>
            <td>Matrice di estensioni file</td>
            <td>Alcuni tipi di file, in particolare quelli multimediali e già compressi, non vengono compressi in modo appropriato. Questa impostazione consente la disattivazione della compressione per tali file e quindi di risparmiare risorse della CPU.</td>
        </tr>
        <tr>
            <td>OptimizeInUseFiles</td>
            <td>Se abilitata, i file con handle verranno considerati conformi ai criteri per l'ottimizzazione.</td>
            <td>True/false</td>
            <td>Abilitare questa impostazione se il carico di lavoro tiene aperti i file per lunghi periodi di tempo. Se questa impostazione non è abilitata, un file non viene mai ottimizzato se il carico di lavoro dispone di un handle aperto,&#39;anche se solo occasionalmente i dati vengono aggiunti alla fine.</td>
        </tr>
        <tr>
            <td>OptimizePartialFiles</td>
            <td>Se abilitata, il valore di MinimumFileAge si applica ai segmenti di un file anziché all'intero file.</td>
            <td>True/false</td>
            <td>Abilitare questa impostazione se il carico di lavoro funziona con file di grandi dimensioni, modificati di frequente, in cui la maggior parte del contenuto del file rimane invariato. Se questa impostazione non è abilitata, questi file non verranno mai ottimizzati poiché continuano a essere modificati, anche se la maggior parte del contenuto dei file è pronto per essere ottimizzato.</td>
        </tr>
        <tr>
            <td>Verifica</td>
            <td>Se abilitata, se l'hash di un blocco corrisponde a un blocco che è già presente nell'Archivio blocchi, i blocchi vengono confrontati byte per byte per verificare che siano identici.</td>
            <td>True/false</td>
            <td>Questa è una funzionalità di integrità che assicura che l'algoritmo hash che confronta i blocchi non commetta errori confrontando due blocchi di dati che sono effettivamente diversi ma hanno lo stesso hash. In pratica, è molto improbabile che questo si verifichi. L'abilitazione della funzionalità di verifica comporta un sovraccarico significativo per il processo di ottimizzazione.</td>
        </tr>
    </tbody>
</table>

## <a id="modifying-dedup-system-settings"></a>Modifica delle impostazioni a livello di sistema per la deduplicazione dati
La deduplicazione dei dati include impostazioni aggiuntive a livello di sistema che possono essere configurate dal [Registro di sistema](https://technet.microsoft.com/library/cc755256(v=ws.11).aspx). Queste impostazioni sono valide per tutti i processi e i volumi eseguiti nel sistema. Prestare particolare attenzione ogni volta che si modifica il Registro di sistema.

Ad esempio, è possibile disattivare completamente Garbage Collection. Altre informazioni sui motivi per cui queste operazioni possono risultare utili per lo scenario sono reperibili nelle [Domande frequenti](#faq-why-disable-full-gc). Per modificare il Registro di sistema con PowerShell:

* Se la deduplicazione dei dati è in esecuzione in un cluster:
    ```PowerShell
    Set-ItemProperty -Path HKLM:\System\CurrentControlSet\Services\ddpsvc\Settings -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    Set-ItemProperty -Path HKLM:\CLUSTER\Dedup -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    ```

* Se la deduplicazione dei dati non è in esecuzione in un cluster:
    ```PowerShell
    Set-ItemProperty -Path HKLM:\System\CurrentControlSet\Services\ddpsvc\Settings -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    ```

### <a id="modifying-dedup-system-settings-available-settings"></a>Impostazioni disponibili a livello di sistema
<table>
    <thead>
        <tr>
            <th style="min-width:125px">Nome dell'impostazione</th>
            <th>Definizione</th>
            <th>Valori accettati</th>
            <th>Motivo per cui si vuole modificare il valore</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>WlmMemoryOverPercentThreshold</td>
            <td>Questa impostazione consente ai processi di usare più memoria rispetto a quella che la deduplicazione dei dati ritiene effettivamente disponibile. Ad esempio, un'impostazione di 300 significa che il processo deve usare tre volte la memoria assegnata per essere annullato.</td>
            <td>Numeri interi positivi (il valore 300 significa 300% o 3 volte)</td>
            <td>Se si ha un'altra attività che verrà interrotta se la deduplicazione dei dati richiede più memoria</td>
        </tr>
        <tr>
            <td>DeepGCInterval</td>
            <td>Questa impostazione consente di configurare l'intervallo in cui i processi di Garbage Collection normali diventano <a href="advanced-settings.md#faq-full-v-regular-gc" data-raw-source="[full Garbage Collection jobs](advanced-settings.md#faq-full-v-regular-gc)">processi di Garbage Collection completi</a>. L'impostazione n significa ogni <sup>n</sup> processi si ha un processo di Garbage Collection completo. Nota che un Garbage Collection completo è sempre disabilitato (indipendentemente dal valore del Registro di sistema) per i volumi con il <a href="understand.md#usage-type-backup" data-raw-source="[Backup Usage Type](understand.md#usage-type-backup)">il tipo di utilizzo Backup</a>. è possibile utilizzare <code>Start-DedupJob -Type GarbageCollection -Full</code> se si desidera eseguire un'operazione di Garbage Collection completa su un volume di backup.</td>
            <td>Numeri interi (-1 indica disabilitato)</td>
            <td>Vedere <a href="advanced-settings.md#faq-why-disable-full-gc" data-raw-source="[this frequently asked question](advanced-settings.md#faq-why-disable-full-gc)">questa domanda nelle Domande frequenti</a></td>
        </tr>
    </tbody>
</table>

## <a id="faq"></a>Domande frequenti
<a id="faq-use-responsibly"></a>**È stata modificata un'impostazione di deduplicazione dati e ora i processi sono lenti o non sono finiti oppure le prestazioni del carico di lavoro sono diminuite. Perché?**  
Queste impostazioni offrono un notevole controllo sul modo in cui viene eseguita la deduplicazione dei dati. Usarle in modo responsabile e [monitorare le prestazioni](run.md#monitoring-dedup).

<a id="faq-running-dedup-jobs-manually"></a>**Si desidera eseguire un processo di deduplicazione dati in questo momento, ma non si desidera creare una nuova pianificazione. è possibile eseguire questa operazione?**  
Sì, [tutti i processi possono essere eseguiti manualmente](run.md#running-dedup-jobs-manually).

<a id="faq-full-v-regular-gc"></a>**Qual è la differenza tra Garbage Collection completa e normale?**  
Esistono due tipi di processi di [Garbage Collection](understand.md#job-info-gc):

- *Processo di Garbage Collection normale*: usa un algoritmo statistico per trovare grandi blocchi senza riferimenti che soddisfano determinati criteri (memoria e IOP ridotti). Un processo di Garbage Collection normale compatta un contenitore di archivio blocchi solo se una percentuale minima dei blocchi è priva di riferimento. Questo tipo di processo di Garbage Collection viene eseguito molto più velocemente e usa meno risorse rispetto a un processo di Garbage Collection completo. La pianificazione predefinita del processo di Garbage Collection normale è l'esecuzione una volta alla settimana.
- *Processo di Garbage Collection completo*: è un processo molto più accurato per la ricerca di blocchi senza riferimento e la liberazione di più spazio su disco. Il processo di Garbage Collection completo compatta ogni contenitore anche se solo un singolo blocco nel contenitore non ha riferimenti. Il processo di Garbage Collection completo inoltre libera sul disco spazio che può essere stato usato se si è verificato un arresto anomalo del sistema o un guasto all'alimentazione durante un processo di ottimizzazione. I processi di Garbage Collection completi ripristinano al 100% lo spazio disponibile che può essere ripristinato in un volume deduplicato richiedendo più tempo e risorse di sistema rispetto a un processo di Garbage Collection normale. Il processo di Garbage Collection completo in genere trova e rilascia fino al 5% più di dati senza riferimenti rispetto a un processo di Garbage Collection normale. La pianificazione predefinita del processo di Garbage Collection completo viene eseguita ogni quarta volta che è pianificato un processo di Garbage Collection.

<a id="faq-why-disable-full-gc"></a>**Perché si vuole disabilitare la Garbage Collection completa?**  
- I processi di Garbage Collection possono influire negativamente sulle copie shadow della durata del volume e sulle dimensioni del backup incrementale. I carichi di lavoro ad alta varianza o con numero elevato di operazioni di I/O possono subire un calo delle prestazioni a causa dei processi di Garbage Collection completi.           
- È possibile eseguire manualmente un processo di Garbage Collection completo da PowerShell per pulire le perdite se si è verificato un arresto anomalo del sistema.
