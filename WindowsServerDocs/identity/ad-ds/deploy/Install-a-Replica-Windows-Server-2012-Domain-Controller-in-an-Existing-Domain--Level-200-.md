---
ms.assetid: e6da5984-d99d-4c34-9c11-4a18cd413f06
title: Installare un controller di dominio di replica di Windows Server 2012 in un dominio esistente (livello 200)
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 12068e5a062358463cf208f777144091e1de8257
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80825204"
---
# <a name="install-a-replica-windows-server-2012-domain-controller-in-an-existing-domain-level-200"></a>Installare un controller di dominio di replica di Windows Server 2012 in un dominio esistente (livello 200)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono illustrati i passaggi necessari per aggiornare una foresta o un dominio esistente a Windows Server 2012, con Server Manager o Windows PowerShell. Viene illustrato come aggiungere controller di dominio che eseguono Windows Server 2012 a un dominio esistente.  
  
-   [Flusso di lavoro di aggiornamento e replica](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [Eseguire l'aggiornamento e la replica di Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)  
  
-   [Distribuzione](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Dep)  
  
## <a name="upgrade-and-replica-workflow"></a><a name="BKMK_Workflow"></a>Flusso di lavoro di aggiornamento e replica  
Il diagramma seguente illustra il processo di configurazione di Servizi di dominio Active Directory, quando in precedenza è stato installato il ruolo Servizi di dominio Active Directory ed è stata avviata la Configurazione guidata Servizi di dominio Active Directory mediante Server Manager per creare un nuovo controller di dominio in un dominio esistente.  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/adds_forestupgrade.png)  
  
## <a name="upgrade-and-replica-windows-powershell"></a><a name="BKMK_PS"></a>Eseguire l'aggiornamento e la replica di Windows PowerShell  
  
|||  
|-|-|  
|**Cmdlet ADDSDeployment**|Argomenti. Gli argomenti in **grassetto** sono obbligatori. Gli argomenti in *corsivo* possono essere specificati usando Windows PowerShell o la Configurazione guidata Servizi di dominio Active Directory.|  
|Install-AddsDomainController|-SkipPreChecks<p>***-DomainName***<p>*-SafeModeAdministratorPassword*<p>*-SiteName*<p>*-ADPrepCredential*<p>-ApplicationPartitionsToReplicate<p>*-AllowDomainControllerReinstall*<p>-Confirm<p>*-CreateDNSDelegation*<p>***-Credential***<p>-CriticalReplicationOnly<p>*-DatabasePath*<p>*-DNSDelegationCredential*<p>-Force<p>*-InstallationMediaPath*<p>*-InstallDNS*<p>*-LogPath*<p>-MoveInfrastructureOperationMasterRoleIfNecessary<p>-NoDnsOnNetwork<p>*-NoGlobalCatalog*<p>-Norebootoncompletion<p>*-ReplicationSourceDC*<p>-SkipAutoConfigureDNS<p>-SiteName<p>*-SystemKey*<p>*-SYSVOLPath*<p>*-UseExistingAccount*<p>*-WhatIf*|  
  
> [!NOTE]  
> L'argomento **-credential** è obbligatorio solo se non si è già connessi come membro dei gruppi Enterprise Admins e Schema Admins (se si deve aggiornare la foresta) o del gruppo Domain Admins (se si deve aggiungere un nuovo controller di dominio a un dominio esistente).  
  
## <a name="deployment"></a><a name="BKMK_Dep"></a>Distribuzione  
  
### <a name="deployment-configuration"></a>Configurazione distribuzione  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDeployConfig.png)  
  
Server Manager inizia l'innalzamento di livello di ogni controller di dominio nella pagina **Configurazione distribuzione** . Le opzioni restanti e i campi obbligatori in questa pagina e nella pagine successive sono diversi a seconda dell'operazione di distribuzione selezionata.  
  
Per aggiornare una foresta esistente o aggiungere un controller di dominio scrivibile a un dominio esistente, fare clic su **Aggiungi un controller di dominio a un dominio esistente** , quindi su **Seleziona** per **Specificare le informazioni di dominio per questa operazione**. Server Manager richiede di specificare delle credenziali valide, se necessario.  
  
L'aggiornamento della foresta richiede di specificare credenziali che includano l'appartenenza a entrambi i gruppi Enterprise Admins e Schema Admins in Windows Server 2012. La Configurazione guidata Servizi di dominio Active Directory le richiede successivamente se le credenziali correnti non dispongono delle necessarie autorizzazioni o appartenenze ai gruppi.  
  
Il processo Adprep automatico è la sola differenza operativa tra l'aggiunta di un controller di dominio a un dominio di Windows Server 2012 esistente e a un dominio in cui i controller di dominio eseguono una versione precedente di Windows Server.  
  
Il cmdlet di ADDSDeployment per Configurazione distribuzione e gli argomenti sono:  
  
```  
Install-AddsDomainController  
-domainname <string>  
-credential <pscredential>  
```  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeSelectDomain.png)  
  
