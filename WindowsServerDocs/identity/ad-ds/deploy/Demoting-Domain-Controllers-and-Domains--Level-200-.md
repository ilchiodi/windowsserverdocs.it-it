---
ms.assetid: 65ed5956-6140-4e06-8d99-8771553637d1
title: L'abbassamento di livello controller di dominio e domini (livello 200)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c254a01da5534c1ddc673bc1e60382c166ddeda7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="demoting-domain-controllers-and-domains-level-200"></a>L'abbassamento di livello controller di dominio e domini (livello 200)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene illustrato come rimuovere di dominio Active Directory utilizzando Server Manager o Windows PowerShell.  
  
-   [Flusso di lavoro per la rimozione di AD DS](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_Workflow)  
  
-   [Abbassamento di livello e rimozione di ruoli Windows PowerShell](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_PS)  
  
-   [Abbassare di livello](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_Demote)  
  
## <a name="BKMK_Workflow"></a>Flusso di lavoro per la rimozione di AD DS  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/adds_demotedomainforest.png)  
  
> [!CAUTION]  
> Rimuovere i ruoli di servizi di dominio Active Directory con Dism.exe o con il modulo di manutenzione di Windows PowerShell dopo l'innalzamento di livello a Controller di dominio non è supportata e impedirà il server venga avviato normalmente.  
>   
> A differenza di Server Manager o il modulo ADDSDeployment per Windows PowerShell, manutenzione è un sistema di manutenzione nativo con senza informazioni inerenti di dominio Active Directory o la relativa configurazione. Non usare Dism.exe o il modulo di manutenzione di Windows PowerShell per disinstallare il ruolo di dominio Active Directory, a meno che il server non è più un controller di dominio.  
  
## <a name="BKMK_PS"></a>Abbassamento di livello e rimozione di ruoli Windows PowerShell  
  
|||  
|-|-|  
|**Cmdlet ADDSDeployment e ServerManager**|Gli argomenti (**grassetto** sono necessari argomenti. *Corsivo* argomenti possono essere specificati usando Windows PowerShell o la configurazione guidata di Active Directory.)|  
|Uninstall-AddsDomainController|-SkipPreChecks<br /><br />*-LocalAdministratorPassword*<br /><br />-Confirm<br /><br />***-Credential***<br /><br />-DemoteOperationMasterRole<br /><br />*-DNSDelegationRemovalCredential*<br /><br />-Force<br /><br />*-ForceRemoval*<br /><br />*-IgnoreLastDCInDomainMismatch*<br /><br />*-IgnoreLastDNSServerForZone*<br /><br />*-LastDomainControllerInDomain*<br /><br />-Norebootoncompletion<br /><br />*-RemoveApplicationPartitions*<br /><br />*-RemoveDNSDelegation*<br /><br />-RetainDCMetadata|  
|Disinstallare-WindowsFeature o Remove-WindowsFeature|***-Name***<br /><br />***-IncludeManagementTools***<br /><br />*-Riavviare*<br /><br />-Remove<br /><br />-Force<br /><br />-ComputerName<br /><br />-Credential<br /><br />-LogPath<br /><br />-Vhd|  
  
> [!NOTE]  
> Il **-credential** argomento è solo necessario se si è non già connessi come membro del gruppo Enterprise Admins (abbassamento di livello ultimo controller di dominio in un dominio) o il gruppo Domain Admins (abbassamento di livello una controller di dominio DC).The **- includemanagementtools** argomento è solo necessario se si desidera rimuovere tutte le utilità di gestione di dominio Active Directory.  
  
## <a name="BKMK_Demote"></a>Abbassare di livello  
  
### <a name="remove-roles-and-features"></a>Rimuovere ruoli e funzionalità  
Server Manager offre due interfacce per la rimozione del ruolo Servizi di dominio Active Directory:  
  
-   Il **Gestisci** menu nel dashboard principale, utilizzando **Rimuovi ruoli e funzionalità**  
  
 ![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Manage.png)  
  
-   Fare clic su **di dominio Active Directory** o **tutti i server** nel riquadro di spostamento. Scorrere verso il basso il **ruoli e funzionalità** sezione. Fare doppio clic su **servizi di dominio Active Directory** nel **ruoli e funzionalità** elenco e fare clic su **Rimuovi ruolo o funzionalità **. Questa interfaccia Salta la **selezione dei Server** pagina.  
  
 ![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection.png)  
  
