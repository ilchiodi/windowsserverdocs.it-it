---
title: Sulla selezione del tipo dell'utilità di pianificazione di hypervisor Hyper-V
description: Fornisce informazioni per amministratori di host Hyper-V sull'utilizzo dell'utilità di pianificazione di Hyper-V modalità
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: c5360d8e2fdc23f8b05c6be0f665407eebedeba2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823882"
---
# <a name="about-hyper-v-hypervisor-scheduler-type-selection"></a>Sulla selezione del tipo dell'utilità di pianificazione di hypervisor Hyper-V

Si applica a:

* Windows Server 2016
* Windows Server, versione 1709
* Windows Server, versione 1803
* Windows Server 2019

Questo documento descrive importanti modifiche valore predefinito di Hyper-V e uso di hypervisor consigliato tipi dell'utilità di pianificazione. Queste modifiche influisce sulla entrambe le prestazioni della protezione e la virtualizzazione del sistema. Gli amministratori di host di virtualizzazione devono rivedere e comprendere le modifiche e le implicazioni descritte in questo documento e valutare con attenzione l'impatto, Guida alla distribuzione suggerito e fattori di rischio coinvolti per meglio comprendere come distribuire e gestire Host Hyper-V in caso di evoluzione panorama per la sicurezza.

>[!IMPORTANT]
>Attualmente note vulnerabilità impiego più architetture di processore può essere sfruttata da una macchina virtuale guest dannosa tramite il comportamento di programmazione del tipo hypervisor legacy classico dell'utilità di pianificazione quando eseguito su host con simultaneo di sicurezza di canale laterale Multithreading (LAM) abilitato.  Se riesce a sfruttare, un carico di lavoro dannoso è stato possibile osservare i dati esterni relativi limiti di partizione. Questa classe di attacchi può essere ridotte configurando l'hypervisor Hyper-V per poter usare il tipo di utilità di pianificazione core hypervisor e il guest riconfigurare le macchine virtuali. Con l'utilità di pianificazione di base, l'hypervisor limita VPs della macchina virtuale guest per l'esecuzione dello stesso core processore fisico, di conseguenza isolamento fortemente capacità della macchina virtuale per accedere ai dati per i limiti del core fisiche in cui viene eseguito.  Si tratta di mitigazione altamente efficace contro questi attacchi al canale laterale, che impedisce l'osservazione se eventuali elementi da altre partizioni, la macchina virtuale radice o da un'altra partizione guest.  Pertanto, Microsoft sta cambiando il valore predefinito e consigliato di impostazioni di configurazione per le macchine virtuali guest e host di virtualizzazione.

## <a name="background"></a>Informazioni

A partire da Windows Server 2016, Hyper-V supporta diversi metodi di pianificazione e la gestione di processori virtuali, denominati tipi dell'utilità di pianificazione di hypervisor.  È disponibili una descrizione dettagliata di tutti i tipi dell'utilità di pianificazione di hypervisor [comprensione e l'utilizzo di tipi dell'utilità di pianificazione di hypervisor Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types).

>[!NOTE]
>Nuovi tipi di utilità di pianificazione di hypervisor furono inizialmente introdotti con Windows Server 2016 e non sono disponibili nelle versioni precedenti. Tutte le versioni di Hyper-V prima di Windows Server 2016 supportano solo l'utilità di pianificazione classico. Supporto per l'utilità di pianificazione di base solo recentemente è stato pubblicato.

## <a name="about-hypervisor-scheduler-types"></a>Informazioni sui tipi dell'utilità di pianificazione di hypervisor

Questo articolo illustra in modo specifico l'uso del nuovo hypervisor core dell'utilità di pianificazione tipo e l'utilità di pianificazione "classico" legacy e come questi tipi di utilità di pianificazione si intersecano con l'uso di Symmetric multi-thread o SMT.  È importante comprendere le differenze tra le utilità di pianificazione di base e modello di distribuzione classica e modo in cui ogni lavoro dalle VM guest nei processori di sistema sottostante.

### <a name="the-classic-scheduler"></a>L'utilità di pianificazione classico

