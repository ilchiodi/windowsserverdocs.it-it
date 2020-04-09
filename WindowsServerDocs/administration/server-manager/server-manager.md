---
title: Server Manager
description: Server Manager
ms.prod: windows-server
ms.technology: manage-server-manager
ms.topic: article
ms.assetid: d996ef40-8bcc-42b0-b6ae-806b828223f6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41d9227dd5472fc55858d75fa25e728dc69c2c7c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851474"
---
# <a name="server-manager"></a>Server Manager

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Server Manager è una console di gestione di Windows Server che consente ai professionisti IT di effettuare il provisioning e gestire server Windows locali e remoti dai propri desktop, senza richiedere l'accesso fisico ai server o la necessità di abilitare le connessioni rdP (Desktop remoto Protocol) a ogni server. Anche se Server Manager è disponibile in Windows Server 2008 R2 e Windows Server 2008, Server Manager è stato aggiornato in Windows Server 2012 per supportare la gestione multiserver remota e contribuire ad aumentare il numero di server che gli amministratori possono gestire.

Nei test, è possibile utilizzare Server Manager in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 per gestire i server fino a 100, a seconda dei carichi di lavoro che eseguono i server. Il numero di server che è possibile gestire tramite un'unica console di Server Manager può variare a seconda della quantità di dati richiesti dai server gestiti e le risorse hardware e di rete disponibili nel computer che esegue Server Manager. Quando la quantità di dati da visualizzare si avvicina alla capacità delle risorse del computer, possono verificarsi rallentamenti nelle risposte di Server Manager e ritardi nel completamento degli aggiornamenti. Per aumentare il numero di server gestibile con Server Manager, si consiglia di limitare i dati dell'evento che Server Manager ottiene dai server gestiti usando le impostazioni nella finestra di dialogo **Configura dati evento**. La finestra di dialogo Configura dati evento può essere aperta dal menu **Attività** nel riquadro **Eventi**. Se è necessario gestire un numero di livello aziendale di server nell'organizzazione, è consigliabile valutare i prodotti di [suite di Microsoft System Center](https://go.microsoft.com/fwlink/p/?LinkId=239437).

In questo argomento e nei relativi argomenti secondari vengono fornite informazioni sull'utilizzo delle funzionalità nella console di Server Manager. In questo argomento sono contenute le seguenti sezioni.

