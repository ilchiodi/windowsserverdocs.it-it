---
ms.assetid: b3d6fb87-c4d4-451c-b3de-a53d2402d295
title: Installare una nuova foresta di Active Directory di Windows Server 2012 (livello 200)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a9bdc3b237d0d0f44995f2c359cc3ef6ed8568a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71400371"
---
# <a name="install-a-new-windows-server-2012-active-directory-forest-level-200"></a>Installare una nuova foresta di Active Directory di Windows Server 2012 (livello 200)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene illustrata la nuova funzionalità di innalzamento di livello dei controller di dominio di Servizi di dominio Active Directory di Windows Server 2012 a livello introduttivo. In Windows Server 2012, Servizi di dominio Active Directory sostituisce lo strumento Dcpromo con un sistema di distribuzione basato su Server Manager e Windows PowerShell.  
  
-   [Active Directory Domain Services amministrazione semplificata](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SimplifiedAdmin)  
  
-   [Panoramica tecnica](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_TechOverview)  
  
-   [Distribuzione di una foresta con Server Manager](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [Distribuzione di una foresta con Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_PSForest)  
  
## <a name="BKMK_SimplifiedAdmin"></a>Active Directory Domain Services amministrazione semplificata  
In Windows Server 2012 è stata introdotta la nuova generazione di amministrazione semplificata di Servizi di dominio Active Directory. Si tratta dell'innovazione più radicale realizzata dopo Windows 2000 Server. L'amministrazione semplificata di Servizi di dominio Active Directory prende le mosse dagli insegnamenti appresi in dodici anni di Active Directory per offrire un'esperienza amministrativa più supportata, flessibile e intuitiva ad architetti e amministratori. Il risultato è stata la creazione di nuove versioni delle tecnologie esistenti oltre all'estensione delle funzionalità dei componenti rilasciati in Windows Server 2008 R2.  
  
### <a name="what-is-ad-ds-simplified-administration"></a>Informazioni sull'amministrazione semplificata di Servizi di dominio Active Directory  
L'Amministrazione semplificata di Servizi di dominio Active Directory è una nuova forma della distribuzione dei domini. Di seguito sono riportate alcune funzionalità:  
  
-   La distribuzione dei ruoli di Servizi di dominio Active Directory fa ora parte della nuova architettura di Server Manager e consente l'installazione remota.  
  
-   Il motore di distribuzione e configurazione di Servizi di dominio Active Directory è ora Windows PowerShell, anche quando si usa un'installazione grafica.  
  
-   L'innalzamento di livello include ora il controllo dei prerequisiti che convalida la disponibilità di foreste e domini, riducendo la possibilità di innalzamenti di livello non riusciti.  
  
-   Il livello di funzionalità della foresta di Windows Server 2012 non implementa nuove caratteristiche e il livello di funzionalità del dominio è richiesto solo per un subset di nuove caratteristiche Kerberos, sollevando gli amministratori dalla frequente necessità di un ambiente di controller di dominio omogeneo.  
  
### <a name="purpose-and-benefits"></a>Scopo e vantaggi  
Queste modifiche potrebbero apparire più complesse anziché più semplici. Nella riprogettazione del processo di distribuzione di Servizi di dominio Active Directory, tuttavia, si è presentata l'opportunità di unire diversi passaggi e procedure consigliate in azioni meno numerose e più semplici. Ciò significa, ad esempio, che la configurazione grafica di un nuovo controller di dominio di replica è ora costituita da otto finestre di dialogo contro le dodici precedenti. La creazione di una nuova foresta Active Directory richiede un *singolo* comando di Windows PowerShell con solo *un* argomento: il nome del dominio.  
  
L'importanza di Windows PowerShell in Windows Server 2012 è data dal fatto che, mentre l'elaborazione distribuita continua a evolvere, Windows PowerShell consente di usare un solo motore per la configurazione e la manutenzione sia dall'interfaccia grafica che da dall'interfaccia della riga di comando. Con Windows PowerShell i professionisti IT possono creare lo script completo di qualsiasi componente esattamente come farebbero gli sviluppatori con un'API. Mentre l'elaborazione basata su cloud diventa universale, Windows PowerShell offre infine la possibilità di amministrare in modalità remota un server, dove un computer senza interfaccia grafica ha le stesse funzionalità di gestione di uno con monitor e mouse.  
  
Un amministratore esperto di Servizi di dominio Active Directory potrà mettere a frutto le proprie precedenti conoscenze, mentre un amministratore agli inizi sperimenterà una curva di apprendimento più bassa.  
  
## <a name="BKMK_TechOverview"></a>Panoramica tecnica  
  
### <a name="what-you-should-know-before-you-begin"></a>Informazioni importanti prima di iniziare  
In questo argomento si presuppone una certa familiarità con le versioni precedenti di Servizi di dominio Active Directory e non vengono forniti dettagli di base sullo scopo e sulle funzionalità. Per altre informazioni su Servizi di dominio Active Directory, vedere le pagine seguenti del portale TechNet:  
  
-   [Active Directory Domain Services per Windows Server 2008 R2](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
  
-   [Active Directory Domain Services per Windows Server 2008](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
  
-   [Documentazione tecnica su Windows Server](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
  
### <a name="functional-descriptions"></a>Descrizioni delle funzionalità  
  
#### <a name="ad-ds-role-installation"></a>Installazione dei ruoli di Servizi di dominio Active Directory  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_SelectServerRoles.gif)  
  
L'installazione di Servizi di dominio Active Directory usa Server Manager e Windows PowerShell, come tutti gli altri ruoli del server e funzionalità in Windows Server 2012. Il programma Dcpromo.exe non contiene più le opzioni di configurazione GUI.  
  
Si usa una procedura guidata grafica in Server Manager o il modulo di ServerManager per Windows PowerShell sia nelle installazioni locali che in quelle remote. Eseguendo più istanze di queste procedure guidate o cmdlet e impostando come destinazione server diversi, è possibile distribuire Servizi di dominio Active Directory in più controller di dominio contemporaneamente, tutti da una sola console. Anche se queste nuove funzionalità non sono compatibili con le versioni precedenti di Windows Server 2008 R2 o sistemi operativi precedenti, è tuttavia possibile usare ancora l'applicazione Dism.exe introdotta in Windows Server 2008 R2 per l'installazione locale del ruolo dalla classica riga di comando.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSAddWindowsFeature.png)  
  
#### <a name="ad-ds-role-configuration"></a>Configurazione del ruolo Servizi di dominio Active Directory  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
Configurazione di Active Directory Domain Services "in precedenza come DCPROMO" è ora un'operazione distinta dall'installazione del ruolo. Dopo l'installazione del ruolo Servizi di dominio Active Directory, un amministratore configura il server come controller di dominio con un'altra procedura guidata in Server Manager o con il modulo ADDSDeployment di Windows PowerShell.  
  
La configurazione del ruolo Servizi di dominio Active Directory si basa su dodici anni di esperienza sul campo e ora i controller di dominio vengono configurati in base alle più recenti procedure consigliate di Microsoft. Domain Name System e i cataloghi globali, ad esempio, vengono installati per impostazione predefinita in ogni controller di dominio.  
  
La configurazione guidata di Active Directory di Server Manager unisce più finestre di dialogo in meno prompt e non nasconde più le impostazioni in modalità "avanzata". L'intero processo di innalzamento di livello viene eseguito in una finestra di dialogo a espansione durante l'installazione. La procedura guidata e il modulo ADDSDeployment di Windows PowerShell mostrano le principali modifiche e problemi di sicurezza, con collegamenti a ulteriori informazioni.  
  
Dcpromo.exe è stato lasciato in Windows Server 2012 solo per le installazioni automatiche dalla riga di comando e non esegue più l'installazione guidata grafica. Si consiglia di interrompere l'uso di Dcpromo.exe per le installazioni automatiche e di sostituirlo con il modulo ADDSDeployment, perché l'eseguibile attualmente deprecato non verrà incluso nella versione successiva di Windows.  
  
Queste nuove funzionalità non sono compatibili con le versioni precedenti di Windows Server 2008 R2 o sistemi operativi precedenti.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSForest.png)  
  
> [!IMPORTANT]
> Dcpromo.exe non contiene più una procedura guidata grafica e non installa più i file binari del ruolo o delle funzionalità. Se si tenta di eseguire Dcpromo.exe dalla shell Esplora risorse, viene restituito:  
> 
> "L'installazione guidata servizi di dominio Active Directory viene spostato in Server Manager. Per ulteriori informazioni, vedere <https://go.microsoft.com/fwlink/?LinkId=220921>. "  
> 
> Se si tenta di eseguire Dcpromo.exe /unattend, i file binari vengono ancora installati, come nei sistemi operativi precedenti, ma viene visualizzato l'avviso:  
> 
> "Il dcpromo in modalità automatica viene sostituito dal modulo ADDSDeployment per Windows PowerShell. Per ulteriori informazioni, vedere <https://go.microsoft.com/fwlink/?LinkId=220924>. "  
> 
> Dcpromo.exe è deprecato in Windows Server 2012 e non verrà incluso nelle versioni future di Windows né verrà ulteriormente migliorato in questo sistema operativo. Gli amministratori devono interromperne l'uso e passare ai moduli supportati di Windows PowerShell se desiderano creare controller di dominio dalla riga di comando.  
  
#### <a name="prerequisite-checking"></a>Controllo dei prerequisiti  
La configurazione dei controller di dominio implementa anche una fase di controllo dei prerequisiti che valuta la foresta e il dominio prima di procedere con l'innalzamento di livello dei controller di dominio. Sono inclusi la disponibilità del ruolo FSMO, i privilegi utente, la compatibilità dello schema esteso e altri requisiti. Questa nuova progettazione riduce i problemi in cui l'innalzamento di livello dei controller di dominio inizia e quindi si ferma a metà con un errore di configurazione irreversibile. Di conseguenza sarà meno possibile che siano presenti metadati di controller di dominio orfani nella foresta o che un server venga erroneamente usato come controller di dominio.  
  
## <a name="BKMK_SMForest"></a>Distribuzione di una foresta con Server Manager  
In questa sezione viene illustrato come installare il primo controller di dominio in un dominio radice della foresta con Server Manager in un computer Windows Server 2012 grafico.  
  
### <a name="server-manager-ad-ds-role-installation-process"></a>Processo di installazione del ruolo Servizi di dominio Active Directory di Server Manager  
Il diagramma seguente illustra il processo di installazione del ruolo Servizi di dominio Active Directory, che inizia con l'esecuzione di ServerManager.exe e termina prima dell'innalzamento di livello del controller di dominio.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment.png)  
  
#### <a name="server-pool-and-add-roles"></a>Pool di server e aggiunta di ruoli  
Tutti i computer Windows Server 2012 accessibili dal computer che esegue Server Manager sono idonei per il pool. Dopo aver creato il pool, è possibile selezionare i server per l'installazione remota di Servizi di dominio Active Directory o per la configurazione di altre opzioni disponibili in Server Manager.  
  
Per aggiungere i server, scegliere una delle procedure seguenti:  
  
-   Fare clic su **Aggiungi altri server da gestire** nel riquadro iniziale del dashboard  
  
-   Scegliere **Aggiungi server** dal menu **Gestisci**  
  
-   Fare clic con il pulsante destro del mouse su **Tutti i server** e scegliere **Aggiungi server.**  
  
Viene visualizzata la finestra di dialogo Aggiungi server:  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddServers.png)  
  
