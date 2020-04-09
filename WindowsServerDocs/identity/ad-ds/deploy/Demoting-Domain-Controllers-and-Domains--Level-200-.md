---
ms.assetid: 65ed5956-6140-4e06-8d99-8771553637d1
title: Abbassamento di livello dei controller di dominio e dei domini (livello 200)
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b8c5502f50b065e8c75d0167328868ac129dfad1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80825430"
---
# <a name="demoting-domain-controllers-and-domains"></a>Domini e controller di dominio abbassamento

>Si applica a: Windows Server

In questo argomento viene illustrato come rimuovere Servizi di dominio Active Directory con Server Manager o Windows PowerShell.
  
## <a name="ad-ds-removal-workflow"></a>Flusso di lavoro rimozione servizi di dominio Active Directory

![Grafico del flusso di lavoro rimozione servizi di dominio Active Directory](media/Demoting-Domain-Controllers-and-Domains--Level-200-/adds_demotedomainforest.png)  

> [!CAUTION]
> La rimozione dei ruoli Servizi di dominio Active Directory con Dism.exe o con il modulo Gestione e manutenzione immagini distribuzione di Windows PowerShell dopo l'innalzamento di livello a controller di dominio non è supportata e impedirà il normale avvio del server.
>
> Diversamente da Server Manager o dal modulo di distribuzione di Servizi di dominio Active Directory per Windows PowerShell, Gestione e manutenzione immagini distribuzione è un sistema di manutenzione nativo senza informazioni inerenti a Servizi di dominio Active Directory o alla sua configurazione. Non usare Dism.exe o il modulo Gestione e manutenzione immagini distribuzione di Windows PowerShell per disinstallare il ruolo Servizi di dominio Active Directory a meno che il server non sia più un controller di dominio.

## <a name="demotion-and-role-removal-with-powershell"></a>Abbassamento di e rimozione dei ruoli con PowerShell

|||  
|-|-|  
|**Cmdlet ADDSDeployment e ServerManager**|Argomenti. Gli argomenti in **grassetto** sono obbligatori. Gli argomenti in *corsivo* possono essere specificati usando Windows PowerShell o la Configurazione guidata Servizi di dominio Active Directory.|  
|Uninstall-ADDSDomainController|-SkipPreChecks<p>*-LocalAdministratorPassword*<p>-Confirm<p>***-Credential***<p>-DemoteOperationMasterRole<p>*-DNSDelegationRemovalCredential*<p>-Force<p>*-ForceRemoval*<p>*-IgnoreLastDCInDomainMismatch*<p>*-IgnoreLastDNSServerForZone*<p>*-LastDomainControllerInDomain*<p>-Norebootoncompletion<p>*-RemoveApplicationPartitions*<p>*-RemoveDNSDelegation*<p>-RetainDCMetadata|  
|Uninstall-WindowsFeature/Remove-WindowsFeature|***-Nome***<p>***-IncludeManagementTools***<p>*-Riavvia*<p>-Remove<p>-Force<p>-ComputerName<p>-Credential<p>-LogPath<p>-Vhd|  
  
> [!NOTE]  
> L'argomento **-credential** è obbligatorio solo se non si è già connessi come membro del gruppo Enterprise Admins (abbassamento di livello dell'ultimo controller di dominio di un dominio) o del gruppo Domain Admins (abbassamento di livello di un controller di dominio di replica). L'argomento **-includemanagementtools** è obbligatorio solo se si desidera rimuovere tutte le utilità di gestione di Servizi di dominio Active Directory.  
  
## <a name="demote"></a>Abbassa di livello  
  
### <a name="remove-roles-and-features"></a>Rimuovi ruoli e funzionalità

Server Manager offre due interfacce per la rimozione del ruolo Servizi di dominio Active Directory:  
  
* Il menu **Gestisci** nel dashboard principale, con **Rimuovi ruoli e funzionalità**  

   ![Server Manager rimuovere ruoli e funzionalità](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Manage.png)  

* Fare clic su **Servizi di dominio Active Directory** o su **Tutti i server** nel pannello di navigazione. Scorrere verso il basso fino alla sezione **Ruoli e funzionalità**. Fare clic con il pulsante destro del mouse su **Servizi di dominio Active Directory** nell'elenco **Ruoli e funzionalità** e scegliere **Rimuovi ruolo o funzionalità**. Questa interfaccia salta la pagina **Selezione dei server**.  

   ![Server Manager-tutti i server-rimuovere ruoli e funzionalità](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection.png)  

I cmdlet **Uninstall-WindowsFeature** e **Remove-WindowsFeature** di ServerManager impediscono di rimuovere il ruolo Servizi di dominio Active Directory fino a quando il controller di dominio non verrà abbassato.
  
