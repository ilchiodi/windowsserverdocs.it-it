---
title: Informazioni sulla selezione del tipo di utilità di pianificazione hypervisor Hyper-V
description: Fornisce informazioni per gli amministratori di host Hyper-V sull'utilizzo delle modalità di pianificazione di Hyper-V
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: 1e77535548cccd1c821163dabbad381f35d2948a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872064"
---
# <a name="about-hyper-v-hypervisor-scheduler-type-selection"></a>Informazioni sulla selezione del tipo di utilità di pianificazione hypervisor Hyper-V

Si applica a:

* Windows Server 2016
* Windows Server, versione 1709
* Windows Server, versione 1803
* Windows Server 2019

Questo documento descrive importanti modifiche apportate all'uso predefinito e consigliato di Hyper-V dei tipi di utilità di pianificazione hypervisor. Queste modifiche influiscano sulle prestazioni della sicurezza del sistema e della virtualizzazione. Gli amministratori dell'host di virtualizzazione devono rivedere e comprendere le modifiche e le implicazioni descritte in questo documento e valutare con attenzione gli effetti, le linee guida per la distribuzione e i fattori di rischio interessati per comprendere meglio la modalità di distribuzione e gestione Host Hyper-V in presenza di un panorama di sicurezza a modifica rapida.

>[!IMPORTANT]
>Attualmente le vulnerabilità della sicurezza del canale laterale note rilevate in più architetture di processori possono essere sfruttate da una macchina virtuale Guest dannosa tramite il comportamento di pianificazione del tipo di utilità di pianificazione classica dell'hypervisor legacy, quando vengono eseguite in host con simultaneamente Multithreading (SMT) abilitato.  Se sfruttate correttamente, un carico di lavoro dannoso può osservare i dati all'esterno del limite della partizione. Questa classe di attacchi può essere mitigata configurando l'hypervisor Hyper-V in modo da usare il tipo di utilità di pianificazione Core hypervisor e riconfigurando le macchine virtuali guest. Con l'utilità di pianificazione principale, l'hypervisor limita l'esecuzione del VPs di una macchina virtuale Guest nello stesso core del processore fisico, isolando quindi in modo sicuro la capacità della VM di accedere ai dati ai limiti del nucleo fisico su cui è in esecuzione.  Si tratta di una mitigazione molto efficace contro questi attacchi del canale laterale, che impedisce alla macchina virtuale di osservare gli artefatti da altre partizioni, sia che si tratti della radice o di un'altra partizione guest.  Microsoft modifica quindi le impostazioni di configurazione predefinite e consigliate per gli host di virtualizzazione e le macchine virtuali guest.

## <a name="background"></a>Sfondo

A partire da Windows Server 2016, Hyper-V supporta diversi metodi di pianificazione e gestione dei processori virtuali, denominati tipi di utilità di pianificazione hypervisor.  Una descrizione dettagliata di tutti i tipi di utilità di pianificazione hypervisor è reperibile in [informazioni e uso dei tipi di utilità di pianificazione hypervisor Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types).

>[!NOTE]
>I nuovi tipi di utilità di pianificazione hypervisor sono stati introdotti per la prima volta con Windows Server 2016 e non sono disponibili nelle versioni precedenti. Tutte le versioni di Hyper-V precedenti a Windows Server 2016 supportano solo l'utilità di pianificazione classica. Il supporto per l'utilità di pianificazione principale è stato pubblicato di recente.

## <a name="about-hypervisor-scheduler-types"></a>Informazioni sui tipi di utilità di pianificazione hypervisor

Questo articolo è incentrato specificamente sull'uso del nuovo tipo di utilità di pianificazione Core hypervisor rispetto all'utilità di pianificazione "classica" e sul modo in cui questi tipi di utilità di pianificazione si intersecano con l'uso del multithreading simmetrico o di SMT.  È importante comprendere le differenze tra le utilità di pianificazione di base e quelle classiche e il modo in cui ogni posizione lavora dalle macchine virtuali guest nei processori di sistema sottostanti.

