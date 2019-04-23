---
ms.assetid: e3d55565-ad45-4504-ad73-8103d1a92170
title: Installare un nuovo dominio albero o un elemento figlio di Active Directory di Windows Server 2012 (livello 200)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7292f76155c2bcb47b6c632b969f54f3afb93d50
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853702"
---
# <a name="install-a-new-windows-server-2012-active-directory-child-or-tree-domain-level-200"></a>Installare un nuovo dominio albero o un elemento figlio di Active Directory di Windows Server 2012 (livello 200)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene illustrato come aggiungere domini figlio e albero a una foresta di Windows Server 2012 esistente, con Server Manager o Windows PowerShell.  
  
-   [Elemento figlio e flusso di lavoro dominio albero](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [Elemento figlio e albero Domain Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)  
  
-   [Distribuzione](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Deployment)  
  
## <a name="BKMK_Workflow"></a>Elemento figlio e flusso di lavoro dominio albero  
Il diagramma seguente illustra il processo di configurazione di Servizi di dominio Active Directory, quando in precedenza è stato installato il ruolo Servizi di dominio Active Directory ed è stata avviata la Configurazione guidata Servizi di dominio Active Directory mediante Server Manager per creare un nuovo dominio in una foresta esistente.  
  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/adds_childtreedeploy_beta1.png)  
  
## <a name="BKMK_PS"></a>Elemento figlio e albero Domain Windows PowerShell  
  
|||  
|-|-|  
|**Cmdlet di ADDSDeployment**|Argomenti. Gli argomenti in **grassetto** sono obbligatori. Gli argomenti in *corsivo* possono essere specificati usando Windows PowerShell o la Configurazione guidata Servizi di dominio Active Directory.|  
|**Install-AddsDomain**|-SkipPreChecks<br /><br />***-NewDomainName***<br /><br />***-ParentDomainName***<br /><br />***-SafeModeAdministratorPassword***<br /><br />*-ADPrepCredential*<br /><br />-AllowDomainReinstall<br /><br />-Confirm<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-NoDNSOnNetwork<br /><br />*-DomainMode*<br /><br />***-DomainType***<br /><br />-Force<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />*-NewDomainNetBIOSName*<br /><br />*-NoGlobalCatalog*<br /><br />-NoNorebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />*-SiteName*<br /><br />-SkipAutoConfigureDNS<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> L'argomento **-credential** è obbligatorio solo se attualmente non si è connessi come membro del gruppo Enterprise Admins. L'argomento **-NewDomainNetBIOSName** è obbligatorio solo se si vuole cambiare il nome di 15 caratteri generato automaticamente in base al prefisso del nome di dominio DNS o se il nome supera i 15 caratteri.  
  
## <a name="BKMK_Deployment"></a>Distribuzione  
  
### <a name="deployment-configuration"></a>Configurazione distribuzione  
Nella cattura di schermata seguente vengono visualizzate le opzioni per aggiungere un dominio figlio:  
  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDeployConfig.png)  
  
Nella cattura di schermata seguente vengono visualizzate le opzioni per aggiungere un dominio albero:  
  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_TreeDeployConfig.png)  
  
Server Manager inizia l'innalzamento di livello di ogni controller di dominio nella pagina **Configurazione distribuzione** . Le opzioni restanti e i campi obbligatori in questa pagina e nella pagine successive sono diversi a seconda dell'operazione di distribuzione selezionata.  
  
In questo argomento vengono combinate due operazioni diverse: innalzamento di livello di un dominio figlio e innalzamento di livello di un dominio albero. La sola differenza tra le due operazioni è il tipo di dominio che si sceglie di creare. Tutti gli altri passaggi sono identici per entrambe le operazioni.  
  
-   Per creare un nuovo dominio figlio, fare clic su **Aggiungi un nuovo dominio a una foresta esistente** e scegliere **Dominio figlio**. Per **Nome dominio padre**, digitare o selezionare il nome del dominio padre. A questo punto digitare il nome del nuovo dominio nella casella **Nuovo nome dominio**. Specificare un nome di dominio figlio con etichetta singola valido. Il nome deve rispettare i requisiti DNS per i nomi di dominio.  
  
-   Per creare un dominio albero in una foresta esistente, fare clic su **Aggiungi un nuovo dominio a una foresta esistente** e scegliere **Dominio albero**. Digitare il nome del dominio radice della foresta e quindi digitare il nome del nuovo dominio. Specificare un nome valido e completo per il dominio radice. Il nome non può avere un'etichetta singola e deve rispettare i requisiti DNS per i nomi di dominio.  
  