L'utilità di pianificazione classico fa riferimento al metodo round robin fair-share, della pianificazione del lavoro su processori virtuali (VPs) nel sistema, tra cui VPs radice nonché VPs che appartengono alle macchine virtuali guest. L'utilità di pianificazione classico è stato il tipo di utilità di pianificazione predefinito utilizzato in tutte le versioni di Hyper-V (fino a Windows Server 2019, come descritto nel presente documento).  Sono ben comprese le caratteristiche delle prestazioni dell'utilità di pianificazione classica, e viene illustrata l'utilità di pianificazione classico ably supportare sottoscrizione in eccesso di carichi di lavoro: sottoscrizione in eccesso, ovvero di rapporto VP:LP dell'host con un margine adeguato (a seconda di tipi di carichi di lavoro viene virtualizzati, complessivo utilizzo delle risorse, ecc.).

Quando eseguita in un host di virtualizzazione con SMT abilitata, l'utilità di pianificazione classico pianificherà VPs guest da qualsiasi macchina virtuale in ogni thread SMT che appartengono a un core in modo indipendente. Di conseguenza, diverse macchine virtuali possono essere eseguiti nello stesso core nello stesso momento (una macchina virtuale in esecuzione in un unico thread di un core, mentre un'altra macchina virtuale è in esecuzione in altri thread).

### <a name="the-core-scheduler"></a>L'utilità di pianificazione di base

L'utilità di pianificazione di core sfrutta le proprietà di SMT per garantire l'isolamento dei carichi di lavoro guest, con un impatto sulle prestazioni del sistema sia la sicurezza. L'utilità di pianificazione di base assicura che VPs da una macchina virtuale sono pianificate sui thread SMT di pari livello. Questa operazione viene eseguita in modo simmetrico in modo che se LPs si trovano in gruppi di due, VPs vengono pianificati nei gruppi di due e un core della CPU di sistema non viene mai condiviso tra le macchine virtuali.

Pianificando VPs guest su sottostante coppie SMT, l'utilità di pianificazione di core offre un limite di sicurezza avanzata per l'isolamento del carico di lavoro e può essere utilizzato anche per ridurre variabilità delle prestazioni per carichi di lavoro sensibili di latenza.

Si noti che quando il Vice presidente del settore è pianificata per una macchina virtuale senza SMT abilitato, che VP utilizzerà l'intera base quando viene eseguita ed elemento di pari livello del core thread SMT resta inattivo.  Questo è necessario fornire l'isolamento del carico di lavoro corretta, ma influisce sulle prestazioni complessive del sistema, in particolare non appena LPs il sistema eccessiva sottoscritto -, ovvero quando è rapporto VP:LP totale supera 1:1. Pertanto, in esecuzione le macchine virtuali guest configurate senza più thread per core è una configurazione ottimale.

### <a name="benefits-of-the-using-the-core-scheduler"></a>Vantaggi dell'oggetto utilizzando l'utilità di pianificazione di base

L'utilità di pianificazione di core offre i vantaggi seguenti:

* Un limite di sicurezza avanzata per l'isolamento del carico di lavoro guest - VPs Guest sono vincolati all'esecuzione su una coppia di core fisici sottostanti, ridurre la vulnerabilità agli attacchi di snooping canale laterale.

* Riduzione del carico di lavoro variabilità - alle variazioni nella velocità effettiva del carico di lavoro Guest viene notevolmente ridotto, che offre maggiore coerenza del carico di lavoro.

* Uso di SMT in macchine virtuali - applicazioni in esecuzione nella macchina virtuale guest e il sistema operativo guest può utilizzare il comportamento SMT ed programming interface (API) per controllare e distribuire il lavoro tra thread SMT, esattamente come quando si verificavano non virtualizzata.

L'utilità di pianificazione di base è attualmente utilizzata negli host di virtualizzazione Azure, in particolare per sfruttare i vantaggi del limite di sicurezza avanzata e variabilty ridotto carico di lavoro. Microsoft ritiene che il tipo di base dell'utilità di pianificazione deve essere e continuerà a essere l'hypervisor predefinito pianificazione di tipo per la maggior parte degli scenari di virtualizzazione.  Pertanto, per garantire che ai clienti sono protetti per impostazione predefinita, Microsoft apporta la modifica per Windows Server 2019 ora.

