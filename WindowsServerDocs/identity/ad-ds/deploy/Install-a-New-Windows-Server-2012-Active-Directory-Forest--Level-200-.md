---
ms.assetid: b3d6fb87-c4d4-451c-b3de-a53d2402d295
title: Installare una nuova foresta di Active Directory di Windows Server 2012 (livello 200)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b8a7502a1b9d27b0f61353f2544182a64d311496
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-new-windows-server-2012-active-directory-forest-level-200"></a>Installare una nuova foresta di Active Directory di Windows Server 2012 (livello 200)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento illustra la nuova funzionalità innalzamento di livello controller di dominio Windows Server 2012 Active Directory Domain Services a livello introduttivo. In Windows Server 2012, servizi di dominio Active Directory sostituisce lo strumento Dcpromo con Server Manager e sistema di distribuzione basato su Windows PowerShell.  
  
-   [Amministrazione semplificata di servizi di dominio Active Directory](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SimplifiedAdmin)  
  
-   [Panoramica tecnica](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_TechOverview)  
  
-   [Distribuzione di una foresta con Server Manager](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [Distribuzione di una foresta con Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_PSForest)  
  
## <a name="BKMK_SimplifiedAdmin"></a>Amministrazione semplificata di servizi di dominio Active Directory  
Windows Server 2012 introduce la nuova generazione di Active Directory dominio amministrazione semplificata di servizi ed è l'innovazione più radicale realizzata dopo Windows 2000 Server. Amministrazione semplificata di Active Directory sfrutta appresi dodici anni di Active Directory e ne un'esperienza amministrativa più intuitiva più supportata, flessibile, ad architetti e amministratori. In questo modo la creazione di nuove versioni di tecnologie esistenti oltre all'estensione delle funzionalità dei componenti rilasciati in Windows Server 2008 R2.  
  
### <a name="what-is-ad-ds-simplified-administration"></a>Amministrazione semplificata di novità di dominio Active Directory?  
Amministrazione semplificata di Active Directory è una nuova forma della distribuzione dei domini. Alcune di queste funzionalità includono:  
  
-   Distribuzione di ruolo di Active Directory fa ora parte della nuova architettura di Server Manager e consente l'installazione remota.  
  
-   Il motore di distribuzione e configurazione di dominio Active Directory è ora Windows PowerShell, anche quando si usa un'installazione grafica.  
  
-   L'innalzamento di livello ora include il controllo dei prerequisiti che convalida la disponibilità di foreste e domini per il nuovo controller di dominio, riducendo la possibilità di innalzamenti di livello riusciti.  
  
-   La funzionalità livello non implementa nuove caratteristiche e il livello di funzionalità del dominio è richiesto solo per un subset di nuove caratteristiche Kerberos, sollevando gli amministratori di frequente della foresta Windows Server 2012 è necessario per un ambiente di controller di dominio omogeneo.  
  
### <a name="purpose-and-benefits"></a>Scopo e vantaggi  
Queste modifiche potrebbero apparire più complesse anziché più semplici. Nella riprogettazione del processo di distribuzione di dominio Active Directory, tuttavia, si è verificato l'opportunità di unire diversi passaggi e procedure consigliate in azioni meno numerose e più semplice. Ciò significa, ad esempio, è possibile che la configurazione grafica di un nuovo controller di dominio di replica è ora otto finestre di dialogo invece le dodici precedenti. La creazione di una nuova foresta di Active Directory richiede un *singolo* comando di Windows PowerShell con solo *uno* argomento: il nome del dominio.  
  
Perché è l'importanza di Windows PowerShell in Windows Server 2012? Durante l'elaborazione distribuita continua a evolvere, Windows PowerShell consente a un solo motore per la configurazione e manutenzione dai grafica e interfacce della riga di comando. Consente di creare lo script completo di qualsiasi componente con lo stesso farebbero per professionisti IT che concede un'API per gli sviluppatori. Come elaborazione basata su cloud diventa universale, Windows PowerShell offre infine la possibilità di amministrare in modalità remota un server, in cui un computer senza interfaccia grafica ha le stesse funzionalità di gestione di uno con monitor e mouse.  
  
Un amministratore di dominio Active Directory veterano dovresti trovare frutto le proprie precedenti conoscenze. Un amministratore di inizio troveranno una curva di apprendimento sperimenterà.  
  
## <a name="BKMK_TechOverview"></a>Panoramica tecnica  
  
### <a name="what-you-should-know-before-you-begin"></a>Cosa è necessario conoscere prima di iniziare  
In questo argomento si presuppone una familiarità con le versioni precedenti di servizi di dominio Active Directory e non vengono forniti i dettagli sullo scopo e sulle funzionalità. Per ulteriori informazioni su servizi di dominio Active Directory, vedere le pagine del portale TechNet collegate di sotto:  
  
-   [Servizi di dominio Active Directory per Windows Server 2008 R2](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
  
-   [Servizi di dominio Active Directory per Windows Server 2008](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
  
-   [Documentazione tecnica su Windows Server](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
  
### <a name="functional-descriptions"></a>Descrizioni delle funzionalità  
  
#### <a name="ad-ds-role-installation"></a>Installazione del ruolo di directory Active Directory  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_SelectServerRoles.gif)  
  
L'installazione di Active Directory Domain Services Usa Server Manager e Windows PowerShell, come tutti gli altri ruoli server e funzionalità in Windows Server 2012. Il programma Dcpromo.exe non fornisce più opzioni di configurazione GUI.  
  
Utilizzare una procedura guidata grafica in Server Manager o il modulo ServerManager per Windows PowerShell nelle installazioni locali e remote. Eseguendo più istanze di queste procedure guidate o cmdlet e destinazione server diversi, è possibile distribuire servizi di dominio Active Directory in più controller di dominio a contemporaneamente, tutti da una sola console. Sebbene queste nuove funzionalità non sono compatibili con Windows Server 2008 R2 o sistemi operativi precedenti, è possibile utilizzare anche l'applicazione Dism.exe introdotta in Windows Server 2008 R2 per l'installazione locale del ruolo dalla classica riga di comando.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSAddWindowsFeature.png)  
  
#### <a name="ad-ds-role-configuration"></a>Configurazione del ruolo di directory Active Directory  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
Configurazione di Active Directory Domain Services "è noto in precedenza come DCPROMO" è ora un'operazione distinta dall'installazione del ruolo. Dopo aver installato il ruolo di dominio Active Directory, un amministratore configura il server come controller di dominio tramite un'altra procedura guidata in Server Manager o il modulo ADDSDeployment di Windows PowerShell.  
  
Configurazione di ruolo di Active Directory si basa su dodici anni di esperienza sul campo e ora consente di configurare i controller di dominio in base alle più recenti procedure consigliate Microsoft. Domain Name System e i cataloghi globali, ad esempio, installare per impostazione predefinita in ogni controller di dominio.  
  
La configurazione guidata di Active Directory di Server Manager unisce più finestre di dialogo in meno prompt e non nasconde più le impostazioni in una modalità "avanzata". Il processo di innalzamento di livello intero è in una finestra di dialogo a espansione durante l'installazione. La procedura guidata e il modulo ADDSDeployment di Windows PowerShell mostrano modifiche degne di nota e problemi di sicurezza, con collegamenti a ulteriori informazioni.  
  
Dcpromo.exe rimane in Windows Server 2012 per solo le installazioni automatiche della riga di comando e non esegue l'installazione guidata grafica. Si consiglia interrompere l'uso di Dcpromo.exe per le installazioni automatiche e sostituirlo con il modulo ADDSDeployment, come l'eseguibile attualmente deprecato non verrà incluso nella prossima versione di Windows.  
  
Queste nuove funzionalità non sono compatibili con le versioni precedenti di Windows Server 2008 R2 o sistemi operativi precedenti.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSForest.png)  
  
> [!IMPORTANT]  
> Dcpromo.exe non contiene più una procedura guidata grafica e non installa più file binari del ruolo o funzionalità. Tentativo di eseguire Dcpromo.exe dalla shell Esplora restituisce:  
>   
> "L'installazione guidata servizi di dominio Active Directory è stato spostato in Server Manager. For more information, see https://go.microsoft.com/fwlink/?LinkId=220921."  
>   
> Il tentativo di eseguire Dcpromo.exe /unattend ancora installa i file binari, come nei sistemi operativi precedenti, ma viene visualizzato un avviso:  
>   
> "Il dcpromo in modalità automatica viene sostituito dal modulo ADDSDeployment per Windows PowerShell. For more information, see https://go.microsoft.com/fwlink/?LinkId=220924."  
>   
> Dcpromo.exe è deprecato in Windows Server 2012 e non sarà incluso nelle versioni future di Windows né verrà ulteriormente miglioramenti in questo sistema operativo. Gli amministratori devono interromperne l'uso e passare a moduli supportati di Windows PowerShell se desiderano creare controller di dominio dalla riga di comando.  
  
#### <a name="prerequisite-checking"></a>Controllo dei prerequisiti  
Configurazione del controller di dominio implementa anche una fase di controllo dei prerequisiti che valuta la foresta e dominio prima di continuare con il controller di dominio. Sono inclusi disponibilità del ruolo FSMO, privilegi utente, compatibilità dello schema esteso e altri requisiti. Questa nuova progettazione riduce i problemi in promozione del controller di dominio inizia e quindi si ferma a metà con un errore di configurazione irreversibile. Questo riduce la possibilità dei metadati di controller di dominio orfani nella foresta o un server che si ritiene erroneamente è un controller di dominio.  
  
## <a name="BKMK_SMForest"></a>Distribuzione di una foresta con Server Manager  
Questa sezione illustra come installare il primo controller di dominio in un dominio radice della foresta con Server Manager in un computer Windows Server 2012 grafico.  
  
### <a name="server-manager-ad-ds-role-installation-process"></a>Processo di installazione di ruolo di Server Manager AD DS  
Il diagramma seguente illustra il processo di installazione del ruolo Servizi di dominio Active Directory, inizia con l'esecuzione di ServerManager.exe e termina prima la promozione del controller di dominio.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment.png)  
  
#### <a name="server-pool-and-add-roles"></a>Server del Pool e aggiungere ruoli  
Tutti i computer Windows Server 2012 accessibili dal computer che esegue Server Manager sono idonei per il pool. Una volta pool, selezionare i server per l'installazione remota di servizi di dominio Active Directory o di altre opzioni di configurazione disponibili in Server Manager.  
  
Per aggiungere server, scegliere una delle operazioni seguenti:  
  
-   Fare clic su **aggiungere altri server da gestire** nel riquadro iniziale del dashboard  
  
-   Fare clic su di **Gestisci** dal menu **Aggiungi server**  
  
-   Fare doppio clic su **tutti i server** e scegli **Aggiungi server**  
  
Verrà visualizzata la finestra di dialogo Aggiungi server:  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddServers.png)  
  
Questo offre tre modi per aggiungere server al pool per usarli o raggrupparli:  
  
-   Ricerca in Active Directory (utilizza LDAP, richiede che i computer appartengano a un dominio, consente di filtrare i sistemi operativi e supporta i caratteri jolly)  
  
-   Ricerca DNS (utilizza gli alias DNS o l'indirizzo IP tramite ARP, trasmissioni NetBIOS o ricerca WINS, non consente il sistema operativo filtro supporta i caratteri jolly)  
  
-   Importazione (utilizza un elenco di file di testo di server separati da CR/LF)  
  
Fare clic su **trova** per restituire un elenco di server dallo stesso dominio Active Directory che il computer è unito a, fare clic su uno o più nomi di server dall'elenco dei server. Fare clic sulla freccia destra per aggiungere i server per il **selezionati** elenco. Utilizzare il **Aggiungi server** finestra di dialogo per aggiungere i server selezionati ai gruppi di ruoli del dashboard. Oppure fare clic su **Gestisci**, quindi fare clic su **Crea gruppo di Server**, oppure fare clic su **Crea gruppo di Server** nel dashboard di **iniziale a Server Manager** riquadro per creare gruppi di server personalizzati.  
  
> [!NOTE]  
> La procedura di aggiunta di server non convalida che un server è online o accessibile. Tuttavia, tutti i server non raggiungibili flag nella visualizzazione gestibilità di Server Manager al successivo aggiornamento  
  
È possibile installare ruoli in modalità remota in qualsiasi Windows Server 2012 computer aggiunto al pool, come illustrato:  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/tADDS_SMI_TR_AddRolesFeatures.png)  
  
È possibile gestire completamente server che eseguono sistemi operativi precedenti a Windows Server 2012. Il **Aggiungi ruoli e funzionalità** selezione è in esecuzione il modulo ServerManager Windows PowerShell **Install-WindowsFeature**.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddADDSToAnotherServer.png)  
  
