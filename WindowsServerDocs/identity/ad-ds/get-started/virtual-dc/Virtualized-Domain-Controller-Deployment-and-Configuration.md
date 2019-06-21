---
ms.assetid: b146f47e-3081-4c8e-bf68-d0f993564db2
title: Distribuzione e configurazione di controller di dominio virtualizzati
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 059bb3c1b15afdc579ba048b8bbb02ed185f3d42
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280949"
---
# <a name="virtualized-domain-controller-deployment-and-configuration"></a>Distribuzione e configurazione di controller di dominio virtualizzati

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vengono trattati gli argomenti seguenti:  
  
-   [Considerazioni sull'installazione](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_InstallConsiderations)  
  
    Illustra i requisiti della piattaforma e altri vincoli importanti.  
  
-   [Dominio virtualizzati la clonazione del Controller](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCCloning)  
  
    Descrive in dettaglio l'intero processo di clonazione di controller di dominio virtualizzati.  
  
-   [Ripristino sicuro di Controller di dominio virtualizzati](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCSafeRestore)  
  
    Illustra in dettaglio le verifiche effettuate durante il ripristino sicuro di controller di dominio virtualizzati.  
  
## <a name="BKMK_InstallConsiderations"></a>Considerazioni sull'installazione  
Non è prevista l'installazione di ruoli o funzionalità speciali per i controller di dominio virtualizzati, che contengono tutti automaticamente funzionalità di clonazione e ripristino sicuro. Non è possibile rimuovere né disabilitare queste funzionalità.  
  
Per usare i controller di dominio di Windows Server 2012, sono necessari lo schema di Servizi di dominio Active Directory di Windows Server 2012, versione 56 o successiva e un livello funzionale di foresta equivalente a Windows Server 2003 nativo o superiore.  
  
I controller di dominio scrivibili e di sola lettura supportano tutti gli aspetti dei controller di dominio virtualizzati, allo stesso modo dei cataloghi globali e dei ruoli FSMO.  
  
> [!IMPORTANT]  
> Il detentore del ruolo FSMO dell'emulatore PDC deve essere online quando viene avviata la clonazione.  
  
### <a name="BKMK_PlatformReqs"></a>Requisiti di piattaforma  
La clonazione di controller di dominio virtualizzati richiede:  
  
-   Ruolo FSMO dell'emulatore PDC ospitato in un controller di dominio di Windows Server 2012  
  
-   Emulatore PDC disponibile durante le operazioni di clonazione  
  
La clonazione e il ripristino sicuro richiedono:  
  
-   Guest virtualizzati di Windows Server 2012  
  
-   Supporto della piattaforma host di virtualizzazione per l'ID di generazione VM (VMGID)  
  
Per informazioni sui prodotti di virtualizzazione, con indicazioni se supportano o meno i controller di dominio virtualizzati e l'ID di generazione VM, esaminare la tabella seguente.  
  
|||  
|-|-|  
|**Prodotto di virtualizzazione**|**Supporto dei controller di dominio virtualizzati e VMGID**|  
|**Server Microsoft Windows Server 2012 con funzionalità Hyper-V**|Yes|  
|**Microsoft Windows Server 2012 Hyper-V Server**|Yes|  
|**Funzionalità di Microsoft Windows 8 con Client Hyper-V**|Yes|  
|**Windows Server 2008 R2 e Windows Server 2008**|No|  
|**Soluzioni di virtualizzazione non Microsoft**|Contattare il fornitore|  
  
Anche se Microsoft supporta Windows 7 Virtual PC, Virtual PC 2007, Virtual PC 2004 e Virtual Server 2005, questi prodotti non possono eseguire guest a 64 bit né supportano l'ID di generazione VM.  
  
Per assistenza sui prodotti di virtualizzazione di terze parti e sul relativo supporto di controller di dominio virtualizzati, contattare direttamente il fornitore.  
  
Per altre informazioni, vedere i criteri di supporto per il [software Microsoft in esecuzione in software di virtualizzazione hardware non Microsoft](https://support.microsoft.com/kb/897615).  
  
### <a name="critical-caveats"></a>Avvertenze importanti  
I controller di dominio virtualizzati *non* supportano il ripristino sicuro degli elementi seguenti:  
  
-   File VHD e VHDX copiati manualmente su file VHD esistenti  
  
-   File VHD e VHDX ripristinati con software di backup di file o di backup completo del disco  
  
> [!NOTE]  
> File VHDX nuovi per Windows Server 2012 Hyper-V.  
  
Nessuna di queste operazioni è inclusa nella semantica dell'ID di generazione VM e pertanto non cambia l'ID di generazione VM. Il ripristino di controller di dominio con questi metodi può comportare un rollback degli USN e la messa in quarantena del controller di dominio o l'introduzione di oggetti residui e l'esigenza di operazioni di pulizia a livello di foresta.  
  
> [!WARNING]  
> Il ripristino sicuro di controller di dominio virtualizzati non sostituisce i backup di stato di sistema e il Cestino di Servizi di dominio Active Directory.  
>   
> Dopo il ripristino di uno snapshot, i delta di modifiche in precedenza non replicate che derivano dal controller di dominio dopo lo snapshot vanno persi definitivamente. Il ripristino sicuro implementa il ripristino automatizzato non autorevole *solo* per evitare la quarantena accidentale di controller di dominio.  
  
Per altre informazioni su bolle USN e oggetti residui, vedere [operazioni di risoluzione dei problemi di Active Directory che non riescono con l'errore 8606: "Attributi insufficienti sono stato passati a creare un oggetto"](https://support.microsoft.com/kb/2028495).  
  
## <a name="BKMK_VDCCloning"></a>Dominio virtualizzati la clonazione del Controller  
La clonazione di un controller di dominio virtualizzato richiede diversi passaggi e fasi, sia che si usino strumenti grafici o Windows PowerShell. In generale, le tre fasi principali sono:  
  
**Preparare l'ambiente**  
  
-   Passaggio 1: Verificare che l'hypervisor supporti l'ID di generazione VM e quindi la clonazione.  
  
-   Passaggio 2: Verificare che il ruolo emulatore PDC sia ospitato da un controller di dominio che esegue Windows Server 2012 e che siano online e raggiungibile dal controller di dominio clonato durante la clonazione.  
  
**Preparare il controller di dominio di origine**  
  
-   Passaggio 3: Autorizzare il controller di dominio di origine per la clonazione.  
  
-   Passaggio 4: Rimuovere servizi o programmi incompatibili o aggiungerli al file CustomDCCloneAllowList.xml.  
  
-   Passaggio 5: Creare il file DCCloneConfig.xml.  
  
-   Passaggio 6: Portare offline il controller di dominio di origine.  
  
**Creare il controller di dominio clonato**  
  
-   Passaggio 7: Copiare o esportare la VM di origine e aggiungere l'XML, se non è già stato copiato.  
  
-   Passaggio 8: Creare una nuova macchina virtuale dalla copia.  
  
-   Passaggio 9: Avviare la nuova macchina virtuale per iniziare la clonazione.  
  
Non esistono differenze procedurali nell'operazione quando si usano strumenti grafici come la console di gestione di Hyper-V o strumenti da riga di comando come Windows PowerShell, quindi i passaggi vengono presentati solo una volta con entrambe le interfacce. Questo argomento fornisce esempi di Windows PowerShell per consentire di esplorare l'automazione end-to-end del processo di clonazione, ma non sono necessari per i passaggi. In Windows Server 2012 non sono inclusi strumenti grafici di gestione per i controller di dominio virtualizzati.  
  
Diversi punti della procedura offrono possibilità di scelta sul modo in cui creare il computer clonato e aggiungere i file XML. Questi passaggi sono illustrati in dettaglio di seguito. Negli altri casi, il processo non è modificabile.  
  
Il diagramma seguente illustra il processo di clonazione di un controller di dominio virtualizzato, laddove il dominio esiste già.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_CloningProcessFlow.png)  
  
### <a name="step-1---validate-the-hypervisor"></a>Passaggio 1 - Convalidare l'hypervisor  
Assicurarsi che il controller di dominio di origine sia in esecuzione in un hypervisor supportato consultando la documentazione del fornitore. I controller di dominio virtualizzati sono indipendenti dall'hypervisor e non richiedono Hyper-V.  
  
Se l'hypervisor è Microsoft Hyper-V, assicurarsi che è in esecuzione in Windows Server 2012. Per verificarlo, è possibile usare Gestione dispositivi.  
  
Aprire **Devmgmt.msc** ed esaminare **Dispositivi di sistema** per controllare i dispositivi e i driver Microsoft Hyper-V installati. Il dispositivo di sistema specifico necessario per un controller di dominio virtualizzato è **Microsoft Hyper-V Generation Counter** (driver: vmgencounter.sys).  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVVMGenIDCounter.png)  
  
### <a name="step-2---verify-the-pdce-fsmo-role"></a>Passaggio 2 - Verificare il ruolo FSMO dell'emulatore PDC  
Prima di provare a clonare un controller di dominio, è necessario verificare che il controller di dominio che ospita il ruolo FSMO dell'emulatore PDC (Primary Domain Controller) esegua Windows Server 2012. L'emulatore PDC è necessario per diversi motivi:  
  
1.  L'emulatore PDC crea il gruppo speciale **Controller di dominio clonabili** e imposta la relativa autorizzazione sulla radice del dominio per consentire l'autoclonazione di un controller di dominio.  
  
2.  Il controller di dominio da clonare contatta direttamente l'emulatore PDC usando il protocollo DRSUAPI RPC per creare oggetti computer per il controller di dominio clone.  
  
    > [!NOTE]  
    > Windows Server 2012 estende il protocollo Directory Replication Service (DRS) Remote Protocol (UUID **E3514235-4B06-11D1-AB04-00C04FC2DCD2**) esistente per includere un nuovo metodo RPC, **IDL_DRSAddCloneDC** (Opnum **28**). Il metodo **IDL_DRSAddCloneDC** crea un nuovo oggetto controller di dominio copiando gli attributi di un oggetto controller di dominio esistente.  
    >   
    > Gli stati di un controller di dominio sono costituiti da computer, server, impostazioni NTDS, servizio Replica file, Replica DFS e oggetti connessione mantenuti per ogni controller di dominio. Quando duplica un oggetto, questo metodo RPC sostituisce tutti i riferimenti al controller di dominio originale con oggetti corrispondenti del nuovo controller di dominio. Il chiamante deve avere i diritti di controllo dell'accesso DS-Clone-Domain-Controller nel contesto di denominazione dei domini.  
    >   
    > L'uso di questo nuovo metodo richiede sempre l'accesso diretto al controller di dominio dell'emulatore PDC da parte del chiamante.  
    >   
    > Poiché questo metodo RPC è nuovo, il software di analisi di rete richiede parser aggiornati che includano i campi per la nuova voce Opnum 28 nell'attuale UUID E3514235-4B06-11D1-AB04-00C04FC2DCD2. In caso contrario, non è possibile analizzare il traffico.  
    >   
    > Per altre informazioni, vedere [4.1.29 IDL_DRSAddCloneDC (Opnum 28)](https://msdn.microsoft.com/library/hh554213(v=prot.13).aspx).  
  
***Questo significa anche quando si usano reti non completamente instradate, clonazione del controller di dominio virtualizzati richiede segmenti di rete con accesso all'emulatore PDC***. È accettabile spostare un controller di dominio clonato in una rete diversa dopo la clonazione, come per i controller di dominio fisici, purché si presti attenzione ad aggiornare le informazioni sul sito logico di Servizi di dominio Active Directory.  
  
> [!IMPORTANT]  
> Quando si clona un dominio contenente solo un singolo controller di dominio, è necessario assicurarsi che il controller di dominio di origine sia di nuovo online prima di iniziare le copie del clone. Un dominio di produzione dovrebbe sempre contenere almeno due controller di dominio.  
  
#### <a name="active-directory-users-and-computers-method"></a>Metodo Utenti e computer di Active Directory  
  
1.  Usando lo snap-in Dsa.msc, fare clic con il pulsante destro del mouse sul dominio e scegliere **Master operazioni**. Prendere nota del controller di dominio specificato nella scheda PDC e chiudere la finestra di dialogo.  
  
2.  Fare clic con il pulsante destro del mouse sull'oggetto computer di questo controller di dominio e scegliere **Proprietà**, quindi verificare le informazioni sul sistema operativo.  
  
#### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
È possibile combinare i seguenti cmdlet del modulo Windows PowerShell di Active Directory per restituire la versione dell'emulatore PDC:  
  
```  
Get-adddomaincontroller  
Get-adcomputer  
```  
  
Se non viene specificato il dominio, questi cmdlet considerano il dominio del computer in cui è in esecuzione.  
  
Il comando seguente restituisce informazioni sull'emulatore PDC e sul sistema operativo:  
  
```  
get-adcomputer(Get-ADDomainController -Discover -Service "PrimaryDC").name -property * | format-list dnshostname,operatingsystem,operatingsystemversion  
```  
  
L'esempio seguente illustra come specificare il nome di dominio e filtrare le proprietà restituite prima della pipeline di Windows PowerShell:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PDCOSInfo.png)  
  
### <a name="step-3---authorize-a-source-dc"></a>Passaggio 3 - Autorizzare un controller di dominio di origine  
Il controller di dominio di origine deve avere il diritto di controllo dell'accesso **Consenti a un controller di dominio di creare un clone di se stesso** nell'intestazione del contesto dei nomi di dominio. Per impostazione predefinita, il ben noto gruppo **Controller di dominio clonabili** ha questa autorizzazione e non contiene membri. L'emulatore PDC crea questo gruppo quando il ruolo FSMO viene trasferito in un controller di dominio di Windows Server 2012.  
  
#### <a name="active-directory-administrative-center-method"></a>Metodo Centro di amministrazione di Active Directory  
  
1.  Avviare Dsac.exe e passare al controller di dominio di origine, quindi aprire la relativa pagina di dettagli.  
  
2.  Nella sezione **Membro di** aggiungere il gruppo **Controller di dominio clonabili** per questo dominio.  
  
#### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
È possibile combinare i cmdlet **get-adcomputer** e **add-adgroupmember** del modulo Windows PowerShell di Active Directory per aggiungere un controller di dominio al gruppo **Controller di dominio clonabili**:  
  
```  
Get-adcomputer <dc name> | %{add-adgroupmember "cloneable domain controllers" $_.samaccountname}  
```  
  
Questo comando aggiunge ad esempio il controller di dominio DC1 server al gruppo, senza la necessità di specificare il nome distinto del membro del gruppo:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_AddDcToGroup.png)  
  
#### <a name="rebuilding-default-permissions"></a>Creazione di nuove autorizzazioni predefinite  
Se si rimuove questa autorizzazione dall'intestazione di dominio, la clonazione non riesce. È possibile ricreare l'autorizzazione usando Centro di amministrazione di Active Directory o Windows PowerShell.  
  
##### <a name="active-directory-administrative-center-method"></a>Metodo Centro di amministrazione di Active Directory  
  
1.  Aprire il **Centro di amministrazione di Active Directory**, fare clic con il pulsante destro del mouse sull'intestazione di dominio, scegliere **Proprietà**, fare clic sulla scheda **Estensioni**, quindi su **Sicurezza** e infine su **Avanzate**. Fare clic su **Solo l'oggetto specificato**.  
  
2.  Fare clic su **Aggiungi**, quindi in **Immettere il nome dell'oggetto da selezionare** digitare il nome del gruppo **Controller di dominio clonabili**.  
  
3.  In Autorizzazioni fare clic su **Consenti a un controller di dominio di creare un clone di se stesso**, quindi fare clic su **OK**.  
  
> [!NOTE]  
> È anche possibile rimuovere l'autorizzazione predefinita e aggiungere singoli controller di dominio. In questo modo, tuttavia, è possibile che si creino problemi con la manutenzione ordinaria, qualora i nuovi amministratori non siano informati di questa personalizzazione. La modifica dell'impostazione predefinita non aumenta la sicurezza ed è sconsigliata.  
  
##### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
Usare i comandi seguenti in un prompt della console di Windows PowerShell con privilegi elevati di amministratore. Questi comandi rilevano il nome di dominio e lo aggiungono nelle autorizzazioni predefinite:  
  
```  
import-module activedirectory  
cd ad:  
$domainNC = get-addomain  
$dcgroup = get-adgroup "Cloneable Domain Controllers"  
$sid1 = (get-adgroup $dcgroup).sid  
$acl = get-acl $domainNC  
$objectguid = new-object Guid 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e  
$ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid1,"ExtendedRight","Allow",$objectguid  
$acl.AddAccessRule($ace1)  
set-acl -aclobject $acl $domainNC  
cd c:  
```  
  
In alternativa, eseguire l'esempio [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms) in una console di Windows PowerShell, avviata come amministratore con privilegi elevati in un controller di dominio del dominio interessato. Le autorizzazioni vengono impostate automaticamente. L'esempio si trova nell'appendice di questo modulo.  
  
### <a name="step-4---remove-incompatible-applications-or-services-if-not-using-customdccloneallowlistxml"></a>Passaggio 4 - Rimuovere applicazioni o servizi incompatibili, se non si usa CustomDCCloneAllowList.xml  
I programmi o i servizi restituiti in precedenza da Get-ADDCCloningExcludedApplicationList, *e non aggiunti a CustomDCCloneAllowList.xml* , devono essere rimossi prima della clonazione. Il metodo consigliato è la disinstallazione dell'applicazione o del servizio.  
  
> [!WARNING]  
> Tutti i programmi o i servizi incompatibili non disinstallati né aggiunti a CustomDCCloneAllowList.xml impediscono la clonazione.  
  
Usare il cmdlet Get-AdComputerServiceAccount per individuare eventuali account del servizio gestito nel dominio e verificare se il computer ne usa uno. Se è installato un account del servizio gestito, usare il cmdlet Uninstall-ADServiceAccount per individuare l'account del servizio installato in locale. Dopo aver portato il controller di dominio di origine offline nel passaggio 6, è possibile riaggiungere l'account del servizio gestito usando Install-ADServiceAccount quando il server è di nuovo online. Per altre informazioni, vedere [Uninstall-ADServiceAccount](https://technet.microsoft.com/library/hh852310).  
  
> [!IMPORTANT]  
> Gli account del servizio gestito autonomi, rilasciati per la prima volta in Windows Server 2008 R2, sono stati sostituiti con account del servizio gestito di gruppo in Windows Server 2012. Questi account supportano la clonazione.  
  
### <a name="step-5---create-dccloneconfigxml"></a>Passaggio 5 - Creare il file DCCloneConfig.xml  
Il file DcCloneConfig.xml è necessario per la clonazione di controller di dominio. Il relativo contenuto consente di specificare dettagli univoci, come il nuovo nome computer e l'indirizzo IP.  
  
Il file CustomDCCloneAllowList.xml è facoltativo, a meno che non si installano applicazioni o servizi Windows potenzialmente incompatibili nel controller di dominio di origine. I file richiedono operazioni precise di denominazione, formattazione e posizionamento; in caso contrario, la clonazione non riesce.  
  
Per questo motivo, è consigliabile usare sempre i cmdlet di Windows PowerShell per creare i file XML e posizionarli nel percorso corretto.  
  
#### <a name="generating-with-new-addccloneconfigfile"></a>Generazione con New-ADDCCloneConfigFile  
Il modulo Windows PowerShell di Active Directory contiene un nuovo cmdlet in Windows Server 2012:  
  
```  
New-ADDCCloneConfigFile  
```  
  
Eseguire il cmdlet nel controller di dominio proposto che si intende clonare. Il cmdlet supporta più argomenti e, quando viene usato, sottopone sempre a test il computer e l'ambiente in cui viene eseguito, a meno che non si specifichi l'argomento -offline.  
  
||||  
|-|-|-|  
|**ActiveDirectory**<br /><br />**Cmdlet**|**Argomenti**|**Spiegazione**|  
|**New-ADDCCloneConfigFile**|*<no argument specified>*|Crea un file DcCloneConfig.xml file vuoto nella directory di lavoro DSA (valore predefinito: %systemroot%\ntds)|  
||-CloneComputerName|Specifica il nome computer del controller di dominio. Tipo di dati stringa.|  
||-Path|Specifica la cartella in cui creare DcCloneConfig.xml. Se il percorso non viene specificato, viene usata la directory di lavoro DSA (valore predefinito: %systemroot%\ntds). Tipo di dati stringa.|  
||-SiteName|Specifica il nome del sito logico di Active Directory da aggiungere durante la creazione del computer clonato. Tipo di dati stringa.|  
||-IPv4Address|Specifica l'indirizzo IPv4 statico del computer clonato. Tipo di dati stringa.|  
||-IPv4SubnetMask|Specifica la subnet mask IPv4 statica del computer clonato. Tipo di dati stringa.|  
||-IPv4DefaultGateway|Specifica l'indirizzo del gateway predefinito IPv4 statico del computer clonato. Tipo di dati stringa.|  
||-IPv4DNSResolver|Specifica le voci DNS IPv4 statiche del computer clonato in un elenco di valori delimitati da virgole. Tipo di dati matrice. È possibile specificare fino a quattro voci.|  
||-PreferredWINSServer|Specifica l'indirizzo IPv4 statico del computer WINS primario. Tipo di dati stringa.|  
||-AlternateWINSServer|Specifica l'indirizzo IPv4 statico del computer WINS secondario. Tipo di dati stringa.|  
||-IPv6DNSResolver|Specifica le voci DNS IPv6 statiche del computer clonato in un elenco di valori delimitati da virgole. Non è possibile impostare informazioni statiche Ipv6 nella clonazione di controller di dominio virtualizzati. Tipo di dati matrice.|  
||-Offline|Non esegue i test di convalida e sovrascrive eventuali file dccloneconfig.xml esistenti. Non ha parametri.|  
||*-Static*|Obbligatorio se si specificano gli argomenti IP statici IPv4SubnetMask, IPv4SubnetMask o IPv4DefaultGateway. Non ha parametri.|  
  
Test eseguiti durante l'esecuzione in modalità online:  
  
-   L'emulatore PDC è Windows Server 2012 o versione successiva  
  
-   Il controller di dominio è un membro del gruppo Controller di dominio clonabili  
  
-   Il controller di dominio di origine non include applicazioni o servizi esclusi  
  
-   Il controller di dominio di origine non contiene già un file DcCloneConfig.xml nel percorso specificato  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewDCCloneConfig.png)  
  
### <a name="step-6---take-the-source-domain-controller-offline"></a>Passaggio 6 - Portare offline il controller di dominio di origine  
Non è possibile copiare un controller di dominio di origine in esecuzione, ma è necessario arrestarlo normalmente. Non clonare un controller di dominio arrestato da un'interruzione di corrente anomala.  
  
#### <a name="graphical-method"></a>Metodo grafico  
Usare il pulsante di arresto all'interno del controller di dominio in esecuzione oppure quello di Gestione Hyper-V.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_Shutdown.png)  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVShutdown.png)  
  
#### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
È possibile arrestare una macchina virtuale con uno dei cmdlet seguenti:  
  
```  
Stop-computer  
Stop-vm  
```  
  
Stop-computer è un cmdlet che supporta l'arresto dei computer indipendentemente dalla virtualizzazione ed è analogo all'utilità legacy Shutdown.exe. Stop-vm è un nuovo cmdlet del modulo Hyper-V di Windows PowerShell di Windows Server 2012 ed è equivalente alle opzioni di risparmio energia di Gestione Hyper-V. È utile negli ambienti di laboratorio in cui il controller di dominio opera spesso in una rete privata virtualizzata.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopComputer2.png)  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopVM.png)  
  
