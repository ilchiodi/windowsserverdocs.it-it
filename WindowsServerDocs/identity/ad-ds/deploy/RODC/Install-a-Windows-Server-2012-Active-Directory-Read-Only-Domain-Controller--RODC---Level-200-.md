---
ms.assetid: 66fa945e-598d-4f18-b603-97a39ce0d836
title: Installare un Controller di dominio di sola lettura di Server 2012 Active Directory di Windows (RODC) (livello 200)
description: In questo argomento viene illustrato come creare un account di sola lettura con installazione di appoggio e quindi associare un server all'account durante l'installazione del controller di dominio. Questo argomento illustra anche come installare un controller di dominio senza eseguire un'installazione di appoggio.
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 78281ca845f79955aaa25aa45394284c59e639cb
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-windows-server-2012-active-directory-read-only-domain-controller-rodc-level-200"></a>Installare un Controller di dominio di sola lettura di Server 2012 Active Directory di Windows (RODC) (livello 200)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene illustrato come creare un account di sola lettura con installazione di appoggio e quindi associare un server all'account durante l'installazione del controller di dominio. Questo argomento illustra anche come installare un controller di dominio senza eseguire un'installazione di appoggio.  
  
## <a name="stage-rodc-workflow"></a>Flusso di lavoro sola fase  
Funziona solo con dominio (RODC) controller installazione leggere una pre-installato in due fasi distinte:  
  
1.  Gestione temporanea di un account computer non occupato  
  
2.  Collegare un controller di dominio all'account durante l'innalzamento di livello  
  
Il diagramma seguente illustra il processo di gestione temporaneo Controller di dominio di Active Directory Domain Services in sola lettura, in cui crei l'account computer controller di dominio nel dominio utilizzando il centro di amministrazione di Active Directory (Dsac.exe).  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stagedcreation.png)  
  
## <a name="BKMK_StagePS"></a>Fase RODC Windows PowerShell  
  
|||  
|-|-|  
|**Cmdlet di ADDSDeployment**|Gli argomenti (**grassetto** sono necessari argomenti. *Corsivo* argomenti possono essere specificati usando Windows PowerShell o la configurazione guidata di Active Directory.)|  
|Aggiungere-addsreadonlydomaincontrolleraccount|-SkipPreChecks<br /><br />***-DomainControllerAccountName***<br /><br />***-DomainName***<br /><br />***-SiteName***<br /><br />*-AllowPasswordReplicationAccountName*<br /><br />***-Credential***<br /><br />*-DelegatedAdministratorAccountName*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />*-NoGlobalCatalog*<br /><br />*-InstallDNS*<br /><br />-/Replicationsourcedc|  
  
> [!NOTE]  
> Il **-credential** argomento è solo necessario se si è non già connessi come membro del gruppo Domain Admins.  
  
## <a name="attach-rodc-workflow"></a>Collegare del flusso di lavoro di sola lettura  
Il diagramma seguente illustra il processo di configurazione di servizi di dominio Active Directory, in cui è già installato il ruolo di dominio Active Directory, l'account di controller di dominio di gestione temporanea e avviata **promuovere il Server a un Controller di dominio** mediante Server Manager per creare un nuovo controller di dominio in un dominio esistente, collegarlo all'account del computer pre-installato.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stageddeploy_beta1.png)  
  
## <a name="BKMK_AttachPS"></a>Collegare i controller di dominio di Windows PowerShell  
  
|||  
|-|-|  
|**Cmdlet di ADDSDeployment**|Gli argomenti (**grassetto** sono necessari argomenti. *Corsivo* argomenti possono essere specificati usando Windows PowerShell o la configurazione guidata di Active Directory.)|  
|Install-AddsDomaincontroller|-SkipPreChecks<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-ApplicationPartitionsToReplicate*<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />-CriticalReplicationOnly<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />*-InstallationMediaPath*<br /><br />*-LogPath*<br /><br />-Norebootoncompletion<br /><br />*-/Replicationsourcedc*<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />***-UseExistingAccount***|  
  
> [!NOTE]  
> Il **-credential** argomento è solo necessario se si è non già connessi come membro del gruppo Domain Admins.  
  
## <a name="staging"></a>Gestione temporanea  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_PreCreateRODC.png)  
  
Eseguire l'operazione di gestione temporanea di un account computer controller di dominio di sola lettura, aprire il centro di amministrazione di Active Directory (**Dsac.exe**). Fare clic sul nome del dominio nel riquadro di spostamento. Fare doppio clic su **i controller di dominio** nell'elenco di gestione. Fare clic su **pre-creare un account di controller di dominio di sola lettura** nel riquadro attività.  
  