### <a name="core-scheduler-performance-impacts-on-guest-workloads"></a>Impatto sulle prestazioni di utilità di pianificazione di base sui carichi di lavoro guest

Anche se richiesto per contrastare con efficacia determinate classi di vulnerabilità, l'utilità di pianificazione di core può potenzialmente ridurre le prestazioni. I clienti possono riscontrare una differenza nelle caratteristiche delle prestazioni con le macchine virtuali e l'impatto e la capacità di carico di lavoro complessivo del loro gli host di virtualizzazione. Nei casi in cui l'utilità di pianificazione di base deve eseguire un non - SMT Vice presidente del settore, solo uno dei flussi di istruzioni nel nucleo logico sottostante viene eseguito mentre l'altra deve rimanere inattiva. Questo ridurrà la capacità totale host per i carichi di lavoro guest.

Seguendo le indicazioni di distribuzione in questo documento possono essere ridotto al minimo questi effetti sulle prestazioni. Gli amministratori di host con attenzione devono prendere in considerazione la virtualizzazione specifica scenari di distribuzione e bilanciare la propria tolleranza ai rischi di protezione contro la necessità di densità di massimo carico di lavoro, oltre il consolidamento di host di virtualizzazione e così via.

## <a name="changes-to-the-default-and-recommended-configurations-for-windows-server-2016-and-windows-server-2019"></a>Modifiche all'impostazione predefinita e le configurazioni consigliate per Windows Server 2016 e Windows Server 2019

Distribuzione di host Hyper-V con le condizioni di garantire la massima sicurezza richiede l'uso del tipo di hypervisor core dell'utilità di pianificazione. Per garantire che ai clienti sono protetti per impostazione predefinita, Microsoft sta cambiando il valore predefinito seguente e le impostazioni consigliate.

>[!NOTE]
>Mentre il supporto interno dell'hypervisor per i tipi dell'utilità di pianificazione è stata inclusa nella versione iniziale di Windows Server 2016, Windows Server 1709 e Windows Server 1803, gli aggiornamenti sono necessari per accedere al controllo configurazione che consente di selezionare il tipo di hypervisor dell'utilità di pianificazione.  Consultare [comprensione e l'utilizzo di tipi dell'utilità di pianificazione di hypervisor Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types) per informazioni dettagliate su questi aggiornamenti.

### <a name="virtualization-host-changes"></a>Modifiche di host di virtualizzazione

* L'hypervisor utilizzerà l'utilità di pianificazione di core per impostazione predefinita a partire con Windows Server 2019.

* Microsoft reccommends configurazione l'utilità di pianificazione di core in Windows Server 2016. Il tipo di hypervisor core dell'utilità di pianificazione è supportato in Windows Server 2016, tuttavia il valore predefinito è l'utilità di pianificazione classico. L'utilità di pianificazione di base è facoltativo e deve essere abilitato in modo esplicito dall'amministratore dell'host Hyper-V.

### <a name="virtual-machine-configuration-changes"></a>Modifiche alla configurazione di macchina virtuale

* In Windows Server 2019, nuove macchine virtuali create utilizzando la versione di macchina virtuale predefinita 9.0 erediterà automaticamente le proprietà SMT (abilitato o disabilitato) dell'host di virtualizzazione. Vale a dire, se SMT è abilitato nel host fisico, appena creato le macchine virtuali avranno anche SMT abilitata ed erediteranno la topologia SMT dell'host per impostazione predefinita, con la macchina virtuale con lo stesso numero di thread di hardware per ogni core del sistema sottostante. Ciò si rifletteranno nella configurazione della macchina virtuale con HwThreadCountPerCore = 0, dove 0 indica che la macchina virtuale deve ereditare le impostazioni di SMT dell'host.

