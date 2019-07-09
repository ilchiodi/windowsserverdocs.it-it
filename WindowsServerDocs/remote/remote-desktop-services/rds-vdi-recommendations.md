---
title: Configurazione consigliata per desktop VDI
description: Impostazioni e configurazione consigliati per ridurre il sovraccarico per desktop Windows 10 1607 (10.0.1393) utilizzati come immagini VDI
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
ms.openlocfilehash: 4f6e68ba1025e23e052d3c40535483ee90cb9b4b
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "63712257"
---
# <a name="recommended-settings-for-vdi-desktops"></a>Impostazioni consigliate per i desktop VDI

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10

Microsoft Desktop Virtualization rileva automaticamente le configurazioni dei dispositivi e le condizioni della rete per rendere operativi gli utenti prima abilitando la configurazione immediata di desktop e applicazioni aziendali e fornisce agli IT il necessario per fornire l'accesso ad applicazioni legacy durante la migrazione a Windows 10.

Anche se il sistema operativo Windows 10 è molto bene ottimizzato per impostazione predefinita, esistono opportunità di ridefinirlo ulteriormente in modo specifico per l'ambiente aziendale di Microsoft Virtual Desktop Infrastructure (VDI). Nell'ambiente VDI, molti servizi e attività in background sono disabilitati all'inizio.

Questo argomento non è un progetto, ma piuttosto una guida o un punto di partenza. Alcuni suggerimenti potrebbero disabilitare la funzionalità che si preferisce usare, è quindi consigliabile valutare il rapporto tra costi e il vantaggio di cambiare qualsiasi impostazione particolare nello scenario corrente.

Queste istruzioni e le impostazioni consigliate sono per Windows 10 1607 (versione 10.0.1393).

> [!NOTE]  
> Eventuali impostazioni non espressamente specificate in questo argomento possono essere lasciate ai valori predefiniti (o impostate secondo i requisiti e criteri preferiti) senza impatto significativo sulle funzionalità VDI.

Quando si crea un'immagine su cui basare la distribuzione VDI, assicurarsi di usare **Current Branch**. Per altre informazioni su Current Branch, vedere [informazioni sulla versione di Windows 10](https://technet.microsoft.com/windows/release-info.aspx).

## <a name="creating-the-windows-10-image"></a>Creazione dell'immagine di Windows 10
Il primo passaggio è installare un'immagine di riferimento di Windows 10 1607 (versione 10.0.1393) in una macchina virtuale o fisica. L'installazione su una macchina virtuale è semplice e consente di salvare le versioni del file (VHD) del disco rigido virtuale, nel caso in cui si desideri eseguire il rollback a una versione precedente.

Durante l'installazione, è possibile scegliere **Impostazioni rapide** oppure **Personalizza**. Le impostazioni disponibili per l'opzione **Personalizza** possono essere modificate mediante Criteri di gruppo, quindi il metodo di installazione del sistema operativo di base non è importante.


Se si è scelto **Personalizza**, è possibile modificare queste impostazioni durante l'installazione:

## <a name="in-customize-settings"></a>In "Personalizza impostazioni"

È inoltre possibile regolare queste impostazioni dopo l'installazione con l'editor dei Criteri di gruppo; vedere la sezione "Impostazioni di criteri di gruppo" di questo argomento.