Per ulteriori informazioni su Centro di amministrazione di Active Directory, vedere [avanzate AD DS gestione utilizzando Active Directory centro di amministrazione di & #40; Livello 200 & #41; ](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md) ed esaminare [centro di amministrazione di Active Directory: Guida introduttiva](https://technet.microsoft.com/library/dd560651(WS.10).aspx).  
  
Se hai esperienza nella creazione di controller di dominio di sola lettura, si noterà che l'installazione guidata ha la stessa interfaccia grafica come appare quando si usano meno recenti di Active Directory Users e snap-in computer da Windows Server 2008 e Usa lo stesso codice, che include l'esportazione della configurazione nel formato di file di installazione automatica usato da dcpromo obsoleto.  
  
Windows Server 2012 introduce un nuovo cmdlet di ADDSDeployment per gli account computer controller di dominio di fase, ma la procedura guidata non usa il cmdlet per il suo funzionamento. Le sezioni seguenti visualizzano il cmdlet equivalenti e gli argomenti per rendere ogni più facili da capire le informazioni associate.  
  
Il **pre-creare un account di controller di dominio di sola lettura** collegamento nel riquadro attività del centro amministrativo di Active Directory è equivalente al cmdlet ADDSDeployment di Windows PowerShell:  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
### <a name="welcome"></a>Pagina iniziale  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_WelcomeStage1.png)  
  
Il **iniziale per l'installazione guidata servizi di dominio Active Directory** finestra di dialogo contiene un'opzione denominata **utilizza installazione in modalità avanzata**. Selezionare questa opzione e fare clic su **Avanti** per mostrare le opzioni dei criteri di replica delle password. Deselezionare questa opzione per utilizzare i valori predefiniti per le opzioni di Criteri replica password (descritto in dettaglio più avanti in questa sezione).  
  
### <a name="network-credentials"></a>Credenziali di rete  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Creds.png)  
  
L'opzione del nome di dominio nel **credenziali di rete** finestra di dialogo Visualizza il dominio di destinazione per il centro di amministrazione di Active Directory per impostazione predefinita. Per impostazione predefinita, vengono utilizzate le credenziali correnti. Se non includono l'appartenenza al gruppo Domain Admins, fare clic su **credenziali alternative**e fare clic su **impostare** per fornire la procedura guidata con un nome utente e una password che è un membro del gruppo Domain Admins.  
  
L'argomento di ADDSDeployment di Windows PowerShell equivalente è:  
  
```  
-credential <pscredential>  
```  
  
Tieni presente che il sistema di gestione temporanea è diretto da Windows Server 2008 R2 e non fornisce la nuova funzionalità Adprep. Se si prevede di distribuire account di sola lettura con installazione di appoggio, deve prima di distribuire un RODC senza installazione di appoggio nel dominio in modo che l'operazione rodcprep automatica o eseguire manualmente adprep.exe /rodcprep prima.  
  
In caso contrario, si riceverà errori "Non sarà in grado di installare un controller di dominio di sola lettura in questo dominio perché non è stato ancora eseguito"adprep /rodcprep"".  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepNotRunError.png)  
  
### <a name="specify-the-computer-name"></a>Specificare il nome del Computer  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1CompName.png)  
  
Il **specificare il nome del Computer** finestra di dialogo richiede di immettere l'etichetta singola **nome Computer** di un controller di dominio che non esiste. Il controller di dominio si configurerà e collegherà in seguito a questo account deve avere lo stesso nome o l'operazione di innalzamento di livello non rileverà l'account con installazione di appoggio.  
  
L'argomento di ADDSDeployment di Windows PowerShell equivalente è:  
  
```  
-domaincontrolleraccountname <string>  
```  
  
### <a name="select-a-site"></a>Selezionare un sito  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Site.png)  
  
Il **selezionare un sito** finestra di dialogo Mostra un elenco di siti di Active Directory per la foresta corrente. L'operazione di controller di dominio di sola lettura con installazione di appoggio richiede di selezionare un singolo sito dall'elenco. Il controller di dominio utilizza queste informazioni per creare l'oggetto impostazioni NTDS nella partizione di configurazione e si aggiunge al sito corretto quando viene avviato per la prima volta dopo la distribuzione.  
  
L'argomento di ADDSDeployment di Windows PowerShell equivalente è:  
  
```  
-sitename <string>  
```  
  
### <a name="additional-domain-controller-options"></a>Opzioni Controller di dominio aggiuntivo  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DCOptions.png)  
  
Il **opzioni Controller di dominio aggiuntivo** finestra di dialogo consente di specificare un controller di dominio che include l'esecuzione come un **Server DNS** e un **catalogo globale**. È consigliabile che i controller di dominio di sola lettura forniscano servizi DNS e catalogo globale, in modo che entrambi sono installati per impostazione predefinita. uno scopo del ruolo di controller di dominio è scenari di succursale in cui la rete di wide area network potrebbe non essere disponibile e senza questi servizi DNS e catalogo globale, i computer della succursale non sarà in grado di utilizzare la funzionalità e le risorse di dominio Active Directory.  
  
Il **controller di dominio di sola lettura (RODC)** opzione è preselezionata e non può essere disabilitata. Gli argomenti ADDSDeployment di Windows PowerShell equivalenti sono:  
  
```  
-installdns <string>  
-NoGlobalCatalog <{$true | $false}>  
  
```  
  
> [!NOTE]  
> Per impostazione predefinita, il **- NoGlobalCatalog** valore è $false, ovvero il controller di dominio sarà un server di catalogo globale, se non viene specificato l'argomento.  
  
### <a name="specify-the-password-replication-policy"></a>Specificare i criteri di replica Password  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRP.png)  
  
Il **specificare i criteri di replica Password** finestra di dialogo consente di modificare l'elenco predefinito di account che è consentito memorizzare le password in questo controller di dominio di sola lettura. Gli account nell'elenco configurato con **Nega** o non in elenco (impliciti) non viene memorizzata la propria password. Gli account che non è consentiti memorizzare le password in tale controller di dominio e non possono connettersi e l'autenticazione a un controller di dominio scrivibile non possono accedere alle risorse o funzionalità fornite da Active Directory.  
  
> [!IMPORTANT]  
> La procedura guidata Mostra questa finestra di dialogo solo se si seleziona il **utilizza installazione in modalità avanzata** casella di controllo nella schermata iniziale. Se si deseleziona questa casella di controllo, la procedura guidata utilizza seguenti valori e i gruppi predefiniti:  
>   
> -   Administrators - Nega  
> -   Server Operators - Nega  
> -   Backup Operators - Nega  
> -   Account Operators - Nega  
> -   Negato a replica passw - Nega  
> -   Autorizzati a replica passw - Consenti  
  