Inoltre, è possibile utilizzare il Dashboard di Server Manager in un controller di dominio esistente per selezionare il server remoto AD DS installazione con il ruolo già preselezionato facendo clic con il pulsante destro sul riquadro dashboard Active Directory e selezionando **aggiungere servizi di dominio Active Directory a un altro Server**. Verrà richiamato **Install-WindowsFeature AD-Domain-Services**.  
  
Il computer in che è in esecuzione Server Manager pool stesso automaticamente. Per installare il ruolo del dominio Active Directory, è sufficiente fare clic il **Gestisci** dal menu **Aggiungi ruoli e funzionalità**.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ManageAddRoles.png)  
  
#### <a name="installation-type"></a>Tipo di installazione  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectInstallationType.png)  
  
Il **tipo di installazione** finestra di dialogo contiene un'opzione che non supporta servizi di dominio Active Directory: il **scenario di Servizi Desktop remoto in base-installazione**. Questa opzione consente solo servizio Desktop remoto in un carico di lavoro distribuito multiserver. Se si seleziona, è possibile installare servizi di dominio Active Directory.  
  
Lasciare sempre la selezione predefinita presenti quando si installa Servizi di dominio Active Directory: **installazione basata su ruoli o basata su funzionalità**.  
  
#### <a name="server-selection"></a>Selezione dei server  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectDestinationServer.png)  
  
