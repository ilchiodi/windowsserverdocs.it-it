---
ms.assetid: 66fa945e-598d-4f18-b603-97a39ce0d836
title: Installare un controller di dominio di sola lettura di Active Directory di Windows Server 2012 (livello 200)
description: In questo argomento viene illustrato come creare un account controller di dominio di sola lettura con installazione di appoggio e quindi collegare un server all'account durante l'installazione del controller di dominio di sola lettura. In questo argomento viene illustrato anche come installare un controller di dominio di sola lettura senza eseguire un'installazione di appoggio.
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: dd3cc1c112560e77d0ab166ffb10a677b62f32e8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80825484"
---
# <a name="install-a-windows-server-2012-active-directory-read-only-domain-controller-rodc-level-200"></a>Installare un controller di dominio di sola lettura di Active Directory di Windows Server 2012 (livello 200)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene illustrato come creare un account controller di dominio di sola lettura con installazione di appoggio e quindi collegare un server all'account durante l'installazione del controller di dominio di sola lettura. In questo argomento viene illustrato anche come installare un controller di dominio di sola lettura senza eseguire un'installazione di appoggio.  
  
## <a name="stage-rodc-workflow"></a>Flusso di lavoro dell'installazione di appoggio del controller di dominio di sola lettura  
Un'installazione di appoggio di un controller di dominio di sola lettura viene eseguita in due fasi distinte:  
  
1.  Gestione temporanea di un account computer non occupato  
  
2.  Collegamento di un controller di dominio di sola lettura all'account durante l'innalzamento di livello  
  
Il diagramma seguente illustra il processo di gestione temporanea del controller di dominio di sola lettura di Servizi di dominio Active Directory, in cui è possibile creare nel dominio un account computer vuoto per il controller di dominio di sola lettura mediante Centro di amministrazione di Active Directory (Dsac.exe).  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stagedcreation.png)  
  
## <a name="stage-rodc-windows-powershell"></a><a name=BKMK_StagePS></a>Windows PowerShell per la fase di RODC  
  
|||  
|-|-|  
|**Cmdlet di ADDSDeployment**|Argomenti. Gli argomenti in **grassetto** sono obbligatori. Gli argomenti in *corsivo* possono essere specificati usando Windows PowerShell o la Configurazione guidata Servizi di dominio Active Directory.|  
|Add-addsreadonlydomaincontrolleraccount|-SkipPreChecks<p>***-DomainControllerAccountName***<p>***-DomainName***<p>***-SiteName***<p>*-AllowPasswordReplicationAccountName*<p>***-Credential***<p>*-DelegatedAdministratorAccountName*<p>*-DenyPasswordReplicationAccountName*<p>*-NoGlobalCatalog*<p>*-InstallDNS*<p>-ReplicationSourceDC|  
  
> [!NOTE]  
> L'argomento **-credential** è obbligatorio solo se l'utente non ha già eseguito l'accesso come membro del gruppo Domain Admins.  
  
## <a name="attach-rodc-workflow"></a>Flusso di lavoro del collegamento del controller di dominio di sola lettura  
Il diagramma seguente illustra il processo di configurazione di Servizi di dominio Active Directory, in cui è già stato installato il ruolo Servizi di dominio Active Directory, è stata eseguita un'installazione di appoggio dell'account controller di dominio di sola lettura ed è stato avviato **Alza di livello il server a controller di dominio** usando Server Manager per creare un nuovo controller di dominio di sola lettura in un dominio esistente, collegandolo all'account computer con installazione di appoggio.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stageddeploy_beta1.png)  
  
## <a name="attach-rodc-windows-powershell"></a><a name=BKMK_AttachPS></a>Connetti RODC Windows PowerShell  
  
|||  
|-|-|  
|**Cmdlet di ADDSDeployment**|Argomenti. Gli argomenti in **grassetto** sono obbligatori. Gli argomenti in *corsivo* possono essere specificati usando Windows PowerShell o la Configurazione guidata Servizi di dominio Active Directory.|  
|Install-AddsDomaincontroller|-SkipPreChecks<p>***-DomainName***<p>*-SafeModeAdministratorPassword*<p>*-ApplicationPartitionsToReplicate*<p>*-CreateDNSDelegation*<p>***-Credential***<p>-CriticalReplicationOnly<p>*-DatabasePath*<p>*-DNSDelegationCredential*<p>*-InstallationMediaPath*<p>*-LogPath*<p>-Norebootoncompletion<p>*-ReplicationSourceDC*<p>*-SystemKey*<p>*-SYSVOLPath*<p>***-UseExistingAccount***|  
  
> [!NOTE]  
> L'argomento **-credential** è obbligatorio solo se l'utente non ha già eseguito l'accesso come membro del gruppo Domain Admins.  
  
## <a name="staging"></a>Gestione temporanea  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_PreCreateRODC.png)  
  
Per eseguire l'operazione di gestione temporanea di un account computer di un controller di dominio di sola lettura, aprire Centro di amministrazione di Active Directory (**Dsac.exe**). Fare clic sul nome del dominio nel riquadro di spostamento. Fare doppio clic su **Controller di dominio** nell'elenco degli elementi da gestire. Fare clic su **Creazione preliminare di un account controller di dominio di sola lettura** nel riquadro delle attività.  
  