Gli argomenti ADDSDeployment di Windows PowerShell equivalenti sono:  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRPAllow.png)  
  
### <a name="delegation-of-rodc-installation-and-administration"></a>Delega dell'installazione del controller e amministrazione  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DelegateAdmin.png)  
  
Il **delega dell'installazione del controller e l'amministrazione** finestra di dialogo consente di configurare un utente o un gruppo contenente gli utenti che sono autorizzati a collegare il server per l'account computer. Fare clic su **impostare** per esplorare il dominio per un utente o gruppo. L'utente o gruppo specificato in questa finestra di dialogo Ottiene le autorizzazioni amministrative locali nel RODC. L'utente specificato o i membri del gruppo specificato possono eseguire operazioni sul controller di dominio con privilegi equivalenti al gruppo Administrators del computer. Sono *non* i membri del gruppo Domain Admins o gruppo incorporato Administrators del dominio.  
  
Utilizzare questa opzione per delegare l'amministrazione della succursale senza concedere l'appartenenza di amministratore della succursale al gruppo Domain Admins. La delega dell'amministrazione di sola lettura non è obbligatorio.  
  
L'argomento di ADDSDeployment di Windows PowerShell equivalente è:  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
### <a name="summary"></a>Riepilogo  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Summary.png)  
  
Il **riepilogo** finestra di dialogo consente di verificare le impostazioni. Questo è l'ultima possibilità per interrompere l'installazione prima che la procedura guidata crea l'account con installazione di appoggio. Fare clic su **Avanti** quando sei pronto per creare l'account computer pre-installato.  Fare clic su **esportare le impostazioni** per salvare un file di risposte nel formato di file di installazione automatica dcpromo obsoleto.  
  
### <a name="creation"></a>Creazione  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1InstallProgress.png)  
  
Il **guidata installazione di servizi di dominio di Active Directory** crea il controller di dominio di sola lettura con installazione di appoggio in Active Directory. È possibile annullare questa operazione dopo l'avvio.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Complete.png)  
  
Per rappresentare un account computer controller di dominio di sola lettura con il modulo ADDSDeployment di Windows PowerShell, usare il cmdlet seguente:  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
Vedere [fase RODC Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_StagePS) per gli argomenti obbligatori e facoltativi.  
  
Poiché **Add-addsreadonlydomaincontrolleraccount** ha una sola azione con due fasi (prerequisiti controllo e installazione), le schermate seguenti mostrano la fase di installazione con gli argomenti obbligatori minimi.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODC.png)  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODCValidating.png)  
  
La fase di operazione di sola lettura crea l'account computer del controller di dominio in Active Directory. Mostra il centro di amministrazione di Active Directory il **tipo di Controller di dominio** come un **Account di Controller di dominio non occupato**. Questo tipo di controller di dominio indica che l'account di sola lettura con installazione di appoggio è pronto per un server a cui connettersi come controller di dominio di sola lettura.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Unoccupied.png)  
  
> [!IMPORTANT]  
> Il centro di amministrazione di Active Directory non è più necessario per associare un server a un account computer controller di dominio di sola lettura. Utilizzare Server Manager e la configurazione guidata servizi di dominio Active Directory o il cmdlet del modulo ADDSDeployment di Windows PowerShell **Install-AddsDomainController** per collegare un nuovo controller di dominio per l'account con installazione di appoggio. La procedura è simile all'aggiunta di un nuovo controller di dominio scrivibile a un dominio esistente, ad eccezione del fatto che l'account computer di sola lettura con installazione di appoggio contiene le opzioni di configurazione scelto al momento di che gestione temporanea impiegata per l'account computer del controller di dominio.  
  
## <a name="attaching"></a>Collegamento  
  
### <a name="deployment-configuration"></a>Configurazione della distribuzione  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
Server Manager inizia ogni controller di dominio con il **configurazione distribuzione** pagina. Le opzioni restanti e i campi obbligatori modificare in questa pagina e le pagine successive, a seconda operazione di distribuzione selezionato.  
  
Per aggiungere un controller di dominio di sola lettura a un dominio esistente, selezionare **aggiungere un controller di dominio a un dominio esistente** e fare clic su di **selezionare** pulsante per **specificare le informazioni di dominio per questo dominio**. Server Manager richiede automaticamente le credenziali valide oppure è possibile fare clic su **modifica**.  
  
Associare un RODC richiede l'appartenenza ai gruppi Domain Admins in Windows Server 2012. La configurazione guidata servizi di dominio Active Directory richiede successivamente se le credenziali correnti non dispongono di autorizzazioni adeguate o appartenenza al gruppo.  
  
Il **configurazione distribuzione** cmdlet ADDSDeployment di Windows PowerShell e gli argomenti sono:  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Opzioni Controller di dominio  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2DCOptions.png)  
  
Il **opzioni Controller di dominio** pagina Mostra le opzioni per il nuovo controller di dominio. Quando questa pagina viene caricata, la configurazione guidata servizi di dominio Active Directory invia una query LDAP a un controller di dominio esistente per cercare gli account non occupati. Se la query trova un controller di dominio non occupato account computer che condivide lo stesso nome del computer corrente, quindi la procedura guidata visualizza un messaggio informativo nella parte superiore della pagina che legge "**nella directory è presente un account di controller di dominio creato in precedenza che corrisponde al nome del server di destinazione. Scegliere se utilizzare tale account o reinstallare questo controller di dominio**. " La procedura guidata utilizza il **Usa account di controller di dominio esistente** come la configurazione predefinita.  
  
