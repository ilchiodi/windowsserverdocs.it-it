---
title: Ottimizzazione di Windows 10 versione 1909 per un ruolo Virtual Desktop Infrastructure (VDI)
description: Impostazioni e configurazione consigliate per ridurre il sovraccarico per i desktop Windows 10 versione 1909 usati come immagini VDI.
ms.prod: windows-server
ms.reviewer: robsmi
ms.technology: remote-desktop-services
ms.author: helohr
ms.topic: article
author: heidilohr
manager: lizross
ms.date: 02/19/2020
ms.openlocfilehash: 44aa465773674625fa392a644ffb188140138bde
ms.sourcegitcommit: 1c75e4b3f5895f9fa33efffd06822dca301d4835
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/20/2020
ms.locfileid: "77519596"
---
# <a name="optimizing-windows-10-version-1909-for-a-virtual-desktop-infrastructure-vdi-role"></a>Ottimizzazione di Windows 10 versione 1909 per un ruolo Virtual Desktop Infrastructure (VDI)

Questo articolo consente di scegliere le impostazioni per Windows 10 versione 1909 (build 18363) allo scopo di garantire le migliori prestazioni in un ambiente Virtual Desktop Infrastructure (VDI). Tutte le impostazioni indicate in questa guida sono raccomandazioni da prendere in considerazione e non sono in alcun modo requisiti obbligatori.

I metodi principali per ottimizzare le prestazioni di Windows 10 consistono nel limitare le attività che ridisegnano la grafica delle app e le attività in background che non offrono vantaggi consistenti all'ambiente VDI e in generale nel ridurre al minimo il numero di processi in esecuzione. Un obiettivo secondario consiste nel ridurre al minimo l'uso di spazio su disco nell'immagine di base. Con le implementazioni VDI, la più piccola base possibile, o anche dimensione dell'immagine "oro", può ridurre leggermente l'utilizzo di memoria sull'hypervisor, così come comportare una piccola riduzione delle operazioni di rete complessive necessarie per fornire l'immagine del desktop al consumatore.

> [!NOTE]
> Questa impostazioni consigliate possono essere applicate ad altre installazioni di Windows 10 1909, comprese quelle eseguite in computer fisici o altre macchine virtuali. Nessuna raccomandazione inclusa in questo articolo influisce sulla supportabilità di Windows 10 1909.

## <a name="vdi-optimization-principles"></a>Principi di ottimizzazione di VDI

Un ambiente VDI presenta una sessione desktop completa, incluse le applicazioni, a un utente di computer in rete. Il veicolo di recapito della rete può essere una rete locale o Internet. Gli ambienti VDI sono un'immagine del sistema operativo "di base", che diventa quindi la base per i desktop successivamente presentati agli utenti. Sono disponibili diverse varianti di implementazioni VDI, ad esempio "persistente", "non persistente" e "sessione desktop". Il tipo persistente mantiene le modifiche apportate al sistema operativo desktop VDI da una sessione all'altra. Il tipo non persistente non mantiene le modifiche apportate al sistema operativo desktop VDI da una sessione all'altra. Per l'utente questo desktop non è molto diverso da quello degli altri dispositivi virtuali o fisici, tranne per il fatto che l'accesso avviene attraverso una rete.

Le impostazioni di ottimizzazione avvengono su un dispositivo di riferimento. Una macchina virtuale è il dispositivo ideale per compilare l'immagine, perché è possibile salvare lo stato, creare i checkpoint ed eseguire i backup. Viene eseguita un'installazione predefinita del sistema operativo nella macchina virtuale di base. La macchina virtuale di base viene quindi ottimizzata rimuovendo le app non necessarie, installando gli aggiornamenti di Windows e altri aggiornamenti, eliminando i file temporanei e applicando le impostazioni.

