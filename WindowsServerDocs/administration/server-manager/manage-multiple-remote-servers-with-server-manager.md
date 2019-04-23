---
title: Gestire più server remoti con Server Manager
description: Server Manager
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3a17e686-e7f2-47e2-b7af-733777c38b5f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4db3e486cec5cb065bfd202b7da2547be41a01a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847612"
---
# <a name="manage-multiple-remote-servers-with-server-manager"></a>Gestire più server remoti con Server Manager

>Si applica a: Windows Server 2016

Server Manager è una console di gestione in Windows Server 2012 R2 e Windows Server 2012 che aiuta i professionisti IT a effettuare il provisioning e gestire i server locali e remoti basati su Windows dai relativi desktop, senza richiedere l'accesso fisico ai server, o è necessario abilitare connessioni Remote Desktop protocol (rdP) per ogni server. Anche se Server Manager è disponibile in Windows Server 2008 R2 e Windows Server 2008, Server Manager è stato aggiornato in Windows Server 2012, per supportare la gestione multiserver remota e contribuire ad aumentare il numero di server che un amministratore può gestire.

Nei test, Server Manager in Windows Server 2012 R2 e Windows Server 2012 è utilizzabile per gestire fino a 100 server configurati con un carico di lavoro tipico. Il numero di server che è possibile gestire tramite un'unica console di Server Manager può variare a seconda della quantità di dati richiesti dai server gestiti e le risorse hardware e di rete disponibili nel computer che esegue Server Manager. Quando la quantità di dati da visualizzare si avvicina alla capacità delle risorse del computer, possono verificarsi rallentamenti nelle risposte di Server Manager e ritardi nel completamento degli aggiornamenti. Per aumentare il numero di server gestibile con Server Manager, si consiglia di limitare i dati dell'evento che Server Manager ottiene dai server gestiti usando le impostazioni nella finestra di dialogo **Configura dati evento**. La finestra di dialogo Configura dati evento può essere aperta dal menu **Attività** nel riquadro **Eventi** . Se è necessario gestire un numero di livello aziendale di server nell'organizzazione, è consigliabile valutare i prodotti di [suite di Microsoft System Center](https://go.microsoft.com/fwlink/p/?LinkId=239437).

In questo argomento e nei relativi sottoargomenti forniscono informazioni su come usare le funzionalità nella console di Server Manager. In questo argomento sono incluse le sezioni seguenti.

