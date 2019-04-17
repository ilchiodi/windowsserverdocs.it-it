---
ms.assetid: e6da5984-d99d-4c34-9c11-4a18cd413f06
title: Installare un Controller di dominio di Replica Windows Server 2012 in un dominio esistente (livello 200)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e3151a8beee2870ecc747a64241df9d562ad4cc2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-replica-windows-server-2012-domain-controller-in-an-existing-domain-level-200"></a>Installare un Controller di dominio di Replica Windows Server 2012 in un dominio esistente (livello 200)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento illustra i passaggi necessari per eseguire l'aggiornamento di un dominio o foresta esistente a Windows Server 2012, usando Server Manager o Windows PowerShell. Viene illustrato come aggiungere controller di dominio che eseguono Windows Server 2012 a un dominio esistente.  
  
-   [Flusso di lavoro di Replica e aggiornamento](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [Aggiornamento e Replica in Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)  
  
-   [Distribuzione](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Dep)  
  
## <a name="BKMK_Workflow"></a>Flusso di lavoro di Replica e aggiornamento  
Il diagramma seguente illustra il processo di configurazione di servizi di dominio Active Directory quando è stato precedentemente installato il ruolo di dominio Active Directory ed è stata avviata il dominio servizi di configurazione guidata Active Directory mediante Server Manager per creare un nuovo controller di dominio in un dominio esistente.  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/adds_forestupgrade.png)  
  
## <a name="BKMK_PS"></a>Aggiornamento e Replica in Windows PowerShell  
  
|||  
|-|-|  
|**Cmdlet di ADDSDeployment**|Gli argomenti (**grassetto** sono necessari argomenti. *Corsivo* argomenti possono essere specificati usando Windows PowerShell o la configurazione guidata di Active Directory.)|  
|Install-AddsDomainController|-SkipPreChecks<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-SiteName*<br /><br />*-ADPrepCredential*<br /><br />-ApplicationPartitionsToReplicate<br /><br />*-AllowDomainControllerReinstall*<br /><br />-Confirm<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />-CriticalReplicationOnly<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-Force<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />-NoDnsOnNetwork<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*-/Replicationsourcedc*<br /><br />-SkipAutoConfigureDNS<br /><br />-SiteName<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />*-UseExistingAccount*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> Il **-credential** argomento è solo necessario se si è non già connessi come membro dei gruppi Enterprise Admins e Schema Admins (se si esegue l'aggiornamento della foresta) o del gruppo Domain Admins (se si aggiunge un nuovo controller di dominio a un dominio esistente).  
  
## <a name="BKMK_Dep"></a>Distribuzione  
  
### <a name="deployment-configuration"></a>Configurazione della distribuzione  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDeployConfig.png)  
  
Server Manager inizia ogni controller di dominio con il **configurazione distribuzione** pagina. Le opzioni restanti e i campi obbligatori modificare in questa pagina e le pagine successive, a seconda operazione di distribuzione selezionato.  
  
Per aggiornare una foresta esistente o aggiungere un controller di dominio scrivibile a un dominio esistente, fare clic su **aggiungere un controller di dominio a un dominio esistente** e fare clic su **selezionare** a **specificare le informazioni di dominio per questo dominio**. Server Manager richiede le credenziali valide se necessario.  
  
Aggiornamento della foresta richiede le credenziali che includano appartenenze a gruppi Enterprise Admins e Schema Admins in Windows Server 2012. La configurazione guidata servizi di dominio Active Directory richiede successivamente se le credenziali correnti non dispongono di autorizzazioni adeguate o appartenenza al gruppo.  
  
Il processo Adprep automatico è la sola differenza operativa tra l'aggiunta di un controller di dominio a un dominio Windows Server 2012 esistente e un dominio in cui i controller di dominio eseguono una versione precedente di Windows Server.  
  
Il cmdlet di ADDSDeployment per configurazione distribuzione e gli argomenti sono:  
  
```  
Install-AddsDomainController  
-domainname <string>  
-credential <pscredential>  
```  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeSelectDomain.png)  
  