### <a name="step-7---copy-disks"></a>Passaggio 7 - Copiare i dischi  
Nella fase di copia è necessaria una scelta a livello amministrativo:  
  
-   Copiare i dischi manualmente, senza Hyper-V  
  
-   Esportare la macchina virtuale con Hyper-V  
  
-   Esportare i dischi uniti con Hyper-V  
  
È necessario che vengano copiati tutti i dischi della macchina virtuale, non solo l'unità di sistema. Se il controller di dominio di origine usa dischi differenze e si prevede di spostare il controller di dominio clonato in un altro host Hyper-V, è necessario scegliere l'esportazione.  
  
La copia manuale dei dischi è consigliata se il controller di dominio di origine ha *una* sola unità. L'operazione di esportazione/importazione è consigliata per le macchine virtuali con *più di una* unità o altre personalizzazioni hardware virtualizzate e complesse, come la presenza di diverse schede di interfaccia di rete.  
  
Se si copiano i dischi manualmente, eliminare eventuali snapshot prima di iniziare. Se si esporta la VM, eliminare gli snapshot prima dell'esportazione oppure dopo l'importazione dalla nuova VM.  
  
> [!WARNING]  
> Gli snapshot sono dischi differenze che possono ripristinare lo stato precedente di un controller di dominio. Se si clonasse un controller di dominio e quindi si ripristinasse lo snapshot pre-clonazione, alla fine si avrebbero controller di dominio duplicati nella foresta. Gli snapshot precedenti non hanno nessun valore in un controller di dominio appena clonato.  
  