I cmdlet ServerManager **Uninstall-WindowsFeature** e **Remove-WindowsFeature** impediscono di rimuovere il ruolo di dominio Active Directory fino a quando non si abbassa di livello il controller di dominio.  
  
### <a name="server-selection"></a>Selezione dei server  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection2.png)  
  
Il **selezione dei Server** finestra di dialogo consente di scegliere uno dei server aggiunti in precedenza al pool, purché sia accessibile. Il server locale che esegue Server Manager è sempre automaticamente disponibile.  
  
### <a name="server-roles-and-features"></a>Funzionalità e ruoli del server  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerRoles.png)  
  
Cancella il **servizi di dominio Active Directory** casella di controllo per abbassare di livello un controller di dominio. Se il server è attualmente un controller di dominio, questo non rimuove il ruolo di dominio Active Directory e quindi ma si passa a un **risultati della convalida** finestra di dialogo con l'offerta per abbassare di livello. In caso contrario, semplicemente rimuove i file binari come qualsiasi altra funzionalità del ruolo.  
  
-   Non rimuovere eventuali altri AD DS-ruolo o funzionalità relativa -, ad esempio DNS, Gestione criteri di gruppo o gli strumenti di amministrazione remota del server - se si desidera promuovere rieseguirà subito il controller di dominio. Rimozione di altri ruoli e funzionalità aumenta il tempo di nuovo innalzamento di livello, come Server Manager Reinstalla queste funzionalità quando si reinstalla il ruolo.  
  
-   Se si intende abbassare definitivamente di livello il controller di dominio, rimuovere i ruoli di dominio Active Directory e le funzionalità a propria discrezione. Sarà necessario deselezionare le caselle di controllo per tali ruoli e funzionalità.  
  
    L'elenco completo di AD DS di ruoli e funzionalità includono:  
  
    -   Funzionalità di Active Directory modulo per Windows PowerShell  
  
    -   Funzionalità di dominio Active Directory e gli strumenti di AD LDS  
  
    -   Funzionalità Centro di amministrazione di Active Directory  
  
    -   Funzionalità di AD DS Snap-in e strumenti da riga di comando  
  
    -   Server DNS  
  
    -   Console Gestione criteri di gruppo  
  
Il cmdlet ADDSDeployment e ServerManager di Windows PowerShell equivalenti sono:  
  
```  
Uninstall-addsdomaincontroller  
Uninstall-windowsfeature  
  
```  
  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_RemoveFeatures.png)  
  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demote.png)  
  
### <a name="credentials"></a>Credenziali  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Credentials.png)  
  
Configurare le opzioni di abbassamento di livello nel **credenziali** pagina. Fornire le credenziali necessarie per eseguire l'abbassamento di livello dall'elenco seguente:  
  
-   L'abbassamento di livello un controller di dominio aggiuntivo richiede le credenziali di amministratore di dominio. Selezione **forzare la rimozione di questo controller di dominio** viene abbassato di livello il controller di dominio senza rimuovere i metadati dell'oggetto controller di dominio da Active Directory.  
  
    > [!WARNING]  
    > Non selezionare questa opzione a meno che il controller di dominio non riesce a contattare altri controller di dominio e non esiste *in alcun modo ragionevole* per risolvere il problema di rete. Abbassamento di livello forzato lascia orfani i metadati in Active Directory nei restanti controller di dominio nella foresta. Inoltre, tutte le modifiche non replicate nel controller di dominio, ad esempio le password o nuovi account utente, andranno perse. I metadati orfani sono la causa principale di un'elevata percentuale di casi di supporto tecnico clienti Microsoft per servizi di dominio Active Directory, Exchange, SQL e altro software.  
    >   
    > Se si forzatamente abbassare di livello un controller di dominio è *deve* manualmente eseguire immediatamente la pulizia dei metadati. For steps, review [Clean Up Server Metadata](https://technet.microsoft.com/library/cc816907(WS.10).aspx).  
  
   ![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ForceDemote.png)  
  
-   Abbassare di livello l'ultimo controller di dominio in un dominio è necessario appartenere al gruppo Enterprise Admins, quanto viene rimosso il dominio stesso (se l'ultimo dominio nella foresta, questo criterio rimuove la foresta). Server Manager segnala che il controller di dominio corrente è l'ultimo controller di dominio nel dominio. Selezionare il **ultimo controller di dominio nel dominio** casella di controllo per verificare il controller di dominio è l'ultimo controller di dominio nel dominio.  
  