Alcuni test eseguire in ogni pagina, alcuni dei quali ripetere in un secondo momento come discreti controlli dei prerequisiti. Ad esempio, se il dominio selezionato non soddisfa i livelli di funzionalità minimi, non è necessario passare attraverso l'innalzamento di livello per il controllo dei prerequisiti per scoprire:  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeFFLError.png)  
  
### <a name="domain-controller-options"></a>Opzioni Controller di dominio  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptions.png)  
  
Il **opzioni Controller di dominio** pagina specifica le funzionalità del controller di dominio per il nuovo controller di dominio. La funzionalità del controller di dominio configurabili sono **server DNS**, **catalogo globale**, e **controller di dominio di sola lettura**. È consigliabile che tutti i controller di dominio forniscano servizi DNS e catalogo globale per la disponibilità elevata negli ambienti distribuiti. Catalogo globale è sempre selezionato per impostazione predefinita e server DNS è selezionato per impostazione predefinita, se il dominio corrente ospita già DNS sui controller di dominio basato su query origine di autorità. Il **opzioni Controller di dominio** pagina è inoltre possibile scegliere di Active Directory appropriato logica **nome sito** dalla configurazione della foresta. Per impostazione predefinita, viene selezionato il sito con la subnet più corretta. Se è presente un solo sito, viene selezionato automaticamente.  
  
> [!NOTE]  
> Se il server non fa parte di una subnet di Active Directory ed è presente più di un sito di Active Directory, viene selezionato nulla e il **Avanti** pulsante non è disponibile fino a quando non viene scelto un sito dall'elenco.  
  
L'oggetto specificato **Password modalità ripristino servizi Directory** deve soddisfare i criteri password applicati al server. Scegliere sempre una password complessa o preferibilmente una passphrase.  
  
Il **opzioni Controller di dominio** gli argomenti ADDSDeployment sono:  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-sitename <string>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> Il nome del sito deve esistere già quando viene fornito come argomento al **- sitename**. Il **install-AddsDomainController** cmdlet non crea i siti. È possibile utilizzare cmdlet **nuovo-adreplicationsite** per creare nuovi siti.  
  
Il **SafeModeAdministratorPassword** operazione dell'argomento è particolare:  
  