> [!IMPORTANT]  
> È possibile utilizzare il **reinstallare questo controller di dominio** opzione quando un controller di dominio ha subito un problema fisico e non è possibile ripristinare la funzionalità. Questo consente di risparmiare tempo quando si configura il controller di dominio di sostituzione, lasciando il controller di dominio account del computer e i metadati in Active Directory dell'oggetto. Installare il nuovo computer con il *stesso nome*e innalzarlo di livello come controller di dominio nel dominio. Il **reinstallare questo controller di dominio** opzione non è disponibile se sono stati rimossi i metadati dell'oggetto controller di dominio da Active Directory (pulizia dei metadati).  
  
Quando si collega un server a un account computer controller di dominio, è possibile configurare opzioni controller di dominio. Configurare opzioni controller di dominio quando si crea l'account computer pre-installato.  
  
L'oggetto specificato **Password modalità ripristino servizi Directory** deve soddisfare i criteri password applicati al server. Scegliere sempre una password complessa o preferibilmente una passphrase.  
  
Il **opzioni Controller di dominio** gli argomenti ADDSDeployment di Windows PowerShell sono:  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> Il nome del sito deve esistere già quando viene fornito come argomento al **- sitename**. Il **install-AddsDomainController** cmdlet non crea i nomi dei siti. È possibile utilizzare cmdlet **nuovo-adreplicationsite** per creare nuovi siti.  
  
Il **Install-ADDSDomainController** argomenti seguono le stesse impostazioni predefinite di Server Manager, se non specificato.  
  
Il **SafeModeAdministratorPassword** operazione dell'argomento è particolare:  
  
-   Se *non specificato* come argomento, il cmdlet chiede di immettere e confermare una password nascosta. Questo è il metodo di utilizzo quando esegue il cmdlet in modo interattivo.  
  
    Ad esempio, per creare un nuovo controller di dominio in corp.contoso.com e ricevere la richiesta di immettere e confermare una password nascosta:  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
    ```  
  
-   Se specificato *con un valore*, il valore deve essere una stringa sicura. Questo non è il metodo di utilizzo quando si esegue il cmdlet in modo interattivo.  
  
Ad esempio, è possibile manualmente richiedere una password utilizzando la **Read-Host** cmdlet per richiedere all'utente una stringa sicura:  
  
```  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
  
```  
  
> [!WARNING]  
> Poiché l'opzione precedente non chiede conferma la password, utilizzala con estrema cautela: la password non è visibile.  
  
È anche possibile specificare una stringa sicura come variabile di testo in chiaro convertita, anche se questo metodo è sconsigliato.  
  
```  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
Infine, si potrebbe archiviare la password offuscata in un file e quindi riutilizzarla in seguito, senza la password di testo non crittografato mai visualizzati. Per esempio:  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> Non è consigliabile inserire o archiviare una password Cancella o di testo offuscato. Chiunque esegua questo comando in uno script o ti spii a conoscenza della password modalità ripristino servizi directory del controller di dominio.  Chiunque abbia accesso al file può invertire la password offuscata. Conoscendola, possono accedere a un controller di dominio avviato in modalità ripristino servizi directory e infine fingersi il controller di dominio, innalzando i propri privilegi al massimo livello in una foresta Active Directory. Un altro set di passaggi utilizzando **System.Security.Cryptography** per crittografare il file di testo dati sono consigliabile ma di fuori ambito. La procedura consigliata è di evitare completamente l'archiviazione delle password.  
  
### <a name="additional-options"></a>Opzioni aggiuntive  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2AdditionalOptions.png)  
  
Il **opzioni aggiuntive** pagina fornisce opzioni di configurazione per denominare un controller di dominio come origine della replica, oppure è possibile utilizzare qualsiasi controller di dominio come origine della replica.  
  
È inoltre possibile installare il controller di dominio usando supporti utilizzando l'installazione dall'opzione di supporto di backup. Il **installa da supporto** casella di controllo offre un'opzione di selezione e deve fare clic su **verifica** per verificare il percorso specificato sia un supporto valido. I supporti compatibili con l'opzione viene creato con Windows Server Backup o Ntdsutil.exe da un altro esistente computer Windows Server 2012. è possibile utilizzare un Windows Server 2008 R2 o un sistema operativo precedente per creare il supporto per un controller di dominio Windows Server 2012. Per ulteriori informazioni sulle modifiche nell'installazione da supporto, vedere [Ntdsutil.exe installazione da supporto modifiche](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM). Se Usa un supporto protetto da SYSKEY, Server Manager richiede la password dell'immagine durante la verifica.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_StagedIFM.png)  
  
Il **opzioni aggiuntive** sono gli argomenti del cmdlet ADDSDeployment:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>Percorsi  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Paths.png)  
  
Il **percorsi** pagina consente di sostituire i percorsi predefiniti del database di Active Directory, i registri delle transazioni di database, e la condivisione SYSVOL. I percorsi predefiniti sono sempre in sottodirectory di % systemroot %. Il **percorsi** sono gli argomenti del cmdlet ADDSDeployment:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>Verifica opzioni e Visualizza Script  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2ReviewOptions.png)  
  
Il **verifica opzioni** pagina consente di convalidare le impostazioni e accertarsi se soddisfano i requisiti prima di iniziare l'installazione. Questo non è l'ultima possibilità per interrompere l'installazione utilizzando Server Manager. Questa pagina consente semplicemente di rivedere e confermare le impostazioni prima di proseguire con la configurazione. Il **verifica opzioni** pagina in Server Manager offre anche un facoltativo **Visualizza Script** pulsante per creare un file di testo Unicode contenente la configurazione ADDSDeployment corrente come singolo script di Windows PowerShell. In questo modo è possibile utilizzare l'interfaccia grafica di Server Manager come un strumento di distribuzione di Windows PowerShell. Utilizzare la configurazione guidata servizi di dominio Active Directory per configurare le opzioni, esportare la configurazione e quindi annullare la procedura guidata. Questo processo crea un esempio valido e sintatticamente corretto utilizzato direttamente o successivamente modificato. Per esempio:  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainName "corp.contoso.com" `  
-LogPath "C:\Windows\NTDS" `  
-SYSVOLPath "C:\Windows\SYSVOL" `  
-UseExistingAccount:$true `  
-Norebootoncompletion:$false  
-Force:$true  
  