Il **selezione dei Server** finestra di dialogo consente di scegliere uno dei server aggiunti in precedenza al pool, purché sia accessibile. Il server locale che esegue Server Manager è automaticamente disponibile.  
  
Inoltre, è possibile selezionare i file di disco rigido virtuale Hyper-V non in linea con il sistema operativo Windows Server 2012 e Server Manager aggiunge il ruolo ad essi direttamente tramite manutenzione dei componenti. Ciò consente di eseguire il provisioning di server virtuali con i componenti necessari prima di configurarli ulteriormente.  
  
#### <a name="server-roles-and-features"></a>Funzionalità e ruoli del server  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectServerRoles.png)  
  
Selezionare il **servizi di dominio Active Directory** ruolo se per alzare di livello un controller di dominio. Tutte le funzionalità di amministrazione di Active Directory e i servizi necessari vengono installati automaticamente, anche se apparentemente fanno parte di un altro ruolo o non risultano selezionati nell'interfaccia di Server Manager.  
  
Server Manager viene anche visualizzata una finestra di dialogo informativa che mostra le funzionalità di gestione di questo ruolo viene installato in modo implicito; Questa operazione equivale al **- IncludeManagementTools** argomento.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddFeaturesDialog.gif)  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectFeatures.png)  
  
Ulteriori **funzionalità** è possibile aggiungere qui come desiderato.  
  
#### <a name="active-directory-domain-services"></a>Servizi di dominio Active Directory  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSIntro.png)  
  
Il **servizi di dominio Active Directory** finestra di dialogo contiene informazioni limitate sui requisiti e procedure consigliate. Funge essenzialmente da un messaggio di conferma che si è scelto il ruolo di dominio Active Directory "Se non viene visualizzata questa schermata, non è stata selezionata AD DS.  
  
#### <a name="confirmation"></a>Conferma  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Confirmation.png)  
  
Il **conferma** finestra di dialogo è l'ultimo controllo prima dell'avvio di installazione del ruolo. Offre un'opzione per riavviare il computer, se necessario, dopo l'installazione del ruolo, ma l'installazione di Active Directory non è necessario un riavvio.  
  