|Impostazione|Valore predefinito|Valore consigliato per l'uso di VDI|  
|-------------------|----------|--------------|
|**Personalizzazione**| | |
|Personalizza i comandi vocali, la digitazione e gli input penna, inviando i dati di input a Microsoft.|    Attivato| Disattivato|
|Invia i dati della digitazione e di input penna a Microsoft per migliorare la piattaforma di riconoscimento e suggerimenti.|  Attivato| Disattivato|
|Consenti alle app di usare il proprio ID pubblicità per un'esperienza tra app.|  Attivato| Disattivato|
|Consenti a Skype (se installato) di connettersi con gli amici nella rubrica dell'utente e verificare il numero di cellulare. Potrebbero essere applicate tariffe SMS e dati.|    Attivato| Disattivato|
|**Posizione**| | |
|Attiva Trova il mio dispositivo e consenti a Windows e alle app di accedere alla posizione, inclusa la cronologia delle posizioni| Attivato| Disattivato|
|Connettività e segnalazione di errori| | |
|Connettiti automaticamente agli hotspot aperti suggeriti. Non tutte le reti sono sicure.|    Attivato| Disattivato|
|Connetti automaticamente e temporaneamente a hotspot per verificare se sono disponibili servizi di rete a pagamento.| Attivato| Disattivato|
|Invia i dati di utilizzo e diagnostica completa a Microsoft. Disattivando questa opzione si inviano solo i dati di base.| Attivato| Disattivato|
|**Browser, protezione e aggiornamenti**| | |
|Usa i servizi online di SmartScreen per proteggere da contenuti e download dannosi nei siti caricati dalle app Store e i browser di Windows|    Attivato| Attivato (se non è disponibile l'accesso a Internet, allora è impostato su Disattivato.)
|Usa la previsione della pagina per migliorare la lettura, velocizzare l'esplorazione e migliorare l'esperienza complessiva nei browser di Windows. I dati vengono inviati a Microsoft.| Attivato| Disattivato|
|Ricevi aggiornamenti e invia aggiornamenti ad altri computer nella rete Internet per velocizzare l'app e i download di Windows Update|   Attivato| Disattivato|

Al termine dell'installazione, è possibile continuare la modifica delle impostazioni a partire dalle **impostazioni di Windows**.

## <a name="in-windows-settings"></a>In Impostazioni di Windows
Per accedere alle impostazioni di Windows, fare clic su **Start** (l'icona di Windows della barra delle applicazioni), quindi fare clic su l'icona **Impostazioni** (a forma di ingranaggio).

### <a name="in-the-system-area-of-windows-settings"></a>Nell'area "Sistema" delle impostazioni di Windows
Nell'area delle impostazioni di Windows, facendo clic sull'icona **Sistema** è possibile accedere a una serie di impostazioni relative al sistema. Non è necessario modificare tutte le impostazioni per un uso ottimale di VDI, queste impostazioni sono quelle più importanti:

#### <a name="apps-and-features"></a>App e funzionalità

Per rimuovere un'app, escludendola dall'immagine VDI, fare clic sull'app e quindi fare clic su **Disinstalla**. Se **Disinstalla** è disabilitata, non è possibile rimuovere l'app con questo metodo. Potrebbe essere possibile rimuoverla con Windows PowerShell oppure provare a eseguire questi passaggi:
1. Fare clic su **Gestisci funzionalità facoltative** (immediatamente sotto l'intestazione **App e funzionalità** della stessa pagina).
2. Scegliere la funzionalità facoltativa, quindi fare clic su **Disinstalla**.

Le funzionalità che si possono rimuovere (se presenti) sono le seguenti:
- **Contattare il supporto**
- **Contenuto demo della vendita al dettaglio in inglese (Stati Uniti)**
- **Contenuto di demo della vendita al dettaglio neutra**
- **Assistenza rapida**

#### <a name="default-apps"></a>App predefinite

Quest'area consente di definire l'app da usare per impostazione predefinita per determinate funzioni generiche, ad esempio posta elettronica, esplorazione Web e mappe. Se si desidera usare un'altra app per una particolare funzione, fare clic sulla voce corrente e quindi fare clic sull'app che si desidera utilizzare nell'immagine VDI. Per poter utilizzare un'app non Microsoft, è necessario installare l'app prima di modificare questa impostazione.

#### <a name="notifications-and-actions"></a>Notifiche e azioni

Questi valori consigliati riducono le notifiche e le attività di rete in background in un ambiente VDI:

|Impostazione|Valore predefinito|Valore consigliato per l'uso di VDI|  
|-------------------|----------|--------------|
|Ricevi notifiche dalle app e altri mittenti| Attivato| Disattivato|
|Mostra le notifiche nella schermata di blocco.|    Attivato| Disattivato|
|Mostra le sveglie, i promemoria e le chiamate VoIP in entrata nella schermata di blocco.|   Attivato| Disattivato|
|Mostra suggerimenti, trucchi e consigli quando si utilizza Windows.|    Attivato| Disattivato|


#### <a name="offline-maps"></a>Mappe offline

Questa impostazione è disponibile solo se è installata l'app Mappe. Il valore predefinito è **Attivato**; per l'uso VDI il valore consigliato **Disattivato**. 

#### <a name="tablet-mode"></a>Modalità tablet

|Impostazione|Valore predefinito|Valore consigliato per l'uso di VDI|  
|-------------------|----------|--------------|
|Quando si accede|    Usa la modalità appropriata per l'hardware in uso|   Usa la modalità desktop|
|Quando il dispositivo passa automaticamente in modalità attiva o disattiva|    Chiedi sempre prima di eseguire la passaggio| Non chiedere più e non eseguire il passaggio|
|Nascondi le icone delle app sulla barra delle applicazioni in modalità tablet|  Attivato| Disattivato|


### <a name="in-the-devices-area-of-windows-settings"></a>Nell'area "Dispositivi" delle impostazioni di Windows
Nell'area delle impostazioni di Windows, facendo clic sull'icona **Dispositivi** è possibile accedere a una serie di impostazioni relative al sistema. Non è necessario modificare tutte le impostazioni per un uso ottimale di VDI, queste impostazioni sono quelle più importanti:

#### <a name="autoplay"></a>Riproduzione automatica

|Impostazione|Valore predefinito|Valore consigliato per l'uso di VDI|  
|-------------------|----------|--------------|
|Usa la funzionalità Riproduzione automatica per tutti i dispositivi e supporti|    Attivato| Disattivato|
|Unità rimovibile:|Scegliere i valori predefiniti per|Non eseguire alcuna azione|
|Scheda di memoria|Scegliere i valori predefiniti per|Non eseguire alcuna azione|

### <a name="in-the-personalization-area-of-windows-settings"></a>Nell'area "Personalizzazione" delle impostazioni di Windows
Nell'area delle impostazioni di Windows, facendo clic sull'icona **Personalizzazione** è possibile accedere a una serie di impostazioni relative al sistema. Non è necessario modificare tutte le impostazioni per un uso ottimale di VDI, queste impostazioni sono quelle più importanti:

#### <a name="background"></a>Informazioni
In alcuni casi, lo sfondo nero predefinito potrebbe far pensare agli utenti che il computer non risponde. Modificare il colore dello sfondo può risultare utile per evitare questo inconveniente. A tale scopo, effettuare le operazioni seguenti:
1. Nell'area **Sfondo**, fare clic sul menu a discesa.
2. Per modificare il colore dello sfondo, fare clic su **Colore a tinta unita**, quindi fare clic su uno qualsiasi dei colori diversi da nero. In alternativa, è possibile fare clic su **Immagine** e quindi selezionare un'immagine da utilizzare come sfondo.

#### <a name="start"></a>Inizio

|Impostazione|Valore predefinito|Valore consigliato per l'uso di VDI|  
|-------------------|----------|--------------|
|Mostra occasionalmente suggerimenti in Start|    Attivato| Disattivato|
|Mostra le app più usate|Attivato|Disattivato|
|Mostra le app aggiunte di recente|Attivato|Disattivato|
|Mostra gli elementi aperti di recente in Jump List in Start o nella barra delle applicazioni|Attivato|Disattivato|

#### <a name="taskbar"></a>Barra delle applicazioni
L'impostazione predefinita prevede i pulsanti della barra delle applicazioni di grandi dimensioni (ovvero con valore "Disattivato" per **Usa i pulsanti della barra delle applicazioni piccoli**). Questa impostazione comporta l'utilizzo di una grande area della barra delle applicazioni da parte di Cortana. Per evitare questo problema, impostare **Usa i pulsanti della barra delle applicazioni piccoli** su "Attivato." Se si preferisce che gli elementi della barra delle applicazioni siano più grandi, ma non si vuole che Cortana occupi molto spazio, fare clic sulla barra delle applicazioni con il pulsante destro del mouse, scegliere **Cortana**e nel menu a comparsa, selezionare **Nascosto**.

### <a name="in-the-privacy-area-of-windows-settings"></a>Nell'area "Privacy" delle impostazioni di Windows
Nell'area delle impostazioni di Windows, facendo clic sull'icona **Privacy** è possibile accedere a una serie di impostazioni relative al sistema. Non è necessario modificare tutte le impostazioni per un uso ottimale di VDI, queste impostazioni sono quelle più importanti:

#### <a name="general"></a>Generale
Alcune di queste impostazioni vengono impostate anche dalla finestra "Personalizza impostazioni" descritta all'inizio di questo argomento.

|Impostazione|Valore predefinito|Valore consigliato per l'uso di VDI|  
|-------------------|----------|--------------|
|Consenti alle app di usare il mio ID annunci per le esperienze tra app (se si disattiva questa opzione l'ID verrà reimpostato)|  Attivato| Disattivato|
|Permetti ai siti Web di accedere all'elenco delle lingue per fornire contenuti pertinenti per la mia specifica area geografica|Attivato|Disattivato|
|Consenti alle app negli altri miei dispositivi di aprire app e continuare le esperienze in questo dispositivo|Attivato|Disattivato|

#### <a name="camera"></a>Fotocamera

Il valore predefinito per "Consenti alle app di usare la fotocamera" è **Attivato**; per l'uso di VDI il valore consigliato è **Disattivato**.


#### <a name="microphone"></a>Microfono

Il valore predefinito per "Consenti alle app di usare il microfono" è **Attivato**; per l'uso di VDI il valore consigliato è **Disattivato**.

#### <a name="notifications"></a>Notifications

Il valore predefinito per "Consenti alle app di accedere alle notifiche" è **Attivato**; per l'uso di VDI il valore consigliato è **Disattivato**.

#### <a name="contacts"></a>Contatti

Il valore predefinito per "Consenti alle app di accedere ai contatti" è **Attivato**; per l'uso di VDI il valore consigliato è **Disattivato**.

#### <a name="calendar"></a>Calendario

Il valore predefinito per "Consenti alle app di accedere al calendario" è **Attivato**; per l'uso di VDI il valore consigliato è **Disattivato**.

#### <a name="call-history"></a>Registro chiamate

Il valore predefinito per "Consenti alle app di accedere al registro chiamate" è **Attivato**; per l'uso di VDI il valore consigliato è **Disattivato**.

#### <a name="email"></a>Posta elettronica

Il valore predefinito per "Consenti alle app di accedere alla posta elettronica e inviare e-mail" è **Attivato**; per l'uso di VDI il valore consigliato è **Disattivato**.

#### <a name="messaging"></a>Messaggistica

Il valore predefinito per "Consenti alle app di leggere o inviare messaggi" è **Attivato**; per l'uso di VDI il valore consigliato è **Disattivato**.

#### <a name="radios"></a>Radio

Il valore predefinito per "Consenti alle app di controllare la radio" è **Attivato**; per l'uso di VDI il valore consigliato è **Disattivato**.

#### <a name="other-devices"></a>Altri dispositivi

Il valore predefinito per "Consenti alle app di condividere e sincronizzare automaticamente le info con dispositivi wireless non associati in modo esplicito al PC, tablet o telefono" è **Attivato**; per l'uso di VDI il valore consigliato è **Disattivato**.

#### <a name="feedback-and-diagnostics"></a>Feedback e diagnostica

Il valore predefinito per "Windows può richiedere il mio feedback" è **Automaticamente**; per l'uso di VDI, il valore consigliato è **Mai**.

#### <a name="background-apps"></a>App in background
Le app elencate hanno il valore predefinito **Attivato**, che consente loro di ricevere informazioni, inviare notifiche e aggiornarsi automaticamente se vengono utilizzate o meno. È necessario disattivare (impostare su **Disattivato**) tutte le app che non si desidera eseguire in background nell'immagine VDI.

### <a name="update-and-security"></a>Aggiornamento e sicurezza
#### <a name="windows-update"></a>Windows Update
Nell'area **Aggiornare le impostazioni**, fare clic su **Opzioni avanzate** e regolare queste impostazioni:

|Impostazione|Valore predefinito|Valore consigliato per l'uso di VDI|  
|-------------------|----------|--------------|
|Scarica aggiornamenti per altri prodotti Microsoft durante l'aggiornamento di Windows|    deselezionato|    selezionato|
|Rinvia aggiornamenti delle funzionalità|deselezionato|selezionato|
|Usa le mie informazioni di accesso per completare automaticamente la configurazione del dispositivo dopo un aggiornamento |deselezionato|Dipende dalla configurazione specifica di VDI|

Nella pagina **Opzioni avanzate** , fare clic su **Scegliere la modalità di consegna degli aggiornamenti** per accedere all'impostazione "Aggiornamenti da più di un'unica posizione." Il valore predefinito è **Attivato**; per l'uso di VDI il valore consigliato **Disattivato**.

## <a name="in-control-panel-and-other-system-utilities"></a>Nel Pannello di controllo e altre utilità di sistema

È possibile modificare le impostazioni in questa sezione dal Pannello di controllo o aprendo direttamente l'utilità.

> [!NOTE]  
> Eventuali impostazioni non espressamente specificate in questo argomento possono essere lasciate ai valori predefiniti (o impostate secondo i requisiti e criteri preferiti) senza impatto significativo sulle funzionalità VDI.


### <a name="task-scheduler"></a>Utilità di pianificazione
Il modo più rapido per aprire Utilità di pianificazione è premere il pulsante Windows e digitare *utilità di pianificazione* oppure *taskschd.msc*. Nei risultati restituiti, fare clic su **Utilità di pianificazione** per aprire l'utilità. Nell'Utilità di pianificazione espandere **Libreria Utilità di pianificazione**, espandere **Microsoft** e infine **Windows**. È ora possibile accedere all'elenco delle raccolte di attività. Per modificare lo stato di ogni attività pianificata, fare clic con il pulsante destro del mouse sull'attività e quindi fare clic sullo stato desiderato (in genere **Disattivato** per l'uso di VDI).

|Raccolta di attività|Nome attività|Stato predefinito|Stato consigliato per l'uso di VDI|  
|-------------------|-------------|----------|--------------|
|Analisi utilizzo software||||
||Consolidamento|Enabled|Disabled|
||KernelCeipTask|Enabled|Disabled|
||UsbCeip|Enabled|Disabled|
|Deframmentazione||||
||ScheduledDefrag|Enabled|Disabled|
|Location||||
||Notifications|Enabled|Disabled|
||WindowsActionDialog|Enabled|Disabled|
|Manutenzione||||
||Strumento Valutazione sistema Windows|Enabled|Disabled|
|Mappe||||
||MapsToastTask|Enabled|Disabled|
||MapsUpdateTask|Enabled|Disabled|
|Account Mobile Broadband||||
||Parser dei metadati MNO|Enabled|Disabled|
|Diagnostica efficienza energetica||||
||Analisi sistema|Enabled|Disabled|
|Ambiente di ripristino||||
||VerifyWinRE|Enabled|Disabled|
|Demo negozio||||
||CleanupOfflineContent|Enabled|Disabled|
|Shell||||
||FamilySafetyMonitor|Enabled|Disabled|
||FamilySafetyRefreshTask|Enabled|Disabled|
|Segnalazione errori Windows||||
||QueueReporting|Enabled|Disabled|
|Condivisione dei file multimediali di Windows||||
||UpdateLibrary|Enabled|Disabled|

Fare clic su **Windows** nuovamente per comprimerlo, quindi fare clic su **XblGameSave**. Ciò consente di accedere alle attività **XBLGameSaveTask** e **XBLGameSaveTaskLogon**; entrambe possono essere impostate su **Disattivato**.

### <a name="performance-monitor"></a>Performance Monitor
Il modo più rapido per aprire Performance Monitor è premere il pulsante Windows e digitare *Performance Monitor i* oppure *perfmon.msc*. Nei risultati che restituiti, fare clic su **Performance Monitor**. In Performance Monitor fare clic su **Insieme agenti di raccolta dati** quindi fare doppio clic su **Sessioni di traccia eventi**. Fare clic con il pulsante destro del mouse su **WiFiSession**; se si trova nello stato predefinito **In esecuzione**, fare clic su **Arresta**.

Fare clic su **StartupEventTraceSessions**, quindi fare clic con il pulsante destro del mouse su **ReadyBoot**; se è in esecuzione, fare clic su **Arresta**. Fare clic su **Sessioni di traccia eventi**, fare clic con il pulsante destro del mouse su **ReadyBoot**, quindi fare clic su **Proprietà**. Nella finestra di dialogo visualizzata, fare clic sulla scheda **Sessione di traccia**. Deselezionare la casella di controllo **Abilitato** .

### <a name="services"></a>Servizi
Il modo più rapido per gestire i servizi è premere il pulsante Windows e digitare *servizi*. Nei risultati restituiti, fare clic su **Servizi**. I servizi seguenti sono buoni candidati da disabilitare in scenari VDI. Tuttavia, potrebbe essere necessario eseguire alcuni test per verificare che non siano necessari per gli scopi desiderati. Per disabilitare un servizio, nella snap-in **Servizi**, fare clic sul nome del servizio e quindi fare clic su **Proprietà**. Nella scheda **Generale** fare clic sul menu a discesa **Tipo di avvio** e quindi fare clic su **Disabilitato**. Fare clic su **OK**.

- BranchCache
- Ottimizzazione recapito
- Host servizio di diagnostica
- Servizio hotspot di Windows Mobile
- Gestione autenticazione Xbox Live
- Giochi salvati su Xbox Live
- Servizio rete di Xbox Live

### <a name="file-explorer-options"></a>Opzioni di Esplora file
Premere il pulsante Windows e digitare *Pannello di controllo*. Nei risultati restituiti, fare clic su **Pannello di controllo**. Nel Pannello di controllo, fare clic su **Opzioni di Esplora File**. Nella finestra di dialogo visualizzata, fare clic sulla scheda **Ricerca** e quindi nell'area **Durante la ricerca dei percorsi non indicizzati** deselezionare la casella di controllo **Includi le directory di sistema**. Fare clic su **OK** per salvare.

### <a name="flash-settings"></a>Impostazioni flash
Premere il pulsante Windows e digitare *Pannello di controllo*. Nei risultati restituiti, fare clic su **Pannello di controllo**. Nel Pannello di controllo, fare clic su **Flash Player** per aprire Gestione impostazioni di Flash Player. Nella scheda **Memoria**, selezionare il pulsante di opzione per **Impedisci a tutti i siti di archiviare le informazioni in questo computer**. Nella finestra di dialogo visualizzata, fare clic su **OK**.

Nella scheda **Fotocamera e microfono** nell'area **Impostazioni fotocamera e microfono**, selezionare il pulsante di opzione per **Impedisci a tutti i siti di usare la fotocamera e il microfono**.

Nella scheda **Riproduzione** nell'area **Reti peer-assisted**, selezionare il pulsante di opzione per **Impedisci a tutti i siti l'utilizzo delle reti peer-assisted**. Chiudere Gestione impostazioni di Flash Player.

### <a name="internet-options"></a>Opzioni Internet
Premere il pulsante Windows e digitare *Pannello di controllo*. Nei risultati restituiti, fare clic su **Pannello di controllo**. Nel Pannello di controllo, fare clic su **Opzioni Internet** per aprire le proprietà Internet. Nell'area **Home page**, immettere l'URL del sito Web che si desidera che gli utenti visualizzino come home page nel browser. Potrebbe trattarsi di un sito web della società o è possibile impostarlo su una pagina iniziale vuota immettendo *about:blank*.

Nell'area **Cronologia esplorazioni**, selezionare la casella di controllo **Elimina la cronologia delle esplorazioni alla chiusura del programma**.

### <a name="power-options"></a>Opzioni risparmio energia
Premere il pulsante Windows e digitare *Pannello di controllo*. Nei risultati restituiti, fare clic su **Pannello di controllo**. Nel Pannello di controllo, fare clic su **Opzioni di risparmio energia** per aprire il pannello di controllo delle opzioni di risparmio energia. Nell'area **Scegliere o personalizzare una combinazione per il risparmio di energia**, fare clic sulla freccia verso il basso per **Mostra le combinazioni aggiuntive** e quindi seleziona il pulsante di opzione per **Alte prestazioni**. Questa impostazione ha un impatto minimo sull'host VDI.

### <a name="system"></a>Sistema
Premere il pulsante Windows e digitare *Pannello di controllo*. Nei risultati restituiti, fare clic su **Pannello di controllo**. Nel Pannello di controllo, fare clic su **Sistema** per aprire il pannello di controllo di sistema. Nel riquadro sinistro, fare clic su **Impostazioni di sistema avanzate**. Nella finestra di dialogo visualizzata, fare clic sulla scheda **Avanzate**. Nell'area **Prestazioni**, fare clic sul pulsante **Impostazioni**, quindi nella scheda **Effetti visivi** nella finestra di dialogo che si apre selezionare il pulsante di opzione **Regola per ottimizzare le prestazioni**. Fare clic su **OK** per salvare e uscire.

## <a name="group-policy-settings"></a>Impostazioni di Criteri di gruppo

Per modificare le impostazioni dei Criteri di gruppo, premere il pulsante Windows e il tipo *Criteri di gruppo* oppure *gpedit.msc*. Nei risultati restituiti, fare clic su **Modifica criteri di gruppo** per aprire l'editor dei criteri di gruppo locali.

> [!NOTE]  
> Eventuali impostazioni non espressamente specificate in questo argomento possono essere lasciate ai valori predefiniti (o impostate secondo i requisiti e criteri preferiti) senza impatto significativo sulle funzionalità VDI.

Sotto **Configurazione computer** espandere **Impostazioni di Windows**  e quindi **Impostazioni di sicurezza**. Fare clic su **Criteri di gestione dell'elenco reti**, quindi fare doppio clic su **Tutte le reti**. Nella finestra di dialogo che si apre, nell'area **Percorso di rete** , selezionare il pulsante di opzione per **L'utente non può cambiare percorso**. Fare clic sul pulsante **OK** per salvare.

Comprimere le **Impostazioni di Windows**, quindi espandere **Modelli amministrativi**. Fare clic o espandere **Rete**e quindi modificare ogni impostazione come indicato di seguito facendo doppio clic sull'impostazione stessa, quindi selezionando il pulsante di opzione per il valore indicato e facendo clic sul pulsante **OK**:

|Area delle impostazioni|Impostazione|Valore consigliato per l'uso di VDI|  
|-------------------|-------|----------|
|Servizio trasferimento intelligente in background (BITS)|||
||Non consentire al client BITS di utilizzare Windows BranchCache|Enabled|
||Non consentire al computer di agire come un client Peer caching BITS|Enabled|
||Non consentire al computer di agire come un server Peer caching BITS|Enabled|
||Consenti il Peer caching BITS|Disabled|
|BranchCache||
||Attiva BranchCache|Disabled|
|Autenticazione hotspot||
||Attiva l'autenticazione hotspot|Disabled|
|Servizi di rete peer-to-peer Microsoft||
||Disattiva i servizi di rete peer-to-peer Microsoft|Enabled|
|File offline||
||Consenti o non consentire l'uso di funzionalità file Offline|Disabled|

Comprimere **Rete**, quindi espandere **Sistema**. Modificare ogni impostazione come indicato di seguito facendo doppio clic sull'impostazione stessa, quindi selezionando il pulsante di opzione per il valore indicato e facendo clic sul pulsante **OK**:

|Area delle impostazioni|Impostazione|Valore consigliato per l'uso di VDI|  
|-------------------|----------|--------------|
|Installazione di dispositivi||
||Non inviare una segnalazione errore di Windows quando si installa un driver generico per un dispositivo|Enabled|
||Impedisci la creazione di un punto di ripristino del sistema durante l'attività del dispositivo che normalmente richiederebbe la creazione di un punto di ripristino|Enabled|
||Impedisci il recupero dei metadati del dispositivo da Internet|Enabled|
||Impedisci a Windows di inviare una segnalazione errore quando un driver di dispositivo richiede software aggiuntivo durante l'installazione|Enabled|
||Disattiva il fumetto "Trovato nuovo hardware" durante l'installazione del dispositivo|Enabled|

Espandere **Filesystem**, fare doppio clic su **NTFS**, fare doppio clic su **Opzioni di creazione nomi brevi**, selezionare il pulsante di opzione per **Abilitato**, e quindi usare il menu a discesa **Opzioni** per selezionare **Abilita in tutti i volumi**. Fare clic sul pulsante **OK** per salvare.

Comprimere **Filesystem** quindi espandere **Gestione comunicazioni Internet**. Fare clic su **Impostazioni comunicazione Internet**. Modificare ogni impostazione come indicato di seguito facendo doppio clic sull'impostazione stessa, quindi selezionando il pulsante di opzione per **Abilitato** e facendo clic sul pulsante **OK**:

- Disattiva collegamenti di "Events.asp" in Visualizzatore eventi
- Disattiva condivisione dati personalizzazione riconoscimento grafia
- Disattiva segnalazione errori di riconoscimento grafia
- Disattiva contenuto nella sezione "Non tutti sanno che" di Guida e supporto tecnico content
- Disattiva ricerca Microsoft Knowledge Base in Guida e supporto tecnico
- Disattiva Connessione guidata Internet se connessione a URL rimanda a Microsoft.com
- Disattiva download Internet per le procedure di pubblicazione guidata sul Web e ordinazione guidata online via Internet
- Disattiva servizio Internet associazioni file
- Disattiva registrazione se connessione a URL rimanda a Microsoft.com
- Disattiva l'operazione immagine "Ordina stampe"
- Disattiva l'operazione per file e cartelle "Pubblica sul Web"
- Disattiva Analisi utilizzo software Windows Messenger
- Disattiva il programma Analisi utilizzo software Windows
- Disattiva Segnalazione errori Windows
- Disattiva ricerca driver di dispositivo in Windows Update

Fare clic su **Risparmio energia** e quindi fare doppio clic su **Seleziona una combinazione attiva per il risparmio di energia**. Selezionare il pulsante di opzione per **Abilitato**, quindi utilizzare il menu a discesa **Opzioni** per selezionare **Alte prestazioni**. Fare clic sul pulsante **OK** per salvare.

Fare clic su **Ripristino**, quindi fare doppio clic su **Consenti ripristino del sistema allo stato predefinito**. Selezionare il pulsante di opzione per **Abilitato**, quindi fare clic sui **OK** per salvare.

Espandere **Risoluzione dei problemi e diagnostica**. Fare clic su **Manutenzione pianificata**, fare doppio clic su **Configura il comportamento di manutenzione pianificata**, quindi selezionare il pulsante di opzione per **Disabilitato**. Fare clic sul pulsante **OK** per salvare.

Selezionare ognuna delle seguenti aree di impostazioni, quindi fare doppio clic su **Configura livello di esecuzione dello scenario**, selezionare il pulsante di opzione per **Disabilitato**, quindi fare clic su **OK** per salvare:

- Diagnostica di Windows Boot Performance
- Diagnostica della perdita di memoria di Windows
- Risoluzione e rilevamento dell'esaurimento risorse di Windows
- Diagnostica delle prestazioni di arresto di Windows
- Diagnostica delle prestazioni di standby/ripresa di Windows
- Diagnostica delle prestazioni di velocità di risposta del sistema di Windows

Comprimere **Sistema** quindi espandere **Componenti Windows**. Modificare ogni impostazione come indicato di seguito facendo doppio clic sull'impostazione stessa, quindi selezionando il pulsante di opzione per il valore indicato e facendo clic sul pulsante **OK**:

|Area delle impostazioni|Impostazione|Valore consigliato per l'uso di VDI|  
|-------------------|-------|----------|
|Aggiungi funzionalità a Windows 10|||
||Impedisci l'esecuzione della procedura guidata|Enabled|
|Criteri di riproduzione automatica|||
||Imposta il comportamento predefinito per l'esecuzione automatica|Abilitato, quindi usare il menu a discesa **Opzioni** per selezionare **Non eseguire i comandi di esecuzione automatica**|
|Contenuto cloud|||
||Non mostrare i suggerimenti di Windows|Enabled|
||Disattiva le esperienze per gli utenti consumer Microsoft|Enabled|
|Raccolta dei dati e versioni di anteprima|||
||Consenti telemetria|Abilitato, quindi usare il menu a discesa **Opzioni** per selezionare **1-Di base**|
||Disabilita funzionalità o impostazioni non definitive|     Disabled|
||Non mostrare notifiche feedback|       Enabled|
||Attiva/disattiva controllo utente per build Insider|      Disabled|
|Gestione finestre desktop|||
||Non consentire chiamata Flip3D|       Enabled|
||Non consentire animazioni delle finestre|       Enabled|
||Usa tinta unita come sfondo di Start|     Enabled|
|Interfaccia utente basata su bordi|||
||Consenti di scorrere verso il bordo|     Disabled|
||Disattiva i suggerimenti della Guida|        Enabled|
|Esplora file|||
||Non visualizzare la notifica "nuova applicazione installata"|     Enabled|
|Esplora giochi|||
||Disattiva il download delle informazioni sui giochi|     Enabled|
||Disattiva gli aggiornamenti dei giochi|        Enabled|
||Disattiva il tracciamento dell'ultimo tempo di gioco nella cartella Giochi|     Enabled|
|Gruppo Home|||
||Impedisci al computer di partecipare a un gruppo home|        Enabled|
|Internet Explorer|||
||Consenti ai servizi Microsoft di fornire suggerimenti avanzati quando l'utente digita nella barra degli indirizzi|        Disabled|
||Disabilita controllo periodico per aggiornamenti software di Internet Explorer|        Enabled|
||Disabilita la visualizzazione della schermata iniziale|        Enabled|
||Installa automaticamente nuove versioni di Internet Explorer|      Disabled|
||Impedisci la partecipazione al programma Analisi utilizzo software|     Enabled|
||Impedisci l'esecuzione del primo avvio guidato Vai direttamente alla home page|   Abilitato, quindi usare il menu a discesa **Opzioni** per selezionare **Vai direttamente alla home page**|
||Imposta la crescita del processo della scheda|Abilitato, quindi digitare il comando seguente nella casella **Crescita processo scheda**: *Bassa*.|
||Specificare il comportamento predefinito per una nuova scheda|Abilitato, quindi usare il menu a discesa **Opzioni** per selezionare **Nuova scheda**|
||Disattiva le notifiche sulle prestazioni dei componenti aggiuntivi|        Enabled|
||Disattiva georilevazione nel browser|     Enabled|
||Disattiva Riapri ultima sessione di esplorazione|        Enabled|
||Disattiva i suggerimenti per tutti i provider installati dall'utente|        Enabled|
||Attiva Siti suggeriti|       Disabled|

Allo stesso livello di **Internet Explorer** le impostazioni sono state modificate solo nella tabella precedente, notare un ulteriore livello di cartelle compreso tra **Acceleratori** e **Barre degli strumenti**. In altre parole, ora si è in Criteri del computer locale > Configurazione computer > Modelli amministrativi > Componenti di Windows > Internet Explorer. 

Aprire la cartella **Elimina cronologia esplorazioni** cartella, fare doppio clic su **Consenti l'eliminazione della cronologia di esplorazione all'uscita**, selezionare **Abilita**e quindi fare clic su **OK**per salvare e uscire.

Utilizzare la freccia indietro nella parte superiore sinistra dell'Editor dei criteri di gruppo locali per tornare indietro al livello di **Internet Explorer**. Fare doppio clic su **Impostazioni Internet**, fare doppio clic su **Impostazioni avanzate**, quindi modificare le impostazioni nelle sottocartelle nel modo seguente:

|Cartella delle impostazioni sotto **Impostazioni avanzate**|Impostazione|Valore consigliato per l'uso di VDI|  
|-------------------|-------|----------|
|**Esplorazione**|||
||Disattiva rilevamento numeri di telefono|Enabled|
|**Contenuti multimediali**|||
||Consenti a Internet Explorer di riprodurre file multimediali che usano codec alternativi|Disabled|

Tornare al livello di **Internet Explorer**, quindi fare doppio clic su **Impostazioni Internet**. In questa cartella, impostare queste due impostazioni sotto **Completamento automatico** su **Abilitato**:

- Disattiva Suggerimenti - URL
- Disattiva la funzionalità Completamento automatico di Windows Search

Tornare indietro di quattro livelli ai **Componenti di Windows**, fare doppio clic su **Posizione e sensori**e quindi impostare queste tre impostazioni su **Abilitato** (per ognuna, fare clic su  **OK** per salvare e chiudere):

- Disattiva posizione
- Disattiva script posizioni
- Disattiva sensori

Al livello **Posizione e sensori**, fare doppio clic su **Localizzatore geografico di Windows** e impostare **Disattiva Localizzatore geografico di Windows** su **Abilitato**. Fare clic su **OK** per salvare e uscire.

Nel riquadro sinistro, fare clic su **Mappe**, impostare queste impostazioni su **Abilitato**, quindi per ognuna fare clic su **OK** per salvare e uscire:

- Disattiva il download e l'aggiornamento automatici dei dati delle mappe
- Disattiva il traffico di rete non richiesto nella pagina di impostazioni Mappe offline

Usando il riquadro a sinistra, immettere tutte le sottocartelle delle impostazioni seguenti e modificare le singole impostazioni come indicato di seguito:

|Cartella di impostazioni sotto **Componenti di Windows**|Impostazione|Valore consigliato per l'uso di VDI|  
|-------------------|-------|----------|
|**OneDrive**|||
||Impedisci uso di OneDrive per archiviazione file|Enabled|
||Salva i documenti su OneDrive per impostazione predefinita|Disabled|
|**Feed RSS**|||
||Impedisci l'individuazione automatica di feed e Web Slice|Enabled|
|**Ricerca**|||
||Consenti Cortana|        Disabled|
||Consenti Cortana sulla schermata di blocco|      Disabled|
||Consenti l'uso della posizione per la ricerca e Cortana|     Disabled|
||Non consentire la ricerca nel Web|      Enabled|
||Non cercare nel Web o visualizzare risultati Web per la ricerca|        Enabled|
||Impedisci l'aggiunta di posizioni UNC da indicizzare dal Pannello di controllo|     Enabled|
||Impedisci l'indicizzazione di file nella cache di file offline|        Enabled|
|**Store**|||
||Disattiva l'offerta di aggiornamento all'ultima versione di Windows|Enabled|
|**Segnalazione errori Windows**|||
||Invia automaticamente i file di backup della memoria per i report di errore generati dal sistema operativo|       Disabled|
||Disattivare Segnalazione errori Windows|      Enabled|
|**Windows Installer**|||
||Dimensione massima del controllo della cache dei file di base|  Abilitato, quindi usare la casella di selezione nell'area **Opzioni** per impostare **Dimensione massima della cache dei file di base** su *5*.|
||Disattiva la creazione di punti di controllo di ripristino configurazione di sistema|      Enabled|
|**Windows Mail**|||
||Disattiva la funzionalità di community|Enabled|
|**Windows Media Player**|||
||Non visualizzare le finestre di dialogo del primo utilizzo|       Enabled|
||Impedisci la condivisione dei file multimediali|        Enabled|
|**Centro PC portatile Windows**|||
||Disattiva Centro PC portatile Windows|Enabled|
|**Analisi di affidabilità di Windows**|||
||Configura i provider WMI di affidabilità|Disabled|
|**Windows Update**|||
||Consenti installazione immediata aggiornamenti automatici|       Enabled|
||Rimuovi l'accesso a tutte le funzionalità di Windows Update|     Enabled|
|Nella cartella**Windows Update**, aprire **Rinvia aggiornamento di Windows**|||
||Seleziona quando vengono ricevuti gli aggiornamenti delle funzionalità|Abilitato, quindi nell'area **Opzioni**, usare il menu a discesa **Seleziona il livello di conformità di branch per gli aggiornamenti delle funzionalità che si desidera ricevere** per selezionare **Current Branch for Business**. Impostare la casella di selezione**Dopo il rilascio di un aggiornamento delle funzionalità, posticipare la ricezione per il numero di giorni seguente** su *180 giorni*.
||Seleziona quando vengono ricevuti gli aggiornamenti qualitativi|Abilitato, quindi nell'area **Opzioni**, impostare la casella di selezione **Dopo il rilascio di un aggiornamento qualitativo, posticipare la ricezione per il numero di giorni seguente** su *30 giorni* e selezionare la casella di controllo **Sospendere gli aggiornamenti qualitativi**.

Nel riquadro sinistro dell'Editor dei criteri di gruppo locali, fare clic su **Configurazione utente**. Usando il riquadro a sinistra, fare clic su **Modelli amministrativi** e immettere tutte le sottocartelle delle impostazioni seguenti e modificare le singole impostazioni come indicato di seguito:

|Cartella di impostazioni sotto **Modelli amministrativi**|Impostazione|Valore consigliato per l'uso di VDI|  
|-------------------|-------|----------|
|**Desktop**|||
||Non aggiungere condivisioni dei documenti aperti recentemente ai percorsi di rete|Enabled|
|Nella cartella**Desktop**, aprire **Active Directory**|||
||Dimensioni massime delle ricerche di Active Directory|Abilitato, quindi nell'area **Opzioni**, usare la casella di selezione per impostare **Numero di oggetti restituiti** su *5.000*.|
|**Menu Start e barra delle applicazioni**|||
||Cancella l'elenco di programmi recenti per i nuovi utenti|     Enabled|
||Non visualizzare o rilevare gli elementi nelle Jump List da posizioni remote|        Enabled|
||Disattiva le notifiche a fumetto degli annunci delle funzionalità|     Enabled|
||Disattiva il monitoraggio dell'utente|       Enabled|
|Nella cartella **Menu Start e barra delle applicazioni**, aprire **Notifiche**|||
||Disattiva notifiche di tipo avviso popup|Enabled|
|Nella cartella **Componenti di Windows** aprire:|||
|**Contenuti cloud**|||
||Disattiva tutte le funzionalità di Contenuti in evidenza di Windows|Enabled|
|**Esplora file**|||
||Disattiva la memorizzazione nella cache delle miniature|       Enabled|
||Disattiva la visualizzazione di voci delle ricerca recenti nella casella di ricerca di Esplora File|        Enabled|
||Disattiva la memorizzazione nella cache delle miniature nel file nascosto thumbs.db|      Enabled|

## <a name="microsoft-store-apps"></a>App del Microsoft Store
Alcune app di Microsoft Store possono essere rimosse dall'immagine VDI. La loro rimozione riduce l'utilizzo della CPU e risparmia spazio su disco. Di seguito sono riportati i possibili candidati per la rimozione:

- Ottieni Office
- Skype (anteprima)
- Attività iniziali (in particolare se non è disponibile nessuna connessione Internet)
- Hub di Feedback
- Microsoft Solitaire Collection
- Wi-Fi a pagamento e cellulare

Per personalizzare il profilo utente predefinito usato per la creazione di immagini VDI, usare l'account Administrator predefinito. Se non è già abilitato, è possibile abilitarlo tramite utenti e gruppi locali in Gestione Computer. Quindi accedere all'account amministratore per completare la procedura seguente.

> [!NOTE]  
> Non rimuovere le app di sistema, ad esempio l'app Store. Sono difficili da reinstallare. Altre app sono reinstallabili facilmente dallo Store.

### <a name="delete-unwanted-apps-from-the-administrator-user-profile"></a>Elimina le app indesiderate dal profilo dell'utente amministratore
1. In Windows PowerShell, eseguire `Get-AppxPackage | ft PackageFamilyName` per visualizzare l'elenco delle App installate.
2. Per ogni pacchetto dell'app che si desidera disinstallare eseguire i cmdlet di questo formato di esempio:

    `Get-AppxPackage *messaging* | Remove-AppxPackage`

    `Get-AppxPackage *WindowsMaps* | Remove-AppxPackage`

    `Get-AppxPackage *ZuneMusic* | Remove-AppxPackage`

### <a name="delete-the-payload-of-unwanted-store-apps"></a>Eliminare il payload delle app dello Store indesiderate
Questo impedisce la reinstallazione delle app.
1. Elencare le app dello Store e altri elementi che hanno effettuato il provisioning dei dati nella memoria con questo cmdlet: `Get-AppxProvisionedPackage -Online`.
2. Rimuovere un pacchetto con `Remove-AppxProvisionedPackage -Online -PackageName MyAppPackage`, usando il MyAppPackage appropriato restituito dal passaggio 1. Ad esempio, per rimuovere il pacchetto Zune, si esegue `Remove-AppxProvisionedPackage -Online -PackageName Microsoft.ZuneMusic_2019.17012.10311.0_neutral_~_8wekyb3d8bbwe`.

## <a name="removing-other-items"></a>Rimozione di altri elementi
È possibile rimuovere l'app e l'icona di OneDrive, disattivare le icone di sistema ed eliminare gli aggiornamenti scaricati.

### <a name="remove-onedrive-icon-and-app"></a>Rimuovere l'app e l'icona di OneDrive
1. Fare clic su **Start** e scorrere all'icona di **OneDrive**.
2. Fare clic con il pulsante destro del mouse sull'icona di **OneDrive** , scegliere **Altro**, quindi fare clic su **Apri percorso file**.
3. Fare clic con il pulsante destro del mouse sull'icona di **OneDrive** nel relativo percorso del file e fare clic su **Elimina**.

Per rimuovere l'app OneDrive:
1. Fare clic su **Start** e scorrere all'icona di **OneDrive**.
2. Fai clic con il pulsante destro del mouse sull'icona di **OneDrive** quindi selezionare **Disinstalla**. Si apre Programmi e funzionalità.
3. In Programmi e funzionalità, fare doppio clic su **Microsoft OneDrive** e fare clic su **Disinstalla**.

### <a name="programs-and-features-from-previous-versions-of-control-panel"></a>Programmi e funzionalità (da versioni precedenti del Pannello di controllo)
1. Premere il pulsante **Start**, digitare *Controllo*, quindi premere INVIO.
2. Toccare o fare doppio clic su **Programmi e funzionalità**.
3. All'estrema sinistra, sotto **pagina iniziale del Pannello di controllo**, toccare o fare clic su **Attiva e disattiva le funzionalità Windows**. Si apre una nuova interfaccia utente.
4. Deselezionare le caselle di controllo per tutti gli elementi che non si desiderano o non sono necessari nell'immagine di base, ad esempio: **Supporto condivisione di file SMB 1.0/CIFS**.

### <a name="turn-system-icons-off"></a>Disattivare le icone di sistema
1. Premere o fare clic su **Start**, quindi fare clic su **Impostazioni** (l'icona a forma di ingranaggio).
2. Nell'area di testo**Trova un'impostazione** , digitare *Barra delle applicazioni*e quindi fare clic su **Impostazioni della barra delle applicazioni**.
3. Nella sezione **Barra delle applicazioni**, scorrere verso il basso fino alla sezione **Area di notifica**.
4. Fare clic o toccare **Attiva o disattiva le icone di notifica**e quindi attiva o disattiva ogni icona di sistema a seconda delle preferenze per l'immagine.

### <a name="delete-downloaded-updates"></a>Eliminare gli aggiornamenti scaricati
1. Usando Esplora File andare a **C:\Windows\Software Distribution\Download**.
2. Eliminare tutti i file e le cartelle in tale directory.













 













