---
title: Configurazione consigliata per desktop VDI
description: Le impostazioni e configurazione per ridurre il sovraccarico per desktop di Windows 10 1607 (10.0.1393) utilizzato come immagini VDI consigliati
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 12/18/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a44dc9f-c221-4bf7-89c3-fb4c86a90f8c
author: jaimeo
manager: dougkim
ms.openlocfilehash: 24704373dedf6a44809b83f3df17bd073cee2bf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818382"
---
# <a name="recommended-settings-for-vdi-desktops"></a>Impostazioni consigliate per i desktop VDI

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

Microsoft Desktop Virtualization rileva automaticamente le configurazioni dei dispositivi e le condizioni della rete per ottenere gli utenti sia operativa prima abilitando l'installazione immediata di desktop e applicazioni aziendali e offre IT per fornire l'accesso a legacy applicazioni durante la migrazione a Windows 10.

Anche se il sistema operativo Windows 10 è molto bene ottimizzato impostazione predefinita, esistono opportunità di ridefinirla ulteriormente in modo specifico per l'ambiente aziendale di Microsoft Virtual Desktop Infrastructure (VDI). Nell'ambiente VDI, molti servizi in background e le attività sono disabilitate, a partire dall'inizio.

In questo argomento non è un progetto, ma piuttosto una Guida o un punto di partenza. Alcuni suggerimenti potrebbero disabilitare la funzionalità che si preferisce usare, è quindi consigliabile valutare il rapporto tra costi e il vantaggio di qualsiasi impostazione particolare nello scenario corrente.

Queste istruzioni e le impostazioni consigliate sono rilevanti per Windows 10 1607 (versione 10.0.1393).

> [!NOTE]  
> Eventuali impostazioni non espressamente specificate in questo argomento è possibile lasciare i valori predefiniti (o set per i requisiti e criteri) senza impatto apprezzabile sulle funzionalità VDI.

Quando si crea un'immagine su cui basare la distribuzione di VDI, assicurarsi di usare la **Current Branch**. Per altre informazioni su Current Branch, vedere [informazioni sulla versione di Windows 10](https://technet.microsoft.com/windows/release-info.aspx).

## <a name="creating-the-windows-10-image"></a>Creazione dell'immagine di Windows 10
Il primo passaggio è installare un'immagine di riferimento di Windows 10 1607 (versione 10.0.1393) in una macchina virtuale o fisica. Installazione di una macchina virtuale è semplice e consente di salvare le versioni del file (VHD) del disco rigido virtuale, nel caso in cui si desidera eseguire il rollback a una versione precedente.

Durante l'installazione, è possibile scegliere **impostazioni rapide** oppure **Personalizza**. Le impostazioni disponibili durante la **Personalizza** opzione possono essere impostati su mediante criteri di gruppo, in modo che il metodo di installazione sistema operativo di base non è importante.


Se si è scelto **Personalizza**, è possibile modificare queste impostazioni durante l'installazione:

## <a name="in-customize-settings"></a>In "Personalizza le impostazioni"

È inoltre possibile regolare queste dopo l'installazione con criteri di gruppo; vedere la sezione "Impostazioni di criteri di gruppo" di questo argomento.

|Impostazione|Valore predefinito|Valore consigliato per l'uso di un'infrastruttura VDI|  
|-------------------|----------|--------------|
|**Personalizzazione**| | |
|Personalizzare i comandi vocali, la tipizzazione e input penna di input, inviare i dati di input a Microsoft.|    Attivato| Disattivato|
|Inviare la digitazione e input penna di dati a Microsoft per migliorare la piattaforma di riconoscimento e suggerimenti.|  Attivato| Disattivato|
|Consentire le app di usare il proprio ID pubblicità per un'esperienza tra app.|  Attivato| Disattivato|
|Lasciare che consentono di Skype (se installato) connettersi con gli amici nella rubrica dell'utente e verificare il numero di cellulare. Potrebbero essere applicate tariffe SMS e i dati.|    Attivato| Disattivato|
|**Posizione**| | |
|Attivare la ricerca mio dispositivo e "Let" Windows e le app nel percorso, tra cui cronologia delle posizioni delle richieste| Attivato| Disattivato|
|Connettività e la segnalazione errori| | |
|Connettiti automaticamente agli hotspot aperti suggeriti. Non tutte le reti sono sicure.|    Attivato| Disattivato|
|Connettersi automaticamente a hotspot temporaneamente per verificare se a pagamento sono disponibili servizi di rete.| Attivato| Disattivato|
|Inviare dati di utilizzo e diagnostica completa a Microsoft. Disattivando questa opzione Invia solo i dati di base.| Attivato| Disattivato|
|**Browser, protezione e aggiornamenti**| | |
|Usare SmartScreen online services per proteggere il contenuto dannoso e Scarica nella siti caricati dall'App Store e i browser di Windows|    Attivato| In (se è presente alcun accesso a Internet, quindi impostato su Off).
|Usare la previsione della pagina per migliorare la lettura, velocizzare l'esplorazione e migliorare l'esperienza complessiva nei browser di Windows. L'esplorazione dei dati vengono inviati a Microsoft.| Attivato| Disattivato|
|Ottenere aggiornamenti e invia aggiornamenti in altri computer nella rete Internet per velocizzare l'app e i download di Windows Update|   Attivato| Disattivato|

Al termine dell'installazione, è possibile continuare la modifica delle impostazioni a partire **delle impostazioni di Windows**.

