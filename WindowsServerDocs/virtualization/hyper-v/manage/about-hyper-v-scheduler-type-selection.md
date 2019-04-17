---
title: Sulla selezione tipo di utilità di pianificazione di hypervisor Hyper-V
description: Fornisce informazioni per gli amministratori di host Hyper-V sull'utilizzo di utilità di pianificazione di Hyper-V modalità
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: c5360d8e2fdc23f8b05c6be0f665407eebedeba2
ms.sourcegitcommit: 546229d6b5fa7e16f725c6c35f4dcc272711b811
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/18/2018
ms.locfileid: "4905128"
---
# Sulla selezione tipo di utilità di pianificazione di hypervisor Hyper-V

Si applica a:

* WindowsServer 2016
* Windows Server, versione 1709
* Windows Server, versione 1803
* Windows Server 2019

Questo documento descrive le modifiche importanti per impostazione predefinita di Hyper-V e utilizzo di hypervisor consigliato tipi di utilità di pianificazione. Queste modifiche hanno effetto entrambe le prestazioni del sistema di sicurezza e la virtualizzazione. Gli amministratori di host di virtualizzazione devono esaminare e comprendere le modifiche e implicazioni descritte in questo documento e valutare attentamente l'impatto, Guida alla distribuzione suggeriti e fattori di rischio coinvolte comprendere meglio come distribuire e gestire Host Hyper-V per affrontare il rapida evoluzione panorama di sicurezza.

>[!IMPORTANT]
>Attualmente note sicurezza canale vulnerabilità avvistate in più architetture del processore può essere utilizzata dalla macchina virtuale guest dannoso tramite il comportamento di programmazione del tipo di utilità di pianificazione classico hypervisor legacy quando viene eseguito su host con Simultaneous Multithreading (SMT) abilitato.  Sfruttando, un carico di lavoro dannoso potrebbe osservare i dati all'esterno il cui limite è partizione. Questa classe di attacchi possa essere evitata configurando l'hypervisor Hyper-V per usare il tipo di utilità di pianificazione di hypervisor core e guest riconfigurare le macchine virtuali. Con l'utilità di pianificazione di base, l'hypervisor limita VPs di un guest della macchina virtuale esecuzione nello stesso core del processore fisico, pertanto isolando fortemente possibilità della macchina virtuale per accedere ai dati per i confini di core fisico in cui viene eseguita.  Questo è un molto efficaci contro questi attacchi di canale, che impedisce la macchina virtuale di osservazione tutti gli elementi da altre partizioni, indica se la radice o un altro partizione guest.  Di conseguenza, Microsoft sta cambiando il valore predefinito e consiglia le impostazioni di configurazione per le macchine virtuali guest e host di virtualizzazione.

## Background

A partire da Windows Server 2016, Hyper-V supporta diversi metodi di programmazione e gestione dei processori virtuali, chiamati tipi di utilità di pianificazione hypervisor.  Una descrizione dettagliata di tutti i tipi di utilità di pianificazione hypervisor è reperibile in [comprensione e utilizzo di tipi di utilità di pianificazione hypervisor Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types).

>[!NOTE]
>Nuovi tipi di utilità di pianificazione hypervisor prima di tutto introdotte con Windows Server 2016 e non sono disponibili nelle versioni precedenti. Tutte le versioni di Hyper-V prima di Windows Server 2016 supportano solo l'utilità di pianificazione classico. Supporto per l'utilità di pianificazione di base solo di recente è stato pubblicato.

## Informazioni sui tipi di utilità di pianificazione hypervisor

Questo articolo è incentrato particolare attenzione all'uso del nuovo tipo di utilità di pianificazione hypervisor core rispetto a utilità di pianificazione "classico" legacy e come questi tipi di utilità di pianificazione interagiscono con l'uso di simmetrica il multithreading o SMT.  È importante capire le differenze tra le utilità di pianificazione core e classica e come ogni posiziona il lavoro da macchine virtuali guest su processori il sistema sottostante.

### L'utilità di pianificazione classico

L'utilità di pianificazione classico si riferisce al metodo round robin sull'equa condivisione, della programmazione di lavoro su processori () in tutto il sistema, tra cui VPs radice, nonché VPs appartenenti alle macchine virtuali guest. L'utilità di pianificazione classico è stata il tipo di utilità di pianificazione predefinito utilizzato in tutte le versioni di Hyper-V (fino a Windows Server 2019, come descritto di seguito).  Le caratteristiche delle prestazioni dell'utilità di pianificazione classica siano comprensibili e l'utilità di pianificazione classico è illustrato ably supportare sottoscrizione un eccesso di carichi di lavoro, vale a dire eccessiva di rapporto VP:LP dell'host da un margine ragionevole (a seconda di tipi di carichi di lavoro viene virtualizzati, complessiva utilizzo delle risorse, ecc.).

