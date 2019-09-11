---
title: Informazioni e utilizzo di tipi di utilità di pianificazione hypervisor Hyper-V
description: Fornisce informazioni per gli amministratori di host Hyper-V sull'utilizzo delle modalità di pianificazione di Hyper-V
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: c7c2de8354d067faf0dcf1787c3e178421e2ac03
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872034"
---
# <a name="managing-hyper-v-hypervisor-scheduler-types"></a>Gestione dei tipi di utilità di pianificazione hypervisor Hyper-V

>Si applica a: Windows 10, Windows Server 2016, Windows Server, versione 1709, Windows Server, versione 1803, Windows Server 2019

Questo articolo descrive le nuove modalità di logica di pianificazione dei processori virtuali introdotta per la prima volta in Windows Server 2016. Queste modalità, o tipi di utilità di pianificazione, determinano il modo in cui l'hypervisor Hyper-V alloca e gestisce il lavoro tra i processori virtuali guest. Un amministratore host Hyper-V può selezionare i tipi di utilità di pianificazione hypervisor più adatti per le macchine virtuali guest (VM) e configurare le VM per sfruttare la logica di pianificazione.

>[!NOTE]
>Gli aggiornamenti sono necessari per usare le funzionalità dell'utilità di pianificazione hypervisor descritte in questo documento. Per informazioni dettagliate, vedere [aggiornamenti richiesti](#required-updates).

## <a name="background"></a>Sfondo

Prima di illustrare la logica e i controlli dietro la pianificazione del processore virtuale Hyper-V, è utile esaminare i concetti di base trattati in questo articolo.

### <a name="understanding-smt"></a>Informazioni su SMT

Il multithreading simultaneo, o SMT, è una tecnica utilizzata nelle progettazioni moderne del processore che consente di condividere le risorse del processore con thread di esecuzione separati e indipendenti. SMT offre in genere un modesto miglioramento delle prestazioni per la maggior parte dei carichi di lavoro parallelizzazione i calcoli, quando possibile, aumentando la velocità effettiva delle istruzioni, anche se non è possibile ottenere un miglioramento delle prestazioni o persino una lieve perdita di prestazioni quando la contesa tra thread si verificano risorse del processore condivise.
I processori che supportano SMT sono disponibili sia da Intel che da AMD. Intel si riferisce alle relative offerte SMT come tecnologia Intel Hyper-Threading o Intel HT.

Ai fini di questo articolo, le descrizioni di SMT e il modo in cui vengono utilizzate da Hyper-V si applicano ugualmente ai sistemi Intel e AMD.

