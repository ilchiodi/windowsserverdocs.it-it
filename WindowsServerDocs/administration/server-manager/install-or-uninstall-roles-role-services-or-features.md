---
title: Installazione o disinstallazione di ruoli, servizi ruolo o funzionalità
description: Server Manager
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04f16d84-45c2-4771-84c1-1cc973d0ee02
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32d356b3ae70b7b15f23a40247e73b4b8f61c3db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822372"
---
# <a name="install-or-uninstall-roles-role-services-or-features"></a>Installazione o disinstallazione di ruoli, servizi ruolo o funzionalità

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In Windows Server, la console di Server Manager e cmdlet di Windows PowerShell per Server di gestione consentire l'installazione di ruoli e funzionalità in server locali o remoti oppure dischi rigidi virtuali (VHD) offline. È possibile installare più ruoli e funzionalità in un server remoto unico o disco rigido virtuale offline in un singolo Aggiungi ruoli e funzionalità guidata o sessione di Windows PowerShell.  
  
> [!IMPORTANT]  
> Server Manager può essere utilizzata per gestire una versione più recente del sistema operativo Windows Server. Server Manager in esecuzione in Windows Server 2012 R2 o Windows 8.1 può essere utilizzata per installare ruoli, servizi ruolo e funzionalità in server che eseguono Windows Server 2016.  
  
In qualità di amministratore per installare o disinstallare ruoli, servizi ruolo e funzionalità, è necessario essere connessi a un server. Se si è connessi al computer locale con un account che non dispone di diritti di amministratore nel server di destinazione, fare clic con il pulsante destro del mouse sul server di destinazione nel riquadro **Server** e quindi scegliere **Account di gestione** per specificare un account con diritti di amministratore. Il server in cui si desidera montare un disco rigido virtuale offline deve essere aggiunto a Server Manager ed è necessario disporre di diritti di amministratore per tale server.  
  