```  
  
> [!NOTE]  
> Server Manager viene compilato in genere tutti gli argomenti con i valori durante l'innalzamento di livello e non si basa sulle impostazioni predefinite (perché queste potrebbero cambiare nelle future versioni di Windows o service pack). L'unica eccezione a questa è la **- safemodeadministratorpassword** argomento. Per forzare una richiesta di conferma omettere il valore quando cmdlet viene eseguito in modo interattivo.  
  
Utilizzare facoltativo **Whatif** argomento con il **Install-ADDSDomainController** cmdlet per rivedere le informazioni di configurazione. In questo modo è possibile visualizzare i valori espliciti e impliciti degli argomenti per un cmdlet.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2WhatIf.png)  
  
### <a name="prerequisites-check"></a>Controllo dei prerequisiti  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2PrereqCheck.png)  
  
Il **controllo dei prerequisiti** è una nuova funzionalità di configurazione di dominio Active Directory. Questa nuova fase convalida che la configurazione del server sia in grado di supportare una nuova foresta di Active Directory.  
  
Quando si installa un dominio radice della nuova foresta, il Server di gestione dominio servizi di configurazione guidata Active Directory richiama una serie di test modulari serializzati. Questi test avvisano l'utente con opzioni di ripristino suggerite. Il numero di volte in base alle esigenze, è possibile eseguire i test. Il processo di installazione di controller di dominio non può continuare finché tutti i prerequisiti test non passare.  
  
Il **controllo dei prerequisiti** copre anche le informazioni pertinenti, ad esempio modifiche alla sicurezza che interessano i sistemi operativi precedenti. Per ulteriori informazioni sui controlli dei prerequisiti, vedere [controllo dei prerequisiti](../../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Non è possibile ignorare il **prerequisiti** quando utilizzando Server Manager, ma è possibile saltare il processo quando si usa il cmdlet di distribuzione di Active Directory con l'argomento seguente:  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft sconsiglia di ignorare il controllo dei prerequisiti, come può portare a una promozione del controller di dominio parziali o danneggiato foresta di Active Directory.  
  
Fare clic su **installare** per avviare il processo di innalzamento di livello controller di dominio. Questo è l'ultima opportunità per annullare l'installazione. È possibile annullare il processo di innalzamento di livello una volta iniziato. Il computer verrà riavviato automaticamente al termine dell'innalzamento di livello, indipendentemente dai risultati di innalzamento di livello.  
  
### <a name="installation"></a>Installazione  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Installation.png)  
  
Quando viene visualizzata la pagina di installazione, la configurazione del controller di dominio inizia e non può essere interrotta o annullata. I dettagli delle operazioni vengono visualizzati in questa pagina e scritti nei log:  
  
-   %systemroot%\Debug\Dcpromo.log.  
  
-   %systemroot%\debug\dcpromoui.log  
  
Per installare una nuova foresta di Active Directory con il modulo ADDSDeployment, usare il cmdlet seguente:  
  
```  
Install-addsdomaincontroller  
  