Facendo clic **installare**, confermare di essere pronto per iniziare l'installazione del ruolo. È possibile annullare l'installazione di un ruolo iniziata.  
  
#### <a name="results"></a>Risultati  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Results.png)  
  
Il **risultati** finestra di dialogo Mostra l'avanzamento e lo stato dell'installazione corrente. Installazione del ruolo continua indipendentemente dal fatto che Server Manager venga chiuso.  
  
Verifica dei risultati dell'installazione è comunque consigliabile. Se si chiude il **risultati** finestra di dialogo prima del completamento dell'installazione, è possibile controllare i risultati con il flag di notifica di Server Manager. Server Manager mostra anche un messaggio di avviso per i server che hanno installato il ruolo di dominio Active Directory ma non stati ulteriormente configurati come controller di dominio.  
  
**Notifiche attività**  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskNotofications.png)  
  
**Dettagli di Active Directory**  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSDetails.png)  
  
**Dettagli attività**  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskDetails.png)  
  
#### <a name="promote-to-domain-controller"></a>Alzare di livello a Controller di dominio  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Promote.png)  
  
Al termine dell'installazione del ruolo di dominio Active Directory, è possibile continuare con la configurazione utilizzando il **promuovere il server a un controller di dominio** collegamento. È necessario per rendere il server di un controller di dominio, ma non è necessario eseguire immediatamente la configurazione guidata. Ad esempio, potrebbe vuoi solo effettuare il provisioning di server con i file binari di dominio Active Directory prima di inviarli a un'altra succursale per una configurazione successiva. Aggiungendo il ruolo di dominio Active Directory prima della distribuzione, risparmiare tempo quando raggiunge la destinazione. È anche possibile seguire la procedura consigliata di non lasciare un controller di dominio offline per giorni o settimane. Infine, consente di aggiornare i componenti prima di promozione del controller di dominio, evitando almeno un riavvio successivo.  
  
Selezionando questo collegamento in un secondo momento vengono richiamati i cmdlet di ADDSDeployment: **install-addsforest**, **install-addsdomain**, o **install-addsdomaincontroller**.  
  
### <a name="uninstallingdisabling"></a>Disinstallazione di disabilitazione  
Per rimuovere il ruolo di dominio Active Directory come qualsiasi altro ruolo, indipendentemente dal fatto che è alzato di livello il server a controller di dominio. Tuttavia, la rimozione del ruolo di dominio Active Directory richiede un riavvio al termine.  
  
Rimozione del ruolo Active Directory Domain Services è diversa dall'installazione, che richiede l'abbassamento di livello controller dominio prima di completare. Ciò è necessario per impedire che un controller di dominio vengano disinstallati senza la pulizia dei metadati appropriati nella foresta relativi file binari del ruolo. Per ulteriori informazioni, vedere [abbassamento di livello controller di dominio e domini & #40; Livello 200 & #41; ](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
> [!WARNING]  
> Rimuovere i ruoli di servizi di dominio Active Directory con Dism.exe o con il modulo di manutenzione di Windows PowerShell dopo l'innalzamento di livello a Controller di dominio non è supportata e impedirà il server venga avviato normalmente.  
>   
> A differenza di Server Manager o il modulo distribuzione Active Directory per Windows PowerShell, manutenzione è un sistema di manutenzione nativo con senza informazioni inerenti di dominio Active Directory o la relativa configurazione. Non usare Dism.exe o il modulo di manutenzione di Windows PowerShell per disinstallare il ruolo di dominio Active Directory, a meno che il server non è più un controller di dominio.  
  
### <a name="create-an-ad-ds-forest-root-domain-with-server-manager"></a>Creare un dominio radice della foresta Active Directory con Server Manager  
Il diagramma seguente illustra il processo di configurazione di servizi di dominio Active Directory, nel caso in cui avere precedentemente installato il ruolo di dominio Active Directory e avviato il **Active Directory Domain Services configurazione guidata** tramite Server Manager.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_forestdeploy2.png)  
  
#### <a name="deployment-configuration"></a>Configurazione della distribuzione  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddNewForest.png)  
  
Server Manager inizia ogni controller di dominio con il **configurazione distribuzione** pagina. Le opzioni restanti e i campi obbligatori modificare in questa pagina e le pagine successive, a seconda operazione di distribuzione selezionato.  
  
Per creare una nuova foresta di Active Directory, fare clic su **aggiungere una nuova foresta**. È necessario fornire un nome di dominio radice valido. non può essere il nome con etichetta singola (ad esempio, il nome deve essere *contoso.com* o simile e non semplicemente *contoso*) e devono utilizzare consentiti requisiti dei nomi di dominio DNS.  
  