Gli argomenti ADDSDeployment di Windows PowerShell equivalenti sono:  
  
```  
-credential <pscredential>  
-forceremoval <{ $true | false }>  
-lastdomaincontrollerindomain <{ $true | false }>  
  
```  
  
### <a name="warnings"></a>Avvisi  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Warnings.png)  
  
Il **avvisi** pagina avverte delle possibili conseguenze della rimozione di questo controller di dominio. Per continuare, è necessario selezionare **Procedi alla rimozione**.  
  
> [!WARNING]  
> Se è stato selezionato in precedenza **forzare la rimozione di questo controller di dominio** nel **credenziali** pagina il **avvisi** pagina Mostra tutti i ruoli Flexible Single Master Operations ospitati da questo controller di dominio. Si *deve* requisire i ruoli da un altro controller di dominio *immediatamente* dopo l'abbassamento di livello questo server. Per ulteriori informazioni sulla requisizione dei ruoli FSMO, vedere [requisire il ruolo di Master operazioni](https://technet.microsoft.com/library/cc816779(WS.10).aspx).  
  
Questa pagina non dispone di un argomento ADDSDeployment di Windows PowerShell equivalente.  
  
### <a name="removal-options"></a>Opzioni di rimozione  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ReviewOptions.png)  
  
Il **opzioni di rimozione** pagina viene visualizzata se in precedenza è stato selezionato **ultimo controller di dominio nel dominio** nel **credenziali** pagina. Questa pagina consente di configurare altre opzioni di rimozione. Selezionare **Ignora ultimo server DNS per zona**, **Rimuovi partizioni applicative**, e **Rimuovi la delega DNS** per esporre il **Avanti** pulsante.  
  
Se applicabile per questo controller di dominio, vengono visualizzate solo le opzioni. Ad esempio, se è presente alcun delega DNS per questo server quindi la casella di controllo non verrà visualizzati.  
  
Fare clic su **modifica** per specificare credenziali amministrative DNS alternative. Fare clic su **Visualizza partizioni** per visualizzare altre partizioni rimosse dalla procedura guidata durante l'abbassamento di livello. Per impostazione predefinita, le sole partizioni aggiuntive sono dominio DNS e zone DNS della foresta. Tutte le altre partizioni sono partizioni non Windows.  
  
Gli argomenti del cmdlet di ADDSDeployment equivalenti sono:  
  
```  
-ignorelastdnsserverforzone <{ $true | false }>  
-removeapplicationpartitions <{ $true | false }>  
-removednsdelegation <{ $true | false }>  
-dnsdelegationremovalcredential <pscredential>  
```  
  
### <a name="new-administrator-password"></a>Nuova Password amministratore  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_NewAdminPwd.png)  
  
Il **nuova Password amministratore** pagina è necessario fornire una password per account di amministratore del computer locale predefinito, una volta completata l'abbassamento di livello e il computer diventa un server membro di dominio o gruppo di lavoro.  
  
Il **Uninstall-ADDSDomainController** cmdlet e gli argomenti seguono le stesse impostazioni predefinite di Server Manager, se non specificato.  
  
Il **LocalAdministratorPassword** argomento è particolare:  
  
-   Se *non specificato* come argomento, il cmdlet richiede di immettere e confermare una password nascosta. Questo è il metodo di utilizzo quando si esegue il cmdlet in modo interattivo.  
  
-   Se specificato *con un valore*, il valore deve essere una stringa sicura. Questo non è il metodo di utilizzo quando si esegue il cmdlet in modo interattivo.  
  
Ad esempio, è possibile manualmente richiedere una password utilizzando la **Read-Host** cmdlet per richiedere all'utente una stringa sicura  
  
```  
-localadministratorpassword (read-host -prompt "Password:" -assecurestring)  
  
```  
  
> [!WARNING]  
> Come le due opzioni precedenti non chiedono conferma la password, utilizzala con estrema cautela: la password non è visibile  
  
È anche possibile specificare una stringa sicura come variabile di testo in chiaro convertita, anche se questo metodo è sconsigliato. Per esempio:  
  