```  
  
Vedere [collegare RODC Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_AttachPS) per gli argomenti obbligatori e facoltativi.  
  
Il **Install-addsdomaincontroller** cmdlet ha solo due fasi (controllo dei prerequisiti e installazione). Nelle due figure seguenti è illustrata la fase di installazione con gli argomenti minimi obbligatori di **- domainname**, **- useexistingaccount**, e **-credential**. Si noti, esattamente come Server Manager, **Install-ADDSDomainController** ricorda che l'innalzamento di livello riavvierà automaticamente il server:  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2.png)  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2Complete.png)  
  
Per accettare automaticamente il prompt di riavvio, usare il **-force** o **-confermare: $false** argomenti con qualsiasi cmdlet di ADDSDeployment di Windows PowerShell. Per impedire il riavvio automatico al termine dell'innalzamento di livello il server, utilizzare il **- norebootoncompletion** argomento.  
  
> [!WARNING]  
> Non è consigliabile eseguire l'override del riavvio. Il controller di dominio deve essere riavviato per funzionare correttamente.  
  
### <a name="results"></a>Risultati  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
Il **risultati** pagina Mostra l'esito positivo o negativo dell'innalzamento di livello e le informazioni amministrative importanti. Il controller di dominio verrà automaticamente riavviato dopo 10 secondi.  
  
## <a name="rodc-without-staging-workflow"></a>Sola lettura senza gestione temporanea del flusso di lavoro  
Il diagramma seguente illustra il processo di configurazione di servizi di dominio Active Directory, quando in precedenza è stato installato il ruolo di dominio Active Directory ed è stata avviata il dominio servizi di configurazione guidata Active Directory mediante Server Manager per creare un nuovo controller di dominio di sola lettura senza installazione di appoggio in un dominio Windows Server 2012 esistente.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_rodcdeploy.png)  
  
## <a name="rodc-without-staging-windows-powershell"></a>Sola lettura senza gestione temporanea di Windows PowerShell  
  
|||  
|-|-|  
|**Cmdlet di ADDSDeployment**|Gli argomenti (**grassetto** sono necessari argomenti. *Corsivo* argomenti possono essere specificati usando Windows PowerShell o la configurazione guidata di Active Directory.)|  
|Install-AddsDomainController|-SkipPreChecks<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />***-SiteName***<br /><br />*-ApplicationPartitionsToReplicate*<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />*-CriticalReplicationOnly*<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-DNSOnNetwork<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*-/Replicationsourcedc*<br /><br />-SkipAutoConfigureDNS<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />*-AllowPasswordReplicationAccountName*<br /><br />*-DelegatedAdministratorAccountName*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />***-ReadOnlyReplica***|  
  
> [!NOTE]  
> Il **-credential** argomento è solo necessario se si è non già connessi come membro del gruppo Domain Admins.  
  
## <a name="rodc-without-staging-deployment"></a>Sola lettura senza gestione temporanea distribuzione  
  
### <a name="deployment-configuration"></a>Configurazione della distribuzione  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
Server Manager inizia ogni controller di dominio con il **configurazione distribuzione** pagina. Le opzioni restanti e i campi obbligatori modificare in questa pagina e le pagine successive, a seconda operazione di distribuzione selezionato.  
  
Per aggiungere un controller di dominio di sola lettura senza installazione di appoggio a un dominio Windows Server 2012 esistente, selezionare **aggiungere un controller di dominio a un dominio esistente** e fare clic su di **selezionare** pulsante per **specificare le informazioni di dominio per questo dominio**. Server Manager richiede automaticamente le credenziali valide oppure è possibile fare clic su **modifica**.  
  
Associare un RODC richiede l'appartenenza ai gruppi Domain Admins in Windows Server 2012. La configurazione guidata servizi di dominio Active Directory richiede successivamente se le credenziali correnti non dispongono di autorizzazioni adeguate o appartenenza al gruppo.  
  
Il **configurazione distribuzione** cmdlet ADDSDeployment di Windows PowerShell e gli argomenti sono:  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Opzioni Controller di dominio  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDCOptions.png)  
  
Il **opzioni Controller di dominio** pagina specifica le funzionalità del controller di dominio per il nuovo controller di dominio. La funzionalità del controller di dominio configurabili sono **server DNS**, **catalogo globale**, e **controller di dominio di sola lettura**. È consigliabile che tutti i controller di dominio forniscano servizi DNS e catalogo globale per la disponibilità elevata negli ambienti distribuiti. Catalogo globale è sempre selezionato per impostazione predefinita e server DNS è selezionato per impostazione predefinita, se il dominio corrente ospita già DNS sui controller di dominio basato su query origine di autorità.  
  
Il **opzioni Controller di dominio** pagina è inoltre possibile scegliere di Active Directory appropriato logica **nome sito** dalla configurazione della foresta. Per impostazione predefinita, viene selezionato il sito con la subnet più corretta. Se è presente un solo sito, il sito viene selezionato automaticamente.  
  
> [!IMPORTANT]  
> Se il server non fa parte di una subnet di Active Directory ed è presente più di un sito di Active Directory, viene selezionato nulla e il **Avanti** pulsante non è disponibile fino a quando non viene scelto un sito dall'elenco.  
  
L'oggetto specificato **Password modalità ripristino servizi Directory** deve soddisfare i criteri password applicati al server. Scegliere sempre una password complessa o preferibilmente una passphrase. Il **opzioni Controller di dominio** gli argomenti ADDSDeployment di Windows PowerShell sono:  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> Il nome del sito deve esistere già quando viene fornito come argomento al **- sitename**. Il **install-AddsDomainController** cmdlet non crea i nomi dei siti. È possibile utilizzare cmdlet **nuovo-adreplicationsite** per creare nuovi siti.  
  
Il **Install-ADDSDomainController** argomenti seguono le stesse impostazioni predefinite di Server Manager, se non specificato.  
  
Il **SafeModeAdministratorPassword** operazione dell'argomento è particolare:  
  
-   Se *non specificato* come argomento, il cmdlet chiede di immettere e confermare una password nascosta. Questo è il metodo di utilizzo quando esegue il cmdlet in modo interattivo.  
  
    Ad esempio, per creare un nuovo controller di dominio in corp.contoso.com e ricevere la richiesta di immettere e confermare una password nascosta:  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
    ```  
  
-   Se specificato *con un valore*, il valore deve essere una stringa sicura. Questo non è il metodo di utilizzo quando si esegue il cmdlet in modo interattivo.  
  
Ad esempio, è possibile manualmente richiedere una password utilizzando la **Read-Host** cmdlet per richiedere all'utente una stringa sicura:  
  
```  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
  
```  
  
> [!WARNING]  
> Poiché l'opzione precedente non chiede conferma la password, utilizzala con estrema cautela: la password non è visibile.  
  
È anche possibile specificare una stringa sicura come variabile di testo in chiaro convertita, anche se questo metodo è sconsigliato.  
  
