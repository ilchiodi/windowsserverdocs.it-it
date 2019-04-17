---
title: Informazioni e uso di tipi di utilità di pianificazione hypervisor Hyper-V
description: Fornisce informazioni per gli amministratori di host Hyper-V sull'utilizzo di utilità di pianificazione di Hyper-V modalità
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: 7af6d68b02367d349580eacb27405c6f37e97ff8
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783693"
---
# Gestione dei tipi di utilità di pianificazione hypervisor Hyper-V

>Si applica a: Windows 10, Windows Server 2016, Windows Server, versione 1709, Windows Server, versione 1803, Windows Server 2019

Questo articolo descrive le nuove modalità di pianificazione prima di tutto introdotta in Windows Server 2016 la logica del processore virtuale. Queste modalità, o tipi di utilità di pianificazione, determinano come l'hypervisor Hyper-V alloca e gestite lavoro tra processori virtuali guest. Un amministratore dell'host Hyper-V può selezionare i tipi di utilità di pianificazione hypervisor che sono particolarmente indicato per le macchine virtuali guest (VM) e configurare le macchine virtuali per sfruttare al meglio la logica di programmazione.

>[!NOTE]
>Gli aggiornamenti devono usare le funzionalità di utilità di pianificazione hypervisor descritte in questo documento. Per ulteriori informazioni, vedi [aggiornamenti obbligatori](#required-updates).

## Background

Prima di esaminare la logica e controlli dietro di pianificazione del processore virtuale Hyper-V, è utile esaminare i concetti di base descritti in questo articolo.

### Informazioni sui SMT

Multithreading simultaneo o SMT, è una tecnica usata nei progetti di processore moderno che consente alle risorse del processore essere condiviso dai thread di esecuzione distinto, indipendente. SMT in genere offre un aumento delle prestazioni minimi per la maggior parte dei carichi di lavoro dalla parallelizzazione calcoli quando possibile, l'aumento di velocità effettiva di istruzione, anche se delle prestazioni guadagno o persino una leggera perdita nelle prestazioni possono verificarsi quando il conflitto tra i thread per si verifica risorse condivise del processore.
Sono disponibili sia Intel e AMD processori supporto SMT. Intel fa riferimento a proprie offerte SMT come la tecnologia Intel Hyper Threading o Intel HT.

Ai fini di questo articolo, le descrizioni degli SMT e come si viene utilizzato da Hyper-V vengono applicate ai sistemi sia Intel e AMD.

* Per altre informazioni sulla tecnologia Intel HT, fai riferimento alla [Tecnologia Intel Hyper Threading](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* Per altre informazioni su AMD SMT, fai riferimento [all'Architettura Core "Zen"](https://www.amd.com/en/technologies/zen-core)

## Comprendere come Hyper-V virtualizza processori

Prima di valutare hypervisor tipi di utilità di pianificazione, è anche utile comprendere l'architettura di Hyper-V. Puoi trovare un riepilogo generale nella [Panoramica della tecnologia Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview). Questi sono concetti importanti per questo articolo:

* Hyper-V crea e gestisce le partizioni di macchina virtuale, tra cui calcolo le risorse sono allocate e condiviso, sotto il controllo dell'hypervisor. Le partizioni forniscono i limiti di isolamento sicuro tra tutte le macchine virtuali guest e tra le macchine virtuali guest e la partizione radice.

* La partizione radice è esso stesso una partizione di macchina virtuale, sebbene abbia proprietà univoco e molto privilegi maggiore rispetto a macchine virtuali guest. La partizione radice fornisce servizi di gestione che controllano tutte le macchine virtuali guest, fornisce il supporto dispositivo virtuale per Guest e gestisce tutti i dispositivi i/o per le macchine virtuali guest. Microsoft consiglia di non i carichi di lavoro qualsiasi applicazione in esecuzione nella partizione radice.

* Ogni processore virtuale (Vicepresidente) della partizione radice è 1:1 mappato a un processore logico sottostante (LP). Un host Vicepresidente verrà sempre eseguita sulla stessa LP sottostante e non c'è alcuna migrazione delle VPs della partizione radice.

* Per impostazione predefinita, i processori logici in cui eseguire VPs host possono essere eseguito anche VPs guest.

* Un Vicepresidente guest può essere pianificati dall'hypervisor per l'esecuzione su tutti i processori logici disponibili. Mentre l'utilità di pianificazione hypervisor si occupa di prendere in considerazione la località della cache temporale, topologia NUMA e molti altri fattori durante la pianificazione di un Vicepresidente guest, in ultima analisi il Vicepresidente potrebbe essere pianificati in qualsiasi host LP.

## Tipi di utilità di pianificazione hypervisor

A partire da Windows Server 2016, l'hypervisor Hyper-V supporta diverse modalità della logica di utilità di pianificazione, che determinano il modo in cui l'hypervisor pianifica processori virtuali processori logici sottostante. Questi tipi di utilità di pianificazione sono:

- [L'utilità di pianificazione classico, sull'equa condivisione](#the-classic-scheduler)
- [L'utilità di pianificazione di base](#the-core-scheduler)
- [L'utilità di pianificazione radice](#the-root-scheduler)

### L'utilità di pianificazione classico

L'utilità di pianificazione classico è stata il valore predefinito per tutte le versioni dell'hypervisor Hyper-V per Windows sin, tra cui Windows Server 2016 Hyper-V. L'utilità di pianificazione classica fornisce un'equa condivisione, modello di programmazione round robin preemptive per processori virtuali guest.

Il tipo di utilità di pianificazione classico è la più appropriata per la maggior parte degli utilizzi di Hyper-V tradizionali, per cloud privati, i provider di hosting e così via. Le caratteristiche di prestazioni siano comprensibili e sono particolarmente ottimizzate per supportare un'ampia gamma di scenari di virtualizzazione, ad esempio un eccesso di sottoscrizione di VPs logici, l'esecuzione simultanea di molti eterogenei macchine virtuali e i carichi di lavoro, che eseguono più grande scalabilità elevata impostare le prestazioni di macchine virtuali, che supporta la funzionalità completa di Hyper-V senza restrizioni e altro ancora.

### L'utilità di pianificazione di base

L'utilità di pianificazione core hypervisor è una nuova alternativa alla logica di utilità di pianificazione classico, introdotta in Windows Server 2016 e Windows 10 versione 1607. L'utilità di pianificazione core offre un limite di sicurezza avanzata per l'isolamento del carico di lavoro guest e variabilità riduzione delle prestazioni per carichi di lavoro all'interno di macchine virtuali in esecuzione in un host di virtualizzazione abilitata SMT. L'utilità di pianificazione core consente di eseguire le macchine virtuali sia SMT non SMT contemporaneamente nello stesso host di virtualizzazione abilitata SMT.

L'utilità di pianificazione core utilizza topologia SMT dell'host di virtualizzazione e facoltativamente espone le coppie SMT per macchine virtuali guest e delle pianificazioni dei gruppi di processori virtuali guest dalla macchina virtuale stessa nei gruppi di processori logici SMT. Questa operazione viene eseguita in modo simmetrico in modo che se processori logici sono gruppi di due, VPs vengono pianificati in gruppi di due e un core mai è condiviso tra le macchine virtuali.
Quando il Vicepresidente è pianificato per una macchina virtuale senza SMT abilitata, che Vicepresidente utilizzerà l'intero core quando viene eseguita.

Il risultato complessivo dell'utilità di pianificazione core è che:

* Guest VPs sono vincolati per l'esecuzione in coppie core fisico sottostante, isolando una macchina virtuale per i confini di core del processore, riducendo così vulnerabilità agli attacchi snooping canale da macchine virtuali dannose.

* La variabilità velocità effettiva è notevolmente ridotta.

* Le prestazioni sono potenzialmente ridotte, perché se solo uno di un gruppo di VPs può essere eseguito, solo uno dei flussi istruzione nel core eseguito mentre l'altra rimane inattiva.

* Il sistema operativo e applicazioni in esecuzione nella macchina virtuale guest possono usare il comportamento SMT e programming interface (API) per controllare e distribuire il lavoro su thread SMT, proprio come che verrebbe eseguito quando non virtualizzati.

* Un limite di sicurezza avanzata per l'isolamento del carico di lavoro guest - VPs Guest sono vincolati per l'esecuzione in coppie core fisico sottostante, ridurre la vulnerabilità agli attacchi snooping canale.

L'utilità di pianificazione core verranno usati per impostazione predefinita a partire da Windows Server 2019. In Windows Server 2016, l'utilità di pianificazione core è facoltativo e deve essere abilitato in modo esplicito per l'amministratore dell'host Hyper-V e l'utilità di pianificazione classico è il valore predefinito.

#### Comportamento di utilità di pianificazione core con host SMT disabilitato

Se l'hypervisor è configurato per utilizzare il tipo di utilità di pianificazione core, ma la funzionalità SMT è disabilitata o non è presente nell'host di virtualizzazione, l'hypervisor userà il comportamento di utilità di pianificazione classico, indipendentemente dal tipo di utilità di pianificazione hypervisor impostazione.

### L'utilità di pianificazione radice

L'utilità di pianificazione radice è stato introdotto con Windows 10 versione 1803. Quando il tipo di utilità di pianificazione della radice è abilitato, l'hypervisor cedes controllo del lavoro di pianificazione per la partizione radice. L'utilità di pianificazione NT nell'istanza del sistema operativo della partizione radice gestisce tutti gli aspetti della programmazione di lavoro al sistema analisi periodica limitata.

L'utilità di pianificazione radice illustra i requisiti univoci inerenti con una partizione di utilità di supporto per fornire l'isolamento del carico di lavoro sicuro, come utilizzato con Windows Defender Application Guard (WDAG). In questo scenario, lasciando la programmazione delle responsabilità alla radice del sistema operativo offre numerosi vantaggi. Controlli delle risorse della CPU applicabile a scenari di contenitore, ad esempio, potrebbero essere utilizzati con la partizione di utilità, semplificando la gestione e distribuzione. Inoltre, l'utilità di pianificazione del sistema operativo principale può facilmente raccogliere le metriche sull'utilizzo della CPU all'interno del contenitore del carico di lavoro e usare questi dati come input per gli stessi criteri applicabili a tutti gli altri carichi di lavoro della pianificazione nel sistema. Queste metriche stesso aiuterà chiaramente attributo lavoro svolto in un contenitore dell'applicazione al sistema host. Queste metriche di tracciamento è più difficile con carichi di lavoro tradizionali macchina virtuale, in cui alcune operazioni per conto di tutti in esecuzione della macchina virtuale viene eseguita nella partizione radice.

#### Uso di utilità di pianificazione radice nei sistemi client

A partire da Windows 10 versione 1803, l'utilità di pianificazione radice viene usato per impostazione predefinita nei sistemi client solo, in cui l'hypervisor può essere abilitato in seguito a WDAG carico di lavoro isolamento e protezione basata su virtualizzazione e per il corretto funzionamento dei sistemi successivi con architetture core eterogenei. Questa è la configurazione di utilità di pianificazione hypervisor supportati solo per i sistemi client. Gli amministratori non tentare di eseguire l'override il tipo di utilità di pianificazione predefinita hypervisor nei sistemi client Windows 10.

#### Controlli delle risorse della CPU macchina virtuale e l'utilità di pianificazione radice

I controlli delle risorse del processore di macchina virtuale forniti da Hyper-V non sono supportati quando è abilitata l'utilità di pianificazione radice hypervisor come logica di utilità di pianificazione del sistema operativo principale è la gestione delle risorse host a livello globale e non dispone di conoscenze di una macchina virtuale impostazioni di configurazione specifici. I controlli di risorsa del processore per macchina virtuale Hyper-V, ad esempio BLOC MAIUSC, pesi e riserve, sono applicabili solo quando l'hypervisor controlla direttamente Vicepresidente di programmazione, ad esempio come con i tipi di utilità di pianificazione classica e core.

#### Uso di utilità di pianificazione radice nei sistemi server

L'utilità di pianificazione radice non è consigliabile per l'utilizzo con Hyper-V nel server in questo momento, come le caratteristiche delle prestazioni non ancora sono state caratterizzate completamente e ottimizzate per supportare la vasta gamma di carichi di lavoro tipici di molte delle distribuzioni di virtualizzazione di server.

## Abilitazione SMT in macchine virtuali guest

Una volta hypervisor dell'host di virtualizzazione è configurato per utilizzare il tipo di utilità di pianificazione di base, le macchine virtuali guest possono essere configurate per utilizzare SMT se lo si desidera. Esposizione al fatto che VPs sono hyperthreading a una macchina virtuale guest consente l'utilità di pianificazione nel sistema operativo guest e i carichi di lavoro in esecuzione nella macchina virtuale per rilevare e utilizzare la topologia SMT nel proprio lavoro di pianificazione. In Windows Server 2016, guest SMT non è configurato per impostazione predefinita e deve essere abilitato in modo esplicito per l'amministratore dell'host Hyper-V. A partire da Windows Server 2019, nuove macchine virtuali create nell'host eredita topologia SMT dell'host per impostazione predefinita.  Vale a dire, una versione che 9.0 della macchina virtuale creata in un host con 2 thread SMT per ogni core anche vedrebbe 2 thread SMT per ogni core.

È necessario utilizzare PowerShell per abilitare SMT in una macchina virtuale guest; esiste un'interfaccia utente fornita nella console di gestione Hyper-V.
Per abilitare SMT in una macchina virtuale guest, Apri una finestra di PowerShell con autorizzazioni sufficienti e digita:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

Dove <n> è il numero di thread SMT per ogni core guest della macchina virtuale verrà visualizzato.  
Tieni presente che <n> = 0 il valore verrà impostato HwThreadCountPerCore in modo che corrisponda conteggio dei thread SMT dell'host per ogni valore di base.

>[!NOTE] 
>L'impostazione HwThreadCountPerCore = 0 è supportata a partire da Windows Server 2019.

Di seguito è riportato un esempio di informazioni di sistema intraprese dal sistema operativo guest in esecuzione in una macchina virtuale con 2 processori virtuali e SMT abilitato. Il sistema operativo guest è rilevare 2 processori logici che appartengono allo stesso core.

![Schermata che mostra msinfo32 in un macchina virtuale con SMT abilitato guest](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## Configurazione del tipo di utilità di pianificazione di hypervisor su Windows Server 2016 Hyper-V

Windows Server 2016 Hyper-V Usa il modello di utilità di pianificazione hypervisor classiche per impostazione predefinita. L'hypervisor può essere configurato per utilizzare l'utilità di pianificazione di core, per aumentare la sicurezza limitando VPs guest per l'esecuzione in corrispondente coppie SMT fisiche e per supportare l'uso di macchine virtuali con SMT pianificazione per la loro VPs guest.

>[!NOTE]
>Microsoft consiglia che tutti i clienti che eseguono Windows Server 2016 Hyper-V selezionano l'utilità di pianificazione core per garantire che gli host di virtualizzazione in modo ottimale siano protetti da macchine virtuali guest potenzialmente dannosi.

## Impostazioni predefinite di Windows Server 2019 Hyper-V tramite l'utilità di pianificazione di base

Per garantire gli host Hyper-V sono distribuiti in guidato la protezione ottimale, Windows Server 2019 Hyper-V userà ora il modello di utilità di pianificazione hypervisor core per impostazione predefinita. L'amministratore dell'host potrebbe facoltativamente configurare l'host per usare l'utilità di pianificazione classica legacy. Gli amministratori devono leggere, comprendere con attenzione e prendere in considerazione l'impatto in termini di che ogni tipo di utilità di pianificazione ha sulla sicurezza e prestazioni di host di virtualizzazione prima di eseguire l'override le impostazioni predefinite di tipo utilità di pianificazione.  Per ulteriori informazioni, vedere [Selezione tipo di informazioni su Hyper-V utilità di pianificazione](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection) .

### Aggiornamenti necessari

>[!NOTE]
>Gli aggiornamenti seguenti devono usare le funzionalità di utilità di pianificazione hypervisor descritte in questo documento. Questi aggiornamenti includono modifiche per supportare la nuova opzione BCD 'hypervisorschedulertype', è necessaria per la configurazione host.

| Versione | Release  | Aggiornamento richiesto | Articolo della Knowledge Base |
|--------------------|------|---------|-------------:|
|WindowsServer 2016 | 1607 | 2018.07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|WindowsServer 2016 | 1703 | 2018.07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|WindowsServer 2016 | 1709 | 2018.07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | Nessuno | Nessuno |

## La scelta del tipo di utilità di pianificazione di hypervisor su Windows Server

La configurazione di utilità di pianificazione hypervisor viene controllata tramite la voce BCD hypervisorschedulertype.

Per selezionare un tipo di utilità di pianificazione, Apri un prompt dei comandi con privilegi di amministratore:

``` command
     bcdedit /set hypervisorschedulertype type
```

Dove `type` è uno dei:

* Classica
* Core

È necessario riavviare il sistema per tutte le modifiche al tipo di utilità di pianificazione hypervisor per rendere effettive.

>[!NOTE]
>L'utilità di pianificazione radice hypervisor non è supportato in Windows Server Hyper-V in questo momento. Gli amministratori di Hyper-V non tentare di configurare l'utilità di pianificazione radice per l'uso con gli scenari di virtualizzazione di server.

## Determinazione del tipo di utilità di pianificazione corrente

Puoi determinare il tipo di utilità di pianificazione hypervisor corrente in uso, esaminando il Registro di memorizzazione nel Visualizzatore eventi per l'evento di avvio più recente di hypervisor ID 2, che indica il tipo di utilità di pianificazione hypervisor configurato all'avvio dell'hypervisor. Gli eventi di avvio dell'hypervisor possono essere ottenuti dal Visualizzatore eventi di Windows o tramite PowerShell.

Evento di lancio hypervisor 2 ID indica il tipo di utilità di pianificazione hypervisor, dove:

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![Schermata che mostra i dettagli evento 2 ID di avvio dell'hypervisor](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![Screenshot che mostra Visualizzatore eventi di visualizzazione di evento di lancio hypervisor ID 2](media/Hyper-V-CoreScheduler-EventViewer.png)

### Esecuzione di query l'evento di lancio tipo di utilità di pianificazione di Hyper-V hypervisor con PowerShell

Per eseguire una query per hypervisor evento ID 2 con PowerShell, Immetti i comandi seguenti da un prompt di PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![Screenshot che mostra la query di PowerShell e i risultati per l'evento di lancio hypervisor ID 2](media/Hyper-V-CoreScheduler-PowerShell.png)