#### <a name="manually-copying-disks"></a>Copia manuale dei dischi  
  
##### <a name="hyper-v-manager-method"></a>Metodo Gestione Hyper-V  
Usare lo snap-in Gestione Hyper-V per determinare quali dischi sono associati al controller di dominio di origine. Usare l'opzione Controlla per verificare se il controller di dominio usa dischi differenze, che richiedono anche la copia del disco padre.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVInspect.png)  
  
Per eliminare gli snapshot, selezionare una VM ed eliminare il sottoalbero snapshot.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDeleteSnapshot.gif)  
  
È quindi possibile copiare manualmente i file VHD o VHDX usando Esplora risorse, Xcopy.exe o Robocopy.exe. Non sono necessari passaggi speciali. Come procedura consigliata, cambiare i nomi dei file anche se vengono spostati in un'altra cartella.  
  
> [!NOTE]  
> Se la copia viene eseguita tra computer host su una rete LAN (a 1 Gbit o superiore), l'opzione **Xcopy.exe /J** copia i file VHD/VHDX a una velocità sensibilmente più elevata rispetto a qualsiasi altro strumento, ma con un utilizzo della larghezza di banda di molto superiore.  
  
##### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
Per determinare i dischi con Windows PowerShell, usare i moduli Hyper-V:  
  