Quando vengono eseguite in un host di virtualizzazione con SMT abilitata, l'utilità di pianificazione classica verranno pianificati VPs guest da qualsiasi macchina virtuale in ogni thread SMT che appartengono a un core in modo indipendente. Di conseguenza, diverse macchine virtuali possono essere in esecuzione nello stesso core allo stesso tempo (una macchina virtuale in esecuzione su un thread di un core mentre un'altra macchina virtuale è in esecuzione su altri thread).

### L'utilità di pianificazione di base

L'utilità di pianificazione core sfrutta le proprietà di SMT per fornire l'isolamento di carichi di lavoro guest, quale impatto la sicurezza e le prestazioni del sistema. L'utilità di pianificazione core garantisce che VPs da una macchina virtuale pianificati in thread SMT di pari livello. Questa operazione viene eseguita in modo simmetrico in modo che se processori logici sono gruppi di due, VPs vengono pianificati in gruppi di due e un core CPU sistema non viene mai condivisa tra le macchine virtuali.

Pianificando VPs guest su coppie SMT sottostante, l'utilità di pianificazione core offre un limite di sicurezza avanzata per l'isolamento del carico di lavoro e può essere usato anche per ridurre la variabilità delle prestazioni per carichi di lavoro sensibili latenza.

Tieni presente che quando il Vicepresidente è pianificato per una macchina virtuale senza SMT abilitata, che Vicepresidente utilizzerà il nucleo intero quando viene eseguita e pari livello del core thread SMT rimane inattivo.  Questo è necessario fornire l'isolamento del carico di lavoro corretto, ma incide prestazioni complessive del sistema, in particolare che i processori logici sistema diventano eccessiva sottoscritti, vale a dire quando rapporto VP:LP totale è maggiore di 1:1. Di conseguenza, l'esecuzione di macchine virtuali guest configurate senza più thread per ogni core è una configurazione ottimale.

### Vantaggi di utilizzando l'utilità di pianificazione di base

L'utilità di pianificazione core offre i vantaggi seguenti:

* Un limite di sicurezza avanzata per l'isolamento del carico di lavoro guest - VPs Guest sono vincolati per l'esecuzione in coppie core fisico sottostante, ridurre la vulnerabilità agli attacchi snooping canale.

* Carico di lavoro ridotto variabilità - variabilità velocità effettiva di Guest carico di lavoro risulta notevolmente ridotto, che offre una maggiore coerenza carico di lavoro.

* Utilizzo di SMT nel guest VM - il sistema operativo e applicazioni in esecuzione nella macchina virtuale guest può usare il comportamento SMT e programming interface (API) per controllare e distribuire il lavoro su thread SMT, proprio come che verrebbe eseguito quando non virtualizzati.

L'utilità di pianificazione di base è attualmente usato negli host di virtualizzazione Azure, in modo specifico per sfruttare il limite di sicurezza avanzata e variabilty basso carico di lavoro. Microsoft ritiene che il tipo di utilità di pianificazione core deve essere e continuerà a essere l'hypervisor predefinito tipo per la maggior parte degli scenari di virtualizzazione di programmazione.  Di conseguenza, per garantire che i nostri clienti sono protetti per impostazione predefinita, Microsoft si prende ora questa modifica per Windows Server 2019.

### Impatto sulle prestazioni di utilità di pianificazione di core carichi di lavoro guest

Anche se è richiesto per ridurre in modo efficace alcune classi di vulnerabilità, l'utilità di pianificazione core potrebbe potenzialmente anche ridurre le prestazioni. Clienti potrebbero visualizzare una differenza tra le caratteristiche di prestazioni con le macchine virtuali e impatti alla capacità carico di lavoro complessivo degli host di virtualizzazione. Nei casi in cui l'utilità di pianificazione core devono essere eseguite un Vicepresidente non - SMT, solo uno dei flussi istruzione nel core logico sottostante esegue mentre l'altra deve rimanere inattiva. Ciò limita la capacità totale host per carichi di lavoro guest.

Questi impatto sulle prestazioni può essere ridotta a icona seguendo le indicazioni di distribuzione in questo documento. Gli amministratori di host devono con attenzione prendere in considerazione la virtualizzazione specifica scenari di distribuzione e bilanciare relativa tolleranza per rischio per la protezione contro la necessità di densità massimo carico di lavoro, consolidamento un eccesso di host di virtualizzazione e così via.

