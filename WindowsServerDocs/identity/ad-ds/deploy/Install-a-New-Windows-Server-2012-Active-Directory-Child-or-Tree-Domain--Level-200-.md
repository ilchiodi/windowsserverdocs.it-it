---
ms.assetid: e3d55565-ad45-4504-ad73-8103d1a92170
title: Installare un nuovo figlio di Active Directory di Windows Server 2012 o dominio albero (livello 200)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fc0eecc44bbc5f7459f22aceb5ebe41cd61948b6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-new-windows-server-2012-active-directory-child-or-tree-domain-level-200"></a>Installare un nuovo figlio di Active Directory di Windows Server 2012 o dominio albero (livello 200)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento spiega come aggiungere domini figlio e albero a una foresta esistente di Windows Server 2012, usando Server Manager o Windows PowerShell.  
  
-   [Flusso di lavoro dominio albero e figlio](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [Figlio e albero dominio Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)  
  
-   [Distribuzione](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Deployment)  
  
## <a name="BKMK_Workflow"></a>Flusso di lavoro dominio albero e figlio  
Il diagramma seguente illustra il processo di configurazione di servizi di dominio Active Directory quando è stato precedentemente installato il ruolo di dominio Active Directory ed è stata avviata il dominio servizi di configurazione guidata Active Directory mediante Server Manager per creare un nuovo dominio in una foresta esistente.  
  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/adds_childtreedeploy_beta1.png)  
  
## <a name="BKMK_PS"></a>Figlio e albero dominio Windows PowerShell  
  
|||  
|-|-|  
|**Cmdlet di ADDSDeployment**|Gli argomenti (**grassetto** sono necessari argomenti. *Corsivo* argomenti possono essere specificati usando Windows PowerShell o la configurazione guidata di Active Directory.)|  
|**Install-AddsDomain**|-SkipPreChecks<br /><br />***-NewDomainName***<br /><br />***-ParentDomainName***<br /><br />***-SafeModeAdministratorPassword***<br /><br />*-ADPrepCredential*<br /><br />-AllowDomainReinstall<br /><br />-Confirm<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-NoDNSOnNetwork<br /><br />*DomainMode-*<br /><br />***-DomainType***<br /><br />-Force<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />*-NewDomainNetBIOSName*<br /><br />*-NoGlobalCatalog*<br /><br />-NoNorebootoncompletion<br /><br />*-/Replicationsourcedc*<br /><br />*-SiteName*<br /><br />-SkipAutoConfigureDNS<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> Il **-credential** argomento è solo necessario quando non attualmente connessi al computer come membro del gruppo Enterprise Admins. Il **- NewDomainNetBIOSName** argomento è obbligatorio se si desidera modificare il nome di 15 caratteri generato automaticamente in base a prefisso del nome di dominio DNS o se il nome eccede i 15 caratteri.  
  
## <a name="BKMK_Deployment"></a>Distribuzione  
  
### <a name="deployment-configuration"></a>Configurazione della distribuzione  
Nella schermata seguente mostra le opzioni per l'aggiunta di un dominio figlio:  
  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDeployConfig.png)  
  
Nella schermata seguente mostra le opzioni per l'aggiunta di un dominio albero:  
  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_TreeDeployConfig.png)  
  
Server Manager inizia ogni controller di dominio con il **configurazione distribuzione** pagina. Le opzioni restanti e i campi obbligatori modificare in questa pagina e le pagine successive, a seconda operazione di distribuzione selezionato.  
  
Questo argomento vengono combinate due operazioni diverse: innalzamento di livello del dominio figlio e innalzamento di livello del dominio albero. L'unica differenza tra le due operazioni è il tipo di dominio che si desidera creare. Tutti gli altri passaggi sono identici tra le due operazioni.  
  
-   Per creare un nuovo dominio figlio, fare clic su **aggiunta di un dominio a una foresta esistente** e scegli **dominio figlio**. Per **nome del dominio padre**, digitare o selezionare il nome del dominio padre. Digitare il nome del nuovo dominio nel **nuovo nome di dominio** casella. Specificare un figlio etichetta singola valido nome di dominio. il nome deve rispettare i requisiti dei nomi di dominio DNS.  
  
-   Per creare un dominio albero in una foresta esistente, fare clic su **aggiunta di un dominio a una foresta esistente** e scegli **dominio albero**. Digitare il nome del dominio radice della foresta e quindi digitare il nome del nuovo dominio. Specificare un nome di dominio radice valido e completo. il nome non può essere con etichetta singola e deve rispettare requisiti dei nomi di dominio DNS.  
  