For more information on valid domain names, see KB article [Naming conventions in Active Directory for computers, domains, sites, and OUs](https://support.microsoft.com/kb/909264).  
  
> [!WARNING]  
> Non creare nuove foreste Active Directory con lo stesso nome di un nome DNS esterno. Ad esempio, se l'URL del DNS Internet è http://contoso.com, è necessario scegliere un nome diverso per la foresta interna evitare successivi problemi di compatibilità. Tale nome deve essere univoco e non scontato per il traffico web. Ad esempio: corp.contoso.com.  
  
Una nuova foresta non necessita di nuove credenziali per l'account Administrator del dominio. Il processo di innalzamento di livello controller di dominio utilizza le credenziali dell'account predefinito Administrator dal primo controller di dominio utilizzato per creare la radice della foresta. Non è possibile (per impostazione predefinita) per disabilitare o bloccare l'account Administrator predefinito e potrebbe essere il solo punto di ingresso in una foresta se gli altri account di dominio amministrativi sono inutilizzabili. È essenziale conoscere la password prima di distribuire una nuova foresta.  
  
**DomainName** richiede un nome DNS completo di dominio valido ed è obbligatorio.  
  
#### <a name="domain-controller-options"></a>Opzioni Controller di dominio  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DCOptions_Forest.gif)  
  
Il **opzioni Controller di dominio** consente di configurare il **livello di funzionalità della foresta** e **livello di funzionalità del dominio** per il dominio radice della nuova foresta. Per impostazione predefinita, queste impostazioni sono Windows Server 2012 in un dominio radice della nuova foresta. Il livello di funzionalità foresta Windows Server 2012 non fornisce alcuna nuova funzionalità rispetto al livello di funzionalità foresta Windows Server 2008 R2. Il livello di funzionalità del dominio Windows Server 2012 è necessario solo per implementare le nuove impostazioni Kerberos "Fornisci sempre attestazioni" e "Rifiuta le richieste di autenticazione non blindate". Degli utilizzi principali dei livelli di funzionalità in Windows Server 2012 consiste nel limitare la partecipazione nel dominio a controller di dominio che soddisfano i requisiti minimi consentiti del sistema operativo. In altre parole, è possibile specificare Windows Server 2012 dominio funzionale livello solo controller di dominio che eseguono Windows Server 2012 possono ospitare il dominio.  Windows Server 2012 implementa un nuovo flag di controller di dominio denominato **DS_WIN8_REQUIRED** nel **DSGetDcName** funzione di NetLogon che individua esclusivamente i controller di dominio Windows Server 2012. In questo modo la flessibilità di una foresta più omogenea ed eterogenea in termini di quali sistemi operativi sono autorizzati a eseguire nei controller di dominio.  
  
For more information about domain controller Location, review [Directory Service Functions](https://msdn.microsoft.com/library/ms675900(VS.85).aspx).  
  
La funzionalità di controller di dominio configurabile solo è l'opzione di server DNS. È consigliabile che tutti i controller di dominio forniscano servizi DNS per garantire un'elevata disponibilità negli ambienti distribuiti, ragione per cui questa opzione è selezionata per impostazione predefinita quando si installa un controller di dominio in qualsiasi modalità o dominio. Il catalogo globale e opzioni controller di dominio solo lettura non sono disponibili durante la creazione di un dominio radice della nuova foresta; il primo controller di dominio deve essere un catalogo globale e non può essere un controller di dominio solo lettura (RODC).  
  
L'oggetto specificato **Password modalità ripristino servizi Directory** deve soddisfare i criteri password applicati al server, che per impostazione predefinita non richiedono una password complessa; solo una non vuota. Scegliere sempre una password complessa o preferibilmente una passphrase.  
  
#### <a name="dns-options-and-dns-delegation-credentials"></a>Opzioni DNS e credenziali di delega DNS  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestDNSOptions.png)  
  
Il **opzioni DNS** pagina consente di configurare la delega DNS e fornire credenziali amministrative DNS alternative.  
  
Non è possibile configurare opzioni DNS o la delega nella configurazione guidata servizi di dominio Active Directory quando si installa un nuovo Active Directory dominio radice della foresta in cui è stato selezionato il **server DNS** nel **opzioni Controller di dominio** pagina. Il **crea delega DNS** opzione è disponibile quando si crea una zona DNS radice della nuova foresta in un'infrastruttura di server DNS esistente. Questa opzione consente di fornire credenziali amministrative DNS alternative con i diritti per aggiornare la zona DNS.  
  
For more information about whether you need to create a DNS delegation, see [Understanding Zone Delegation](https://technet.microsoft.com/library/cc771640.aspx).  
  
#### <a name="additional-options"></a>Opzioni aggiuntive  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestAdditionalOptions.png)  
  
Il **opzioni aggiuntive** pagina Mostra il nome NetBIOS del dominio e consente di sostituirlo. Per impostazione predefinita, il nome di dominio NetBIOS corrisponde all'etichetta a sinistra del nome di dominio completo fornito sul **configurazione distribuzione** pagina. Ad esempio, se è stato specificato il nome di dominio completo di corp.contoso.com, il nome di dominio NetBIOS predefinito è CORP.  
  
Se il nome è di 15 caratteri o meno e non è in conflitto con un altro nome NetBIOS, non viene modificato. Se è in conflitto con un altro nome NetBIOS, un numero viene aggiunto al nome. Se il nome è più di 15 caratteri, la procedura guidata fornisce un suggerimento univoco troncato. In entrambi i casi, la procedura guidata convalida prima il nome non è già in uso tramite una ricerca WINS e una trasmissione NetBIOS.  
  
For more information on valid domain names, see KB article [Naming conventions in Active Directory for computers, domains, sites, and OUs](https://support.microsoft.com/kb/909264).  
  
#### <a name="paths"></a>Percorsi  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPaths.png)  
  
Il **percorsi** pagina consente di sostituire i percorsi predefiniti del database di Active Directory, i registri delle transazioni di database, e la condivisione SYSVOL. I percorsi predefiniti sono sempre in sottodirectory di % systemroot % (ad esempio, C:\Windows).  
  
#### <a name="review-options-and-view-script"></a>Verifica opzioni e Visualizza Script  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestReviewOptions.png)  
  
Il **verifica opzioni** pagina consente di convalidare le impostazioni e accertarsi se soddisfano i requisiti prima di iniziare l'installazione. Questo non è l'ultima possibilità per interrompere l'installazione quando si utilizza Server Manager. Si tratta semplicemente un'opzione per confermare le impostazioni prima di proseguire con la configurazione  
  
Il **verifica opzioni** pagina in Server Manager offre anche un facoltativo **Visualizza Script** pulsante per creare un file di testo Unicode contenente la configurazione ADDSDeployment corrente come singolo script di Windows PowerShell. In questo modo è possibile utilizzare l'interfaccia grafica di Server Manager come un strumento di distribuzione di Windows PowerShell. Utilizzare la configurazione guidata servizi di dominio Active Directory per configurare le opzioni, esportare la configurazione e quindi annullare la procedura guidata. Questo processo crea un esempio valido e sintatticamente corretto utilizzato direttamente o successivamente modificato. Per esempio:  
  
```powershell 
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSForest `  
-CreateDNSDelegation `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainMode "Win2012" `  
-DomainName "corp.contoso.com" `  
-DomainNetBIOSName "CORP" `  
-ForestMode "Win2012" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-NoRebootOnCompletion:$false `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Server Manager viene compilato in genere tutti gli argomenti con i valori durante l'innalzamento di livello e non si basa sulle impostazioni predefinite (perché queste potrebbero cambiare nelle future versioni di Windows o service pack). L'unica eccezione a questa è la **- safemodeadministratorpassword** argomento (che viene deliberatamente omesso dallo script). Per forzare una richiesta di conferma, omettere il valore quando cmdlet viene eseguito in modo interattivo.  
  
#### <a name="prerequisites-check"></a>Controllo dei prerequisiti  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPrereqCheck.png)  
  
Il **controllo dei prerequisiti** è una nuova funzionalità di configurazione di dominio Active Directory. Questa nuova fase convalida che la configurazione del server sia in grado di supportare una nuova foresta di Active Directory.  
  
Quando si installa un dominio radice della nuova foresta, il Server di gestione dominio servizi di configurazione guidata Active Directory richiama una serie di test modulari. Questi test avvisano l'utente con opzioni di ripristino suggerite. Il numero di volte in base alle esigenze, è possibile eseguire i test. Il processo di controller di dominio non può continuare finché tutti i prerequisiti test non passare.  
  
Il **controllo dei prerequisiti** copre anche le informazioni pertinenti, ad esempio modifiche alla sicurezza che interessano i sistemi operativi precedenti.  
  
Per ulteriori informazioni sui controlli dei prerequisiti specifici, vedere [controllo dei prerequisiti](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
#### <a name="installation"></a>Installazione  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestInstallation.png)  
  
Quando il **installazione** pagina viene visualizzata, la configurazione del controller di dominio inizia e non può essere interrotta o annullata. I dettagli delle operazioni vengono visualizzati in questa pagina e scritti nei log:  
  
-   %systemroot%\Debug\Dcpromo.log.  
  
-   %systemroot%\debug\dcpromoui.log  
  
> [!NOTE]  
> È possibile eseguire più di installazione del ruolo e di dominio Active Directory la configurazione guidata dalla console di Server Manager stesso contemporaneamente.  
  
#### <a name="results"></a>Risultati  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
Il **risultati** pagina Mostra l'esito positivo o negativo dell'innalzamento di livello e le informazioni amministrative importanti. Il controller di dominio verrà automaticamente riavviato dopo 10 secondi.  
  
## <a name="BKMK_PSForest"></a>Distribuzione di una foresta con Windows PowerShell  
Questa sezione illustra come installare il primo controller di dominio in un dominio radice della foresta con Windows PowerShell in un computer Windows Server 2012 principale.  
  
### <a name="windows-powershell-ad-ds-role-installation-process"></a>Processo di installazione di Windows PowerShell AD DS ruolo  
Implementando alcuni semplici cmdlet di distribuzione ServerManager nei processi di distribuzione, si per realizzare che gli obiettivi di dominio Active Directory amministrazione semplificata.  
  
La figura successiva illustra il processo di installazione del ruolo Servizi di dominio Active Directory, inizia con l'esecuzione **PowerShell.exe** e termina prima la promozione del controller di dominio.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment_powershell.png)  
  
|||  
|-|-|  
|ServerManager Cmdlet|Gli argomenti (**grassetto** sono necessari argomenti. *Corsivo* argomenti possono essere specificati usando Windows PowerShell o la configurazione guidata di Active Directory.)|  
|Install-WindowsFeature o aggiungere-WindowsFeature|***-Name***<br /><br />*-Riavviare*<br /><br />*-IncludeAllSubFeature*<br /><br />*-IncludeManagementTools*<br /><br />-Origine<br /><br />*-ComputerName*<br /><br />-Credential<br /><br />-LogPath<br /><br />*-Vhd*<br /><br />*-ConfigurationFilePath*|  
  
> [!NOTE]  
> Mentre non è obbligatorio, l'argomento **- IncludeManagementTools** è consigliabile quando si installano i file binari del ruolo di dominio Active Directory  
  
Il modulo ServerManager espone le parti di installazione, stato e la rimozione ruolo del nuovo modulo di manutenzione per Windows PowerShell. Questa sovrapposizione semplifica la maggior parte delle attività e riduce la necessità di utilizzo diretto del avanzato (ma pericoloso se mal utilizzato) il modulo di manutenzione.  
  
Utilizzare **Get-Command** per esportare gli alias e i cmdlet in ServerManager.  
  
```powershell  
Get-Command -module ServerManager  
```  
  
Per esempio:  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetCommand.png)  
  
Per aggiungere il ruolo di servizi di dominio Active Directory, è sufficiente eseguire il **Install-WindowsFeature** con il nome del ruolo di dominio Active Directory come argomento. Come Server Manager, tutti i servizi necessari inerenti il ruolo di dominio Active Directory vengono installati automaticamente.  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services  
```  
  
Se si desidera inoltre gli strumenti di gestione di dominio Active Directory installati - e consigliato - quindi fornire il **- IncludeManagementTools** argomento:  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools  
```  
  
Per esempio:  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallWinFeature.png)  
  
Per elencare tutte le funzionalità e ruoli con lo stato dell'installazione, utilizzare **Get-WindowsFeature** senza argomenti. Specificare **- ComputerName** argomento per lo stato di installazione da un server remoto.  
  
```powershell  
Get-WindowsFeature  
```  
  
Poiché **Get-WindowsFeature** non dispone di un filtro meccanismo, è necessario utilizzare **Where-Object** con una pipeline per trovare funzionalità specifiche. La pipeline è un canale usato tra più cmdlet per passare i dati e il cmdlet Where-Object funge da filtro. Predefinito **$_** variabile funge da oggetto corrente che passa attraverso la pipeline con tutte le proprietà può contenere.  
  
```powershell  
Get-WindowsFeature | where-object <options>  
```  
  
Ad esempio, per trovare tutte le funzionalità contenenti "Active Dir" nella loro **nome visualizzato** proprietà, utilizzare:  
  
```powershell  
Get-WindowsFeature | where displayname -like "*active dir*"  
```  
  
Sono riportati altri esempi illustrati di seguito:  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetWindowsFeature.png)  
  
For more information about more Windows PowerShell operations with pipelines and Where-Object, see [Piping and the Pipeline in Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
Si noti che Windows PowerShell 3.0 stati considerevolmente semplificati gli argomenti della riga di comando necessari in questa operazione con la pipeline. Windows PowerShell 2.0 richiede:  
  
```powershell  
Get-WindowsFeature | where {$_.displayname - like "*active dir*"}  
```  
  
Tramite la pipeline di Windows PowerShell, è possibile creare risultati leggibili. Per esempio:  
  
```powershell  
Install-WindowsFeature | Format-List  
Install-WindowsFeature | select-object | Format-List  
  
```  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDS.png)  
  
Nota come l'utilizzo di **Select-Object** cmdlet con il **- expandproperty** argomento vengono restituiti dati interessanti:  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSWithTools.png)  
  
> [!NOTE]  
> Il **Select-Object - expandproperty** argomento rallenta leggermente le prestazioni generali di installazione.  
  
### <a name="BKMK_PS"></a>Creare un dominio radice della foresta Active Directory con Windows PowerShell  
Per installare una nuova foresta di Active Directory con il modulo ADDSDeployment, usare il cmdlet seguente:  
  
```powershell  
Install-addsforest  
```  
  
Il **Install-AddsForest** cmdlet ha solo due fasi (controllo dei prerequisiti e installazione). Nelle due figure seguenti è illustrata la fase di installazione con l'argomento minimo obbligatorio di **- domainname**.  
  
|||  
|-|-|  
|Cmdlet di ADDSDeployment|Gli argomenti (**grassetto** sono necessari argomenti. *Corsivo* argomenti possono essere specificati usando Windows PowerShell o la configurazione guidata di Active Directory.)|  
|Install-Addsforest|-Confirm<br /><br />*-CreateDNSDelegation*<br /><br />*-DatabasePath*<br /><br />*DomainMode-*<br /><br />***-DomainName***<br /><br />***-DomainNetBIOSName***<br /><br />*-DNSDelegationCredential*<br /><br />*-ForestMode*<br /><br />-Force<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-NoDnsOnNetwork<br /><br />-NoRebootOnCompletion<br /><br />*-SafeModeAdministratorPassword*<br /><br />-SkipAutoConfigureDNS<br /><br />-SkipPreChecks<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> Il **- DomainNetBIOSName** argomento è obbligatorio se si desidera modificare il nome di 15 caratteri generato automaticamente in base a prefisso del nome di dominio DNS o se il nome eccede i 15 caratteri.  
  
L'equivalente di Server Manager **configurazione distribuzione** cmdlet ADDSDeployment e gli argomenti sono:  
  
```powershell  
Install-ADDSForest  
-DomainName <string>  
```  
  
Gli argomenti equivalenti cmdlet ADDSDeployment di opzioni Controller di dominio di Server Manager sono:  
  
```powershell  
-ForestMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-InstallDNS <{$false | $true}>  
-SafeModeAdministratorPassword <secure string>  
  
```  
  
Il **Install-ADDSForest** argomenti seguono le stesse impostazioni predefinite di Server Manager, se non specificato.  
  
Il **SafeModeAdministratorPassword** operazione dell'argomento è particolare:  
  
-   Se *non specificato* come argomento, il cmdlet chiede di immettere e confermare una password nascosta. Questo è il metodo di utilizzo quando esegue il cmdlet in modo interattivo.  
  
    Ad esempio, per creare una nuova foresta denominata corp.contoso.com e ricevere la richiesta di immettere e confermare una password nascosta:  
  
    ```powershell  
    Install-ADDSForest "DomainName corp.contoso.com  
    ```  
  
-   Se specificato *con un valore*, il valore deve essere una stringa sicura. Questo non è il metodo di utilizzo quando si esegue il cmdlet in modo interattivo.  
  
Ad esempio, è possibile manualmente richiedere una password utilizzando la **Read-Host** cmdlet per richiedere all'utente una stringa sicura:  
  
```powershell  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> Poiché l'opzione precedente non chiede conferma la password, utilizzala con estrema cautela: la password non è visibile.  
  
È anche possibile specificare una stringa sicura come variabile di testo in chiaro convertita, anche se questo metodo è sconsigliato.  
  
```powershell  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
Infine, si potrebbe archiviare la password offuscata in un file e quindi riutilizzarla in seguito, senza la password di testo non crittografato mai visualizzati. Per esempio:  
  
```powershell  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> Non è consigliabile inserire o archiviare una password Cancella o di testo offuscato. Chiunque esegua questo comando in uno script o ti spii a conoscenza della password modalità ripristino servizi directory del controller di dominio. Chiunque abbia accesso al file può invertire la password offuscata. Conoscendola, possono accedere a un controller di dominio avviato in modalità ripristino servizi directory e infine fingersi il controller di dominio, innalzando i propri privilegi al massimo livello in una foresta di Active Directory. Un altro set di passaggi utilizzando **System.Security.Cryptography** per crittografare il file di testo dati sono consigliabile ma di fuori ambito. La procedura consigliata è di evitare completamente l'archiviazione delle password.  
  
Il cmdlet ADDSDeployment offre un'altra opzione per ignorare la configurazione automatica di impostazioni del client DNS, server d'inoltro e parametri radice. È possibile ignorare questa opzione di configurazione quando si utilizza Server Manager. Questo argomento è importante solo se è installato il ruolo Server DNS prima di configurare il controller di dominio:  
  
```powershell  
-SkipAutoConfigureDNS  
```  
  
Il **DomainNetBIOSName** operazione anche è particolare:  
  
-   Se il **DomainNetBIOSName** argomento non è specificato con un nome di dominio NetBIOS e il nome di dominio con prefisso con etichetta singola nel **DomainName** argomento è di 15 caratteri o meno, l'innalzamento di livello continua con un nome generato automaticamente.  
  
-   Se il **DomainNetBIOSName** argomento non è specificato con un nome di dominio NetBIOS e il nome di dominio con prefisso con etichetta singola nel **DomainName** argomento è di 16 caratteri o più, l'innalzamento di livello ha esito negativo.  
  
-   Se il **DomainNetBIOSName** argomento è specificato con un nome di dominio NetBIOS di 15 caratteri o meno, l'innalzamento di livello continua con il nome specificato.  
  
-   Se il **DomainNetBIOSName** argomento specificato con un nome di dominio NetBIOS di 16 caratteri o più, l'innalzamento di livello ha esito negativo.  
  
È l'equivalente argomento cmdlet ADDSDeployment di opzioni aggiuntive di Server Manager:  
  
```powershell  
-domainnetbiosname <string>  
```  
  
L'equivalente di Server Manager **percorsi** sono gli argomenti del cmdlet ADDSDeployment:  
  
```powershell  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
  
```  
  
Utilizzare facoltativo **Whatif** argomento con il **Install-ADDSForest** cmdlet per rivedere le informazioni di configurazione. In questo modo è possibile visualizzare i valori espliciti e impliciti degli argomenti di un cmdlet.  
  
Per esempio:  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSPaths.png)  
  
Non è possibile ignorare il **prerequisiti** quando utilizzando Server Manager, ma è possibile saltare il processo quando si usa il cmdlet di distribuzione di Active Directory con l'argomento seguente:  
  
```powershell  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft sconsiglia di ignorare il controllo dei prerequisiti, come può portare a una promozione del controller di dominio parziali o danneggiato foresta di Active Directory.  
  
Si noti, esattamente come Server Manager, **Install-ADDSForest** ricorda che l'innalzamento di livello riavvierà automaticamente il server.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSReboot.png)  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallProgress.png)  
  
Per accettare automaticamente il prompt di riavvio, usare il **-force** o **-confermare: $false** argomenti con qualsiasi cmdlet di ADDSDeployment di Windows PowerShell. Per impedire il riavvio automatico al termine dell'innalzamento di livello il server, utilizzare il **- norebootoncompletion** argomento.  
  
> [!WARNING]  
> Non è consigliabile eseguire l'override del riavvio. Il controller di dominio deve essere riavviato per funzionare correttamente.  
  
## <a name="see-also"></a>Vedere anche  
[Servizi di dominio Active Directory (portale TechNet)](https://technet.microsoft.com/library/cc770946(WS.10).aspx)  
[Servizi di dominio Active Directory per Windows Server 2008 R2](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
[Servizi di dominio Active Directory per Windows Server 2008](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
[Documentazione tecnica su Windows Server (Windows Server 2003)](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
[Centro di amministrazione di Active Directory: Introduzione (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd560651(WS.10).aspx)  
[Amministrazione di Active Directory con Windows PowerShell (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd378937(WS.10).aspx)  
[Chiedi a Directory Services Team (Blog di assistenza tecnica commerciale Microsoft ufficiale)](http://blogs.technet.com/b/askds)  
  