La finestra offre tre modi per aggiungere i server al pool per usarli o raggrupparli:  
  
-   Ricerca in Active Directory (utilizza LDAP, richiede che i computer appartengano a un dominio, consente di filtrare i sistemi operativi e supporta i caratteri jolly)  
  
-   Ricerca DNS (utilizza gli alias DNS o gli indirizzi IP tramite ARP, trasmissioni NetBIOS o ricerca WINS; non consente di filtrare i sistemi operativi e non supporta i caratteri jolly)  
  
-   Importazione (utilizza un elenco di server nel formato file di testo con CR/LF come separatori)  
  
Fare clic su **Trova** per ottenere un elenco di server dallo stesso dominio Active Directory a cui è stato aggiunto il computer e fare clic su uno o più nomi di server nell'elenco. Fare clic sulla freccia destra per aggiungere i server all'elenco **Selezionati**. Usare la finestra di dialogo **Aggiungi server** per aggiungere i server selezionati ai gruppi di ruoli del dashboard. In alternativa fare clic su **Gestisci** e quindi su **Crea gruppo di server** oppure fare clic su **Crea gruppo di server** nel riquadro **Server Manager** del dashboard per creare gruppi di server personalizzati.  
  
> [!NOTE]  
> La procedura di aggiunta dei server non verifica se un server è online o accessibile. Al successivo aggiornamento, tuttavia, tutti i server che non sono raggiungibili vengono contrassegnati nella visualizzazione Gestibilità di Server Manager.  
  