## Modifiche di predefinito e le configurazioni consigliate per Windows Server 2016 e Windows Server 2019

Distribuzione di host Hyper-V con le condizioni di sicurezza massimo richiede l'utilizzo del tipo di utilità di pianificazione di hypervisor core. Per garantire che i nostri clienti sono protetti per impostazione predefinita, Microsoft sta cambiando il valore predefinito seguente e impostazioni consigliate.

>[!NOTE]
>Mentre il supporto interno dell'hypervisor per i tipi di utilità di pianificazione è stata incluso nella versione iniziale di Windows Server 2016, Windows Server 1709 e Windows Server 1803, gli aggiornamenti sono obbligatori per accedere al controllo di configurazione che consente di selezionare il tipo di utilità di pianificazione hypervisor.  Fai riferimento a [informazioni e uso di tipi di utilità di pianificazione hypervisor Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types) per informazioni dettagliate su questi aggiornamenti.

### Modifiche di host di virtualizzazione

* L'hypervisor userà l'utilità di pianificazione di base per impostazione predefinita a partire con Windows Server 2019.

* Microsoft reccommends configurazione l'utilità di pianificazione di base su Windows Server 2016. Il tipo di utilità di pianificazione hypervisor core è supportato in Windows Server 2016, tuttavia, il valore predefinito è l'utilità di pianificazione classico. L'utilità di pianificazione di base è facoltativo e deve essere abilitato in modo esplicito per l'amministratore dell'host Hyper-V.

### Modifiche di configurazione macchina virtuale

* In Windows Server 2019, nuove macchine virtuali create usando la versione di macchina virtuale predefinita 9.0 eredita automaticamente le proprietà SMT (abilitato o disabilitato) dell'host di virtualizzazione. Vale a dire se SMT viene abilitato su host fisico appena creato le macchine virtuali avranno anche SMT abilitato ed eredita la topologia SMT dell'host per impostazione predefinita, con la macchina virtuale con lo stesso numero di thread hardware per core come sistema sottostante. Questo si rifletteranno nella configurazione della macchina virtuale con HwThreadCountPerCore = 0, dove 0 indica che la macchina virtuale deve ereditare impostazioni SMT dell'host.

* Le macchine virtuali esistenti con una versione di macchina virtuale di 8.2 o versioni precedente verranno conservare impostazioni originale per HwThreadCountPerCore i processori della macchina virtuale e il valore predefinito per 8.2 guest versione della macchina virtuale è HwThreadCountPerCore = 1. Quando questi guest vengono eseguite in un host Windows Server 2019, verranno trattate come indicato di seguito:

    1. Se la macchina virtuale ha un conteggio Vicepresidente che è minore o uguale al numero di core LP, la macchina virtuale verrà considerata come una non - SMT macchina virtuale per l'utilità di pianificazione di base. Quando il Vicepresidente guest viene eseguita in un singolo thread SMT, pari livello del core thread SMT sarà inattivo. Questo è non ottimale e determinerà riduzione complessiva delle prestazioni.

    2. Se la macchina virtuale ha altre VPs più core LP, la macchina virtuale verrà considerata una VM SMT dall'utilità di pianificazione core. Tuttavia, la macchina virtuale non osserverai altre indicazioni che è una VM SMT. Ad esempio, Usa dell'istruzione CPUID o le API di Windows per eseguire una query topologia di CPU dal sistema operativo o le applicazioni non indicherà che SMT sia abilitato.

* Quando una macchina virtuale esistente viene aggiornata in modo esplicito da versioni della macchina virtuale eariler alla versione 9.0 tramite l'operazione di aggiornamento-VM, macchina virtuale manterrà il valore corrente per HwThreadCountPerCore.  La macchina virtuale non avranno SMT forza abilitato.

* In Windows Server 2016, si consiglia di abilitare SMT per macchine virtuali guest.  Per impostazione predefinita, le macchine virtuali create in Windows Server 2016 sarebbe SMT disabilitato, che è che hwthreadcountpercore è impostato su 1, a meno che non venga modificata in modo esplicito.

>[!NOTE]
>Windows Server 2016 non supporta l'impostazione HwThreadCountPerCore su 0.

#### Gestione della configurazione SMT macchina virtuale