Per ulteriori informazioni su Centro di amministrazione di Active Directory, vedere [Avanzate AD DS gestione utilizzando Active Directory centro di amministrazione di & #40; Livello 200 & #41;](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md) ed esaminare [Centro di amministrazione di Active Directory: Guida introduttiva](https://technet.microsoft.com/library/dd560651(WS.10).aspx).  
  
Se si ha esperienza nella creazione di controller di dominio di sola lettura, si noterà che l'installazione guidata ha la stessa interfaccia grafica del precedente snap-in Utenti e computer di Active Directory di Windows Server 2008 e usa lo stesso codice, che include l'esportazione della configurazione nel formato di file di installazione automatica usato da dcpromo obsoleto.  
  
In Windows Server 2012 è stato introdotto il nuovo cmdlet di ADDSDeployment per eseguire un'installazione di appoggio degli account computer dei controller di dominio di sola lettura, ma la procedura guidata non usa il cmdlet per questa operazione. Nelle sezioni seguenti vengono illustrati il cmdlet e gli argomenti equivalenti per facilitare la comprensione delle informazioni associate a ciascuno di essi.  
  
Il **pre-creare un account di controller di dominio di sola lettura** collegamento nel riquadro attività del centro amministrativo di Active Directory è equivalente al cmdlet ADDSDeployment di Windows PowerShell:  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
### <a name="welcome"></a>Welcome  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_WelcomeStage1.png)  
  
La finestra di dialogo **Installazione guidata Servizi di dominio Active Directory** contiene un'opzione denominata **Utilizza installazione in modalità avanzata**. Selezionare questa opzione e fare clic su **Avanti** per visualizzare le opzioni dei criteri di replica password. Deselezionare questa opzione per usare i valori predefiniti per le opzioni dei criteri di replica password (ulteriori dettagli sono disponibili più avanti in questa sezione).  
  
### <a name="network-credentials"></a>Credenziali di rete  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Creds.png)  
  
L'opzione del nome di dominio nella finestra di dialogo **Credenziali di rete** mostra il dominio di destinazione predefinito del Centro di amministrazione di Active Directory. Per impostazione predefinita, vengono usate le credenziali correnti. Se non includono l'appartenenza al gruppo Domain Admins, fare clic su **Credenziali alternative** e fare clic su **Imposta** per immettere nella procedura guidata un nome utente membro di Domain Admins e la relativa password.  
  
L'argomento ADDSDeployment di Windows PowerShell equivalente è:  
  
```  
-credential <pscredential>  
```  
  
Tenere presente che il sistema di gestione temporanea consente il passaggio diretto da Windows Server 2008 R2 e non fornisce la nuova funzionalità Adprep. Se si prevede di distribuire account di controller di dominio di sola lettura con installazione di appoggio, è prima di tutto necessario distribuire un controller di dominio di sola lettura senza installazione di appoggio nel dominio in modo da eseguire l'operazione rodcprep automatica oppure eseguire prima manualmente adprep.exe /rodcprep.  
  
In caso contrario, verrà visualizzato un errore in cui non sarà possibile installare un controller di dominio di sola lettura in questo dominio perché adprep/rodcprep non è stato ancora eseguito.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepNotRunError.png)  
  
### <a name="specify-the-computer-name"></a>Impostazione nome computer  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1CompName.png)  
  
La finestra di dialogo **Impostazione nome computer** richiede di immettere il **Nome computer** con etichetta singola di un controller di dominio inesistente. Il controller di dominio che si configurerà e collegherà in seguito deve avere lo stesso nome o l'operazione di innalzamento di livello non individuerà l'account con installazione di appoggio.  
  
L'argomento ADDSDeployment di Windows PowerShell equivalente è:  
  
```  
-domaincontrolleraccountname <string>  
```  
  
### <a name="select-a-site"></a>Selezione sito  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Site.png)  
  
Nella finestra di dialogo **Selezione sito** sono elencati i siti di Active Directory per la foresta corrente. L'operazione relativa al controller di dominio di sola lettura con installazione di appoggio richiede di selezionare un solo sito dall'elenco. Il controller di dominio di sola lettura usa queste informazioni per creare l'oggetto Impostazioni NTDS nella partizione di configurazione e si aggiunge al sito corretto quando viene avviato per la prima volta dopo la distribuzione.  
  
L'argomento ADDSDeployment di Windows PowerShell equivalente è:  
  
```  
-sitename <string>  
```  
  
### <a name="additional-domain-controller-options"></a>Opzioni controller di dominio aggiuntivo  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DCOptions.png)  
  
La finestra di dialogo **Opzioni controller di dominio aggiuntivo** consente di specificare che un controller di dominio include l'esecuzione come **Server DNS** e **Catalogo globale**. Microsoft consiglia che i controller di dominio di sola lettura implementino i servizi DNS e catalogo globale, in modo che entrambi vengano installati per impostazione predefinita. Una finalità del ruolo controller di dominio di sola lettura sono gli scenari delle succursali in cui la Wide Area Network potrebbe non essere disponibile e, senza questi servizi DNS e catalogo globale, i computer della succursale non potranno usare le risorse e la funzionalità di Servizi di dominio Active Directory.  
  
L'opzione **Controller di dominio di sola lettura** è preselezionata e non può essere disabilitata. Gli argomenti ADDSDeployment di Windows PowerShell equivalenti sono:  
  
```  
-installdns <string>  
-NoGlobalCatalog <{$true | $false}>  
  
```  
  
> [!NOTE]  
> Per impostazione predefinita, il **- NoGlobalCatalog** è $false, ovvero il controller di dominio sarà un server di catalogo globale, se non viene specificato l'argomento.  
  
### <a name="specify-the-password-replication-policy"></a>Impostazione criteri di replica password  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRP.png)  
  
La finestra di dialogo **Impostazione criteri di replica password** consente di modificare l'elenco predefinito di account a cui è consentito memorizzare le password in questo controller di dominio di sola lettura. La password degli account dell'elenco con l'impostazione **Nega** o di quelli non presenti nell'elenco (impliciti) non viene memorizzata. Gli account a cui non è consentito memorizzare le password sul controller di dominio di sola lettura e che non possono connettersi ed essere autenticati con un controller di dominio scrivibile non possono accedere alle risorse o alla funzionalità di Active Directory.  
  
> [!IMPORTANT]  
> Questa finestra di dialogo della procedura guidata viene visualizzata solo se si seleziona la casella di controllo **Utilizza installazione in modalità avanzata** nella schermata iniziale. Se si deseleziona questa casella di controllo, la procedura guidata usa i gruppi e i valori predefiniti seguenti:  
>   
> -   Administrators - Nega  
> -   Server Operators - Nega  
> -   Backup Operators - Nega  
> -   Account Operators - Nega  
> -   Ogg. non autoriz. a replica passw. in controller sola lettura - Nega  
> -   Ogg. autorizzati a replica passw. in controller sola lettura - Consenti  
  
Gli argomenti ADDSDeployment di Windows PowerShell equivalenti sono:  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRPAllow.png)  
  
### <a name="delegation-of-rodc-installation-and-administration"></a>Delega dell'installazione e amministrazione del controller di dominio di sola lettura  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DelegateAdmin.png)  
  