È possibile installare i ruoli in modalità remota in qualsiasi computer Windows Server 2012 aggiunto al pool, come indicato:  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/tADDS_SMI_TR_AddRolesFeatures.png)  
  
Non è possibile gestire completamente server che eseguono sistemi operativi precedenti a Windows Server 2012. La selezione di **Aggiungi ruoli e funzionalità** esegue il modulo **Install-WindowsFeature**di Windows PowerShell ServerManager.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddADDSToAnotherServer.png)  
  
È anche possibile usare il dashboard di Server Manager in un controller di dominio esistente per selezionare l'installazione di Servizi di dominio Active Directory del server remoto con il ruolo già preselezionato facendo clic con il pulsante destro del mouse sul riquadro del dashboard di Servizi di dominio Active Directory e scegliendo **Aggiungi Servizi di dominio Active Directory a un altro server**. Verrà richiamato **Install-WindowsFeature AD-Domain-Services**.  
  
Il computer che esegue Server Manager viene automaticamente inserito nel pool. Per installare qui il ruolo Servizi di dominio Active Directory, è sufficiente scegliere **Aggiungi ruoli e funzionalità** dal menu **Gestisci**.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ManageAddRoles.png)  
  
#### <a name="installation-type"></a>Tipo di installazione  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectInstallationType.png)  
  
La finestra di dialogo **Tipo di installazione** contiene un'opzione che non supporta Servizi di dominio Active Directory: **Installazione basata sullo scenario di Servizi Desktop remoto**. Questa opzione consente Desktop remoto solo in un carico di lavoro distribuito multiserver. Se la si seleziona, non è possibile installare Servizi di dominio Active Directory.  
  
Quando si installa Servizi di dominio Active Directory, lasciare sempre la selezione predefinita: **Installazione basata su ruoli o basata su funzionalità**.  
  
#### <a name="server-selection"></a>Selezione dei server  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectDestinationServer.png)  
  
La finestra di dialogo **Selezione dei server** consente di scegliere uno dei server aggiunti in precedenza al pool, purché sia accessibile. Il server locale che esegue Server Manager è automaticamente disponibile.  
  
Inoltre, selezionando i file VHD di Hyper-V offline con il sistema operativo Windows Server 2012, Server Manager aggiunge direttamente il ruolo ai file mediante la manutenzione dei componenti. Ciò consente di effettuare il provisioning dei componenti necessari ai server virtuali prima di configurarli ulteriormente.  
  
#### <a name="server-roles-and-features"></a>Ruoli del server e funzionalità  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectServerRoles.png)  
  
Per alzare di livello un controller di dominio, selezionare il ruolo **Servizi di dominio Active Directory**. Tutte le funzionalità di amministrazione di Active Directory e i servizi necessari vengono installati automaticamente, anche se apparentemente fanno parte di un altro ruolo o non risultano selezionati nell'interfaccia di Server Manager.  
  
In Server Manager è inoltre disponibile una finestra di dialogo informativa che mostra le funzionalità di gestione installate implicitamente da questo ruolo. È l'equivalente dell'argomento **-IncludeManagementTools**.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddFeaturesDialog.gif)  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectFeatures.png)  
  
Se necessario, è possibile aggiungere qui altre **Funzionalità** .  
  
#### <a name="active-directory-domain-services"></a>Servizi di dominio di Active Directory  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSIntro.png)  
  
La finestra di dialogo **Servizi di dominio Active Directory** contiene informazioni limitate sui requisiti e sulle procedure consigliate. Funge essenzialmente da un messaggio di conferma che si è scelto il ruolo di dominio Active Directory "Se non viene visualizzata questa schermata, non è stata selezionata AD DS.  
  
#### <a name="confirmation"></a>Conferma  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Confirmation.png)  
  