```  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
Infine, si potrebbe archiviare la password offuscata in un file e quindi riutilizzarla in seguito, senza la password di testo non crittografato mai visualizzati. Per esempio:  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> Non è consigliabile inserire o archiviare una password Cancella o di testo offuscato. Chiunque esegua questo comando in uno script o ti spii a conoscenza della password modalità ripristino servizi directory del controller di dominio.  Chiunque abbia accesso al file può invertire la password offuscata. Conoscendola, possono accedere a un controller di dominio avviato in modalità ripristino servizi directory e infine fingersi il controller di dominio, innalzando i propri privilegi al massimo livello in una foresta Active Directory. Un altro set di passaggi utilizzando **System.Security.Cryptography** per crittografare il file di testo dati sono consigliabile ma di fuori ambito. La procedura consigliata è di evitare completamente l'archiviazione delle password.  
  
### <a name="rodc-options"></a>Opzioni controller di dominio  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCOptions.png)  
  
Il **Opzioni controller di dominio** pagina consente di modificare le impostazioni:  
  
-   Account amministratore delegato  
  
-   Account autorizzati alla replica delle password nel RODC  
  
-   Account non autorizzati alla replica delle password nel RODC  
  
Gli account di amministratore delegato ottengono autorizzazioni amministrative locali per tale controller di dominio. Questi utenti possono operare con privilegi equivalenti al gruppo Administrators del computer locale.  Non sono membri del gruppo Domain Admins o il gruppo incorporato Administrators del dominio. Questa opzione è utile per delegare l'amministrazione della succursale senza concedere le autorizzazioni amministrative di dominio. Configurare la delega dell'amministrazione non è obbligatorio.  
  
L'argomento di ADDSDeployment di Windows PowerShell equivalente è:  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
Gli account che non è consentiti memorizzare le password in tale controller di dominio e non possono connettersi e l'autenticazione a un controller di dominio scrivibile non possono accedere alle risorse o funzionalità fornite da Active Directory.  
  
> [!IMPORTANT]  
> Se non è stato modificato, vengono utilizzate i gruppi predefiniti e le impostazioni:  
>   
> -   Administrators - Nega  
> -   Server Operators - Nega  
> -   Backup Operators - Nega  
> -   Account Operators - Nega  
> -   Negato a replica passw - Nega  
> -   Autorizzati a replica passw - Consenti  
  
Gli argomenti ADDSDeployment di Windows PowerShell equivalenti sono:  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_SelectDelAdmin.png)  
  
### <a name="additional-options"></a>Opzioni aggiuntive  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCAdditionalOptions.png)  
  
Il **opzioni aggiuntive** pagina fornisce opzioni di configurazione per denominare un controller di dominio come origine della replica, oppure è possibile utilizzare qualsiasi controller di dominio come origine della replica.  
  
È inoltre possibile installare il controller di dominio usando supporti utilizzando l'installazione dall'opzione di supporto di backup. Il **installa da supporto** casella di controllo offre un'opzione di selezione e deve fare clic su **verifica** per verificare il percorso specificato sia un supporto valido. I supporti compatibili con l'opzione viene creato con Windows Server Backup o Ntdsutil.exe da un altro esistente computer Windows Server 2012. è possibile utilizzare un Windows Server 2008 R2 o un sistema operativo precedente per creare il supporto per un controller di dominio Windows Server 2012.  Appendici sono disponibili ulteriori informazioni sulle modifiche nell'installazione da supporto. Se Usa un supporto protetto da SYSKEY, Server Manager richiede la password dell'immagine durante la verifica.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSIFM.png)  
  
Gli argomenti del cmdlet ADDSDeployment di opzioni aggiuntive sono:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>Percorsi  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPaths.png)  
  
Il **percorsi** pagina consente di sostituire i percorsi predefiniti del database di Active Directory, i registri delle transazioni di database, e la condivisione SYSVOL. I percorsi predefiniti sono sempre in sottodirectory di % systemroot %. Il **percorsi** sono gli argomenti del cmdlet ADDSDeployment:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>Opzioni di preparazione  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepOptions.png)  
  
Il **opzioni di preparazione** pagina segnala che la configurazione di Active Directory include l'estensione dello Schema (forestprep) e l'aggiornamento del dominio (domainprep). Questa pagina viene visualizzata solo quando la foresta o dominio non è stato preparato da un'installazione precedente del controller dominio Windows Server 2012 o dall'esecuzione manuale di Adprep.exe. La configurazione guidata servizi di dominio Active Directory, ad esempio, Elimina questa pagina se si aggiunge un nuovo controller di dominio di replica a un dominio radice della foresta Windows Server 2012 esistente.  
  
Estensione dello Schema e l'aggiornamento del dominio non si verificano quando si fa clic **Avanti**. Questi eventi si verificano solo durante la fase di installazione. Questa pagina elenca semplicemente gli eventi che si verificheranno in seguito durante l'installazione.  
  
Questa pagina inoltre convalida che le credenziali dell'utente corrente sono membri dei gruppi Schema Admins ed Enterprise Admins, in base alle esigenze di questi gruppi per estendere lo schema o preparare un dominio. Fare clic su **modifica** per fornire le credenziali utente adeguate se la pagina informa che le credenziali correnti non forniscono autorizzazioni sufficienti.  
  
È l'argomento del cmdlet ADDSDeployment di opzioni aggiuntive:  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> Come con le versioni precedenti di Windows Server, la preparazione del dominio automatizzata di Windows Server 2012 non esegue GPPREP. Eseguire **adprep.exe /gpprep** manualmente per tutti i domini non preparati in precedenza per Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2. È consigliabile eseguire GPPrep una sola volta nella cronologia di un dominio, non a ogni aggiornamento. Adprep.exe non viene eseguito automaticamente /gpprep perché l'operazione può causare tutti i file e cartelle nella cartella SYSVOL per nuovamente replicato in tutti i controller di dominio.  
>   
> RODCPrep viene eseguito automaticamente quando si innalza di livello il primo controller di dominio senza installazione di appoggio in un dominio. Non si verifica quando si innalza di livello il primo controller di dominio Windows Server 2012 scrivibile. È possibile eseguire anche manualmente **adprep.exe /rodcprep** se si prevede di distribuire controller di dominio di sola lettura.  
  
### <a name="review-options-and-view-script"></a>Verifica opzioni e Visualizza Script  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCReviewOptions.png)  
  
Il **verifica opzioni** pagina consente di convalidare le impostazioni e accertarsi se soddisfano i requisiti prima di iniziare l'installazione. Questo non è l'ultima possibilità per interrompere l'installazione utilizzando Server Manager. Questa pagina consente semplicemente di rivedere e confermare le impostazioni prima di proseguire con la configurazione.  
  
Il **verifica opzioni** pagina in Server Manager offre anche un facoltativo **Visualizza Script** pulsante per creare un file di testo Unicode contenente la configurazione ADDSDeployment corrente come singolo script di Windows PowerShell. In questo modo è possibile utilizzare l'interfaccia grafica di Server Manager come un strumento di distribuzione di Windows PowerShell. Utilizzare la configurazione guidata servizi di dominio Active Directory per configurare le opzioni, esportare la configurazione e quindi annullare la procedura guidata. Questo processo crea un esempio valido e sintatticamente corretto utilizzato direttamente o successivamente modificato. Per esempio:  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-AllowPasswordReplicationAccountName @("CORP\Allowed RODC Password Replication Group", "CORP\Chicago RODC Admins", "CORP\Chicago RODC Users and Computers") `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DelegatedAdministratorAccountName "CORP\Chicago RODC Admins" `  
-DenyPasswordReplicationAccountName @("BUILTIN\Administrators", "BUILTIN\Server Operators", "BUILTIN\Backup Operators", "BUILTIN\Account Operators", "CORP\Denied RODC Password Replication Group") `  
-DomainName "corp.contoso.com" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-ReadOnlyReplica:$true `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Server Manager viene compilato in genere tutti gli argomenti con i valori durante l'innalzamento di livello e non si basa sulle impostazioni predefinite (perché queste potrebbero cambiare nelle future versioni di Windows o service pack). L'unica eccezione a questa è la **- safemodeadministratorpassword** argomento. Per forzare una richiesta di conferma, omettere il valore quando cmdlet viene eseguito in modo interattivo.  
  
Usare l'argomento facoltativo Whatif con il cmdlet Install-ADDSDomainController per esaminare le informazioni di configurazione. In questo modo è possibile visualizzare i valori espliciti e impliciti degli argomenti per un cmdlet.  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCWhatIf.png)  
  
### <a name="prerequisites-check"></a>Controllo dei prerequisiti  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrereqCheck.png)  
  
Il **controllo dei prerequisiti** è una nuova funzionalità di configurazione di dominio Active Directory. Questa nuova fase convalida che la configurazione del server sia in grado di supportare una nuova foresta di Active Directory.  
  
Quando si installa un dominio radice della nuova foresta, il Server di gestione dominio servizi di configurazione guidata Active Directory richiama una serie di test modulari serializzati. Questi test avvisano l'utente con opzioni di ripristino suggerite. Il numero di volte in base alle esigenze, è possibile eseguire i test. Il processo di controller di dominio non può continuare finché tutti i prerequisiti test non passare.  
  
Il **controllo dei prerequisiti** copre anche le informazioni pertinenti, ad esempio modifiche alla sicurezza che interessano i sistemi operativi precedenti.  
  
Non è possibile ignorare il **prerequisiti** quando utilizzando Server Manager, ma è possibile saltare il processo quando si usa il cmdlet di distribuzione di Active Directory con l'argomento seguente:  
  
```  
-skipprechecks  
  