* Per ulteriori informazioni sulla tecnologia Intel HT, fare riferimento alla [tecnologia Intel Hyper-Threading](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* Per ulteriori informazioni su AMD SMT, vedere [l'architettura di base "Zen"](https://www.amd.com/en/technologies/zen-core) .

## <a name="understanding-how-hyper-v-virtualizes-processors"></a>Informazioni sul modo in cui Hyper-V virtualizza i processori

Prima di prendere in considerazione i tipi di utilità di pianificazione hypervisor, è anche utile comprendere l'architettura di Hyper-V. È possibile trovare un riepilogo generale nella [Panoramica della tecnologia Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview). Questi sono concetti importanti per questo articolo:

* Hyper-V crea e gestisce le partizioni delle macchine virtuali, in cui le risorse di calcolo vengono allocate e condivise, sotto il controllo dell'hypervisor. Le partizioni forniscono limiti di isolamento forti tra tutte le macchine virtuali guest e tra le macchine virtuali guest e la partizione radice.

* La partizione radice è a sua volta una partizione di macchina virtuale, anche se dispone di proprietà univoche e privilegi molto maggiori rispetto alle macchine virtuali guest. La partizione radice fornisce i servizi di gestione che controllano tutte le macchine virtuali guest, fornisce il supporto per i dispositivi virtuali per i guest e gestisce tutte le I/O dei dispositivi per le macchine virtuali guest. Microsoft consiglia vivamente di non eseguire i carichi di lavoro dell'applicazione nella partizione radice.

* Ogni processore virtuale (VP) della partizione radice viene mappato a 1:1 a un processore logico sottostante (LP). Un host VP verrà sempre eseguito sullo stesso LP sottostante. non viene eseguita alcuna migrazione del VPs della partizione radice.

* Per impostazione predefinita, l'LP su cui è in esecuzione VPs host può eseguire anche il VPs Guest.

* Un VP Guest può essere pianificato dall'hypervisor per l'esecuzione in qualsiasi processore logico disponibile. Mentre l'utilità di pianificazione dell'hypervisor prende in considerazione la località della cache temporale, la topologia NUMA e molti altri fattori durante la pianificazione di un VP Guest, infine il VP potrebbe essere pianificato in qualsiasi LP host.

## <a name="hypervisor-scheduler-types"></a>Tipi di utilità di pianificazione hypervisor

A partire da Windows Server 2016, l'hypervisor Hyper-V supporta diverse modalità di logica dell'utilità di pianificazione, che determinano il modo in cui l'hypervisor Pianifica i processori virtuali sui processori logici sottostanti. Questi tipi di utilità di pianificazione sono:

- [Utilità di pianificazione classica con condivisione equa](#the-classic-scheduler)
- [Utilità di pianificazione principale](#the-core-scheduler)
- [Utilità di pianificazione radice](#the-root-scheduler)

### <a name="the-classic-scheduler"></a>Utilità di pianificazione classica

L'utilità di pianificazione classica è stata l'impostazione predefinita per tutte le versioni di Windows Hyper-V Hypervisor sin dall'inizio, incluso Windows Server 2016 Hyper-V. L'utilità di pianificazione classica fornisce una condivisione equa, il modello di pianificazione Round Robin preemptive per i processori virtuali guest.

Il tipo di utilità di pianificazione classica è il più appropriato per la maggior parte degli usi tradizionali di Hyper-V: per i cloud privati, i provider di hosting e così via. Le caratteristiche delle prestazioni sono ben note e sono ottimizzate per supportare un'ampia gamma di scenari di virtualizzazione, ad esempio l'oversubscription di VPs a LPs, l'esecuzione simultanea di molte macchine virtuali e carichi di lavoro eterogeneo, con scalabilità superiore Macchine virtuali delle prestazioni, che supportano il set completo di funzionalità di Hyper-V senza restrizioni e altro ancora.

### <a name="the-core-scheduler"></a>Utilità di pianificazione principale

L'utilità di pianificazione principale dell'hypervisor è una nuova alternativa alla logica dell'utilità di pianificazione classica, introdotta in Windows Server 2016 e Windows 10 versione 1607. L'utilità di pianificazione principale offre un limite di sicurezza forte per l'isolamento dei carichi di lavoro Guest e una ridotta variabilità delle prestazioni per i carichi di lavoro all'interno di macchine virtuali in esecuzione in un host di virtualizzazione abilitato per SMT. L'utilità di pianificazione core consente di eseguire contemporaneamente le macchine virtuali SMT e non SMT nello stesso host di virtualizzazione abilitato per SMT.

L'utilità di pianificazione principale utilizza la topologia SMT dell'host di virtualizzazione e, facoltativamente, espone le coppie SMT alle macchine virtuali guest e pianifica i gruppi di processori virtuali guest dalla stessa macchina virtuale su gruppi di processori logici SMT. Questa operazione viene eseguita in modo simmetrico, in modo che se LPs si trovano in gruppi di due, i VPs sono pianificati in gruppi di due e un core non viene mai condiviso tra le macchine virtuali.
Quando il VP è pianificato per una macchina virtuale senza SMT abilitato, il VP utilizzerà l'intero nucleo durante l'esecuzione.

Il risultato complessivo dell'utilità di pianificazione principale è che:

* Il VPs Guest è vincolato per l'esecuzione su coppie di core fisiche sottostanti, isolando una macchina virtuale ai limiti di core del processore, riducendo così la vulnerabilità agli attacchi di snooping del canale laterale da VM dannose.

* La variabilità della velocità effettiva è notevolmente ridotta.

* Le prestazioni sono potenzialmente ridotte, perché se è possibile eseguire solo uno di un gruppo di VPs, viene eseguito solo uno dei flussi di istruzioni nel core mentre l'altra rimane inattiva.

* Il sistema operativo e le applicazioni in esecuzione nella macchina virtuale guest possono usare il comportamento SMT e le interfacce di programmazione (API) per controllare e distribuire il lavoro tra thread SMT, proprio come se fossero eseguiti non virtualizzati.

* Un limite di sicurezza forte per l'isolamento del carico di lavoro Guest: il VPs Guest è vincolato per l'esecuzione su coppie di core fisiche sottostanti, riducendo la vulnerabilità agli attacchi di snooping del canale laterale.

Per impostazione predefinita, l'utilità di pianificazione Core verrà usata a partire da Windows Server 2019. In Windows Server 2016, l'utilità di pianificazione principale è facoltativa e deve essere abilitata in modo esplicito dall'amministratore host Hyper-V e l'utilità di pianificazione classica è quella predefinita.

#### <a name="core-scheduler-behavior-with-host-smt-disabled"></a>Comportamento dell'utilità di pianificazione principale con l'host SMT disabilitato

Se l'hypervisor è configurato per l'utilizzo del tipo di utilità di pianificazione principale, ma la funzionalità SMT è disabilitata o non è presente nell'host di virtualizzazione, l'hypervisor utilizzerà il comportamento dell'utilità di pianificazione classica, indipendentemente dall'impostazione del tipo di utilità di pianificazione hypervisor.

### <a name="the-root-scheduler"></a>Utilità di pianificazione radice

L'utilità di pianificazione radice è stata introdotta con Windows 10 versione 1803. Quando il tipo di utilità di pianificazione radice è abilitato, l'hypervisor cede il controllo della pianificazione del lavoro alla partizione radice. L'utilità di pianificazione NT nell'istanza del sistema operativo della partizione radice gestisce tutti gli aspetti della pianificazione del lavoro per gli LPs del sistema.

L'utilità di pianificazione radice soddisfa i requisiti univoci inerenti al supporto di una partizione di utilità per garantire un forte isolamento dei carichi di lavoro, come usato con Windows Defender Application Guard (WDAG). In questo scenario, l'uscita dalle responsabilità della pianificazione al sistema operativo radice offre diversi vantaggi. Ad esempio, i controlli delle risorse della CPU applicabili agli scenari del contenitore possono essere usati con la partizione dell'utilità, semplificando la gestione e la distribuzione. Inoltre, l'utilità di pianificazione del sistema operativo radice può raccogliere rapidamente le metriche sull'utilizzo della CPU del carico di lavoro all'interno del contenitore e usare questi dati come input per gli stessi criteri di pianificazione applicabili a tutti gli altri carichi di lavoro nel sistema. Queste stesse metriche consentono anche di attribuire chiaramente il lavoro svolto in un contenitore dell'applicazione al sistema host. Tenere traccia di queste metriche è più difficile con i carichi di lavoro delle macchine virtuali tradizionali, in cui si verificano alcune operazioni eseguite su tutte le VM in esecuzione nella partizione radice.

#### <a name="root-scheduler-use-on-client-systems"></a>Utilizzo dell'utilità di pianificazione radice nei sistemi client

A partire da Windows 10 versione 1803, l'utilità di pianificazione radice viene utilizzata per impostazione predefinita solo nei sistemi client, in cui l'hypervisor può essere abilitato per supportare la sicurezza basata sulla virtualizzazione e l'isolamento del carico di lavoro WDAG e per il corretto funzionamento dei sistemi futuri con architetture di base eterogenee. Questa è l'unica configurazione dell'utilità di pianificazione hypervisor supportata per i sistemi client. Gli amministratori non devono tentare di eseguire l'override del tipo di utilità di pianificazione hypervisor predefinito nei sistemi client Windows 10.

#### <a name="virtual-machine-cpu-resource-controls-and-the-root-scheduler"></a>Controlli delle risorse della CPU della macchina virtuale e utilità di pianificazione radice

I controlli delle risorse del processore della macchina virtuale forniti da Hyper-V non sono supportati quando l'utilità di pianificazione radice dell'hypervisor è abilitata, perché la logica dell'utilità di pianificazione del sistema operativo radice gestisce le risorse host su base globale e non ha la conoscenza di una VM impostazioni di configurazione specifiche. I controlli delle risorse del processore Hyper-V per macchina virtuale, ad esempio Caps, pesi e riserve, sono applicabili solo quando l'hypervisor controlla direttamente la pianificazione del VP, ad esempio con i tipi di utilità di pianificazione classici e Core.

#### <a name="root-scheduler-use-on-server-systems"></a>Utilizzo dell'utilità di pianificazione radice nei sistemi server

L'utilità di pianificazione radice non è consigliata per l'uso con Hyper-V nei server in questo momento, perché le relative caratteristiche di prestazioni non sono state ancora completate e ottimizzate per supportare l'ampia gamma di carichi di lavoro tipici di molte distribuzioni di virtualizzazione del server.

## <a name="enabling-smt-in-guest-virtual-machines"></a>Abilitazione di SMT nelle macchine virtuali guest

Quando l'hypervisor dell'host di virtualizzazione è configurato per l'utilizzo del tipo di utilità di pianificazione principale, le macchine virtuali guest possono essere configurate per l'utilizzo di SMT, se lo si desidera. Esponendo il fatto che VPs è multithread a una macchina virtuale Guest consente all'utilità di pianificazione nel sistema operativo guest e ai carichi di lavoro in esecuzione nella macchina virtuale di rilevare e usare la topologia SMT nella propria pianificazione del lavoro. In Windows Server 2016, la versione SMT Guest non è configurata per impostazione predefinita e deve essere abilitata in modo esplicito dall'amministratore host Hyper-V. A partire da Windows Server 2019, le nuove macchine virtuali create nell'host erediteranno la topologia SMT dell'host per impostazione predefinita.  Ovvero una macchina virtuale versione 9,0 creata in un host con 2 thread SMT per core, vedrà anche 2 thread SMT per core.

PowerShell deve essere usato per abilitare SMT in una macchina virtuale guest. nella console di gestione di Hyper-V non è disponibile alcuna interfaccia utente.
Per abilitare SMT in una macchina virtuale Guest, aprire una finestra di PowerShell con autorizzazioni sufficienti e digitare:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

Dove <n> è il numero di thread SMT per core che verrà visualizzato dalla VM guest.  
Si noti <n> che = 0 imposterà il valore HwThreadCountPerCore in modo che corrisponda al numero di thread SMT dell'host per valore di base.

>[!NOTE] 
>L'impostazione HwThreadCountPerCore = 0 è supportata a partire da Windows Server 2019.

Di seguito è riportato un esempio di informazioni di sistema prese dal sistema operativo guest in esecuzione in una macchina virtuale con 2 processori virtuali e SMT abilitato. Il sistema operativo guest rileva due processori logici appartenenti allo stesso core.

![Screenshot che mostra msinfo32 in una macchina virtuale guest con SMT abilitato](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## <a name="configuring-the-hypervisor-scheduler-type-on-windows-server-2016-hyper-v"></a>Configurazione del tipo di utilità di pianificazione hypervisor in Windows Server 2016 Hyper-V

Per impostazione predefinita, Windows Server 2016 Hyper-V usa il modello di utilità di pianificazione hypervisor classico. È possibile configurare facoltativamente l'hypervisor per l'uso dell'utilità di pianificazione principale, per aumentare la sicurezza limitando il VPs Guest per l'esecuzione su coppie fisiche SMT corrispondenti e per supportare l'uso di macchine virtuali con la pianificazione SMT per il VPs Guest.

>[!NOTE]
>Microsoft consiglia a tutti i clienti che eseguono Windows Server 2016 Hyper-V di selezionare l'utilità di pianificazione principale per garantire la protezione ottimale degli host di virtualizzazione da VM guest potenzialmente dannose.

## <a name="windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler"></a>Windows Server 2019 Hyper-V usa l'utilità di pianificazione core per impostazione predefinita

Per garantire la distribuzione degli host Hyper-V nella configurazione di sicurezza ottimale, per impostazione predefinita, Windows Server 2019 Hyper-V utilizzerà il modello di utilità di pianificazione hypervisor di base. L'amministratore host può facoltativamente configurare l'host per l'uso dell'utilità di pianificazione classica legacy. Gli amministratori devono leggere attentamente, comprendere e considerare gli effetti di ogni tipo di utilità di pianificazione sulla sicurezza e sulle prestazioni degli host di virtualizzazione prima di eseguire l'override delle impostazioni predefinite del tipo di utilità di pianificazione.  Per ulteriori informazioni, vedere [informazioni sulla selezione del tipo di utilità di pianificazione Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection) .

### <a name="required-updates"></a>Aggiornamenti necessari

>[!NOTE]
>Per utilizzare le funzionalità dell'utilità di pianificazione hypervisor descritte in questo documento sono necessari gli aggiornamenti seguenti. Questi aggiornamenti includono modifiche per supportare la nuova opzione BCD ' hypervisorschedulertype ', necessaria per la configurazione host.

| Versione | Release  | Aggiornamento richiesto | Articolo della Knowledge |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018,07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018,07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1709 | 2018,07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | Nessuna | Nessuna |

## <a name="selecting-the-hypervisor-scheduler-type-on-windows-server"></a>Selezione del tipo di utilità di pianificazione hypervisor in Windows Server

La configurazione dell'utilità di pianificazione hypervisor viene controllata tramite la voce BCD hypervisorschedulertype.

Per selezionare un tipo di utilità di pianificazione, aprire un prompt dei comandi con privilegi di amministratore:

``` command
     bcdedit /set hypervisorschedulertype type
```

Dove `type` è uno dei seguenti:

* Classico
* Core
* Root

Il sistema deve essere riavviato per rendere effettive le modifiche apportate al tipo di utilità di pianificazione hypervisor.

>[!NOTE]
>L'utilità di pianificazione radice dell'hypervisor non è attualmente supportata in Windows Server Hyper-V. Gli amministratori di Hyper-V non devono tentare di configurare l'utilità di pianificazione radice per l'uso con scenari di virtualizzazione del server.

## <a name="determining-the-current-scheduler-type"></a>Determinazione del tipo di utilità di pianificazione corrente

È possibile determinare il tipo di utilità di pianificazione hypervisor corrente in uso esaminando il registro di sistema in Visualizzatore eventi per l'evento di avvio hypervisor più recente 2, che segnala il tipo di utilità di pianificazione hypervisor configurato all'avvio dell'hypervisor. Gli eventi di avvio dell'hypervisor possono essere ottenuti dal Visualizzatore eventi Windows o tramite PowerShell.

L'evento di avvio hypervisor con ID 2 indica il tipo di utilità di pianificazione hypervisor, dove:

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![Screenshot che mostra l'evento di avvio dell'hypervisor con ID 2 Dettagli](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![Screenshot che Mostra Visualizzatore eventi visualizzare l'evento di avvio hypervisor con ID 2](media/Hyper-V-CoreScheduler-EventViewer.png)

### <a name="querying-the-hyper-v-hypervisor-scheduler-type-launch-event-using-powershell"></a>Esecuzione di query sull'evento di avvio del tipo di utilità di pianificazione hypervisor Hyper-V tramite PowerShell

Per eseguire una query per l'ID evento hypervisor 2 con PowerShell, immettere i comandi seguenti da un prompt di PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![Screenshot che mostra la query e i risultati di PowerShell per l'evento di avvio hypervisor con ID 2](media/Hyper-V-CoreScheduler-PowerShell.png)
