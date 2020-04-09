---
title: Installazione o disinstallazione di ruoli, servizi ruolo o funzionalità
description: Server Manager
ms.prod: windows-server
ms.technology: manage-server-manager
ms.topic: article
ms.assetid: 04f16d84-45c2-4771-84c1-1cc973d0ee02
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8c270fbdacf5359af4e3150d61693470207f566b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851524"
---
# <a name="install-or-uninstall-roles-role-services-or-features"></a>Installazione o disinstallazione di ruoli, servizi ruolo o funzionalità

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In Windows Server, la console di Server Manager e cmdlet di Windows PowerShell per Server di gestione consentire l'installazione di ruoli e funzionalità in server locali o remoti oppure dischi rigidi virtuali (VHD) offline. È possibile installare più ruoli e funzionalità in un singolo server remoto o in un disco rigido virtuale offline in un'unica procedura guidata Aggiungi ruoli e funzionalità o sessione di Windows PowerShell.  
  
> [!IMPORTANT]  
> Server Manager può essere utilizzata per gestire una versione più recente del sistema operativo Windows Server. Non è possibile usare Server Manager in esecuzione in Windows Server 2012 R2 o Windows 8.1 per installare ruoli, servizi ruolo e funzionalità in server che eseguono Windows Server 2016.  
  
In qualità di amministratore per installare o disinstallare ruoli, servizi ruolo e funzionalità, è necessario essere connessi a un server. Se si è connessi al computer locale con un account che non dispone di diritti di amministratore nel server di destinazione, fare clic con il pulsante destro del mouse sul server di destinazione nel riquadro **Server** e quindi scegliere **Account di gestione** per specificare un account con diritti di amministratore. Il server in cui si desidera montare un disco rigido virtuale offline deve essere aggiunto a Server Manager ed è necessario disporre di diritti di amministratore per tale server.  
  