La finestra di dialogo **Conferma** è l'ultimo controllo prima dell'avvio dell'installazione del ruolo. Contiene un'opzione per riavviare il computer, se necessario, dopo l'installazione del ruolo, anche se l'installazione di Servizi di dominio Active Directory non richiede un riavvio.  
  
Per confermare di essere pronti a iniziare l'installazione del ruolo, fare clic su **Installa**. Non è possibile annullare l'installazione di un ruolo, una volta iniziata.  
  
#### <a name="results"></a>Risultati  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Results.png)  
  
La finestra di dialogo **Risultati** mostra l'avanzamento e lo stato dell'installazione corrente. L'installazione del ruolo continua indipendentemente dal fatto che Server Manager venga chiuso o meno.  
  
La verifica dei risultati dell'installazione è tuttavia una procedura consigliata. Se si chiude la finestra di dialogo **Risultati** prima del completamento dell'installazione, è possibile controllare i risultati con il contrassegno di notifica di Server Manager. Server Manager mostra anche un messaggio di avviso per i server in cui è installato il ruolo Servizi di dominio Active Directory, ma che non sono stati ulteriormente configurati come controller di dominio.  
  
**Notifiche delle attività**  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskNotofications.png)  
  
**Dettagli di servizi di dominio Active Directory**  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSDetails.png)  
  
**Dettagli attività**  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskDetails.png)  
  
#### <a name="promote-to-domain-controller"></a>Alzare di livello a controller di dominio  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Promote.png)  
  
Al termine dell'installazione del ruolo Servizi di dominio Active Directory, è possibile continuare con la configurazione usando il collegamento **Alza di livello il server a controller di dominio** . Questa operazione è richiesta per impostare il server come controller di dominio, ma non è necessario eseguire immediatamente la configurazione guidata. È ad esempio possibile avere la necessità di effettuare il provisioning dei file binari di Servizi di dominio Active Directory ai server prima di inviarli a un'altra succursale per una configurazione successiva. Aggiungendo il ruolo Servizi di dominio Active Directory prima dell'invio, si risparmia tempo quando raggiunge la destinazione. In questo modo si applica inoltre la procedura consigliata di non lasciare un controller di dominio offline per giorni o settimane. Infine questo consente di aggiornare i componenti prima dell'innalzamento di livello del controller di dominio, evitando almeno un riavvio successivo.  
  
Se si seleziona questo collegamento in un secondo momento, vengono richiamati i cmdlet ADDSDeployment: **install-addsforest**, **install-addsdomain**o **install-addsdomaincontroller**.  
  
### <a name="uninstallingdisabling"></a>Disinstallazione/Disabilitazione  
Il ruolo Servizi di dominio Active Directory viene rimosso come tutti gli altri ruoli, indipendentemente dal fatto che il server sia stato alzato di livello a controller di dominio o meno. Tuttavia, al termine della rimozione del ruolo Servizi di dominio Active Directory sarà necessario eseguire un riavvio.  
  
Per poter completare la rimozione del ruolo Servizi di dominio Active Directory, a differenza dell'installazione, è necessario abbassare di livello il controller di dominio. Questa operazione è necessaria per evitare che i file binari del ruolo di un controller di dominio vengano disinstallati senza la pulizia dei metadati appropriati nella foresta. Per ulteriori informazioni, vedere [abbassamento di livello controller di dominio e domini & #40; Livello 200 & #41;](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
> [!WARNING]  
> La rimozione dei ruoli Servizi di dominio Active Directory con Dism.exe o con il modulo Gestione e manutenzione immagini distribuzione di Windows PowerShell dopo l'innalzamento di livello a controller di dominio non è supportata e impedirà il normale avvio del server.  
>   
> Diversamente da Server Manager o dal modulo di distribuzione di Servizi di dominio Active Directory per Windows PowerShell, Gestione e manutenzione immagini distribuzione è un sistema di manutenzione nativo senza informazioni inerenti a Servizi di dominio Active Directory o alla sua configurazione. Non usare Dism.exe o il modulo Gestione e manutenzione immagini distribuzione di Windows PowerShell per disinstallare il ruolo Servizi di dominio Active Directory a meno che il server non sia più un controller di dominio.  
  
### <a name="create-an-ad-ds-forest-root-domain-with-server-manager"></a>Creare un dominio radice della foresta Servizi di dominio Active Directory con Server Manager  
Il diagramma seguente illustra il processo di configurazione di Servizi di dominio Active Directory, nel caso in cui in precedenza sia stato installato il ruolo Servizi di dominio Active Directory e sia stata avviata la **Configurazione guidata Servizi di dominio Active Directory** mediante Server Manager.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_forestdeploy2.png)  
  
#### <a name="deployment-configuration"></a>Configurazione distribuzione  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddNewForest.png)  
  
Server Manager inizia l'innalzamento di livello di ogni controller di dominio nella pagina **Configurazione distribuzione** . Le opzioni restanti e i campi obbligatori in questa pagina e nella pagine successive sono diversi a seconda dell'operazione di distribuzione selezionata.  
  
Per creare una nuova foresta Active Directory, fare clic su **Aggiungi una nuova foresta**. È necessario specificare un nome di dominio radice valido. Al nome non può essere assegnato un nome con etichetta singola (ad esempio, il nome deve essere *contoso.com* o simile e non semplicemente *contoso*) e devono essere usati i requisiti di denominazione dei domini DNS consentiti.  
  
Per altre informazioni sui nomi di dominio validi, vedere l'articolo della KB [Convenzioni di denominazione in Active Directory per computer, domini, siti e unità organizzative](https://support.microsoft.com/kb/909264).  
  