```  
Get-vmidecontroller  
Get-vmscsicontroller  
Get-vmfibrechannelhba  
Get-vmharddiskdrive  
```  
  
È ad esempio possibile restituire tutti i dischi rigidi IDE di una VM chiamata **DC2** con l'esempio seguente:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_ReturnIDE.png)  
  
Se il percorso del disco punta a un file AVHD o AVHDX, si tratta di uno snapshot. Per eliminare gli snapshot associati a un disco e unirli in un VHD o VHDX reale, usare i cmdlet:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Ad esempio, per eliminare tutti gli snapshot da una VM chiamata DC2-SOURCECLONE:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_DelSnapshots.png)  
  
Per copiare i file con Windows PowerShell, usare il cmdlet seguente:  
  
```  
Copy-Item  
```  
  
Combinarlo con i cmdlet VM in pipeline per supportare l'automazione. La pipeline è un canale usato tra più cmdlet per passare i dati. Ad esempio, per copiare l'unità di un controller di dominio di origine offline chiamato DC2-SOURCECLONE in un nuovo disco c:\temp\copy.vhd senza la necessità di conoscere il percorso esatto della relativa unità di sistema:  
  
```  
Get-VMIdeController dc2-sourceclone | Get-VMHardDiskDrive | select-Object {copy-item -path $_.path -destination c:\temp\copy.vhd}  
```  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSCopyDrive.png)  
  