La finestra di dialogo **Delega dell'installazione e amministrazione del controller di dominio di sola lettura** consente di configurare un utente o un gruppo contenente gli utenti a cui è consentito collegare il server all'account computer del controller di dominio di sola lettura. Fare clic su **Imposta** per cercare un utente o un gruppo nel dominio. L'utente o il gruppo specificato in questa finestra di dialogo ottiene le autorizzazioni amministrative locali per il controller di dominio di sola lettura. L'utente specificato o i membri del gruppo specificato possono eseguire operazioni sul controller di DOMINIO con privilegi equivalenti al gruppo Administrators del computer. *Non* sono membri del gruppo Domain Admins o del gruppo incorporato Administrators del dominio.  
  
Usare questa opzione per delegare l'amministrazione della succursale senza concedere all'amministratore della succursale l'appartenenza al gruppo Domain Admins. Non è obbligatorio delegare l'amministrazione del controller di dominio di sola lettura.  
  
L'argomento ADDSDeployment di Windows PowerShell equivalente è:  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
### <a name="summary"></a>Riepilogo  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Summary.png)  
  
La finestra di dialogo **Riepilogo** consente di verificare le impostazioni. È l'ultima opportunità di arrestare l'installazione prima che la procedura guidata crei l'account con installazione di appoggio. Fare clic su **Avanti** quando si è pronti a creare l'account computer del controller di dominio di sola lettura con installazione di appoggio.  Fare clic su **Esporta impostazioni** per salvare un file di risposte nel formato di file di installazione automatica dcpromo obsoleto.  
  
### <a name="creation"></a>Creazione  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1InstallProgress.png)  
  
L'**Installazione guidata di Servizi di dominio Active Directory** crea il controller di dominio di sola lettura con installazione di appoggio in Active Directory. Non è possibile annullare questa operazione una volta iniziata.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Complete.png)  
  
Usare il cmdlet seguente per eseguire un'installazione di appoggio di un account computer di un controller di dominio di sola lettura con il modulo ADDSDeployment di Windows PowerShell:  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
Per gli argomenti obbligatori e facoltativi, vedere [Eseguire un'installazione di appoggio di Windows PowerShell per il controller di dominio di sola lettura](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_StagePS).  
  
Poiché **Add-addsreadonlydomaincontrolleraccount** ha una sola azione con due fasi (controllo dei prerequisiti e installazione), le schermate seguenti mostrano la fase di installazione con gli argomenti obbligatori minimi.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODC.png)  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODCValidating.png)  
  
L'operazione di installazione di appoggio del controller di dominio di sola lettura crea l'account computer del controller di dominio di sola lettura in Active Directory. Nel Centro di amministrazione di Active Directory il **Tipo di controller di dominio** è **Account controller di dominio non occupato**. Questo tipo di controller di dominio indica che è possibile collegare un server come controller di dominio di sola lettura all'account del controller di dominio di sola lettura con installazione di appoggio.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Unoccupied.png)  
  
> [!IMPORTANT]  
> Il Centro di amministrazione di Active Directory non deve più collegare un server a un account computer di un controller di dominio di sola lettura. Usare Server Manager e la Configurazione guidata Servizi di dominio Active Directory o il cmdlet **Install-AddsDomainController** del modulo ADDSDeployment di Windows PowerShell per collegare un nuovo controller di dominio di sola lettura all'account con installazione di appoggio. I passaggi sono simili a quelli per aggiungere un nuovo controller di dominio scrivibile a un dominio esistente, con la differenza che l'account computer del controller di dominio di sola lettura con installazione di appoggio contiene le opzioni di configurazione impostate quando è stata eseguita l'installazione di appoggio dell'account computer del controller di dominio di sola lettura.  
  
## <a name="attaching"></a>collegamento  
  
### <a name="deployment-configuration"></a>Configurazione distribuzione  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
Server Manager inizia l'innalzamento di livello di ogni controller di dominio nella pagina **Configurazione distribuzione** . Le opzioni restanti e i campi obbligatori in questa pagina e nella pagine successive sono diversi a seconda dell'operazione di distribuzione selezionata.  
  
Per aggiungere un controller di dominio di sola lettura a un dominio esistente, selezionare **Aggiungi un controller di dominio a un dominio esistente** e fare clic sul pulsante **Seleziona** per **Specificare le informazioni di dominio per questa operazione**. Server Manager richiede automaticamente le credenziali valide. In alternativa, è possibile fare clic su **Cambia**.  
  
Per collegare un controller di dominio di sola lettura, è necessaria l'appartenenza ai gruppi Domain Admins in Windows Server 2012. La Configurazione guidata Servizi di dominio Active Directory le richiede successivamente se le credenziali correnti non dispongono delle necessarie autorizzazioni o appartenenze ai gruppi.  
  
Il cmdlet di ADDSDeployment per **Configurazione distribuzione** di Windows PowerShell e gli argomenti sono:  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Opzioni controller di dominio  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2DCOptions.png)  
  
La pagina **Opzioni controller di dominio** mostra le opzioni per il nuovo controller di dominio. Quando questa pagina viene caricata, la Configurazione guidata Servizi di dominio Active Directory invia una query LDAP a un controller di dominio esistente per cercare gli account non occupati. Se la query trova un account computer di un controller di dominio non occupato che condivide lo stesso nome del computer corrente, la procedura guidata visualizza un messaggio informativo nella parte superiore della pagina che legge **un account di controller di dominio di sola lettura creato in precedenza che corrisponde al nome del server di destinazione presente nella directory. Scegliere se utilizzare l'account RODC esistente o reinstallare il controller di dominio**. La procedura guidata usa **Usa account di controller di dominio di sola lettura esistente** come configurazione predefinita.  
  
> [!IMPORTANT]  
> È possibile usare l'opzione **Reinstalla controller di dominio** quando si è verificato un problema fisico in un controller di dominio di cui non è possibile ripristinare la funzionalità. In questo modo è possibile risparmiare tempo quando si configura il controller di dominio di sostituzione, perché l'account computer del controller di dominio e i metadati dell'oggetto vengono lasciati in Active Directory. Installare il nuovo computer con lo *stesso nome* e innalzarlo di livello come controller di dominio nel dominio. Il **reinstallare questo controller di dominio** opzione non è disponibile se sono stati rimossi i metadati dell'oggetto controller di dominio da Active Directory (pulizia dei metadati).  
  