Per ulteriori informazioni su quali sono i ruoli, servizi ruolo e funzionalità, vedere [ruoli, servizi ruolo e funzionalità](https://go.microsoft.com/fwlink/p/?LinkId=239558).  
  
In questo argomento sono contenute le seguenti sezioni.  
  
-   [Installare ruoli, servizi ruolo e funzionalità con l'aggiunta guidata ruoli e funzionalità](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard)  
  
-   [Installare ruoli, servizi ruolo e funzionalità con cmdlet di Windows PowerShell](#install-roles-role-services-and-features-by-using-windows-powershell-cmdlets)  
  
-   [Rimuovere ruoli, servizi ruolo e funzionalità con la rimozione guidata ruoli e funzionalità](#remove-roles-role-services-and-features-by-using-the-remove-roles-and-features-wizard)  
  
-   [Rimuovere ruoli, servizi ruolo e funzionalità con cmdlet di Windows PowerShell](#remove-roles-role-services-and-features-by-using-windows-powershell-cmdlets)  
  
-   [Installare ruoli e funzionalità in più server eseguendo uno script di Windows PowerShell](#install-roles-and-features-on-multiple-servers-by-running-a-windows-powershell-script)  
  
-   [Installare .NET Framework 3.5 e altre funzionalità su richiesta](#install-net-framework-35-and-other-features-on-demand)  
  
## <a name="install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard"></a>Installare ruoli, servizi ruolo e funzionalità con l'aggiunta guidata ruoli e funzionalità  
In una singola sessione dell'aggiunta guidata ruoli e funzionalità è possibile installare ruoli, servizi ruolo e funzionalità nel server locale, un server remoto che è stato aggiunto a Server Manager o un disco rigido virtuale offline. Per ulteriori informazioni su come aggiungere un server a Server Manager per gestire, vedere [aggiungere server a Server Manager](add-servers-to-server-manager.md).  
  
> [!NOTE]  
> Se si esegue Server Manager in Windows Server 2016 o Windows 10, è possibile usare l'aggiunta guidata ruoli e funzionalità per installare ruoli e funzionalità solo nei server e nei dischi rigidi virtuali offline che eseguono Windows Server 2016.  
  
#### <a name="to-install-roles-and-features-by-using-the-add-roles-and-features-wizard"></a>Per installare ruoli e funzionalità con l'aggiunta guidata ruoli e funzionalità  
  
1.  Se Server Manager è già aperto, andare al passaggio successivo. Se Server Manager non è aperto, aprirlo in uno dei modi seguenti.  
  
    -   Nel desktop di Windows avviare Server Manager facendo clic su **Server Manager** nella barra delle applicazioni di Windows.  
  
    -   Nella schermata **Start** di Windows fare clic sul riquadro **Server Manager** .  
  
2.  Scegliere **Aggiungi ruoli e funzionalità**dal menu **Gestisci** .  
  
3.  Nella pagina **Prima di iniziare** verificare che il server di destinazione e l'ambiente di rete siano preparati per il ruolo e la funzionalità che si desidera installare. Fare clic su **Avanti**.  
  
4.  Nella pagina **Selezione tipo di installazione** scegliere **Installazione basata su ruoli o basata su funzionalità** per installare tutte le parti di ruoli o funzionalità in un singolo server oppure scegliere **Installazione di Servizi Desktop remoto** per installare un'infrastruttura desktop basata su macchine virtuali oppure un'infrastruttura desktop basata su sessioni per Servizi Desktop remoto. L'opzione **Installazione di Servizi Desktop remoto** consente di distribuire parti logiche di Servizi Desktop remoto su server diversi, in base alle esigenze degli amministratori. Fare clic su **Avanti**.  
  
5.  Nella pagina **Selezione server di destinazione** scegliere un server dal pool di server oppure selezionare un disco rigido virtuale offline. Per selezionare un disco rigido virtuale offline come server di destinazione, selezionare innanzitutto il server in cui montare il disco rigido virtuale, quindi selezionare il file del disco rigido virtuale. Per informazioni su come aggiungere server al pool di server, vedere [aggiungere server a Server Manager](add-servers-to-server-manager.md). Dopo aver selezionato il server di destinazione, fare clic su **Avanti**.  
  
    > [!NOTE]  
    > Per installare ruoli e funzionalità in dischi rigidi virtuali offline, i dischi rigidi virtuali di destinazione devono soddisfare i requisiti seguenti.  
    >   
    > -   I dischi rigidi virtuali devono essere in esecuzione la versione di Windows Server che corrisponde alla versione di Server Manager è in esecuzione. Vedere la nota all'inizio dell' [installazione dei ruoli, dei servizi ruolo e delle funzionalità con l'aggiunta guidata ruoli e funzionalità](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard).  
    > -   I dischi rigidi virtuali non possono contenere più di un volume o una partizione di sistema.  
    > -   La cartella condivisa di rete in cui è archiviato il file VHD deve concedere i dritti di accesso seguenti all'account computer (o sistema locale) del server selezionato per montare il disco rigido virtuale. L'accesso con account solo utente non è sufficiente. La condivisione può concedere le autorizzazioni **Lettura** e **Scrittura** al gruppo **Everyone** per consentire l'accesso al disco rigido virtuale, ma si tratta di un'impostazione sconsigliata per motivi di sicurezza.  
    >   
    >     -   Accesso in **Lettura/Scrittura** alla finestra di dialogo **Condivisione file**.  
    >     -   **Controllo completo** accesso il **sicurezza** scheda, file o cartella **proprietà** la finestra di dialogo.  
  
6.  Selezionare i ruoli, selezionare i servizi ruolo per il ruolo se applicabile e quindi fare clic su **Avanti** per selezionare le funzionalità.  
  
    Man mano che si procede, l'aggiunta guidata ruoli e funzionalità informa automaticamente se sono stati rilevati conflitti nel server di destinazione che possono impedire l'installazione o il normale funzionamento dei ruoli o delle funzionalità selezionate. Viene inoltre richiesto di aggiungere eventuali ruoli, servizi ruolo o funzionalità necessari in base ai ruoli o alle funzionalità selezionati.  
  
    Inoltre, se si prevede di gestire il ruolo in modalità remota da un altro server o da un computer basato su client Windows che esegue Strumenti di amministrazione remota del server, è possibile scegliere di non installare strumenti di gestione e snap-in per i ruoli nel server di destinazione. Per impostazione predefinita, nell'aggiunta guidata ruoli e funzionalità, gli strumenti di gestione sono selezionati per l'installazione.  
  
7.  Nella pagina **Conferma selezioni per l'installazione**, rivedere le selezioni di ruolo funzionalità e server. Se si è pronti per l'installazione, fare clic su **Installa**.  
  
    È anche possibile esportare le selezioni in un file di configurazione basato su XML che è possibile utilizzare per le installazioni automatiche con Windows PowerShell. Per esportare la configurazione specificata in questa procedura guidata Aggiungi ruoli e funzionalità, fare clic su **Esporta impostazioni di configurazione**e quindi salvare il file XML in un percorso appropriato.  
  
    Il comando **Specificare un percorso di origine alternativo** nella pagina **Conferma selezioni per l'installazione** consente di specificare un percorso di origine alternativo per i file necessari all'installazione di ruoli e funzionalità sul server selezionato. In Windows Server 2012 e versioni successive di Windows Server, [funzionalità su richiesta](https://go.microsoft.com/fwlink/p/?LinkID=241573) consentono di ridurre la quantità di spazio su disco utilizzato dal sistema operativo, rimuovendo i file di ruolo e funzionalità dai server gestiti esclusivamente in modalità remota. Se sono stati rimossi file di ruolo e funzionalità da un server mediante il cmdlet `Uninstall-WindowsFeature -remove`, è possibile installarli sul server in futuro specificando un percorso di origine alternativo o una condivisione su cui vengono archiviati i file di ruolo e funzionalità necessari. Il percorso di origine o la condivisione file deve concedere autorizzazioni di **lettura** al gruppo **Everyone** (sconsigliato per motivi di sicurezza) o all'account computer (*dominio*\\*SERverNAME*$) del server di destinazione. la concessione dell'accesso all'account utente non è sufficiente. Per altre informazioni sulle funzionalità su richiesta, vedere [Opzioni di installazione di Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573).  
  
    È possibile specificare un file WIM come origine del file di funzionalità alternativo quando si installano ruoli, servizi ruolo e funzionalità in un server fisico in esecuzione. Il percorso di origine per un file WIM dovrebbe essere nel formato seguente, con il prefisso **WIM** e l'indice della posizione dei file della funzionalità come suffisso: **WIM:e:\sources\install.wim:4**. È tuttavia possibile utilizzare un file WIM direttamente come origine per l'installazione di ruoli, servizi ruolo e funzionalità di un disco rigido Virtuale offline. è necessario montare il disco rigido Virtuale offline e puntare al percorso di montaggio per i file di origine oppure puntare a una cartella che contiene una copia del contenuto del file WIM.  
  
8.  Dopo aver fatto clic su **Installa**, nella pagina **stato dell'installazione** vengono visualizzati lo stato dell'installazione, i risultati e messaggi quali avvisi, errori o procedure di configurazione successive all'installazione necessarie per i ruoli o le funzionalità installate. In Windows Server 2012 e versioni successive di Windows Server, è possibile chiudere l'aggiunta guidata ruoli e funzionalità durante l'installazione è ancora in corso e visualizzare i risultati dell'installazione o altri messaggi nell'area **notifiche** nella parte superiore della console di Server Manager. Fare clic su di **notifiche** icona del flag per visualizzare ulteriori dettagli sulle installazioni o altre attività in esecuzione in Server Manager.  
  
## <a name="install-roles-role-services-and-features-by-using-windows-powershell-cmdlets"></a>Installare ruoli, servizi ruolo e funzionalità con cmdlet di Windows PowerShell  
I cmdlet di distribuzione Server Manager per Windows PowerShell funzionano in modo analogo all'aggiunta guidata ruoli e funzionalità e alla rimozione guidata ruoli e funzionalità, con una differenza importante. In Windows PowerShell, a differenza dell'aggiunta guidata ruoli e funzionalità, gli strumenti di gestione e gli snap-in per un ruolo non sono inclusi per impostazione predefinita. Per includere gli strumenti di gestione come parte dell'installazione di un ruolo, aggiungere il parametro `IncludeManagementTools` al cmdlet. Se si installa ruoli e funzionalità in un server che esegue l'opzione di installazione Server Core di Windows Server 2012 o versioni successive, è possibile aggiungere gli strumenti di gestione di un ruolo a un'installazione, ma gli strumenti di gestione basata su GUI e snap-in non può essere installato nei server che eseguono l'opzione di installazione Server Core di Windows Server. Solo della riga di comando e gli strumenti di gestione di Windows PowerShell possono essere installati l'opzione di installazione Server Core.  
  
#### <a name="to-install-roles-and-features-by-using-the-install-windowsfeature-cmdlet"></a>Per installare ruoli e funzionalità mediante il cmdlet Install-WindowsFeature  
  
1. Per aprire una sessione di Windows PowerShell con diritti utente elevati, eseguire una di queste operazioni.  
  
   > [!NOTE]  
   > Se si installano ruoli e funzionalità in un server remoto, non è necessario eseguire Windows PowerShell con diritti utente elevati.  
  
   -   Nel desktop di Windows fare clic con il pulsante destro del mouse su **Windows PowerShell** nella barra delle applicazioni e scegliere **Esegui come amministratore**.  
  
   -   In Windows **avviare** schermata, fare clic sul riquadro per Windows PowerShell e quindi nella barra dell'app, fare clic su **Esegui come amministratore**.  
  
2. Digitare **Get-WindowsFeature** e quindi premere **INVIO** per visualizzare un elenco dei ruoli e delle funzionalità disponibili e installati nel server locale. Se il computer locale non è un server o se si desiderano informazioni su un server remoto, eseguire **Get-WindowsFeature-computername <** <em>computer_name</em> **>** , in cui *computer_name* rappresenta il nome di un computer remoto che esegue Windows Server 2016. I risultati del cmdlet contengono i nomi di comando dei ruoli e funzionalità che si aggiungono al cmdlet nel passaggio 4.  
  
   > [!NOTE]  
   > In Windows PowerShell 3.0 e versioni successive di Windows PowerShell, non è necessario per importare il modulo di cmdlet di Server Manager nella sessione di Windows PowerShell prima di eseguire i cmdlet che fanno parte del modulo. Un modulo viene importato automaticamente alla prima esecuzione di un cmdlet che fa parte del modulo. Inoltre, i cmdlet di Windows PowerShell né i nomi di funzionalità utilizzati con i cmdlet di maiuscole e minuscole.  
  
3. Digitare **Get-Help Install-WindowsFeature**e quindi premere **invio** per visualizzare la sintassi e i parametri accettati per il cmdlet `Install-WindowsFeature`.  
  
4. digitare quanto segue, quindi premere **invio**, dove *feature_name* rappresenta il nome del comando di un ruolo o di una funzionalità che si desidera installare (ottenuto nel passaggio 2) e *computer_name* rappresenta un computer remoto in cui si desidera installare ruoli e funzionalità. Usare virgole per separare più valori per *nome_funzionalità*. Il parametro `Restart` riavvia automaticamente il server di destinazione se richiesto dall'installazione del ruolo o della funzionalità.  
  
   ```  
   Install-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
   ```  
  
   Per installare ruoli e funzionalità in un disco rigido virtuale offline, aggiungere il parametro `computerName` e il parametro `VHD` . Se non si aggiunge il parametro `computerName`, il cmdlet parte dal presupposto che il computer locale sia montato per accedere al disco rigido virtuale. Il parametro `computerName` contiene il nome del server in cui montare il disco rigido virtuale e il parametro `VHD` contiene il percorso del file VHD nel server specificato.  
  
   > [!NOTE]  
   > È necessario aggiungere il `computerName` parametro se si esegue il cmdlet da un computer che esegue un sistema operativo client Windows.  
   >   
   > Per installare ruoli e funzionalità in dischi rigidi virtuali offline, i dischi rigidi virtuali di destinazione devono soddisfare i requisiti seguenti.  
   >   
   > -   I dischi rigidi virtuali devono essere in esecuzione la versione di Windows Server che corrisponde alla versione di Server Manager è in esecuzione. Vedere la nota all'inizio dell' [installazione dei ruoli, dei servizi ruolo e delle funzionalità con l'aggiunta guidata ruoli e funzionalità](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard).  
   > -   I dischi rigidi virtuali non possono contenere più di un volume o una partizione di sistema.  
   > -   La cartella condivisa di rete in cui è archiviato il file VHD deve concedere i dritti di accesso seguenti all'account computer (o sistema locale) del server selezionato per montare il disco rigido virtuale. L'accesso con account solo utente non è sufficiente. La condivisione può concedere le autorizzazioni **Lettura** e **Scrittura** al gruppo **Everyone** per consentire l'accesso al disco rigido virtuale, ma si tratta di un'impostazione sconsigliata per motivi di sicurezza.  
   >   
   >     -   Accesso in **Lettura/Scrittura** alla finestra di dialogo **Condivisione file**.  
   >     -   **Controllo completo** accesso il **sicurezza** scheda, file o cartella **proprietà** la finestra di dialogo.  
  
   ```  
   Install-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
   ```  
  
   **Esempio:** Il cmdlet seguente installa il ruolo Servizi di dominio Active Directory e la funzionalità di gestione di Criteri di gruppo su un server remoto, ContosoDC1. Vengono aggiunti strumenti di gestione e snap-in usando il parametro `IncludeManagementTools`, e il server di destinazione viene riavviato automaticamente, se richiesto dal processo di installazione.  
  
   ```  
   Install-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
   ```  
  
5. Al termine dell'installazione, verificare l'installazione aprendo il **tutti i server** pagina in Server Manager, selezionando un server in cui è installato i ruoli e funzionalità e visualizzando il **ruoli e funzionalità** riquadro della pagina per il server selezionato. È anche possibile eseguire il cmdlet `Get-WindowsFeature` indirizzato al server selezionato (Get-WindowsFeature-ComputerName <*computer_name*>) per visualizzare un elenco di ruoli e funzionalità installati nel server.  
  
## <a name="remove-roles-role-services-and-features-by-using-the-remove-roles-and-features-wizard"></a>Rimuovere ruoli, servizi ruolo e funzionalità con la rimozione guidata ruoli e funzionalità  
In qualità di amministratore per disinstallare ruoli, servizi ruolo e funzionalità, è necessario essere connessi a un server. Se si è connessi al computer locale con un account che non dispone di diritti di amministratore nel server di destinazione della disinstallazione, fare clic con il pulsante destro del mouse sul server di destinazione nel riquadro **Server** e quindi scegliere **Account di gestione** per specificare un account con diritti di amministratore. Il server in cui si desidera montare un disco rigido virtuale offline deve essere aggiunto a Server Manager ed è necessario disporre di diritti di amministratore per tale server.  
  
#### <a name="to-remove-roles-and-features-by-using-the-remove-roles-and-features-wizard"></a>Per rimuovere ruoli e funzionalità con la rimozione guidata ruoli e funzionalità  
  
1.  Se Server Manager è già aperto, andare al passaggio successivo. Se Server Manager non è aperto, aprirlo in uno dei modi seguenti.  
  
    -   Nel desktop di Windows avviare Server Manager facendo clic su **Server Manager** nella barra delle applicazioni di Windows.  
  
    -   Nella schermata **Start** di Windows fare clic sul riquadro **Server Manager**.  
  
2.  Scegliere **Rimuovi ruoli e funzionalità** dal menu **Gestione**.  
  
3.  Nella pagina **Prima di iniziare** verificare che sia stata eseguita la preparazione per la rimozione di ruoli o funzioni da un server. Fare clic su **Avanti**.  
  
4.  Nella pagina **Selezione server di destinazione** selezionare un server dal pool di server oppure selezionare un disco rigido virtuale offline. Per selezionare un disco rigido virtuale offline, selezionare innanzitutto il server in cui montare il disco rigido virtuale, quindi selezionare il file del disco rigido virtuale.  
  
    > [!NOTE]  
    > La cartella condivisa di rete in cui è archiviato il file VHD deve concedere i dritti di accesso seguenti all'account computer (o sistema locale) del server selezionato per montare il disco rigido virtuale. L'accesso con account solo utente non è sufficiente. La condivisione può concedere le autorizzazioni **Lettura** e **Scrittura** al gruppo **Everyone** per consentire l'accesso al disco rigido virtuale, ma si tratta di un'impostazione sconsigliata per motivi di sicurezza.  
    >   
    > -   Accesso in **Lettura/Scrittura** alla finestra di dialogo **Condivisione file**.  
    > -   Accesso **Controllo completo** alla scheda **Sicurezza**, finestra di dialogo **Proprietà** per file o cartelle.  
  
    Per informazioni su come aggiungere server al pool di server, vedere [aggiungere server a Server Manager](add-servers-to-server-manager.md). Dopo aver selezionato il server di destinazione, fare clic su **Avanti**.  
  
    > [!NOTE]  
    > È possibile utilizzare la rimozione guidata ruoli e funzionalità per rimuovere ruoli e funzionalità dai server che eseguono la stessa versione di Windows Server che supporta la versione di Server Manager in uso. Non è possibile rimuovere ruoli, servizi ruolo o funzionalità dai server che eseguono Windows Server 2016, se si esegue Server Manager in Windows Server 2012 R2, Windows Server 2012 o Windows 8. Non è possibile utilizzare la rimozione guidata ruoli e funzionalità per rimuovere ruoli e funzionalità dai server che eseguono Windows Server 2008 o Windows Server 2008 R2.  
  
5.  Selezionare i ruoli, selezionare i servizi ruolo per il ruolo se applicabile e quindi fare clic su **Avanti** per selezionare le funzionalità.  
  
    Quando si procede, la rimozione guidata ruoli e funzionalità richiede automaticamente di rimuovere eventuali ruoli, servizi ruolo o funzionalità che non possono essere eseguiti senza i ruoli o le funzionalità che si stanno rimuovendo.  
  
    Inoltre, è possibile scegliere di rimuovere gli strumenti di gestione e gli snap-in per i ruoli nel server di destinazione. Per impostazione predefinita, nella rimozione guidata ruoli e funzionalità, gli strumenti di gestione sono selezionati per la rimozione. È possibile lasciare strumenti di gestione e snap-in se si prevede di usare il server selezionato per la gestione del ruolo su altri server remoti.  
  
6.  Nella pagina **Conferma selezioni per la rimozione** rivedere le selezioni di ruolo, funzionalità e server. Se si è pronti per rimuovere i ruoli o le funzionalità, fare clic su **Rimuovi**.  
  
7.  Dopo aver fatto clic su **Rimuovi**, nella pagina **stato rimozione** vengono visualizzati lo stato della rimozione, i risultati e messaggi quali avvisi, errori o procedure di configurazione successive alla rimozione necessarie, ad esempio il riavvio del server di destinazione. In Windows Server 2012 e versioni successive di Windows Server, è possibile chiudere la rimozione guidata ruoli e funzionalità mentre la rimozione è ancora in corso e visualizzare i risultati della rimozione o altri messaggi nell'area **notifiche** nella parte superiore della console di Server Manager. Fare clic su di **notifiche** flag per visualizzare ulteriori dettagli sulle rimozioni o altre attività in esecuzione in Server Manager.  
  
## <a name="remove-roles-role-services-and-features-by-using-windows-powershell-cmdlets"></a>Rimuovere ruoli, servizi ruolo e funzionalità con cmdlet di Windows PowerShell  
I cmdlet di distribuzione Server Manager per Windows PowerShell funzionano in modo analogo alla rimozione guidata ruoli e funzionalità basata su GUI, con una differenza importante. In Windows PowerShell, a differenza della rimozione guidata ruoli e funzionalità, gli strumenti di gestione e gli snap-in per un ruolo non sono rimossi per impostazione predefinita. Per includere gli strumenti di gestione come parte della rimozione di un ruolo, aggiungere il parametro `IncludeManagementTools` al cmdlet. Se si sta disinstallando funzionalità, ruoli e funzionalità da un server che esegue l'opzione di installazione Server Core di Windows Server 2012 o versione successiva di Windows Server, questo viene rimosso il parametro della riga di comando e gli strumenti di gestione di Windows PowerShell per i ruoli specificati.  
  
#### <a name="to-remove-roles-and-features-by-using-the-uninstall-windowsfeature-cmdlet"></a>Per rimuovere ruoli e funzionalità mediante il cmdlet Uninstall-WindowsFeature  
  
1. Per aprire una sessione di Windows PowerShell con diritti utente elevati, eseguire una di queste operazioni.  
  
   > [!NOTE]  
   > Se si disinstallano ruoli e funzionalità da un server remoto, non è necessario eseguire Windows PowerShell con diritti utente elevati.  
  
   -   Nel desktop di Windows fare clic con il pulsante destro del mouse su **Windows PowerShell** nella barra delle applicazioni e scegliere **Esegui come amministratore**.  
  
   -   Nella schermata **Start** di Windows fare clic con il pulsante destro del mouse sul riquadro Windows PowerShell e quindi, nella barra dell'app, fare clic su **Esegui come amministratore**.  
  
2. Digitare **Get-WindowsFeature** e quindi premere **INVIO** per visualizzare un elenco dei ruoli e delle funzionalità disponibili e installati nel server locale. Se il computer locale non è un server o se si desiderano informazioni su un server remoto, eseguire **Get-WindowsFeature-computername <** <em>computer_name</em> **>** , in cui *computer_name* rappresenta il nome di un computer remoto che esegue Windows Server 2016. I risultati del cmdlet contengono i nomi di comando dei ruoli e funzionalità che si aggiungono al cmdlet nel passaggio 4.  
  
   > [!NOTE]  
   > In Windows PowerShell 3.0 e versioni successive di Windows PowerShell, non è necessario per importare il modulo di cmdlet di Server Manager nella sessione di Windows PowerShell prima di eseguire i cmdlet che fanno parte del modulo. Un modulo viene importato automaticamente alla prima esecuzione di un cmdlet che fa parte del modulo. Inoltre, i cmdlet di Windows PowerShell né i nomi di funzionalità utilizzati con i cmdlet di maiuscole e minuscole.  
  
3. Digitare **Get-Help Uninstall-WindowsFeature**e quindi premere **invio** per visualizzare la sintassi e i parametri accettati per il cmdlet `Uninstall-WindowsFeature`.  
  
4. Digitare quanto segue, quindi premere **INVIO**, dove *nome_funzionalità* rappresenta il nome del comando di un ruolo o di una funzionalità che si desidera installare (come descritto nel passaggio 2) e *computer_name* rappresenta il computer remoto da cui rimuovere ruoli e funzionalità. Usare virgole per separare più valori per *nome_funzionalità*. Il parametro `Restart` riavvia automaticamente il server di destinazione se richiesto dalla rimozione del ruolo o della funzionalità.  
  
   ```  
   Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
   ```  
  
   Per rimuovere ruoli e funzionalità da un disco rigido virtuale offline, aggiungere il parametro `computerName` e il parametro `VHD`. Se non si aggiunge il parametro `computerName`, il cmdlet parte dal presupposto che il computer locale sia montato per accedere al disco rigido virtuale. Il parametro `computerName` contiene il nome del server in cui montare il disco rigido virtuale e il parametro `VHD` contiene il percorso del file VHD nel server specificato.  
  
   > [!NOTE]  
   > È necessario aggiungere il `computerName` parametro se si esegue il cmdlet da un computer che esegue un sistema operativo client Windows.  
   >   
   > La cartella condivisa di rete in cui è archiviato il file VHD deve concedere i dritti di accesso seguenti all'account computer (o sistema locale) del server selezionato per montare il disco rigido virtuale. L'accesso con account solo utente non è sufficiente. La condivisione può concedere le autorizzazioni **Lettura** e **Scrittura** al gruppo **Everyone** per consentire l'accesso al disco rigido virtuale, ma si tratta di un'impostazione sconsigliata per motivi di sicurezza.  
   >   
   > -   Accesso in **Lettura/Scrittura** alla finestra di dialogo **Condivisione file**.  
   > -   **Controllo completo** accesso il **sicurezza** scheda, file o cartella **proprietà** la finestra di dialogo.  
  
   ```  
   Uninstall-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
   ```  
  
   **Esempio:** Il cmdlet seguente rimuove il ruolo Servizi di dominio Active Directory e la funzionalità di gestione Criteri di gruppo da un server remoto, ContosoDC1. Vengono anche rimossi strumenti di gestione e snap-in, e il server di destinazione viene riavviato automaticamente, se richiesto dal processo di rimozione.  
  
   ```  
   Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
   ```  
  
5. Termine della rimozione, verificare che i ruoli e funzionalità sono stati effettivamente rimossi aprendo la **tutti i server** pagina in Server Manager, selezionando il server da cui sono stati rimossi ruoli e funzionalità e visualizzando il **ruoli e funzionalità** riquadro della pagina per il server selezionato. È anche possibile eseguire il cmdlet `Get-WindowsFeature` indirizzato al server selezionato (Get-WindowsFeature-ComputerName <*computer_name*>) per visualizzare un elenco di ruoli e funzionalità installati nel server.  
  
## <a name="install-roles-and-features-on-multiple-servers-by-running-a-windows-powershell-script"></a>Installare ruoli e funzionalità in più server eseguendo uno script di Windows PowerShell  
Sebbene non sia possibile utilizzare l'aggiunta guidata ruoli e funzionalità per installare ruoli, servizi ruolo e funzionalità in più di un server di destinazione in una singola sessione della procedura guidata, è possibile utilizzare uno script di Windows PowerShell per installare ruoli, servizi ruolo e funzionalità in più server di destinazione gestiti tramite Server Manager. Lo script utilizzato per eseguire la distribuzione batch, quando viene chiamato questo processo, punta a un file di configurazione XML che è possibile creare facilmente utilizzando l'aggiunta guidata ruoli e funzionalità e facendo clic su **Esporta impostazioni di configurazione** dopo aver passato la procedura guidata alla pagina **Conferma selezioni** per l'installazione dell'aggiunta guidata ruoli e funzionalità.  
  
> [!IMPORTANT]  
> Tutti i server di destinazione specificati nello script devono essere in esecuzione la versione di Windows Server che corrisponde alla versione di Server Manager sono in esecuzione nel computer locale. Ad esempio, se si eseguono Server Manager in Windows 10, è possibile installare ruoli, servizi ruolo e funzionalità in server che eseguono Windows Server 2016. Se vengono aggiunti strumenti di gestione basata su GUI per l'installazione, il processo di installazione converte automaticamente i server di destinazione che eseguono l'opzione di installazione Server Core di Windows Server per l'opzione di installazione completa (server con un'interfaccia utente grafica completa, anche nota come esecuzione Shell grafica Server).  
>   
> Lo script fornito in questa sezione è riportato un esempio di come eseguire la distribuzione in batch utilizzando il `Install-WindowsFeature` cmdlet e uno script Windows PowerShell. Esistono altri script e metodi possibili per eseguire la distribuzione in batch in più server. Per cercare o fornire altri script per la distribuzione di ruoli e funzionalità, eseguire una ricerca nell' [archivio centrale di script](https://gallery.technet.microsoft.com/ScriptCenter).  
  
#### <a name="to-install-roles-and-features-on-multiple-servers"></a>Per installare ruoli e funzionalità in più server  
  
1.  Se non è già stato fatto, creare un file di configurazione XML che contiene i ruoli, servizi ruolo e funzionalità che si desidera installare in più server. Per creare questo file di configurazione, è possibile eseguire l'aggiunta guidata ruoli e funzionalità, selezionare i ruoli, i servizi ruolo e le funzionalità desiderate, quindi fare clic su **Esporta impostazioni di configurazione** dopo aver passato la procedura guidata alla pagina **Conferma selezioni** per l'installazione. Salvare il file di configurazione in una posizione appropriata. Non è necessario fare clic su **Installa** o completare la procedura guidata, se viene eseguita esclusivamente per creare un file di configurazione.  
  
2.  Per aprire una sessione di Windows PowerShell con diritti utente elevati, eseguire una di queste operazioni.  
  
    -   Nel desktop di Windows fare clic con il pulsante destro del mouse su **Windows PowerShell** nella barra delle applicazioni e scegliere **Esegui come amministratore**.  
  
    -   Nella schermata **Start** di Windows fare clic con il pulsante destro del mouse sul riquadro Windows PowerShell e quindi, nella barra dell'app, fare clic su **Esegui come amministratore**.  
  
3.  Copiare e incollare lo script seguente nella sessione di Windows PowerShell.  
  
    ```  
    function Invoke-WindowsFeatureBatchDeployment {  
        param (  
            [parameter(mandatory)]  
            [string[]] $computerNames,  
            [parameter(mandatory)]  
            [string] $ConfigurationFilepath  
        )  
  
        # Deploy the features on multiple computers simultaneously.  
        $jobs = @()  
        foreach($computerName in $computerNames) {  
            $jobs += start-Job -Command {  
                Install-WindowsFeature -ConfigurationFilepath $using:ConfigurationFilepath -computerName $using:computerName -Restart  
            }   
        }  
  
        Receive-Job -Job $jobs -Wait | select-Object Success, RestartNeeded, exitCode, FeatureResult  
    }  
    ```  
  
    I server di destinazione vengono riavviati automaticamente se necessario per i ruoli e le funzionalità selezionate.  
  
4.  Eseguire la funzione come segue:  
  
    1.  Creare una variabile in cui archiviare i nomi dei computer di destinazione, separati da virgole. Nell'esempio seguente, nella variabile `$ServerNames` sono archiviati i nomi dei server di destinazione *Contoso_01* e *Contoso_02*. Premere **INVIO**.  
  
        ```  
        # Sample Invocation  
        $ServerNames = 'Contoso_01','Contoso_02'  
        Invoke-WindowsFeatureBatchDeployment -computerNames $ServerNames -ConfigurationFilepath C:\Users\sampleuser\Desktop\DeploymentConfigTemplate.xml  
        ```  
  
    2.  Per eseguire la funzione, digitare il comando seguente e quindi premere **INVIO**, dove `$ServerNames` è un esempio della variabile creata nel passaggio precedente e *C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml* è un esempio di un percorso del file di configurazione creato nel passaggio 1.  
  
        **Invoke-WindowsFeatureBatchDeployment-computerNames $ServerNames-ConfigurationFilepath C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml**  
  
5.  Al termine dell'installazione, verificare l'installazione aprendo il **tutti i server** pagina in Server Manager, selezionando un server in cui è installato i ruoli e funzionalità e visualizzando il **ruoli e funzionalità** riquadro della pagina per il server selezionato. È inoltre possibile eseguire il cmdlet `Get-WindowsFeature` destinato a un server specifico (`Get-WindowsFeature -computerName` <*computer_name*>) per visualizzare un elenco di ruoli e funzionalità installati nel server.  
  
## <a name="install-net-framework-35-and-other-features-on-demand"></a>Installare .NET Framework 3.5 e altre funzionalità su richiesta  
a partire da Windows Server 2012 e Windows 8, i file di funzionalità per .NET Framework 3,5 (che include .NET Framework 2,0 e .NET Framework 3,0) non sono disponibili nel computer locale per impostazione predefinita. I file sono stati rimossi. I file per le funzionalità che sono stati rimossi in una configurazione Funzionalità su richiesta e i file delle funzionalità per .NET Framework 3.5 sono disponibili tramite Windows Update. Per impostazione predefinita, se i file di funzionalità non sono disponibili nel server di destinazione che esegue Windows Server 2012 o versioni successive, il processo di installazione cerca i file mancanti connettendosi a Windows Update. È possibile sostituire il comportamento predefinito configurando un'impostazione di Criteri di gruppo o specificando un percorso di origine alternativo durante l'installazione, indipendentemente dal fatto che si stia installando utilizzando l'aggiunta guidata ruoli e funzionalità GUI o una riga di comando.  
  
È possibile installare .NET Framework 3.5 in uno dei modi seguenti.  
  
-   Fare riferimento all'argomento [Per installare .NET Framework 3.5 mediante il cmdlet Install-WindowsFeature](#to-install-net-framework-35-by-running-the-install-windowsfeature-cmdlet) per aggiungere il parametro `Source` e specificare un'origine da cui recuperare i file di funzionalità di .NET Framework 3.5. Se non si aggiunge il parametro `Source`, il processo di installazione determina innanzitutto se il percorso ai file di funzionalità è stato specificato da impostazioni di Criteri di gruppo, e in caso contrario, cerca i file di funzionalità mancanti tramite Windows Update.  
  
-   Utilizzare [per installare .NET Framework 3,5 utilizzando l'aggiunta guidata ruoli e funzionalità](#to-install-net-framework-35-by-using-the-add-roles-and-features-wizard) per specificare un percorso del file di origine alternativo nella pagina **Conferma opzioni di installazione** dell'aggiunta guidata ruoli e funzionalità.  
  
-   Fare riferimento all'argomento [Per installare .NET Framework 3.5 utilizzando DISM](#to-install-net-framework-35-by-using-dism) per recuperare i file da Windows Update per impostazione predefinita oppure specificando un percorso di origine per il supporto di installazione.  
  
[Configurare le origini alternative per i file di funzionalità in Criteri di gruppo](#configure-alternate-sources-for-feature-files-in-group-policy) per .NET Framework 3.5 o altre funzionalità, se i file di funzionalità non sono presenti nel computer locale.  
  
> [!IMPORTANT]  
> Quando si installano i file di funzionalità da un'origine remota, il percorso di origine o la condivisione file deve concedere autorizzazioni di **Lettura** al gruppo **Everyone** (non consigliato per motivi di sicurezza) o all'account del computer (sistema locale) del server di destinazione. La concessione dell'accesso all'account utente non è sufficiente.  
>   
> I server appartenenti a gruppi di lavoro non possono accedere a condivisioni file esterne, anche se l'account computer del server del gruppo di lavoro dispone delle autorizzazioni **Lettura** per la condivisione esterna. Tra le posizioni di origine alternative valide per i server di gruppi di lavoro sono inclusi supporti di installazione, Windows Update e file VHD o WIM archiviati nel server del gruppo di lavoro locale.  
>   
> È possibile specificare un file WIM come origine del file di funzionalità alternativo quando si installano ruoli, servizi ruolo e funzionalità in un server fisico in esecuzione. Il percorso di origine per un file WIM dovrebbe essere nel formato seguente, con il prefisso **WIM** e l'indice della posizione dei file della funzionalità come suffisso: **WIM:e:\sources\install.wim:4**. È tuttavia possibile utilizzare un file WIM direttamente come origine per l'installazione di ruoli, servizi ruolo e funzionalità di un disco rigido Virtuale offline. è necessario montare il disco rigido Virtuale offline e puntare al percorso di montaggio per i file di origine oppure puntare a una cartella che contiene una copia del contenuto del file WIM.  
  
### <a name="to-install-net-framework-35-by-running-the-install-windowsfeature-cmdlet"></a>Per installare .NET Framework 3.5 mediante il cmdlet Install-WindowsFeature  
  
1.  Per aprire una sessione di Windows PowerShell con diritti utente elevati, eseguire una di queste operazioni.  
  
    > [!NOTE]  
    > Se si installano ruoli e funzionalità da un server remoto, non è necessario eseguire Windows PowerShell con diritti utente elevati.  
  
    -   Nel desktop di Windows fare clic con il pulsante destro del mouse su **Windows PowerShell** nella barra delle applicazioni e scegliere **Esegui come amministratore**.  
  
    -   Nella schermata **Start** di Windows fare clic con il pulsante destro del mouse sul riquadro Windows PowerShell e quindi, nella barra dell'app, fare clic su **Esegui come amministratore**.  
  
    -   In un server che esegue l'opzione di installazione dei componenti di base del server di Windows Server 2012 R2 o Windows Server 2012, digitare **PowerShell** in un prompt dei comandi, quindi premere **invio**.  
  
2.  digitare il comando seguente e quindi premere **invio**. Nell'esempio seguente i file di origine sono posizionati in un archivio affiancato (abbreviato in **SxS**) in un supporto di installazione nell'unità D.  
  
    ```  
    Install-WindowsFeature NET-Framework-Core -Source D:\Sources\SxS  
    ```  
  
    Se si vuole che il comando usi Windows Update come origine per i file di funzionalità mancanti o se è già stata configurata un'origine predefinita mediante Criteri di gruppo, non è necessario aggiungere il parametro `Source` a meno che non si intenda specificare un'origine diversa.  
  
### <a name="to-install-net-framework-35-by-using-the-add-roles-and-features-wizard"></a>Per installare .NET Framework 3,5 usando l'aggiunta guidata ruoli e funzionalità  
  
1. Nel menu **gestisci** Server Manager fare clic su **Aggiungi ruoli e funzionalità**.  
  
2. Selezionare un server di destinazione che esegue Windows Server 2016.  
  
3. Nella pagina **Selezione funzionalità** dell'aggiunta guidata ruoli e funzionalità selezionare **.NET Framework 3,5**.  
  
4. Se le impostazioni di Criteri di gruppo del computer locale lo consentono, il processo di installazione tenta di recuperare i file di funzionalità mancanti tramite Windows Update. Fare clic su **Installa**. Non è necessario procedere al passaggio successivo.  
  
   Se Criteri di gruppo impostazioni non consentono o si desidera utilizzare un'altra origine per i file di funzionalità .NET Framework 3,5, nella pagina **Conferma selezioni** per l'installazione della procedura guidata fare clic su **specificare un percorso di origine alternativo**.  
  
5. Immettere un percorso a un archivio affiancato (denominato **SxS**) nel supporto di installazione o a un file WIM. Nell'esempio seguente il supporto di installazione è posizionato nell'unità D.  
  
   **\\ D:\Sources\SxS**  
  
   Per specificare un file WIM, aggiungere un prefisso **WIM:** e aggiungere l'indice dell'immagine da usare nel file WIM come un suffisso, come mostrato nell'esempio seguente.  
  
   **Wim:\\\\** <em>server_name</em> **\share\install.wim: 3**  
  
6. Fare clic su **OK** e quindi su **Installa**.  
  
### <a name="to-install-net-framework-35-by-using-dism"></a>Per installare .NET Framework 3.5 utilizzando DISM  
  
1.  Per aprire una sessione di Windows PowerShell con diritti utente elevati, eseguire una di queste operazioni.  
  
    > [!NOTE]  
    > Se si installano ruoli e funzionalità da un server remoto, non è necessario eseguire Windows PowerShell con diritti utente elevati.  
  
    -   Nel desktop di Windows fare clic con il pulsante destro del mouse su **Windows PowerShell** nella barra delle applicazioni e scegliere **Esegui come amministratore**.  
  
    -   In Windows **avviare** schermata, fare clic sul riquadro di Windows PowerShell e quindi nella barra dell'app, fare clic su **Esegui come amministratore**.  
  
    -   In un server che esegue l'opzione di installazione dei componenti di base del server, digitare **PowerShell** in un prompt dei comandi, quindi premere **invio**.  
  
2.  Eseguire uno dei comandi DISM seguenti.  
  
    -   Se il computer ha accesso a Windows Update oppure è già stato configurato un percorso del file di origine predefinito in Criteri di gruppo, eseguire il comando seguente.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All  
        ```  
  
    -   Se il computer ha accesso al supporto di installazione, eseguire un comando simile al seguente. Nell'esempio seguente il supporto di installazione del sistema operativo è posizionato nell'unità D. Il parametro `LimitAccess` impedisce che il comando tenti di contattare Windows Update o un server che esegue WSUS.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All /LimitAccess /Source:d:\sources\sxs  
        ```  
  
    > [!NOTE]  
    > Per il comando DISM viene fatta distinzione tra maiuscole e minuscole.  
  
### <a name="configure-alternate-sources-for-feature-files-in-group-policy"></a>Configurare le origini alternative per i file di funzionalità in Criteri di gruppo  
L'impostazione di Criteri di gruppo descritta in questa sezione specifica i percorsi di origine autorizzati per i file .NET Framework 3.5 e altri file di funzionalità rimossi nell'ambito di una configurazione Funzionalità su richiesta. L'impostazione dei criteri **specifica le impostazioni per l'installazione e il ripristino dei componenti facoltativi** si trova nella cartella **configurazione computer\modelli amministrativi\Sistema** del Console Gestione Criteri di gruppo o dell'Editor criteri di gruppo locale.  
  
> [!NOTE]  
> Per modificare le impostazioni di Criteri di gruppo nel computer locale è necessario essere un membro del gruppo Administrators. Se le impostazioni di Criteri di gruppo per il computer che si vuole gestire sono controllate a livello di dominio, per modificare le impostazioni di Criteri di gruppo è necessario essere un membro del gruppo Domain Administrators.  
  
##### <a name="to-configure-a-default-alternate-source-path-in-group-policy"></a>Per configurare un percorso di origine alternativo predefinito in Criteri di gruppo  
  
1. In Editor Criteri di gruppo locale o Console Gestione Criteri di gruppo aprire l'impostazione dei criteri seguente.  
  
   **Configurazione computer\Modelli Amministrativi\sistema\specifica le impostazioni per l'installazione e il ripristino dei componenti facoltativi**  
  
2. Sselect **abilitato** per abilitare l'impostazione dei criteri, se non è già abilitata.  
  
3. Nella casella di testo **Percorso file di origine alternativo** dell'area **Opzioni** specificare un percorso completo a una cartella condivisa o a un file WIM. Per specificare un file WIM come percorso file di origine alternativo, aggiungere il prefisso **WIM:** al percorso, quindi aggiungere l'indice dell'immagine da usare nel file WIM come suffisso. Di seguito sono illustrati alcuni esempi di valori che è possibile specificare.  
  
   - percorso di una cartella condivisa: **\\\\** <em>server_name</em> **\share\\** <em>Folder_Name</em>  
  
   - percorso di un file WIM, in cui **3** rappresenta l'indice dell'immagine in cui sono stati trovati i file di funzionalità: **WIM:\\\\** <em>server_name</em> **\share\install.wim: 3**  
  
4. Se non si desidera che i computer controllati da questa impostazione dei criteri cerchino i file di funzionalità mancanti in Windows Update, selezionare **non tentare mai di scaricare il payload da Windows Update**.  
  
5. Se i computer controllati da questa impostazione di criterio in genere ricevono aggiornamenti tramite WSUS ma per trovare i file di funzionalità mancanti si preferisce usare Windows Update, selezionare **Contatta direttamente Windows Update invece di Windows Server Update Services (WSUS) per scaricare il contenuto di ripristino**.  
  
6. Al termine della modifica dell'impostazione di criterio, fare clic su **OK** e chiudere l'Editor Criteri di gruppo.  
  
## <a name="see-also"></a>Vedi anche  
[Opzioni di installazione di Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573)  
[Considerazioni sulla distribuzione di Microsoft .NET Framework 3,5](https://go.microsoft.com/fwlink/p/?LinkId=248869)  
[Come abilitare o disabilitare le funzionalità di Windows](https://go.microsoft.com/fwlink/p/?LinkId=246552)  
  