### <a name="the-classic-scheduler"></a>Utilità di pianificazione classica

L'utilità di pianificazione classica si riferisce alla condivisione equa, round robin metodo di pianificazione del lavoro sui processori virtuali (VPs) in tutto il sistema, incluso il VPs radice e il VPs appartenente alle macchine virtuali guest. L'utilità di pianificazione classica è stata il tipo di utilità di pianificazione predefinito usato in tutte le versioni di Hyper-V (fino a Windows Server 2019, come descritto nel presente documento).  Le caratteristiche di prestazioni dell'utilità di pianificazione classica sono ben comprese e l'utilità di pianificazione classica è stata illustrata in modo da supportare con maggiore sottoscrizione i carichi di lavoro, ovvero l'oversubscription del rapporto VP: LP dell'host per un margine ragionevole (a seconda del tipi di carichi di lavoro virtualizzati, utilizzo complessivo delle risorse e così via.

Quando viene eseguito in un host di virtualizzazione con SMT abilitato, l'utilità di pianificazione classica Pianifica il VPs Guest da qualsiasi macchina virtuale in ogni thread SMT appartenente a un core in modo indipendente. È pertanto possibile che diverse macchine virtuali siano in esecuzione nello stesso Core nello stesso momento, ovvero una macchina virtuale in esecuzione in un thread di un core mentre un'altra VM è in esecuzione nell'altro thread.

### <a name="the-core-scheduler"></a>Utilità di pianificazione principale

L'utilità di pianificazione principale sfrutta le proprietà di SMT per garantire l'isolamento dei carichi di lavoro Guest, che influiscano sulle prestazioni di sicurezza e del sistema. L'utilità di pianificazione principale garantisce che il VPs da una macchina virtuale venga pianificato in thread di pari livello SMT. Questa operazione viene eseguita in modo simmetrico, in modo che se LPs si trovano in gruppi di due, i VPs sono pianificati in gruppi di due e un core CPU di sistema non viene mai condiviso tra le macchine virtuali.

Pianificando il VPs Guest sulle coppie SMT sottostanti, l'utilità di pianificazione Core offre un limite di sicurezza forte per l'isolamento del carico di lavoro e può essere usato anche per ridurre la variabilità delle prestazioni per i carichi di lavoro sensibili alla latenza.

Si noti che quando il VP è pianificato per una macchina virtuale senza SMT abilitato, il VP utilizzerà l'intero nucleo durante l'esecuzione e il thread SMT di pari livello di base verrà lasciato inattivo.  Questa operazione è necessaria per fornire l'isolamento del carico di lavoro corretto, ma influisca sulle prestazioni complessive del sistema, soprattutto quando il sistema LPs viene sottoposto a oversubscription, ovvero quando il rapporto totale VP: LP supera 1:1. Pertanto, l'esecuzione di macchine virtuali guest configurate senza più thread per core è una configurazione non ottimale.

### <a name="benefits-of-the-using-the-core-scheduler"></a>Vantaggi dell'uso dell'utilità di pianificazione principale

L'utilità di pianificazione principale offre i vantaggi seguenti:

* Un limite di sicurezza forte per l'isolamento del carico di lavoro Guest: il VPs Guest è vincolato per l'esecuzione su coppie di core fisiche sottostanti, riducendo la vulnerabilità agli attacchi di snooping del canale laterale.

* Minore variabilità del carico di lavoro: la variabilità della velocità effettiva del carico di lavoro Guest è notevolmente ridotta e offre maggiore coerenza del carico

* Uso di SMT nelle VM Guest: il sistema operativo e le applicazioni in esecuzione nella macchina virtuale guest possono usare il comportamento SMT e le interfacce di programmazione (API) per controllare e distribuire il lavoro tra thread SMT, proprio come se fossero eseguiti in modalità non virtualizzata.

L'utilità di pianificazione principale è attualmente usata negli host di virtualizzazione di Azure, in particolare per sfruttare i vantaggi del limite di sicurezza forte e del carico di lavoro variabilty basso. Microsoft considera che il tipo di utilità di pianificazione principale deve essere e continuerà a essere il tipo di pianificazione hypervisor predefinito per la maggior parte degli scenari di virtualizzazione.  Pertanto, per garantire che i clienti siano protetti per impostazione predefinita, Microsoft sta apportando questa modifica per Windows Server 2019 ora.

### <a name="core-scheduler-performance-impacts-on-guest-workloads"></a>Effetti sulle prestazioni dell'utilità di pianificazione di base sui carichi di lavoro Guest

Sebbene sia necessario per attenuare efficacemente determinate classi di vulnerabilità, è possibile che l'utilità di pianificazione principale riduca anche le prestazioni. I clienti possono notare una differenza nelle caratteristiche di prestazioni con le proprie macchine virtuali e influiscano sulla capacità complessiva del carico di lavoro degli host di virtualizzazione. Nei casi in cui l'utilità di pianificazione principale deve eseguire un VP non SMT, solo uno dei flussi di istruzioni nel core logico sottostante viene eseguito mentre l'altro deve essere lasciato inattivo. Questa operazione limiterà la capacità totale dell'host per i carichi di lavoro Guest.

È possibile ridurre al minimo questi effetti sulle prestazioni seguendo le istruzioni per la distribuzione di questo documento. Gli amministratori host devono considerare attentamente gli scenari di distribuzione di virtualizzazione specifici e bilanciare la loro tolleranza per i rischi di sicurezza in base alla necessità di densità massima del carico di lavoro, di consolidamento degli host di virtualizzazione, ecc.

## <a name="changes-to-the-default-and-recommended-configurations-for-windows-server-2016-and-windows-server-2019"></a>Modifiche alle configurazioni predefinite e consigliate per Windows Server 2016 e Windows Server 2019

Per la distribuzione di host Hyper-V con il comportamento di sicurezza massimo, è necessario usare il tipo di utilità di pianificazione Core hypervisor. Per garantire che i clienti siano protetti per impostazione predefinita, Microsoft sta cambiando le impostazioni predefinite e consigliate seguenti.

>[!NOTE]
>Anche se il supporto interno dell'hypervisor per i tipi di utilità di pianificazione è stato incluso nella versione iniziale di Windows Server 2016, Windows Server 1709 e Windows Server 1803, gli aggiornamenti sono necessari per accedere al controllo di configurazione che consente di selezionare il tipo di utilità di pianificazione hypervisor.  Per informazioni dettagliate su questi aggiornamenti, vedere [informazioni e utilizzo dei tipi di utilità di pianificazione hypervisor Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types) .

### <a name="virtualization-host-changes"></a>Modifiche dell'host di virtualizzazione

* Per impostazione predefinita, l'hypervisor userà l'utilità di pianificazione principale a partire da Windows Server 2019.

* Microsoft reccommends configurazione dell'utilità di pianificazione principale in Windows Server 2016. Il tipo di utilità di pianificazione Core hypervisor è supportato in Windows Server 2016, ma il valore predefinito è l'utilità di pianificazione classica. L'utilità di pianificazione principale è facoltativa e deve essere abilitata in modo esplicito dall'amministratore dell'host Hyper-V.

### <a name="virtual-machine-configuration-changes"></a>Modifiche alla configurazione della macchina virtuale

* In Windows Server 2019 le nuove macchine virtuali create con la versione VM predefinita 9,0 erediteranno automaticamente le proprietà SMT (abilitate o disabilitate) dell'host di virtualizzazione. Ovvero, se SMT è abilitato nell'host fisico, per le macchine virtuali appena create sarà abilitata anche la funzionalità SMT e la topologia SMT dell'host verrà ereditata per impostazione predefinita, con la macchina virtuale con lo stesso numero di thread hardware per core del sistema sottostante. Questa operazione verrà applicata alla configurazione della macchina virtuale con HwThreadCountPerCore = 0, dove 0 indica che la macchina virtuale deve ereditare le impostazioni SMT dell'host.

* Le macchine virtuali esistenti con una versione VM 8,2 o precedenti manterranno la relativa impostazione del processore di VM originale per HwThreadCountPerCore e il valore predefinito per 8,2 VM versione Guest è HwThreadCountPerCore = 1. Quando questi Guest vengono eseguiti in un host Windows Server 2019, verranno trattati come segue:

    1. Se la macchina virtuale ha un numero di VP minore o uguale al numero di core LP, la macchina virtuale verrà considerata come una macchina virtuale non SMT dall'utilità di pianificazione di base. Quando il VP Guest viene eseguito in un singolo thread SMT, il thread SMT di pari livello di base verrà inattivo. Si tratta di una soluzione non ottimale che comporterà una perdita complessiva delle prestazioni.

    2. Se la macchina virtuale ha più VPs rispetto a LP core, la macchina virtuale verrà trattata come macchina virtuale SMT dall'utilità di pianificazione principale. Tuttavia, la macchina virtuale non osserverà altre indicazioni che si tratta di una macchina virtuale SMT. Ad esempio, l'uso dell'istruzione CPUID o delle API Windows per eseguire query sulla topologia della CPU da parte del sistema operativo o delle applicazioni non indicherà che SMT è abilitato.

* Quando una macchina virtuale esistente viene aggiornata in modo esplicito dalle versioni della VM precedente alla versione 9,0 tramite l'operazione Update-VM, la VM manterrà il valore corrente per HwThreadCountPerCore.  Per la macchina virtuale non sarà abilitata la forza di SMT.

* In Windows Server 2016, Microsoft consiglia di abilitare SMT per le macchine virtuali guest.  Per impostazione predefinita, le macchine virtuali create in Windows Server 2016 avranno disabilitato SMT, ovvero HwThreadCountPerCore è impostato su 1, a meno che non sia stato modificato in modo esplicito.

>[!NOTE]
>Windows Server 2016 non supporta l'impostazione di HwThreadCountPerCore su 0.

#### <a name="managing-virtual-machine-smt-configuration"></a>Gestione della configurazione SMT della macchina virtuale

La configurazione SMT della macchina virtuale guest viene impostata in base alla VM. L'amministratore host può ispezionare e configurare la configurazione SMT di una macchina virtuale per selezionare una delle opzioni seguenti:

    1. Configurare le macchine virtuali per l'esecuzione come abilitata per SMT, ereditando facoltativamente la topologia SMT dell'host automaticamente

    2. Configurare le macchine virtuali per l'esecuzione come non SMT

Il configurazione SMT per una macchina virtuale viene visualizzato nei riquadri di riepilogo nella console di gestione di Hyper-V.  La configurazione delle impostazioni SMT di una macchina virtuale può essere eseguita usando le impostazioni della macchina virtuale o PowerShell.

#### <a name="configuring-vm-smt-settings-using-powershell"></a>Configurazione delle impostazioni SMT della macchina virtuale con PowerShell

Per configurare le impostazioni di SMT per una macchina virtuale Guest, aprire una finestra di PowerShell con autorizzazioni sufficienti e digitare:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

Dove:

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

Per leggere le impostazioni di SMT per una macchina virtuale Guest, aprire una finestra di PowerShell con autorizzazioni sufficienti e digitare:

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

Si noti che le macchine virtuali guest configurate con HwThreadCountPerCore = 0 indicano che SMT verrà abilitato per il Guest e esporrà lo stesso numero di thread SMT al Guest così come sono nell'host di virtualizzazione sottostante, in genere 2.

### <a name="guest-vms-may-observe-changes-to-cpu-topology-across-vm-mobility-scenarios"></a>Le macchine virtuali guest possono osservare le modifiche alla topologia della CPU in scenari di mobilità delle VM

Il sistema operativo e le applicazioni in una macchina virtuale possono visualizzare modifiche alle impostazioni host e VM prima e dopo gli eventi del ciclo di vita della macchina virtuale, ad esempio le operazioni di migrazione in tempo reale o salvataggio e ripristino. Durante un'operazione in cui viene salvato e ripristinato lo stato della macchina virtuale, viene eseguita la migrazione dell'impostazione HwThreadCountPerCore della macchina virtuale e del valore realizzato, ovvero la combinazione calcolata dell'impostazione della macchina virtuale e della configurazione dell'host di origine. La VM continuerà a essere eseguita con queste impostazioni nell'host di destinazione. Nel momento in cui la macchina virtuale viene arrestata e riavviata, è possibile che il valore realizzato osservato dalla macchina virtuale venga modificato. Questo comportamento deve essere benigno, poiché il software a livello di sistema operativo e applicazione deve cercare le informazioni sulla topologia della CPU nell'ambito dei normali flussi di codice di avvio e di inizializzazione. Tuttavia, poiché queste sequenze di inizializzazione del tempo di avvio vengono ignorate durante le operazioni di migrazione in tempo reale o di salvataggio/ripristino, le macchine virtuali sottoposte a queste transizioni di stato potrebbero osservare il valore realizzato inizialmente calcolato fino a quando non vengono arrestate e riavviate.  

### <a name="alerts-regarding-non-optimal-vm-configurations"></a>Avvisi relativi alle configurazioni di VM non ottimali

Le macchine virtuali configurate con un numero di VPs superiore al numero di core fisici nell'host generano una configurazione non ottimale. L'utilità di pianificazione hypervisor considererà queste macchine virtuali come se fossero compatibili con SMT. Tuttavia, il software del sistema operativo e dell'applicazione nella macchina virtuale verrà presentato una topologia della CPU che mostra SMT è disabilitata. Quando viene rilevata questa condizione, il processo di lavoro di Hyper-V registrerà un evento nell'host di virtualizzazione che avvisa l'amministratore dell'host che la configurazione della macchina virtuale non è ottimale e che è consigliabile abilitare SMT per la macchina virtuale.

#### <a name="how-to-identify-non-optimally-configured-vms"></a>Come identificare le macchine virtuali non configurate in modo ottimale

È possibile identificare le macchine virtuali non SMT esaminando il registro di sistema in Visualizzatore eventi per il processo di lavoro Hyper-V ID evento 3498, che verrà attivato per una VM ogni volta che il numero di VPs nella macchina virtuale è superiore al numero di core fisico. Gli eventi del processo di lavoro possono essere ottenuti da Visualizzatore eventi o tramite PowerShell.

#### <a name="querying-the-hyper-v-worker-process-vm-event-using-powershell"></a>Esecuzione di query sull'evento VM del processo di lavoro Hyper-V tramite PowerShell

Per eseguire una query per il processo di lavoro Hyper-V ID evento 3498 usando PowerShell, immettere i comandi seguenti da un prompt di PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### <a name="impacts-of-guest-smt-configuaration-on-the-use-of-hypervisor-enlightenments-for-guest-operating-systems"></a>Effetti dell'configurazione SMT Guest sull'uso di illuminamenti hypervisor per i sistemi operativi guest

L'hypervisor Microsoft offre più illuminamenti, o hint, che il sistema operativo in esecuzione in una macchina virtuale guest può eseguire query e usare per attivare le ottimizzazioni, ad esempio quelle che potrebbero trarre vantaggio dalle prestazioni o migliorare la gestione di varie condizioni durante l'esecuzione virtualizzato. Un chiarimento introdotto di recente riguarda la gestione della pianificazione del processore virtuale e l'uso di mitigazioni del sistema operativo per gli attacchi del canale laterale che sfruttano SMT.

>[!NOTE]
>Microsoft consiglia agli amministratori host di abilitare SMT per le macchine virtuali guest per ottimizzare le prestazioni del carico di lavoro.

Di seguito sono riportati i dettagli di questa illuminazione Guest, tuttavia la chiave per gli amministratori degli host di virtualizzazione è che le macchine virtuali devono avere HwThreadCountPerCore configurate in modo che corrispondano alla configurazione SMT fisica dell'host. Questo consente all'hypervisor di segnalare che non è presente alcuna condivisione di base non architetturale. Pertanto, qualsiasi sistema operativo guest che supporta le ottimizzazioni che richiedono l'illuminazione potrebbe essere abilitato. In Windows Server 2019, creare nuove macchine virtuali e lasciare il valore predefinito HwThreadCountPerCore (0). È possibile aggiornare le macchine virtuali precedenti migrate da host Windows Server 2016 alla versione di configurazione di Windows Server 2019. Al termine di questa operazione, Microsoft consiglia di impostare HwThreadCountPerCore = 0.  In Windows Server 2016, Microsoft consiglia di impostare HwThreadCountPerCore in modo che corrisponda alla configurazione host (in genere 2).

### <a name="nononarchitecturalcoresharing-enlightenment-details"></a>Dettagli sull'illuminazione NoNonArchitecturalCoreSharing

A partire da Windows Server 2016, l'hypervisor definisce una nuova illuminazione per descrivere la gestione della pianificazione e del posizionamento del VP al sistema operativo guest. Questa illuminazione è definita nella [specifica funzionale hypervisor di primo livello v 5.0 c](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs).

Hypervisor sintetico CPUID foglia CPUID. 0x40000004. EAX: 18 [NoNonArchitecturalCoreSharing = 1] indica che un processore virtuale non condividerà mai un core fisico con un altro processore virtuale, ad eccezione dei processori virtuali segnalati come elementi di pari livello SMT thread. Ad esempio, un VP Guest non viene mai eseguito su un thread SMT insieme a un VP radice che esegue contemporaneamente in un thread SMT di pari livello sullo stesso core del processore. Questa condizione è possibile solo quando l'esecuzione è virtualizzata e pertanto rappresenta un comportamento SMT non architettonico che presenta anche gravi implicazioni di sicurezza. Il sistema operativo guest può usare NoNonArchitecturalCoreSharing = 1 per indicare che è sicuro abilitare le ottimizzazioni, che può essere utile per evitare il sovraccarico delle prestazioni dell'impostazione di STIBP.

In alcune configurazioni, l'hypervisor non indicherà che NoNonArchitecturalCoreSharing = 1. Ad esempio, se per un host è abilitato SMT ed è configurato per l'uso dell'utilità di pianificazione classica dell'hypervisor, NoNonArchitecturalCoreSharing sarà 0. Questo potrebbe impedire ai Guest illuminati di abilitare determinate ottimizzazioni. Pertanto, Microsoft consiglia che gli amministratori host che usano SMT si affidino all'utilità di pianificazione principale dell'hypervisor e garantiscano che le macchine virtuali siano configurate per ereditare la configurazione SMT dall'host per garantire prestazioni ottimali del carico di lavoro.

## <a name="summary"></a>Riepilogo

Il panorama delle minacce per la sicurezza continua a evolversi. Per garantire che i clienti siano protetti per impostazione predefinita, Microsoft sta cambiando la configurazione predefinita per l'hypervisor e le macchine virtuali a partire da Windows Server 2019 Hyper-V e fornisce indicazioni e consigli aggiornati per i clienti che eseguono Windows Server 2016 Hyper-V. Gli amministratori dell'host di virtualizzazione devono:

* Leggere e comprendere le indicazioni fornite in questo documento

* Valutare e modificare con attenzione le distribuzioni di virtualizzazione per assicurarsi che soddisfino gli obiettivi di sicurezza, prestazioni, densità di virtualizzazione e velocità di risposta del carico di lavoro per i requisiti univoci

* Valutare la possibilità di riconfigurare gli host Hyper-V esistenti di Windows Server 2016 per sfruttare i vantaggi di sicurezza avanzati offerti dall'utilità di pianificazione Core hypervisor

* Aggiornare le macchine virtuali non SMT esistenti per ridurre l'effetto sulle prestazioni della pianificazione dei vincoli imposti dall'isolamento VP che risolve le vulnerabilità della sicurezza hardware