### <a name="server-selection"></a>Selezione dei server

![Rimozione guidata ruoli e funzionalità Selezione server di destinazione](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection2.png)  

La finestra di dialogo **Selezione dei server** consente di scegliere uno dei server aggiunti in precedenza al pool, purché sia accessibile. Il server locale che esegue Server Manager è sempre automaticamente disponibile.  

### <a name="server-roles-and-features"></a>Ruoli del server e funzionalità

![Rimozione guidata ruoli e funzionalità-selezione dei ruoli da rimuovere](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerRoles.png)  

Deselezionare la casella di controllo **Servizi di dominio Active Directory** per abbassare di livello un controller di dominio. Se il server è attualmente un controller di dominio, il ruolo Servizi di dominio Active Directory non viene rimosso, ma si passa alla finestra di dialogo **Risultati convalida** con l'opzione per l'abbassamento di livello. In caso contrario, rimuove i file binari come qualsiasi altra funzionalità del ruolo.  

* Non rimuovere nessun altro ruolo o funzionalità relativa a Servizi di dominio Active Directory, ad esempio DNS, Console Gestione Criteri di gruppo o gli strumenti di Strumenti di amministrazione remota del server, se si intende innalzare di nuovo di livello il controller di dominio immediatamente. Se si rimuovono altri ruoli e funzionalità, il nuovo innalzamento di livello richiede più tempo, perché Server Manager reinstalla queste funzionalità quando si reinstalla il ruolo.  
* Se si intende abbassare definitivamente di livello il controller di dominio, rimuovere i ruoli e le funzionalità di Servizi di dominio Active Directory non necessari a propria discrezione. Sarà necessario deselezionare le caselle di controllo relative a tali ruoli e funzionalità.  

   L'elenco completo di ruoli e funzionalità di Servizi di dominio Active Directory include:  
  
   * Funzionalità Modulo Active Directory per Windows PowerShell  
   * Funzionalità Strumenti per Servizi di dominio Active Directory e AD LDS  
   * Funzionalità Centro di amministrazione di Active Directory  
   * Funzionalità Snap-in e strumenti da riga di comando di Servizi di dominio Active Directory  
   * Server DNS  
   * Console Gestione Criteri di gruppo  
  
I cmdlet ADDSDeployment e ServerManager di Windows PowerShell equivalenti sono:  
  
```
Uninstall-addsdomaincontroller  
Uninstall-windowsfeature  
```

![Rimozione guidata ruoli e funzionalità-finestra di dialogo di conferma](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_RemoveFeatures.png)  

![Rimozione guidata ruoli e funzionalità-convalida](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demote.png)  

### <a name="credentials"></a>Credenziali

![Configurazione guidata di Active Directory Domain Services-selezione credenziali](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Credentials.png)  

Nella pagina **Credenziali** è possibile configurare le opzioni di abbassamento di livello. Fornire le credenziali necessarie per eseguire l'abbassamento di livello attenendosi all'elenco seguente:  

* L'abbassamento di livello di un controller di dominio aggiuntivo richiede le credenziali Domain Admin. Se **si seleziona forza la rimozione di questo controller di dominio** , il controller di dominio viene abbassato di dominio senza rimuovere i metadati dell'oggetto controller di dominio da Active Directory.  

   > [!WARNING]  
   > Non selezionare questa opzione se il controller di dominio non riesce a contattare altri controller di dominio e non vi è alcuna *soluzione ragionevole* per risolvere il problema di rete. L'abbassamento di livello forzato lascia orfani i metadati di Active Directory nei restanti controller di dominio della foresta. Inoltre tutte le modifiche non replicate nel rispettivo controller di dominio, come password o nuovi account utente, andranno perse. I metadati orfani sono la causa principale di una elevata percentuale di casi riportati al supporto tecnico Microsoft riguardanti i Servizi di dominio Active Directory, Exchange, SQL e altre applicazioni software.  
   >
   > Quando si esegue l'abbassamento di livello forzato di un controller di dominio, è *necessario* effettuare immediatamente la pulizia dei metadati. Per i passaggi, esaminare [pulizia dei metadati del Server](ad-ds-metadata-cleanup.md).  

   ![Configurazione guidata di Active Directory Domain Services-rimozione forzata delle credenziali](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ForceDemote.png)  
  
* Per abbassare il livello dell'ultimo controller di dominio in un dominio è necessario appartenere al gruppo Enterprise Admins, in quanto viene rimosso il dominio stesso (se è l'ultimo dominio nella foresta, viene rimossa anche la foresta). Server Manager segnala che il controller di dominio corrente è l'ultimo controller nel dominio. Selezionare la casella di controllo **Ultimo controller di dominio del dominio** per confermare che il controller è l'ultimo controller di dominio nel dominio.  