In ogni pagina vengono determinati alcuni test, alcuni dei quali vengono ripetuti in seguito come controlli dei prerequisiti distinte. Ad esempio, se il dominio selezionato non soddisfa i livelli minimi di funzionalità, per scoprirlo non è necessario completare l'innalzamento di livello fino al controllo dei prerequisiti:  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeFFLError.png)  
  
### <a name="domain-controller-options"></a>Opzioni controller di dominio  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptions.png)  
  
La pagina **Opzioni controller di dominio** specifica le funzionalità per il nuovo controller di dominio. Le funzionalità configurabili per il controller di dominio sono **Server DNS**, **Catalogo globale** e **Controller di dominio di sola lettura**. È consigliabile che tutti i controller di dominio forniscano servizi DNS e di catalogo globale per garantire un'elevata disponibilità negli ambienti distribuiti. Il catalogo globale è sempre selezionato per impostazione predefinita e il server DNS è selezionato per impostazione predefinita se il dominio corrente ospita già DNS sui controller di dominio in base alla query Origine di autorità. Nella pagina **Opzioni controller di dominio** è anche possibile scegliere dalla configurazione della foresta un **nome del sito** di Active Directory logico e appropriato. Per impostazione predefinita, viene selezionato il sito con la subnet più corretta. Se è presente un solo sito, viene selezionato automaticamente.  
  
> [!NOTE]  
> Se il server non fa parte di una subnet di Active Directory ed è presente più di un sito di Active Directory, non viene selezionato nulla e il pulsante **Avanti** non sarà disponibile finché non viene scelto un sito dall'elenco.  
  
La **Password modalità ripristino servizi directory** specificata deve soddisfare i criteri password applicati al server. Scegliere sempre una password complessa e di difficile individuazione o preferibilmente una passphrase.  
  
Gli argomenti ADDSDeployment per **Opzioni controller di dominio** sono:  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-sitename <string>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> Il nome del sito deve esistere già quando viene fornito come argomento di **-sitename**. Il cmdlet **install-AddsDomainController** non crea i siti. È possibile usare il cmdlet **new-adreplicationsite** per creare nuovi siti.  
  
L'operazione dell'argomento **SafeModeAdministratorPassword** è particolare:  
  
-   Se *non viene specificato* come argomento, il cmdlet richiede di inserire e confermare una password nascosta. Questo è il metodo di utilizzo consigliabile quando il cmdlet viene eseguito in modo interattivo.  
  
    Ad esempio, per creare un altro controller di dominio nel dominio treyresearch.net e ricevere la richiesta di immettere e confermare una password nascosta:  
  
    ```  
    Install-ADDSDomainController "DomainName treyresearch.net "credential (get-credential)  
    ```  
  
-   Se viene indicato *con un valore*, tale valore deve essere una stringa sicura. Questo non è il metodo di utilizzo consigliabile quando il cmdlet viene eseguito in modo interattivo.  
  
Ad esempio, è possibile utilizzare il cmdlet **Read-Host** per richiedere all'utente una stringa sicura:  
  
```  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> Poiché l'opzione precedente non chiede conferma della password, utilizzarla con estrema cautela, in quanto la password non è visibile.  
  
È inoltre possibile specificare una stringa sicura come variabile di testo in chiaro convertita, anche se questo metodo è sconsigliato.  
  
```  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
  