-   [Rivedere le considerazioni e requisiti di sistema](#BKMK_1.1)

-   [Attività che è possibile eseguire in Server Manager](#BKMK_tasks)

-   [avviare Server Manager](#BKMK_start)

-   [Riavviare i server remoti](#BKMK_restart)

-   [Esportare le impostazioni di Server Manager ad altri computer](#BKMK_export)

## <a name="BKMK_1.1"></a>Rivedere le considerazioni e requisiti di sistema
Le sezioni seguenti elencano alcune considerazioni iniziali che è necessario esaminare, nonché i requisiti hardware e software per Server Manager.

### <a name="hardware-requirements"></a>Requisiti hardware
Server Manager viene installato per impostazione predefinita con tutte le edizioni di Windows Server 2012 R2 e Windows Server 2012. Non esistono altri requisiti hardware per Server Manager.

### <a name="BKMK_softconfig"></a>Requisiti di configurazione e software
Server Manager viene installato per impostazione predefinita con tutte le edizioni di Windows Server 2012. Sebbene sia possibile utilizzare Server Manager per gestire [opzioni di installazione Server Core](https://go.microsoft.com/fwlink/p/?LinkID=241573) di Windows Server 2012 e Windows Server 2008 R2 in esecuzione su computer remoti, Server Manager non viene eseguito direttamente sulle installazioni Server Core Opzioni.

Per gestire completamente i server remoti che eseguono Windows Server 2008 o Windows Server 2008 R2, installare gli aggiornamenti seguenti nei server che si desidera gestire, nell'ordine indicato.

Per gestire i server che eseguono Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 con Server Manager in Windows Server 2012 R2, applicare gli aggiornamenti seguenti ai sistemi operativi precedenti.

-   [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)

-   [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881). Il pacchetto di download di Windows Management Framework 4.0 Aggiorna provider Strumentazione gestione Windows (WMI) in Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008. Il provider WMI aggiornati consentono a Server Manager di raccogliere informazioni sui ruoli e funzionalità installati sui server gestiti. Fino a quando non viene applicato l'aggiornamento, i server che eseguono Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 con stato gestibilità **non è accessibile**.

-   L'aggiornamento delle prestazioni associati [articolo della Knowledge Base 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) consente a Server Manager per raccogliere i dati sulle prestazioni da Windows Server 2008 e Windows Server 2008 R2. Questo aggiornamento per le prestazioni non è necessario nei server che eseguono Windows Server 2012.

Per gestire i server che eseguono Windows Server 2008 R2 o Windows Server 2008, applicare gli aggiornamenti seguenti ai sistemi operativi precedenti.

-   [.NET framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)

-   [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) pacchetto di download di Windows Management Framework 3.0 Aggiorna provider Strumentazione gestione Windows (WMI) su Windows Server 2008 e Windows Server 2008 R2. Il provider WMI aggiornati consentono a Server Manager di raccogliere informazioni sui ruoli e funzionalità installati sui server gestiti. Fino a quando non viene applicato l'aggiornamento, i server che eseguono Windows Server 2008 o Windows Server 2008 R2 con stato gestibilità **non accessibile: verificare le versioni precedenti eseguire Windows Management Framework 3.0**.

-   L'aggiornamento delle prestazioni associati [articolo della Knowledge Base 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) consente a Server Manager per raccogliere i dati sulle prestazioni da Windows Server 2008 e Windows Server 2008 R2.

Server Manager viene eseguito nell'interfaccia grafica Server minima, vale a dire quando la funzionalità Shell grafica Server disinstallata. La funzionalità Shell grafica Server è installata per impostazione predefinita in Windows Server 2012 R2 e Windows Server 2012. Se si disinstalla Shell grafica Server, la console di Server Manager viene eseguita, ma alcune applicazioni o strumenti della console non sono disponibili. I browser Internet non è possibile eseguire senza Shell grafica Server, le pagine Web e applicazioni, ad esempio della Guida HTML (la Guida F1 di mmc, ad esempio) non può essere aperto. Non è possibile aprire finestre di dialogo per la configurazione di Windows di aggiornamenti automatici e feedback quando Shell grafica Server non è installata; i comandi che aprono queste finestre di dialogo nella console di Server Manager vengono reindirizzati per eseguire **sconfig. cmd**.

#### <a name="manage-remote-computers-from-a-client-computer"></a>Gestire i computer remoti da un computer client
Console di Server Manager è inclusa con [strumenti di amministrazione remota del Server](https://go.microsoft.com/fwlink/?LinkID=304145) per Windows 8.1 e [strumenti di amministrazione remota del Server](https://go.microsoft.com/fwlink/p/?LinkID=238560) per Windows 8. Si noti che quando viene installato Strumenti di amministrazione remota del Server in un computer client, è possibile gestire il computer locale tramite Server Manager; Server Manager può essere utilizzata per gestire i computer o dispositivi che eseguono un sistema operativo client Windows. È possibile utilizzare Server Manager per gestire solo i server basati su Windows.

|Sistema operativo di gestione di server di origine|Destinata a Windows Server 2012 R2 |Destinato a Windows Server 2012 |Destinati a Windows Server 2008 R2 o Windows Server 2008 |Destinato a Windows Server 2003|
|-------------------------------|---------------------------------------|------------------------------------|-----------------------------------------------------------------------|------------------|
|Windows 8 o Windows Server 2012 |Non supportato|Supporto completo|Quando i [Requisiti di configurazione e software](#BKMK_softconfig) sono soddisfatti, è possibile eseguire la maggiore parte delle attività di gestione, ad eccezione dell'installazione o della disinstallazione di ruoli o funzionalità|Supporto limitato; solo stato online e offline|
|Windows 8.1 o Windows Server 2012 R2 |Supporto completo|Supporto completo|Quando i [Requisiti di configurazione e software](#BKMK_softconfig) sono soddisfatti, è possibile eseguire la maggiore parte delle attività di gestione, ad eccezione dell'installazione o della disinstallazione di ruoli o funzionalità|Supporto limitato; solo stato online e offline|

###### <a name="to-start-server-manager-on-a-client-computer"></a>Per avviare Server Manager in un computer client

1.  Seguire le istruzioni [distribuire strumenti di amministrazione Server remota](https://go.microsoft.com/fwlink/?LinkID=238562) installare Remote Server Administration Tools per Windows 8.1 o Remote Server Administration Tools per Windows 8.

2.  Nel **avviare** schermata, fare clic su **Server Manager**. Il riquadro **Server Manager** è disponibile dopo l'installazione di Strumenti di amministrazione remota del Server.

3.  Se non si specifica la **strumenti di amministrazione** né la **Server Manager** i riquadri vengono visualizzati nella **avviare** schermata dopo l'installazione di Remote Server Administration Tools, e la ricerca per Server Manager nel **avviare** dello schermo non vengono visualizzati risultati, verificare che il **Mostra strumenti di amministrazione** è attivata. Per visualizzare questa impostazione, passare il cursore del mouse sull'angolo superiore destro della **avviare** schermata e quindi fare clic su **impostazioni**. Se l'impostazione **Mostra strumenti di amministrazione** è disattivata, attivarla per visualizzare gli strumenti installati nell'ambito di Strumenti di amministrazione remota del server.

per altre informazioni sull'esecuzione di remoto Server Administration Tools per Windows 8 per gestire i server remoti, vedere [strumenti di amministrazione remota del Server](https://go.microsoft.com/fwlink/?LinkID=221055) su Wiki di TechNet.

#### <a name="configure-remote-management-on-servers-that-you-want-to-manage"></a>Configurare la gestione remota su server che si desidera gestire

> [!IMPORTANT]
> Per impostazione predefinita, gestione remota di Server Manager e Windows PowerShell è abilitata in Windows Server 2012 R2 e Windows Server 2012.

Per eseguire attività di gestione su server remoti utilizzando Server Manager, i server remoti che si desidera gestire devono essere configurati per consentire la gestione remota tramite Server Manager e Windows PowerShell. Gestione remota è stata disabilitata in Windows Server 2012 R2 o Windows Server 2012, se si desidera riabilitarla, eseguire la procedura seguente.

##### <a name="BKMK_windows"></a>Per configurare Gestione remota di Server Manager in Windows Server 2012 R2 o Windows Server 2012 tramite l'interfaccia di Windows

1.  > [!NOTE]
    > Le impostazioni controllate dal **configurare la gestione remota** nella finestra di dialogo non riguardano parti di Server Manager che utilizzano DCOM per le comunicazioni remote.

    Eseguire una delle operazioni seguenti per aprire Server Manager, se non è già aperto.

    -   Sulla barra delle applicazioni di Windows, fare clic sul pulsante di Server Manager.

    -   Nel **avviare** schermata, fare clic su **Server Manager**.

2.  Nel **delle proprietà** area della **server locali** pagina, fare clic sul valore con collegamento ipertestuale per il **gestione remota** proprietà.

3.  Eseguire una delle operazioni seguenti, quindi fare clic su **OK**.

    -   Per impedire che il computer gestito in modalità remota tramite Server Manager (o Windows PowerShell se è installato), deselezionare il **abilitare la gestione remota del server da altri computer** casella di controllo.

    -   Per consentire a questo computer di gestione remota tramite Server Manager o Windows PowerShell, selezionare **Consenti gestione remota del server da altri computer**.

##### <a name="BKMK_ps"></a>Per abilitare gestione remota di Server Manager in Windows Server 2012 R2 o Windows Server 2012 tramite Windows PowerShell

1.  Effettuare una delle operazioni seguenti.

    -   Per eseguire Windows PowerShell come amministratore dal **avviare** schermata, fare doppio clic il **Windows PowerShell** riquadro e quindi fare clic su **Esegui come amministratore**.

    -   Per eseguire Windows PowerShell come amministratore dal desktop, fare doppio clic il **Windows PowerShell** scelta rapida nella barra delle applicazioni e quindi fare clic su **Esegui come amministratore**.

2.  digitare il comando seguente e quindi premere **invio** per abilitare tutte le eccezioni alle regole firewall necessarie.

    **Configure-SMremoting.exe -Enable**

    > [!NOTE]
    > Questo comando funziona anche in un prompt dei comandi aperto con diritti utente elevati (Esegui come amministratore).

    in caso di abilitazione della gestione remota, vedere [about_remote_Troubleshooting](https://go.microsoft.com/fwlink/p/?LinkID=135188) su Microsoft TechNet per la risoluzione dei suggerimenti e procedure consigliate.

###### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-older-operating-systems"></a>Per abilitare la gestione remota di Server Manager e Windows PowerShell in sistemi operativi precedenti

-   Effettuare una delle operazioni seguenti.

    -   Per abilitare gestione remota sui server che eseguono Windows Server 2008 R2, consultare [gestione remota con Server Manager](https://go.microsoft.com/fwlink/?LinkID=137378) nella Guida di Windows Server 2008 R2.

    -   Per abilitare gestione remota sui server che eseguono Windows Server 2008, vedere [abilitare e usare i comandi remoti in Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=242565).

    -   Per abilitare la gestione remota su server che eseguono Windows Server 2003, abilitare le eccezioni DCOM di WMI in Windows Firewall. Per altre informazioni su come eseguire questa operazione sui server che eseguono Windows Server 2003, vedere [Connessione attraverso Windows Firewall](https://msdn.microsoft.com/library/aa389286.aspx) su MSDN.

## <a name="BKMK_tasks"></a>Attività che è possibile eseguire in Server Manager
Server Manager rendono l'amministrazione del server più efficiente consentendo agli amministratori di eseguire le attività nella tabella seguente tramite un singolo strumento. In Windows Server 2012 R2 e Windows Server 2012, sia gli utenti standard di un server sia i membri del gruppo Administrators possono eseguire attività di gestione in Server Manager, ma per impostazione predefinita, gli utenti standard sono autorizzati a eseguire determinate attività, come indicato di Nella tabella seguente.

Gli amministratori possono usare due cmdlet di Windows PowerShell nel modulo per i cmdlet di Server Manager [Enable ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205470.aspx) e [Disable ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205468.aspx), a controllare ulteriormente l'accesso utente standard ad alcuni dati aggiuntivi. Il **Enable ServerManagerStandardUserremoting** cmdlet può fornire uno o più accessi utente standard senza privilegi di amministratore per eventi, service, contatore delle prestazioni e i dati di inventario di ruolo e funzionalità.

> [!IMPORTANT]
> Server Manager può essere utilizzata per gestire una versione più recente del sistema operativo Windows Server. Server Manager in esecuzione in Windows Server 2012 o Windows 8 può essere utilizzata per gestire i server che eseguono Windows Server 2012 R2.

|Descrizione dell'attività|Administrators (incluso l'account Administrator predefinito)|Utenti server standard|
|----------|----------------------------------|-------------|
|aggiungere server remoti a un pool di server che Server Manager può essere usato per gestire.|Yes|No|
|creare e modificare gruppi personalizzati di server, ad esempio i server che si trovano in una posizione geografia specifica o uno scopo specifico.|Yes|Yes|
|Installare o disinstallare ruoli, servizi ruolo e funzionalità in locale o in server remoti che eseguono Windows Server 2012 R2 o Windows Server 2012. Per le definizioni dei ruoli, servizi ruolo e funzionalità, vedere [ruoli, servizi ruolo e funzionalità](https://go.microsoft.com/fwlink/p/?LinkId=239558).|Yes|No|
|Visualizzare e modificare le funzionalità e i ruoli server installati sui server locali o remoti. **Nota:** In Server Manager, i dati di ruolo e funzionalità vengono visualizzati nella lingua di base del sistema, denominato anche la lingua GUI predefinita del sistema o la lingua selezionata durante l'installazione del sistema operativo.|Yes|Gli utenti standard possono visualizzare e gestire ruoli e funzionalità, nonché eseguire attività quali visualizzazione degli eventi ruolo, ma non possono aggiungere né rimuovere i servizi ruolo.|
|avviare strumenti di gestione, ad esempio Windows PowerShell o lo snap-in di mmc. È possibile avviare una sessione di Windows PowerShell destinata a un server remoto facendo clic con il server nel **i server** riquadro e scegliendo **Windows PowerShell**. È possibile avviare lo snap-in mmc dal **strumenti** menu della console Server Manager e quindi puntare il mmc su un computer remoto dopo lo snap-in è aperto.|Yes|Yes|
|Gestire i server remoti con credenziali diverse facendo clic con il pulsante destro del mouse su un server nel riquadro **Server** e quindi scegliendo **Account di gestione**. È possibile utilizzare **Account di gestione** per attività di gestione generali per Servizi file e archiviazione e server.|Yes|No|
|Eseguire attività di gestione associata con il ciclo di vita operativo dei server, quali avvio o arresto dei servizi; e avviare altri strumenti che consentono di configurare le impostazioni di rete del server, utenti e gruppi e le connessioni Desktop remoto.|Yes|Gli utenti standard non possono avviare o arrestare servizi. Può modificare nome, gruppo di lavoro o l'appartenenza al dominio e le impostazioni Desktop remoto del server locale, ma viene richiesto da controllo Account utente di fornire le credenziali di amministratore prima di poter completare queste attività. Non possono modificare le impostazioni di gestione remota.|
|Eseguire attività di gestione associate al ciclo di vita operativo dei ruoli installati nei server, tra cui l'analisi dei ruoli per verificarne la conformità alle procedure consigliate.|Yes|Gli utenti standard non è possibile eseguire le analisi di Best Practices Analyzer.|
|Determinare lo stato del server, identificare eventi critici, nonché analizzare e risolvere problemi o errori di configurazione.|Yes|Yes|
|Personalizzazione di eventi, i dati sulle prestazioni, servizi e i risultati di Best Practices Analyzer intorno al quale si desidera essere avvisati sul dashboard di Server Manager.|Yes|Yes|
|Riavviare i server.|Yes|No|
|Aggiornare i dati che viene visualizzati nella console di gestione Server sui server gestiti.|Yes|No|

> [!NOTE]
> Server Manager può ricevere lo stato solo online o offline dai server che eseguono Windows Server 2003. Server Manager può essere utilizzata per aggiungere ruoli e funzionalità ai server che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_start"></a>avviare Server Manager
Per impostazione predefinita nei server che eseguono Windows Server 2012 quando un membro del gruppo Administrators effettua l'accesso a un server viene avviato automaticamente Server Manager. Se si chiude Server Manager, riavviarlo in uno dei modi seguenti. In questa sezione contiene inoltre i passaggi per la modifica del comportamento predefinito e impedisce l'avvio automatico di Server Manager.

#### <a name="to-start-server-manager-from-the-start-screen"></a>Per avviare Server Manager dalla schermata start

-   Nella finestra di Windows **avviare** schermata, fare clic il **Server Manager** riquadro.

#### <a name="to-start-server-manager-from-the-windows-desktop"></a>Per avviare Server Manager dal desktop di Windows

-   Sulla barra delle applicazioni di Windows fare clic su **Server Manager**.

#### <a name="to-prevent-server-manager-from-starting-automatically"></a>Per impedire l'avvio automatico di Server Manager

1.  Nella console di Server Manager, sul **Gestisci** menu, fare clic su **delle proprietà di Server Manager**.

2.  Nella finestra di dialogo **Proprietà di Server Manager** compilare la casella di controllo **Non avviare automaticamente Server Manager all'accesso**. Fare clic su **OK**.

3.  In alternativa, è possibile impedire Server Manager di avvio automatico abilitando l'impostazione di criteri di gruppo **non avviare automaticamente Server Manager all'accesso**. Il percorso di questa impostazione di criteri, nella console di editor Criteri di gruppo locali è computer Configurazione computer\Modelli amministrativi\sistema\server Manager.

## <a name="BKMK_restart"></a>Riavviare i server remoti
È possibile riavviare un server remoto dal **server** porzione di una pagina ruolo o gruppo in Server Manager.

> [!IMPORTANT]
> Il riavvio di un server remoto comporta il riavvio del server, anche se gli utenti sono ancora connessi al server remoto e anche se sono ancora aperti programmi con dati non salvati. Questo comportamento si differenzia dall'arresto o dal riavvio del computer locale, su cui verrebbe richiesto di salvare i dati di programmi non salvati e di verificare che si desiderava indurre gli utenti connessi a disconnettersi. È sicuro che è possibile indurre altri utenti a disconnettersi dai server remoti e che è possibile eliminare i dati non salvati nei programmi eseguiti nei server remoti.
> 
> Se un aggiornamento automatico in Server Manager avviene mentre un server gestito è in corso l'arresto e il riavvio, lo stato di aggiornamento e alla gestibilità per il server gestito, possono verificarsi errori perché Server Manager non è possibile connettersi al server remoto fino al termine il riavvio.

#### <a name="to-restart-remote-servers-in-server-manager"></a>Per riavviare server remoti in Server Manager

1.  Aprire una gruppo home page di ruoli o di server in Server Manager.

2.  Selezionare uno o più server remoti aggiunti a Server Manager. Premere e tenere premuto **CTRL** mentre si fa clic per selezionare più server contemporaneamente. Per altre informazioni su come aggiungere server al pool di server di gestione, vedere [aggiungere server a Server Manager](add-servers-to-server-manager.md).

3.  Fare clic con il pulsante destro del mouse sui server selezionati e quindi scegliere **Riavvia server**.

## <a name="BKMK_export"></a>Esportare le impostazioni di Server Manager ad altri computer
In Server Manager, l'elenco dei server gestiti, le modifiche alle impostazioni di console Server Manager e gruppi personalizzati creati vengono archiviati in due file seguenti. È possibile riutilizzare queste impostazioni su altri computer che eseguono la stessa versione di Server Manager (non i computer che eseguono l'opzione di installazione Server Core) o Windows 8. Strumenti di amministrazione remota del Server deve essere in esecuzione nei computer basati su client Windows per esportare le impostazioni di Server Manager in tali computer.

-   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

-   %*AppData*%\Local\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

> [!NOTE]
> -   Le credenziali Account di gestione (o alternative) per i server del pool di server non vengono archiviate nel profilo mobile. Gli utenti di Server Manager è necessario aggiungerli in ogni computer da cui si desidera gestire.
> -   Il profilo mobile della condivisione di rete non viene creato finché un utente non accede alla rete e non si disconnette per la prima volta. Il **Serverlist.xml** file viene creato in questo momento.

È possibile esportare le impostazioni di Server Manager, rendere portatili le impostazioni di Server Manager o utilizzarle su altri computer in uno dei due modi seguenti.

-   Per esportare le impostazioni in un altro computer aggiunti al dominio, configurare l'utente di Server Manager per disporre di un profilo mobile in active directory gli utenti e computer. È necessario essere un amministratore di dominio per modificare le proprietà utente in active directory gli utenti e computer.

-   Per esportare le impostazioni in un altro computer in un gruppo di lavoro, copiare i due file precedenti nello stesso percorso nel computer da cui si desidera gestire utilizzando Server Manager.

#### <a name="to-export-server-manager-settings-to-other-domain-joined-computers"></a>Per esportare le impostazioni di Server Manager ad altri computer appartenenti al dominio

1.  In active directory utenti e computer, aprire il **proprietà** finestra di dialogo per un utente di Server Manager.

2.  Nel **profilo** scheda, aggiungere un percorso a una condivisione di rete per archiviare il profilo dell'utente.

3.  Effettuare una delle operazioni seguenti.

    -   Nelle build in Inglese (en-us) le compilazioni, diventa il **serverlist** file vengono automaticamente salvate nel profilo. Andare al passaggio successivo.

    -   In altre build copiare i due file seguenti dal computer che esegue Server Manager per la condivisione di rete che fa parte del profilo mobile dell'utente.

        -   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

        -   %*localappdata*%\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

4.  Fare clic su **OK** per salvare le modifiche e chiudere la finestra di dialogo **Proprietà** .

#### <a name="to-export-server-manager-settings-to-computers-in-workgroups"></a>Per esportare le impostazioni di Server Manager in computer dei gruppi di lavoro

-   In un computer da cui si desidera gestire i server remoti sovrascrivere i due file seguenti con gli stessi file da un altro computer che esegue Server Manager e che contiene le impostazioni desiderate.

    -   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

    -   %*localappdata*%\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config