-   [Esaminare le considerazioni e i requisiti di sistema iniziali](#review-initial-considerations-and-system-requirements)

-   [Attività che è possibile eseguire in Server Manager](#tasks-that-you-can-perform-in-server-manager)

-   [Avvia Server Manager](#start-server-manager)

-   [Riavviare i server remoti](#restart-remote-servers)

-   [Esporta le impostazioni Server Manager in altri computer](#export-server-manager-settings-to-other-computers)

## <a name="review-initial-considerations-and-system-requirements"></a>Rivedere le considerazioni e i requisiti di sistema
Nelle sezioni seguenti sono elencate alcune considerazioni iniziali che è necessario esaminare, oltre ai requisiti hardware e software per Server Manager.

### <a name="hardware-requirements"></a>Requisiti hardware
Server Manager viene installato per impostazione predefinita con tutte le edizioni di Windows Server 2016. Non esistono altri requisiti hardware per Server Manager.

### <a name="software-and-configuration-requirements"></a>Requisiti di configurazione e software
Server Manager viene installato per impostazione predefinita con tutte le edizioni di Windows Server 2016. È possibile utilizzare Server Manager in Windows Server 2016 per gestire [Opzioni di installazione Server Core](https://go.microsoft.com/fwlink/p/?LinkID=241573) di Windows Server 2016, Windows Server 2012 e Windows Server 2008 R2 che sono in esecuzione in computer remoti. L'opzione di installazione Server Core di Windows Server 2016 eseguire Server Manager.

Server Manager viene eseguito nell'interfaccia grafica di Server minimo; vale a dire quando la funzionalità Shell grafica Server non è installata. La funzionalità Shell grafica Server non è installata per impostazione predefinita in Windows Server 2016. Se non si esegue Shell grafica Server, Server Manager console viene eseguito, ma non sono disponibili alcune applicazioni o gli strumenti disponibili dalla console. I browser Internet non possono essere eseguiti senza la shell grafica server, pertanto le pagine Web e le applicazioni come la Guida HTML, ad esempio la Guida F1 di MMC, non possono essere aperte. Non è possibile aprire finestre di dialogo per la configurazione di Windows di aggiornamenti automatici e feedback quando Shell grafica Server non è installata; i comandi che aprono queste finestre di dialogo nella console di Server Manager vengono reindirizzati per eseguire **sconfig. cmd**.

Per gestire i server che eseguono versioni di Windows Server precedente a Windows Server 2016, installare i seguenti aggiornamenti software e per semplificare le versioni precedenti di Windows Server tramite Server Manager in Windows Server 2016.

|Sistema operativo|Software necessario|
|----------|-----------|
| Windows Server 2012 R2 o Windows Server 2012 |-   [.NET Framework 4,6](https://www.microsoft.com/download/details.aspx?id=45497)<br />-   [Windows Management Framework 5,0](https://go.microsoft.com/fwlink/?LinkID=395058). Il pacchetto di download di Windows Management Framework 5.0 Aggiorna provider Strumentazione gestione Windows (WMI) su Windows Server 2012 R2 e Windows Server 2012. Il provider WMI aggiornati consentono a Server Manager di raccogliere informazioni sui ruoli e funzionalità installati sui server gestiti. Fino a quando non viene applicato l'aggiornamento, i server che eseguono Windows Server 2012 R2 o Windows Server 2012 presenta lo stato di gestibilità di **non accessibile**.<br />-L'aggiornamento delle prestazioni associati [articolo della Knowledge Base 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) non è più necessario nei server che eseguono Windows Server 2012 R2 o Windows Server 2012.|
| Windows Server 2008 R2 |-   [.NET Framework 4,5](https://www.microsoft.com/download/details.aspx?id=30653)<br />-   [Windows Management Framework 4,0](https://go.microsoft.com/fwlink/?LinkId=293881). Il pacchetto di download di Windows Management Framework 4.0 Aggiorna provider Strumentazione gestione Windows (WMI) in Windows Server 2008 R2. Il provider WMI aggiornati consentono a Server Manager di raccogliere informazioni sui ruoli e funzionalità installati sui server gestiti. Fino a quando non viene applicato l'aggiornamento, i server che eseguono Windows Server 2008 R2 con stato gestibilità **non accessibile**.<br />-L'aggiornamento delle prestazioni associati [articolo della Knowledge Base 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) consente di raccogliere dati sulle prestazioni da Windows Server 2008 R2 Server Manager.|
| Windows Server 2008 |-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />-   [Windows Management framework 3,0](https://go.microsoft.com/fwlink/p/?LinkID=229019) Windows management Framework 3,0 scaricare i provider degli aggiornamenti del pacchetto Strumentazione gestione Windows (WMI) in windows Server 2008. Il provider WMI aggiornati consentono a Server Manager di raccogliere informazioni sui ruoli e funzionalità installati sui server gestiti. Fino a quando non viene applicato l'aggiornamento, i server che eseguono Windows Server 2008 hanno uno stato di gestibilità **non accessibile: verificare che le versioni precedenti eseguano Windows Management Framework 3,0**.<br />-L'aggiornamento delle prestazioni associati [articolo della Knowledge Base 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) consente di raccogliere dati sulle prestazioni da Windows Server 2008 Server Manager.|

#### <a name="manage-remote-computers-from-a-client-computer"></a>Gestire i computer remoti da un computer client
La console di Server Manager è inclusa in [Strumenti di amministrazione remota del Server](https://go.microsoft.com/fwlink/?LinkID=404281) per Windows 10. Si noti che quando viene installato Strumenti di amministrazione remota del Server in un computer client, è possibile gestire il computer locale tramite Server Manager; Server Manager può essere utilizzata per gestire i computer o dispositivi che eseguono un sistema operativo client Windows. È possibile utilizzare solo Server Manager per gestire i server basati su Windows.

|Sistema operativo di gestione di server di origine|Destinato a Windows Server 2016|Destinata a Windows Server 2012 R2 |Destinato a Windows Server 2012 |Destinato a Windows Server 2008 R2 o Windows Server 2008 |Destinato a Windows Server 2003|
|-------------------------------|--------------------------------------------|---------------------------------------|------------------------------------|-----------------------------------------------------------------------|------------------|
|Windows 10 o Windows Server 2016|Supporto completo|Supporto completo|Supporto completo|Dopo che i [requisiti di configurazione e software](#software-and-configuration-requirements) sono stati soddisfatti, è possibile eseguire la maggiore parte delle attività di gestione, ad eccezione dell'installazione o della disinstallazione di ruoli o funzionalità|Non supportato|
|Windows 8.1 o Windows Server 2012 R2 |Non supportato|Supporto completo|Supporto completo|Dopo che i [requisiti di configurazione e software](#software-and-configuration-requirements) sono stati soddisfatti, è possibile eseguire la maggiore parte delle attività di gestione, ad eccezione dell'installazione o della disinstallazione di ruoli o funzionalità|Supporto limitato; solo stato online e offline|
|Windows 8 o Windows Server 2012 |Non supportato|Non supportato|Supporto completo|Dopo che i [requisiti di configurazione e software](#software-and-configuration-requirements) sono stati soddisfatti, è possibile eseguire la maggiore parte delle attività di gestione, ad eccezione dell'installazione o della disinstallazione di ruoli o funzionalità|Supporto limitato; solo stato online e offline|

###### <a name="to-start-server-manager-on-a-client-computer"></a>Per avviare Server Manager in un computer client

1.  Seguire le istruzioni [Strumenti di amministrazione remota del Server](../../remote/remote-server-administration-tools.md) per installare Remote Server Administration Tools per Windows 10.

2.  Nella schermata **Start** fare clic su **Server Manager**. Il riquadro **Server Manager** è disponibile dopo l'installazione di Strumenti di amministrazione remota del Server.

3.  Se non vengono visualizzati né gli **strumenti di amministrazione** né i riquadri **Server Manager** nella schermata **start** dopo l'installazione di strumenti di amministrazione remota del server e la ricerca di Server Manager nella schermata **Start** non Visualizza i risultati, verificare che l'impostazione **Mostra strumenti di amministrazione** sia attivata. Per visualizzare questa impostazione, posizionare il cursore del mouse sull'angolo superiore destro della schermata **Start** , quindi fare clic su **Impostazioni**. Se l'impostazione **Mostra strumenti di amministrazione** è disattivata, attivarla per visualizzare gli strumenti installati nell'ambito di Strumenti di amministrazione remota del server.

Per ulteriori informazioni sull'esecuzione di Strumenti di amministrazione remota del server per Windows 10 per gestire i server remoti, vedere [strumenti di amministrazione remota del server](https://go.microsoft.com/fwlink/?LinkID=221055) su TechNet wiki.

#### <a name="configure-remote-management-on-servers-that-you-want-to-manage"></a>Configurare la gestione remota su server che si desidera gestire

> [!IMPORTANT]
> Per impostazione predefinita, gestione remota di Server Manager e Windows PowerShell è abilitata in Windows Server 2016.

Per eseguire attività di gestione su server remoti utilizzando Server Manager, i server remoti che si desidera gestire devono essere configurati per consentire la gestione remota tramite Server Manager e Windows PowerShell. Se la gestione remota è stata disabilitata in Windows Server 2012 R2 o Windows Server 2012 e si vuole abilitarla di nuovo, seguire questa procedura.

##### <a name="to-configure-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-the-windows-interface"></a>Per configurare Server Manager gestione remota in Windows Server 2012 R2 o Windows Server 2012 usando l'interfaccia di Windows

1.  > [!NOTE]
    > Le impostazioni controllate dalla finestra di dialogo **Configura gestione remota** non interessano parti di Server Manager che utilizzano DCOM per le comunicazioni remote.

    Eseguire una delle operazioni seguenti per aprire Server Manager se non è già aperto.

    -   Sulla barra delle applicazioni di Windows fare clic sul pulsante Server Manager.

    -   Nella schermata **Start** fare clic su **Server Manager**.

2.  Nell'area **Proprietà** della pagina **server locali** fare clic sul valore con collegamento ipertestuale per la proprietà **gestione remota** .

3.  Eseguire una delle operazioni seguenti, quindi fare clic su **OK**.

    -   Per impedire che questo computer venga gestito in modalità remota tramite Server Manager (o Windows PowerShell se è installato), deselezionare la casella di controllo **Abilita gestione remota del server da altri computer** .

    -   Per consentire a questo computer di gestione remota tramite Server Manager o Windows PowerShell, selezionare **Consenti gestione remota del server da altri computer**.

##### <a name="to-enable-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-windows-powershell"></a>Per abilitare la gestione remota di Server Manager in Windows Server 2012 R2 o Windows Server 2012 usando Windows PowerShell

1.  Effettuare una delle operazioni riportate di seguito.

    -   Per eseguire Windows PowerShell come amministratore dalla schermata **Start** , fare clic con il pulsante destro del mouse sul riquadro **Windows PowerShell** e quindi scegliere **Esegui come amministratore**.

    -   Per eseguire Windows PowerShell come amministratore dal desktop, fare clic con il pulsante destro del mouse sul collegamento di **Windows PowerShell** nella barra delle applicazioni e quindi scegliere **Esegui come amministratore**.

2.  digitare quanto segue, quindi premere **invio** per abilitare tutte le eccezioni alle regole firewall necessarie.

    **Configure-SMRemoting. exe-Abilita**

    > [!NOTE]
    > Questo comando funziona anche in un prompt dei comandi aperto con diritti utente elevati (Esegui come amministratore).

    Se l'abilitazione della gestione remota non riesce, vedere [about_remote_Troubleshooting](https://go.microsoft.com/fwlink/p/?LinkID=135188) in Microsoft TechNet per suggerimenti e procedure consigliate per la risoluzione dei problemi.

###### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-older-operating-systems"></a>Per abilitare la gestione remota di Server Manager e Windows PowerShell in sistemi operativi precedenti

-   Effettuare una delle operazioni riportate di seguito.

    -   Per abilitare la gestione remota nei server che eseguono Windows Server 2008 R2, vedere [gestione remota con Server Manager](https://go.microsoft.com/fwlink/?LinkID=137378) nella Guida di windows Server 2008 R2.

    -   Per abilitare la gestione remota nei server che eseguono Windows Server 2008, vedere [abilitare e usare i comandi remoti in Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=242565).

## <a name="tasks-that-you-can-perform-in-server-manager"></a>Le attività che è possibile eseguire in Server Manager
Server Manager rende più efficiente l'amministrazione del server, consentendo agli amministratori di eseguire attività nella tabella seguente tramite un singolo strumento. In Windows Server 2012 R2 e Windows Server 2012, sia gli utenti standard di un server sia i membri del gruppo Administrators possono eseguire attività di gestione in Server Manager, ma per impostazione predefinita gli utenti standard non possono eseguire alcune attività, come illustrato nella tabella seguente.

Per controllare ulteriormente l'accesso utente standard ad alcuni dati aggiuntivi, gli amministratori possono usare due cmdlet di Windows PowerShell nel modulo cmdlet di Server Manager, [Enable-ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205470.aspx) e [Disable-ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205468.aspx). Il cmdlet **Enable-ServerManagerStandardUserremoting** può fornire uno o più utenti standard e non amministratori per accedere a dati di inventario di eventi, servizi, contatori delle prestazioni e ruoli e funzionalità.

> [!IMPORTANT]
> Server Manager può essere utilizzata per gestire una versione più recente del sistema operativo Windows Server. Non è possibile usare Server Manager in esecuzione in Windows Server 2012 o Windows 8 per gestire i server che eseguono Windows Server 2012 R2.

|Descrizione dell'attività|Administrators (incluso l'account Administrator predefinito)|Utenti server standard|
|----------|----------------------------------|-------------|
|aggiungere server remoti a un pool di server che Server Manager possibile utilizzare per la gestione.|Sì|No|
|creare e modificare gruppi personalizzati di server, ad esempio i server che si trovano in una posizione geografica specifica o che svolgono uno scopo specifico.|Sì|Sì|
|Installare o disinstallare ruoli, servizi ruolo e funzionalità nei server locali o remoti che eseguono Windows Server 2012 R2 o Windows Server 2012. Per le definizioni di ruoli, servizi ruolo e funzionalità, vedere [ruoli, servizi ruolo e funzionalità](https://go.microsoft.com/fwlink/p/?LinkId=239558).|Sì|No|
|Visualizzare e modificare le funzionalità e i ruoli server installati sui server locali o remoti. **Nota:** In Server Manager, i dati relativi ai ruoli e alle funzionalità vengono visualizzati nella lingua di base del sistema, denominata anche lingua GUI predefinita del sistema o la lingua selezionata durante l'installazione del sistema operativo.|Sì|Gli utenti standard possono visualizzare e gestire ruoli e funzionalità, nonché eseguire attività quali visualizzazione degli eventi ruolo, ma non possono aggiungere né rimuovere i servizi ruolo.|
|avviare strumenti di gestione come Windows PowerShell o snap-in MMC. È possibile avviare una sessione di Windows PowerShell destinata a un server remoto facendo clic con il pulsante destro del mouse sul server nel riquadro **Server** e quindi scegliendo **Windows PowerShell**. È possibile avviare gli snap-in MMC dal menu **strumenti** della console di Server Manager, quindi puntare lo MMC verso un computer remoto dopo l'apertura dello snap-in.|Sì|Sì|
|Gestire i server remoti con credenziali diverse facendo clic con il pulsante destro del mouse su un server nel riquadro **Server** e quindi scegliendo **Account di gestione**. È possibile utilizzare **Account di gestione** per attività di gestione generali per Servizi file e archiviazione e server.|Sì|No|
|Eseguire attività di gestione associate al ciclo di vita operativo dei server, ad esempio l'avvio o l'arresto di servizi; e avviare altri strumenti che consentono di configurare le impostazioni di rete di un server, gli utenti e i gruppi e le connessioni Desktop remoto.|Sì|Gli utenti standard non possono avviare o arrestare servizi. Possono modificare il nome del server locale, il gruppo di lavoro o l'appartenenza al dominio e le impostazioni di Desktop remoto, ma vengono richieste da controllo dell'account utente per fornire le credenziali di amministratore prima di poter completare queste attività. Non possono modificare le impostazioni di gestione remota.|
|Eseguire attività di gestione associate al ciclo di vita operativo dei ruoli installati nei server, tra cui l'analisi dei ruoli per verificarne la conformità alle procedure consigliate.|Sì|Gli utenti standard non possono eseguire analisi Best Practices Analyzer.|
|Determinare lo stato del server, identificare eventi critici, nonché analizzare e risolvere problemi o errori di configurazione.|Sì|Sì|
|Personalizzare gli eventi, i dati sulle prestazioni, i servizi e i risultati Best Practices Analyzer per i quali si desidera ricevere avvisi nel Dashboard Server Manager.|Sì|Sì|
|Riavviare i server.|Sì|No|
|Aggiornare i dati visualizzati nella console di Server Manager sui server gestiti.|Sì|No|

> [!NOTE]
> Server Manager non può essere utilizzato per aggiungere ruoli e funzionalità per i server che eseguono Windows Server 2008 R2 o Windows Server 2008.

## <a name="start-server-manager"></a>Avvia Server Manager
Per impostazione predefinita nei server che eseguono Windows Server 2016 quando un membro del gruppo Administrators accede a un server viene avviato automaticamente Server Manager. Se si chiude Server Manager, riavviarlo in uno dei modi seguenti. Questa sezione contiene anche i passaggi per modificare il comportamento predefinito e impedire l'avvio automatico del Server Manager.

#### <a name="to-start-server-manager-from-the-start-screen"></a>Per avviare Server Manager dalla schermata Start

-   Nella schermata **Start** di Windows fare clic sul riquadro **Server Manager** .

#### <a name="to-start-server-manager-from-the-windows-desktop"></a>Per avviare Server Manager dal desktop di Windows

-   Sulla barra delle applicazioni di Windows fare clic su **Server Manager**.

#### <a name="to-prevent-server-manager-from-starting-automatically"></a>Per impedire l'avvio automatico di Server Manager

1.  Nel menu **Gestisci** della console di Server Manager fare clic su **Server Manager Proprietà**.

2.  Nella finestra di dialogo **Proprietà di Server Manager** compilare la casella di controllo **Non avviare automaticamente Server Manager all'accesso**. Fare clic su **OK**.

3.  In alternativa, è possibile impedire l'avvio automatico di Server Manager abilitando l'impostazione Criteri di gruppo, **non avviare Server Manager automaticamente all'accesso**. Il percorso di questa impostazione dei criteri, nella console Criteri di gruppo Editor locale, è Configurazione computer\Modelli Amministrativi\sistema\server Manager.

## <a name="restart-remote-servers"></a>Riavviare i server remoti
È possibile riavviare un server remoto dal riquadro **Server** di una pagina ruolo o gruppo in Server Manager.

> [!IMPORTANT]
> Il riavvio di un server remoto comporta il riavvio del server, anche se gli utenti sono ancora connessi al server remoto e anche se sono ancora aperti programmi con dati non salvati. Questo comportamento si differenzia dall'arresto o dal riavvio del computer locale, su cui verrebbe richiesto di salvare i dati di programmi non salvati e di verificare che si desiderava indurre gli utenti connessi a disconnettersi. È sicuro che è possibile indurre altri utenti a disconnettersi dai server remoti e che è possibile eliminare i dati non salvati nei programmi eseguiti nei server remoti.
> 
> Se si verifica un aggiornamento automatico in Server Manager mentre è in corso l'arresto e il riavvio di un server gestito, possono verificarsi errori di stato di aggiornamento e gestibilità per il server gestito, perché Server Manager non è in grado di connettersi al server remoto finché non viene terminato il riavvio.

#### <a name="to-restart-remote-servers-in-server-manager"></a>Per riavviare server remoti in Server Manager

1.  Aprire un gruppo di ruoli o di server home page in Server Manager.

2.  Selezionare uno o più server remoti aggiunti a Server Manager. Premere e tenere premuto **CTRL** mentre si fa clic per selezionare più server contemporaneamente. Per ulteriori informazioni su come aggiungere server al pool di server Server Manager, vedere [aggiungere server a Server Manager](add-servers-to-server-manager.md).

3.  Fare clic con il pulsante destro del mouse sui server selezionati e quindi scegliere **Riavvia server**.

## <a name="export-server-manager-settings-to-other-computers"></a>Esportare le impostazioni di Server Manager ad altri computer
In Server Manager, l'elenco dei server gestiti, le modifiche alle impostazioni di console Server Manager e gruppi personalizzati creati vengono archiviati in due file seguenti. È possibile riutilizzare queste impostazioni su altri computer che eseguono la stessa versione di Server Manager (o Windows 10 con strumenti di amministrazione Server remota installato). Strumenti di amministrazione remota del Server deve essere in esecuzione nei computer basati su client Windows per esportare le impostazioni di Server Manager in tali computer.

-   %*AppData*% \ Microsoft\Windows\ServerManager\Serverlist.XML

-   %*AppData*% \ Local \ Microsoft_Corporation \Servermanager. exe_StrongName_*GUID*\6.2.0.0\User.config

> [!NOTE]
> -   Le credenziali Account di gestione (o alternative) per i server del pool di server non vengono archiviate nel profilo mobile. Gli utenti di Server Manager è necessario aggiungerli in ogni computer da cui si desidera gestire.
> -   Il profilo mobile della condivisione di rete non viene creato finché un utente non accede alla rete e non si disconnette per la prima volta. Il **Serverlist.xml** file viene creato in questo momento.

È possibile esportare le impostazioni di Server Manager, rendere le impostazioni Server Manager portabili o utilizzarle su altri computer in uno dei due modi seguenti.

-   Per esportare le impostazioni in un altro computer aggiunto al dominio, configurare l'utente Server Manager in modo che disponga di un profilo mobile in utenti e computer di Active Directory. Per modificare le proprietà utente in utenti e computer di Active Directory, è necessario essere un amministratore di dominio.

-   Per esportare le impostazioni in un altro computer di un gruppo di lavoro, copiare i due file precedenti nello stesso percorso nel computer da cui si desidera gestire utilizzando Server Manager.

#### <a name="to-export-server-manager-settings-to-other-domain-joined-computers"></a>Per esportare le impostazioni di Server Manager ad altri computer appartenenti al dominio

1.  In utenti e computer di Active Directory aprire la finestra di dialogo **Proprietà** per un utente di Server Manager.

2.  Nella scheda **profilo** aggiungere un percorso a una condivisione di rete per archiviare il profilo dell'utente.

3.  Effettuare una delle operazioni riportate di seguito.

    -   Nelle compilazioni in inglese (en-US) degli Stati Uniti, le modifiche apportate al file **Server. XML** vengono salvate automaticamente nel profilo. Andare al passaggio successivo.

    -   In altre Build copiare i due file seguenti dal computer che esegue Server Manager alla condivisione di rete che fa parte del profilo mobile dell'utente.

        -   %*AppData*% \ Microsoft\Windows\ServerManager\Serverlist.XML

        -   %*LocalAppData*% \ Microsoft_Corporation \Servermanager. exe_StrongName_*GUID*\6.2.0.0\User.config

4.  Fare clic su **OK** per salvare le modifiche e chiudere la finestra di dialogo **Proprietà**.

#### <a name="to-export-server-manager-settings-to-computers-in-workgroups"></a>Per esportare le impostazioni di Server Manager in computer dei gruppi di lavoro

-   In un computer da cui si desidera gestire i server remoti, sovrascrivere i due file seguenti con gli stessi file di un altro computer che esegue Server Manager e con le impostazioni desiderate.

    -   %*AppData*% \ Microsoft\Windows\ServerManager\Serverlist.XML

    -   %*LocalAppData*% \ Microsoft_Corporation \Servermanager. exe_StrongName_*GUID*\6.2.0.0\User.config