```  
  
Infine, è possibile archiviare la password offuscata in un file e quindi riutilizzarla in seguito, senza visualizzare mai la password non crittografata. Ad esempio,  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> È sconsigliabile inserire o archiviare una password non crittografata o di testo offuscato. In questo modo, la password per la modalità ripristino servizi directory per il controller di dominio diverrebbe nota a chiunque esegua questo comando in uno script o riesca a leggerla dallo schermo dell'utente.  Tutti gli utenti con accesso al file possono invertire la password offuscata. Conoscendo la password sarebbe quindi possibile accedere a un controller di dominio avviato in modalità ripristino servizi directory e infine fingersi il controller di dominio, innalzando i propri privilegi al massimo livello in una foresta Active Directory. Esiste un altro set di passaggi consigliati che prevedono l'uso di **System.Security.Cryptography** per crittografare i dati dei file di testo, ma che esulano dall'ambito di questo articolo. La procedura consigliata è di evitare completamente l'archiviazione delle password.  
  
Il cmdlet ADDSDeployment offre un'altra opzione per ignorare la configurazione automatica di impostazioni del client DNS, server d'inoltro e parametri radice. Non è possibile ignorare questa opzione di configurazione quando si usa Server Manager. Questo argomento è importante solo se il ruolo del server DNS è stato installato prima di configurare il controller di dominio:  
  
```  
-SkipAutoConfigureDNS  
```  
  
La pagina **Opzioni controller di dominio** avvisa che non è possibile creare controller di dominio di sola lettura se i controller di dominio esistenti eseguono Windows Server 2003. Questo comportamento è previsto ed è possibile ignorare l'avviso.  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptionsError.png)  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>Opzioni DNS e credenziali di delega DNS  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDNSOptions.png)  
  
La pagina **Opzioni DNS** consente di configurare la delega DNS se è stata selezionata l'opzione **Server DNS** nella pagina *Opzioni controller di dominio* e se si fa riferimento a una zona in cui le deleghe DNS sono consentite. Potrebbe essere necessario specificare credenziali alternative di un utente membro del gruppo **DNS Admins** .  
  
Gli argomenti del cmdlet di ADDSDeployment per **Opzioni DNS** sono:  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
Per altre informazioni sulla necessità di creare una delega DNS, vedere [Informazioni sulla delega delle zone](https://technet.microsoft.com/library/cc771640.aspx).  
  
### <a name="additional-options"></a>Opzioni aggiuntive  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeAdditionalOptions.png)  
  
Nella pagina **Opzioni aggiuntive** è disponibile l'opzione di configurazione per denominare un controller di dominio come origine della replica o per usare qualsiasi controller di dominio come origine della replica.  
  
È inoltre possibile decidere di installare il controller di dominio usando i supporti di backup tramite l'opzione Installazione da supporto. Selezionando la casella di controllo **Installazione da supporto** , viene visualizzata un'opzione di selezione ed è necessario fare clic su **Verifica** per assicurarsi che il percorso specificato sia un supporto valido. I supporti compatibili con l'opzione Installazione da supporto vengono creati con Windows Server Backup o Ntdsutil.exe esclusivamente da un altro computer Windows Server 2012; non è possibile utilizzare Windows Server 2008 R2 o un sistema operativo precedente per creare un supporto per un controller di dominio Windows Server 2012. Per altre informazioni sulle modifiche apportate all'installazione da supporto, vedere [Appendice sull'amministrazione semplificata](../../ad-ds/deploy/Simplified-Administration-Appendix.md). Se si usa un supporto protetto da SYSKEY, Server Manager richiede la password dell'immagine durante la verifica.  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_NtdsutilIFM.png)  
  
Gli argomenti del cmdlet di ADDSDeployment per **Opzioni aggiuntive** sono:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-syskey <secure string>  
```  
  
### <a name="paths"></a>Percorsi  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
Nella pagina **Percorsi** è possibile sostituire i percorsi predefiniti delle cartelle per il database di Servizi di dominio Active Directory, i registri delle transazioni del database e la condivisione SYSVOL. Le posizioni predefinite sono sempre in sottodirectory di %systemroot%.  
  
Gli argomenti del cmdlet di ADDSDeployment per Percorsi di Active Directory sono:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>Opzioni di preparazione  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptions.png)  
  
La pagina **Opzioni di preparazione** avvisa che la configurazione di Servizi di dominio Active Directory include l'estensione dello schema (forestprep) e l'aggiornamento del dominio (domainprep).  Questa pagina viene visualizzata solo quando la foresta e il dominio non sono stati preparati da un'installazione precedente del controller di dominio Windows Server 2012 o dall'esecuzione manuale di Adprep.exe. La Configurazione guidata Servizi di dominio Active Directory, ad esempio, elimina questa pagina se si aggiunge un nuovo controller di dominio a un dominio radice della foresta Windows Server 2012 esistente.  
  