Configurazione di SMT della macchina virtuale guest è impostata su una base per macchina virtuale. L'amministratore dell'host può esaminare e configurare configuration SMT una macchina virtuale per selezionare una delle opzioni seguenti:

    1. Configurare le macchine virtuali in esecuzione come SMT abilitato, facoltativamente, eredita automaticamente la topologia SMT host

    2. Configurare le macchine virtuali in esecuzione con non SMT

Il guidato SMT per una macchina virtuale viene visualizzato nei riquadri di riepilogo nella console di gestione di Hyper-V.  Configurazione delle impostazioni SMT una macchina virtuale può essere eseguita con le impostazioni della macchina virtuale o PowerShell.

#### Configurazione delle impostazioni di VM SMT con PowerShell

Per configurare le impostazioni SMT per una macchina virtuale guest, Apri una finestra di PowerShell con autorizzazioni sufficienti e digita:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

Dove:

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

Per leggere le impostazioni SMT per una macchina virtuale guest, Apri una finestra di PowerShell con autorizzazioni sufficienti e digita:

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

Tieni presente che le macchine virtuali configurate con HwThreadCountPerCore guest = 0 indica che SMT sarà abilitato per il guest e dovrà esporre lo stesso numero di thread SMT Guest come si trovano su host di virtualizzazione sottostante, in genere 2.

### Macchine virtuali guest potrebbe osservare le modifiche alla topologia di CPU in scenari di mobilità della macchina virtuale

Il sistema operativo e le applicazioni in una macchina virtuale potrebbero vedere le modifiche apportate a entrambi host e le impostazioni della macchina virtuale prima e dopo che gli eventi del ciclo di vita della macchina virtuale, ad esempio migrazione in tempo reale o salvare e ripristino le operazioni. Durante un'operazione in quale macchina virtuale viene salvato e ripristinato lo stato, vengono migrate impostazione HwThreadCountPerCore della macchina virtuale sia il valore realizzato (vale a dire la combinazione calcolata di impostazione della macchina virtuale e la configurazione dell'host di origine). La macchina virtuale continuerà a essere eseguita con queste impostazioni nell'host di destinazione. In corrispondenza del punto della macchina virtuale viene arrestato e riavviato, è possibile che il valore realizzato osservato dalla macchina virtuale verrà modificato. Questo deve essere inoffensivo, come sistema operativo e applicazioni software livello dovrebbe cercare le informazioni sulla topologia di CPU come parte dei flussi di codice di inizializzazione e avvio normali. Tuttavia, poiché potrebbe osservare questi inizializzazione in fase di avvio sequenze vengono ignorate durante live migration o salvare/ripristinare le operazioni, le macchine virtuali che subiscono queste transizioni di stato originariamente calcolato realizzati valore fino a quando non sono vengono interrotti e riavviato.  

### Avvisi relativi alle configurazioni della macchina virtuale non ottimale

Macchine virtuali configurate con altre VPs rispetto ai core fisici il risultato di host in una configurazione non ottimale. L'utilità di pianificazione di hypervisor considererà queste macchine virtuali come se fossero SMT riconoscere. Tuttavia, sistema operativo e applicazioni nella macchina virtuale verranno visualizzata una topologia di CPU che mostra SMT è disabilitato. Quando questa condizione viene rilevata, il processo di lavoro Hyper-V registrerà un evento su host di virtualizzazione di avviso all'amministratore dell'host che la configurazione della macchina virtuale è non ottimale e aprirà automaticamente SMT essere abilitato per la macchina virtuale.

#### Come identificare in modo non ottimale configurato le macchine virtuali

È possibile identificare non - SMT macchine virtuali esaminando il Registro di sistema nel Visualizzatore eventi di evento di processo di lavoro Hyper-V 3498 ID, quali verranno attivati per una macchina virtuale per ogni volta che il numero di VPs nella macchina virtuale è maggiore del numero di core fisico. Eventi di processo di lavoro possono essere ottenuti dal Visualizzatore eventi o tramite PowerShell.

#### Esecuzione di query l'evento di macchina virtuale del processo di lavoro Hyper-V con PowerShell

Per eseguire una query per l'evento di processo di lavoro Hyper-V 3498 ID tramite PowerShell, Immetti i comandi seguenti da un prompt di PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### Impatto in termini di guest SMT guidato sull'utilizzo di hypervisor gli enlightenment per sistemi operativi guest

L'hypervisor di Microsoft offre più gli enlightenment o suggerimenti, che il sistema operativo in esecuzione in un macchina virtuale guest può eseguire una query e Usa per attivare le ottimizzazioni, ad esempio quelli che potrebbero essere prestazioni o in caso contrario migliora la gestione delle condizioni diverse durante l'esecuzione virtualizzato. One probabili recente introduzione riguardano la gestione del processore virtuale pianificazione e l'uso di misure di prevenzione del sistema operativo per gli attacchi di canale che sfruttano SMT.