> [!WARNING]  
> Non creare nuove foreste Active Directory con gli stessi nomi del nome DNS esterno. Se ad esempio l'URL DNS Internet è http://contoso.com, è necessario scegliere un nome diverso per la foresta interna per evitare problemi di compatibilità futuri. Il nome deve essere univoco e non scontato per il traffico Web. Ad esempio, corp.contoso.com.  
  
Una nuova foresta non necessita di nuove credenziali per l'account Administrator del dominio. Il processo di innalzamento di livello del controller di dominio usa le credenziali dell'account predefinito Administrator dal primo controller di dominio usato per creare la radice della foresta. Non è possibile (per impostazione predefinita) disabilitare o bloccare l'account predefinito Administrator, che potrebbe essere il solo punto di ingresso in una foresta se gli altri account di dominio amministrativi sono inutilizzabili. È essenziale conoscere la password prima di distribuire una nuova foresta.  
  
**DomainName** richiede un nome DNS di dominio completo ed è obbligatorio.  
  
#### <a name="domain-controller-options"></a>Opzioni controller di dominio  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DCOptions_Forest.gif)  
  
**Opzioni controller di dominio** consente di configurare il **livello funzionalità foresta** e il **livello funzionalità dominio** per il nuovo dominio radice della foresta. Per impostazione predefinita, queste impostazioni sono Windows Server 2012 in un dominio radice della foresta di nuovo. Il livello di funzionalità foresta Windows Server 2012 non forniscono alcuna nuova funzionalità il livello di funzionalità foresta Windows Server 2008 R2. Il livello funzionale del dominio Windows Server 2012 è necessario solo per implementare le nuove impostazioni Kerberos "Fornisci sempre attestazioni" e "Rifiuta le richieste di autenticazione non blindate". Degli utilizzi principali dei livelli funzionalità in Windows Server 2012 è quello di limitare la partecipazione nei controller di dominio a dominio che soddisfano i requisiti del sistema operativo minimo consentito. In altre parole, è possibile specificare Windows Server 2012 dominio funzionale livello solo controller di dominio che eseguono Windows Server 2012 possono ospitare il dominio.  Windows Server 2012 implementa un nuovo flag di controller di dominio denominato **DS_WIN8_REQUIRED** nel **DSGetDcName** funzione di NetLogon che individua esclusivamente i controller di dominio di Windows Server 2012. Questo offre la flessibilità di una foresta più omogenea ed eterogenea in termini di sistemi operativi che è possibile eseguire sui controller di dominio.  
  
