---
title: Comprendere e usare i tipi dell'utilità di pianificazione di hypervisor Hyper-V
description: Fornisce informazioni per amministratori di host Hyper-V sull'utilizzo dell'utilità di pianificazione di Hyper-V modalità
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: c0c2f85fbbeca9e8ac5d40bbcb71f286fabfb65c
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501665"
---
# <a name="managing-hyper-v-hypervisor-scheduler-types"></a>La gestione dei tipi dell'utilità di pianificazione di hypervisor Hyper-V

>Si applica a: Windows 10, Windows Server 2016, Windows Server, versione 1709, Windows Server, versione 1803, Windows Server 2019

Questo articolo descrive le nuove modalità di pianificazione per la logica introdotta in Windows Server 2016 del processore virtuale. Queste modalità o i tipi dell'utilità di pianificazione, determinano l'hypervisor Hyper-V alloca e gestisce il lavoro tra processori virtuali guest. Un amministratore di host Hyper-V è possibile selezionare tipi dell'utilità di pianificazione hypervisor che sono più adatti per le macchine virtuali guest (macchine virtuali) e configurare le macchine virtuali per sfruttare i vantaggi della logica di pianificazione.

>[!NOTE]
>Aggiornamenti sono necessari per usare le funzionalità di hypervisor dell'utilità di pianificazione descritte in questo documento. Per informazioni dettagliate, vedere [gli aggiornamenti richiesti](#required-updates).

## <a name="background"></a>Informazioni

Prima di discutere i controlli alla base di pianificazione del processore virtuale Hyper-V e per la logica, è consigliabile rivedere i concetti di base illustrati in questo articolo.

### <a name="understanding-smt"></a>Understanding SMT

Il multithreading simultaneo o SMT, è una tecnica usata nelle architetture di processore moderno che consente alle risorse del processore da condividere dai thread di esecuzione separato e indipendente. SMT in genere offre un aumento di prestazioni modesti per la maggior parte dei carichi di lavoro grazie alla parallelizzazione i calcoli quando possibile, aumentando la velocità effettiva (istruzione), anche se delle prestazioni guadagno o persino una lieve riduzione delle prestazioni può verificarsi quando il conflitto tra thread risorse del processore condiviso viene generato.
Sono disponibili da Intel e AMD processori che supportano SMT. Intel si riferisce alle loro offerte SMT come la tecnologia Intel Hyper Threading o Hyper-Threading di Intel.

Ai fini di questo articolo, le descrizioni di SMT e come usata da Hyper-V sono ugualmente applicabili a sistemi di entrambi Intel e AMD.