-   Se *non specificato* come argomento, il cmdlet chiede di immettere e confermare una password nascosta. Questo è il metodo di utilizzo quando esegue il cmdlet in modo interattivo.  
  
    Ad esempio, per creare un controller di dominio nel dominio treyresearch.net e ricevere la richiesta di immettere e confermare una password nascosta:  
  
    ```  
    Install-ADDSDomainController "DomainName treyresearch.net "credential (get-credential)  
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
> Non è consigliabile inserire o archiviare una password Cancella o di testo offuscato. Chiunque esegua questo comando in uno script o ti spii a conoscenza della password modalità ripristino servizi directory del controller di dominio.  Chiunque abbia accesso al file può invertire la password offuscata. Conoscendola, possono accedere a un controller di dominio avviato in modalità ripristino servizi directory e infine fingersi il controller di dominio, innalzando i propri privilegi al massimo livello in una foresta di Active Directory. Un altro set di passaggi utilizzando **System.Security.Cryptography** per crittografare il file di testo dati sono consigliabile ma di fuori ambito. La procedura consigliata è di evitare completamente l'archiviazione delle password.  
  
Il cmdlet ADDSDeployment offre un'altra opzione per ignorare la configurazione automatica di impostazioni del client DNS, server d'inoltro e parametri radice. È possibile ignorare questa opzione di configurazione quando si utilizza Server Manager. Questo argomento è importante solo se è installato il ruolo Server DNS prima di configurare il controller di dominio:  
  
```  
-SkipAutoConfigureDNS  
```  
  
Il **opzioni Controller di dominio** pagina avvisa che è possibile creare lettura solo controller di dominio se il controller di dominio esistenti eseguono Windows Server 2003. È previsto, ed è possibile ignorare l'avviso.  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptionsError.png)  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>Opzioni DNS e credenziali di delega DNS  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDNSOptions.png)  
  
Il **opzioni DNS** pagina consente di configurare la delega DNS se è stato selezionato il **server DNS** opzione il *opzioni Controller di dominio* pagina e se si fa riferimento a una zona in cui le deleghe DNS sono consentite. Potrebbe essere necessario fornire credenziali alternative di un utente che è membro del **DNS Admins** gruppo.  
  
Il **opzioni DNS** sono gli argomenti del cmdlet ADDSDeployment:  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
For more information about whether you need to create a DNS delegation, see [Understanding Zone Delegation](https://technet.microsoft.com/library/cc771640.aspx).  
  
### <a name="additional-options"></a>Opzioni aggiuntive  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeAdditionalOptions.png)  
  
Il **opzioni aggiuntive** pagina fornisce l'opzione di configurazione per denominare un controller di dominio come origine della replica, oppure è possibile utilizzare qualsiasi controller di dominio come origine della replica.  
  
È inoltre possibile installare il controller di dominio usando supporti utilizzando l'installazione dall'opzione di supporto di backup. Il **installa da supporto** casella di controllo offre un'opzione di selezione e deve fare clic su **verifica** per verificare il percorso specificato sia un supporto valido. I supporti compatibili con l'opzione viene creato con Windows Server Backup o Ntdsutil.exe da un altro esistente computer Windows Server 2012. è possibile utilizzare un Windows Server 2008 R2 o un sistema operativo precedente per creare il supporto per un controller di dominio Windows Server 2012. Per ulteriori informazioni sulle modifiche nell'installazione da supporto, vedere [appendice sull'amministrazione semplificata](../../ad-ds/deploy/Simplified-Administration-Appendix.md). Se Usa un supporto protetto da SYSKEY, Server Manager richiede la password dell'immagine durante la verifica.  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_NtdsutilIFM.png)  
  
Il **opzioni aggiuntive** sono gli argomenti del cmdlet ADDSDeployment:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-syskey <secure string>  
```  
  
### <a name="paths"></a>Percorsi  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
Il **percorsi** pagina consente di sostituire i percorsi predefiniti del database di Active Directory, i registri delle transazioni di database, e la condivisione SYSVOL. I percorsi predefiniti sono sempre in sottodirectory di % systemroot %.  
  
Gli argomenti del cmdlet ADDSDeployment per percorsi Active Directory sono:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>Opzioni di preparazione  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptions.png)  
  
Il **opzioni di preparazione** pagina segnala che la configurazione di Active Directory include l'estensione dello Schema (forestprep) e l'aggiornamento del dominio (domainprep).  Questa pagina viene visualizzata solo quando la foresta e il dominio non sono stati preparati da un'installazione precedente del controller dominio Windows Server 2012 o dall'esecuzione manuale di Adprep.exe. La configurazione guidata servizi di dominio Active Directory, ad esempio, Elimina questa pagina se si aggiunge un nuovo controller di dominio a un dominio radice della foresta Windows Server 2012 esistente.  
  
Estensione dello Schema e l'aggiornamento del dominio non si verificano quando si fa clic **Avanti**. Questi eventi si verificano solo durante la fase di installazione. Questa pagina elenca semplicemente gli eventi che si verificheranno in seguito durante l'installazione.  
  
Questa pagina inoltre convalida che le credenziali dell'utente corrente sono membri dei gruppi Schema Admins ed Enterprise Admins, in base alle esigenze di questi gruppi per estendere lo schema o preparare un dominio. Fare clic su **modifica** per fornire le credenziali utente adeguate se la pagina informa che le credenziali correnti non forniscono autorizzazioni sufficienti.  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptionsCreds.png)  
  