Per altre informazioni sui nomi DNS, vedi [Convenzioni di denominazione in Active Directory per computer, domini, siti e unità organizzative](https://support.microsoft.com/kb/909264).  
  
La Configurazione guidata Servizi di dominio Active Directory di Server Manager richiede l'immissione delle credenziali per il dominio se le proprie attuali credenziali non appartengono al dominio. Fare clic su **Cambia** per fornire le credenziali per il dominio per l'operazione di innalzamento di livello.  
  
Il cmdlet di ADDSDeployment per Configurazione distribuzione e gli argomenti sono:  
  
```  
Install-AddsDomain  
-domaintype <{childdomain | treedomain}>  
-parentdomainname <string>  
-newdomainname <string>  
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Opzioni controller di dominio  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_DCOptions_Child.gif)  
  
La pagina **Opzioni controller di dominio** specifica le opzioni per il nuovo controller di dominio. Le opzioni configurabili per il controller di dominio includono **Server DNS** e **Catalogo globale**. Non è possibile configurare un controller di dominio di sola lettura come primo controller in un nuovo dominio.  
  
È consigliabile che tutti i controller di dominio forniscano servizi DNS e di catalogo globale per garantire un'elevata disponibilità negli ambienti distribuiti. Il catalogo globale è sempre selezionato per impostazione predefinita e DNS è selezionato per impostazione predefinita se il dominio corrente ospita già DNS sui controller di dominio in base a una query Origine di autorità. È necessario specificare anche un **Livello di funzionalità dominio**. Il livello di funzionalità dominio predefinito è Windows Server 2012 ed è possibile scegliere qualsiasi altro valore uguale o superiore al livello di funzionalità foresta corrente.  
  
Nella pagina **Opzioni controller di dominio** è anche possibile scegliere dalla configurazione della foresta un **nome del sito** di Active Directory logico e appropriato. Per impostazione predefinita, viene selezionato il sito con la subnet più corretta. Se è presente un solo sito, viene selezionato automaticamente.  
  
> [!IMPORTANT]  
> Se il server non fa parte di una subnet di Active Directory ed è presente più di un sito di Active Directory, non viene selezionato nulla e il pulsante **Avanti** non sarà disponibile finché non viene scelto un sito dall'elenco.  
  
La **Password modalità ripristino servizi directory** specificata deve soddisfare i criteri password applicati al server. Scegliere sempre una password complessa e di difficile individuazione o preferibilmente una passphrase.  
  
Gli argomenti del cmdlet ADDSDeployment per **Opzioni controller di dominio** sono:  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-Sitename <string>  
-SafeModeAdministratorPassword <secure string>  
-Credential <pscredential>  
```  
  
> [!IMPORTANT]  
> Il nome del sito deve esistere già quando viene fornito come valore per l'argomento di **sitename** . Il cmdlet **install-AddsDomainController** non crea i nomi dei siti. È possibile usare il cmdlet **new-adreplicationsite** per creare nuovi siti.  
  
Gli argomenti del cmdlet **Install-ADDSDomainController**, se non specificati, seguono le stesse impostazioni predefinite di Server Manager.  
  
L'operazione dell'argomento **SafeModeAdministratorPassword** è particolare:  
  
-   Se *non viene specificato* come argomento, il cmdlet richiede di inserire e confermare una password nascosta. Questo è il metodo di utilizzo consigliabile quando il cmdlet viene eseguito in modo interattivo.  
  
    Ad esempio, per creare un nuovo dominio figlio denominato NorthAmerica nella foresta Contoso.com e ricevere la richiesta di immettere e confermare una password nascosta:  
  
    ```  
    Install-ADDSDomain "NewDomainName NorthAmerica "ParentDomainName Contoso.com "DomainType Child  
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
  
Infine, è possibile archiviare la password offuscata in un file e quindi riutilizzarla in seguito, senza visualizzare mai la password non crittografata. Ad esempio:   
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> È sconsigliabile inserire o archiviare una password non crittografata o di testo offuscato. In questo modo, la password per la modalità ripristino servizi directory per il controller di dominio diverrebbe nota a chiunque esegua questo comando in uno script o riesca a leggerla dallo schermo dell'utente.  Tutti gli utenti con accesso al file possono invertire la password offuscata. Conoscendo la password sarebbe quindi possibile accedere a un controller di dominio avviato in modalità ripristino servizi directory e infine fingersi il controller di dominio, innalzando i propri privilegi al massimo livello in una foresta Active Directory. Esiste un altro set di passaggi consigliati che prevedono l'uso di **System.Security.Cryptography** per crittografare i dati dei file di testo, ma che esulano dall'ambito di questo articolo. La procedura consigliata è di evitare completamente l'archiviazione delle password.  
  
Il modulo ADDSDeployment offre un'altra opzione per ignorare la configurazione automatica di impostazioni del client DNS, server d'inoltro e parametri radice. Non è configurabile quando si usa Server Manager. Questo argomento è importante solo se il servizio del server DNS è già stato installato prima di configurare il controller di dominio:  
  
```  
-SkipAutoConfigureDNS  
  
```  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>Opzioni DNS e credenziali di delega DNS  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDNSOptions.png)  
  
La pagina **Opzioni DNS** consente di specificare credenziali amministrative DNS alternative per la delega.  
  
Quando si installa un nuovo dominio in una foresta esistente, in cui è stata selezionata l'installazione DNS nella pagina **Opzioni controller di dominio**, non è possibile configurare alcuna opzione. La delega viene eseguita automaticamente e irrevocabilmente. È possibile specificare credenziali amministrative DNS alternative con i diritti per aggiornare la struttura.  
  
Gli argomenti ADDSDeployment di Windows PowerShell per **Opzioni DNS** sono:  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
Per altre informazioni sulla delega DNS, vedi [Informazioni sulla delega delle zone](https://technet.microsoft.com/library/cc771640.aspx).  
  
### <a name="additional-options"></a>Opzioni aggiuntive  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildAdditionalOptions.png)  
  
La pagina **Opzioni aggiuntive** mostra il nome NetBIOS del dominio e consente di sostituirlo. Per impostazione predefinita, il nome di dominio NetBIOS corrisponde all'etichetta a sinistra del nome di dominio completo specificato nella pagina **Configurazione distribuzione**. Se, ad esempio, è stato specificato il nome di dominio completo corp.contoso.com, il nome di dominio NetBIOS predefinito è CORP.  
  
Se il nome non contiene più di 15 caratteri e non è in conflitto con un altro nome NetBIOS, non viene modificato. Se è in conflitto con un altro nome NetBIOS, al nome viene aggiunto un numero. Se il nome contiene più di 15 caratteri, la procedura guidata propone un suggerimento univoco troncato. In entrambi i casi, la procedura guidata convalida prima il nome che non è già in uso tramite una ricerca WINS e una trasmissione NetBIOS.  
  
Per altre informazioni sui nomi DNS, vedi [Convenzioni di denominazione in Active Directory per computer, domini, siti e unità organizzative](https://support.microsoft.com/kb/909264).  
  
Gli argomenti **Install-AddsDomain** , se non specificati, seguono le stesse impostazioni predefinite di Server Manager. L'operazione **DomainNetBIOSName** è particolare:  
  
1.  Se l'argomento **NewDomainNetBIOSName** non è specificato con un nome di dominio NetBIOS e il nome di dominio con prefisso con etichetta singola nell'argomento **DomainName** è di 15 caratteri o meno, l'innalzamento di livello continua con un nome generato automaticamente.  
  
2.  Se l'argomento **NewDomainNetBIOSName** non è specificato con un nome di dominio NetBIOS e il nome di dominio con prefisso con etichetta singola nell'argomento **DomainName** è di 16 caratteri o più, l'innalzamento di livello ha esito negativo.  
  
3.  Se l'argomento **NewDomainNetBIOSName** è specificato con un nome di dominio NetBIOS di 15 caratteri o meno, l'innalzamento di livello continua con il nome specificato.  
  
4.  Se l'argomento **NewDomainNetBIOSName** è specificato con un nome di dominio NetBIOS di 16 caratteri o più, l'innalzamento di livello ha esito negativo.  
  
L'argomento del cmdlet di ADDSDeployment per **Opzioni aggiuntive** è:  
  
```  
-newdomainnetbiosname <string>  
```  
  
### <a name="paths"></a>Percorsi  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
Nella pagina **Percorsi** è possibile sostituire i percorsi predefiniti delle cartelle per il database di Servizi di dominio Active Directory, i registri delle transazioni del database e la condivisione SYSVOL. Le posizioni predefinite sono sempre in sottodirectory di %systemroot%.  
  
Gli argomenti del cmdlet di ADDSDeployment per **Percorsi** sono:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>Verifica opzioni e Visualizza script  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildReviewOptions.png)  
  
Nella pagina **Verifica opzioni** è possibile convalidare le impostazioni e accertarsi se soddisfano i requisiti prima di iniziare l'installazione. Questa non è l'ultima possibilità per interrompere l'installazione quando si utilizza Server Manager. È semplicemente un'opzione per confermare le impostazioni prima di proseguire con la configurazione.  
  
La pagina **Verifica opzioni** di Server Manager include inoltre un pulsante opzionale **Visualizza script** , che consente di creare un file di testo Unicode contenente la configurazione ADDSDeployment corrente come singolo script di Windows PowerShell. In questo modo è possibile utilizzare l'interfaccia grafica di Server Manager come strumento di distribuzione di Windows PowerShell. Utilizzare la Configurazione guidata Servizi di dominio Active Directory per configurare le opzioni, esportare la configurazione e annullare la procedura guidata.  Questo processo crea un esempio valido e sintatticamente corretto che può essere utilizzato direttamente o successivamente modificato. Ad esempio:   
  
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
> Server Manager completa in genere tutti gli argomenti con i valori durante l'innalzamento di livello e non usa le impostazioni predefinite perché queste potrebbero cambiare nelle future versioni di Windows o nei Service Pack. La sola eccezione è l'argomento **-safemodeadministratorpassword**, che viene deliberatamente omesso dallo script. Per forzare un prompt di conferma, omettere il valore quando si esegue il cmdlet in modo interattivo.  
  
Usare l'argomento facoltativo **Whatif** con il cmdlet **Install-ADDSForest** per rivedere le informazioni sulla configurazione. In questo modo è possibile visualizzare i valori espliciti e impliciti degli argomenti per un cmdlet.  
  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildWhatIf.png)  
  
### <a name="prerequisites-check"></a>Controllo dei prerequisiti  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildPrereqCheck.png)  
  
**Controllo dei prerequisiti** è una nuova funzionalità nella configurazione del dominio di Servizi di dominio Active Directory. Questa nuova fase convalida che la configurazione server sia in grado di supportare un nuovo dominio Servizi di dominio Active Directory.  
  
Quando si installa un nuovo dominio radice della foresta, la Configurazione guidata Servizi di dominio Active Directory di Server Manager richiama una serie di test modulari serializzati. Questi test avvisano l'utente con le opzioni di ripristino suggerite. È possibile eseguire i test ogni volta che è necessario. Il processo del controller di dominio non può continuare finché tutti i test sui prerequisiti non vengono superati.  
  
**Controllo dei prerequisiti** copre anche le informazioni pertinenti, ad esempio le modifiche alla sicurezza, che interessano i sistemi operativi precedenti.  
  
Per ulteriori informazioni sui controlli dei prerequisiti specifici, vedere [controllo dei prerequisiti](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Non è possibile ignorare il **Controllo prerequisiti** quando si usa Server Manager, ma è possibile saltare il processo quando si usa il cmdlet di distribuzione di Servizi di dominio Active Directory con l'argomento seguente:  
  
```  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft sconsiglia di ignorare il controllo dei prerequisiti perché l'innalzamento di livello del controller di dominio potrebbe essere solo parziale o la foresta Servizi di dominio Active Directory potrebbe risultare danneggiata.  
  
Fare clic su **Installa** per iniziare il processo di innalzamento di livello del controller di dominio. È l'ultima opportunità per annullare l'installazione. Non è possibile annullare il processo di innalzamento di livello una volta iniziato. Al termine dell'innalzamento di livello il computer verrà riavviato automaticamente, indipendentemente dai risultati.  
  
### <a name="installation"></a>Installazione  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildInstallation.png)  
  
Quando la pagina **Installazione** viene visualizzata, la configurazione del controller di dominio inizia e non può essere interrotta o annullata. I dettagli delle operazioni vengono visualizzati in questa pagina e scritti nei log:  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Per installare un nuovo dominio Active Directory con il modulo ADDSDeployment, usare il cmdlet seguente:  
  
```  
Install-addsdomain  
```  
  
Per gli argomenti obbligatori e facoltativi, vedere [Windows PowerShell per domini figlio e domini albero](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS). Il cmdlet **Install-addsdomain** ha solo due fasi (controllo dei prerequisiti e installazione). Nelle due figure seguenti è illustrata la fase di installazione con gli argomenti minimi obbligatori di **-domaintype**, **-newdomainname**, **-parentdomainname** e **-credential**. Si noti che, esattamente come Server Manager, **Install-ADDSDomain** ricorda che l'innalzamento di livello riavvierà automaticamente il server.  
  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomain.png)  
  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomainProgress.png)  
  
Per accettare automaticamente il prompt di riavvio, usare gli argomenti **-force** o **-confirm:$false** con un cmdlet ADDSDeployment di Windows PowerShell. Per evitare il riavvio automatico del server al termine dell'innalzamento di livello, usare l'argomento **-norebootoncompletion**.  
  
> [!WARNING]  
> Si sconsiglia di eseguire l'override del riavvio. Il controller di dominio deve essere riavviato per funzionare correttamente.  
  
### <a name="results"></a>Risultati  
![Installare un nuovo figlio di Active Directory](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
La pagina **Risultati** mostra l'esito positivo o negativo dell'innalzamento di livello e le informazioni amministrative importanti. Il controller di dominio verrà automaticamente riavviato dopo 10 secondi.  
  