* Per altre informazioni sulla tecnologia Intel Hyper-Threading, fare riferimento a [tecnologia Intel Hyper-Threading](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* Per altre informazioni su AMD SMT, fare riferimento a [l'architettura di base "Zen"](https://www.amd.com/en/technologies/zen-core)

## <a name="understanding-how-hyper-v-virtualizes-processors"></a>Comprendere la modalità Hyper-V virtualizza processori

Prima di considerare hypervisor tipi dell'utilità di pianificazione, è anche utile comprendere l'architettura di Hyper-V. È possibile trovare un riepilogo generale nelle [Panoramica della tecnologia Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview). Si tratta di concetti importanti di questo articolo:

* Hyper-V crea e gestisce le partizioni di macchina virtuale, tra cui calcolo delle risorse sono allocate e condivisa, sotto il controllo di hypervisor. Le partizioni forniscono i limiti di isolamento forte tra tutte le macchine virtuali guest e tra le macchine virtuali guest e nella partizione radice.

* La partizione radice è a sua volta una partizione di macchina virtuale, anche se presenta proprietà univoche e molto maggiori privilegi rispetto alle macchine virtuali guest. La partizione radice fornisce i servizi di gestione che consentono di controllare tutte le macchine virtuali guest, fornisce il supporto di dispositivi virtuali per i guest e gestisce tutti i dispositivi dei / o per le macchine virtuali guest. Microsoft consiglia vivamente di non in esecuzione carichi di lavoro dell'applicazione nella partizione radice.

* Ogni processore virtuale (VP) della partizione radice è 1:1 con mapping a un processore logico sottostante (LP). Un host VP verrà sempre eseguito sulla stessa LP sottostante, non vi è alcuna migrazione del VPs della partizione radice.

* Per impostazione predefinita, il LPs su cui eseguire VPs host possono essere eseguiti anche VPs guest.

* Vice presidente del settore un guest possono essere pianificati dall'hypervisor per l'esecuzione su tutti i processori logici disponibili. Mentre l'utilità di pianificazione di hypervisor gestisce da prendere in considerazione la località della cache temporali, la topologia NUMA e molti altri fattori quando si pianifica un guest Vice presidente del settore, in definitiva la VP è stato possibile pianificare in qualsiasi host LP.

## <a name="hypervisor-scheduler-types"></a>Tipi dell'utilità di pianificazione di hypervisor

A partire da Windows Server 2016, l'hypervisor Hyper-V supporta diverse modalità di logica dell'utilità di pianificazione, che determinano come l'hypervisor pianifica processori virtuali sui processori logici sottostanti. Questi tipi di utilità di pianificazione sono:

- [L'utilità di pianificazione classico, fair-share](#the-classic-scheduler)
- [L'utilità di pianificazione di base](#the-core-scheduler)
- [L'utilità di pianificazione di radice](#the-root-scheduler)

### <a name="the-classic-scheduler"></a>L'utilità di pianificazione classico

L'utilità di pianificazione classico è stato il valore predefinito per tutte le versioni di hypervisor Hyper-V di Windows fin dalle sue origini, tra cui Windows Server 2016 Hyper-V. L'utilità di pianificazione classico fornisce una condivisione equa, preemptive modello pianificazione round robin per i processori virtuali guest.

Il tipo dell'utilità di pianificazione classico risulta più appropriato per la maggior parte degli utilizzi di Hyper-V tradizionali, cloud privati, i provider di hosting, e così via. Le caratteristiche delle prestazioni sono ben comprese e sono meglio ottimizzate per supportare un'ampia gamma di scenari di virtualizzazione, ad esempio di sottoscrizione in eccesso di VPs a LPs, eseguite contemporaneamente molte macchine virtuali e i carichi di lavoro eterogenei, in esecuzione più grande scalabilità elevata le prestazioni di macchine virtuali, che supportano la funzionalità completa impostato di Hyper-V senza restrizioni e altro ancora.

### <a name="the-core-scheduler"></a>L'utilità di pianificazione di base

L'utilità di pianificazione di hypervisor core è una nuova alternativa alla logica dell'utilità di pianificazione classico, introdotta in Windows Server 2016 e Windows 10 versione 1607. L'utilità di pianificazione di core offre un limite di sicurezza avanzata per l'isolamento del carico di lavoro guest e variabilità una riduzione delle prestazioni per carichi di lavoro all'interno di macchine virtuali in esecuzione in un host di virtualizzazione abilitate SMT. L'utilità di pianificazione di base consente di SMT e non-SMT macchine virtuali in esecuzione contemporaneamente nello stesso host di virtualizzazione abilitate SMT.

L'utilità di pianificazione di base Usa topologia SMT dell'host di virtualizzazione e, facoltativamente, espone le coppie SMT per le macchine virtuali guest e le pianificazioni di gruppi di processori virtuali guest dalla stessa macchina virtuale nei gruppi di processori logici SMT. Questa operazione viene eseguita in modo simmetrico in modo che se LPs si trovano in gruppi di due, VPs vengono pianificati nei gruppi di due e un core non viene mai condiviso tra le macchine virtuali.
Quando il Vice presidente del settore è pianificata per una macchina virtuale senza SMT abilitato, che VP utilizzerà l'intera base quando è in esecuzione.

Il risultato complessivo dell'utilità di pianificazione di base è che:

* VPs guest sono vincolati all'esecuzione su coppie di fisiche core sottostante, isolamento di una macchina virtuale per i limiti di core processore, riducendo in tal modo vulnerabilità agli attacchi di snooping canale laterale dalle macchine virtuali dannose.

* La variabilità della velocità effettiva risulta notevolmente ridotto.

* Potenzialmente, le prestazioni sono ridotte perché se solo uno di un gruppo di VPs eseguibili, solo uno dei flussi di istruzioni nel nucleo viene eseguito mentre l'altro è inattivo.

* Il sistema operativo e applicazioni in esecuzione nella macchina virtuale guest possono usare il comportamento SMT e programming interface (API) per controllare e distribuire il lavoro tra thread SMT, esattamente come si verificavano quando non virtualizzata.

* Un limite di sicurezza avanzata per l'isolamento del carico di lavoro guest - VPs Guest sono vincolati all'esecuzione su una coppia di core fisici sottostanti, ridurre la vulnerabilità agli attacchi di snooping canale laterale.

L'utilità di pianificazione di base verrà utilizzato per impostazione predefinita a partire da Windows Server 2019. In Windows Server 2016, l'utilità di pianificazione di base è facoltativo e deve essere abilitato in modo esplicito dall'amministratore dell'host Hyper-V e il valore predefinito è l'utilità di pianificazione classico.

#### <a name="core-scheduler-behavior-with-host-smt-disabled"></a>Comportamento dell'utilità di pianificazione di base con host SMT disabilitato

Se l'hypervisor è configurato per usare il tipo di base dell'utilità di pianificazione, ma la funzionalità SMT è disabilitato o non è presente nell'host di virtualizzazione, quindi l'hypervisor verrà utilizzato il comportamento dell'utilità di pianificazione classico, indipendentemente dal fatto l'impostazione del tipo di utilità di pianificazione di hypervisor.

### <a name="the-root-scheduler"></a>L'utilità di pianificazione di radice

L'utilità di pianificazione di radice è stata introdotta con Windows 10 versione 1803. Quando il tipo di radice dell'utilità di pianificazione è abilitato, l'hypervisor cede il controllo della pianificazione di lavoro e la partizione radice. L'utilità di pianificazione di NT nell'istanza del sistema operativo della partizione radice gestisce tutti gli aspetti della pianificazione del lavoro al sistema LPs.

L'utilità di pianificazione radice risolve i requisiti univoci inerenti con una partizione di utilità di supporto per fornire un isolamento forte carico di lavoro, quando usato con Windows Defender Application Guard (WDAG). In questo scenario, lasciando la programmazione di responsabilità per la radice del sistema operativo offre diversi vantaggi. Ad esempio, i controlli delle risorse della CPU applicabile a scenari di contenitore potrebbero utilizzabile con la partizione di utilità, semplificando la gestione e la distribuzione. Inoltre, l'utilità di pianificazione del sistema operativo radice immediatamente possibile raccogliere le metriche sull'utilizzo della CPU all'interno del contenitore del carico di lavoro e utilizzare questi dati come input per i criteri di pianificazione stesso applicabili a tutti gli altri carichi di lavoro nel sistema. Queste stesse metriche consentono anche di chiaramente attributo lavoro svolto in un contenitore di applicazione al sistema host. Tiene traccia di queste metriche è più difficile con i carichi di lavoro di macchine virtuali tradizionali, in cui alcune operazioni per conto di tutte le VM in esecuzione viene eseguita nella partizione radice.

#### <a name="root-scheduler-use-on-client-systems"></a>Uso dell'utilità di pianificazione radice nei sistemi client

A partire da Windows 10 versione 1803, l'utilità di pianificazione di radice viene utilizzato per impostazione predefinita nei sistemi client, in cui l'hypervisor può essere abilitata per supportare la sicurezza basata sulla virtualizzazione e isolamento del carico di lavoro WDAG e per il corretto funzionamento dei sistemi in futuro con architetture di core eterogenei. Questa è la configurazione dell'utilità di pianificazione di hypervisor supportato solo per i sistemi client. Gli amministratori non tentare di ignorare il tipo dell'utilità di pianificazione di hypervisor predefinito nei sistemi client Windows 10.

#### <a name="virtual-machine-cpu-resource-controls-and-the-root-scheduler"></a>I controlli delle risorse CPU macchina virtuale e l'utilità di pianificazione di radice

I controlli delle risorse del processore macchina virtuale forniti da Hyper-V non sono supportati quando l'utilità di pianificazione di hypervisor radice è abilitata per la logica dell'utilità di pianificazione del sistema operativo radice è la gestione delle risorse di host a livello globale e non dispone della Knowledge base di una macchina virtuale impostazioni di configurazione specifiche. I controlli delle risorse del processore per ogni VM Hyper-V, ad esempio BLOC MAIUSC, i pesi e le riserve, sono applicabili solo quando l'hypervisor controlla direttamente VP di programmazione, ad esempio come con i tipi di modello di distribuzione classica e di core dell'utilità di pianificazione.

#### <a name="root-scheduler-use-on-server-systems"></a>Uso dell'utilità di pianificazione radice nei sistemi server

L'utilità di pianificazione radice non è consigliabile per l'utilizzo con Hyper-V nei server in questo momento, come le caratteristiche delle prestazioni non ancora sono state caratterizzate completamente e ottimizzate per soddisfare l'ampia gamma di carichi di lavoro tipici di molte distribuzioni di virtualizzazione di server.

## <a name="enabling-smt-in-guest-virtual-machines"></a>L'abilitazione di SMT nelle macchine virtuali guest

Dopo hypervisor dell'host di virtualizzazione è configurato per usare il tipo di base dell'utilità di pianificazione, le macchine virtuali guest può essere configurate per utilizzare SMT se lo si desidera. Il fatto che VPs sono Hyper-Threading per una macchina virtuale guest l'esposizione consente l'utilità di pianificazione nel sistema operativo guest e i carichi di lavoro in esecuzione nella macchina virtuale per rilevare e utilizzare la topologia SMT nel proprio lavoro di pianificazione. In Windows Server 2016, guest SMT non è configurato per impostazione predefinita e deve essere abilitato in modo esplicito dall'amministratore dell'host Hyper-V. A partire da Windows Server 2019, nuove macchine virtuali create nello stesso host erediterà topologia SMT dell'host per impostazione predefinita.  Vale a dire, una versione che 9.0 macchina virtuale creata in un host con 2 thread SMT per ogni core anche vedrebbe 2 thread SMT per ogni core.

È necessario utilizzare PowerShell per abilitare SMT in una macchina virtuale guest; non è disponibile alcuna interfaccia utente disponibile in Gestione Hyper-V.
Per abilitare SMT in una macchina virtuale guest, aprire una finestra di PowerShell con autorizzazioni sufficienti, quindi digitare:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

In cui <n> è guest della macchina virtuale verrà visualizzato il numero di thread SMT per core.  
Si noti che <n> = 0 il valore verrà impostato HwThreadCountPerCore alla corrispondenza tra il numero di thread SMT dell'host per ogni valore di base.

>[!NOTE] 
>Impostazione HwThreadCountPerCore = 0 è supportata a partire da Windows Server 2019.

Di seguito è riportato un esempio di sistema informazioni ottenute dal sistema operativo guest in esecuzione in una macchina virtuale con 2 processori virtuali e SMT abilitata. Il sistema operativo guest sta rilevando che appartengono a stesso core con 2 processori logici.

![Screenshot che mostra msinfo32 in una macchina virtuale con SMT abilitato guest](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## <a name="configuring-the-hypervisor-scheduler-type-on-windows-server-2016-hyper-v"></a>Configurazione del tipo di utilità di pianificazione di hypervisor in Windows Server 2016 Hyper-V

Windows Server 2016 Hyper-V Usa il modello di utilità di pianificazione di hypervisor classica per impostazione predefinita. L'hypervisor può essere facoltativamente configurato per usare l'utilità di pianificazione di base, per aumentare la sicurezza limitando VPs guest per eseguire in coppie SMT fisiche corrispondenti e per supportare l'uso di macchine virtuali con SMT pianificazione per il loro VPs guest.

>[!NOTE]
>Microsoft consiglia che tutti i clienti che eseguono Windows Server 2016 Hyper-V selezionare l'utilità di pianificazione di core per garantire che loro gli host di virtualizzazione siano protetti in modo ottimale nelle macchine virtuali guest potenzialmente dannosi.

## <a name="windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler"></a>Valore predefinito è Windows Server 2019 Hyper-V usando l'utilità di pianificazione di base

Per assicurare che gli host Hyper-V vengono distribuiti nella configurazione di una sicurezza ottimale, Windows Server 2019 Hyper-V utilizzerà ora il modello fondamentale di utilità di pianificazione hypervisor per impostazione predefinita. L'amministratore di host può configurare facoltativamente l'host per usare l'utilità di pianificazione classico legacy. Gli amministratori devono attentamente letto e compreso prendere in considerazione l'impatto di che ogni tipo di pianificazione ha sulla sicurezza e prestazioni degli host di virtualizzazione prima di eseguire l'override le impostazioni predefinite del tipo dell'utilità di pianificazione.  Visualizzare [selezione del tipo di informazioni su Hyper-V utilità di pianificazione](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection) per altre informazioni.

### <a name="required-updates"></a>Aggiornamenti necessari

>[!NOTE]
>Gli aggiornamenti seguenti sono necessari per usare le funzionalità di hypervisor dell'utilità di pianificazione descritte in questo documento. Questi aggiornamenti includono modifiche per supportare la nuova opzione di BCD 'hypervisorschedulertype', che è necessaria per la configurazione dell'host.

| Version | Rilascio  | Aggiornamento richiesto | Articolo della Knowledge Base |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018.07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018.07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1709 | 2018.07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | Nessuno | Nessuno |

## <a name="selecting-the-hypervisor-scheduler-type-on-windows-server"></a>Selezione del tipo di hypervisor dell'utilità di pianificazione in Windows Server

La configurazione dell'utilità di pianificazione di hypervisor è controllata tramite la voce di BCD hypervisorschedulertype.

Per selezionare un tipo di pianificazione, aprire un prompt dei comandi con privilegi di amministratore:

``` command
     bcdedit /set hypervisorschedulertype type
```

In cui `type` è uno di:

* Classico
* Core
* Root

Qualsiasi per il tipo di hypervisor dell'utilità di pianificazione per rendere effettive le modifiche, è necessario riavviare il sistema.

>[!NOTE]
>L'utilità di pianificazione di hypervisor radice non è supportato in Windows Server Hyper-V in questo momento. Gli amministratori di Hyper-V non provare a configurare l'utilità di pianificazione radice per l'uso con scenari di virtualizzazione server.

## <a name="determining-the-current-scheduler-type"></a>Determinazione del tipo dell'utilità di pianificazione corrente

È possibile determinare il tipo di utilità di pianificazione hypervisor corrente in uso, esaminare il Registro di sistema nel Visualizzatore eventi per l'evento di avvio più recente di hypervisor ID 2, che segnala il tipo di hypervisor dell'utilità di pianificazione configurato al momento del lancio di hypervisor. Eventi di lancio di hypervisor possono essere ottenuti dal Visualizzatore eventi Windows, o tramite PowerShell.

Evento di lancio di hypervisor ID 2 indica il tipo dell'utilità di pianificazione di hypervisor, dove:

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![Cattura di schermata che mostra i dettagli dell'evento ID 2 lancio hypervisor](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![Cattura di schermata che mostra il Visualizzatore eventi la visualizzazione di eventi di lancio hypervisor ID 2](media/Hyper-V-CoreScheduler-EventViewer.png)

### <a name="querying-the-hyper-v-hypervisor-scheduler-type-launch-event-using-powershell"></a>L'esecuzione di query l'evento di lancio tipo di utilità di pianificazione di Hyper-V hypervisor tramite PowerShell

Per eseguire query per eventi di hypervisor ID 2 usando PowerShell, immettere i comandi seguenti da un prompt di PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![Cattura di schermata che mostra i risultati per eventi di lancio hypervisor ID 2 e query di PowerShell](media/Hyper-V-CoreScheduler-PowerShell.png)