È l'argomento del cmdlet ADDSDeployment di opzioni aggiuntive:  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> Come con le versioni precedenti di Windows Server, preparazione del dominio automatizzata per i controller di dominio che eseguono Windows Server 2012 non esegue GPPREP. Eseguire **adprep.exe /gpprep** manualmente per tutti i domini non preparati in precedenza per Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2. È consigliabile eseguire GPPrep una sola volta nella cronologia di un dominio, non a ogni aggiornamento. Adprep.exe non viene eseguito automaticamente /gpprep perché l'operazione può causare tutti i file e cartelle nella cartella SYSVOL per nuovamente replicato in tutti i controller di dominio.  
>   
> RODCPrep viene eseguito automaticamente quando si innalza di livello il primo controller di dominio senza installazione di appoggio in un dominio. Non si verifica quando si innalza di livello il primo controller di dominio Windows Server 2012 scrivibile. È possibile anche manualmente **adprep.exe /rodcprep** se si prevede di distribuire controller di dominio di sola lettura.  
  
### <a name="review-options-and-view-script"></a>Verifica opzioni e Visualizza Script  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeReviewOptions.png)  
  
Il **verifica opzioni** pagina consente di convalidare le impostazioni e accertarsi se soddisfano i requisiti prima di iniziare l'installazione. Questo non è l'ultima possibilità per interrompere l'installazione utilizzando Server Manager. Questa pagina consente semplicemente di rivedere e confermare le impostazioni prima di proseguire con la configurazione.  
  
Il **verifica opzioni** pagina in Server Manager offre anche un facoltativo **Visualizza Script** pulsante per creare un file di testo Unicode contenente la configurazione ADDSDeployment corrente come singolo script di Windows PowerShell. In questo modo è possibile utilizzare l'interfaccia grafica di Server Manager come un strumento di distribuzione di Windows PowerShell. Utilizzare la configurazione guidata servizi di dominio Active Directory per configurare le opzioni, esportare la configurazione e quindi annullare la procedura guidata.  Questo processo crea un esempio valido e sintatticamente corretto utilizzato direttamente o successivamente modificato.  
  