L'estensione dello schema e l'aggiornamento del dominio non vengono eseguiti quando si fa clic su **Avanti**. Questi eventi si verificano solo durante la fase di installazione. Questa pagina elenca semplicemente gli eventi che si verificheranno in seguito durante l'installazione.  
  
Questa pagina inoltre convalida che le credenziali utente correnti siano membri dei gruppi Schema Admins ed Enterprise Admins, perché l'appartenenza a questi gruppi è necessaria per estendere lo schema o preparare un dominio. Fare clic su **Cambia** per specificare le credenziali utente adeguate se la pagina informa che le credenziali correnti non forniscono autorizzazioni sufficienti.  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptionsCreds.png)  
  
L'argomento del cmdlet di ADDSDeployment per Opzioni aggiuntive è:  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> Come con le versioni precedenti di Windows Server, la preparazione del dominio automatizzata per i controller di dominio che eseguono Windows Server 2012 non esegue GPPREP. Eseguire **adprep.exe /gpprep** manualmente per tutti i domini non preparati in precedenza per Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2. È consigliabile eseguire GPPrep una sola volta nella cronologia di un dominio e non a ogni aggiornamento. Adprep.exe non esegue automaticamente run /gpprep perché l'operazione può causare una nuova replica di tutti i file e le cartelle nella cartella SYSVOL su tutti i controller di dominio.  
>   
> RODCPrep viene eseguito automaticamente quando si innalza di livello il primo controller di dominio di sola lettura senza installazione di appoggio in un dominio. Non si verifica quando si innalza di livello il primo controller di dominio di Windows Server 2012 scrivibile. È possibile eseguire manualmente **adprep.exe /rodcprep** anche se si prevede di distribuire controller di dominio di sola lettura.  
  
### <a name="review-options-and-view-script"></a>Verifica opzioni e Visualizza script  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeReviewOptions.png)  
  
Nella pagina **Verifica opzioni** è possibile convalidare le impostazioni e accertarsi se soddisfano i requisiti prima di iniziare l'installazione. Questa non è l'ultima possibilità per interrompere l'installazione utilizzando Server Manager. La pagina consente semplicemente di rivedere e confermare le impostazioni prima di proseguire con la configurazione.  
  
La pagina **Verifica opzioni** di Server Manager include inoltre un pulsante opzionale **Visualizza script** , che consente di creare un file di testo Unicode contenente la configurazione ADDSDeployment corrente come singolo script di Windows PowerShell. In questo modo è possibile utilizzare l'interfaccia grafica di Server Manager come strumento di distribuzione di Windows PowerShell. Utilizzare la Configurazione guidata Servizi di dominio Active Directory per configurare le opzioni, esportare la configurazione e annullare la procedura guidata.  Questo processo crea un esempio valido e sintatticamente corretto che può essere utilizzato direttamente o successivamente modificato.  
  
Ad esempio,  
  
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
> Server Manager completa in genere tutti gli argomenti con i valori durante l'innalzamento di livello e non usa le impostazioni predefinite perché queste potrebbero cambiare nelle future versioni di Windows o nei Service Pack. La sola eccezione è l'argomento **-safemodeadministratorpassword**. Per forzare un prompt di conferma, omettere il valore quando si esegue il cmdlet in modo interattivo.  
>   
> Usare l'argomento facoltativo **Whatif** con il cmdlet **Install-ADDSDomainController** per rivedere le informazioni sulla configurazione. In questo modo è possibile visualizzare i valori espliciti e impliciti degli argomenti per un cmdlet.  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSWhatIf.png)  
  
### <a name="prerequisites-check"></a>Controllo dei prerequisiti  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrereqCheck.png)  
  
**Controllo dei prerequisiti** è una nuova funzionalità nella configurazione del dominio di Servizi di dominio Active Directory. Questa nuova fase convalida che il dominio e la foresta sono in grado di supportare un nuovo controller di dominio di Windows Server 2012.  
  
Quando si installa un nuovo controller di dominio, la Configurazione guidata Servizi di dominio Active Directory di Server Manager richiama una serie di test modulari serializzati. Questi test avvisano l'utente con le opzioni di ripristino suggerite. È possibile eseguire i test ogni volta che è necessario. Il processo del controller di dominio non può continuare finché tutti i test sui prerequisiti non vengono superati.  
  
**Controllo dei prerequisiti** copre anche le informazioni pertinenti, ad esempio le modifiche alla sicurezza, che interessano i sistemi operativi precedenti.  
  