Non è possibile configurare le opzioni del controller di dominio mentre si collega un server a un account computer di un controller di dominio di sola lettura. Le opzioni del controller di dominio vengono configurate quando si crea l'account computer del controller di dominio di sola lettura con installazione di appoggio.  
  
La **Password modalità ripristino servizi directory** specificata deve soddisfare i criteri password applicati al server. Scegliere sempre una password complessa e di difficile individuazione o preferibilmente una passphrase.  
  
Gli argomenti ADDSDeployment di Windows PowerShell di **Opzioni controller di dominio** sono:  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> Il nome del sito deve esistere già quando viene fornito come argomento di **-sitename**. Il cmdlet **install-AddsDomainController** non crea i nomi dei siti. È possibile usare il cmdlet **new-adreplicationsite** per creare nuovi siti.  
  
Gli argomenti **Install-ADDSDomainController**, se non specificati, seguono le stesse impostazioni predefinite di Server Manager.  
  
L'operazione dell'argomento **SafeModeAdministratorPassword** è particolare:  
  
-   Se *non viene specificato* come argomento, il cmdlet richiede di inserire e confermare una password nascosta. Questo è il metodo di utilizzo consigliabile quando il cmdlet viene eseguito in modo interattivo.  
  
    Ad esempio, per creare un nuovo controller di dominio di sola lettura in corp.contoso.com e ricevere la richiesta di immettere e confermare una password nascosta:  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
    ```  
  
-   Se viene indicato *con un valore*, tale valore deve essere una stringa sicura. Questo non è il metodo di utilizzo consigliabile quando il cmdlet viene eseguito in modo interattivo.  
  
Ad esempio, è possibile utilizzare il cmdlet **Read-Host** per richiedere all'utente una stringa sicura:  
  
```  
-safemodeadministratorpassword (read-host -prompt Password: -assecurestring)  
  
```  
  
> [!WARNING]  
> Poiché l'opzione precedente non chiede conferma della password, utilizzarla con estrema cautela, in quanto la password non è visibile.  
  
È inoltre possibile specificare una stringa sicura come variabile di testo in chiaro convertita, anche se questo metodo è sconsigliato.  
  
```  
-safemodeadministratorpassword (convertto-securestring Password1 -asplaintext -force)  
```  
  
Infine, è possibile archiviare la password offuscata in un file e quindi riutilizzarla in seguito, senza visualizzare mai la password non crittografata. Ad esempio,  
  
```  
$file = c:\pw.txt  
$pw = read-host -prompt Password: -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> È sconsigliabile inserire o archiviare una password non crittografata o di testo offuscato. In questo modo, la password per la modalità ripristino servizi directory per il controller di dominio diverrebbe nota a chiunque esegua questo comando in uno script o riesca a leggerla dallo schermo dell'utente.  Tutti gli utenti con accesso al file possono invertire la password offuscata. Conoscendo la password sarebbe quindi possibile accedere a un controller di dominio avviato in modalità ripristino servizi directory e infine fingersi il controller di dominio, innalzando i propri privilegi al massimo livello in una foresta Active Directory. Esiste un altro set di passaggi consigliati che prevedono l'uso di **System.Security.Cryptography** per crittografare i dati dei file di testo, ma che esulano dall'ambito di questo articolo. La procedura consigliata è di evitare completamente l'archiviazione delle password.  
  
### <a name="additional-options"></a>Opzioni aggiuntive  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2AdditionalOptions.png)  
  
Nella pagina **Opzioni aggiuntive** sono disponibili le opzioni di configurazione per denominare un controller di dominio come origine della replica o per usare qualsiasi controller di dominio come origine della replica.  
  
È inoltre possibile decidere di installare il controller di dominio usando i supporti di backup tramite l'opzione Installazione da supporto. Selezionando la casella di controllo **Installazione da supporto** , viene visualizzata un'opzione di selezione ed è necessario fare clic su **Verifica** per assicurarsi che il percorso specificato sia un supporto valido.

Linee guida per l'origine installazione da supporto:
*    Il supporto usato dall'opzione installazione da supporto viene creato con Windows Server Backup o Ntdsutil. exe da un altro controller di dominio di Windows Server esistente con la stessa versione del sistema operativo. Ad esempio, non è possibile usare un sistema operativo Windows Server 2008 R2 o precedente per creare un supporto per un controller di dominio Windows Server 2012.
*    I dati di origine installazione da supporto devono provenire da un controller di dominio scrivibile. Sebbene un'origine del RODC funzioni tecnicamente per la creazione di un nuovo RODC, sono presenti avvisi di replica falsi positivi che il RODC di origine installazione da supporto non sta replicando.

Per altre informazioni sulle modifiche nell'installazione da supporto, vedere [Modifiche all'installazione da supporto di Ntdsutil.exe](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM). Se si usa un supporto protetto da SYSKEY, Server Manager richiede la password dell'immagine durante la verifica. 
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_StagedIFM.png)  
  
Gli argomenti del cmdlet di ADDSDeployment per **Opzioni aggiuntive** sono:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>Percorsi  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Paths.png)  
  
Nella pagina **Percorsi** è possibile sostituire i percorsi predefiniti delle cartelle per il database di Servizi di dominio Active Directory, i registri delle transazioni del database e la condivisione SYSVOL. Le posizioni predefinite sono sempre in sottodirectory di %systemroot%. Gli argomenti del cmdlet di ADDSDeployment per **Percorsi** sono:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>Verifica opzioni e Visualizza script  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2ReviewOptions.png)  
  