```  
  
Fare clic su **installare** per avviare il processo di innalzamento di livello controller di dominio. Questo è l'ultima opportunità per annullare l'installazione. È possibile annullare il processo di innalzamento di livello una volta iniziato. Il computer verrà riavviato automaticamente al termine dell'innalzamento di livello, indipendentemente dai risultati di innalzamento di livello.  
  
### <a name="installation"></a>Installazione  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCInstallation.png)  
  
Quando il **installazione** pagina viene visualizzata, la configurazione del controller di dominio inizia e non può essere interrotta o annullata. I dettagli delle operazioni vengono visualizzati in questa pagina e scritti nei log:  
  
-   %systemroot%\Debug\Dcpromo.log.  
  
-   %systemroot%\debug\dcpromoui.log  
  
Per installare una nuova foresta di Active Directory con il modulo ADDSDeployment, usare il cmdlet seguente:  
  
```  
Install-addsdomaincontroller  
  
```  
  
Vedere il **Cmdlet di ADDSDeployment** tabella all'inizio di questa sezione per gli argomenti obbligatori e facoltativi.  
  
Il **Install-addsdomaincontroller** cmdlet ha solo due fasi (controllo dei prerequisiti e installazione). Nelle due figure seguenti è illustrata la fase di installazione con gli argomenti minimi obbligatori di **- domainname**, **- readonlyreplica**, **- sitename**, e **-credential**. Si noti, esattamente come Server Manager, **Install-ADDSDomainController** ricorda che l'innalzamento di livello riavvierà automaticamente il server:  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODC.png)  
  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODCProgress.png)  
  
Per accettare automaticamente il prompt di riavvio, usare il **-force** o **-confermare: $false** argomenti con qualsiasi cmdlet di ADDSDeployment di Windows PowerShell. Per impedire il riavvio automatico al termine dell'innalzamento di livello il server, utilizzare il **- norebootoncompletion** argomento.  
  
> [!WARNING]  
> Non è consigliabile eseguire l'override del riavvio. Il controller di dominio deve essere riavviato per funzionare correttamente. Se ci si disconnette il controller di dominio, è possibile riaccedere modo interattivo fino al successivo riavvio.  
  
### <a name="results"></a>Risultati  
![Installare RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCSignoff.png)  
  
Il **risultati** pagina Mostra l'esito positivo o negativo dell'innalzamento di livello e le informazioni amministrative importanti. Il controller di dominio verrà automaticamente riavviato dopo 10 secondi.  
  