Gli argomenti ADDSDeployment di Windows PowerShell equivalenti sono:  

```
-credential <pscredential>  
-forceremoval <{ $true | false }>  
-lastdomaincontrollerindomain <{ $true | false }>  
```

### <a name="warnings"></a>Warnings

![Configurazione guidata di Active Directory Domain Services-l'effetto dei ruoli FSMO sulle credenziali](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Warnings.png)  

La pagina **Avvisi** avverte delle possibili conseguenze della rimozione di questo controller di dominio. Per continuare, è necessario selezionare **Procedi alla rimozione**.

> [!WARNING]  
> Se in precedenza è stato selezionato **Rimozione forzata controller di dominio** nella pagina **Credenziali**, la pagina **Avvisi** mostra tutti i ruoli Flexible Single Master Operations ospitati da questo controller di dominio. È necessario *requisire i ruoli da un altro controller di dominio* immediatamente *dopo aver abbassato di livello questo server.* Per altre informazioni sulla riassegnazione dei ruoli FSMO, vedere [Riassegnare il ruolo Master operazioni](https://technet.microsoft.com/library/cc816779(WS.10).aspx).

Questa pagina non ha un argomento ADDSDeployment di Windows PowerShell equivalente.

### <a name="removal-options"></a>Opzioni di rimozione

![Configurazione guidata di Active Directory Domain Services-le credenziali rimuovono le partizioni DNS e dell'applicazione](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ReviewOptions.png)  

La pagina **Opzioni di rimozione** viene visualizzata se in precedenza è stato selezionato **Ultimo controller di dominio del dominio** nella pagina **Credenziali**. Questa pagina consente di configurare altre opzioni di rimozione. Selezionare **Ignora ultimo server DNS per zona**, **Rimuovi partizioni applicazione**e **Rimuovi delega DNS** per abilitare il pulsante **Avanti** .

Le opzioni vengono visualizzate solo se applicabili a questo controller di dominio. Se, ad esempio, non esistono deleghe DNS per questo server, tale casella di controllo non verrà visualizzata.

Fare clic su **Cambia** per specificare credenziali amministrative DNS alternative. Fare clic su **Visualizza partizioni** per visualizzare altre partizioni rimosse dalla procedura guidata durante l'abbassamento di livello. Per impostazione predefinita, le sole partizioni aggiuntive sono quelle del DNS dominio e delle zone DNS della foresta. Tutte le altre sono partizioni non Windows.

Gli argomenti del cmdlet di ADDSDeployment equivalenti sono:  

```
-ignorelastdnsserverforzone <{ $true | false }>  
-removeapplicationpartitions <{ $true | false }>  
-removednsdelegation <{ $true | false }>  
-dnsdelegationremovalcredential <pscredential>  
```

### <a name="new-administrator-password"></a>Nuova password amministratore

![Configurazione guidata di Active Directory Domain Services-credenziali nuova password amministratore](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_NewAdminPwd.png)  

Il **Nuova Password amministratore** pagina è necessario fornire una password per account di amministratore del computer locale predefinito, una volta completata l'abbassamento di livello e il computer diventa un server membro del dominio o gruppo di lavoro.

Il cmdlet e gli argomenti **Uninstall-ADDSDomainController** , se non specificati, seguono le stesse impostazioni predefinite di Server Manager.

L'argomento **LocalAdministratorPassword** è particolare:

* Se *non viene specificato* come argomento, il cmdlet richiede di inserire e confermare una password nascosta. Questo è il metodo di utilizzo consigliabile quando il cmdlet viene eseguito in modo interattivo.
* Se viene indicato *con un valore*, tale valore deve essere una stringa sicura. Questo non è il metodo di utilizzo consigliabile quando il cmdlet viene eseguito in modo interattivo.

Ad esempio, è possibile richiedere manualmente una password usando il cmdlet **Read-Host** per richiedere all'utente una stringa sicura.

```
-localadministratorpassword (read-host -prompt "Password:" -assecurestring)
```

> [!WARNING]
> Poiché le due opzioni precedenti non confermano la password, usare estrema cautela: la password non è visibile.

È inoltre possibile specificare una stringa sicura come variabile di testo in chiaro convertita, anche se questo metodo è sconsigliato. Ad esempio,

```
-localadministratorpassword (convertto-securestring "Password1" -asplaintext -force)
```

> [!WARNING]
> È sconsigliabile inserire o archiviare una password di testo in chiaro. In questo modo, la password di amministratore locale per il computer diverrebbe nota a chiunque esegua questo comando in uno script o riesca a leggerla dallo schermo dell'utente. Conoscendola, chiunque avrebbe accesso a tutti i dati e potrebbe rappresentare il server stesso.

### <a name="confirmation"></a>Conferma

![Configurazione guidata di Active Directory Domain Services-opzioni di verifica](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Confirmation.png)

La pagina **Conferma** mostra l'abbassamento di livello pianificato, ma non le opzioni di configurazione dell'abbassamento di livello. È l'ultima pagina visualizzata dalla procedura guidata prima dell'inizio dell'abbassamento di livello. Usare il pulsante Visualizza script per creare uno script di abbassamento di livello di Windows PowerShell.

Fare clic su **Abbassa di livello** per eseguire il cmdlet di distribuzione di Servizi di dominio Active Directory seguente:

```
Uninstall-ADDSDomainController
```

Usare l'argomento facoltativo **Whatif** con il cmdlet **Uninstall-ADDSDomainController** e il cmdlet per rivedere le informazioni sulla configurazione. In questo modo è possibile visualizzare i valori espliciti e impliciti degli argomenti di un cmdlet.

Ad esempio,

![Esempio di disinstallazione di PowerShell-ADDSDomainController](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstall.png)

Il prompt per riavviare è l'ultima opportunità per annullare questa operazione quando si usa ADDSDeployment per Windows PowerShell. Per sostituire il prompt, usare gli argomenti **-force** o **confirm:$false**.  

### <a name="demotion"></a>Abbasamento di livello

![Configurazione guidata di Active Directory Domain Services-abbassamento di passo in corso](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demotion.png)  

Quando la pagina **Abbassamento di livello** viene visualizzata, la configurazione del controller di dominio inizia e non può essere interrotta o annullata. I dettagli delle operazioni vengono visualizzati in questa pagina e scritti nei log:  

* %systemroot%\debug\dcpromo.log
* %systemroot%\debug\dcpromoui.log

Poiché **Uninstall-AddsDomainController** e **Uninstall-WindowsFeature** hanno solo un'azione a parte, vengono visualizzati qui nella fase di conferma con gli argomenti obbligatori minimi. Premendo INVIO, viene avviato il processo di abbassamento di livello irrevocabile e viene riavviato il computer.

![Esempio di disinstallazione di PowerShell-ADDSDomainController](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallConfirm.png)

![Esempio di disinstallazione di PowerShell-WindowsFeature](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallWindowsFeature.png)

Per accettare automaticamente il prompt di riavvio, usare gli argomenti **-force** o **-confirm:$false** con un cmdlet ADDSDeployment di Windows PowerShell. Per evitare il riavvio automatico del server al termine dell'innalzamento di livello, usare l'argomento **-norebootoncompletion:$false**.

> [!WARNING]
> Si sconsiglia di eseguire l'override del riavvio. Il server membro deve essere riavviato per funzionare correttamente.

![Esempio di disinstallazione di PowerShell-ADDSDomainController Force](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallFinished.png)

Il seguente è un esempio di abbassamento forzato di livello con il numero minimo di argomenti obbligatori **-forceremoval** e **-demoteoperationmasterrole**. L'argomento **-credential** non è obbligatorio perché l'utente ha eseguito l'accesso come membro del gruppo Enterprise Admins:

![Esempio di disinstallazione di PowerShell-ADDSDomainController](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleForce.png)

Il seguente è un esempio di rimozione dell'ultimo controller di dominio nel dominio con il numero minimo di argomenti obbligatori **-lastdomaincontrollerindomain** e **-removeapplicationpartitions**:

![Esempio di disinstallazione di PowerShell-ADDSDomainController-LastDomainControllerInDomain](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleLastDC.png)

Se si tenta di rimuovere il ruolo Servizi di dominio Active Directory prima di abbassamento il server, Windows PowerShell blocca l'errore:

![Un passaggio dei prerequisiti di disinstallazione non è riuscito durante la rimozione di servizi di dominio Active Directory e la disinstallazione non può continuare. 1. Il controller di dominio deve essere abbassato di grado prima di disinstallare il ruolo Servizi DirectoryDomain attivi.](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallError.png)

> [!IMPORTANT]
> È necessario riavviare il computer dopo aver abbassato il livello del server prima di poter rimuovere i file binari del ruolo AD-Domain-Services.

### <a name="results"></a>Risultati

![Si sta per essere disconnesso avviso dopo la rimozione di servizi di dominio Active Directory](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_DemoteSignoff.png)

La pagina **Risultati** mostra l'esito positivo o negativo dell'innalzamento di livello e le informazioni amministrative importanti. Il controller di dominio verrà automaticamente riavviato dopo 10 secondi.