* Le macchine virtuali esistenti con una versione di macchina virtuale del 8.2 o versioni precedenti verrà mantenere l'impostazione di processore della macchina virtuale originale per HwThreadCountPerCore e il valore predefinito per gli utenti guest versione 8.2 di macchina virtuale è HwThreadCountPerCore = 1. Quando questi guest eseguito in un host Windows Server 2019, essi verranno gestiti come indicato di seguito:

    1. Se la VM ha un conteggio Vice presidente del settore che è minore o uguale al numero di core LP, la macchina virtuale verrà considerata come una VM non SMT dall'utilità di pianificazione di base. Quando la VP guest viene eseguito in un singolo thread SMT, elemento di pari livello del core thread SMT sarà inattivo. Questo è non ottimali e comporterà la riduzione complessiva delle prestazioni.

    2. Se la VM ha più VPs più core LP, la macchina virtuale viene considerata come una VM di SMT dall'utilità di pianificazione di base. Tuttavia, la macchina virtuale non verrà considerato dalla altre indicazioni che è una VM di SMT. Ad esempio, usare l'istruzione CPUID o API di Windows per eseguire query topologia della CPU per le applicazioni o del sistema operativo non viene indicato che SMT è abilitata.

* Quando una macchina virtuale esistente viene aggiornata in modo esplicito rispetto alle versioni di macchine Virtuali di versioni precedenti alla versione 9.0 tramite l'operazione di aggiornamento-VM, la macchina virtuale manterrà il valore corrente per HwThreadCountPerCore.  La macchina virtuale non sarà possibile SMT force abilitate.

* In Windows Server 2016, Microsoft consiglia l'abilitazione di SMT per le macchine virtuali guest.  Per impostazione predefinita, le macchine virtuali create in Windows Server 2016 sarebbe stato disabilitato SMT, che è che hwthreadcountpercore è impostato su 1, a meno che non modificata in modo esplicito.

>[!NOTE]
>Windows Server 2016 non supporta l'impostazione HwThreadCountPerCore su 0.

#### <a name="managing-virtual-machine-smt-configuration"></a>Gestione della configurazione di macchina virtuale SMT

Configurazione di SMT della macchina virtuale guest è impostata su una base per ogni VM. L'amministratore di host può esaminare e impostare la configurazione di SMT della macchina virtuale per selezionare una delle opzioni seguenti:

    1. Configurare le macchine virtuali per l'esecuzione come SMT-abilitato, se lo si desidera ereditare automaticamente la topologia SMT host

    2. Configurare le macchine virtuali per l'esecuzione come non-SMT

Il lt;{0}&gt SMT per una macchina virtuale viene visualizzato nei riquadri di riepilogo nella console di gestione di Hyper-V.  Configurazione delle impostazioni di SMT della macchina virtuale può essere eseguita usando le impostazioni della macchina virtuale o PowerShell.

#### <a name="configuring-vm-smt-settings-using-powershell"></a>Configurazione delle impostazioni di SMT VM usando PowerShell

Per configurare le impostazioni di SMT per una macchina virtuale guest, aprire una finestra di PowerShell con autorizzazioni sufficienti, quindi digitare:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

Dove:

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

Per leggere le impostazioni di SMT per una macchina virtuale guest, aprire una finestra di PowerShell con autorizzazioni sufficienti, quindi digitare:

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

Si noti che le macchine virtuali configurate con HwThreadCountPerCore guest = 0 indica che verranno abilitati per il guest SMT ed esporrà lo stesso numero di thread SMT al guest quando si trovano in host di virtualizzazione sottostante, in genere 2.

### <a name="guest-vms-may-observe-changes-to-cpu-topology-across-vm-mobility-scenarios"></a>Le macchine virtuali guest possono osservare le modifiche alla topologia della CPU in scenari di mobilità delle macchine Virtuali