Nella pagina **Verifica opzioni** è possibile convalidare le impostazioni e accertarsi se soddisfano i requisiti prima di iniziare l'installazione. Questa non è l'ultima possibilità per interrompere l'installazione utilizzando Server Manager. La pagina consente semplicemente di rivedere e confermare le impostazioni prima di proseguire con la configurazione. La pagina **Verifica opzioni** di Server Manager include inoltre un pulsante opzionale **Visualizza script** , che consente di creare un file di testo Unicode contenente la configurazione ADDSDeployment corrente come singolo script di Windows PowerShell. In questo modo è possibile utilizzare l'interfaccia grafica di Server Manager come strumento di distribuzione di Windows PowerShell. Utilizzare la Configurazione guidata Servizi di dominio Active Directory per configurare le opzioni, esportare la configurazione e annullare la procedura guidata. Questo processo crea un esempio valido e sintatticamente corretto che può essere utilizzato direttamente o successivamente modificato. Ad esempio,  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath C:\Windows\NTDS `  
-DomainName corp.contoso.com `  
-LogPath C:\Windows\NTDS `  
-SYSVOLPath C:\Windows\SYSVOL `  
-UseExistingAccount:$true `  
-Norebootoncompletion:$false  
-Force:$true  
  
```  
  
> [!NOTE]  
> Server Manager completa in genere tutti gli argomenti con i valori durante l'innalzamento di livello e non usa le impostazioni predefinite perché queste potrebbero cambiare nelle future versioni di Windows o nei Service Pack. La sola eccezione è l'argomento **-safemodeadministratorpassword**. Per forzare un prompt di conferma, omettere il valore quando si esegue il cmdlet in modo interattivo.  
  
Usare l'argomento facoltativo **Whatif** con il cmdlet **Install-ADDSDomainController** per rivedere le informazioni sulla configurazione. In questo modo è possibile visualizzare i valori espliciti e impliciti degli argomenti per un cmdlet.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2WhatIf.png)  
  
### <a name="prerequisites-check"></a>Controllo dei prerequisiti  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2PrereqCheck.png)  
  
**Controllo dei prerequisiti** è una nuova funzionalità nella configurazione del dominio di Servizi di dominio Active Directory. Questa nuova fase convalida che la configurazione server sia in grado di supportare una nuova foresta Servizi di dominio Active Directory.  
  
Quando si installa un nuovo dominio radice della foresta, la Configurazione guidata Servizi di dominio Active Directory di Server Manager richiama una serie di test modulari serializzati. Questi test avvisano l'utente con le opzioni di ripristino suggerite. È possibile eseguire i test ogni volta che è necessario. Il processo di installazione del controller di dominio non può continuare finché tutti i test sui prerequisiti non vengono superati.  
  
**Controllo dei prerequisiti** copre anche le informazioni pertinenti, ad esempio le modifiche alla sicurezza, che interessano i sistemi operativi precedenti. Per altre informazioni sui controlli dei prerequisiti, vedere [Controllo dei prerequisiti](../../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Non è possibile ignorare il **Controllo prerequisiti** quando si usa Server Manager, ma è possibile saltare il processo quando si usa il cmdlet di distribuzione di Servizi di dominio Active Directory con l'argomento seguente:  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft sconsiglia di ignorare il controllo dei prerequisiti perché l'innalzamento di livello del controller di dominio potrebbe essere solo parziale o la foresta Servizi di dominio Active Directory potrebbe risultare danneggiata.  
  
Fare clic su **Installa** per iniziare il processo di innalzamento di livello del controller di dominio. È l'ultima opportunità per annullare l'installazione. Non è possibile annullare il processo di innalzamento di livello una volta iniziato. Al termine dell'innalzamento di livello il computer verrà riavviato automaticamente, indipendentemente dai risultati.  
  
### <a name="installation"></a>Installazione  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Installation.png)  
  
Quando la pagina Installazione viene visualizzata, la configurazione del controller di dominio inizia e non può essere interrotta o annullata. I dettagli delle operazioni vengono visualizzati in questa pagina e scritti nei log:  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Per installare una nuova foresta Active Directory con il modulo ADDSDeployment, usare il cmdlet seguente:  
  
```  
Install-addsdomaincontroller  
  
```  
  
Per gli argomenti obbligatori e facoltativi, vedere [Collegare Windows PowerShell per il controller di dominio di sola lettura](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_AttachPS).  
  
Il cmdlet **Install-addsdomaincontroller** ha solo due fasi (controllo dei prerequisiti e installazione). Nelle due figure seguenti è illustrata la fase di installazione con gli argomenti minimi obbligatori di **-domainname**, **-useexistingaccount** e **-credential**. Si noti che, esattamente come Server Manager, **Install-ADDSDomainController** ricorda che l'innalzamento di livello riavvierà automaticamente il server.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2.png)  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2Complete.png)  
  
Per accettare automaticamente il prompt di riavvio, usare gli argomenti **-force** o **-confirm:$false** con un cmdlet ADDSDeployment di Windows PowerShell. Per evitare il riavvio automatico del server al termine dell'innalzamento di livello, usare l'argomento **-norebootoncompletion**.  
  
> [!WARNING]  
> Si sconsiglia di eseguire l'override del riavvio. Il controller di dominio deve essere riavviato per funzionare correttamente.  
  
### <a name="results"></a>Risultati  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
La pagina **Risultati** mostra l'esito positivo o negativo dell'innalzamento di livello e le informazioni amministrative importanti. Il controller di dominio verrà automaticamente riavviato dopo 10 secondi.  
  
## <a name="rodc-without-staging-workflow"></a>Flusso di lavoro del controller di dominio di sola lettura senza gestione temporanea  
Il diagramma seguente illustra il processo di configurazione di Servizi di dominio Active Directory, quando in precedenza è stato installato il ruolo Servizi di dominio Active Directory ed è stata avviata la Configurazione guidata Servizi di dominio Active Directory mediante Server Manager per creare un nuovo controller di dominio di sola lettura senza installazione di appoggio in un dominio esistente di Windows Server 2012.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_rodcdeploy.png)  
  
## <a name="rodc-without-staging-windows-powershell"></a>Controller di dominio di sola lettura senza gestione temporanea di Windows PowerShell  
  