Per esempio:  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-CreateDNSDelegation `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainName "root.fabrikam.com" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Server Manager viene compilato in genere tutti gli argomenti con i valori durante l'innalzamento di livello e non si basa sulle impostazioni predefinite (perché queste potrebbero cambiare nelle future versioni di Windows o service pack). L'unica eccezione a questa è la **- safemodeadministratorpassword** argomento. Per forzare una richiesta di conferma omettere il valore quando cmdlet viene eseguito in modo interattivo.  
>   
> Utilizzare facoltativo **Whatif** argomento con il **Install-ADDSDomainController** cmdlet per rivedere le informazioni di configurazione. In questo modo è possibile visualizzare i valori espliciti e impliciti degli argomenti per un cmdlet.  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSWhatIf.png)  
  
### <a name="prerequisites-check"></a>Controllo dei prerequisiti  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrereqCheck.png)  
  
Il **controllo dei prerequisiti** è una nuova funzionalità di configurazione di dominio Active Directory. Questa nuova fase convalida che il dominio e foresta sono in grado di supportare un nuovo controller di dominio di Windows Server 2012.  
  
Quando si installa un nuovo controller di dominio, il Server di gestione dominio servizi di configurazione guidata Active Directory richiama una serie di test modulari serializzati. Questi test avvisano l'utente con opzioni di ripristino suggerite. Il numero di volte in base alle esigenze, è possibile eseguire i test. Il processo di controller di dominio non può continuare finché tutti i prerequisiti test non passare.  
  
Il **controllo dei prerequisiti** copre anche le informazioni pertinenti, ad esempio modifiche alla sicurezza che interessano i sistemi operativi precedenti.  
  
Per ulteriori informazioni sui controlli dei prerequisiti specifici, vedere [controllo dei prerequisiti](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Non è possibile ignorare il **prerequisiti** quando utilizzando Server Manager, ma è possibile saltare il processo quando si usa il cmdlet di distribuzione di Active Directory con l'argomento seguente:  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft sconsiglia di ignorare il controllo dei prerequisiti, come può portare a una promozione del controller di dominio parziali o danneggiato foresta di Active Directory.  
  
Fare clic su **installare** per avviare il processo di innalzamento di livello controller di dominio. Questo è l'ultima opportunità per annullare l'installazione. È possibile annullare il processo di innalzamento di livello una volta iniziato. Il computer verrà riavviato automaticamente al termine dell'innalzamento di livello, indipendentemente dai risultati di innalzamento di livello. Il **controllo dei prerequisiti** pagina Visualizza tutti i problemi incontrato durante il processo e indicazioni per la risoluzione del problema.  
  
### <a name="installation"></a>Installazione  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeInstallProgress.png)  
  
Quando il **installazione** pagina viene visualizzata, la configurazione del controller di dominio inizia e non può essere interrotta o annullata. I dettagli delle operazioni vengono visualizzati in questa pagina e scritti nei log:  
  
-   %systemroot%\Debug\Dcpromo.log.  
  
-   %systemroot%\debug\dcpromoui.log  
  
-   %systemroot%\debug\adprep\logs  
  
-   %systemroot%\Debug\Netsetup.log (se il server si trova in un gruppo di lavoro)  
  
Per installare una nuova foresta di Active Directory con il modulo ADDSDeployment, usare il cmdlet seguente:  
  
```  
Install-addsdomaincontroller  
```  
  
Vedere [aggiornamento e Replica in Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS) per gli argomenti obbligatori e facoltativi.  
  
Il **Install-AddsDomainController** cmdlet ha solo due fasi (controllo dei prerequisiti e installazione). Nelle due figure seguenti è illustrata la fase di installazione con gli argomenti minimi obbligatori di **- domainname** e **-credential**. Si noti come l'operazione Adprep viene eseguita automaticamente come parte di aggiungere il primo controller di dominio di Windows Server 2012 a una foresta esistente di Windows Server 2003:  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSGetCred.png)  
  
Si noti, esattamente come Server Manager, **Install-ADDSDomainController** ricorda che l'innalzamento di livello riavvierà automaticamente il server. Per accettare automaticamente il prompt di riavvio, usare il **-force** o **-confermare: $false** argomenti con qualsiasi cmdlet di ADDSDeployment di Windows PowerShell. Per impedire il riavvio automatico al termine dell'innalzamento di livello il server, utilizzare il **- norebootoncompletion** argomento.  
  
> [!WARNING]  
> Non è consigliabile eseguire l'override del riavvio. Il controller di dominio deve essere riavviato per funzionare correttamente.  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeConfirm.gif)  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeProgress.png)  
  
Per configurare un controller di dominio in remoto mediante Windows PowerShell, eseguire il wrapping di **install-adddomaincontroller** cmdlet *all'interno di* del **invoke-command** cmdlet. È necessario usare le parentesi graffe.  
  
```  
invoke-command {install-addsdomaincontroller "domainname <domain> -credential (get-credential)} -computername <dc name>  
```  
  
Per esempio:  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeExample.gif)  
  
> [!NOTE]  
> Per ulteriori informazioni su come il processo di installazione e Adprep, vedere il [risoluzione dei problemi di distribuzione del Controller di dominio](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).  
  
### <a name="results"></a>Risultati  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
Il **risultati** pagina Mostra l'esito positivo o negativo dell'innalzamento di livello e le informazioni amministrative importanti. Se ha esito positivo, il controller di dominio verrà automaticamente riavviato dopo 10 secondi.  
  
Come con le versioni precedenti di Windows Server, preparazione del dominio automatizzata per i controller di dominio che eseguono Windows server 2012 non esegue GPPREP. Eseguire **adprep.exe /gpprep** manualmente per tutti i domini non preparati in precedenza per Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2. È consigliabile eseguire GPPrep una sola volta nella cronologia di un dominio, non a ogni aggiornamento. Adprep.exe non viene eseguito automaticamente /gpprep perché l'operazione può causare tutti i file e cartelle nella cartella SYSVOL per nuovamente replicato in tutti i controller di dominio.  
  