Il sistema operativo e applicazioni in una macchina virtuale possono vedere le modifiche per gli host e le impostazioni della macchina virtuale prima e dopo gli eventi del ciclo di vita della macchina virtuale, ad esempio migrazione in tempo reale o salvare e ripristino le operazioni. Durante un'operazione in quale VM viene salvato e ripristinato lo stato, impostazione HwThreadCountPerCore della macchina virtuale sia il valore realizzato (vale a dire, calcolata combinazione di configurazione dell'host di origine e di impostazione della macchina virtuale) viene eseguita la migrazione. La macchina virtuale continuerà in esecuzione con queste impostazioni nell'host di destinazione. In corrispondenza del punto la macchina virtuale viene arrestata e riavviata, è possibile che il valore realizzato ottenuto dalla macchina virtuale verrà modificato. Deve essere non dannoso, come sistema operativo e applicazioni software di livello deve cercare le informazioni sulla topologia della CPU durante i normale avvio e l'inizializzazione i flussi del codice. Tuttavia, poiché queste sequenze vengono ignorate durante la migrazione o salvare/ripristinare operazioni attive, le macchine virtuali che vengono sottoposti a queste transizioni di stato inizializzazione in fase di avvio è stato possibile osservare originariamente calcolato realizzata valore fino a quando non vengano arrestate e nuovamente avviato.  

### <a name="alerts-regarding-non-optimal-vm-configurations"></a>Avvisi relativi alle configurazioni non ottimali delle macchine Virtuali

Macchine virtuali configurate con altre VPs rispetto a quanti sono i core fisici del risultato di host in una configurazione non ottimali. L'utilità di pianificazione di hypervisor considera queste macchine virtuali come se fossero in grado di riconoscere SMT. Tuttavia, sistema operativo e il software dell'applicazione nella macchina virtuale verrà visualizzato una topologia di CPU che mostra SMT è disabilitato. Quando viene rilevata questa condizione, il processo di lavoro Hyper-V registrerà un evento nell'host di virtualizzazione, l'amministratore di host di avviso che la configurazione della macchina virtuale è non ottimali e consigliare SMT essere abilitato per la macchina virtuale.

#### <a name="how-to-identify-non-optimally-configured-vms"></a>Come identificare in modo non ottimale configurate macchine virtuali

È possibile identificare non - SMT VM esaminando il Registro di sistema nel Visualizzatore eventi per l'evento di processo di lavoro di Hyper-V 3498 ID, che verrà attivato per una macchina virtuale ogni volta che il numero di VPs nella macchina virtuale è maggiore del numero di core fisici. Eventi del processo di lavoro possono essere ottenuti dal Visualizzatore eventi o tramite PowerShell.

#### <a name="querying-the-hyper-v-worker-process-vm-event-using-powershell"></a>L'esecuzione di query l'evento di macchina virtuale del processo ruolo di lavoro Hyper-V con PowerShell

Per eseguire query per l'evento di processo di lavoro Hyper-V ID 3498 usando PowerShell, immettere i comandi seguenti da un prompt di PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### <a name="impacts-of-guest-smt-configuaration-on-the-use-of-hypervisor-enlightenments-for-guest-operating-systems"></a>Impatto del guest SMT lt;{0}&gt sull'utilizzo degli hypervisor gli enlightenment per i sistemi operativi guest

L'hypervisor di Microsoft offre gli enlightenment più o suggerimenti, che il sistema operativo in esecuzione in una macchina virtuale guest può eseguire una query e usare per attivare le ottimizzazioni, ad esempio quelle che potrebbero trarre vantaggio delle prestazioni o in caso contrario, migliorare la gestione delle diverse condizioni durante l'esecuzione virtualizzati. Illuminazione introdotte di recente una riguarda la gestione della pianificazione dei processori virtuali e l'uso di soluzioni di prevenzione del sistema operativo per attacchi al canale laterale che sfruttano SMT.

>[!NOTE]
>È consigliabile che gli amministratori di host abilitare SMT per le macchine virtuali guest ottimizzare le prestazioni del carico di lavoro.