|||  
|-|-|  
|**Cmdlet di ADDSDeployment**|Argomenti. Gli argomenti in **grassetto** sono obbligatori. Gli argomenti in *corsivo* possono essere specificati usando Windows PowerShell o la Configurazione guidata Servizi di dominio Active Directory.|  
|Install-AddsDomainController|-SkipPreChecks<p>***-DomainName***<p>*-SafeModeAdministratorPassword*<p>***-SiteName***<p>*-ApplicationPartitionsToReplicate*<p>*-CreateDNSDelegation*<p>***-Credential***<p>*-CriticalReplicationOnly*<p>*-DatabasePath*<p>*-DNSDelegationCredential*<p>-DNSOnNetwork<p>*-InstallationMediaPath*<p>*-InstallDNS*<p>*-LogPath*<p>-MoveInfrastructureOperationMasterRoleIfNecessary<p>*-NoGlobalCatalog*<p>-Norebootoncompletion<p>*-ReplicationSourceDC*<p>-SkipAutoConfigureDNS<p>*-SystemKey*<p>*-SYSVOLPath*<p>*-AllowPasswordReplicationAccountName*<p>*-DelegatedAdministratorAccountName*<p>*-DenyPasswordReplicationAccountName*<p>***-ReadOnlyReplica***|  
  
> [!NOTE]  
> L'argomento **-credential** è obbligatorio solo se l'utente non ha già eseguito l'accesso come membro del gruppo Domain Admins.  
  
## <a name="rodc-without-staging-deployment"></a>Distribuzione del controller di dominio di sola lettura senza gestione temporanea  
  
### <a name="deployment-configuration"></a>Configurazione distribuzione  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
Server Manager inizia l'innalzamento di livello di ogni controller di dominio nella pagina **Configurazione distribuzione** . Le opzioni restanti e i campi obbligatori in questa pagina e nella pagine successive sono diversi a seconda dell'operazione di distribuzione selezionata.  
  
Per aggiungere un controller di dominio di sola lettura senza installazione di appoggio a un dominio Windows Server 2012 esistente, selezionare **Aggiungi un controller di dominio a un dominio esistente** e fare clic sul pulsante **Seleziona** per **Specificare le informazioni di dominio per questa operazione**. Server Manager richiede automaticamente le credenziali valide. In alternativa, è possibile fare clic su **Cambia**.  
  
Per collegare un controller di dominio di sola lettura, è necessaria l'appartenenza ai gruppi Domain Admins in Windows Server 2012. La Configurazione guidata Servizi di dominio Active Directory le richiede successivamente se le credenziali correnti non dispongono delle necessarie autorizzazioni o appartenenze ai gruppi.  
  
Il cmdlet di ADDSDeployment per **Configurazione distribuzione** di Windows PowerShell e gli argomenti sono:  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Opzioni controller di dominio  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDCOptions.png)  
  
La pagina **Opzioni controller di dominio** specifica le funzionalità per il nuovo controller di dominio. Le funzionalità configurabili per il controller di dominio sono **Server DNS**, **Catalogo globale** e **Controller di dominio di sola lettura**. È consigliabile che tutti i controller di dominio forniscano servizi DNS e di catalogo globale per garantire un'elevata disponibilità negli ambienti distribuiti. Il catalogo globale è sempre selezionato per impostazione predefinita e il server DNS è selezionato per impostazione predefinita se il dominio corrente ospita già DNS sui controller di dominio in base alla query Origine di autorità.  
  
Nella pagina **Opzioni controller di dominio** è anche possibile scegliere dalla configurazione della foresta un **nome del sito** di Active Directory logico e appropriato. Per impostazione predefinita, viene selezionato il sito con la subnet più corretta. Se è presente un solo sito, il sito viene selezionato automaticamente.  
  
> [!IMPORTANT]  
> Se il server non fa parte di una subnet di Active Directory ed è presente più di un sito di Active Directory, non viene selezionato nulla e il pulsante **Avanti** non sarà disponibile finché non viene scelto un sito dall'elenco.  
  
La **Password modalità ripristino servizi directory** specificata deve soddisfare i criteri password applicati al server. Scegliere sempre una password complessa o di difficile individuazione o preferibilmente una passphrase. Gli argomenti ADDSDeployment di Windows PowerShell per **Opzioni controller di dominio** sono:  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> Il nome del sito deve esistere già quando viene fornito come argomento di **-sitename**. Il cmdlet **install-AddsDomainController** non crea i nomi dei siti. È possibile usare il cmdlet **new-adreplicationsite** per creare nuovi siti.  
  
Gli argomenti **Install-ADDSDomainController**, se non specificati, seguono le stesse impostazioni predefinite di Server Manager.  
  
L'operazione dell'argomento **SafeModeAdministratorPassword** è particolare:  
  
-   Se *non viene specificato* come argomento, il cmdlet richiede di inserire e confermare una password nascosta. Questo è il metodo di utilizzo consigliabile quando il cmdlet viene eseguito in modo interattivo.  
  
    Ad esempio, per creare un nuovo controller di dominio di sola lettura in corp.contoso.com e ricevere la richiesta di immettere e confermare una password nascosta:  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
    ```  
  
-   Se viene indicato *con un valore*, tale valore deve essere una stringa sicura. Questo non è il metodo di utilizzo consigliabile quando il cmdlet viene eseguito in modo interattivo.  
  
Ad esempio, è possibile utilizzare il cmdlet **Read-Host** per richiedere all'utente una stringa sicura:  
  
```  
-safemodeadministratorpassword (read-host -prompt Password: -assecurestring)  
  
```  
  
> [!WARNING]  
> Poiché l'opzione precedente non chiede conferma della password, utilizzarla con estrema cautela, in quanto la password non è visibile.  
  
È inoltre possibile specificare una stringa sicura come variabile di testo in chiaro convertita, anche se questo metodo è sconsigliato.  
  
```  
-safemodeadministratorpassword (convertto-securestring Password1 -asplaintext -force)  
```  
  
Infine, è possibile archiviare la password offuscata in un file e quindi riutilizzarla in seguito, senza visualizzare mai la password non crittografata. Ad esempio,  
  
```  
$file = c:\pw.txt  
$pw = read-host -prompt Password: -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> È sconsigliabile inserire o archiviare una password non crittografata o di testo offuscato. In questo modo, la password per la modalità ripristino servizi directory per il controller di dominio diverrebbe nota a chiunque esegua questo comando in uno script o riesca a leggerla dallo schermo dell'utente.  Tutti gli utenti con accesso al file possono invertire la password offuscata. Conoscendo la password sarebbe quindi possibile accedere a un controller di dominio avviato in modalità ripristino servizi directory e infine fingersi il controller di dominio, innalzando i propri privilegi al massimo livello in una foresta Active Directory. Esiste un altro set di passaggi consigliati che prevedono l'uso di **System.Security.Cryptography** per crittografare i dati dei file di testo, ma che esulano dall'ambito di questo articolo. La procedura consigliata è di evitare completamente l'archiviazione delle password.  
  