> [!IMPORTANT]  
> Non è possibile usare dischi passthrough con la clonazione perché non usano un disco virtuale ma un disco rigido fisico.  
  
> [!NOTE]  
> Per altre informazioni su ulteriori operazioni di Windows PowerShell con le pipeline, vedere [Piping e pipeline in Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
#### <a name="exporting-the-vm"></a>Esportazione della macchina virtuale  
In alternativa a copiare i dischi, è possibile esportare l'intera VM Hyper-V come copia. L'esportazione crea automaticamente una cartella denominata per la VM, contenente tutti i dischi e le informazioni di configurazione.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVExport.png)  
  
##### <a name="hyper-v-manager-method"></a>Metodo Gestione Hyper-V  
Per esportare una VM con Gestione Hyper-V:  
  
1.  Fare clic con il pulsante destro del mouse sul controller di dominio di origine, quindi scegliere **Esporta**.  
  
2.  Selezionare una cartella esistente come contenitore dell'esportazione.  
  
3.  Attendere che nella colonna Stato non venga più visualizzato il messaggio **Esportazione**.  
  
##### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
Per esportare una VM usando il modulo Hyper-V di Windows PowerShell, usare il cmdlet:  
  
```  
Export-vm  
```  
  
Ad esempio, per esportare una VM chiamata DC2-SOURCECLONE in una cartella C:\VM:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSExport.png)  
  
> [!NOTE]  
> Hyper-V di Windows Server 2012 supporta nuove funzionalità di esportazione e importazione che esulano dall'ambito di questa esercitazione. Per altre informazioni, vedere il sito TechNet.  
  
#### <a name="exporting-merged-disks-using-hyper-v"></a>Esportare i dischi uniti con Hyper-V  
L'opzione finale consiste nell'usare le opzioni di unione e conversione di dischi all'interno di Hyper-V, che consentono di effettuare una copia di una struttura del disco esistente, anche se si includono i file AVHD/AVHDX di snapshot, in un nuovo disco singolo. Come nello scenario di copia manuale dei dischi, questo è destinato principalmente a essere più semplice macchine virtuali che utilizzano solo una singola unità, ad esempio c:\\. L'unico vantaggio è che, a differenza della copia manuale, non richiede di eliminare prima gli snapshot. Questa operazione è necessariamente più lenta della semplice eliminazione degli snapshot e della copia dei dischi.  
  
##### <a name="hyper-v-manager-method"></a>Metodo Gestione Hyper-V  
Per creare un disco unito con Gestione Hyper-V:  
  
1.  Fare clic su **Modifica disco**.  
  
2.  Individuare il disco figlio minore. Se ad esempio si usa un disco differenze, il disco figlio è il figlio minore. Se la macchina virtuale ha uno o più snapshot, quello attualmente selezionato è il disco figlio minore.  
  
3.  Selezionare l'opzione **Unisci** per creare un singolo disco dall'intera struttura padre-figlio.  
  
4.  Selezionare un nuovo disco rigido virtuale e specificare un percorso. I file VHD/VHDX vengono riconciliati in una nuova unità portatile che non rischia di ripristinare precedenti snapshot.  
  
##### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
Per creare un disco unito da un set complesso di elementi padre con il modulo Hyper-V di Windows PowerShell, usare il cmdlet:  
  
```  
Convert-vm  
```  
  
Ad esempio, per esportare un'intera catena di snapshot di dischi di VM (questa volta senza includere dischi differenze) e il disco padre in un nuovo disco singolo chiamato DC4-CLONED.VHDX:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSConvertVhd.png)  
  
#### <a name="BKMK_Offline"></a>Aggiunta di XML per il disco del sistema non in linea  
Se il file Dccloneconfig.xml è stato copiato nel controller di dominio di origine in esecuzione, è adesso necessario copiare il file dccloneconfig.xml aggiornato nel disco di sistema copiato/esportato offline. A seconda delle applicazioni installate rilevate con Get-ADDCCloningExcludedApplicationList in precedenza, può anche essere necessario copiare il file CustomDCCloneAllowList.xml nel disco.  
  
I percorsi seguenti possono contenere il file DcCloneConfig.xml:  
  
1.  Directory di lavoro DSA  
  
2.  %windir%\ntds\  
  
3.  Supporti rimovibili di lettura/scrittura in ordine di lettera di unità, nella radice dell'unità  
  
Questi percorsi non sono configurabili. Dopo l'avvio della clonazione, questi percorsi vengono controllati nell'ordine specifico e viene usato il primo file DcCloneConfig.xml trovato, indipendentemente dal contenuto di altre cartelle.  
  
I percorsi seguenti possono contenere il file CustomDCCloneAllowList.xml:  
  
1.  HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters  
  
    AllowListFolder (*REG_SZ*)  
  