I dettagli di illuminazione questo guest sono fornite di seguito, tuttavia le informazioni chiave per gli amministratori di host di virtualizzazione sono che le macchine virtuali deve avere HwThreadCountPerCore configurato in modo da corrispondere la configurazione dell'host fisica SMT. In questo modo l'hypervisor segnalare che non vi sia alcun core non dell'architettura di condivisione. Pertanto, è possibile abilitare le ottimizzazioni di supporto del sistema operativo guest che richiedono l'illuminazione. Windows Server 2019, creare nuove macchine virtuali e lasciare il valore predefinito di HwThreadCountPerCore (0). Eseguire la migrazione di macchine virtuali meno recenti da Windows Server 2016 gli host possono essere aggiornati alla versione di configurazione di Windows Server 2019. Al termine dell'operazione, Microsoft consiglia di impostare HwThreadCountPerCore = 0.  In Windows Server 2016, Microsoft consiglia di impostazione HwThreadCountPerCore corrispondere la configurazione dell'host (in genere 2).

### <a name="nononarchitecturalcoresharing-enlightenment-details"></a>Dettagli di illuminazione NoNonArchitecturalCoreSharing

A partire da Windows Server 2016, l'hypervisor definisce un nuovo illuminazione per descrivere la gestione della pianificazione Vice presidente del settore e il posizionamento del sistema operativo guest. Questo illuminazione è definito nel [v5.0c specifica funzionale Hypervisor Top Level](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs).

Foglia CPUID sintetica di hypervisor CPUID.0x40000004.EAX:18[NoNonArchitecturalCoreSharing = 1] indica che un processore virtuale mai condividono un core fisico con un altro processore virtuale, ad eccezione di processori virtuali che vengono segnalati come elemento di pari livello SMT thread. Ad esempio, Vice presidente del settore un guest non verrà mai eseguito su un thread SMT insieme a una radice Vice presidente del settore esecuzione contemporanea su un elemento di pari livello thread SMT dello stesso core processore. Questa condizione è possibile solo quando in esecuzione virtualizzato e pertanto rappresenta un comportamento SMT non architetturale che dispone anche di compromettere gravemente la sicurezza. Il sistema operativo guest può usare NoNonArchitecturalCoreSharing = 1, per indicare che è possibile abilitare le ottimizzazioni, che può essere utile, evitare l'overhead delle prestazioni di impostazione STIBP.

In alcune configurazioni, l'hypervisor non viene indicato che NoNonArchitecturalCoreSharing = 1. Ad esempio, se un host dispone SMT abilitato ed è configurato per usare l'utilità di pianificazione hypervisor classico, NoNonArchitecturalCoreSharing sarà pari a 0. Questo potrebbe impedire l'abilitazione di alcune ottimizzazioni abilitate per gli utenti guest. Pertanto, è consigliabile che gli amministratori di host usando SMT si basano sull'utilità di pianificazione core hypervisor e assicurarsi che le macchine virtuali siano configurate in modo da ereditare la configurazione di SMT dall'host per garantire prestazioni ottimali del carico di lavoro.

## <a name="summary"></a>Riepilogo

Il panorama delle minacce di sicurezza continua a evolvere. Per assicurarsi che i nostri clienti sono protetti per impostazione predefinita, Microsoft sta modificando la configurazione predefinita per l'hypervisor e delle macchine virtuali di avvio in Windows Server 2019 Hyper-V e fornendo aggiornato indicazioni e consigli per i clienti che eseguono Windows Server 2016 Hyper-V. Gli amministratori di host di virtualizzazione devono:

* Leggere e comprendere le indicazioni fornite in questo documento

* Valutare con attenzione e regolare le distribuzioni di virtualizzazione per garantire che soddisfino la sicurezza, prestazioni, virtualizzazione prevista e gli obiettivi di tempi di risposta del carico di lavoro per requisiti aziendali specifici

* Provare a ripetere la configurazione host Windows Server 2016 Hyper-V esistenti per sfruttare i vantaggi di sicurezza avanzata offerti dall'utilità di pianificazione di base hypervisor

* Aggiornamento non - SMT le macchine virtuali esistenti in modo da ridurre l'impatto sulle prestazioni dalla pianificazione vincoli imposti dall'isolamento Vice presidente del settore in grado di risolvere le vulnerabilità di sicurezza hardware