Per altre informazioni sui controlli dei prerequisiti specifici, vedere [Controllo dei prerequisiti](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Non è possibile ignorare il **Controllo prerequisiti** quando si usa Server Manager, ma è possibile saltare il processo quando si usa il cmdlet di distribuzione di Servizi di dominio Active Directory con l'argomento seguente:  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft sconsiglia di ignorare il controllo dei prerequisiti perché l'innalzamento di livello del controller di dominio potrebbe essere solo parziale o la foresta Servizi di dominio Active Directory potrebbe risultare danneggiata.  
  
Fare clic su **Installa** per iniziare il processo di innalzamento di livello del controller di dominio. È l'ultima opportunità per annullare l'installazione. Non è possibile annullare il processo di innalzamento di livello una volta iniziato. Il computer verrà riavviato automaticamente al termine dell'innalzamento di livello, indipendentemente dai risultati di questa operazione. Nella pagina **Controllo dei prerequisiti** sono visualizzati tutti i problemi incontrati durante il processo e le indicazioni per risolverli.  
  
### <a name="installation"></a>Installazione  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeInstallProgress.png)  
  
Quando la pagina **Installazione** viene visualizzata, la configurazione del controller di dominio inizia e non può essere interrotta o annullata. I dettagli delle operazioni vengono visualizzati in questa pagina e scritti nei log:  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
-   %systemroot%\debug\adprep\logs  
  
-   %systemroot%\debug\netsetup.log (se il server è in un gruppo di lavoro)  
  
Per installare una nuova foresta Active Directory con il modulo ADDSDeployment, usare il cmdlet seguente:  
  
```  
Install-addsdomaincontroller  
```  
  
Per gli argomenti obbligatori e facoltativi, vedere [Aggiornamento e replica in Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS).  
  
Il cmdlet **Install-AddsDomainController** ha solo due fasi (controllo dei prerequisiti e installazione). Nelle due figure seguenti è illustrata la fase di installazione con gli argomenti minimi obbligatori di **-domainname**, **-credential**. Si noti che l'operazione Adprep viene eseguita automaticamente durante l'aggiunta del primo controller di dominio di Windows Server 2012 a una foresta di Windows Server 2003 esistente:  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSGetCred.png)  
  
Si noti che, esattamente come Server Manager, **Install-ADDSDomainController** ricorda che l'innalzamento di livello riavvierà automaticamente il server. Per accettare automaticamente il prompt di riavvio, usare gli argomenti **-force** o **-confirm:$false** con un cmdlet ADDSDeployment di Windows PowerShell. Per evitare il riavvio automatico del server al termine dell'innalzamento di livello, usare l'argomento **-norebootoncompletion**.  
  
> [!WARNING]  
> Si sconsiglia di eseguire l'override del riavvio. Il controller di dominio deve essere riavviato per funzionare correttamente.  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeConfirm.gif)  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeProgress.png)  
  
Per configurare un controller di dominio in modalità remota con Windows PowerShell, eseguire il wrapping del cmdlet **Install-AddsDomainController** *all'interno* del cmdlet **Invoke-Command** . È necessario usare le parentesi graffe.  
  
```  
invoke-command {install-addsdomaincontroller "domainname <domain> -credential (get-credential)} -computername <dc name>  
```  
  
Ad esempio,  
  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeExample.gif)  
  
> [!NOTE]  
> Per altre informazioni sul funzionamento sul processo di installazione e Adprep, vedere [Risoluzione dei problemi relativi alla distribuzione di controller di dominio](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).  
  
### <a name="results"></a>Risultati  
![Installare una replica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
La pagina **Risultati** mostra l'esito positivo o negativo dell'innalzamento di livello e le informazioni amministrative importanti. In caso di esito positivo, il controller di dominio verrà automaticamente riavviato dopo 10 secondi.  
  
Come con le versioni precedenti di Windows Server, la preparazione del dominio automatizzata per i controller di dominio che eseguono Windows Server 2012 non esegue GPPREP. Eseguire **adprep.exe /gpprep** manualmente per tutti i domini non preparati in precedenza per Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2. È consigliabile eseguire GPPrep una sola volta nella cronologia di un dominio e non a ogni aggiornamento. Adprep.exe non esegue automaticamente run /gpprep perché l'operazione può causare una nuova replica di tutti i file e le cartelle nella cartella SYSVOL su tutti i controller di dominio.  
  