### <a name="rodc-options"></a>Opzioni controller di dominio di sola lettura  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCOptions.png)  
  
La pagina **Opzioni controller di dominio di sola lettura** consente di modificare le impostazioni:  
  
-   Account amministratore delegato  
  
-   Account autorizzati alla replica delle password nel controller di dominio di sola lettura  
  
-   Account non autorizzati alla replica delle password nel controller di dominio di sola lettura  
  
Gli account amministratore con delega ottengono autorizzazioni amministrative locali per il controller di dominio di sola lettura. Questi utenti possono operare con privilegi equivalenti al gruppo Administrators del computer locale.  Non sono membri del gruppo Domain Admins o del gruppo incorporato Administrators del dominio. Questa opzione risulta utile per delegare l'amministrazione in una succursale senza dover assegnare autorizzazioni amministrative per il dominio. La configurazione delle deleghe di amministrazione non è necessaria.  
  
L'argomento ADDSDeployment di Windows PowerShell equivalente è:  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
Gli account a cui non è consentito memorizzare le password sul controller di dominio di sola lettura e che non possono connettersi ed essere autenticati con un controller di dominio scrivibile non possono accedere alle risorse o alla funzionalità di Active Directory.  
  
> [!IMPORTANT]  
> Se non modificati, vengono usati i gruppi e le impostazioni predefiniti:  
>   
> -   Administrators - Nega  
> -   Server Operators - Nega  
> -   Backup Operators - Nega  
> -   Account Operators - Nega  
> -   Ogg. non autoriz. a replica passw. in controller sola lettura - Nega  
> -   Ogg. autorizzati a replica passw. in controller sola lettura - Consenti  
  
Gli argomenti ADDSDeployment di Windows PowerShell equivalenti sono:  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_SelectDelAdmin.png)  
  
### <a name="additional-options"></a>Opzioni aggiuntive  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCAdditionalOptions.png)  
  
Nella pagina **Opzioni aggiuntive** sono disponibili le opzioni di configurazione per denominare un controller di dominio come origine della replica o per usare qualsiasi controller di dominio come origine della replica.  
  
È inoltre possibile decidere di installare il controller di dominio usando i supporti di backup tramite l'opzione Installazione da supporto. Selezionando la casella di controllo **Installazione da supporto** , viene visualizzata un'opzione di selezione ed è necessario fare clic su **Verifica** per assicurarsi che il percorso specificato sia un supporto valido.

Linee guida per l'origine installazione da supporto:
*    Il supporto usato dall'opzione installazione da supporto viene creato con Windows Server Backup o Ntdsutil. exe da un altro controller di dominio di Windows Server esistente con la stessa versione del sistema operativo. Ad esempio, non è possibile usare un sistema operativo Windows Server 2008 R2 o precedente per creare un supporto per un controller di dominio Windows Server 2012.
*    I dati di origine installazione da supporto devono provenire da un controller di dominio scrivibile. Sebbene un'origine del RODC funzioni tecnicamente per la creazione di un nuovo RODC, sono presenti avvisi di replica falsi positivi che il RODC di origine installazione da supporto non sta replicando.

Per altre informazioni sulle modifiche nell'installazione da supporto, vedere [Modifiche all'installazione da supporto di Ntdsutil.exe](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM). Se si usa un supporto protetto da SYSKEY, Server Manager richiede la password dell'immagine durante la verifica.
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSIFM.png)  
  
Gli argomenti dei cmdlet di ADDSDeployment per Opzioni aggiuntive sono:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>Percorsi  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPaths.png)  
  
Nella pagina **Percorsi** è possibile sostituire i percorsi predefiniti delle cartelle per il database di Servizi di dominio Active Directory, i registri delle transazioni del database e la condivisione SYSVOL. Le posizioni predefinite sono sempre in sottodirectory di %systemroot%. Gli argomenti del cmdlet di ADDSDeployment per **Percorsi** sono:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>Opzioni di preparazione  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepOptions.png)  
  
La pagina **Opzioni di preparazione** avvisa che la configurazione di Servizi di dominio Active Directory include l'estensione dello schema (forestprep) e l'aggiornamento del dominio (domainprep). Questa pagina viene visualizzata solo quando la foresta o il dominio non è stato preparato da un'installazione precedente del controller di dominio Windows Server 2012 o dall'esecuzione manuale di Adprep.exe. La Configurazione guidata Servizi di dominio Active Directory, ad esempio, elimina questa pagina se si aggiunge un nuovo controller di dominio di replica a un dominio radice della foresta Windows Server 2012 esistente.  
  
L'estensione dello schema e l'aggiornamento del dominio non vengono eseguiti quando si fa clic su **Avanti**. Questi eventi si verificano solo durante la fase di installazione. Questa pagina elenca semplicemente gli eventi che si verificheranno in seguito durante l'installazione.  
  
Questa pagina inoltre convalida che le credenziali utente correnti siano membri dei gruppi Schema Admins ed Enterprise Admins, perché l'appartenenza a questi gruppi è necessaria per estendere lo schema o preparare un dominio. Fare clic su **Cambia** per specificare le credenziali utente adeguate se la pagina informa che le credenziali correnti non forniscono autorizzazioni sufficienti.  
  
L'argomento del cmdlet di ADDSDeployment per Opzioni aggiuntive è:  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> Come con le versioni precedenti di Windows Server, la preparazione del dominio automatizzata di Windows Server 2012 non esegue GPPREP. Eseguire **adprep.exe /gpprep** manualmente per tutti i domini non preparati in precedenza per Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2. È consigliabile eseguire GPPrep una sola volta nella cronologia di un dominio e non a ogni aggiornamento. Adprep.exe non esegue automaticamente run /gpprep perché l'operazione può causare una nuova replica di tutti i file e le cartelle nella cartella SYSVOL su tutti i controller di dominio.  
>   
> RODCPrep viene eseguito automaticamente quando si innalza di livello il primo controller di dominio di sola lettura senza installazione di appoggio in un dominio. Non si verifica quando si innalza di livello il primo controller di dominio di Windows Server 2012 scrivibile. È possibile eseguire manualmente **adprep.exe /rodcprep** anche se si prevede di distribuire controller di dominio di sola lettura.  
  