Per ulteriori informazioni sui nomi DNS, vedere [convenzioni di denominazione in Active Directory per computer, domini, siti e unità organizzative](https://support.microsoft.com/kb/909264).  
  
Il Server di gestione dominio servizi di configurazione guidata Active Directory richiede l'immissione delle credenziali di dominio se le credenziali correnti non dal dominio. Fare clic su **modifica** per fornire le credenziali di dominio per l'operazione di innalzamento di livello.  
  
Il cmdlet di ADDSDeployment per configurazione distribuzione e gli argomenti sono:  
  
```  
Install-AddsDomain  
-domaintype <{childdomain | treedomain}>  
-parentdomainname <string>  
-newdomainname <string>  
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Opzioni Controller di dominio  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_DCOptions_Child.gif)  
  
Il **opzioni Controller di dominio** pagina specifica le opzioni controller di dominio per il nuovo controller di dominio. Le opzioni controller di dominio configurabili includono **server DNS** e **catalogo globale**; è possibile configurare i controller di dominio di sola lettura come primo controller di dominio in un nuovo dominio.  
  
È consigliabile che tutti i controller di dominio forniscano servizi DNS e catalogo globale per la disponibilità elevata negli ambienti distribuiti. Catalogo globale è sempre selezionato per impostazione predefinita e DNS è selezionato per impostazione predefinita, se il dominio corrente ospita già DNS sui controller di dominio basato su una query origine di autorità. È inoltre necessario specificare un **livello di funzionalità del dominio**. È possibile scegliere qualsiasi altro valore uguale o maggiore rispetto al livello di funzionalità foresta corrente e il livello di funzionalità predefinito è Windows Server 2012.  
  
Il **opzioni Controller di dominio** pagina è inoltre possibile scegliere di Active Directory appropriato logica **nome sito** dalla configurazione della foresta. Per impostazione predefinita, viene selezionato il sito con la subnet più corretta. Se è presente un solo sito, viene selezionato automaticamente.  
  
> [!IMPORTANT]  
> Se il server non fa parte di una subnet di Active Directory ed è presente più di un sito di Active Directory, viene selezionato nulla e il **Avanti** pulsante non è disponibile fino a quando non viene scelto un sito dall'elenco.  
  
L'oggetto specificato **Password modalità ripristino servizi Directory** deve soddisfare i criteri password applicati al server. Scegliere sempre una password complessa o preferibilmente una passphrase.  
  
Il **opzioni Controller di dominio** sono gli argomenti del cmdlet ADDSDeployment:  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-Sitename <string>  
-SafeModeAdministratorPassword <secure string>  
-Credential <pscredential>  
```  
  
> [!IMPORTANT]  
> Il nome del sito deve esistere già quando viene fornito come valore per il **sitename** argomento. Il **install-AddsDomainController** cmdlet non crea i nomi dei siti. È possibile utilizzare il **nuovo-adreplicationsite** cmdlet per creare nuovi siti.  
  
Il **Install-ADDSDomainController** gli argomenti del cmdlet seguono le stesse impostazioni predefinite di Server Manager, se non specificato.  
  
Il **SafeModeAdministratorPassword** operazione dell'argomento è particolare:  
  
-   Se *non specificato* come argomento, il cmdlet chiede di immettere e confermare una password nascosta. Questo è il metodo di utilizzo quando esegue il cmdlet in modo interattivo.  
  
    Ad esempio, per creare un nuovo figlio di dominio denominato NorthAmerica nella foresta Contoso.com e richiesto di immettere e confermare una password nascosta:  
  
    ```  
    Install-ADDSDomain "NewDomainName NorthAmerica "ParentDomainName Contoso.com "DomainType Child  
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
  
Il modulo ADDSDeployment offre un'altra opzione per ignorare la configurazione automatica di impostazioni del client DNS, server d'inoltro e parametri radice. Non è configurabile quando si utilizza Server Manager. Questo argomento è importante solo se è già installato il servizio Server DNS prima di configurare il controller di dominio:  
  
```  
-SkipAutoConfigureDNS  
  
```  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>Opzioni DNS e credenziali di delega DNS  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDNSOptions.png)  
  
Il **opzioni DNS** pagina consente di fornire credenziali amministrative DNS alternative per la delega.  
  
Quando si installa un nuovo dominio in una foresta esistente - in selezionato installazione DNS nel **opzioni Controller di dominio** pagina - non è possibile configurare le opzioni; la delega avviene automaticamente e definitivamente. Hai la possibilità di fornire credenziali amministrative DNS alternative con i diritti per aggiornare la struttura.  
  
Il **opzioni DNS** gli argomenti ADDSDeployment di Windows PowerShell sono:  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
Per ulteriori informazioni sulla delega DNS, vedere [informazioni sulla delega delle Zone](https://technet.microsoft.com/library/cc771640.aspx).  
  
### <a name="additional-options"></a>Opzioni aggiuntive  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildAdditionalOptions.png)  
  
Il **opzioni aggiuntive** pagina Mostra il nome NetBIOS del dominio e consente di sostituirlo. Per impostazione predefinita, il nome di dominio NetBIOS corrisponde all'etichetta a sinistra del nome di dominio completo fornito sul **configurazione distribuzione** pagina. Ad esempio, se è stato specificato il nome di dominio completo di corp.contoso.com, il nome di dominio NetBIOS predefinito è CORP.  
  
Se il nome è di 15 caratteri o meno e non è in conflitto con un altro nome NetBIOS, non viene modificato. Se è in conflitto con un altro nome NetBIOS, un numero viene aggiunto al nome. Se il nome è più di 15 caratteri, la procedura guidata fornisce un suggerimento univoco troncato. In entrambi i casi, la procedura guidata convalida prima il nome non è già in uso tramite una ricerca WINS e una trasmissione NetBIOS.  
  
Per ulteriori informazioni sui nomi DNS, vedere [convenzioni di denominazione in Active Directory per computer, domini, siti e unità organizzative](https://support.microsoft.com/kb/909264).  
  
Il **Install-AddsDomain** argomenti seguono le stesse impostazioni predefinite di Server Manager, se non specificato. Il **DomainNetBIOSName** operazione è particolare:  
  
1.  Se il **NewDomainNetBIOSName** argomento non è specificato con un nome di dominio NetBIOS e il nome di dominio con prefisso con etichetta singola nel **DomainName** argomento è di 15 caratteri o meno, l'innalzamento di livello continua con un nome generato automaticamente.  
  
2.  Se il **NewDomainNetBIOSName** argomento non è specificato con un nome di dominio NetBIOS e il nome di dominio con prefisso con etichetta singola nel **DomainName** argomento è di 16 caratteri o più, l'innalzamento di livello ha esito negativo.  
  
3.  Se il **NewDomainNetBIOSName** argomento è specificato con un nome di dominio NetBIOS di 15 caratteri o meno, l'innalzamento di livello continua con il nome specificato.  
  
4.  Se il **NewDomainNetBIOSName** argomento specificato con un nome di dominio NetBIOS di 16 caratteri o più, l'innalzamento di livello ha esito negativo.  
  
Il **opzioni aggiuntive** argomento cmdlet ADDSDeployment:  
  
```  
-newdomainnetbiosname <string>  
```  
  
### <a name="paths"></a>Percorsi  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
Il **percorsi** pagina consente di sostituire i percorsi predefiniti del database di dominio Active Directory, i registri delle transazioni di base dei dati e la condivisione SYSVOL. I percorsi predefiniti sono sempre in sottodirectory di % systemroot %.  
  
Il **percorsi** sono gli argomenti del cmdlet ADDSDeployment:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>Verifica opzioni e Visualizza Script  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildReviewOptions.png)  
  
Il **verifica opzioni** pagina consente di convalidare le impostazioni e accertarsi se soddisfano i requisiti prima di iniziare l'installazione. Questo non è l'ultima possibilità per interrompere l'installazione quando si utilizza Server Manager. Si tratta semplicemente un'opzione per confermare le impostazioni prima di proseguire con la configurazione  
  
Il **verifica opzioni** pagina in Server Manager offre anche un facoltativo **Visualizza Script** pulsante per creare un file di testo Unicode contenente la configurazione ADDSDeployment corrente come singolo script di Windows PowerShell. In questo modo è possibile utilizzare l'interfaccia grafica di Server Manager come un strumento di distribuzione di Windows PowerShell. Utilizzare la configurazione guidata servizi di dominio Active Directory per configurare le opzioni, esportare la configurazione e quindi annullare la procedura guidata.  Questo processo crea un esempio valido e sintatticamente corretto utilizzato direttamente o successivamente modificato. Per esempio:  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomain `  
-NoGlobalCatalog:$false `  
-CreateDNSDelegation `  
-Credential (Get-Credential) `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainMode "Win2012" `  
-DomainType "ChildDomain" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-NewDomainName "research" `  
-NewDomainNetBIOSName "RESEARCH" `  
-ParentDomainName "corp.contoso.com" `  
-Norebootoncompletion:$false `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Server Manager viene compilato in genere tutti gli argomenti con i valori durante l'innalzamento di livello e non si basa sulle impostazioni predefinite (perché queste potrebbero cambiare nelle future versioni di Windows o service pack). L'unica eccezione a questa è la **- safemodeadministratorpassword** argomento (che viene deliberatamente omesso dallo script). Per forzare una richiesta di conferma, omettere il valore quando cmdlet viene eseguito in modo interattivo.  
  
Utilizzare facoltativo **Whatif** argomento con il **Install-ADDSForest** cmdlet per rivedere le informazioni di configurazione. In questo modo è possibile visualizzare i valori espliciti e impliciti degli argomenti per un cmdlet.  
  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildWhatIf.png)  
  