```  
-localadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
  
```  
  
> [!WARNING]  
> Non è consigliabile inserire o archiviare una password di testo non crittografato. Chiunque esegua questo comando in uno script o ti spii a conoscenza della password di amministratore locale del computer. Conoscendola, hanno accesso a tutti i dati e possono rappresentare il server stesso.  
  
### <a name="confirmation"></a>Conferma  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Confirmation.png)  
  
Il **conferma** pagina Mostra l'abbassamento di livello pianificato; la pagina non include opzioni di configurazione dell'abbassamento di livello. Questo è l'ultima pagina visualizzata dalla procedura guidata prima dell'inizio dell'abbassamento di livello. Il pulsante Visualizza Script crea uno script di abbassamento di livello di Windows PowerShell.  
  
Fare clic su **Abbassa di livello** per eseguire il seguente cmdlet di distribuzione di Active Directory:  
  
```  
Uninstall-DomainController  
  
```  
  
Utilizzare facoltativo **Whatif** argomento con il **Uninstall-ADDSDomainController** e cmdlet per rivedere le informazioni di configurazione. In questo modo è possibile visualizzare i valori espliciti e impliciti degli argomenti di un cmdlet.  
  
Per esempio:  
  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstall.png)  
  
La richiesta di riavviare è l'ultima opportunità per annullare questa operazione quando si usa ADDSDeployment di Windows PowerShell. Per sostituire il prompt, usare il **-force** o **confermare: $false** argomenti.  
  
### <a name="demotion"></a>Abbassamento di livello  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demotion.png)  
  
Quando il **abbassamento di livello** pagina viene visualizzata, la configurazione del controller di dominio inizia e non può essere interrotta o annullata. I dettagli delle operazioni vengono visualizzati in questa pagina e scritti nei log:  
  
-   %systemroot%\Debug\Dcpromo.log.  
  
-   %systemroot%\debug\dcpromoui.log  
  
Poiché **Uninstall-AddsDomainController** e **Uninstall-WindowsFeature** solo avere un'azione ciascuno, vengono visualizzati qui nella fase di conferma con gli argomenti obbligatori minimi. Premendo INVIO, avvia il processo di abbassamento di livello irrevocabile e riavvia il computer.  
  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallConfirm.png)  
  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallWindowsFeature.png)  
  
Per accettare automaticamente il prompt di riavvio, usare il **-force** o **-confermare: $false** argomenti con qualsiasi cmdlet di ADDSDeployment di Windows PowerShell. Per impedire il riavvio automatico al termine dell'innalzamento di livello il server, utilizzare il **- norebootoncompletion: $false** argomento.  
  
> [!WARNING]  
> Non è consigliabile eseguire l'override del riavvio. Il server membro deve essere riavviato per funzionare correttamente.  
  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallFinished.png)  
  
Ecco un esempio di abbassamento forzato di livello con il minimo di argomenti obbligatori **- forceremoval** e **- demoteoperationmasterrole**. Il **-credential** argomento non è obbligatorio perché l'utente connesso come membro del gruppo Enterprise Admins:  
  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleForce.png)  
  
Ecco un esempio di rimozione dell'ultimo controller di dominio nel dominio con il minimo di argomenti obbligatori **- lastdomaincontrollerindomain** e **- removeapplicationpartitions**:  
  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleLastDC.png)  
  
Se si tenta di rimuovere il ruolo di dominio Active Directory prima di abbassare di livello il server, Windows PowerShell si blocca con un errore intenzionale:  
  
```  
Uninstall-WindowsFeature : An uninstallation prerequisite step failed duringthe removal of AD-Domain-Services, and uninstallation cannot continue.1. The domain controller needs to be demoted before the Active DirectoryDomain Services Role can be uninstalled.  
```  
  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallError.png)  
  
> [!IMPORTANT]  
> È necessario riavviare il computer dopo l'abbassamento di livello il server prima di poter rimuovere i file binari del ruolo Servizi di dominio Active Directory.  
  
### <a name="results"></a>Risultati  
![Abbassare di livello controller di dominio](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_DemoteSignoff.png)  
  
Il **risultati** pagina Mostra l'esito positivo o negativo dell'innalzamento di livello e le informazioni amministrative importanti. Il controller di dominio verrà automaticamente riavviato dopo 10 secondi.  
  