2.  Directory di lavoro DSA  
  
3.  %windir%\ntds\  
  
4.  Supporti rimovibili di lettura/scrittura in ordine di lettera di unità, nella radice dell'unità  
  
È possibile usare New-ADDCCloneConfigFile con l'argomento **-offline** (detto anche modalità offline) per creare il file DcCloneConfig.xml e posizionarlo nel percorso corretto. Gli esempi seguenti mostrano come eseguire New-ADDCCloneConfigFile in modalità offline.  
  
Per creare un controller di dominio clone denominato CloneDC1 in modalità non in linea, in un sito denominato "REDMOND" con indirizzo IPv4 statico, digitare:  
  
```  
New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS  
```  
  
Per creare un controller di dominio clone denominato Clone2 in modalità offline con impostazioni IPv4 statiche e IPv6 statiche, digitare:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS  
```  
  
Per creare un controller di dominio clone in modalità offline con impostazioni IPv4 statiche e IPv6 dinamiche e specificare più server DNS per le impostazioni del resolver DNS, digitare:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS   
```  
  
Per creare un controller di dominio clone denominato Clone1 in modalità offline con impostazioni IPv4 dinamiche e IPv6 statiche, digitare:  
  
```  
New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS  
```  
  
Per creare un controller di dominio clone in modalità offline con impostazioni IPv4 dinamiche e IPv6 dinamiche, digitare:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS  
  
```  
  
##### <a name="windows-explorer-method"></a>Metodo Esplora risorse  
Windows Server 2012 offre ora un'opzione grafica per montare file VHD e VHDX. In questo caso è necessario installare la funzionalità Esperienza desktop di Windows Server 2012.  
  
1.  Fare clic sul file VHD/VHDX appena copiato che contiene l'unità di sistema del controller di dominio o la cartella del percorso della directory di lavoro DSA, quindi scegliere **Monta** dal menu **Strumenti immagini disco**.  
  
2.  Nell'unità appena montata, copiare i file XML in un percorso valido. Potrebbero essere richieste le autorizzazioni per la cartella.  
  
3.  Fare clic sull'unità montata e scegliere **Espelli** dal menu **Strumenti immagini disco**.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVClickMountedDrive.png)  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDetailsMountedDrive.gif)  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVEjectMountedDrive.gif)  
  
##### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
In alternativa, è possibile montare il disco offline e copiare il file XML usando i cmdlet di Windows PowerShell:  
  
```  
mount-vhd  
get-disk  
get-partition  
get-volume  
Add-PartitionAccessPath  
Copy-Item  
```  
  
In questo modo è possibile acquisire il controllo completo del processo. È ad esempio possibile montare l'unità con una lettera specifica, copiare il file e smontare l'unità.  
  
```  
mount-vhd <disk path> -passthru -nodriveletter | get-disk | get -partition | get-volume | get-partition | where {$_.partition number -eq 2} | Add-PartitionAccessPath -accesspath <drive letter>  
  
copy-item <xml file path><destination path>\dccloneconfig.xml  
  