Per ulteriori informazioni sulla posizione dei controller di dominio, [funzioni del servizio Directory](https://msdn.microsoft.com/library/ms675900(VS.85).aspx).  
  
La sola funzionalità configurabile dei controller di dominio è l'opzione del server DNS. È consigliabile che tutti i controller di dominio forniscano servizi DNS per garantire un'elevata disponibilità negli ambienti distribuiti, ragione per cui questa opzione è selezionata per impostazione predefinita quando si installa un controller di dominio in qualsiasi modalità o dominio. Le opzioni del catalogo globale e dei controller di dominio di sola lettura non sono disponibili quando si crea un nuovo dominio radice della foresta. Il primo controller di dominio deve essere un catalogo globale e non può essere un controller di dominio di sola lettura.  
  
La **Password modalità ripristino servizi directory** specificata deve soddisfare i criteri password applicati al server, che per impostazione predefinita non richiedono una password complessa, ma semplicemente una non vuota. Scegliere sempre una password complessa e di difficile individuazione o preferibilmente una passphrase.  
  
#### <a name="dns-options-and-dns-delegation-credentials"></a>Opzioni DNS e credenziali di delega DNS  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestDNSOptions.png)  
  
La pagina **Opzioni DNS** consente di configurare la delega DNS e di specificare le credenziali amministrative DNS.  
  
Non è possibile configurare le opzioni o la delega DNS nella Configurazione guidata Servizi di dominio Active Directory quando si installa un nuovo dominio radice della foresta Active Directory in cui si è selezionato il **server DNS** nella pagina **Opzioni controller di dominio**. L'opzione **Crea delega DNS** è disponibile quando si crea una nuova zona DNS radice della foresta in un'infrastruttura di server DNS esistente. Questa opzione consente di specificare credenziali amministrative DNS alternative con i diritti per aggiornare la zona DNS.  
  
Per altre informazioni sulla necessità di creare una delega DNS, vedere [Informazioni sulla delega delle zone](https://technet.microsoft.com/library/cc771640.aspx).  
  
#### <a name="additional-options"></a>Opzioni aggiuntive  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestAdditionalOptions.png)  
  
La pagina **Opzioni aggiuntive** mostra il nome NetBIOS del dominio e consente di sostituirlo. Per impostazione predefinita, il nome di dominio NetBIOS corrisponde all'etichetta a sinistra del nome di dominio completo specificato nella pagina **Configurazione distribuzione**. Se, ad esempio, è stato specificato il nome di dominio completo corp.contoso.com, il nome di dominio NetBIOS predefinito è CORP.  
  
Se il nome non contiene più di 15 caratteri e non è in conflitto con un altro nome NetBIOS, non viene modificato. Se è in conflitto con un altro nome NetBIOS, al nome viene aggiunto un numero. Se il nome contiene più di 15 caratteri, la procedura guidata propone un suggerimento univoco troncato. In entrambi i casi, la procedura guidata convalida prima il nome che non è già in uso tramite una ricerca WINS e una trasmissione NetBIOS.  
  
Per altre informazioni sui nomi di dominio validi, vedere l'articolo della KB [Convenzioni di denominazione in Active Directory per computer, domini, siti e unità organizzative](https://support.microsoft.com/kb/909264).  
  
#### <a name="paths"></a>Percorsi  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPaths.png)  
  
Nella pagina **Percorsi** è possibile sostituire i percorsi predefiniti delle cartelle per il database di Servizi di dominio Active Directory, i registri delle transazioni del database e la condivisione SYSVOL. Le posizioni predefinite sono sempre in sottodirectory di %systemroot% (ad esempio, C:\Windows).  
  
#### <a name="review-options-and-view-script"></a>Verifica opzioni e Visualizza script  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestReviewOptions.png)  
  
Nella pagina **Verifica opzioni** è possibile convalidare le impostazioni e accertarsi se soddisfano i requisiti prima di iniziare l'installazione. Questa non è l'ultima possibilità per interrompere l'installazione quando si utilizza Server Manager. È semplicemente un'opzione per confermare le impostazioni prima di proseguire con la configurazione.  
  
La pagina **Verifica opzioni** di Server Manager include inoltre un pulsante opzionale **Visualizza script** , che consente di creare un file di testo Unicode contenente la configurazione ADDSDeployment corrente come singolo script di Windows PowerShell. In questo modo è possibile utilizzare l'interfaccia grafica di Server Manager come strumento di distribuzione di Windows PowerShell. Utilizzare la Configurazione guidata Servizi di dominio Active Directory per configurare le opzioni, esportare la configurazione e annullare la procedura guidata. Questo processo crea un esempio valido e sintatticamente corretto che può essere utilizzato direttamente o successivamente modificato. Esempio:  
  
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
> Server Manager completa in genere tutti gli argomenti con i valori durante l'innalzamento di livello e non usa le impostazioni predefinite perché queste potrebbero cambiare nelle future versioni di Windows o nei Service Pack. La sola eccezione è l'argomento **-safemodeadministratorpassword**, che viene deliberatamente omesso dallo script. Per forzare un prompt di conferma, omettere il valore quando si esegue il cmdlet in modo interattivo.  
  
#### <a name="prerequisites-check"></a>Controllo dei prerequisiti  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPrereqCheck.png)  
  
**Controllo dei prerequisiti** è una nuova funzionalità nella configurazione del dominio di Servizi di dominio Active Directory. Questa nuova fase convalida che la configurazione server sia in grado di supportare una nuova foresta Servizi di dominio Active Directory.  
  
Quando si installa un nuovo dominio radice della foresta, la Configurazione guidata Servizi di dominio Active Directory di Server Manager richiama una serie di test modulari. Questi test avvisano l'utente con le opzioni di ripristino suggerite. È possibile eseguire i test ogni volta che è necessario. Il processo del controller di dominio non può continuare finché tutti i test sui prerequisiti non vengono superati.  
  
**Controllo dei prerequisiti** copre anche le informazioni pertinenti, ad esempio le modifiche alla sicurezza, che interessano i sistemi operativi precedenti.  
  
Per ulteriori informazioni sui controlli dei prerequisiti specifici, vedere [controllo dei prerequisiti](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
#### <a name="installation"></a>Installazione  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestInstallation.png)  
  
Quando la pagina **Installazione** viene visualizzata, la configurazione del controller di dominio inizia e non può essere interrotta o annullata. I dettagli delle operazioni vengono visualizzati in questa pagina e scritti nei log:  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
> [!NOTE]  
> È possibile eseguire simultaneamente dalla stessa console di Server Manager più procedure guidate di installazione dei ruoli e di configurazione di Servizi di dominio Active Directory.  
  
#### <a name="results"></a>Risultati  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
La pagina **Risultati** mostra l'esito positivo o negativo dell'innalzamento di livello e le informazioni amministrative importanti. Il controller di dominio verrà automaticamente riavviato dopo 10 secondi.  
  
## <a name="BKMK_PSForest"></a>Distribuzione di una foresta con Windows PowerShell  
In questa sezione viene illustrato come installare il primo controller di dominio in un dominio radice della foresta con Windows PowerShell in un computer Windows Server 2012 principale.  
  
### <a name="windows-powershell-ad-ds-role-installation-process"></a>Processo di installazione del ruolo Servizi di dominio Active Directory di Windows PowerShell  
Implementando alcuni semplici cmdlet di distribuzione ServerManager nei processi di distribuzione, è possibile semplificare ulteriormente l'amministrazione di Servizi di dominio Active Directory.  
  
La figura successiva illustra il processo di installazione del ruolo Servizi di dominio Active Directory, che inizia con l'esecuzione di **PowerShell.exe** e termina prima dell'innalzamento di livello del controller di dominio.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment_powershell.png)  
  
|||  
|-|-|  
|Cmdlet ServerManager|Argomenti. Gli argomenti in **grassetto** sono obbligatori. Gli argomenti in *corsivo* possono essere specificati usando Windows PowerShell o la Configurazione guidata Servizi di dominio Active Directory.|  
|Install-WindowsFeature/Add-WindowsFeature|***-Nome***<br /><br />*-Riavvia*<br /><br />*-IncludeAllSubFeature*<br /><br />*-IncludeManagementTools*<br /><br />-Source<br /><br />*-ComputerName*<br /><br />-Credential<br /><br />-LogPath<br /><br />*-VHD*<br /><br />*-ConfigurationFilePath*|  
  
> [!NOTE]  
> Anche se non è obbligatorio, l'argomento **-IncludeManagementTools** è consigliato quando si installano i file binari del ruolo Servizi di dominio Active Directory.  
  
Il modulo ServerManager espone le parti relative a installazione, stato e rimozione del ruolo del nuovo modulo Gestione e manutenzione immagini distribuzione per Windows PowerShell. Questa sovrapposizione semplifica la maggior parte delle attività e riduce la necessità di utilizzo diretto del modulo Gestione e manutenzione immagini distribuzione avanzato (ma pericoloso se mal utilizzato).  
  
Usare **Get-Command** per esportare gli alias e i cmdlet in ServerManager.  
  
```powershell  
Get-Command -module ServerManager  
```  
  
Esempio:  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetCommand.png)  
  
Per aggiungere il ruolo Servizi di dominio Active Directory, è sufficiente eseguire **Install-WindowsFeature** con il nome del ruolo Servizi di dominio Active Directory come argomento. Come Server Manager, tutti i servizi necessari inerenti il ruolo Servizi di dominio Active Directory vengono installati automaticamente.  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services  
```  
  
Per installare anche gli strumenti di gestione di Servizi di dominio Active Directory (consigliato), specificare l'argomento **-IncludeManagementTools** :  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools  
```  
  
Esempio:  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallWinFeature.png)  
  