Per ulteriori informazioni su quali sono i ruoli, servizi ruolo e funzionalità, vedere [ruoli, servizi ruolo e funzionalità](https://go.microsoft.com/fwlink/p/?LinkId=239558).  
  
In questo argomento sono incluse le sezioni seguenti.  
  
-   [Installare ruoli, servizi ruolo e funzionalità mediante l'aggiunta guidata ruoli e funzionalità](#BKMK_installarfw)  
  
-   [Installare ruoli, servizi ruolo e funzionalità con cmdlet di Windows PowerShell](#BKMK_installwps)  
  
-   [Rimuovere ruoli, servizi ruolo e funzionalità tramite la rimozione guidata ruoli e funzionalità](#BKMK_removerrfw)  
  
-   [Rimuovere ruoli, servizi ruolo e funzionalità con cmdlet di Windows PowerShell](#BKMK_removewps)  
  
-   [Installare ruoli e funzionalità in più server eseguendo uno script di Windows PowerShell](#BKMK_batch)  
  
-   [Installare .NET Framework 3.5 e altre funzionalità su richiesta](#BKMK_FoD)  
  
## <a name="BKMK_installarfw"></a>Installare ruoli, servizi ruolo e funzionalità mediante l'aggiunta guidata ruoli e funzionalità  
In una singola sessione nel Aggiungi ruoli e funzionalità guidata, è possibile installare ruoli, servizi ruolo e funzionalità nel server locale, un server remoto che è stato aggiunto a Server Manager o un disco rigido virtuale offline. Per ulteriori informazioni su come aggiungere un server a Server Manager per gestire, vedere [aggiungere server a Server Manager](add-servers-to-server-manager.md).  
  
> [!NOTE]  
> Se si eseguono Server Manager in Windows Server 2016 o Windows 10, è possibile utilizzare l'Aggiungi ruoli e funzionalità guidata per installare ruoli e funzionalità solo nei server e dischi rigidi virtuali offline che eseguono Windows Server 2016.  
  
#### <a name="to-install-roles-and-features-by-using-the-add-roles-and-features-wizard"></a>Per installare ruoli e funzionalità tramite l'aggiunta guidata ruoli e funzionalità  
  
1.  Se Server Manager è già aperto, andare al passaggio successivo. Se Server Manager non è aperto, aprirlo in uno dei modi seguenti.  
  
    -   Nel desktop di Windows avviare Server Manager facendo clic su **Server Manager** nella barra delle applicazioni di Windows.  
  
    -   Nella finestra di Windows **avviare** schermata, fare clic il **Server Manager** riquadro.  
  
2.  Nel **Manage** menu, fare clic su **Aggiungi ruoli e funzionalità**.  
  
3.  Nella pagina **Prima di iniziare** verificare che il server di destinazione e l'ambiente di rete siano preparati per il ruolo e la funzionalità che si desidera installare. Fare clic su **Avanti**.  
  
4.  Nella pagina **Selezione tipo di installazione** scegliere **Installazione basata su ruoli o basata su funzionalità** per installare tutte le parti di ruoli o funzionalità in un singolo server oppure scegliere **Installazione di Servizi Desktop remoto** per installare un'infrastruttura desktop basata su macchine virtuali oppure un'infrastruttura desktop basata su sessioni per Servizi Desktop remoto. L'opzione **Installazione basata su scenario di Servizi Desktop remoto** distribuisce parti logiche di Servizi Desktop remoto attraverso server diversi, come richiesto dagli amministratori. Fare clic su **Avanti**.  
  
5.  Nella pagina **Selezione server di destinazione** scegliere un server dal pool di server oppure selezionare un disco rigido virtuale offline. Per selezionare un disco rigido virtuale offline come server di destinazione, selezionare innanzitutto il server in cui montare il disco rigido virtuale, quindi selezionare il file del disco rigido virtuale. Per informazioni su come aggiungere server al pool di server, vedere [aggiungere server a Server Manager](add-servers-to-server-manager.md). Dopo aver selezionato il server di destinazione, fare clic su **Avanti**.  
  
    > [!NOTE]  
    > Per installare ruoli e funzionalità in dischi rigidi virtuali offline, i dischi rigidi virtuali di destinazione devono soddisfare i requisiti seguenti.  
    >   
    > -   I dischi rigidi virtuali devono essere in esecuzione la versione di Windows Server che corrisponde alla versione di Server Manager è in esecuzione. Vedere la nota riportata all'inizio della [installare ruoli, servizi ruolo e funzionalità mediante l'aggiunta guidata ruoli e funzionalità](#BKMK_installarfw).  
    > -   I dischi rigidi virtuali non possono contenere più di un volume o una partizione di sistema.  
    > -   La cartella condivisa di rete in cui è archiviato il file VHD deve concedere i dritti di accesso seguenti all'account computer (o sistema locale) del server selezionato per montare il disco rigido virtuale. L'accesso con account solo utente non è sufficiente. La condivisione può concedere le autorizzazioni **Lettura** e **Scrittura** al gruppo **Everyone** per consentire l'accesso al disco rigido virtuale, ma si tratta di un'impostazione sconsigliata per motivi di sicurezza.  
    >   
    >     -   Accesso in **Lettura/Scrittura** alla finestra di dialogo **Condivisione file**.  
    >     -   **Controllo completo** accesso il **sicurezza** scheda, file o cartella **proprietà** la finestra di dialogo.  
  
6.  Selezionare i ruoli, selezionare i servizi ruolo per il ruolo se applicabile e quindi fare clic su **Avanti** per selezionare le funzionalità.  
  
    Come si procede, l'aggiunta guidata ruoli e funzionalità informa automaticamente se sono stati rilevati conflitti sul server di destinazione che possono impedire a determinati ruoli o funzionalità dall'installazione o il normale funzionamento. Viene inoltre richiesto di aggiungere eventuali ruoli, servizi ruolo o funzionalità necessari in base ai ruoli o alle funzionalità selezionati.  
  
    Inoltre, se si prevede di gestire il ruolo in modalità remota da un altro server o da un computer basato su client Windows che esegue Strumenti di amministrazione remota del server, è possibile scegliere di non installare strumenti di gestione e snap-in per i ruoli nel server di destinazione. Per impostazione predefinita, in cui l'aggiunta guidata ruoli e funzionalità, gli strumenti di gestione sono selezionati per l'installazione.  
  
7.  Nella pagina **Conferma selezioni per l'installazione** , rivedere le selezioni di ruolo funzionalità e server. Se si è pronti per l'installazione, fare clic su **Installa**.  
  
    È anche possibile esportare le selezioni in un file di configurazione basato su XML che è possibile utilizzare per le installazioni automatiche con Windows PowerShell. Per esportare la configurazione specificata in questa aggiungere ruoli e sessione guidata funzionalità, fare clic su **Esporta impostazioni di configurazione**e quindi salvare il file XML nel percorso desiderato.  
  
    Il comando **Specificare un percorso di origine alternativo** nella pagina **Conferma selezioni per l'installazione** consente di specificare un percorso di origine alternativo per i file necessari all'installazione di ruoli e funzionalità sul server selezionato. In Windows Server 2012 e versioni successive di Windows Server, [funzionalità su richiesta](https://go.microsoft.com/fwlink/p/?LinkID=241573) consentono di ridurre la quantità di spazio su disco utilizzato dal sistema operativo, rimuovendo i file di ruolo e funzionalità dai server gestiti esclusivamente in modalità remota. Se sono stati rimossi file di ruolo e funzionalità da un server mediante il cmdlet `Uninstall-WindowsFeature -remove`, è possibile installarli sul server in futuro specificando un percorso di origine alternativo o una condivisione su cui vengono archiviati i file di ruolo e funzionalità necessari. La condivisione di file o percorso di origine deve concedere **lettura** le autorizzazioni al **tutti gli utenti** (non consigliato per motivi di sicurezza), gruppo o all'account del computer (*dominio* \\ *SERverNAME*$) del server di destinazione; concessione dell'accesso all'account utente non è sufficiente. Per altre informazioni sulle funzionalità su richiesta, vedere [Opzioni di installazione di Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573).  
  
    È possibile specificare un file WIM come origine del file di funzionalità alternativo quando si installano ruoli, servizi ruolo e funzionalità in un server fisico in esecuzione. Il percorso di origine per un file WIM dovrebbe essere nel formato seguente, con **WIM** come un prefisso e l'indice in cui i file di funzionalità come suffisso: **WIM:e:\Sources\Install.wim:4**. È tuttavia possibile utilizzare un file WIM direttamente come origine per l'installazione di ruoli, servizi ruolo e funzionalità di un disco rigido Virtuale offline. è necessario montare il disco rigido Virtuale offline e puntare al percorso di montaggio per i file di origine oppure puntare a una cartella che contiene una copia del contenuto del file WIM.  
  
8.  Dopo aver fatto clic **installare**, il **lo stato dell'installazione** pagina viene visualizzato lo stato dell'installazione, risultati e messaggi quali avvisi, errori o passaggi di configurazione post-installazione sono obbligatorio per i ruoli o funzionalità che è stato installato. In Windows Server 2012 e versioni successive di Windows Server, è possibile chiudere l'aggiunta guidata ruoli e funzionalità mentre è ancora in corso e visualizzare i risultati di installazione o altri messaggi nell'installazione di **notifiche** area nella parte superiore la console di Server Manager. Fare clic su di **notifiche** icona del flag per visualizzare ulteriori dettagli sulle installazioni o altre attività in esecuzione in Server Manager.  
  
## <a name="BKMK_installwps"></a>Installare ruoli, servizi ruolo e funzionalità con cmdlet di Windows PowerShell  
I cmdlet di distribuzione di Server Manager per la funzione di Windows PowerShell in modo simile al basata su GUI Aggiungi ruoli e funzionalità guidata e rimozione guidata ruoli e funzionalità, con una differenza importante. In Windows PowerShell, a differenza nell'Aggiunta guidata ruoli e funzionalità, gli strumenti di gestione e snap-in per un ruolo non presenti per impostazione predefinita. Per includere gli strumenti di gestione come parte dell'installazione di un ruolo, aggiungere il parametro `IncludeManagementTools` al cmdlet. Se si installa ruoli e funzionalità in un server che esegue l'opzione di installazione Server Core di Windows Server 2012 o versioni successive, è possibile aggiungere gli strumenti di gestione di un ruolo a un'installazione, ma gli strumenti di gestione basata su GUI e snap-in non può essere installato nei server che eseguono l'opzione di installazione Server Core di Windows Server. Solo della riga di comando e gli strumenti di gestione di Windows PowerShell possono essere installati l'opzione di installazione Server Core.  
  
#### <a name="to-install-roles-and-features-by-using-the-install-windowsfeature-cmdlet"></a>Per installare ruoli e funzionalità mediante il cmdlet Install-WindowsFeature  
  
1.  Per aprire una sessione di Windows PowerShell con diritti utente elevati, eseguire una di queste operazioni.  
  
    > [!NOTE]  
    > Se si installano ruoli e funzionalità in un server remoto, non è necessario eseguire Windows PowerShell con diritti utente elevati.  
  
    -   Nel desktop di Windows fare clic con il pulsante destro del mouse su **Windows PowerShell** nella barra delle applicazioni e scegliere **Esegui come amministratore**.  
  
    -   In Windows **avviare** schermata, fare clic sul riquadro per Windows PowerShell e quindi nella barra dell'app, fare clic su **Esegui come amministratore**.  
  
2.  Digitare **Get-WindowsFeature** e quindi premere **INVIO** per visualizzare un elenco dei ruoli e delle funzionalità disponibili e installati nel server locale. Se il computer locale non è un server o se si vogliono ottenere informazioni su un server remoto, eseguire **Get-WindowsFeature - computerName <***computer_name***>**, in cui  *computer_name* rappresenta il nome di un computer remoto che esegue Windows Server 2016. I risultati del cmdlet contengono i nomi di comando dei ruoli e funzionalità che si aggiungono al cmdlet nel passaggio 4.  
  
    > [!NOTE]  
    > In Windows PowerShell 3.0 e versioni successive di Windows PowerShell, non è necessario per importare il modulo di cmdlet di Server Manager nella sessione di Windows PowerShell prima di eseguire i cmdlet che fanno parte del modulo. Un modulo viene importato automaticamente alla prima esecuzione di un cmdlet che fa parte del modulo. Inoltre, i cmdlet di Windows PowerShell né i nomi di funzionalità utilizzati con i cmdlet di maiuscole e minuscole.  
  
3.  tipo di **Get-help Install-WindowsFeature**, quindi premere **invio** per visualizzare la sintassi e i parametri accettati per il `Install-WindowsFeature` cmdlet.  
  
4.  digitare il comando seguente e quindi premere **invio**, dove *nome_funzionalità* rappresenta il nome di comando di un ruolo o funzionalità che si desidera installare (ottenuto nel passaggio 2), e *nome_computer* rappresenta un computer remoto in cui si desidera installare ruoli e funzionalità. Utilizzare virgole per separare più valori per *nome_funzionalità* . Il parametro `Restart` riavvia automaticamente il server di destinazione se richiesto dall'installazione del ruolo o della funzionalità.  
  
    ```  
    Install-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
    ```  
  
    Per installare ruoli e funzionalità in un disco rigido virtuale offline, aggiungere il parametro `computerName` e il parametro `VHD` . Se non si aggiunge il parametro `computerName`, il cmdlet parte dal presupposto che il computer locale sia montato per accedere al disco rigido virtuale. Il parametro `computerName` contiene il nome del server in cui montare il disco rigido virtuale e il parametro `VHD` contiene il percorso del file VHD nel server specificato.  
  
    > [!NOTE]  
    > È necessario aggiungere il `computerName` parametro se si esegue il cmdlet da un computer che esegue un sistema operativo client Windows.  
    >   
    > Per installare ruoli e funzionalità in dischi rigidi virtuali offline, i dischi rigidi virtuali di destinazione devono soddisfare i requisiti seguenti.  
    >   
    > -   I dischi rigidi virtuali devono essere in esecuzione la versione di Windows Server che corrisponde alla versione di Server Manager è in esecuzione. Vedere la nota riportata all'inizio della [installare ruoli, servizi ruolo e funzionalità mediante l'aggiunta guidata ruoli e funzionalità](#BKMK_installarfw).  
    > -   I dischi rigidi virtuali non possono contenere più di un volume o una partizione di sistema.  
    > -   La cartella condivisa di rete in cui è archiviato il file VHD deve concedere i dritti di accesso seguenti all'account computer (o sistema locale) del server selezionato per montare il disco rigido virtuale. L'accesso con account solo utente non è sufficiente. La condivisione può concedere le autorizzazioni **Lettura** e **Scrittura** al gruppo **Everyone** per consentire l'accesso al disco rigido virtuale, ma si tratta di un'impostazione sconsigliata per motivi di sicurezza.  
    >   
    >     -   Accesso in **Lettura/Scrittura** alla finestra di dialogo **Condivisione file**.  
    >     -   **Controllo completo** accesso il **sicurezza** scheda, file o cartella **proprietà** la finestra di dialogo.  
  
    ```  
    Install-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
    ```  
  
    **Esempio:** Il cmdlet seguente installa il ruolo Servizi di dominio di active directory e la funzionalità Gestione criteri di gruppo in un server remoto, ContosoDC1. Vengono aggiunti strumenti di gestione e snap-in usando il parametro `IncludeManagementTools`, e il server di destinazione viene riavviato automaticamente, se richiesto dal processo di installazione.  
  
    ```  
    Install-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
    ```  
  
5.  Al termine dell'installazione, verificare l'installazione aprendo il **tutti i server** pagina in Server Manager, selezionando un server in cui è installato i ruoli e funzionalità e visualizzando il **ruoli e funzionalità** riquadro della pagina per il server selezionato. È anche possibile eseguire la `Get-WindowsFeature` cmdlet come destinata il server selezionato (Get-WindowsFeature - computerName <*nome_computer*>) per visualizzare un elenco di ruoli e funzionalità installati nel server.  
  
## <a name="BKMK_removerrfw"></a>rimuovere ruoli, servizi ruolo e funzionalità tramite la rimozione guidata ruoli e funzionalità  
In qualità di amministratore per disinstallare ruoli, servizi ruolo e funzionalità, è necessario essere connessi a un server. Se si è connessi al computer locale con un account che non dispone di diritti di amministratore nel server di destinazione della disinstallazione, fare clic con il pulsante destro del mouse sul server di destinazione nel riquadro **Server** e quindi scegliere **Account di gestione** per specificare un account con diritti di amministratore. Il server in cui si desidera montare un disco rigido virtuale offline deve essere aggiunto a Server Manager ed è necessario disporre di diritti di amministratore per tale server.  
  
#### <a name="to-remove-roles-and-features-by-using-the-remove-roles-and-features-wizard"></a>Per rimuovere ruoli e funzionalità tramite la rimozione guidata ruoli e funzionalità  
  
1.  Se Server Manager è già aperto, andare al passaggio successivo. Se Server Manager non è aperto, aprirlo in uno dei modi seguenti.  
  
    -   Nel desktop di Windows avviare Server Manager facendo clic su **Server Manager** nella barra delle applicazioni di Windows.  
  
    -   Nella schermata **Start** di Windows fare clic sul riquadro **Server Manager** .  
  
2.  Scegliere **Rimuovi ruoli e funzionalità** dal menu **Gestione**.  
  
3.  Nella pagina **Prima di iniziare** verificare che sia stata eseguita la preparazione per la rimozione di ruoli o funzioni da un server. Fare clic su **Avanti**.  
  
4.  Nel **selezione Server di destinazione** pagina, selezionare un server dal pool di server oppure selezionare un disco rigido virtuale offline. Per selezionare un disco rigido virtuale offline, selezionare innanzitutto il server in cui montare il disco rigido virtuale, quindi selezionare il file del disco rigido virtuale.  
  
    > [!NOTE]  
    > La cartella condivisa di rete in cui è archiviato il file VHD deve concedere i dritti di accesso seguenti all'account computer (o sistema locale) del server selezionato per montare il disco rigido virtuale. L'accesso con account solo utente non è sufficiente. La condivisione può concedere le autorizzazioni **Lettura** e **Scrittura** al gruppo **Everyone** per consentire l'accesso al disco rigido virtuale, ma si tratta di un'impostazione sconsigliata per motivi di sicurezza.  
    >   
    > -   Accesso in **Lettura/Scrittura** alla finestra di dialogo **Condivisione file**.  
    > -   Accesso **Controllo completo** alla scheda **Sicurezza**, finestra di dialogo **Proprietà** per file o cartelle.  
  
    Per informazioni su come aggiungere server al pool di server, vedere [aggiungere server a Server Manager](add-servers-to-server-manager.md). Dopo aver selezionato il server di destinazione, fare clic su **Avanti**.  
  
    > [!NOTE]  
    > È possibile usare la Rimuovi ruoli e funzionalità guidata per rimuovere ruoli e funzionalità dai server che eseguono la stessa versione di Windows Server che supporta la versione di Server Manager che si sta utilizzando. È possibile rimuovere ruoli, servizi ruolo o funzionalità dai server che eseguono Windows Server 2016, se è in esecuzione Server Manager in Windows Server 2012 R2, Windows Server 2012 o Windows 8. Non è possibile utilizzare la rimozione guidata ruoli e funzionalità per rimuovere ruoli e funzionalità dai server che eseguono Windows Server 2008 o Windows Server 2008 R2.  
  
5.  Selezionare i ruoli, selezionare i servizi ruolo per il ruolo se applicabile e quindi fare clic su **Avanti** per selezionare le funzionalità.  
  
    Come si procede, la rimozione dei ruoli e funzionalità guidata automaticamente richiesto di rimuovere tutti i ruoli, servizi ruolo o funzionalità che non è possibile eseguire senza i ruoli o le funzionalità che si desidera rimuovere.  
  
    Inoltre, è possibile scegliere di rimuovere gli strumenti di gestione e snap-in per i ruoli del server di destinazione. Per impostazione predefinita, nella schermata Rimozione ruoli e funzionalità guidata, gli strumenti di gestione sono selezionati per la rimozione. È possibile lasciare strumenti di gestione e snap-in se si prevede di usare il server selezionato per la gestione del ruolo su altri server remoti.  
  
6.  Nella pagina **Conferma selezioni per la rimozione** rivedere le selezioni di ruolo, funzionalità e server. Se si è pronti a rimuovere ruoli o funzionalità, fare clic su **rimuovere**.  
  
7.  Dopo aver fatto clic **rimuovere**, il **stato rimozione** pagina Visualizza stato rimozione, risultati e messaggi quali avvisi, errori o procedure di configurazione post-rimozione necessarie, come il riavvio del server di destinazione. In Windows Server 2012 e versioni successive di Windows Server, è possibile chiudere la rimozione guidata ruoli e funzionalità mentre la rimozione è ancora nello stato di avanzamento e visualizzare i risultati della rimozione o altri messaggi nel **notifiche** area nella parte superiore del Console di Server Manager. Fare clic su di **notifiche** flag per visualizzare ulteriori dettagli sulle rimozioni o altre attività in esecuzione in Server Manager.  
  
## <a name="BKMK_removewps"></a>rimuovere ruoli, servizi ruolo e funzionalità usando i cmdlet di Windows PowerShell  
I cmdlet di distribuzione di Server Manager per la funzione di Windows PowerShell in modo simile alla grafica Rimozione guidata ruoli e funzionalità, con una differenza importante. In Windows PowerShell, a differenza nella schermata Rimozione ruoli e funzionalità guidata, gli strumenti di gestione e snap-in per un ruolo non vengono rimossi per impostazione predefinita. Per includere gli strumenti di gestione come parte della rimozione di un ruolo, aggiungere il parametro `IncludeManagementTools` al cmdlet. Se si sta disinstallando funzionalità, ruoli e funzionalità da un server che esegue l'opzione di installazione Server Core di Windows Server 2012 o versione successiva di Windows Server, questo viene rimosso il parametro della riga di comando e gli strumenti di gestione di Windows PowerShell per i ruoli specificati.  
  
#### <a name="to-remove-roles-and-features-by-using-the-uninstall-windowsfeature-cmdlet"></a>Per rimuovere ruoli e funzionalità mediante il cmdlet Uninstall-WindowsFeature  
  
1.  Per aprire una sessione di Windows PowerShell con diritti utente elevati, eseguire una di queste operazioni.  
  
    > [!NOTE]  
    > Se si disinstallano ruoli e funzionalità da un server remoto, non è necessario eseguire Windows PowerShell con diritti utente elevati.  
  
    -   Nel desktop di Windows fare clic con il pulsante destro del mouse su **Windows PowerShell** nella barra delle applicazioni e scegliere **Esegui come amministratore**.  
  
    -   Nella finestra di Windows **avviare** schermata, fare clic sul riquadro Windows PowerShell e quindi nella barra dell'app, fare clic su **Esegui come amministratore**.  
  
2.  Digitare **Get-WindowsFeature** e quindi premere **INVIO** per visualizzare un elenco dei ruoli e delle funzionalità disponibili e installati nel server locale. Se il computer locale non è un server o se si vogliono ottenere informazioni su un server remoto, eseguire **Get-WindowsFeature - computerName <***computer_name***>**, in cui  *computer_name* rappresenta il nome di un computer remoto che esegue Windows Server 2016. I risultati del cmdlet contengono i nomi di comando dei ruoli e funzionalità che si aggiungono al cmdlet nel passaggio 4.  
  
    > [!NOTE]  
    > In Windows PowerShell 3.0 e versioni successive di Windows PowerShell, non è necessario per importare il modulo di cmdlet di Server Manager nella sessione di Windows PowerShell prima di eseguire i cmdlet che fanno parte del modulo. Un modulo viene importato automaticamente alla prima esecuzione di un cmdlet che fa parte del modulo. Inoltre, i cmdlet di Windows PowerShell né i nomi di funzionalità utilizzati con i cmdlet di maiuscole e minuscole.  
  
3.  tipo di **Get-help Uninstall-WindowsFeature**, quindi premere **invio** per visualizzare la sintassi e i parametri accettati per il `Uninstall-WindowsFeature` cmdlet.  
  
4.  Digitare quanto segue, quindi premere **INVIO**, dove *nome_funzionalità* rappresenta il nome del comando di un ruolo o di una funzionalità che si desidera installare (come descritto nel passaggio 2) e *computer_name* rappresenta il computer remoto da cui rimuovere ruoli e funzionalità. Utilizzare virgole per separare più valori per *nome_funzionalità* . Il parametro `Restart` riavvia automaticamente il server di destinazione se richiesto dalla rimozione del ruolo o della funzionalità.  
  
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
  
    **Esempio:** Il cmdlet seguente rimuove il ruolo Servizi di dominio di active directory e la funzionalità Gestione criteri di gruppo da un server remoto, ContosoDC1. Vengono anche rimossi strumenti di gestione e snap-in, e il server di destinazione viene riavviato automaticamente, se richiesto dal processo di rimozione.  
  
    ```  
    Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
    ```  
  
5.  Termine della rimozione, verificare che i ruoli e funzionalità sono stati effettivamente rimossi aprendo la **tutti i server** pagina in Server Manager, selezionando il server da cui sono stati rimossi ruoli e funzionalità e visualizzando il **ruoli e funzionalità** riquadro della pagina per il server selezionato. È anche possibile eseguire la `Get-WindowsFeature` cmdlet come destinata il server selezionato (Get-WindowsFeature - computerName <*nome_computer*>) per visualizzare un elenco di ruoli e funzionalità installati nel server.  
  
## <a name="BKMK_batch"></a>Installare ruoli e funzionalità in più server eseguendo uno script di Windows PowerShell  
Anche se è possibile utilizzare l'aggiunta guidata ruoli e funzionalità per installare ruoli, servizi ruolo e funzionalità in più di un server di destinazione in una singola sessione, è possibile usare uno script di Windows PowerShell per installare ruoli, servizi ruolo e funzionalità in più di destinazione server che si siano gestendo tramite Server Manager. Lo script che consente di eseguire una distribuzione di batch, questo processo viene chiamato, punta a un file di configurazione XML che è possibile creare facilmente tramite l'aggiunta guidata ruoli e funzionalità e scegliendo **Esporta impostazioni di configurazione** dopo raggiunto la procedura guidata per la **Conferma selezioni per l'installazione** pagina l'aggiunta guidata ruoli e funzionalità.  
  
> [!IMPORTANT]  
> Tutti i server di destinazione specificati nello script devono essere in esecuzione la versione di Windows Server che corrisponde alla versione di Server Manager sono in esecuzione nel computer locale. Ad esempio, se si eseguono Server Manager in Windows 10, è possibile installare ruoli, servizi ruolo e funzionalità in server che eseguono Windows Server 2016. Se vengono aggiunti strumenti di gestione basata su GUI per l'installazione, il processo di installazione converte automaticamente i server di destinazione che eseguono l'opzione di installazione Server Core di Windows Server per l'opzione di installazione completa (server con un'interfaccia utente grafica completa, anche nota come esecuzione Shell grafica Server).  
>   
> Lo script fornito in questa sezione è riportato un esempio di come eseguire la distribuzione in batch utilizzando il `Install-WindowsFeature` cmdlet e uno script Windows PowerShell. Esistono altri script e metodi possibili per eseguire la distribuzione in batch in più server. Per cercare o fornire altri script per la distribuzione di ruoli e funzionalità, eseguire una ricerca nell' [archivio centrale di script](https://gallery.technet.microsoft.com/ScriptCenter).  
  
#### <a name="to-install-roles-and-features-on-multiple-servers"></a>Per installare ruoli e funzionalità in più server  
  
1.  Se non è già stato fatto, creare un file di configurazione XML che contiene i ruoli, servizi ruolo e funzionalità che si desidera installare in più server. È possibile creare questo file di configurazione eseguendo l'aggiunta guidata ruoli e funzionalità, la selezione di ruoli, servizi ruolo e funzionalità che si desidera e facendo clic **Esporta impostazioni di configurazione** dopo aver raggiunto la procedura guidata per la **Conferma selezioni per l'installazione** pagina. Salvare il file di configurazione in una posizione appropriata. Non è necessario fare clic su **Installa** o completare la procedura guidata, se viene eseguita esclusivamente per creare un file di configurazione.  
  
2.  Per aprire una sessione di Windows PowerShell con diritti utente elevati, eseguire una di queste operazioni.  
  
    -   Nel desktop di Windows fare clic con il pulsante destro del mouse su **Windows PowerShell** nella barra delle applicazioni e scegliere **Esegui come amministratore**.  
  
    -   Nella finestra di Windows **avviare** schermata, fare clic sul riquadro Windows PowerShell e quindi nella barra dell'app, fare clic su **Esegui come amministratore**.  
  
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
  
        **Invoke-WindowsFeatureBatchDeployment -computerNames $ServerNames -ConfigurationFilepath C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml**  
  
5.  Al termine dell'installazione, verificare l'installazione aprendo il **tutti i server** pagina in Server Manager, selezionando un server in cui è installato i ruoli e funzionalità e visualizzando il **ruoli e funzionalità** riquadro della pagina per il server selezionato. È anche possibile eseguire la `Get-WindowsFeature` cmdlet su un server specifico (`Get-WindowsFeature -computerName` <*nome_computer*>) per visualizzare un elenco di ruoli e funzionalità installati nel server.  
  
## <a name="BKMK_FoD"></a>Installare .NET Framework 3.5 e altre funzionalità su richiesta  
a partire da Windows Server 2012 e Windows 8, i file di funzionalità per .NET Framework 3.5 (che include .NET Framework 2.0 e .NET Framework 3.0) non sono disponibili nel computer locale per impostazione predefinita. I file sono stati rimossi. I file per le funzionalità che sono stati rimossi in una configurazione Funzionalità su richiesta e i file delle funzionalità per .NET Framework 3.5 sono disponibili tramite Windows Update. Per impostazione predefinita, se i file di funzionalità non sono disponibili nel server di destinazione che esegue Windows Server 2012 o versioni successive, il processo di installazione cerca i file mancanti connettendosi a Windows Update. È possibile eseguire l'override del comportamento predefinito configurando un'impostazione di criteri di gruppo o specificando un percorso di origine alternativo durante l'installazione, se si installano usando Aggiungi ruoli e funzionalità guidata GUI o una riga di comando.  
  
È possibile installare .NET Framework 3.5 in uno dei modi seguenti.  
  
-   Usare [Per installare .NET Framework 3.5 mediante il cmdlet Install-WindowsFeature](#BKMK_dotnetcmdlet) per aggiungere il parametro `Source` e specificare un'origine da cui recuperare i file di funzionalità di .NET Framework 3.5. Se non si aggiunge il parametro `Source`, il processo di installazione determina innanzitutto se il percorso ai file di funzionalità è stato specificato da impostazioni di Criteri di gruppo, e in caso contrario, cerca i file di funzionalità mancanti tramite Windows Update.  
  
-   Uso [per installare .NET Framework 3.5 tramite l'aggiunta guidata ruoli e funzionalità](#BKMK_arfw) per specificare un percorso di file di origine alternativo nella **Conferma opzioni di installazione** pagina l'aggiunta guidata ruoli e funzionalità.  
  
-   Usare [Per installare .NET Framework 3.5 usando DISM](#BKMK_dism) per recuperare i file da Windows Update per impostazione predefinita oppure specificando un percorso di origine per il supporto di installazione.  
  
[Configurare le origini alternative per i file di funzionalità in Criteri di gruppo](#BKMK_configgp) per .NET Framework 3.5 o altre funzionalità, se i file di funzionalità non sono presenti nel computer locale.  
  
> [!IMPORTANT]  
> Quando si installano i file di funzionalità da un'origine remota, il percorso di origine o la condivisione file deve concedere autorizzazioni di **Lettura** al gruppo **Everyone** (non consigliato per motivi di sicurezza) o all'account del computer (sistema locale) del server di destinazione. La concessione dell'accesso all'account utente non è sufficiente.  
>   
> I server appartenenti a gruppi di lavoro non possono accedere a condivisioni file esterne, anche se l'account computer del server del gruppo di lavoro dispone delle autorizzazioni **Lettura** per la condivisione esterna. Tra le posizioni di origine alternative valide per i server di gruppi di lavoro sono inclusi supporti di installazione, Windows Update e file VHD o WIM archiviati nel server del gruppo di lavoro locale.  
>   
> È possibile specificare un file WIM come origine del file di funzionalità alternativo quando si installano ruoli, servizi ruolo e funzionalità in un server fisico in esecuzione. Il percorso di origine per un file WIM dovrebbe essere nel formato seguente, con **WIM** come un prefisso e l'indice in cui i file di funzionalità come suffisso: **WIM:e:\Sources\Install.wim:4**. È tuttavia possibile utilizzare un file WIM direttamente come origine per l'installazione di ruoli, servizi ruolo e funzionalità di un disco rigido Virtuale offline. è necessario montare il disco rigido Virtuale offline e puntare al percorso di montaggio per i file di origine oppure puntare a una cartella che contiene una copia del contenuto del file WIM.  
  
### <a name="BKMK_dotnetcmdlet"></a>Per installare .NET Framework 3.5 mediante il cmdlet Install-WindowsFeature  
  
1.  Per aprire una sessione di Windows PowerShell con diritti utente elevati, eseguire una di queste operazioni.  
  
    > [!NOTE]  
    > Se si installano ruoli e funzionalità da un server remoto, non occorre eseguire Windows PowerShell con diritti utente elevati.  
  
    -   Nel desktop di Windows fare clic con il pulsante destro del mouse su **Windows PowerShell** nella barra delle applicazioni e scegliere **Esegui come amministratore**.  
  
    -   Nella finestra di Windows **avviare** schermata, fare clic sul riquadro Windows PowerShell e quindi nella barra dell'app, fare clic su **Esegui come amministratore**.  
  
    -   In un server che esegue l'opzione di installazione Server Core di Windows Server 2012 R2 o Windows Server 2012, digitare **PowerShell** in un prompt dei comandi, quindi premere **invio**.  
  
2.  digitare il comando seguente e quindi premere **invio**. Nell'esempio seguente i file di origine sono posizionati in un archivio affiancato (abbreviato in **SxS**) in un supporto di installazione nell'unità D.  
  
    ```  
    Install-WindowsFeature NET-Framework-Core -Source D:\Sources\SxS  
    ```  
  
    Se si vuole che il comando usi Windows Update come origine per i file di funzionalità mancanti o se è già stata configurata un'origine predefinita mediante Criteri di gruppo, non è necessario aggiungere il parametro `Source` a meno che non si intenda specificare un'origine diversa.  
  
### <a name="BKMK_arfw"></a>Installare .NET Framework 3.5 usando l'aggiunta guidata ruoli e funzionalità  
  
1.  Nel **Manage** dal menu in Server Manager fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Selezionare un server di destinazione che esegue Windows Server 2016.  
  
3.  Nel **Selezione funzionalità** pagina l'aggiunta guidata ruoli e funzionalità, selezionare **.NET Framework 3.5**.  
  
4.  Se le impostazioni di Criteri di gruppo del computer locale lo consentono, il processo di installazione tenta di recuperare i file di funzionalità mancanti tramite Windows Update. Fare clic su **Installa**. Non è necessario procedere al passaggio successivo.  
  
    Se le impostazioni di criteri di gruppo non lo consentono, o si desidera utilizzare un'altra origine per i file di funzionalità di .NET Framework 3.5, nella **Conferma selezioni per l'installazione** pagina della procedura guidata, fare clic su **specificare un percorso di origine alternativo** .  
  
5.  Immettere un percorso a un archivio affiancato (denominato **SxS**) nel supporto di installazione o a un file WIM. Nell'esempio seguente il supporto di installazione è posizionato nell'unità D.  
  
    **D:\Sources\SxS\\**  
  
    Per specificare un file WIM, aggiungere un prefisso **WIM:** e aggiungere l'indice dell'immagine da utilizzare nel file WIM come un suffisso, come mostrato nell'esempio seguente.  
  
    **WIM:\\\\***nome_server***\share\install.wim:3**  
  
6.  Fare clic su **OK**e quindi su **Installa**.  
  
### <a name="BKMK_dism"></a>Per installare .NET Framework 3.5 usando DISM  
  
1.  Per aprire una sessione di Windows PowerShell con diritti utente elevati, eseguire una di queste operazioni.  
  
    > [!NOTE]  
    > Se si installano ruoli e funzionalità da un server remoto, non occorre eseguire Windows PowerShell con diritti utente elevati.  
  
    -   Nel desktop di Windows fare clic con il pulsante destro del mouse su **Windows PowerShell** nella barra delle applicazioni e scegliere **Esegui come amministratore**.  
  
    -   In Windows **avviare** schermata, fare clic sul riquadro di Windows PowerShell e quindi nella barra dell'app, fare clic su **Esegui come amministratore**.  
  
    -   In un server che esegue l'opzione di installazione Server Core, digitare **PowerShell** in un prompt dei comandi, quindi premere **invio**.  
  
2.  Eseguire uno dei comandi DISM seguenti.  
  
    -   Se il computer abbia accesso a Windows Update o un percorso di file di origine predefinita è già stato configurato in Criteri di gruppo, eseguire il comando seguente.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All  
        ```  
  
    -   Se il computer abbia accesso al supporto di installazione, eseguire un comando simile al seguente. Nell'esempio seguente il supporto di installazione del sistema operativo è posizionato nell'unità D. Il parametro `LimitAccess` impedisce che il comando tenti di contattare Windows Update o un server che esegue WSUS.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All /LimitAccess /Source:d:\sources\sxs  
        ```  
  
    > [!NOTE]  
    > Per il comando DISM viene fatta distinzione tra maiuscole e minuscole.  
  
### <a name="BKMK_configgp"></a>Configurare le origini alternative per i file di funzionalità in Criteri di gruppo  
L'impostazione di Criteri di gruppo descritta in questa sezione specifica i percorsi di origine autorizzati per i file .NET Framework 3.5 e altri file di funzionalità rimossi nell'ambito di una configurazione Funzionalità su richiesta. L'impostazione dei criteri **specificare le impostazioni per l'installazione del componente facoltativo e il ripristino** si trova nel **computer Configurazione computer\Modelli amministrativi\Sistema** cartella nei criteri di gruppo Editor Criteri di gruppo locali o Console di gestione.  
  
> [!NOTE]  
> Per modificare le impostazioni di Criteri di gruppo nel computer locale è necessario essere un membro del gruppo Administrators. Se le impostazioni di Criteri di gruppo per il computer che si vuole gestire sono controllate a livello di dominio, per modificare le impostazioni di Criteri di gruppo è necessario essere un membro del gruppo Domain Administrators.  
  
##### <a name="to-configure-a-default-alternate-source-path-in-group-policy"></a>Per configurare un percorso di origine alternativo predefinito in Criteri di gruppo  
  
1.  In editor Criteri di gruppo locali o nella Console Gestione criteri di gruppo, aprire l'impostazione dei criteri seguenti.  
  
    **computer Configurazione computer\Modelli amministrativi\sistema\specifica le impostazioni per l'installazione del componente facoltativo e il ripristino**  
  
2. Selezionare **abilitato** per abilitare l'impostazione di criteri, se non è già abilitato.  
  
3.  Nella casella di testo **Percorso file di origine alternativo** dell'area **Opzioni** specificare un percorso completo a una cartella condivisa o a un file WIM. Per specificare un file WIM come percorso file di origine alternativo, aggiungere il prefisso **WIM:** al percorso, quindi aggiungere l'indice dell'immagine da utilizzare nel file WIM come suffisso. Di seguito sono illustrati alcuni esempi di valori che è possibile specificare.  
  
    -   percorso di una cartella condivisa: **\\\\***nome_server***\share\\* * * nome_cartella*  
  
    -   percorso di un file WIM, in cui **3** rappresenta l'indice dell'immagine in cui si trovano i file delle funzionalità:  **WIM:\\\\***nome_server***\share\install.wim:3**  
  
4.  Se non si desidera che i computer controllati da questa impostazione di criterio cerchino per file di funzionalità mancanti in Windows Update, selezionare **tentare mai di scaricare payload da Windows Update**.  
  
5.  Se i computer controllati da questa impostazione di criterio in genere ricevono aggiornamenti tramite WSUS ma per trovare i file di funzionalità mancanti si preferisce utilizzare Windows Update, selezionare **Contatta direttamente Windows Update invece di Windows Server Update Services (WSUS) per scaricare il contenuto di ripristino**.  
  
6.  Al termine della modifica dell'impostazione di criterio, fare clic su **OK** e chiudere l'Editor Criteri di gruppo.  
  
## <a name="see-also"></a>Vedere anche  
[Opzioni di installazione di Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573)  
[Considerazioni sulla distribuzione di Microsoft .NET Framework 3.5](https://go.microsoft.com/fwlink/p/?LinkId=248869)  
[Come abilitare o disabilitare le funzionalità di Windows](https://go.microsoft.com/fwlink/p/?LinkId=246552)  
  