dismount-vhd <disk path>  
```  
  
Ad esempio:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSMountVHD.png)  
  
In alternativa, è possibile usare il nuovo cmdlet **Mount-DiskImage** per montare un file VHD (o ISO).  
  
### <a name="step-8---create-the-new-virtual-machine"></a>Passaggio 8 - Creare la nuova macchina virtuale  
Il passaggio di configurazione finale prima di avviare il processo di clonazione consiste nel creare una nuova macchina virtuale che usa i dischi del controller di dominio di origine copiato. In base alla selezione effettuata nella fase di copia dei dischi, sono disponibili due opzioni:  
  
1.  Associare una nuova VM al disco copiato  
  
2.  Importare la VM esportata  
  
#### <a name="associating-a-new-vm-with-copied-disks"></a>Associazione di una nuova macchina virtuale ai dischi copiati  
Se il disco di sistema è stato copiato manualmente, è necessario creare una nuova macchina virtuale usando il disco copiato. L'hypervisor imposta automaticamente l'ID di generazione VM quando viene creata una nuova VM. Non è necessario effettuare modifiche alla configurazione della VM o dell'host Hyper-V.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVConnectVHD.gif)  
  
##### <a name="hyper-v-manager-method"></a>Metodo Gestione Hyper-V  
  
1.  Creare una nuova macchina virtuale.  
  
2.  Specificare il nome, la memoria e la rete della macchina virtuale.  
  
3.  Nella pagina Connessione disco rigido specificare il disco di sistema copiato.  
  
4.  Completare la procedura guidata per creare la VM.  
  
Se ci sono più dischi, schede di rete o altre personalizzazioni, configurare questi componenti prima di avviare il controller di dominio. Per le macchine virtuali complesse, è consigliabile scegliere il metodo "esportazione-importazione" per la copia dei dischi.  
  
##### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
È possibile usare il modulo Hyper-V di Windows PowerShell per automatizzare la creazione di VM in Windows Server 2012, usando il cmdlet seguente:  
  
```  
New-VM  
```  
  
Ad esempio, in questo caso viene creata la VM DC4-CLONEDFROMDC2, con 1 GB di RAM, avvio dal file c:\vm\dc4-systemdrive-clonedfromdc2.vhd e uso della rete virtuale 10.0:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewVM.png)  
  
#### <a name="import-vm"></a>Importare VM  
Se in precedenza è stata esportata la VM, a questo punto è necessario reimportarla come copia. L'XML esportato viene usato per ricreare il computer con tutte le precedenti impostazioni, unità, reti e impostazioni di memoria.  
  
Se si intende creare altre copie della stessa VM esportata, è possibile creare tutte quelle necessarie. Quindi usare Importa per ogni copia.  
  
> [!IMPORTANT]  
> È importante usare l'opzione **Copia** , in quanto l'esportazione preserva tutte le informazioni dell'origine. L'importazione del server con l'opzione **Sposta** o **Sul posto** causa collisioni delle informazioni, se eseguita nello stesso server host Hyper-V.  
  
##### <a name="hyper-v-manager-method"></a>Metodo Gestione Hyper-V  
Per importare con lo snap-in Gestione Hyper-V:  
  
1.  Fare clic su **Importa macchina virtuale**.  
  
2.  Nella pagina **Individua cartella** selezionare il file di definizione della VM esportata usando il pulsante Sfoglia.  
  
3.  Nella pagina **Seleziona macchina virtuale** fare clic sul computer di origine.  
  
4.  Nella pagina **Scegli il tipo di importazione** fare clic su **Copia macchina virtuale (crea nuovo ID univoco)** , quindi su **Fine**.  
  
5.  Rinominare la VM importata, se l'importazione viene eseguita nello stesso host Hyper-V. Dovrà avere lo stesso nome del controller di dominio di origine esportato.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportLocateFolder.png)  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportSelectVM.png)  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportChooseType.gif)  
  
Ricordarsi di rimuovere eventuali snapshot importati usando lo snap-in Gestione Hyper-V:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportDelSnap.gif)  
  
> [!WARNING]  
> L'eliminazione di eventuali snapshot importati è di importanza critica. Se applicati, gli snapshot riporterebbero il controller di dominio clonato allo stato di un controller di dominio precedente, possibilmente attivo, causando un errore di replica, informazioni IP duplicate e altre interruzioni.  
  
##### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
È possibile usare il modulo Hyper-V di Windows PowerShell per automatizzare l'importazione di VM in Windows Server 2012, usando il cmdlet seguente:  
  
```  
Import-VM  
Rename-VM  
```  
  
Ad esempio, in questo caso la VM DC2-CLONED esportata viene importata usando il file XML determinato automaticamente, quindi viene immediatamente rinominata con il nuovo nome VM DC5-CLONEDFROMDC2:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSImportVM.png)  
  
Ricordarsi di rimuovere eventuali snapshot importati, usando i cmdlet seguenti:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Ad esempio:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSGetVMSnap.png)  
  
> [!WARNING]
> Assicurarsi che, quando si importa il computer, gli indirizzi MAC statici non siano stati assegnati al controller di dominio di origine. Se viene clonato un computer di origine con un indirizzo MAC statico, i computer copiati non invieranno né riceveranno correttamente traffico di rete. Impostare un nuovo indirizzo MAC univoco, statico o dinamico, se questo è il caso. È possibile verificare se una VM usa indirizzi MAC statici con il comando:  
> 
> **Get-VM -VMName**   
>  ***test-vm* | Get-VMNetworkAdapter | fl \\** *  
  
### <a name="step-9---clone-the-new-virtual-machine"></a>Passaggio 9 - Creare la nuova macchina virtuale  
Facoltativamente, prima di iniziare la clonazione, avviare il controller di dominio di origine offline. Assicurarsi che l'emulatore PDC sia online, in ogni caso.  
  
Per iniziare la clonazione, avviare semplicemente la nuova macchina virtuale. Il processo inizia automaticamente e al termine il controller di dominio viene riavviato automaticamente.  
  
> [!IMPORTANT]  
> È sconsigliabile mantenere i controller di dominio disattivati per un periodo prolungato di tempo e, se il clone viene aggiunto allo stesso sito del controller di dominio di origine, può essere necessario più tempo per creare la topologia iniziale di replica all'interno e tra siti se il controller di dominio di origine è offline.  
  
Se si usa Windows PowerShell per avviare una VM, il nuovo cmdlet del modulo Hyper-V è il seguente:  
  
```  
Start-VM  
```  
  
Ad esempio:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSStartVM.png)  
  
Dopo il riavvio al termine della clonazione, il computer è un controller di dominio ed è possibile effettuare l'accesso normalmente per verificarne il regolare funzionamento. Se sono presenti errori, il server è impostato per essere avviato in modalità ripristino servizi directory per l'analisi della causa.  
  
## <a name="BKMK_VDCSafeRestore"></a>Protezioni relative alla virtualizzazione  
A differenza della clonazione di controller di dominio virtualizzati, le misure di sicurezza di Windows Server 2012 non prevedono passaggi di configurazione. La funzionalità viene eseguita senza intervento dell'utente, purché vengano rispettate alcune semplici condizioni:  
  
-   L'hypervisor supporta l'ID di generazione VM.  
  
-   Esiste un valido controller di dominio partner da cui un controller di dominio ripristinato può replicare le modifiche in modo non autorevole.  
  
### <a name="validate-the-hypervisor"></a>Convalidare l'hypervisor  
Assicurarsi che il controller di dominio di origine sia in esecuzione in un hypervisor supportato consultando la documentazione del fornitore. I controller di dominio virtualizzati sono indipendenti dall'hypervisor e non richiedono Hyper-V.  
  
Rivedere la precedente sezione [Requisiti della piattaforma](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_PlatformReqs) per informazioni sul supporto noto dell'ID di generazione VM.  
  
Se si esegue la migrazione delle VM da un hypervisor di origine a un hypervisor di destinazione diverso, le misure di sicurezza della virtualizzazione possono essere o meno attivate a seconda che gli hypervisor supportino o meno l'ID di generazione VM, come illustrato nella tabella seguente.  
  
|Hypervisor di origine|Hypervisor di destinazione|Risultato|  
|---------------------|---------------------|----------|  
|Supporta l'ID di generazione VM|Non supporta l'ID di generazione VM|Misure di sicurezza non attivate (se è presente un file DCCloneConfigFile.xml, il controller di dominio verrà avviato in modalità ripristino servizi directory)|  
|Non supporta l'ID di generazione VM|Supporta l'ID di generazione VM|Misure di sicurezza attivate|  
|Supporta l'ID di generazione VM|Supporta l'ID di generazione VM|Misure di sicurezza non attivate, perché la definizione della VM non è cambiata, il che significa che l'ID di generazione VM rimane inalterato|  
  
### <a name="validate-the-replication-topology"></a>Convalidare la topologia di replica.  
Le misure di sicurezza della virtualizzazione avviano una replica in ingresso non autorevole per il delta della replica di Active Directory, oltre a una risincronizzazione non autorevole di tutto il contenuto SYSVOL. In questo modo il controller di dominio viene restituito da uno snapshot con la piena funzionalità ed è alla fine coerente con il resto dell'ambiente.  
  
Questa nuova funzionalità prevede diversi requisiti e limitazioni:  
  
-   Un controller di dominio ripristinato deve essere in grado di contattare un controller di dominio scrivibile  
  
-   I controller di dominio di un dominio non devono essere ripristinati tutti simultaneamente  
  
-   Le eventuali modifiche derivanti da un controller di dominio ripristinato che non sono state replicate in uscita dopo l'acquisizione di uno snapshot vanno perse per sempre  
  
Anche se la sezione sulla risoluzione dei problemi illustra questi scenari, i dettagli seguenti assicurano che non venga creata una topologia che potrebbe causare problemi.  
  
#### <a name="writable-domain-controller-availability"></a>Disponibilità di controller di dominio scrivibili  
Se ripristinato, un controller di dominio deve avere connettività con un controller di dominio scrivibile. Un controller di dominio di sola lettura non può inviare il delta degli aggiornamenti. La topologia dovrebbe essere già corretta per questo scenario, in quanto un controller di dominio scrivibile necessita sempre di un partner scrivibile. Se tuttavia tutti i controller di dominio scrivibili vengono ripristinati simultaneamente, nessuno di essi può trovare un'origine valida. Lo stesso vale se i controller di dominio scrivibili sono offline per manutenzione o altrimenti irraggiungibili tramite la rete.  
  
#### <a name="simultaneous-restore"></a>Ripristino simultaneo  
Non ripristinare tutti i controller di dominio di un singolo dominio simultaneamente Se tutti gli snapshot vengono ripristinati contemporaneamente, la replica di Active Directory funziona normalmente, mentre quella di SYSVOL si arresta. L'architettura di ripristino del servizio Replica file e di Replica DFS richiede l'impostazione della relativa istanza di replica sulla modalità di sincronizzazione non autorevole. Se tutti i controller di dominio vengono ripristinati contemporaneamente e ognuno di essi si contrassegna in modo non autorevole per SYSVOL, proveranno tutti a sincronizzare Criteri di gruppo e script di un partner autorevole. A quel punto, però, anche i partner sono tutti non autorevoli.  
  
> [!IMPORTANT]  
> Se tutti i controller di dominio vengono ripristinati contemporaneamente, consultare gli articoli seguenti per impostare un controller di dominio, in genere l'emulatore PDC, come autorevole, in modo che gli altri possano tornare al normale funzionamento:  
>   
> [Utilizzo della chiave del Registro di sistema BurFlags per reinizializzare i set di repliche del servizio Replica file](https://support.microsoft.com/kb/290762)  
>   
> [Come forzare una sincronizzazione autorevole e non autorevole di DFSR-replicated SYSVOL (ad esempio, "D4/D2" per FRS)](https://support.microsoft.com/kb/2218556)  
  
> [!WARNING]  
> Non eseguire tutti i controller di dominio in una foresta o in un dominio nello stesso host dell'hypervisor. In caso contrario, si introduce un singolo punto di errore che danneggia Servizi di dominio Active Directory, Exchange, SQL e altre operazioni aziendali ogni volta che l'hypervisor passa offline. Questa situazione non è diversa dall'uso di un singolo controller di dominio per un intero dominio o foresta. Più controller di dominio in più piattaforme assicurano ridondanza e tolleranza di errore.  
  
#### <a name="post-snapshot-replication"></a>Replica post-snapshot  
Non ripristinare gli snapshot finché tutte le modifiche di origine locale ed effettuate dopo la creazione degli snapshot non vengono replicate in uscita. Le modifiche di origine vanno perse per sempre se altri controller di dominio non le hanno già ricevute tramite la replica.  
  
Usare Repadmin.exe per mostrare eventuali modifiche in uscite non replicate tra un controller di dominio e i relativi partner:  
  
1.  Restituire i nomi dei partner del controller di dominio e i GUID degli oggetti DSA con:  
  
    ```  
    Repadmin.exe /showrepl <DC Name of the partner> /repsto  
    ```  
  
2.  Restituire la replica in ingresso in sospeso del controller di dominio partner al controller di dominio da ripristinare:  
  
    ```  
    Repadmin.exe /showchanges < Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare>  
    ```  
  
In alternativa, esaminare solo il numero di modifiche non replicate:  
  
```  
Repadmin.exe /showchanges <Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare> /statistics  
```  
  
Ad esempio (con l'output modificato per la leggibilità e le voci importanti in ***corsivo***), ecco le partnership di replica di DC4:  
  
```  
C:\>repadmin.exe /showrepl dc4.corp.contoso.com /repsto  
  