Per elencare tutte le funzionalità e i ruoli con lo stato dell'installazione, usare **Get-WindowsFeature** senza argomenti. Specificare l'argomento **-ComputerName** per lo stato dell'installazione da un server remoto.  
  
```powershell  
Get-WindowsFeature  
```  
  
Poiché **Get-WindowsFeature** non dispone di un meccanismo di filtro, è necessario usare **Where-Object** con una pipeline per trovare funzionalità specifiche. La pipeline è un canale usato tra più cmdlet per passare i dati e il cmdlet Where-Object funge da filtro. La variabile **$_** predefinita funge da oggetto corrente che passa attraverso la pipeline con tutte le proprietà che può contenere.  
  
```powershell  
Get-WindowsFeature | where-object <options>  
```  
  
Ad esempio, per trovare tutte le funzionalità contenenti "Active Dir" nella proprietà **Nome visualizzato**, usare:  
  
```powershell  
Get-WindowsFeature | where displayname -like "*active dir*"  
```  
  
Di seguito sono riportati altri esempi:  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetWindowsFeature.png)  
  
Per altre informazioni su ulteriori operazioni di Windows PowerShell con le pipeline e Where-Object, vedere [Piping e pipeline in Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
In Windows PowerShell 3.0, inoltre, sono stati considerevolmente semplificati gli argomenti della riga di comando necessari in questa operazione con la pipeline. In Windows PowerShell 2.0 richiede:  
  
```powershell  
Get-WindowsFeature | where {$_.displayname - like "*active dir*"}  
```  
  
Usando la pipeline di Windows PowerShell, è possibile creare risultati leggibili. Esempio:  
  
```powershell  
Install-WindowsFeature | Format-List  
Install-WindowsFeature | select-object | Format-List  
  
```  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDS.png)  
  
Se si usa il cmdlet **Select-Object** con l'argomento **-expandproperty**, vengono restituiti dati interessanti:  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSWithTools.png)  
  
> [!NOTE]  
> L'argomento **Select-Object -expandproperty** riduce leggermente le prestazioni generali dell'installazione.  
  
### <a name="BKMK_PS"></a>Creare un dominio radice della foresta di servizi di dominio Active Directory con Windows PowerShell  
Per installare una nuova foresta Active Directory con il modulo ADDSDeployment, usare il cmdlet seguente:  
  
```powershell  
Install-addsforest  
```  
  
Il cmdlet **Install-AddsForest** ha solo due fasi (controllo dei prerequisiti e installazione). Nelle due figure seguenti è illustrata la fase di installazione con l'argomento minimo obbligatorio di **-domainname**.  
  
|||  
|-|-|  
|Cmdlet di ADDSDeployment|Argomenti. Gli argomenti in **grassetto** sono obbligatori. Gli argomenti in *corsivo* possono essere specificati usando Windows PowerShell o la Configurazione guidata Servizi di dominio Active Directory.|  
|install-addsforest|-Confirm<br /><br />*-CreateDNSDelegation*<br /><br />*-DatabasePath*<br /><br />*-DomainMode*<br /><br />***-DomainName***<br /><br />***-DomainNetBIOSName***<br /><br />*-DNSDelegationCredential*<br /><br />*-ForestMode*<br /><br />-Force<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-NoDnsOnNetwork<br /><br />-NoRebootOnCompletion<br /><br />*-SafeModeAdministratorPassword*<br /><br />-SkipAutoConfigureDNS<br /><br />-SkipPreChecks<br /><br />*-SYSVOLPath*<br /><br />*-WhatIf*|  
  
> [!NOTE]  
> L'argomento **DomainNetBIOSName** è obbligatorio se si desidera modificare il nome di 15 caratteri generato automaticamente sulla base del prefisso del nome di dominio DNS o se il nome eccede i 15 caratteri.  
  
Il cmdlet ADDSDeployment di **Configurazione distribuzione** di Server Manager e gli argomenti equivalenti sono:  
  
```powershell  
Install-ADDSForest  
-DomainName <string>  
```  
  
Gli argomenti equivalenti del cmdlet ADDSDeployment di Opzioni controller di dominio di Server Manager sono:  
  
```powershell  
-ForestMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-InstallDNS <{$false | $true}>  
-SafeModeAdministratorPassword <secure string>  
  
```  
  
Gli argomenti **Install-ADDSForest**, se non specificati, seguono le stesse impostazioni predefinite di Server Manager.  
  
L'operazione dell'argomento **SafeModeAdministratorPassword** è particolare:  
  
-   Se *non viene specificato* come argomento, il cmdlet richiede di inserire e confermare una password nascosta. Questo è il metodo di utilizzo consigliabile quando il cmdlet viene eseguito in modo interattivo.  
  
    Ad esempio, per creare una nuova foresta denominata corp.contoso.com e ricevere la richiesta di immettere e confermare una password nascosta:  
  
    ```powershell  
    Install-ADDSForest "DomainName corp.contoso.com  
    ```  
  