### <a name="prerequisites-check"></a>Controllo dei prerequisiti  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildPrereqCheck.png)  
  
Il **controllo dei prerequisiti** è una nuova funzionalità di configurazione di dominio Active Directory. Questa nuova fase convalida che la configurazione del server sia in grado di supportare un nuovo dominio di Active Directory.  
  
Quando si installa un dominio radice della nuova foresta, il Server di gestione dominio servizi di configurazione guidata Active Directory richiama una serie di test modulari serializzati. Questi test avvisano l'utente con opzioni di ripristino suggerite. Il numero di volte in base alle esigenze, è possibile eseguire i test. Il processo di controller di dominio non può continuare finché tutti i prerequisiti test non passare.  
  
Il **controllo dei prerequisiti** copre anche le informazioni pertinenti, ad esempio modifiche alla sicurezza che interessano i sistemi operativi precedenti.  
  
Per ulteriori informazioni sui controlli dei prerequisiti specifici, vedere [controllo dei prerequisiti](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Non è possibile ignorare il **prerequisiti** quando utilizzando Server Manager, ma è possibile saltare il processo quando si usa il cmdlet di distribuzione di Active Directory con l'argomento seguente:  
  
```  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft sconsiglia di ignorare il controllo dei prerequisiti, come può portare a una promozione del controller di dominio parziali o danneggiato foresta di Active Directory.  
  
Fare clic su **installare** per avviare il processo di innalzamento di livello controller di dominio. Questo è l'ultima opportunità per annullare l'installazione. È possibile annullare il processo di innalzamento di livello una volta iniziato. Il computer verrà riavviato automaticamente al termine dell'innalzamento di livello, indipendentemente dai risultati di innalzamento di livello.  
  
### <a name="installation"></a>Installazione  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildInstallation.png)  
  
Quando il **installazione** pagina viene visualizzata, la configurazione del controller di dominio inizia e non può essere interrotta o annullata. I dettagli delle operazioni vengono visualizzati in questa pagina e scritti nei log:  
  
-   %systemroot%\Debug\Dcpromo.log.  
  
-   %systemroot%\debug\dcpromoui.log  
  
Per installare un nuovo dominio di Active Directory con il modulo ADDSDeployment, usare il cmdlet seguente:  
  
```  
Install-addsdomain  
```  
  
Vedere [figlio e albero di dominio Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS) per gli argomenti obbligatori e facoltativi. Il **Install-addsdomain** cmdlet ha solo due fasi (controllo dei prerequisiti e installazione). Nelle due figure seguenti è illustrata la fase di installazione con gli argomenti minimi obbligatori di **- domaintype**, **- newdomainname**, **- parentdomainname**, e **-credential**. Si noti, esattamente come Server Manager, **Install-ADDSDomain** ricorda che l'innalzamento di livello riavvierà automaticamente il server.  
  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomain.png)  
  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomainProgress.png)  
  
Per accettare automaticamente il prompt di riavvio, usare il **-force** o **-confermare: $false** argomenti con qualsiasi cmdlet di ADDSDeployment di Windows PowerShell. Per impedire il riavvio automatico al termine dell'innalzamento di livello il server, utilizzare il **- norebootoncompletion** argomento.  
  
> [!WARNING]  
> Non è consigliabile eseguire l'override del riavvio. Il controller di dominio deve essere riavviato per funzionare correttamente  
  
### <a name="results"></a>Risultati  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
Il **risultati** pagina Mostra l'esito positivo o negativo dell'innalzamento di livello e le informazioni amministrative importanti. Il controller di dominio verrà automaticamente riavviato dopo 10 secondi.  
  