Default-First-Site-Name\DC4  
DSA Options: IS_GC  
Site Options: (none)  
DSA object GUID: 5d083398-4bd3-48a4-a80d-fb2ebafb984f  
DSA invocationID: 730fafec-b6d4-4911-88f2-5b64e48fc2f1  
  
==== OUTBOUND NEIGHBORS FOR CHANGE NOTIFICATIONS ============  
  
DC=corp,DC=contoso,DC=com  
    Default-First-Site-Name\DC3 via RPC  
        DSA object GUID: f62978a8-fcf7-40b5-ac00-40aa9c4f5ad3  
        Last attempt @ 2011-11-11 15:04:12 was successful.  
    Default-First-Site-Name\DC2 via RPC  
        DSA object GUID: 3019137e-d223-4b62-baaa-e241a0c46a11  
        Last attempt @ 2011-11-11 15:04:15 was successful.  
```  
  
A questo punto è chiaro che la replica avviene con DC2 e DC3. Viene quindi mostrato l'elenco di modifiche che DC2 dichiara di non avere ancora ricevuto da DC4 e si noti che c'è un nuovo gruppo:  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com  
  
==== SOURCE DSA: (null) ====  
Objects returned: 1  
(0) add CN=newgroup4,CN=Users,DC=corp,DC=contoso,DC=com  
    1> parentGUID: 55fc995a-04f4-4774-b076-d6a48ac1af99  
    1> objectGUID: 96b848a2-df1d-433c-a645-956cfbf44086  
    2> objectClass: top; group  
    1> instanceType: 0x4 = ( WRITE )  
    1> whenCreated: 11/11/2011 3:03:57 PM Eastern Standard Time  
```  
  
Viene inoltre testato l'altro partner per verificare che non sia stato già replicato.  
  
In alternativa, se non è importante sapere quali oggetti sono stati replicati ma solo se ci sono oggetti in sospeso, è possibile usare l'opzione **/statistics**:  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com /statistics  
  
***********************************************  
********* Grand total *************************  
Packets:              1  
Objects:              1Object Additions:     1Object Modifications: 0Object Deletions:     0Object Moves:         0Attributes:           12Values:               13  
```  
  
> [!IMPORTANT]  
> Testare tutti i partner scrivibili se vengono notati errori o repliche in sospeso. Purché almeno uno sia convergente, è in genere possibile ripristinare lo snapshot, in quanto la replica transitiva alla fine si riconcilia con gli altri server.  
>   
> Assicurarsi di prendere nota di eventuali errori di replica mostrati da /showchanges e non continuare finché non saranno risolti.  
  
### <a name="windows-powershell-snapshot-cmdlets"></a>Cmdlet di Windows PowerShell per snapshot  
I seguenti cmdlet del modulo Hyper-V di Windows PowerShell forniscono funzionalità di snapshot in Windows Server 2012:  
  
```  
Checkpoint-VM  
Export-VMSnapshot  
Get-VMSnapshot  
Remove-VMSnapshot  
Rename-VMSnapshot  
Restore-VMSnapshot  
```  
  