-   Se viene indicato *con un valore*, tale valore deve essere una stringa sicura. Questo non è il metodo di utilizzo consigliabile quando il cmdlet viene eseguito in modo interattivo.  
  
Ad esempio, è possibile utilizzare il cmdlet **Read-Host** per richiedere all'utente una stringa sicura:  
  
```powershell  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> Poiché l'opzione precedente non chiede conferma della password, utilizzarla con estrema cautela, in quanto la password non è visibile.  
  
È inoltre possibile specificare una stringa sicura come variabile di testo in chiaro convertita, anche se questo metodo è sconsigliato.  
  
```powershell  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
Infine, è possibile archiviare la password offuscata in un file e quindi riutilizzarla in seguito, senza visualizzare mai la password non crittografata. Esempio:  
  
```powershell  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> È sconsigliabile inserire o archiviare una password non crittografata o di testo offuscato. In questo modo, la password per la modalità ripristino servizi directory per il controller di dominio diverrebbe nota a chiunque esegua questo comando in uno script o riesca a leggerla dallo schermo dell'utente. Tutti gli utenti con accesso al file possono invertire la password offuscata. Conoscendo la password sarebbe quindi possibile accedere a un controller di dominio avviato in modalità ripristino servizi directory e infine fingersi il controller di dominio, innalzando i propri privilegi al massimo livello in una foresta Active Directory. Esiste un altro set di passaggi consigliati che prevedono l'uso di **System.Security.Cryptography** per crittografare i dati dei file di testo, ma che esulano dall'ambito di questo articolo. La procedura consigliata è di evitare completamente l'archiviazione delle password.  
  
Il cmdlet ADDSDeployment offre un'altra opzione per ignorare la configurazione automatica di impostazioni del client DNS, server d'inoltro e parametri radice. Non è possibile ignorare questa opzione di configurazione quando si usa Server Manager. Questo argomento è importante solo se il ruolo del server DNS è stato installato prima di configurare il controller di dominio:  
  
```powershell  
-SkipAutoConfigureDNS  
```  
  
Anche l'operazione **DomainNetBIOSName** è particolare:  
  
-   Se l'argomento **DomainNetBIOSName** non è specificato con un nome di dominio NetBIOS e il nome di dominio con prefisso con etichetta singola nell'argomento **DomainName** è di 15 caratteri o meno, l'innalzamento di livello continua con un nome generato automaticamente.  
  
-   Se l'argomento **DomainNetBIOSName** non è specificato con un nome di dominio NetBIOS e il nome di dominio con prefisso con etichetta singola nell'argomento **DomainName** è di 16 caratteri o più, l'innalzamento di livello ha esito negativo.  
  
-   Se l'argomento **DomainNetBIOSName** è specificato con un nome di dominio NetBIOS di 15 caratteri o meno, l'innalzamento di livello continua con il nome specificato.  
  
-   Se l'argomento **DomainNetBIOSName** è specificato con un nome di dominio NetBIOS di 16 caratteri o più, l'innalzamento di livello ha esito negativo.  
  
Gli argomenti equivalenti del cmdlet ADDSDeployment di Opzioni aggiuntive di Server Manager sono:  
  
```powershell  
-domainnetbiosname <string>  
```  
  
Gli argomenti equivalenti del cmdlet ADDSDeployment di **Percorsi** di Server Manager sono:  
  
```powershell  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
  
```  
  
Usare l'argomento facoltativo **Whatif** con il cmdlet **Install-ADDSForest** per rivedere le informazioni sulla configurazione. In questo modo è possibile visualizzare i valori espliciti e impliciti degli argomenti di un cmdlet.  
  
Esempio:  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSPaths.png)  
  
Non è possibile ignorare il **Controllo prerequisiti** quando si usa Server Manager, ma è possibile saltare il processo quando si usa il cmdlet di distribuzione di Servizi di dominio Active Directory con l'argomento seguente:  
  
```powershell  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft sconsiglia di ignorare il controllo dei prerequisiti perché l'innalzamento di livello del controller di dominio potrebbe essere solo parziale o la foresta Servizi di dominio Active Directory potrebbe risultare danneggiata.  
  
Si noti che, esattamente come Server Manager, **Install-ADDSForest** ricorda che l'innalzamento di livello riavvierà automaticamente il server.  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSReboot.png)  
  
![Installare una nuova foresta](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallProgress.png)  
  
Per accettare automaticamente il prompt di riavvio, usare gli argomenti **-force** o **-confirm:$false** con un cmdlet ADDSDeployment di Windows PowerShell. Per evitare il riavvio automatico del server al termine dell'innalzamento di livello, usare l'argomento **-norebootoncompletion**.  
  
> [!WARNING]  
> Si sconsiglia di eseguire l'override del riavvio. Il controller di dominio deve essere riavviato per funzionare correttamente.  
  
## <a name="see-also"></a>Vedere anche  
[Active Directory Domain Services (portale TechNet)](https://technet.microsoft.com/library/cc770946(WS.10).aspx)  
[Active Directory Domain Services per Windows Server 2008 R2](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
[Active Directory Domain Services per Windows Server 2008](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
[Documentazione tecnica su Windows Server (Windows Server 2003)](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
Centro di amministrazione della directory [Active: Introduzione (Windows Server 2008 R2) ](https://technet.microsoft.com/library/dd560651(WS.10).aspx)  
[Amministrazione di Active Directory con Windows PowerShell (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd378937(WS.10).aspx)  
[Domande al team di servizi directory (Blog ufficiale del supporto tecnico Microsoft)](http://blogs.technet.com/b/askds)  
  