Esistono altri tipi di VDI, ad esempio Servizi Desktop remoto (RDS) e il servizio [Desktop virtuale Windows](https://azure.microsoft.com/services/virtual-desktop/) rilasciato di recente. Una discussione approfondita su queste tecnologie esula dall'ambito di questo articolo. Questo articolo è incentrato sulle impostazioni dell'immagine di base di Windows, senza riferimenti ad altri fattori dell'ambiente, ad esempio l'ottimizzazione dell'host.

Quando si tratta di prodotti e servizi, la sicurezza e la stabilità sono le principali priorità per Microsoft. I clienti aziendali possono scegliere di utilizzare l'app integrata Sicurezza di Windows, una suite di servizi che funzionano perfettamente con o senza Internet. Per gli ambienti VDI non connessi a Internet, le firme di sicurezza possono essere scaricate più volte al giorno. Microsoft può infatti rilasciare più aggiornamenti delle firme ogni giorno. Tali firme possono quindi essere fornite alle macchine virtuali VDI e ne può essere pianificata l'installazione durante la produzione, indipendentemente dal fatto che l'installazione sia persistente o non persistente. In questo modo, la protezione della macchina virtuale è la più aggiornata possibile.

Alcune impostazioni di sicurezza non sono applicabili agli ambienti VDI non connessi a Internet e pertanto non sono in grado di partecipare alla sicurezza abilitata per il cloud. I dispositivi Windows "normali" possono utilizzare altre impostazioni, ad esempio l'esperienza cloud, Windows Store e così via. La rimozione dell'accesso a funzionalità non usate riduce il footprint, la larghezza di banda di rete e la superficie di attacco.

Per quanto riguarda gli aggiornamenti, Windows 10 utilizza un algoritmo di aggiornamento mensile e non è quindi necessario che i client tentino di eseguire l'aggiornamento. Nella maggior parte dei casi, gli amministratori VDI controllano il processo di aggiornamento tramite un processo di arresto delle macchine virtuali basate su un'immagine "master" o "gold", sbloccano l'immagine che è di sola lettura, applicano patch all'immagine e quindi eseguono di nuovo il sealing e ritrasferiscono l'immagine nell'ambiente produzione. Di conseguenza, non è necessario che le macchine virtuali VDI controllino Windows Update. In alcuni casi, ad esempio nelle macchine virtuali VDI persistenti, vengono eseguite le normali procedure di applicazione delle patch. È possibile usare anche Windows Update o Microsoft Intune. System Center Configuration Manager può essere usato per gestire l'aggiornamento e il recapito di altri pacchetti. È compito di ogni organizzazione stabilire l'approccio migliore per aggiornare VDI.

> [!TIP]  
> Uno script che implementa le ottimizzazioni discusse in questo argomento, nonché un file di esportazione di un oggetto Criteri di gruppo che è possibile importare con **LGPO.exe**, è disponibile su [TheVDIGuys](https://github.com/TheVDIGuys) su GitHub.

Questo script è stato progettato per soddisfare ambiente e requisiti specifici. Il codice principale è PowerShell e le attività vengono eseguite usando file di input (testo normale) con file di esportazione dello strumento Oggetto Criteri di gruppo locale (LGPO). Questi file contengono elenchi di app da rimuovere e servizi da disabilitare. Se non vuoi rimuovere un'app specifica o disabilitare un servizio specifico, modifica il file di testo corrispondente e rimuovi l'elemento. Infine, esistono impostazioni di criteri locali che possono essere importate nel dispositivo. È preferibile che alcune impostazioni siano presenti all'interno dell'immagine di base, anziché applicare le impostazioni tramite Criteri di gruppo, poiché alcune impostazioni diventano effettive al riavvio successivo o quando un componente viene usato per la prima volta.

### <a name="persistent-vdi"></a>VDI persistente

La VDI persistente è, a livello di base, una macchina virtuale che mantiene lo stato del sistema operativo tra un riavvio e l'altro. Altri livelli software della soluzione VDI forniscono agli utenti un accesso facile e uniforme alle macchine virtuali assegnate, spesso con una soluzione Single Sign-On.

Esistono diversi tipi di implementazione VDI permanente:

- Macchine virtuali tradizionali, in cui la macchina virtuale ha un proprio file di disco virtuale, si avvia normalmente e salva le modifiche tra una sessione e l'altra. La differenza riguarda la modalità di accesso da parte dell'utente alla macchina virtuale. Potrebbe esserci un portale Web a cui l'utente accede e che indirizza automaticamente l'utente a una o più macchine virtuali VDI assegnate.

- Macchine virtuali persistenti basate su immagine, facoltativamente con dischi virtuali personali. In questo tipo di implementazione è presente un'immagine di base/oro su uno o più server host. Viene creata una macchina virtuale e uno o più dischi virtuali vengono creati e assegnati a questo disco per l'archiviazione permanente.

    - Quando la macchina virtuale viene avviata, una copia dell'immagine di base viene letta nella memoria della macchina. Allo stesso tempo, un disco virtuale persistente viene assegnato a tale macchina virtuale, con eventuali modifiche precedenti del sistema operativo unite tramite una procedura complessa.

    - Le modifiche, ad esempio operazioni di scrittura registro eventi, scritture nel registro e così via vengono reindirizzate al disco virtuale di lettura/scrittura assegnate a tale macchina virtuale.

    - In questa circostanza, la manutenzione del sistema operativo e delle app potrebbe funzionare normalmente, usando software di manutenzione tradizionale come Windows Server Update Services o altre tecnologie di gestione.

    - La differenza tra una macchina VDI persistente e una macchina virtuale "normale" consiste nella relazione con l'immagine master o gold. A un certo punto gli aggiornamenti devono essere applicati all'immagine master. Questo è il punto in cui le implementazioni stabiliscono come devono essere gestite le modifiche permanenti dell'utente. In alcuni casi, il disco con le modifiche viene rimosso e/o reimpostato, con la conseguente impostazione di un nuovo checkpoint. È anche possibile che le modifiche apportate dall'utente vengano mantenute tramite aggiornamenti qualitativi mensili e che la base venga reimpostata a seguito di un aggiornamento delle funzionalità.

### <a name="non-persistent-vdi"></a>VDI non persistente

Quando un'implementazione VDI non persistente è basata su un'immagine di base o "oro", le ottimizzazioni vengono eseguite principalmente nell'immagine di base e quindi attraverso impostazioni e criteri locali.

Con l'infrastruttura VDI non persistente basata su immagine, l'immagine di base è di sola lettura. Quando viene avviata una macchina virtuale non persistente, a tale macchina viene trasmessa una copia dell'immagine di base. Attività che si verifica durante l'avvio e successivamente finché il riavvio successivo non viene reindirizzato a un percorso temporaneo. Agli utenti vengono in genere forniti percorsi di rete per archiviare i dati. In alcuni casi, il profilo dell'utente viene unito con la macchina virtuale standard per fornire all'utente le relative impostazioni.

Un aspetto importante dell'infrastruttura VDI non persistente basata su una singola immagine è la manutenzione. Gli aggiornamenti del sistema operativo e dei componenti vengono in genere distribuiti una volta al mese. Con la VDI basata su immagine è necessario eseguire un set di processi per ottenere gli aggiornamenti dell'immagine:

- In un determinato host tutte le macchine virtuali presenti che derivano dall'immagine di base devono essere arrestate o disabilitate. Ciò significa che gli utenti vengono reindirizzati ad altre macchine virtuali.

- L'immagine di base viene quindi aperta e avviata. Vengono quindi eseguite tutte le attività di manutenzione, come gli aggiornamenti del sistema operativo, gli aggiornamenti .NET, gli aggiornamenti delle applicazioni, ecc.

- In questa fase vengono applicate tutte le nuove impostazioni che è necessario applicare.

- Le altre operazioni di manutenzione vengono eseguite in questo momento.

- L'immagine di base viene quindi arrestata.

- Viene eseguito il sealing dell'immagine di base, che viene quindi impostata per tornare in produzione.

- Gli utenti possono eseguire nuovamente l'accesso.

> [!NOTE]
> Windows 10 esegue automaticamente un set di attività di manutenzione con cadenza periodica. Per impostazione predefinita, ogni giorno alle 15:00 viene eseguita un'attività pianificata. Questa attività pianificata esegue un elenco di attività, tra cui la pulizia di Windows Update. È possibile visualizzare tutte le categorie di manutenzione eseguite automaticamente tramite questo comando PowerShell:
>
>```powershell
>Get-ScheduledTask | ? {$_.Settings.MaintenanceSettings}
>```
>

Una delle principali problematiche della VDI non persistente è relativa al fatto che, quando un utente si disconnette, quasi tutte le attività del sistema operativo vengono rimosse. Lo stato e/o il profilo dell'utente potrebbero essere salvati in una posizione centralizzata, ma la macchina virtuale stessa rimuove quasi tutte le modifiche apportate dopo l'ultimo avvio. Di conseguenza, le ottimizzazioni destinate a un computer Windows che consente di salvare lo stato tra una sessione e l'altra non sono applicabili.

A seconda dell'architettura della macchina virtuale con VDI, elementi come PreFetch e SuperFetch non saranno di aiuto tra una sessione e l'altra, in quanto tutte le ottimizzazioni vengono rimosse al riavvio della macchina virtuale. L'indicizzazione potrebbe rappresentare una perdita parziale di risorse, così come qualsiasi ottimizzazione del disco come una deframmentazione tradizionale.

> [!NOTE]
> Se prepari un'immagine con la virtualizzazione e durante il processo di creazione dell'immagine sei connesso a Internet, al primo accesso devi posticipare gli aggiornamenti delle funzionalità passando a **Impostazioni**, **Windows Update**.

### <a name="to-sysprep-or-not-sysprep"></a>Sysprep o no?

Windows 10 offre una funzionalità incorporata denominata [Utilità preparazione sistema](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview), spesso abbreviata con "Sysprep". Lo strumento Sysprep viene usato per preparare un'immagine personalizzata di Windows 10 per fini di duplicazione. Il processo Sysprep garantisce che il sistema operativo risultante sia univoco per l'esecuzione in ambiente di produzione.

Ci sono ragioni a favore e contro l'esecuzione di Sysprep. Nel caso di VDI, si potrebbe voler personalizzare il profilo utente predefinito da usare come modello di profilo per gli utenti successivi che si connettono usando questa immagine. Potrebbero esserci applicazioni che si desidera installare, ma si potrebbe volere anche controllare le impostazioni per ogni singola app.

L'alternativa è quella di usare un file .ISO standard da cui eseguire l'installazione, possibilmente usando un file di risposte all'installazione invisibile all'utente, e una sequenza di operazioni per installare o rimuovere applicazioni. Puoi anche usare una sequenza di attività per regolare le impostazioni dei criteri locali nell'immagine, ad esempio tramite lo [strumento Oggetto Criteri di gruppo locale (LGPO)](https://docs.microsoft.com/archive/blogs/secguide/lgpo-exe-local-group-policy-object-utility-v1-0).

### <a name="supportability"></a>Supportabilità

Ogni volta che vengono modificate le impostazioni predefinite di Windows, sorgono domande relative alla supportabilità. Dopo la personalizzazione di un'immagine VDI (macchina virtuale o sessione), è necessario tenere traccia di ogni modifica apportata all'immagine in un registro modifiche. In fase di risoluzione dei problemi, spesso un'immagine può essere isolata in un pool e configurata per l'analisi dei problemi. Dopo che un problema è stato monitorato fino a risalire alla causa radice, la modifica può essere implementata prima nell'ambiente di test e infine nel carico di lavoro di produzione.

Questo documento evita intenzionalmente di trattare temi come i servizi di sistema, i criteri o le attività che influiscono sulla sicurezza. A questi si aggiunge il tema della manutenzione di Windows. Viene eliminata la possibilità di eseguire la manutenzione delle immagini VDI al di fuori delle finestre di manutenzione, poiché tali finestre sono quelle in cui si verificano la maggior parte degli eventi di manutenzione negli ambienti VDI, *tranne gli aggiornamenti software della sicurezza*. Microsoft ha pubblicato linee guida per la sicurezza di Windows negli ambienti VDI. Per altre informazioni, vedi la [Guida alla distribuzione per Windows Defender Antivirus in un ambiente Virtual Desktop Infrastructure (VDI)](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus).

Prendi in considerazione la supportabilità quando modifichi le impostazioni predefinite di Windows. Quando si modificano i servizi di sistema, i criteri o le attività pianificate in nome della protezione avanzata, di un "alleggerimento" della protezione e così via, possono verificarsi problemi complessi. Per i problemi attualmente noti riguardanti le impostazioni predefinite modificate, consulta Microsoft Knowledge Base. Per quanto riguarda i problemi noti, se presenti, le istruzioni contenute in questo documento e lo script associato disponibile in GitHub verranno aggiornati. Inoltre, puoi segnalare a Microsoft gli eventuali problemi in diversi modi.

Per visualizzare i problemi noti relativi ai valori iniziali predefiniti per i servizi, puoi usare il motore di ricerca preferito con i termini ""start value" site:support.microsoft.com".

Puoi notare che questo documento e gli script associati in GitHub non modificano alcuna autorizzazione predefinita. Se ti interessa aumentare le impostazioni di sicurezza, inizia con il progetto definito **AaronLocker**. Per altre informazioni, vedi [ANNUNCIO: Inserimento delle applicazioni nell'elenco elementi consentiti con "AaronLocker"](https://docs.microsoft.com/archive/blogs/aaron_margosis/announcing-application-whitelisting-with-aaronlocker).

#### <a name="vdi-optimization-categories"></a>Categorie di ottimizzazione di VDI

- Categorie di impostazioni del sistema operativo globali:

    - Pulizia di app UWP

    - Pulizia di funzionalità facoltative

    - Impostazioni del criterio locale

    - Servizi di sistema

    - Attività pianificate

    - Applica aggiornamenti di Windows e di altro tipo

    - Tracce automatiche di Windows

    - Pulizia disco prima dell'immagine di finalizzazione (sealing)

    - Impostazioni utente

    - Impostazioni di hypervisor/host

### <a name="universal-windows-platform-uwp-application-cleanup"></a>Pulizia di applicazioni UWP (Universal Windows Platform)

Uno degli obiettivi di un'immagine VDI è quello di avere dimensioni minime. Un modo per ridurre le dimensioni dell'immagine consiste nel rimuovere le applicazioni UWP che non verranno usate nell'ambiente. Con le app UWP, sono presenti i file dell'applicazione principali, noti anche come payload. È presente una piccola quantità di dati archiviati in ciascun profilo utente per impostazioni specifiche dell'applicazione. È anche presente una piccola quantità di dati nel profilo relativo a tutti gli utenti.

Connettività e tempistica sono fattori importanti per la pulizia delle app UWP. Se distribuisci l'immagine di base in un dispositivo senza connettività di rete, Windows 10 non può connettersi a Microsoft Store e scaricare le app per poi provare a installarle mentre provi a disinstallarle. Potrebbe essere una strategia valida per concederti il tempo necessario per personalizzare l'immagine e quindi aggiornare gli elementi rimanenti in una fase successiva del processo di creazione dell'immagine.

Se modifichi il file WIM di base usato per installare Windows 10 e rimuovi dal file WIM le app UWP non necessarie prima dell'installazione, le app non verranno installate per iniziare e i tempi di creazione del profilo saranno più brevi. Più avanti in questa sezione sono riportate informazioni utili per rimuovere le app UWP dal file WIM di installazione.

Una buona strategia per VDI è quella di fornire le applicazioni desiderate nell'immagine di base, quindi limitare o bloccare l'accesso a Microsoft Store in seguito. Le applicazioni Microsoft Store vengono aggiornate periodicamente in background sui normali computer. Le app UWP possono essere aggiornate durante la finestra di manutenzione mentre vengono applicati altri aggiornamenti. Per altre informazioni, vedi [Universal Windows Platform Apps](https://docs.citrix.com/citrix-virtual-apps-desktops/manage-deployment/applications-manage/universal-apps.html) (App UWP (Universal Windows Platform)).

#### <a name="delete-the-payload-of-uwp-apps"></a>Eliminazione del payload delle app UWP

Le app UWP che non sono necessarie sono ancora nel file system e usano una piccola quantità di spazio su disco. Per quanto riguarda le app che non saranno mai necessarie, il payload delle app UWP indesiderate può essere rimosso dall'immagine di base usando i comandi PowerShell.

Infatti, se si rimuovono dal file .WIM di installazione usando i link forniti più avanti in questa sezione, dovrebbe essere possibile iniziare da capo con un elenco molto ridotto di app UWP.

Esegui il comando seguente per enumerare le app UWP di cui è stato effettuato il provisioning da un sistema operativo in esecuzione, come indicato in questo esempio troncato di output da PowerShell:

```powershell

    Get-AppxProvisionedPackage -Online

    DisplayName  : Microsoft.3DBuilder
    Version      : 13.0.10349.0  
    Architecture : neutral
    ResourceId   : \~ 
    PackageName  : Microsoft.3DBuilder_13.0.10349.0_neutral_\~_8wekyb3d8bbwe
    Regions      :
    ...
```

Le app UWP fornite a un sistema possono essere rimosse durante l'installazione del sistema operativo come parte di una sequenza di operazioni, o successivamente dopo l'installazione del sistema operativo. Ciò potrebbe essere il metodo preferito perché rende modulare l'intero processo di creazione o mantenimento di un'immagine. Se sviluppi gli script, in caso di modifiche in una build successiva, modificherai uno script esistente anziché ripetere il processo da zero. Di seguito sono riportati alcuni link a informazioni su questo argomento:

[Rimozione delle applicazioni inbox Windows 10 durante una sequenza di attività](https://blogs.technet.microsoft.com/mniehaus/2015/11/11/removing-windows-10-in-box-apps-during-a-task-sequence/)

[Rimozione delle applicazioni integrate nel file WIM di Windows 10 con Powershell - Versione 1.3](https://gallery.technet.microsoft.com/Removing-Built-in-apps-65dc387b)

[Windows 10 1607: impedire il ritorno delle applicazioni durante la distribuzione dell'aggiornamento delle funzionalità](https://blogs.technet.microsoft.com/mniehaus/2016/08/23/windows-10-1607-keeping-apps-from-coming-back-when-deploying-the-feature-update/)

Eseguire quindi il comando [Remove-AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) PowerShell per rimuovere i payload delle app UWP:

```powershell
Remove-AppxProvisionedPackage -Online -PackageName
```

Ogni app UWP deve essere valutata per verificarne l'applicabilità in ogni singolo ambiente. Se vuoi installare un'installazione predefinita di Windows 10 1909, considera quali app sono in esecuzione e utilizzano memoria. Ad esempio, potresti provare a rimuovere le app che vengono avviate automaticamente o quelle che mostrano automaticamente informazioni nel menu Start, come Meteo e Notizie, e che potrebbero non essere utili nel tuo ambiente.

>[!NOTE]
>Se utilizzi gli script di GitHub, puoi controllare facilmente quali app vengono rimosse prima di eseguire lo script. Dopo aver scaricato i file di script, individua il file Win10_1909_AppxPackages.txt, modificalo e rimuovi le voci relative alle app che vuoi mantenere, ad esempio Calcolatrice, Memo e così via.

### <a name="manage-windows-optional-features-using-powershell"></a>Gestire le funzionalità facoltative di Windows con PowerShell

Puoi gestire le funzionalità facoltative di Windows con PowerShell. Per altre informazioni, vedi [Windows 10: gestione delle funzionalità facoltative con PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/39386.windows-10-managing-optional-features-with-powershell.aspx). Per enumerare le funzionalità di Windows attualmente installate, esegui il comando seguente di PowerShell:

```powershell
Get-WindowsOptionalFeature -Online
```

Puoi abilitare o disabilitare una specifica funzionalità facoltativa di Windows, come illustrato in questo esempio:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName "DirectPlay" -All
```

Puoi disabilitare le funzionalità nell'immagine VDI, come illustrato in questo esempio:

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName “WindowsMediaPlayer”
```

In un secondo momento, puoi decidere di rimuovere il pacchetto di Windows Media Player. In Windows 10 1909 sono presenti due pacchetti di Windows Media Player:

```powershell
Get-WindowsPackage -Online -PackageName *media*

PackageName       : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1
Applicable        : True
Copyright        : Copyright (c) Microsoft Corporation. All Rights Reserved
Company         :
CreationTime       :
Description       : Play audio and video files on your local device and on the Internet.
InstallClient      : DISM Package Manager Provider
InstallPackageName    : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1.mum
InstallTime       : 3/19/2019 6:20:22 AM
...

Features         : {}

PackageName       : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.449
Applicable        : True
Copyright        : Copyright (c) Microsoft Corporation. All Rights Reserved
Company         :
CreationTime       :
Description       : Play audio and video files on your local device and on the Internet.
InstallClient      : UpdateAgentLCU
InstallPackageName    : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.449.mum
InstallTime       : 10/29/2019 5:15:17 AM
...
```

Se vuoi rimuovere il pacchetto di Windows Media Player (per liberare circa 60 MB di spazio su disco):

```powershell
 Remove-WindowsPackage -PackageName Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1 -Online

 Remove-WindowsPackage -PackageName Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1 -Online
```

#### <a name="enable-or-disable-windows-features-using-dism"></a>Abilitare o disabilitare le funzionalità di Windows con Gestione e manutenzione immagini distribuzione

Per enumerare e controllare le funzionalità facoltative di Windows, puoi usare lo strumento incorporato Dism.exe. È possibile sviluppare ed eseguire uno script di Dism.exe durante la sequenza delle attività di installazione di un sistema operativo. La tecnologia Windows interessata è chiamata [funzionalità su richiesta](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).

#### <a name="default-user-settings"></a>Impostazioni utente predefinite

È possibile effettuare personalizzazioni in un file del Registro di sistema di Windows denominato C:\Users\Default\NTUSER.DAT. Tutte le impostazioni effettuate in questo file verranno applicate a tutti i profili utente successivi creati da un dispositivo che esegue questa immagine. Puoi controllare le impostazioni da applicare al profilo utente predefinito modificando il file Win10_1909_DefaultUserSettings.txt. Un'impostazione che può essere utile considerare con attenzione, nuova in questa iterazione di raccomandazioni sulle impostazioni, è quella denominata **TaskbarSmallIcons**. Prima di implementare questa impostazione, è consigliabile effettuare una verifica con la base degli utenti. **TaskbarSmallIcons** riduce le dimensioni della barra delle applicazioni di Windows e utilizza una minor quantità di spazio sullo schermo, rende le icone più compatte, riduce a icona l'interfaccia di ricerca ed è raffigurata nelle illustrazioni seguenti (prima e dopo l'impostazione):

Figura 1: Barra delle applicazioni normale di Windows 10, versione 1909

![Versione standard della barra delle applicazioni di Windows 10, versione 1909](media/rds-vdi-recommendations-1909/standard-taskbar.png)

Figura 2: Barra delle applicazioni con impostazione a icone piccole

![Barra delle applicazioni con impostazione a icone piccole](media/rds-vdi-recommendations-1909/taskbar-sm-icons.png)

Inoltre, per ridurre la trasmissione di immagini nell'infrastruttura VDI, puoi impostare lo sfondo predefinito su un colore tinta unita anziché sull'immagine di Windows 10 predefinita. Puoi impostare su un colore tinta unita anche la schermata di accesso, nonché disabilitare l'effetto sfocato opaco all'accesso.

Le impostazioni seguenti vengono applicate all'hive del Registro di sistema del profilo utente predefinito, principalmente per ridurre le animazioni. Se tutte o alcune di queste impostazioni sono indesiderate, elimina le impostazioni da non applicare ai nuovi profili utente basati su questa immagine. L'obiettivo di queste impostazioni è quello di abilitare le impostazioni equivalenti seguenti:

Figura 3: Proprietà di sistema ottimizzate, Opzioni prestazioni

![Proprietà di sistema ottimizzate, Opzioni prestazioni](media/rds-vdi-recommendations-1909/performance-options.png)

Di seguito sono riportate le impostazioni di ottimizzazione applicate all'hive del Registro di sistema del profilo utente predefinito per ottimizzare le prestazioni:

```
Delete HKLM\Temp\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /v OneDriveSetup /f
add "HKLM\Temp\Control Panel\Desktop" /v DragFullWindows /t REG_SZ /d 0 /f
add "HKLM\Temp\Control Panel\Desktop" /v WallPaper /t REG_SZ /d "" /f
add "HKLM\Temp\Control Panel\Desktop\WindowMetrics" /v MinAnimate /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v AccentColor /t REG_DWORD /d 4292311040 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v ColorizationColor /t REG_DWORD /d 4292311040 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v AlwaysHibernateThumbnails /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v EnableAeroPeek /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v AlwaysHibernateThumbnails /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v AutoCheckSelect /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v HideIcons /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v ListviewAlphaSelect /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v ListViewShadow /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v ShowInfoTip /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v TaskbarAnimations /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v TaskbarSmallIcons /t REG_DWORD /d 1 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\People /v PeopleBand /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\AnimateMinMax /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\ComboBoxAnimation /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\ControlAnimations /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\DWMAeroPeekEnabled /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\DWMSaveThumbnailEnabled /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\MenuAnimation /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\SelectionFade /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\TaskbarAnimations /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\TooltipAnimation /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SubscribedContent-338388Enabled /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SubscribedContent-338389Enabled /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SystemPaneSuggestionsEnabled /t REG_DWORD /d 0 /f
```

Nelle impostazioni dei criteri locali puoi decidere di disabilitare le immagini per gli sfondi in VDI.  Se vuoi le immagini, puoi creare immagini di sfondo personalizzate con un'intensità di colore ridotta per limitare la larghezza di banda di rete usata per la trasmissione delle informazioni sull'immagine. Se decidi di specificare l'assenza dell'immagine di sfondo nei criteri locali, puoi decidere di impostare il colore di sfondo prima dei criteri locali, perché una volta impostati i criteri, l'utente non può in alcun modo modificare il colore di sfondo. Sarebbe preferibile specificare "(null)" come immagine di sfondo. Nella prossima sezione è presente un'altra impostazione dei criteri relativa al non uso dello sfondo nelle sessioni Remote Desktop Protocol.

### <a name="local-policy-settings"></a>Impostazioni del criterio locale

Usando i criteri di Windows, possono essere effettuate diverse ottimizzazioni per Windows 10 in un ambiente VDI. Le impostazioni elencate nella tabella di questa sezione possono essere applicate localmente all'immagine di base o gold. Se impostazioni equivalenti non vengono specificate in altro modo, ad esempio tramite criteri di gruppo, le impostazioni continuano ad essere applicate.

Alcune decisioni possono essere basate sulle specifiche dell'ambiente, ad esempio:

- L'ambiente VDI è abilitato all'accesso a Internet?

- La soluzione VDI è persistente o non persistente?

Le impostazioni seguenti sono state scelte in modo da non contrastare o confliggere con le impostazioni correlate alla sicurezza. Queste impostazioni sono state scelte per rimuovere le impostazioni o disabilitare le funzionalità che possono non essere applicabili agli ambienti VDI.

| Impostazione di criteri | Item | Elemento secondario | Impostazione possibile e commenti|
| -------------- | ---- | -------- | ---------------------------- |
| Criteri del computer locale \\ Configurazione computer \\ Impostazioni di Windows \\ Impostazioni di sicurezza | | | |
| Criteri Gestione elenco reti | Proprietà di Tutte le reti | Percorso di rete | Cambiamento del percorso non consentito |
| Criteri del computer locale \\ Configurazione computer \\ Modelli amministrativi \\ Pannello di controllo | | | |
| *Pannello di controllo | Consenti i suggerimenti online | | Disattivato. Le impostazioni non contatteranno Servizi per i contenuti Microsoft per recuperare i suggerimenti e il contenuto della Guida. |
| *Pannello di controllo\Personalizzazione | Non visualizzare la schermata di blocco | Abilitata. Questa impostazione controlla se la schermata di blocco viene visualizzata per gli utenti. Se abiliti questa impostazione dei criteri, gli utenti che non sono obbligati a premere CTRL+ALT+CANC prima di accedere vedranno il loro riquadro selezionato dopo il blocco del computer. |
| *Pannello di controllo\Personalizzazione | Imponi immagine specifica per l'accesso e la schermata di blocco predefinita | [![Interfaccia utente per impostare il percorso della schermata di blocco](media/lock-screen-image-settings.png)](media/lock-screen-image-settings.png) | Abilitata. Questa impostazione ti consente di specificare la schermata di blocco predefinita e l'immagine di accesso visualizzata quando nessun utente ha eseguito l'accesso. Imposta anche l'immagine specificata come predefinita per tutti gli utenti e sostituisce l'immagine predefinita.<p>Consigliamo di usare un'immagine non complessa e a bassa risoluzione, in modo da trasmettere in rete una minor quantità di dati ogni volta che viene eseguito il rendering dell'immagine. |
| *Pannello di controllo\Opzioni internazionali e della lingua\Personalizzazione riconoscimento grafia | Disattiva apprendimento automatico | | Abilitata. Se abiliti questa impostazione dei criteri, l'apprendimento automatico viene interrotto e tutti i dati archiviati vengono eliminati. Questa impostazione non può essere configurata dagli utenti nel Pannello di controllo. |
| Criteri del computer locale \\ Configurazione computer \\ Modelli amministrativi \\ Rete | | | |
| Servizio trasferimento intelligente in background (BITS) | Non consentire al client BITS di utilizzare Windows BranchCache |  | Enabled |
| Servizio trasferimento intelligente in background (BITS) | Non consentire al computer di agire come un client Peer caching BITS |  | Enabled |
| Servizio trasferimento intelligente in background (BITS) | Non consentire al computer di agire come un server Peer caching BITS |  | Enabled |
| Servizio trasferimento intelligente in background (BITS) | Consenti il Peer caching BITS |  | Disabled |
| BranchCache | Attiva BranchCache |  | Disabled |
| *Tipi di carattere | Abilita provider di tipi di carattere |  | Disattivato. Windows non si connette a un provider di tipi di carattere online ed enumera solo i tipi di carattere installati in locale. |
| Autenticazione hotspot | Attiva l'autenticazione hotspot |  | Disabled |
| Servizi di rete peer-to-peer Microsoft | Disattiva i servizi di rete peer-to-peer Microsoft |  | Enabled |
| Indicatore stato connettività di rete | Specifica polling passivo | Disabilita polling passivo (casella di controllo) | Abilitata. Usa questa impostazione se ti trovi in una rete isolata o usi un indirizzo IP statico. |
| File offline | Consenti o proibisci l'uso della funzione File offline |  | Disabled |
| Impostazioni TCPIP \\ Tecnologie di transizione IPv6 | Imposta stato Teredo | Stato disabilitato | Abilitata. Nello stato disabilitato non sono presenti interfacce Teredo nell'host. |
| Servizio WLAN \\ Impostazioni WLAN | Consenti a Windows di connettersi automaticamente agli hotspot aperti suggeriti, alle reti condivise dai contatti e agli hotspot che offrono servizi a pagamento |  | Disattivato. Le impostazioni **Connettiti agli hotspot aperti suggeriti**, **Connettiti alle reti condivise dai miei contatti** e **Abilita servizi a pagamento** sono disabilitate, ma gli utenti di questo dispositivo possono abilitarle. |
| Criteri del computer locale \\ Configurazione computer \\ Modelli amministrativi \\ Barra delle applicazioni e menu Start |  |  |  |
| *Notifiche | Disattiva l'uso della rete per le notifiche |  | Abilitata. Se abiliti questa impostazione, le app e le funzionalità di sistema non saranno in grado di ricevere notifiche di rete da WNS o usando API di polling delle notifiche. |
| Criteri del computer locale \\ Configurazione computer \\ Modelli amministrativi \\ Sistema |  |  |  |
| Installazione dispositivi | Non inviare una segnalazione errore di Windows quando si installa un driver generico per un dispositivo |  | Enabled |
| Installazione dispositivi | Impedisci la creazione di un punto di ripristino di sistema durante l'attività dei dispositivi che richiedono normalmente la creazione di un punto di ripristino |  | Enabled |
| Installazione dispositivi | Impedisci il recupero dei metadati del dispositivo da Internet |  | Enabled |
| Installazione dispositivi | Impedisci a Windows di inviare una segnalazione errore quando un driver di dispositivo richiede software aggiuntivo durante l'installazione |  | Enabled |
| Installazione dispositivi | Disattiva fumetti **Trovato nuovo hardware** durante l'installazione del dispositivo. |  | Enabled |
| Filesystem \\ NTFS | Opzioni di creazione nomi brevi | Disabilitato su tutti i volumi | Enabled |
| *Criteri di gruppo | Configura i collegamenti tra Web e app con i gestori degli URL dell'app |  | Disattivato. Disabilita il collegamento tra Web e app e gli URI http(s) vengono aperti nel browser predefinito invece di avviare l'app associata. |
| *Criteri di gruppo | Continua esperienze in questo dispositivo |  | Disattivato. Il dispositivo Windows non è individuabile da altri dispositivi e non può partecipare a esperienze tra più dispositivi. |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva accesso a tutte le funzionalità di Windows Update |  |Abilitata. Se abiliti questa impostazione dei criteri, tutte le funzionalità di Windows Update vengono rimosse. Include il blocco dell'accesso al sito Web Windows Update https://windowsupdate.microsoft.com, dal collegamento ipertestuale Windows Update nel menu Start e inoltre sul strumenti menu in Internet Explorer. Anche il servizio di aggiornamento automatico di Windows viene disabilitato. Non riceverai alcuna notifica né aggiornamenti critici da Windows Update. Questa impostazione dei criteri impedisce inoltre a Gestione dispositivi di installare automaticamente gli aggiornamenti dei driver dal sito Web Windows Update. |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva aggiornamento automatico certificati radice |  |Abilitata. Se abiliti questa impostazione dei criteri, quando ti viene presentato un certificato rilasciato da un'autorità radice non attendibile il tuo computer non contatterà il sito Web Windows Update per verificare se Microsoft ha aggiunto la CA all'elenco delle autorità affidabili. NOTA: Usare questo criterio solo se si dispone di un metodo alternativo per l'elenco di revoche di certificati. |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva collegamenti di "Events.asp" in Visualizzatore eventi |  | Enabled |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva condivisione dati personalizzazione riconoscimento grafia |  | Enabled |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva segnalazione errori di riconoscimento grafia |  | Enabled |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva contenuto nella sezione "Non tutti sanno che" di Guida e supporto tecnico content |  | Enabled |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva ricerca Microsoft Knowledge Base in Guida e supporto tecnico |  | Enabled |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva Connessione guidata Internet se connessione a URL rimanda a Microsoft.com |  | Enabled |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva download Internet per le procedure di pubblicazione guidata sul Web e ordinazione guidata online via Internet |  | Enabled |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva servizio Internet associazioni file |  | Enabled |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva registrazione se connessione a URL rimanda a Microsoft.com |  | Enabled |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva l'operazione immagine "Ordina stampe" |  | Enabled |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva l'operazione per file e cartelle "Pubblica sul Web" |  | Enabled |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva Analisi utilizzo software Windows Messenger |  | Enabled |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva il programma Analisi utilizzo software Windows |  | Enabled |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva test attivi dell'indicatore di stato della connettività di rete Windows |  | Abilitata. Questa impostazione dei criteri disabilita i test attivi eseguiti dall'indicatore di stato della connettività di rete di Windows (NCSI) per determinare se il tuo computer è connesso a Internet o a una rete più limitata. Nell'ambito della determinazione del livello di connettività, NCSI esegue uno dei due test attivi: download di una pagina da un server Web dedicato o invio di una richiesta DNS per un indirizzo dedicato. Se si attiva questa impostazione del criterio, NCSI non esegue nessuno dei due test attivi. Questo comportamento può ridurre la capacità di NCSI, e di altri componenti che usano NCSI, di determinare l'accesso a Internet) NOTA: Esistono altri criteri che consentono di reindirizzare i test NCSI alle risorse interne, se si desidera questa funzionalità. |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva Segnalazione errori Windows |  | Enabled |
| Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet | Disattiva ricerca driver di dispositivo in Windows Update |  | Enabled |
| Accesso | Mostra animazione primo accesso |  | Disabled |
| Accesso | Disattiva le notifiche nella schermata di blocco |  | Enabled |
| Accesso | Disattiva il suono di avvio di Windows |  | Enabled |
| Risparmio energia | Seleziona una combinazione attiva per il risparmio di energia | Prestazioni elevate | Enabled |
| Ripristino | Consenti il ripristino del sistema allo stato predefinito |  | Disabled |
| *Integrità archiviazione | Consenti download degli aggiornamenti per il modello predittivo di errore del disco |  | Disattivato. Gli aggiornamenti non vengono scaricati per il modello predittivo di errore del disco. |
| *Servizio Ora di Windows \\ Provider servizi orari | Abilita client Windows NTP |  | Disattivato. Se disabiliti o non configuri questa impostazione dei criteri, l'orologio del computer locale non sincronizza l'ora con i server NTP. NOTA: valuta attentamente l'applicazione di questa impostazione. I dispositivi Windows che sono collegati a un dominio devono usare **NT5DS**. Il controller di dominio al controller di dominio padre può usare NTP. Il ruolo PDCe potrebbe usare NTP. Le macchine virtuali a volte usano "miglioramenti" o "servizi di integrazione". |
| Risoluzione dei problemi e diagnostica \\ Manutenzione pianificata | Configura comportamento di manutenzione pianificata |   | Disabled |
| Risoluzione dei problemi e diagnostica \\ Diagnostica prestazioni avvio Windows | Configura il livello di esecuzione di uno scenario |   | Disabled |
| Risoluzione dei problemi e diagnostica \\ Diagnosi delle perdite di memoria di Windows | Configura il livello di esecuzione di uno scenario |   | Disabled |
| Risoluzione dei problemi e diagnostica \\ Rilevamento/Risoluzione esaurimento risorse di Microsoft Windows | Configura il livello di esecuzione di uno scenario |   | Disabled |
| Risoluzione dei problemi e diagnostica \\ Diagnostica prestazioni arresto Windows | Configura il livello di esecuzione di uno scenario |   | Disabled |
| Risoluzione dei problemi e diagnostica \\ Diagnostica prestazioni standby o riattivazione Windows | Configura il livello di esecuzione di uno scenario |   | Disabled |
| Risoluzione dei problemi e diagnostica \\ Diagnostica prestazioni velocità di risposta sistema Windows | Configura il livello di esecuzione di uno scenario |   | Disabled |
| *Profili utente | Disattiva ID annunci |   | Abilitata. Se abiliti questa impostazione dei criteri, l'ID annunci viene disabilitato. Le app non possono usare l'ID per le esperienze tra app. |
| Criteri del computer locale \\ Configurazione computer \\ Modelli amministrativi \\ Componenti di Windows |  |  |  |
| Aggiungi funzionalità a Windows 10 | Impedisci l'esecuzione della procedura guidata |  | Enabled |
| *Privacy delle app | Impedisci l'esecuzione della procedura guidata |  | Enabled |
| *Privacy delle app | Consenti alle app di Windows di accedere alle informazioni sugli account | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non sono autorizzate ad accedere alle informazioni sull'account e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consenti alle app di Windows di accedere all'intera cronologia | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non sono autorizzate ad accedere alla cronologia delle chiamate e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consenti alle app di Windows di accedere ai contatti | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non sono autorizzate ad accedere ai contatti e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consenti alle app di Windows di accedere alle informazioni di diagnostica riguardo le altre app | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se disabiliti o non configuri questa impostazione dei criteri, i dipendenti dell'organizzazione possono decidere se le app di Windows possono ottenere informazioni diagnostiche su altre app usando Impostazioni > Privacy sul dispositivo. |
| *Privacy delle app | Consenti alle app di Windows di accedere alle e-mail | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Consenti**, le app di Windows sono autorizzate ad accedere alla posta elettronica e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consenti alle app di Windows di accedere alla posizione | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non sono autorizzate ad accedere alla posizione e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consenti alle app di Windows di accedere alla messaggistica | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non sono autorizzate ad accedere alla messaggistica e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consenti alle app di Windows di accedere al movimento | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non sono autorizzate ad accedere ai dati di movimento e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consenti alle app di Windows di accedere alle notifiche | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non sono autorizzate ad accedere alle notifiche e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consenti alle app di Windows di accedere alle attività | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non sono autorizzate ad accedere alle attività e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consenti alle app di Windows di accedere al calendario | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non sono autorizzate ad accedere al calendario e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consenti alle app di Windows di accedere alla fotocamera | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non sono autorizzate ad accedere alla fotocamera e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consenti alle app di Windows di accedere al microfono | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non sono autorizzate ad accedere al microfono e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consenti alle app di Windows di accedere ai dispositivi attendibili | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non sono autorizzate ad accedere a dispositivi attendibili e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consentire alle app di Windows di comunicare con i dispositivi non associati | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non sono autorizzate a comunicare con dispositivi wireless disassociati e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consenti alle app di Windows di accedere alle radio | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non potranno accedere alle radio di controllo e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consenti alle app di Windows di effettuare chiamate | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non sono autorizzate a effettuare chiamate telefoniche e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| *Privacy delle app | Consenti l'esecuzione delle app Windows in background | Valore predefinito per tutte le app: Forza Nega | Abilitata. Se scegli l'opzione **Forza Nega**, le app di Windows non sono autorizzate all'esecuzione in background e i dipendenti dell'organizzazione non possono modificare questa impostazione. |
| Criteri AutoPlay | Imposta il comportamento predefinito per l'esecuzione automatica | Non eseguire i comandi di esecuzione automatica | Enabled |
| *Criteri AutoPlay | Disattiva AutoPlay |   | Abilitata. Se abiliti questa impostazione dei criteri, AutoPlay viene disabilitato su CD-ROM e supporti rimovibili oppure viene disabilitato su tutte le unità. |
| *Contenuto cloud | Non mostrare i suggerimenti di Windows | Abilitata. Questa impostazione dei criteri impedisce la visualizzazione dei suggerimenti di Windows agli utenti. |
| *Contenuto cloud | Disattiva le esperienze per gli utenti consumer Microsoft | Abilitata. Se abiliti questa impostazione dei criteri, gli utenti non vedranno più i consigli personalizzati di Microsoft e le notifiche relative al loro account Microsoft. |
| *Raccolta dati e versioni di anteprima | Consenti telemetria | 0 - Sicurezza [solo enterprise] | Abilitata. L'impostazione di un valore 0 è applicabile solo ai dispositivi che eseguono le edizioni Enterprise, Education, IoT o Windows Server. |
| *Raccolta dati e versioni di anteprima | Non mostrare notifiche feedback |  | Enabled |
| *Raccolta dati e versioni di anteprima | Attiva/disattiva controllo utente per build Insider  |  | Disabled |
| Ottimizzazione recapito | Modalità download | Modalità download: Semplice (99) | 99 = Modalità di download semplice senza peering. Ottimizzazione recapito scarica solo tramite HTTP e non tenta di contattare i servizi cloud di Ottimizzazione recapito. |
| Gestione finestre desktop |  Non consentire chiamata Flip3D |  | Enabled |
| Gestione finestre desktop |  Non consentire animazioni delle finestre |  | Enabled |
| Gestione finestre desktop |  Usa tinta unita come sfondo di Start |  | Enabled |
| Interfaccia utente basata su bordi |  Consenti scorrimento rapido dai bordi |  | Disabled |
| Interfaccia utente basata su bordi |  Disabilita suggerimenti della Guida |  | Enabled |
| Interfaccia utente basata su bordi | Disattiva il rilevamento di utilizzo delle app |  | Enabled |
| *Esplora file |  Configura Windows Defender SmartScreen |  | Disattivato. SmartScreen verrà disabilitato per tutti gli utenti. Gli utenti non verranno avvertiti se provano a eseguire app sospette da Internet. NOTA: In assenza di connessione a Internet, questo impedirà ai computer di tentare di contattare Microsoft per ottenere informazioni su SmartScreen. |
| Esplora file |  Non visualizzare la notifica **nuova applicazione installata** |  | Enabled |
| *Trova il mio dispositivo |  Attivare/disattivare Trova il mio dispositivo |  | Disattivato. Quando Trova il mio dispositivo è disattivato, il dispositivo e la relativa posizione non vengono registrati e la funzionalità Trova il mio dispositivo non funziona. L'utente non sarà nemmeno in grado di visualizzare nel dispositivo la posizione dell'ultimo utilizzo del digitalizzatore attivo. |
| Esplora file | Disattiva la memorizzazione nella cache delle miniature |  | Enabled |
| Esplora file | Disattiva la visualizzazione di voci delle ricerca recenti nella casella di ricerca di Esplora File |  | Enabled |
| Esplora file | Disattiva la memorizzazione nella cache delle miniature nel file nascosto thumbs.db |  | Enabled |
| Esplora giochi | Disattiva il download delle informazioni sui giochi |  | Enabled |
| Esplora giochi | Disattiva gli aggiornamenti dei giochi |  | Enabled |
| Esplora giochi | Disattiva il tracciamento dell'ultimo tempo di gioco nella cartella Giochi |  | Enabled |
| Gruppo Home | Impedisci al computer di partecipare a un gruppo home |  | Enabled |
| *Internet Explorer | Consenti ai servizi Microsoft di fornire suggerimenti avanzati quando l'utente digita nella barra degli indirizzi |  | Disattivato. Gli utenti non riceveranno suggerimenti avanzati durante la digitazione nella barra degli indirizzi. Inoltre, non potranno modificare l'impostazione Suggerimenti. |
| Internet Explorer | Disabilita controllo periodico per aggiornamenti software di Internet Explorer |  | Enabled |
| Internet Explorer | Disabilita la visualizzazione della schermata iniziale |  | Enabled |
| Internet Explorer | Installa automaticamente nuove versioni di Internet Explorer |  | Disabled |
| Internet Explorer | Impedisci la partecipazione al programma Analisi utilizzo software |  | Enabled |
| Internet Explorer | Impedisci l'esecuzione guidata alla prima esecuzione | Passa direttamente alla home page | Enabled |
| Internet Explorer | Imposta la crescita del processo della scheda | Bassa | Enabled |
| Internet Explorer | Specificare il comportamento predefinito per una nuova scheda | Pagina Nuova scheda | Enabled |
| Internet Explorer | Disattiva le notifiche sulle prestazioni dei componenti aggiuntivi |  | Enabled |
| *Internet Explorer | Disattiva la funzionalità Completamento automatico per gli indirizzi Web |  | Abilitata. Se abiliti questa impostazione dei criteri, all'utente non verranno suggerite corrispondenze quando immette indirizzi Web. L'utente non può modificare l'opzione di completamento automatico per l'impostazione degli indirizzi Web. |
| *Internet Explorer | Disattiva georilevazione nel browser |  | Abilitata. Se abiliti questa impostazione dei criteri, il supporto alla georilevazione del browser è disabilitato. |
| *Internet Explorer | Disattiva Riapri ultima sessione di esplorazione |  | Enabled |
| Internet Explorer | Disattiva Riapri ultima sessione di esplorazione |  | Enabled |
| *Internet Explorer | Attiva Siti suggeriti |  | Disattivato. Se disabiliti questa impostazione dei criteri, le funzionalità e i punti di immissione associati a questa funzionalità sono disabilitati. |
| *Internet Explorer \\ Visualizzazione Compatibilità | Disattiva Visualizzazione Compatibilità |  | Abilitata. Se abiliti questa impostazione dei criteri, l'utente non può usare il pulsante Visualizzazione Compatibilità o gestire l'elenco dei siti di Visualizzazione Compatibilità. |
| *Internet Explorer \\ Pannello di controllo Internet \\ Scheda Avanzate | Riproduci animazioni in pagine Web |  | Disabled |
| *Internet Explorer \\ Pannello di controllo Internet \\ Scheda Avanzate | Riproduci video in pagine Web |  | Disabled |
| *Internet Explorer \\ Pannello di controllo Internet \\ Scheda Avanzate | Disattiva lo scorrimento in avanti con le funzionalità di previsione della pagina |  | Abilitata. Microsoft raccoglie la cronologia di esplorazione per migliorare lo scorrimento in avanti con la previsione della pagina. Questa funzionalità non è disponibile in Internet Explorer per il desktop. Se abiliti questa impostazione dei criteri, lo scorrimento in avanti con previsione della pagina è disabilitato e la pagina Web successiva non viene caricata in background. |
| Internet Explorer \\ Impostazioni Internet \\ Impostazioni avanzate \\ Esplorazione | Disattiva rilevamento numeri di telefono |  | Enabled |
| *Percorso e sensori | Disattiva posizione |  | Abilitata. Se abiliti questa impostazione dei criteri, la funzionalità di localizzazione viene disabilitata e tutti i programmi di questo computer non possono accedere alle informazioni rilevate dalla funzionalità di localizzazione. |
| Percorso e sensori | Disattiva sensori |  | Enabled |
| Percorso e sensori \\ Localizzatore geografico di Windows | Disattiva il Localizzatore geografico di Windows |  | Enabled |
| *Mappe | Disattiva il download e l'aggiornamento automatici dei dati delle mappe |  | Abilitata. Se abiliti questa impostazione, il download e l'aggiornamento automatici dei dati delle mappe vengono disabilitati. |
| *Mappe | Disattiva il traffico di rete non richiesto nella pagina di impostazioni Mappe offline |  | Abilitata. Se abiliti questa impostazione dei criteri, le funzionalità che generano traffico di rete nella pagina Impostazioni delle mappe offline vengono disabilitate. Nota: può essere disabilitata tutta la pagina delle impostazioni. |
| *Messaggistica | Consenti la sincronizzazione dei messaggi del servizio Cloud |  | Disattivato. Questa impostazione dei criteri consente il backup e ripristino di SMS dei telefoni cellulari nei servizi cloud Microsoft. |
| *Microsoft Edge | Consenti i suggerimenti dell'elenco a discesa della barra degli indirizzi |  | Disabled |
| *Microsoft Edge | Consentire gli aggiornamenti delle configurazioni per la raccolta della documentazione |  | Disattivato. Disabilita gli elenchi di compatibilità in Microsoft Edge. |
| *Microsoft Edge | Consenti elenco di compatibilità Microsoft |  | Disattivato. Se disabiliti questa impostazione, l'elenco di compatibilità Microsoft non viene usato durante l'esplorazione. |
| *Microsoft Edge | Consenti il contenuto Web nella pagina Nuova scheda |  | Disattivato. Determina l'apertura di Microsoft Edge con contenuto vuoto quando si apre una nuova scheda. |
| *Microsoft Edge | Configura riempimento automatico |  | Disattivato. Disabilita il riempimento automatico nella barra degli indirizzi. |
| *Microsoft Edge | Configura Do Not Track |  | Abilitata. Se abiliti questa impostazione, vengono sempre inviate richieste Do Not Track ai siti Web che richiedono informazioni di monitoraggio. |
| *Microsoft Edge | Configura lo strumento per la gestione delle password |  | Disattivato. Se disabiliti questa impostazione, i dipendenti non possono usare lo strumento per la gestione delle password per salvare le password in locale. |
| *Microsoft Edge | Configura i suggerimenti per la ricerca nella barra degli indirizzi |  | Disattivato. Gli utenti non possono vedere i suggerimenti per la ricerca nella barra degli indirizzi di Microsoft Edge. |
| *Microsoft Edge | Configura le pagine iniziali |  | Abilitata. Se abiliti questa impostazione, puoi configurare una o più pagine iniziali. Se questa impostazione è abilitata, devi anche includere gli URL delle pagine, separando più pagine con le parentesi acute in questo formato: <support.contoso.com><support.microsoft.com> Windows 10, versione 1703 o successiva. Se non vuoi inviare traffico a Microsoft, puoi usare il valore <about:blank>, che viene applicato per i dispositivi collegati o meno a un dominio quando è l'unico URL configurato. |
| *Microsoft Edge | Configura Windows Defender SmartScreen |  | Disattivato. Windows Defender SmartScreen viene disabilitato e i dipendenti non possono abilitarlo. NOTA: prendere in considerazione questa impostazione all'interno dell'ambiente. In assenza di connessione a Internet, questo impedirà ai computer di tentare di contattare Microsoft per ottenere informazioni su SmartScreen. |
| *Microsoft Edge | Impedisci l'apertura della pagina Web Primo avvio in Microsoft Edge |  | Abilitata. Gli utenti non vedranno la pagina Primo avvio alla prima apertura di Microsoft Edge. |
| OneDrive | Impedisci a OneDrive di generare traffico di rete finché l'utente non accede a OneDrive |  | Abilitata. Abilita questa impostazione per impedire al client di sincronizzazione OneDrive (OneDrive.exe) di generare traffico di rete (controllo degli aggiornamenti e così via) fino a quando l'utente non accede a OneDrive o inizia a sincronizzare i file nel computer locale. |
| *OneDrive | Impedisci uso di OneDrive per archiviazione file |  | Abilitata. A meno che OneDrive non sia usato in locale o non in locale. |
| OneDrive | Salva i documenti su OneDrive per impostazione predefinita |  | Disattivato. A meno che OneDrive non sia usato in locale o non in locale. |
| Feed RSS | Impedisci l'individuazione automatica di feed e Web Slice |  | Enabled |
|*Feed RSS | Disattiva la sincronizzazione in background per feed e Web Slice |  | Abilitata. Se abiliti questa impostazione dei criteri, la possibilità di sincronizzare feed e Web Slice in background viene disabilitata. |
|*Ricerca | Consenti Cortana |  | Disattivato. Quando Cortana è disattivata, gli utenti saranno comunque in grado di usare la ricerca per trovare dati nel dispositivo. |
|Ricerca | Consenti Cortana sulla schermata di blocco |   | Disabled |
|*Ricerca | Consenti l'uso della posizione per la ricerca e Cortana |  | Disabled |
|Ricerca | Non consentire la ricerca nel Web |   | Enabled |
|*Ricerca | Non cercare nel Web o visualizzare risultati Web per la ricerca |  | Abilitata. Se abiliti questa impostazione dei criteri, le query non verranno eseguite nel Web e i risultati Web non verranno visualizzati quando un utente esegue una query in Ricerca. |
|Ricerca | Impedisci l'aggiunta di posizioni UNC da indicizzare dal Pannello di controllo |  | Enabled |
|Ricerca | Impedisci l'indicizzazione di file nella cache di file offline |  | Enabled |
|*Ricerca | Imposta le informazioni condivise nella ricerca, Informazioni anonime |  | Abilitata. Condividi le informazioni sull'utilizzo ma non condividere la cronologia di ricerca, le informazioni sull'account Microsoft o la posizione specifica. |
|*Software Protection Platform | Disattivare la convalida AVC Online del Client Server di gestione delle chiavi | Abilitata. L'abilitazione di questa impostazione impedisce al computer di inviare dati a Microsoft relativi al proprio stato di attivazione. |
|*Riconoscimento vocale | Consenti aggiornamento automatico dei dati del riconoscimento vocale |  | Disattivato. Non controlla periodicamente se sono presenti modelli di riconoscimento vocale aggiornati. |
|*Archivio | Disattiva download e installazione automatici aggiornamenti |  | Abilitata. Se abiliti questa impostazione, il download e l'installazione automatici degli aggiornamenti delle app vengono disabilitati. |
|*Archivio | Disattiva download automatici di aggiornamenti su dispositivi Win8 | Abilitata. Se abiliti questa impostazione, il download automatico degli aggiornamenti delle app viene disabilitato. |
| Archivio | Disattiva l'offerta di aggiornamento all'ultima versione di Windows |  | Enabled |
|*Sincronizza le impostazioni | Non sincronizzare | Consenti agli utenti di attivare la sincronizzazione (non selezionato) | Abilitata. Se abiliti questa impostazione dei criteri, le opzioni di Sincronizza le impostazioni verranno disabilitate e nessuno dei gruppi di Sincronizza le impostazioni verrà sincronizzato nel dispositivo. |
| Input di testo | Migliora riconoscimento di input penna e digitazione |  | Disabled |
| Windows Defender Antivirus \\ MAPS | Partecipa alle Mappe Microsoft |  | Disattivato. Se disabiliti o non configuri questa impostazione, non potrai partecipare a Microsoft MAPS. |
| Windows Defender Antivirus \\ MAPS | Invia campioni di file quando è necessaria un'ulteriore analisi | Non inviare mai | Abilitata. Solo se non consentito esplicitamente per i dati di diagnostica di MAPS. |
| Windows Defender Antivirus \\ Segnalazione errori | Disattiva le notifiche avanzate |  | Abilitata. Se abiliti questa impostazione, le notifiche avanzate di Windows Defender Antivirus non verranno visualizzate nei client. |
| Windows Defender Antivirus \\ Aggiornamenti delle firme | Definisci l'ordine delle origini per il download degli aggiornamenti delle definizioni | FileShares | Abilitata. Se abiliti questa impostazione, le origini di aggiornamento delle definizioni verranno contattate nell'ordine specificato. Dopo il download degli aggiornamenti delle definizioni da un'origine specificata, le altre origini indicate nell'elenco non verranno contattate. |
| Segnalazione errori Windows | Invia automaticamente i file di backup della memoria per i report di errore generati dal sistema operativo |  | Disabled |
| Segnalazione errori Windows | Disattivare Segnalazione errori Windows |  | Enabled |
| Registrazione e trasmissione dei giochi di Windows | Abilita o disabilita la registrazione e la trasmissione di Giochi per Windows | | Disabled |
| Windows Installer | Dimensione massima del controllo della cache dei file di base | 5 | Enabled |
| Windows Installer | Disattiva la creazione di punti di controllo di ripristino configurazione di sistema |  | Enabled |
| Windows Mail | Disattiva la funzionalità di community |  | Enabled |
| Windows Media Player | Non visualizzare le finestre di dialogo del primo utilizzo |  | Enabled |
| Windows Media Player | Impedisci la condivisione dei file multimediali |  | Enabled |
| Centro PC portatile Windows | Disattiva Centro PC portatile Windows |  | Enabled |
| Analisi dell'affidabilità di Windows | Configura i provider WMI di affidabilità |  | Disabled |
| Windows Update | Consenti installazione immediata aggiornamenti automatici |  | Enabled |
| Windows Update | Non si connettono a qualsiasi percorso Internet di Windows Update |  | Abilitata. L'abilitazione di questo criterio disabilita tale funzionalità e potrebbe causare l'interruzione del funzionamento della connessione ai servizi pubblici come Windows Store. NOTA: Questo criterio si applica solo quando il dispositivo è configurato per connettersi a un servizio di aggiornamento di Intranet usando l'impostazione dei criteri "Specificare intranet aggiornamento percorso del servizio Microsoft". |
| Windows Update | Rimuovi l'accesso a tutte le funzionalità di Windows Update |   | Enabled |
| *Windows Update \\ Windows Update per le aziende | Gestisci le versioni di anteprima | Imposta il comportamento per la ricezione di versioni di anteprima: | Abilitata. Se selezioni Disattiva le versioni di anteprima, le versioni di anteprima non verranno installate nel dispositivo. Questo impedirà agli utenti di acconsentire esplicitamente al Programma Windows Insider attraverso Impostazioni -> Aggiornamento e sicurezza.<br>Disattivato. Disabilita le versioni di anteprima. |
| *Windows Update \\ Windows Update per le aziende | Seleziona il momento per la ricezione delle build di anteprima e degli aggiornamenti delle funzionalità | Canale semestrale<br>Rinvio: 365 giorni<br>Inizio sospensione: aaa-mm-gg. | Abilitata. Abilitare questo criterio per specificare il livello di versione di anteprima o di aggiornamenti delle funzionalità da ricevere e il momento in cui riceverli. |
| Windows Update \\ Windows Update per le aziende | Seleziona quando vengono ricevuti gli aggiornamenti qualitativi | 1. 30 giorni<br>2. Sospendi gli aggiornamenti qualitativi a partire dal aaa-mm-gg | Enabled |
| Impostazioni dei criteri personalizzati di traffico con restrizioni di Windows | Impedisci a OneDrive di generare traffico di rete finché l'utente non accede a OneDrive |  | Abilitata. Abilita questa impostazione se desideri impedire al client di sincronizzazione OneDrive (OneDrive.exe) di generare traffico di rete (controllo degli aggiornamenti e così via) fino a quando l'utente non accede a OneDrive o inizia a sincronizzare i file nel computer locale. |
| Impostazioni dei criteri personalizzati di traffico con restrizioni di Windows | Disattiva le notifiche Windows Defender |  | Abilitata. Se abiliti questa impostazione dei criteri, Windows Defender non invierà notifiche con informazioni critiche sull'integrità e sulla sicurezza del dispositivo. |
| Criteri del computer locale \\ Configurazione utente \\ Modelli amministrativi  |  |  |
|Panello di controllo \\ Opzioni internazionali e della lingua | Disattiva le proposte di completamento del testo durante la digitazione |  | Enabled |
| Desktop | Non aggiungere condivisioni dei documenti aperti recentemente ai percorsi di rete |  | Enabled |
| Desktop | Disattiva movimento del mouse per riduzione a icona finestre Aero Shake |  | Enabled |
| Desktop \\ Active Directory | Dimensioni massime delle ricerche di Active Directory | 2500 | Enabled |
| Menu Start e barra delle applicazioni | Non consentire l'aggiunta delle app dello Store alla barra delle applicazioni |  | Enabled |
| Menu Start e barra delle applicazioni | Non visualizzare o rilevare gli elementi nelle Jump List da posizioni remote |  | Enabled |
| Menu Start e barra delle applicazioni | Non usare il metodo basato sulla ricerca durante la risoluzione di collegamenti shell |  | Abilitata. Il sistema non esegue la ricerca di unità finale. Visualizza semplicemente un messaggio che indica che non è stato possibile trovare il file. |
| Menu Start e barra delle applicazioni | Rimuovi la barra Contatti dalla barra delle applicazioni |  | Abilitata. L'icona Contatti verrà rimossa dalla barra delle applicazioni, il pulsante delle impostazioni corrispondenti viene rimosso dalla pagina delle impostazioni della barra delle applicazioni e gli utenti non potranno aggiungere contatti alla barra delle applicazioni. |
| Menu Start e barra delle applicazioni | Disattiva le notifiche a fumetto degli annunci delle funzionalità |  | Abilitata. Gli utenti non possono aggiungere l'app dello Store alla barra delle applicazioni. Se l'app dello Store è già stata aggiunta alla barra delle applicazioni, verrà rimossa dalla stessa barra all'accesso successivo. |
| Menu Start e barra delle applicazioni | Disattiva il monitoraggio dell'utente |  | Enabled |
| Menu Start e barra delle applicazioni \\ Notifiche | Disattiva notifiche di tipo avviso popup |  | Enabled |
| Componenti di Windows \\ Contenuto cloud | Disattiva tutte le funzionalità di Contenuti in evidenza di Windows |  | Enabled |

### <a name="notes-about-network-connectivity-status-indicator"></a>Note sull'indicatore dello stato di connettività di rete

Le impostazioni del criterio di gruppo precedente includono le impostazioni da disattivare per verificare se il sistema è connesso a Internet. Se l'ambiente non si connette a Internet o si connette indirettamente, è possibile impostare un criterio di gruppo per rimuovere l'icona Rete dalla barra delle applicazioni. Il motivo per cui si potrebbe voler rimuovere l'icona Rete dalla barra delle applicazioni è rappresentato dal fatto che, se si disattivano i controlli di connettività Internet, sarà visibile una bandiera gialla sull'icona Rete, anche se la rete funziona normalmente. Se si desidera rimuovere l'icona della rete come un'impostazione del criterio di gruppo, è possibile scoprire che in questo percorso:

| Impostazione di criteri | Item | Elemento secondario | Impostazione possibile e commenti|
| -------------- | ---- | -------- | ---------------------------- |
| Windows Update o Windows Update per le aziende | Seleziona quando vengono ricevuti gli aggiornamenti qualitativi | 1. 30 giorni<br>2. Sospendi gli aggiornamenti qualitativi a partire dal aaa-mm-gg | Enabled |
| Criteri del computer locale \\ Configurazione utente \\ Modelli amministrativi |  |  |  |
| Menu Start e barra delle applicazioni | Rimuovi l'icona della rete |  | Abilitata. L'icona della rete non viene visualizzata nell'area di notifica del sistema. |

Per altre informazioni sull'indicatore di stato della connettività di rete (NCSI), vedi [Gestire gli endpoint di connessione per Windows 10 Enterprise, versione 1903](https://docs.microsoft.com/windows/privacy/manage-windows-1903-endpoints) e [Gestire le connessioni tra i componenti del sistema operativo Windows 10 e i servizi Microsoft](https://docs.microsoft.com/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services).

### <a name="system-services"></a>Servizi di sistema

Se stai prendendo in considerazione l'idea di disabilitare i servizi di sistema per risparmiare risorse, valuta attentamente se il servizio in questione è in qualche modo un componente di un altro servizio. Si noti che alcuni servizi non sono inclusi nell'elenco perché non possono essere disabilitati in maniera supportata.

Quasi tutte queste raccomandazioni rispecchiano quelle per Windows Server 2016, installato con Esperienza desktop in [Linee guida sulla sicurezza per la disabilitazione dei servizi in Windows Server 2016 con Esperienza desktop](https://docs.microsoft.com/windows-server/security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server).

Molti servizi che possono sembrare idonei alla disabilitazione sono impostati sul tipo di avvio manuale. Questo significa che il servizio non verrà avviato automaticamente e non viene avviato a meno che un processo o un evento non attivi una richiesta di disabilitazione del servizio. I servizi che sono già impostati per l'avvio manuale in genere non sono elencati di seguito.

> [!NOTE]
> Puoi enumerare i servizi in esecuzione con questo codice di esempio di PowerShell, generando come output solo il nome breve del servizio:

```powershell
 Get-Service | Where-Object {$_.Status -eq "Running"} | select -ExpandProperty Name
 ```

| Servizio Windows | Item | Commenti|
| -------------- | ---- | ---------------------------- |
| CDPUserService | Questo servizio utente è usato per gli scenari relativi alla piattaforma dispositivi connessi | Si tratta di un servizio per utente e, di conseguenza, il *servizio modello* deve essere disabilitato. |
| Esperienze utente connesse e telemetria | Abilita le funzionalità che supportano esperienze utente connesse e nell'applicazione. Questo servizio gestisce inoltre la raccolta e la trasmissione basate su eventi delle informazioni diagnostiche e sull'utilizzo (usate per migliorare l'esperienza e la qualità della piattaforma Windows) quando le opzioni di privacy per la diagnostica e l'utilizzo sono abilitate in Feedback e diagnostica. | È consigliabile la disabilitazione in caso di rete disconnessa. |
| Dati contatti | Indicizza i dati dei contatti per ricerche più rapide. Se il servizio viene arrestato o disabilitato, i contatti potrebbero non essere inclusi nei risultati di ricerca. | Si tratta di un servizio per utente e, di conseguenza, il *servizio modello* deve essere disabilitato. |
| Servizio Criteri di diagnostica | Consente di rilevare e risolvere i problemi relativi ai componenti di Windows. Se il servizio viene arrestato, le funzionalità di diagnostica verranno disattivate. | |
| Gestione mappe scaricate | Servizio di Windows per consentire alle applicazioni di accedere alle mappe scaricate. Il servizio viene avviato su richiesta dall'applicazione che accede alle mappe scaricate. La disabilitazione di questo servizio impedirà alle app di accedere alle mappe. | |
| Servizio di georilevazione | Consente di monitorare la posizione corrente del sistema e gestisce i recinti virtuali | |
| Servizio utente GameDVR e trasmissione | Questo servizio utente viene usato per le registrazioni e la trasmissione in tempo reale dei giochi | Si tratta di un servizio per utente e, di conseguenza, il servizio modello deve essere disabilitato. |
| MessagingService | Servizio che supporta la messaggistica di testo e la relativa funzionalità. | Si tratta di un servizio per utente e, di conseguenza, il *servizio modello* deve essere disabilitato. |
| Ottimizza unità | Consente al computer di funzionare in modo più efficiente ottimizzando i file nelle unità di archiviazione. | Le soluzioni VDI normalmente non traggono vantaggio dall'ottimizzazione del disco. Queste "unità" non sono unità tradizionali e spesso rappresentano solo un'allocazione di memorizzazione temporanea. |
| Ottimizzazione avvio | Mantiene e migliora nel tempo le prestazioni del sistema. | In generale non migliora le prestazioni della VDI, soprattutto se non persistente, dato che lo stato del sistema operativo viene rimosso a ogni riavvio. |
| Servizio tastiera virtuale e pannello per la grafia | Abilita le funzionalità di input penna della tastiera virtuale e del pannello per la grafia | |
| Segnalazione errori Windows | Consente di segnalare gli errori quando i programmi smettono di funzionare o si bloccano e di ricevere soluzioni. Consente inoltre di generare registri per i servizi di diagnostica e ripristino. Se il servizio viene arrestato, è possibile che la segnalazione errori non funzioni correttamente e che i risultati dei servizi di diagnostica e ripristino non vengano visualizzati. | Con la VDI, la diagnostica viene spesso eseguita in uno scenario offline e non nell'ambiente di produzione tradizionale. Inoltre, alcuni clienti disabilitano comunque la Segnalazione errori Windows (WER). Segnalazione errori Windows (WER) comporta l'uso di una quantità minima di risorse per molti elementi diversi, tra cui la mancata installazione di un dispositivo o di un aggiornamento. |
| Servizio di condivisione in rete Windows Media Player | Condivide le librerie di Windows Media Player con altri lettori in rete e dispositivi multimediali che usano Universal Plug and Play | Non è necessario, a meno che i clienti non condividano le librerie WMP sulla rete. |
| Servizio hotspot di Windows Mobile | Consente la condivisione della connessione alla rete dati con un altro dispositivo. | |
| Windows Search | Fornisce indicizzazione di contenuto, memorizzazione di proprietà nella cache e risultati di ricerca per file, posta elettronica e altro contenuto.                                                                    | Probabilmente non è necessario in particolare con la VDI non persistente |

#### <a name="per-user-services-in-windows"></a>Servizi per utente in Windows

I servizi per utente sono servizi creati quando un utente accede a Windows o Windows Server e arrestati ed eliminati quando l'utente si disconnette. Questi servizi vengono eseguiti nel contesto di protezione dell'account utente. Ciò offre una migliore gestione delle risorse rispetto al precedente approccio di esecuzione di questo tipo di servizi in Explorer, associati a un account preconfigurato, o come attività.

[Servizi per utente in Windows 10 e Windows Server](https://docs.microsoft.com/windows/application-management/per-user-services-in-windows)

Se si intende modificare un valore di avvio del servizio, il metodo preferito consiste nell'aprire un prompt dei comandi con privilegi elevati ed eseguire lo strumento Gestione controllo servizi Sc.exe. Per altre informazioni sull'uso di Sc.exe, vedi [Sc](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc754599(v=ws.11)).

### <a name="scheduled-tasks"></a>Attività pianificate

Come altri elementi in Windows, verifica che un elemento non sia necessario prima di valutare la possibilità di disabilitarlo.

Nell'elenco di attività seguente sono riportate quelle che eseguono ottimizzazioni o raccolte di dati su computer che mantengono lo stato al riavvio. Quando un'attività di macchina virtuale VDI si riavvia e rimuove tutte le modifiche apportate dall'ultimo avvio, le ottimizzazioni destinate ai computer fisici non sono utili.

Puoi ottenere tutte le attività pianificate correnti, incluse le descrizioni, con il codice seguente di PowerShell:

```powershell
 Get-ScheduledTask | Select-Object -Property TaskPath,TaskName,State,Description
```

>[!NOTE]
> Esistono diverse attività che non possono essere disabilitate tramite script, anche stai operando con privilegi elevati. Consigliamo di non disabilitare le attività che non possono essere disabilitate tramite uno script.

Nome dell'attività pianificata:

- Cellulare
- Consolidator
- Diagnostica
- FamilySafetyMonitor
- FamilySafetyRefreshTask
- MaintenanceTasks
- MapsToastTask
- Compatibilità
- Microsoft-Windows-DiskDiagnosticDataCollector
- MNO
- NotificationTask
- PerformRemediation
- ProactiveScan
- ProcessMemoryDiagnosticEvents
- ProgramDataUpdater
- Proxy
- QueueReporting
- RecommendedTroubleshootingScanner
- ReconcileFeatures
- ReconcileLanguageResources
- RefreshCache
- RegIdleBackup
- ResPriStaticDbSync
- RunFullMemoryDiagnostic
- ScanForUpdates
- ScanForUpdatesAsUser
- Pianificate
- ScheduledDefrag
- sihpostreboot
- SilentCleanup
- SmartRetry
- SpaceAgentTask
- SpaceManagerTask
- SpeechModelDownloadTask
- Sqm-Tasks
- SR
- StartComponentCleanup
- StartupAppTask
- StorageSense
- SyspartRepair
- Sysprep
- UninstallDeviceTask
- UpdateLibrary
- UpdateModelTask
- UsbCeip
- Usb-Notifications
- USO_UxBroker
- WiFi
- WIM-Hash-Management
- WindowsActionDialog
- Strumento Valutazione sistema Windows
- Cartelle
- WsSwapAssessmentTask
- XblGameSaveTask

### <a name="apply-windows-and-other-updates"></a>Applica aggiornamenti di Windows e di altro tipo

Sia da Microsoft Update che da risorse interne, applica gli aggiornamenti disponibili, incluse le firme di Windows Defender. Questo è il momento adatto per applicare altri aggiornamenti disponibili, compresi Microsoft Office, se installato, e altri aggiornamenti software. Se PowerShell rimarrà nell'immagine, potrai scaricare la Guida più recente disponibile per PowerShell eseguendo il comando [Update-Help](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/update-help?view=powershell-7).

#### <a name="servicing-the-operating-system-and-apps"></a>Manutenzione del sistema operativo e delle app

A un certo punto durante il processo di ottimizzazione dell'immagine devono essere applicati gli aggiornamenti di Windows disponibili. Nelle impostazioni di aggiornamento di Windows 10 è presente un'impostazione che può fornire aggiornamenti aggiuntivi:

![Aggiornamenti aggiuntivi](media/rds-vdi-recommendations-1909/servicing.png)

Questa operazione è un'impostazione valida nel caso in cui si desidera installare le applicazioni Microsoft, ad esempio Microsoft Office, per l'immagine di base. In questo modo Office è aggiornato quando l'immagine viene messa in servizio. Esistono anche alcuni aggiornamenti di .NET e componenti di terze parti, ad esempio Adobe, che mettono a disposizione gli aggiornamenti tramite Windows Update.

Una considerazione molto importante per le macchine virtuali VDI non persistenti sono gli aggiornamenti di sicurezza, compresi i file di definizione del software di sicurezza. Questi aggiornamenti possono essere rilasciati una sola volta o più volte al giorno. Può esserci un modo per mantenere questi aggiornamenti, compresi Windows Defender e i componenti di terze parti.

Per Windows Defender è consigliabile consentire l'esecuzione degli aggiornamenti, anche su VDI non persistenti. Gli aggiornamenti verranno applicati quasi a ogni sessione di accesso, ma sono piccoli e non dovrebbero rappresentare un problema. Inoltre, la macchina virtuale non rimarrà indietro con gli aggiornamenti perché verranno applicati solo gli aggiornamenti disponibili più recenti. Lo stesso può essere applicato ai file di definizione di terze parti.

> [!NOTE]
> Le app dello Store (app UWP) si aggiornano tramite Windows Store. Le versioni moderne di Office, come Office 365, si aggiornano attraverso i propri meccanismi quando sono direttamente collegate a Internet, o tramite tecnologie di gestione quando non lo sono.

### <a name="windows-system-startup-event-traces"></a>Tracce degli eventi di avvio del sistema Windows

Windows è configurato, per impostazione predefinita, per raccogliere e salvare dati diagnostici limitati. Lo scopo è quello di abilitare la diagnostica oppure registrare i dati nel caso in cui siano necessarie ulteriori attività di risoluzione dei problemi. Le tracce automatiche di sistema possono essere trovate nella posizione indicata nella figura seguente:

![Tracce di sistema](media/rds-vdi-recommendations-1909/system-traces.png)

Alcune delle tracce visualizzate in **Sessioni di traccia eventi** e **Sessioni di traccia eventi di avvio** non possono e non devono essere arrestate. Altre, ad esempio la traccia WiFiSession, possono essere arrestate. Per arrestare una traccia in esecuzione in **Sessione di traccia eventi**, fai clic con il pulsante destro del mouse sulla traccia e quindi scegli Arresta. Per evitare che le tracce vengano avviate automaticamente all'avvio, attieniti a questa procedura:

1. Fai clic sulla cartella **Sessioni di traccia eventi di avvio**.

2. Individuare la traccia di interesse e quindi fare doppio clic su tale traccia.

3. Fai clic sulla scheda **Sessione di traccia**.

4. Fai clic sulla casella con etichetta **Abilitato** per rimuovere il segno di spunta.

5. Fare clic su **OK**.

Di seguito sono riportate alcune tracce di sistema di cui considerare la disabilitazione per l'uso della VDI:

| Nome                    | Commento                       |
| ----------------------- | ----------------------------- |
| AppModel | Una raccolta di tracce, una delle quali è il telefono |
| CloudExperienceHostOOBE | |
| DiagLog | |
| NtfsLog | |
| TileStore | |
| UBPM | |
| WiFiDriverIHVSession | Se non si usa un dispositivo Wi-Fi |
| WiFiSession | |
| WinPhoneCritical | |

### <a name="windows-defender-optimization-with-vdi"></a>Ottimizzazione di Windows Defender con VDI

Microsoft ha pubblicato di recente la documentazione relativa a Windows Defender in un ambiente VDI. Per altre informazioni, vedi la [Guida alla distribuzione per Windows Defender Antivirus in un ambiente Virtual Desktop Infrastructure (VDI)](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus).

L'articolo sopra indicato contiene le procedure per la manutenzione dell'immagine VDI gold e per la gestione dei client VDI in esecuzione. Per ridurre la larghezza di banda di rete quando i computer VDI devono aggiornare le firme di Windows Defender, riavviare il sistema e pianificare i riavvii durante le ore di inattività, se possibile. Gli aggiornamenti delle firme di Windows Defender possono essere contenuti internamente sulle condivisioni di file e, dove possibile, contenere tali file condivisi sugli stessi segmenti di rete o su segmenti di rete vicini come le macchine virtuali VDI.

### <a name="client-network-performance-tuning-by-registry-settings"></a>Ottimizzazione delle prestazioni della rete client tramite le impostazioni del Registro di sistema

Esistono alcune impostazioni del Registro di sistema che possono migliorare le prestazioni di rete. Questo aspetto è importante soprattutto negli ambienti in cui la VDI o il computer fisico ha un carico di lavoro basato principalmente sulla rete. Le impostazioni riportate in questa sezione sono consigliate per influire sulle prestazioni di rete, con la configurazione di buffer e cache aggiuntivi per elementi come le voci di directory.

>[!NOTE]
> Alcune impostazioni di questa sezione sono solo basate sul Registro di sistema e devono essere incorporate nell'immagine di base prima che questa venga distribuita per l'uso nell'ambiente di produzione.

Le impostazioni seguenti sono documentate in [Linee guida di ottimizzazione delle prestazioni per Windows Server 2016](https://docs.microsoft.com/windows-server/administration/performance-tuning/), pubblicate in Microsoft.com dal gruppo di prodotti Windows.

#### <a name="disablebandwidththrottling"></a>DisableBandwidthThrottling

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableBandwidthThrottling`

Si applica a Windows 10. Il valore predefinito è **0**. Per impostazione predefinita, il redirector SMB limita la velocità effettiva nelle connessioni di rete ad alta latenza, in alcuni casi per evitare timeout relativi alla rete. Se si imposta su 1 questo valore del Registro di sistema, si disabilita tale limitazione, rendendo possibile una velocità effettiva di trasferimento file superiore su connessioni di rete ad alta latenza. È consigliabile impostare questo valore su **1**.

#### <a name="fileinfocacheentriesmax"></a>FileInfoCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheEntriesMax` Si applica a Windows 10. Il valore predefinito è **64** e l'intervallo valido è compreso tra 1 e 65536. Questo valore viene usato per determinare la quantità di metadati dei file che può essere memorizzata nella cache dal client. Aumentando il valore, è possibile ridurre il traffico di rete e migliorare le prestazioni quando si accede a diversi file. Provare ad aumentare questo valore a **1024**.

#### <a name="directorycacheentriesmax"></a>DirectoryCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntriesMax`

Si applica a Windows 10. Il valore predefinito è **16** e l'intervallo valido è compreso tra 1 e 4096. Questo valore viene usato per determinare la quantità di informazioni delle directory che può essere memorizzata nella cache dal client. Aumentando il valore, è possibile ridurre il traffico di rete e migliorare le prestazioni quando si accede a directory di grandi dimensioni. Considerare la possibilità di aumentare questo valore a **1024**.

#### <a name="filenotfoundcacheentriesmax"></a>FileNotFoundCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheEntriesMax`

Si applica a Windows 10. Il valore predefinito è **128** e l'intervallo valido è compreso tra 1 e 65536. Questo valore viene usato per determinare la quantità di informazioni relative ai nomi dei file che può essere memorizzata nella cache dal client. Aumentando il valore, è possibile ridurre il traffico di rete e migliorare le prestazioni quando si accede a diversi nomi di file. Considerare la possibilità di aumentare questo valore a **2048**.

#### <a name="dormantfilelimit"></a>DormantFileLimit

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantFileLimit`

Si applica a Windows 10. Il valore predefinito è **1023**. Questo parametro specifica il numero massimo di file che è consigliabile lasciare aperti in una risorsa condivisa dopo che l'applicazione ha chiuso il file. Quando migliaia di client si connettono a server SMB, è consigliabile ridurre questo valore a **256**.

È possibile configurare molte di queste impostazioni SMB usando i cmdlet [Set-SmbClientConfiguration](https://docs.microsoft.com/powershell/module/smbshare/set-smbclientconfiguration?view=win10-ps) e [Set-SmbServerConfiguration](https://docs.microsoft.com/powershell/module/smbshare/set-smbserverconfiguration?view=win10-ps) di Windows PowerShell. È possibile configurare le impostazioni solo basate sul Registro di sistema anche usando Windows PowerShell, come illustrato nell'esempio seguente:

```powershell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecuritySignature -Value 0 -Force
```

Impostazioni aggiuntive tratte dalle linee guida relative alla baseline con funzionalità limitata di traffico con restrizioni di Windows - Microsoft ha rilasciato una baseline, creata con le stesse procedure delle [baseline di sicurezza di Windows](https://docs.microsoft.com/powershell/module/smbshare/set-smbserverconfiguration?view=win10-ps), per ambienti che non sono connessi direttamente a Internet o che puntano a ridurre i dati inviati a Microsoft e ad altri servizi.

Le impostazioni della [baseline con funzionalità limitata di traffico con restrizioni di Windows](https://docs.microsoft.com/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services) sono contrassegnate con un asterisco nella tabella dei criteri di gruppo.

#### <a name="disk-cleanup-including-using-the-disk-cleanup-wizard"></a>Pulizia del disco (compreso l'uso della procedura guidata Pulizia disco)

La pulizia del disco può essere particolarmente utile con le implementazioni VDI dell'immagine gold o master. Dopo che l'immagine è stata preparata, aggiornata e configurata, una delle ultime attività da eseguire è la pulizia del disco. È disponibile uno strumento integrato denominato Pulizia disco che consente di pulire la maggior parte delle potenziali aree di risparmio di spazio su disco. In una macchina virtuale con pochi elementi installati ma con tutte le patch applicate è in genere possibile ottenere circa 4 GB di spazio libero su disco con l'esecuzione di Pulizia disco.

Ecco alcuni suggerimenti per varie attività di pulizia del disco. Tutte queste attività devono essere testate prima dell'implementazione:

1. Eseguire la procedura guidata Pulizia disco (con privilegi elevati) dopo aver applicato tutti gli aggiornamenti. Includere le categorie Ottimizzazione recapito e Pulizia Windows Update. Questo processo può essere automatizzato usando la riga di comando `Cleanmgr.exe` con l'opzione `/SAGESET:11`. L'opzione `/SAGESET` imposta i valori del Registro di sistema che possono essere usati successivamente per automatizzare la pulizia del disco, usando tutte le opzioni disponibili nella procedura guidata Pulizia disco.

    1. In una macchina virtuale di test, da un'installazione pulita, l'esecuzione di `Cleanmgr.exe /SAGESET:11` rivela che per impostazione predefinita sono abilitate solo due opzioni di pulizia automatica del disco:
    
        - File di programma scaricati

        - File temporanei di Internet

    2. Se si impostano altre opzioni, o tutte le opzioni, queste opzioni vengono registrate nel Registro di sistema, secondo il valore di **Index** fornito nel comando precedente (`Cleanmgr.exe /SAGESET:11`). In questo caso, useremo il valore `11` come indice, per una successiva procedura automatica di pulizia del disco.

    3. Dopo aver eseguito `Cleanmgr.exe /SAGESET:11`, vedrai diverse categorie di opzioni di pulizia del disco. Puoi controllare ogni opzione e quindi fare clic su **OK**. La procedura Pulizia disco scompare e le impostazioni vengono salvate nel Registro di sistema.

2. Pulire la memoria della copia shadow del volume, se in uso.

    - Aprire un prompt dei comandi con privilegi elevati ed eseguire prima il comando `vssadmin list shadows` e quindi il comando `vssadmin list shadowstorage`.
    
        Se l'output di questi comandi è **Impossibile trovare elementi che soddisfano la query**, non è in uso alcuna memoria VSS.

3. Pulire i file temporanei e i registri. Al prompt dei comandi con privilegi elevati eseguire il comando `Del C:\*.tmp /s`, il comando `Del C:\Windows\Temp\.` e il comando `Del %temp%\.`.

4. Eliminare tutti i profili non utilizzati nel sistema eseguendo `wmic path win32_UserProfile where LocalPath="c:\users\<user>" Delete`.

### <a name="remove-onedrive-components"></a>Rimuovere i componenti di OneDrive

La rimozione di OneDrive implica la rimozione del pacchetto, la disinstallazione e la rimozione dei file *.lnk. Il codice di esempio seguente di PowerShell può essere usato per semplificare la rimozione di OneDrive dall'immagine ed è incluso negli script di ottimizzazione di VDI di GitHub:

```azurecli

Taskkill.exe /F /IM "OneDrive.exe"
Taskkill.exe /F /IM "Explorer.exe"` 
    if (Test-Path "C:\\Windows\\System32\\OneDriveSetup.exe")`
     { Start-Process "C:\\Windows\\System32\\OneDriveSetup.exe"`
         -ArgumentList "/uninstall"`
         -Wait }
    if (Test-Path "C:\\Windows\\SysWOW64\\OneDriveSetup.exe")`
     { Start-Process "C:\\Windows\\SysWOW64\\OneDriveSetup.exe"`
         -ArgumentList "/uninstall"`
         -Wait }
Remove-Item -Path
"C:\\Windows\\ServiceProfiles\\LocalService\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\OneDrive.lnk" -Force
Remove-Item -Path "C:\\Windows\\ServiceProfiles\\NetworkService\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\OneDrive.lnk" -Force \# Remove the automatic start item for OneDrive from the default user profile registry hive
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Load HKLM\\Temp C:\\Users\\Default\\NTUSER.DAT" -Wait
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Delete HKLM\\Temp\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Run /v OneDriveSetup /f" -Wait
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Unload HKLM\\Temp" -Wait Start-Process -FilePath C:\\Windows\\Explorer.exe -Wait
```

Per qualsiasi domanda o dubbio sulle informazioni contenute nel presente documento, contattare il team del proprio account Microsoft, eseguire ricerche nel blog Microsoft VDI, inviare un messaggio ai forum Microsoft o contattare Microsoft per domande o dubbi.

## <a name="turn-windows-update-back-on"></a>Riattivare Windows Update

Se desideri riattivare Windows Update, come nel caso di VDI persistente, attieniti alla procedura seguente:

- Abilita di nuovo queste impostazioni dei criteri di gruppo:

    - Criteri del computer locale \\ Configurazione computer \\ Modelli amministrativi \\ Sistema \\ Gestione comunicazioni Internet \\ Impostazioni di comunicazione Internet

        - Disabilita l'accesso a tutte le funzionalità di Windows Update (passa da **Abilitato** a **Non configurato**).

    - Criteri del computer locale \\ Configurazione computer \\ Modelli amministrativi \\ Componenti di Windows \\ Windows Update

        - Rimuovi l'accesso a tutte le funzionalità di Windows Update (modifica da **Abilitato** a **Non configurato**).

        - Non connetterti ad alcuna posizione Internet di Windows Update (modifica da **Abilitato** a **Non configurato**).

    - Criteri del computer locale \\ Configurazione computer \\ Modelli amministrativi \\ Componenti di Windows \\ Windows Update \\ Windows Update per le aziende

        - Seleziona la ricezione degli aggiornamenti qualitativi (modifica da Abilitato a Non configurato).

    -   Criteri del computer locale \\ Configurazione computer \\ Modelli amministrativi \\ Componenti di Windows \\ Windows Update \\ Windows Update per le aziende

        - Seleziona la ricezione delle versioni di anteprima e degli aggiornamenti di funzionalità (modifica da **Abilitato** a **Non configurato**).

-  Riattiva i servizi

    - Aggiorna il servizio Orchestrator (modifica da **Disabilitato** ad **Automatico (avvio ritardato)** ).

    - Modifica le impostazioni seguenti del Registro di sistema di Windows:

        - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\PolicyState

            - DeferQualityUpdates (modifica da **1** a **0**)

        - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings

            - PausedQualityDate (elimina qualsiasi valore esistente)

        - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer\WAU

            - Disabled

-  Riattiva le attività pianificate

    - Libreria Utilità di pianificazione \\ Microsoft \\ Windows \\ InstallService\\ ScanForUpdates

    - Libreria Utilità di pianificazione \\ Microsoft \\ Windows \\ InstallService \\ ScanForUpdatesAsUser

Per rendere effettive tutte queste impostazioni, riavvia il dispositivo. Se non vuoi che questo dispositivo offra aggiornamenti delle funzionalità, passa a Impostazioni \\ Windows Update \\ Opzioni avanzate \\ Scegli quando installare gli aggiornamenti e quindi imposta manualmente l'opzione **Un aggiornamento delle funzionalità include funzionalità e miglioramenti nuovi. Può essere rimandato per il numero di giorni seguente: un valore diverso da zero, ad esempio 180, 365 e così via.**

### <a name="references"></a>Riferimenti

- [What is VDI (virtual desktop infrastructure)?](https://www.citrix.com/glossary/vdi.html) (Che cos'è VDI (Virtual Desktop Infrastructure)?)

- [Sysprep non riesce dopo aver rimosso o aggiornato le app di Microsoft Store che includono immagini Windows incorporate](https://support.microsoft.com/help/2769827/sysprep-fails-after-you-remove-or-update-windows-store-apps-that-inclu)