## <a name="in-windows-settings"></a>Nelle impostazioni di Windows
Per accedere alle impostazioni di Windows, fare clic su **avviare** (l'icona di Windows della barra delle applicazioni), quindi fare clic sui **impostazioni** icona (la forma di ingranaggio).

### <a name="in-the-system-area-of-windows-settings"></a>Nell'area "Sistema" delle impostazioni di Windows
Nell'area delle impostazioni di Windows, fare clic sui **sistema** icona consente di accedere a una serie di impostazioni relative al sistema. Non tutte è necessario modificare per l'uso ottimale di VDI, queste impostazioni sono quelle più importanti:

#### <a name="apps-and-features"></a>App e funzionalità

Per rimuovere un'app, in tal modo viene escluso dall'immagine VDI, fare clic sull'app e quindi fare clic su **Disinstalla**. Se **Disinstalla** è disabilitata, sarà possibile rimuoverlo da questo metodo; potrebbe essere in grado di rimuoverlo con Windows PowerShell oppure provare a eseguire questi passaggi:
1. Fare clic su **Gestisci funzionalità facoltative** (immediatamente sotto il **App e funzionalità** intestazione della pagina stessa).
2. Scegliere la funzionalità facoltativa, quindi **Disinstalla**.

Le funzionalità per provare a rimuovere (se presente) includono quanto segue:
- **Contattare il supporto tecnico**
- **Inglese (Stati Uniti) delle vendite al dettaglio Demo contenuto**
- **Contenuto di Demo delle vendite al dettaglio neutro**
- **Fornire assistenza rapida**

#### <a name="default-apps"></a>App predefinite

Quest'area consente di definire l'app da usare per impostazione predefinita per determinate funzioni generiche, ad esempio posta elettronica, esplorazione del web e mappe. Se si desidera un'altra app da usare per una particolare funzione, fare clic sulla voce corrente e quindi fare clic sull'app desiderato da utilizzare nell'immagine VDI. Per un'app di Microsoft non sia disponibile un'opzione, è necessario installare l'app prima di modificare questa impostazione.

#### <a name="notifications-and-actions"></a>Notifiche e azioni

Questi valori consigliati ridurrà le notifiche e attività di rete in background in un ambiente VDI:

|Impostazione|Valore predefinito|Valore consigliato per l'uso di un'infrastruttura VDI|  
|-------------------|----------|--------------|
|Ottenere notifiche dalle App e altri mittenti| Attivato| Disattivato|
|Mostra le notifiche nella schermata di blocco.|    Attivato| Disattivato|
|Mostra gli allarmi, dei promemoria e chiamate VoIP in ingresso nella schermata di blocco.|   Attivato| Disattivato|
|Mostrare suggerimenti, trucchi e suggerimenti quando si utilizza Windows.|    Attivato| Disattivato|


#### <a name="offline-maps"></a>Mappe offline

Questa impostazione è disponibile solo se è installata l'app esegue il mapping. Il valore predefinito è **sul**; per l'uso di un'infrastruttura VDI è il valore consigliato **Off**. 

#### <a name="tablet-mode"></a>Modalità tablet

|Impostazione|Valore predefinito|Valore consigliato per l'uso di un'infrastruttura VDI|  
|-------------------|----------|--------------|
|Quando accede|    Usare la modalità appropriata per l'hardware in uso|   Usare la modalità desktop|
|Quando il dispositivo passa automaticamente in modalità attiva o disattiva|    Chiedi sempre prima del passaggio| Non visualizzare più e non passare|
|Nascondi le icone delle app sulla barra delle applicazioni in modalità tablet|  Attivato| Disattivato|


### <a name="in-the-devices-area-of-windows-settings"></a>Nell'area "Dispositivi" delle impostazioni di Windows
Nell'area delle impostazioni di Windows, fare clic sui **dispositivi** icona consente di accedere a una serie di impostazioni relative al sistema. Non tutte è necessario modificare per l'uso ottimale di VDI, queste impostazioni sono quelle più importanti:

#### <a name="autoplay"></a>Autoplay

|Impostazione|Valore predefinito|Valore consigliato per l'uso di un'infrastruttura VDI|  
|-------------------|----------|--------------|
|Utilizzare la funzionalità Autoplay per tutti i dispositivi e supporti|    Attivato| Disattivato|
|Unità rimovibile:|Scegliere un valore predefinito|Non eseguire alcuna azione|
|Scheda di memoria|Scegliere un valore predefinito|Non eseguire alcuna azione|

### <a name="in-the-personalization-area-of-windows-settings"></a>Nell'area "Personalizzazione" delle impostazioni di Windows
Nell'area delle impostazioni di Windows, fare clic sui **personalizzazione** icona consente di accedere a una serie di impostazioni relative al sistema. Non tutte è necessario modificare per l'uso ottimale di VDI, queste impostazioni sono quelle più importanti:

#### <a name="background"></a>Informazioni
In alcuni casi, lo sfondo nero predefinito potrebbe agli utenti di ritiene che il computer non risponde. Modificare il colore di sfondo può risultare utile renderlo più chiaro. A tale scopo, effettuare le operazioni seguenti:
1. Nel **sfondo** area, fare clic sul menu a discesa.
2. Per modificare il colore di sfondo, fare clic su **colore a tinta unita**, quindi fare clic su uno qualsiasi dei colori diversi da nero. In alternativa, è possibile fare clic su **Picture** e quindi selezionare un'immagine da utilizzare come sfondo.

#### <a name="start"></a>Inizio

|Impostazione|Valore predefinito|Valore consigliato per l'uso di un'infrastruttura VDI|  
|-------------------|----------|--------------|
|In alcuni casi Mostra suggerimenti nel menu Start|    Attivato| Disattivato|
|App Mostra più usato|Attivato|Disattivato|
|Visualizzare le app aggiunte di recente|Attivato|Disattivato|
|Mostra elementi aperti di recente nelle Jump List su inizio o alla barra delle applicazioni|Attivato|Disattivato|

#### <a name="taskbar"></a>Barra delle applicazioni
L'impostazione predefinita consiste nell'usare i pulsanti di grandi dimensioni sulla barra delle applicazioni (ovvero, un valore di "Off" per **usare i pulsanti della barra delle applicazioni piccole**). Questa impostazione fa in modo che l'elemento di Cortana per l'utilizzo di molte dell'area della barra delle applicazioni. Per evitare questo problema, impostare **usare i pulsanti della barra delle applicazioni piccole** su "Sì". Se si preferisce che gli elementi della barra delle applicazioni rimangono più grandi, ma non desiderano avere Cortana occupano molto spazio, fare clic sulla barra delle applicazioni, scegliere **Cortana**e nel menu quindi l'uscita, selezionare **Hidden**.

### <a name="in-the-privacy-area-of-windows-settings"></a>Nell'area "Privacy" delle impostazioni di Windows
Nell'area delle impostazioni di Windows, fare clic sui **Privacy** icona consente di accedere a una serie di impostazioni relative al sistema. Non tutte è necessario modificare per l'uso ottimale di VDI, queste impostazioni sono quelle più importanti:

#### <a name="general"></a>Generale
Alcune di queste impostazioni vengono impostate anche nella finestra del "Personalizza le impostazioni" descritto all'inizio di questo argomento.

|Impostazione|Valore predefinito|Valore consigliato per l'uso di un'infrastruttura VDI|  
|-------------------|----------|--------------|
|Consentire le app di usare il mio ID pubblicità per esperienze tra App (disabilitazione di questa impostazione verrà reimpostato il proprio ID)|  Attivato| Disattivato|
|Permetti ai siti Web di accedere all'elenco delle lingue per fornire contenuti pertinenti per la mia specifica area geografica|Attivato|Disattivato|
|Consentire le app su altri dispositivi aprire App personali e continuare a esperienze su questo dispositivo|Attivato|Disattivato|

#### <a name="camera"></a>Fotocamera

Il valore predefinito per "Consentire le app usare la fotocamera digitale" viene **sul**; per l'uso di un'infrastruttura VDI è il valore consigliato **Off**.


#### <a name="microphone"></a>Microfono

Il valore predefinito per "Consentire le app usare il microfono" viene **sul**; per l'uso di un'infrastruttura VDI è il valore consigliato **Off**.

#### <a name="notifications"></a>Notifiche

Il valore predefinito per "Consentire app di accedere a notifiche personali" viene **sul**; per l'uso di un'infrastruttura VDI è il valore consigliato **Off**.

#### <a name="contacts"></a>Contatti

Il valore predefinito per "Consentire app di accedere ai contatti personali" viene **sul**; per l'uso di un'infrastruttura VDI è il valore consigliato **Off**.

#### <a name="calendar"></a>Calendar

Il valore predefinito per "Consentire le app accedere al calendario personale" viene **sul**; per l'uso di un'infrastruttura VDI è il valore consigliato **Off**.

#### <a name="call-history"></a>Registro chiamate

Il valore predefinito per "Consentire le app accedere al registro chiamate" viene **sul**; per l'uso di un'infrastruttura VDI è il valore consigliato **Off**.

#### <a name="email"></a>Posta elettronica

Il valore predefinito per "Consentire l'accesso delle App e inviare email" è **sul**; per l'uso di un'infrastruttura VDI è il valore consigliato **Off**.

#### <a name="messaging"></a>Messaggistica

Il valore predefinito per "Consentire l'app di leggere o inviare messaggi (SMS o MMS)" viene **sul**; per l'uso di un'infrastruttura VDI è il valore consigliato **Off**.

#### <a name="radios"></a>Radio

Il valore predefinito per "Consentire le app controllo radio" viene **sul**; per l'uso di un'infrastruttura VDI è il valore consigliato **Off**.

#### <a name="other-devices"></a>Altri dispositivi

Il valore predefinito di "Let le tue App automaticamente condividere e sincronizzare le informazioni con dispositivi wireless non associati esplicitamente con i PC, tablet o telefono" viene **sul**; per l'uso di un'infrastruttura VDI è il valore consigliato **Off**.

#### <a name="feedback-and-diagnostics"></a>Feedback e diagnostica

Il valore predefinito per "Windows deve richiedere i miei commenti" è **automaticamente**; per l'uso di un'infrastruttura VDI, il valore consigliato è **Never**.

#### <a name="background-apps"></a>App in background
Le app elencate hanno un valore predefinito di **su**, che consente loro di ricevere informazioni, inviare notifiche e aggiornano automaticamente se sono venga utilizzate o meno. È necessario disattivare (impostare su **Off**) tutte le altre App non si desidera che in esecuzione in background nell'immagine VDI.

### <a name="update-and-security"></a>Aggiornamento e sicurezza
#### <a name="windows-update"></a>Windows Update
Nel **aggiornare le impostazioni** area, fare clic su **opzioni avanzate** regolare queste impostazioni:

|Impostazione|Valore predefinito|Valore consigliato per l'uso di un'infrastruttura VDI|  
|-------------------|----------|--------------|
|Scarica gli aggiornamenti per altri prodotti Microsoft durante l'aggiornamento di Windows|    Deselezionata|    selezionato|
|Rinvia gli aggiornamenti delle funzionalità|Deselezionata|selezionato|
|Usa le mie informazioni di accesso per completare automaticamente la configurazione del dispositivo dopo un aggiornamento |Deselezionata|Dipende dalla configurazione specifica di VDI|

Nel **Advanced options** pagina, fare clic su **scegliere la modalità di recapito degli aggiornamenti** per accedere all'impostazione di "Aggiornamenti da più di un'unica posizione." Il valore predefinito è **sul**; per l'uso di un'infrastruttura VDI è il valore consigliato **Off**.

## <a name="in-control-panel-and-other-system-utilities"></a>Nel Pannello di controllo e altre utilità di sistema

Le impostazioni in questa sezione sono regolabili spostarsi nel Pannello di controllo o aprendo direttamente l'utilità.

> [!NOTE]  
> Eventuali impostazioni non espressamente specificate in questo argomento è possibile lasciare i valori predefiniti (o set per i requisiti e criteri) senza impatto apprezzabile sulle funzionalità VDI.


### <a name="task-scheduler"></a>Utilità di pianificazione
Il modo più rapido per aprire Utilità di pianificazione è eseguire il push sul pulsante Windows e digitare *utilità di pianificazione* oppure *Taskschd*. Nei risultati che restituiscono, fare clic su **utilità di pianificazione** per aprire l'utilità. Nell'utilità di pianificazione, espandere **libreria utilità di pianificazione**, espandere **Microsoft**, quindi espandere **Windows**. È ora possibile accedere all'elenco delle raccolte di attività. Per modificare lo stato di ogni attività pianificata, pulsante destro del mouse e quindi fare clic su stato desiderato (in genere **disabilitato** per l'uso di un'infrastruttura VDI).

|Raccolta di attività|Nome attività|Stato predefinito|Stato consigliato per l'uso di un'infrastruttura VDI|  
|-------------------|-------------|----------|--------------|
|Analisi utilizzo software||||
||Consolidamento|Enabled|Disabled|
||KernelCeipTask|Enabled|Disabled|
||UsbCeip|Enabled|Disabled|
|Deframmentazione||||
||ScheduledDefrag|Enabled|Disabled|
|Località||||
||Notifiche|Enabled|Disabled|
||WindowsActionDialog|Enabled|Disabled|
|Manutenzione||||
||WinSAT|Enabled|Disabled|
|Mappe||||
||MapsToastTask|Enabled|Disabled|
||MapsUpdateTask|Enabled|Disabled|
|Account a banda larga mobili||||
||Parser dei metadati MNO|Enabled|Disabled|
|Diagnostica di efficienza di risparmio energia||||
||Analisi sistema|Enabled|Disabled|
|Ambiente di ripristino||||
||VerifyWinRE|Enabled|Disabled|
|Demo delle vendite al dettaglio||||
||CleanupOfflineContent|Enabled|Disabled|
|Shell||||
||FamilySafetyMonitor|Enabled|Disabled|
||FamilySafetyRefreshTask|Enabled|Disabled|
|Segnalazione errori Windows||||
||QueueReporting|Enabled|Disabled|
|Windows Media Sharing||||
||UpdateLibrary|Enabled|Disabled|

Fare clic su **Windows** nuovamente per comprimere, quindi fare clic su **XblGameSave**. Ciò consente di accedere alle attività **XBLGameSaveTask** e **XBLGameSaveTaskLogon**; entrambi questi può essere impostati su **disabilitato**.

### <a name="performance-monitor"></a>Performance Monitor
Il modo più rapido per aprire Performance Monitor è eseguire il push sul pulsante Windows e digitare *monitoraggio delle prestazioni* oppure *Perfmon. msc*. Nei risultati che restituiscono, fare clic su **Performance Monitor**. In Performance Monitor, fare clic su **insiemi agenti di raccolta dati** e quindi fare doppio clic su **sessioni di traccia eventi**. Fare doppio clic su **WiFiSession**; se si trova in stato predefinito del **che esegue**, quindi fare clic su **arrestare**.

Fare clic su **StartupEventTraceSessions**, quindi fare doppio clic su **ReadyBoot**; se è in esecuzione, fare clic su **arrestare**. Fare clic su **sessioni di traccia eventi**, fare doppio clic su **ReadyBoot**, quindi fare clic su **proprietà**. Nella finestra di dialogo visualizzata, scegliere il **sessione di traccia** scheda. Cancella il **abilitato** casella di controllo.

### <a name="services"></a>Servizi
Il modo più rapido per gestire i servizi è eseguire il push sul pulsante Windows e digitare *services*. Nei risultati che restituiscono, fare clic su **Services**. I servizi seguenti sono buoni candidati per disabilitare l'uso in scenari VDI. Tuttavia, potrebbe essere necessario eseguire alcuni test per verificare che essi non sono necessari per gli scopi desiderati. Per disabilitare un servizio, nelle **Services** snap-in, fare clic sul nome del servizio e quindi fare clic su **proprietà**. Nel **generali** scheda, fare clic sui **tipo di avvio** menu a discesa e quindi fare clic su **disabilitato**. Fare clic su **OK**.

- BranchCache
- Ottimizzazione recapito
- Host del servizio diagnostica
- Servizio Hotspot Mobile di Windows
- Xbox Live Auth Manager
- Salva gioco con Xbox Live
- Servizio rete di Xbox Live

### <a name="file-explorer-options"></a>Opzioni di Esplora file
Eseguire il push sul pulsante Windows e digitare *Pannello di controllo*. Nei risultati che restituiscono, fare clic su **Pannello di controllo**. Nel Pannello di controllo, fare clic su **opzioni di Esplora File**. Nella finestra di dialogo visualizzata, fare clic sui **ricerca** scheda e quindi nel **durante la ricerca dei percorsi non indicizzate** area, deselezionare il casella di controllo **includono le directory di sistema**. Fare clic su **OK** per salvare.

### <a name="flash-settings"></a>Impostazioni di memoria flash
Eseguire il push sul pulsante Windows e digitare *Pannello di controllo*. Nei risultati che restituiscono, fare clic su **Pannello di controllo**. Nel Pannello di controllo, fare clic su **Flash Player** aprire Flash Player Settings Manager. Nel **memorizzazione** scheda, selezionare il pulsante di opzione per **Blocca tutti i siti di archiviare le informazioni in questo computer**. Nella finestra di dialogo visualizzata, fare clic su **OK**.

Nel **fotocamera e microfono** nella scheda il **fotocamera e microfono impostazioni** area, selezionare il pulsante di opzione per **bloccare tutti i siti di usare la fotocamera e microfono**.

Nel **riproduzione** nella scheda il **Peer assistita rete** area, selezionare il pulsante di opzione per **bloccare tutti i siti dall'uso della rete di peer assistita**. Chiudere Flash Player Settings Manager.

### <a name="internet-options"></a>Opzioni Internet
Eseguire il push sul pulsante Windows e digitare *Pannello di controllo*. Nei risultati che restituiscono, fare clic su **Pannello di controllo**. Nel Pannello di controllo, fare clic su **Opzioni Internet** per aprire le proprietà Internet. Nel **homepage** area, immettere l'URL per il sito web si desidera che gli utenti di visualizzare la home page nel browser. Potrebbe trattarsi di un sito web della società o è possibile impostarlo su una pagina iniziale vuota immettendo *sulle: vuoto*.

Nel **cronologia esplorazioni** area, selezionare la casella di controllo **Elimina la cronologia delle esplorazioni alla chiusura del programma**.

### <a name="power-options"></a>Opzioni risparmio energia
Eseguire il push sul pulsante Windows e digitare *Pannello di controllo*. Nei risultati che restituiscono, fare clic su **Pannello di controllo**. Nel Pannello di controllo, fare clic su **opzioni di risparmio energia** per aprire il pannello di controllo Opzioni di risparmio energia. Nel **scegliere o personalizzare un piano di risparmio energia** area, fare clic sulla freccia verso il basso per **mostrano i piani aggiuntivi**e quindi selezionare il pulsante di opzione per **ad alte prestazioni**. Questa impostazione avrà un impatto minimo sull'host VDI.

### <a name="system"></a>Sistema
Eseguire il push sul pulsante Windows e digitare *Pannello di controllo*. Nei risultati che restituiscono, fare clic su **Pannello di controllo**. Nel Pannello di controllo, fare clic su **sistema** per aprire il pannello di controllo di sistema. Nel riquadro sinistro, fare clic su **impostazioni di sistema avanzate**. Nella finestra di dialogo visualizzata, scegliere il **avanzate** scheda. Nel **prestazioni** area, fare clic sui **impostazioni** pulsante, quindi nella **effetti visivi** scheda nella finestra di dialogo che si apre selezionare il **Adjust per ottimizzare le prestazioni**  pulsante di opzione. Fare clic su **OK** per salvare e uscire.

## <a name="group-policy-settings"></a>Impostazioni di Criteri di gruppo

Per modificare le impostazioni di criteri di gruppo, premere il pulsante Windows e il tipo *criteri di gruppo* oppure *gpedit. msc*. Nei risultati che restituiscono, fare clic su **Modifica criteri di gruppo** per aprire Editor criteri di gruppo locali.

> [!NOTE]  
> Eventuali impostazioni non espressamente specificate in questo argomento è possibile lasciare i valori predefiniti (o set per i requisiti e criteri) senza impatto apprezzabile sulle funzionalità VDI.

Sotto **configurazione Computer**, espandere **le impostazioni di Windows**, quindi espandere **impostazioni di sicurezza**. Fare clic su **Criteri gestione elenco reti**, quindi fare doppio clic su **tutte le reti**. Nella finestra di dialogo che viene aperta, il **percorso di rete** area, selezionare il pulsante di opzione per **utente non è possibile cambiare percorso**. Scegliere il **OK** consente di salvare.

Comprimi **delle impostazioni di Windows**, quindi espandere **modelli amministrativi**. Fare clic o espandere **Network**e quindi modificare ogni impostazione come indicato di seguito facendo doppio clic su esso, quindi selezionando il pulsante di opzione per il valore indicato e facendo clic la **OK** pulsante:

|Area per le impostazioni|Impostazione|Valore consigliato per l'uso di un'infrastruttura VDI|  
|-------------------|-------|----------|
|Servizio trasferimento intelligente in background (BITS)|||
||Non consentire al client BITS utilizza Windows BranchCache|Enabled|
||Non consentire al computer di agire come un client peer caching BITS|Enabled|
||Non consentire al computer di agire come server peer caching BITS|Enabled|
||Consenti peer caching BITS|Disabled|
|BranchCache||
||Attivare BranchCache|Disabled|
|Autenticazione di hotspot||
||Abilitare l'autenticazione di Hotspot|Disabled|
|Servizi di rete di Peer-to-Peer Microsoft||
||Disattivare i servizi Microsoft Peer-to-Peer Networking|Enabled|
|File offline||
||Consentire o non consentire Usa la funzionalità file Offline|Disabled|

Comprimi **Network**, quindi espandere **sistema**. Modificare ogni impostazione come indicato di seguito facendo doppio clic su esso, quindi selezionando il pulsante di opzione per il valore indicato e facendo clic la **OK** pulsante:

|Area per le impostazioni|Impostazione|Valore consigliato per l'uso di un'infrastruttura VDI|  
|-------------------|----------|--------------|
|Installazione dei dispositivi||
||Non inviare una segnalazione di errore di Windows quando è installato un driver generico in un dispositivo|Enabled|
||Impedire la creazione di un punto di ripristino di sistema durante l'attività del dispositivo che viene in genere viene richiesta la creazione di un punto di ripristino|Enabled|
||Evitare il recupero dei metadati di dispositivo da Internet|Enabled|
||Impedire l'invio di un report di errore quando un software aggiuntivo le richieste dei driver di dispositivo durante l'installazione di Windows|Enabled|
||Disattivare le bolle "Nuovo Hardware" durante l'installazione del dispositivo|Enabled|

Espandere **Filesystem**, fare doppio clic su **NTFS**, fare doppio clic su **opzioni di creazione nomi brevi**, selezionare il pulsante di opzione per **abilitato**, e quindi usare il **le opzioni** menu a discesa per selezionare **Abilita in tutti i volumi**. Scegliere il **OK** consente di salvare.

Comprimi **Filesystem**, quindi espandere **Gestione comunicazioni Internet**. Fare clic su **impostazioni di comunicazione Internet**. Modificare ogni impostazione come indicato di seguito, facendovi doppio clic e quindi scegliere il pulsante di opzione per **Enabled**e quindi scegliendo il **OK** pulsante:

- Disattiva collegamenti di "Events. asp"
- Disattiva condivisione dati Personalizzazione riconoscimento grafia
- Disattivare la segnalazione di errori di riconoscimento della grafia
- Disattiva Guida e supporto tecnico "Non tutti sanno che" content
- Disattiva ricerca della Guida e supporto tecnico Microsoft Knowledge Base
- Disattiva Connessione guidata Internet se connessione a URL rimanda a Microsoft.com
- Disattiva download Internet per la pubblicazione e ordinazione guidata online via Web
- Disattiva servizio Internet associazioni File
- Disattiva registrazione se connessione a URL rimanda a Microsoft.com
- Disattiva l'operazione immagine "Ordina stampe"
- Disattiva l'attività "Pubblica sul Web" per i file e cartelle
- Disattiva analisi utilizzo software Windows Messenger
- Disattiva analisi utilizzo software Windows
- Disattiva segnalazione errori Windows
- Disattiva ricerca driver dispositivi in Windows Update

Fare clic su **risparmio energia** e quindi fare doppio clic su **selezionare un piano di risparmio energia attivi**. Selezionare il pulsante di opzione per **Enabled**, quindi utilizzare il **opzioni** menu a discesa per selezionare **ad alte prestazioni**. Scegliere il **OK** consente di salvare.

Fare clic su **Recovery**, quindi fare doppio clic su **Consenti ripristino del sistema allo stato predefinito**. Selezionare il pulsante di opzione per **Enabled**, quindi fare clic sui **OK** consente di salvare.

Espandere **risoluzione dei problemi e diagnostica**. Fare clic su **manutenzione pianificata**, fare doppio clic su **configurare il comportamento di manutenzione pianificata**, quindi selezionare il pulsante di opzione per **disabilitato**. Scegliere il **OK** consente di salvare.

Per ognuna delle seguenti aree impostazioni, selezionarlo, quindi fare doppio clic su **configurare a livello di esecuzione di uno Scenario**, selezionare il pulsante di opzione per **disabilitato**, quindi fare clic su di **OK**consente di salvare:

- Diagnostica prestazioni avvio Windows
- Diagnostica delle perdite di memoria Windows
- Risoluzione e rilevamento/Risoluzione esaurimento risorse Windows
- Diagnostica prestazioni arresto di Windows
- Diagnostica prestazioni Standby o riprendere l'esecuzione di Windows
- Diagnostica delle prestazioni della velocità di risposta del sistema di Windows

Comprimi **System**, quindi espandere **componenti Windows**. Modificare ogni impostazione come indicato di seguito facendo doppio clic su esso, quindi selezionando il pulsante di opzione per il valore indicato e facendo clic la **OK** pulsante:

|Area per le impostazioni|Impostazione|Valore consigliato per l'uso di un'infrastruttura VDI|  
|-------------------|-------|----------|
|Aggiungere funzionalità a Windows 10|||
||Impedire l'esecuzione la procedura guidata|Enabled|
|Criteri di AutoPlay|||
||Impostare il comportamento predefinito per l'esecuzione automatica|Abilitato, quindi usare il **le opzioni** menu a discesa per selezionare **non eseguire i comandi di esecuzione automatica**|
|Contenuto cloud|||
||Non mostrare suggerimenti di Windows|Enabled|
||Disattiva le esperienze per gli utenti consumer Microsoft|Enabled|
|Raccolta dei dati e build di anteprima|||
||Consenti telemetria|Abilitato, quindi usare il **le opzioni** menu a discesa per selezionare **1-di base**|
||Disabilita funzionalità o impostazioni non definitive|     Disabled|
||Non mostrare notifiche feedback|       Enabled|
||Attiva/disattiva controllo utente per build Insider|      Disabled|
|Gestione finestre desktop|||
||Non consentire Flip3D chiamata|       Enabled|
||Non consentire animazioni delle finestre|       Enabled|
||Usare tinta unita come sfondo iniziale|     Enabled|
|Bordo interfaccia utente|||
||Consente di scorrere verso il bordo|     Disabled|
||Disattivare i suggerimenti della Guida|        Enabled|
|Esplora file|||
||Non visualizzare la notifica 'nuova applicazione installato'|     Enabled|
|Esplora giochi|||
||Disattiva download di sui giochi|     Enabled|
||Disattivare gli aggiornamenti del gioco|        Enabled|
||Disattivare il rilevamento dell'ultimo play dei giochi nella cartella di giochi|     Enabled|
|Gruppo Home|||
||Impedisci al computer di partecipare a un gruppo home|        Enabled|
|Internet Explorer|||
||Consenti ai servizi Microsoft di fornire suggerimenti avanzati quando l'utente digita nella barra degli indirizzi|        Disabled|
||Disabilita controllo periodico per aggiornamenti software di Internet Explorer|        Enabled|
||Disabilita la visualizzazione della schermata|        Enabled|
||Installa automaticamente nuove versioni di Internet Explorer|      Disabled|
||Evitare che il programma Analisi utilizzo software di partecipazione|     Enabled|
||Impedire l'esecuzione primo avvio guidato di passare direttamente alla home page|   Abilitato, quindi usare il **le opzioni** menu a discesa per selezionare **passare direttamente alla home page**|
||Impostare la crescita di scheda processo|Abilitato, quindi digitare il comando seguente nel **scheda processo crescita** casella: *Bassa*.|
||Specificare il comportamento predefinito per una nuova scheda|Abilitato, quindi usare il **le opzioni** menu a discesa per selezionare **nuova pagina della scheda**|
||Disattiva le notifiche sulle prestazioni dei componenti aggiuntivi|        Enabled|
||Disattiva georilevazione nel browser|     Enabled|
||Disattivare Riapri ultima sessione di esplorazione|        Enabled|
||Disattivare i suggerimenti per tutti i provider installati dall'utente|        Enabled|
||Attivare siti suggeriti|       Disabled|

Allo stesso livello di **Internet Explorer** le impostazioni sono state modificate solo nella tabella precedente, notare un ulteriore livello di cartelle compreso tra **acceleratori** per **barre degli strumenti**. In altre parole, trovano ora a criteri del Computer locale > Configurazione Computer > modelli amministrativi > componenti di Windows > Internet Explorer. 

Aprire il **Elimina cronologia esplorazioni** cartella, fare doppio clic su **consente l'eliminazione della cronologia di esplorazione all'uscita**, selezionare **abilitare**e quindi fare clic su **OK**per salvare e uscire.

Utilizzare la freccia indietro nella parte superiore sinistra locale Editor criteri di gruppo per tornare indietro per la **Internet Explorer** livello. Fare doppio clic su **impostazioni Internet**, fare doppio clic su **impostazioni avanzate**, quindi regolare le impostazioni nelle sottocartelle nel modo seguente:

|Cartella impostazione **impostazioni avanzate**|Impostazione|Valore consigliato per l'uso di un'infrastruttura VDI|  
|-------------------|-------|----------|
|**Esplorazione**|||
||Disattiva rilevamento numeri di telefono|Enabled|
|**Funzionalità multimediali**|||
||Consentire a Internet Explorer per riprodurre file multimediali che usano i codec alternativi|Disabled|

Vai indietro fino al livello **Internet Explorer**, quindi fare doppio clic su **impostazioni Internet**. In questa cartella, impostare queste due impostazioni sotto **AutoComplete** al **abilitato**:

- Disattiva Suggerimenti - URL
- Disattivare la funzionalità Completamento automatico di Windows Search

Passare a eseguire il backup dei quattro livelli di **i componenti di Windows**, fare doppio clic su **posizione e sensori**e quindi impostare queste tre impostazioni **abilitato** (per ognuna, fare clic su  **OK** per salvare e chiudere):

- Disattivare la località
- Disattivare l'opzione Percorso script
- La disattivazione dei sensori

Periodo di tempo a livello di **posizione e sensori**, fare doppio clic su **Provider di posizione Windows** e impostare **disattivare il Provider di posizione Windows** a **abilitato**. Fare clic su **OK** per salvare e uscire.

Nel riquadro sinistro, fare clic su **mappe**, configurare queste impostazioni **Enabled**; per ciascuno, quindi fare clic su **OK** per salvare e uscire:

- Disattiva il download e l'aggiornamento automatici dei dati delle mappe
- Disattiva il traffico di rete non richiesto nella pagina di impostazioni Mappe offline

Usando il riquadro a sinistra, immettere tutte le sottocartelle delle impostazioni seguenti e modificare le singole impostazioni come indicato di seguito:

|Cartella di impostazioni sotto **i componenti di Windows**|Impostazione|Valore consigliato per l'uso di un'infrastruttura VDI|  
|-------------------|-------|----------|
|**OneDrive**|||
||Impedisci uso di OneDrive per archiviazione file|Enabled|
||Salvare i documenti di OneDrive per impostazione predefinita|Disabled|
|**Feed RSS**|||
||Evitare l'individuazione automatica di feed e le Web Slice|Enabled|
|**Ricerca**|||
||Consenti Cortana|        Disabled|
||Consenti a Cortana schermata di blocco|      Disabled|
||Consenti l'uso della posizione per la ricerca e Cortana|     Disabled|
||Non consentire la ricerca nel Web|      Enabled|
||Non eseguire la ricerca sul web o visualizzare i risultati web nella ricerca|        Enabled|
||Evitare l'aggiunta di UNC percorsi da indicizzare dal Pannello di controllo|     Enabled|
||Impedire l'indicizzazione di file nella cache di file offline|        Enabled|
|**Store**|||
||Disattivare l'offerta di aggiornamento alla versione più recente di Windows|Enabled|
|**Segnalazione errori Windows**|||
||Invia automaticamente i dump di memoria per i report di errore generati dal sistema operativo|       Disabled|
||Disattivare Segnalazione errori Windows|      Enabled|
|**Windows Installer**|||
||Dimensioni massime del controllo della cache dei file baseline|  Abilitato, quindi usare la casella di selezione nel **opzioni** un'area da impostare **dimensioni massime della cache del file Baseline** al *5*.|
||Disattivare la creazione di checkpoint di ripristino configurazione di sistema|      Enabled|
|**Posta elettronica di Windows**|||
||Disattivare la funzionalità di community|Enabled|
|**Windows Media Player**|||
||Non visualizzare più finestre di dialogo primo utilizzo|       Enabled|
||Evitare di condivisione di file multimediali|        Enabled|
|**Windows Mobility Center**|||
||Disattivare Centro PC portatile Windows|Enabled|
|**Analisi di affidabilità di Windows**|||
||Configurare i provider WMI di affidabilità|Disabled|
|**Windows Update**|||
||Consenti installazione immediata aggiornamenti automatici|       Enabled|
||Rimuovere l'accesso a tutte le funzionalità di Windows Update|     Enabled|
|Nel **Windows Update** cartella, aprire **Rinvia aggiornamento di Windows**|||
||Selezionare quando vengono ricevuti gli aggiornamenti delle funzionalità|Abilitato, quindi nella **le opzioni** area, usare il **selezionare il livello di conformità di branch per gli aggiornamenti delle funzionalità che si desidera ricevere** menu a discesa per selezionare **Current Branch for Business**. Impostare il **dopo il rilascio di un aggiornamento delle funzionalità, posticipare la ricezione per questo numero di giorni** alla casella di selezione *180 giorni*.
||Selezionare quando vengono ricevuti gli aggiornamenti qualitativi|Abilitato, quindi nella **le opzioni** area, impostare il **dopo il rilascio di un aggiornamento di qualità, posticipare la ricezione per questo numero di giorni** casella di selezione per *30 giorni* e selezionare la casella di controllo **Sospendere gli aggiornamenti qualitativi**.

Nel riquadro sinistro dell'Editor criteri di gruppo locali, fare clic su **configurazione utente**. Utilizzo del riquadro a sinistra, fare clic su **modelli amministrativi** e quindi immettere tutte le sottocartelle delle impostazioni seguenti e modificare le singole impostazioni come indicato di seguito:

|Cartella di impostazioni sotto **modelli amministrativi**|Impostazione|Valore consigliato per l'uso di un'infrastruttura VDI|  
|-------------------|-------|----------|
|**Desktop**|||
||Non aggiungere condivisioni dei documenti aperti recentemente ai percorsi di rete|Enabled|
|Nel **Desktop** cartella, aprire **Active Directory**|||
||Dimensioni massime di ricerche in Active Directory|Abilitato, quindi nella **opzioni** area, usare la casella di selezione per impostare **numero di oggetti restituiti** a *5000*.|
|**Manu Start e barra delle applicazioni**|||
||Cancellare l'elenco di programmi recenti per i nuovi utenti|     Enabled|
||Non visualizzare o rilevare gli elementi nelle Jump List da posizioni remote|        Enabled|
||Disattiva fumetti notifica di annuncio funzionalità|     Enabled|
||Disattivare il rilevamento delle azioni utente|       Enabled|
|Nel **dal Menu Start e barra delle applicazioni** cartella, aprire **notifiche**|||
||Disattiva notifiche di tipo avviso popup|Enabled|
|Nel **i componenti di Windows** cartella aperta:|||
|**Contenuto cloud**|||
||Disattiva tutte le funzionalità di Contenuti in evidenza di Windows|Enabled|
|**Esplora file**|||
||Disattivare la memorizzazione nella cache delle immagini di anteprima|       Enabled|
||Disattivare la visualizzazione di voci di ricerca recenti nella casella di ricerca di Esplora File|        Enabled|
||Disattivare la memorizzazione nella cache delle anteprime nel file Thumbs nascosti|      Enabled|

## <a name="microsoft-store-apps"></a>App di Microsoft Store
Esistono una serie di App di Microsoft Store che si potrebbe voler rimuovere dall'immagine VDI; la loro rimozione sarà ridurre l'utilizzo della CPU e risparmiare spazio su disco. Di seguito sono riportati i buoni candidati per la rimozione:

- Ottieni Office
- Skype (anteprima)
- Iniziare a usare (in particolare se non è presente nessuna connessione Internet)
- Hub di Feedback
- Raccolta di Solitario Microsoft
- A pagamento Wi-Fi e cellulare

Per personalizzare il profilo utente predefinito usato per la creazione di immagini VDI, usare l'account Administrator predefinito. Se non è già abilitato, è possibile farlo tramite utenti e gruppi locali in Gestione Computer. Quindi accedere all'account di amministratore per completare la procedura seguente.

> [!NOTE]  
> Non rimuovere le app di sistema, ad esempio l'app Store. Sono difficili da reinstallare. Altre App è facilmente reinstallable compresi la Store.

### <a name="delete-unwanted-apps-from-the-administrator-user-profile"></a>Elimina App indesiderate dal profilo dell'utente amministratore
1. In Windows PowerShell, eseguire `Get-AppxPackage | ft PackageFamilyName` per visualizzare l'elenco delle App installate.
2. Per ogni packager di app che si desidera disinstallare eseguire i cmdlet di questo formato di esempio:

    `Get-AppxPackage *messaging* | Remove-AppxPackage`

    `Get-AppxPackage *WindowsMaps* | Remove-AppxPackage`

    `Get-AppxPackage *ZuneMusic* | Remove-AppxPackage`

### <a name="delete-the-payload-of-unwanted-store-apps"></a>Eliminare il payload dell'App Store indesiderate
Ciò impedirà l'App venga reinstallato.
1. App Store di elenco e altri elementi che è sono effettuato il provisioning dei dati nell'archiviazione con questo cmdlet: `Get-AppxProvisionedPackage -Online`.
2. Rimuovere un pacchetto con `Remove-AppxProvisionedPackage -Online -PackageName MyAppPackage`, usando il MyAppPackage appropriato restituiti dal passaggio 1. Ad esempio, per rimuovere il pacchetto Zune correlati, si esegue `Remove-AppxProvisionedPackage -Online -PackageName Microsoft.ZuneMusic_2019.17012.10311.0_neutral_~_8wekyb3d8bbwe`.

## <a name="removing-other-items"></a>Rimozione di altri elementi
È possibile rimuovere l'app e l'icona di OneDrive, disattivare le icone di sistema ed eliminare gli aggiornamenti scaricati.

### <a name="remove-onedrive-icon-and-app"></a>Rimuovere l'app e l'icona di OneDrive
1. Fare clic su **avviare** e scorrere il **OneDrive** icona.
2. Fare doppio clic sui **OneDrive** icona, scegliere **ulteriori**, quindi fare clic su **Apri percorso file**.
3. Fare doppio clic il **OneDrive** icona nel relativo percorso di file e scegliere **eliminare**.

Per rimuovere l'app OneDrive:
1. Fare clic su **avviare** e scorrere il **OneDrive** icona.
2. Fare doppio clic il **OneDrive** icona e quindi fare clic su **Disinstalla**. Programmi e funzionalità apre.
3. In programmi e funzionalità, fare doppio clic su **Microsoft OneDrive** e fare clic su **Disinstalla**.

### <a name="programs-and-features-from-previous-versions-of-control-panel"></a>Programmi e funzionalità (da versioni precedenti del Pannello di controllo)
1. Eseguire il push il **avviare** pulsante, digitare *controllo*, quindi premere INVIO.
2. Toccare o fare doppio clic su **programmi e funzionalità**.
3. All'estrema sinistra, sotto **pagina iniziale Pannello di controllo**, toccare o fare clic su **o disattivazione delle funzionalità Windows attivare**. Verrà aperta una nuova interfaccia utente.
4. Deselezionare le caselle di controllo per tutti gli elementi che desidera o non necessario nell'immagine di base, ad esempio: **Supporto per condivisione File SMB 1.0/CIFS**.

### <a name="turn-system-icons-off"></a>Disattivare le icone di sistema
1. Eseguire il push o fare clic su **avviare**, quindi fare clic su **impostazioni** (l'icona a forma di ingranaggio).
2. Nel **trovare un'impostazione** area di testo, digitare *sulla barra delle applicazioni*e quindi fare clic su **impostazioni sulla barra delle applicazioni**.
3. Sotto il **sulla barra delle applicazioni** sezione, scorrere o Scorri rapidamente verso il basso per il **area di notifica** sezione.
4. Fare clic o toccare **attivare o disattivare la icone di notifica**e quindi attivare ogni icona sistema o disattivare quanto lo si desidera per l'immagine.

### <a name="delete-downloaded-updates"></a>Eliminare gli aggiornamenti scaricati
1. Usando Esplora File passare a **C:\Windows\Software Distribution\Download**.
2. Eliminare tutti i file e cartelle in tale directory.













 