>[!NOTE]
>Microsoft consiglia che gli amministratori di host abilitare SMT per le macchine virtuali guest ottimizzare le prestazioni del carico di lavoro.

I dettagli di questo probabili guest vengono forniti sotto, tuttavia la chiave per gli amministratori di host di virtualizzazione è considerare che le macchine virtuali dovrebbe avere HwThreadCountPerCore configurato in modo che corrisponda configurazione SMT fisica dell'host. In questo modo l'hypervisor segnalare che non vi sia alcun core non architettura la condivisione. Di conseguenza, qualsiasi ottimizzazioni di supporto del sistema operativo guest che richiedono la probabili possono essere abilitate. In Windows Server 2019, creare nuove macchine virtuali e lasciare il valore predefinito di HwThreadCountPerCore (0). Le macchine virtuali precedenti la migrazione da Windows Server 2016 host possono essere aggiornati per la versione di configurazione di Windows Server 2019. Dopo questa operazione, Microsoft consiglia di impostazione HwThreadCountPerCore = 0.  In Windows Server 2016, Microsoft consiglia impostazione HwThreadCountPerCore in modo che corrisponda la configurazione di host (in genere 2).

### Dettagli probabili NoNonArchitecturalCoreSharing

A partire da Windows Server 2016, l'hypervisor definisce un nuovo probabili per descrivere la gestione delle Vicepresidente pianificazione e la posizione per il sistema operativo guest. Questo probabili sono definito nel [Hypervisor alto livello Functional Specification v5.0c](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs).

Hypervisor foglia CPUID sintetico CPUID.0x40000004.EAX:18[NoNonArchitecturalCoreSharing = 1] indica che un processore virtuale mai condivideranno un core fisico con un altro del processore virtuale, ad eccezione di processori virtuali segnalati come SMT di pari livello thread. Ad esempio, un Vicepresidente guest non verrà mai eseguita su un thread SMT insieme a un Vicepresidente radice in esecuzione contemporaneamente su un thread SMT nello stesso core del processore di pari livello. Questa condizione è possibile solo quando si esegue virtualizzato e pertanto rappresenta un comportamento SMT non architettura che contiene anche compromettere gravemente la sicurezza. Il sistema operativo guest può usare NoNonArchitecturalCoreSharing = 1 come un'indicazione che è sicuro attivare le ottimizzazioni, che consentano di evitare l'overhead delle prestazioni delle STIBP dell'impostazione.

In alcune configurazioni, l'hypervisor non indicherà che NoNonArchitecturalCoreSharing = 1. Ad esempio, se un host ha SMT abilitato ed è configurato per utilizzare l'utilità di pianificazione hypervisor classico, NoNonArchitecturalCoreSharing è 0. Ciò potrebbe impedire l'abilitazione di alcune ottimizzazioni guest riconoscimento dei dati aziendali. Di conseguenza, Microsoft consiglia che gli amministratori di host utilizzando SMT si basano su Utilità di pianificazione core hypervisor e garantire che le macchine virtuali sono configurate per ereditano la configurazione dei loro SMT dall'host a garantire prestazioni ottimali carico di lavoro.

## Riepilogo

Panorama delle minacce alla sicurezza continua evoluzione. Per garantire che i nostri clienti sono protetti per impostazione predefinita, Microsoft sta cambiando la configurazione predefinita per l'hypervisor e macchine virtuali di avvio in Windows Server 2019 Hyper-V e fornendo aggiornato consigli e indicazioni per i clienti che eseguono Windows Server 2016 Hyper-V. Gli amministratori di host di virtualizzazione devono:

* Leggere e comprendere le indicazioni fornite in questo documento

* Valutare attentamente e regolare le distribuzioni di virtualizzazione per garantire che soddisfino la sicurezza, prestazioni, densità di virtualizzazione e obiettivi di velocità di risposta carico di lavoro per i requisiti univoci

* Prendi in considerazione nuovamente la configurazione esistente host di Windows Server 2016 Hyper-V possono sfruttare i vantaggi di sicurezza avanzata offerti dall'utilità di pianificazione di base di hypervisor

* Aggiornare esistenti non - SMT macchine virtuali per ridurre l'impatto sulle prestazioni da pianificazione limitazioni di isolamento Vicepresidente delle vulnerabilità di sicurezza hardware