### <a name="review-options-and-view-script"></a>Verifica opzioni e Visualizza script  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCReviewOptions.png)  
  
Nella pagina **Verifica opzioni** è possibile convalidare le impostazioni e accertarsi se soddisfano i requisiti prima di iniziare l'installazione. Questa non è l'ultima possibilità per interrompere l'installazione utilizzando Server Manager. La pagina consente semplicemente di rivedere e confermare le impostazioni prima di proseguire con la configurazione.  
  
La pagina **Verifica opzioni** di Server Manager include inoltre un pulsante opzionale **Visualizza script** , che consente di creare un file di testo Unicode contenente la configurazione ADDSDeployment corrente come singolo script di Windows PowerShell. In questo modo è possibile utilizzare l'interfaccia grafica di Server Manager come strumento di distribuzione di Windows PowerShell. Utilizzare la Configurazione guidata Servizi di dominio Active Directory per configurare le opzioni, esportare la configurazione e annullare la procedura guidata. Questo processo crea un esempio valido e sintatticamente corretto che può essere utilizzato direttamente o successivamente modificato. Ad esempio,  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-AllowPasswordReplicationAccountName @(CORP\Allowed RODC Password Replication Group, CORP\Chicago RODC Admins, CORP\Chicago RODC Users and Computers) `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath C:\Windows\NTDS `  
-DelegatedAdministratorAccountName CORP\Chicago RODC Admins `  
-DenyPasswordReplicationAccountName @(BUILTIN\Administrators, BUILTIN\Server Operators, BUILTIN\Backup Operators, BUILTIN\Account Operators, CORP\Denied RODC Password Replication Group) `  
-DomainName corp.contoso.com `  
-InstallDNS:$true `  
-LogPath C:\Windows\NTDS `  
-ReadOnlyReplica:$true `  
-SiteName Default-First-Site-Name `  
-SYSVOLPath C:\Windows\SYSVOL  
-Force:$true  
  
```  
  
> [!NOTE]  
> Server Manager completa in genere tutti gli argomenti con i valori durante l'innalzamento di livello e non usa le impostazioni predefinite perché queste potrebbero cambiare nelle future versioni di Windows o nei Service Pack. La sola eccezione è l'argomento **-safemodeadministratorpassword**. Per forzare un prompt di conferma, omettere il valore quando si esegue il cmdlet in modo interattivo.  
  
Usare l'argomento facoltativo Whatif con il cmdlet Install-ADDSDomainController per rivedere le informazioni sulla configurazione. In questo modo è possibile visualizzare i valori espliciti e impliciti degli argomenti per un cmdlet.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCWhatIf.png)  
  
### <a name="prerequisites-check"></a>Controllo dei prerequisiti  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrereqCheck.png)  
  
**Controllo dei prerequisiti** è una nuova funzionalità nella configurazione del dominio di Servizi di dominio Active Directory. Questa nuova fase convalida che la configurazione server sia in grado di supportare una nuova foresta Servizi di dominio Active Directory.  
  
Quando si installa un nuovo dominio radice della foresta, la Configurazione guidata Servizi di dominio Active Directory di Server Manager richiama una serie di test modulari serializzati. Questi test avvisano l'utente con le opzioni di ripristino suggerite. È possibile eseguire i test ogni volta che è necessario. Il processo del controller di dominio non può continuare finché tutti i test sui prerequisiti non vengono superati.  
  
**Controllo dei prerequisiti** copre anche le informazioni pertinenti, ad esempio le modifiche alla sicurezza, che interessano i sistemi operativi precedenti.  
  
Non è possibile ignorare il **Controllo prerequisiti** quando si usa Server Manager, ma è possibile saltare il processo quando si usa il cmdlet di distribuzione di Servizi di dominio Active Directory con l'argomento seguente:  
  
```  
-skipprechecks  
  
```  
  
Fare clic su **Installa** per iniziare il processo di innalzamento di livello del controller di dominio. È l'ultima opportunità per annullare l'installazione. Non è possibile annullare il processo di innalzamento di livello una volta iniziato. Al termine dell'innalzamento di livello il computer verrà riavviato automaticamente, indipendentemente dai risultati.  
  
### <a name="installation"></a>Installazione  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCInstallation.png)  
  
Quando la pagina **Installazione** viene visualizzata, la configurazione del controller di dominio inizia e non può essere interrotta o annullata. I dettagli delle operazioni vengono visualizzati in questa pagina e scritti nei log:  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Per installare una nuova foresta Active Directory con il modulo ADDSDeployment, usare il cmdlet seguente:  
  
```  
Install-addsdomaincontroller  
  
```  
  
Per gli argomenti obbligatori e facoltativi, vedere la tabella **Cmdlet di ADDSDeployment** all'inizio di questa sezione.  
  
Il cmdlet **Install-addsdomaincontroller** ha solo due fasi (controllo dei prerequisiti e installazione). Nelle due figure seguenti è illustrata la fase di installazione con gli argomenti minimi obbligatori di **-domainname**, **-readonlyreplica**, **-sitename** e **-credential**. Si noti che, esattamente come Server Manager, **Install-ADDSDomainController** ricorda che l'innalzamento di livello riavvierà automaticamente il server.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODC.png)  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODCProgress.png)  
  
Per accettare automaticamente il prompt di riavvio, usare gli argomenti **-force** o **-confirm:$false** con un cmdlet ADDSDeployment di Windows PowerShell. Per evitare il riavvio automatico del server al termine dell'innalzamento di livello, usare l'argomento **-norebootoncompletion**.  
  
> [!WARNING]  
> Si sconsiglia di eseguire l'override del riavvio. Il controller di dominio deve essere riavviato per funzionare correttamente. Se ci si disconnette dal controller di dominio, non è possibile riaccedere in modo interattivo finché non lo si riavvia.  
  
### <a name="results"></a>Risultati  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCSignoff.png)  
  
La pagina **Risultati** mostra l'esito positivo o negativo dell'innalzamento di livello e le informazioni amministrative importanti. Il controller di dominio verrà automaticamente riavviato dopo 10 secondi.  
  

