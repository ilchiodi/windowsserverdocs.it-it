---
ms.assetid: b146f47e-3081-4c8e-bf68-d0f993564db2
title: Configurazione e distribuzione del Controller di dominio virtualizzati
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dcb377cef003458bcdf2e4b3564167cee4310020
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-deployment-and-configuration"></a>Configurazione e distribuzione del Controller di dominio virtualizzati

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento si applica:  
  
-   [Considerazioni sull'installazione di](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_InstallConsiderations)  
  
    Questo include i requisiti della piattaforma e altri vincoli importanti.  
  
-   [Dominio virtualizzati la clonazione del Controller](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCCloning)  
  
    Illustra in dettaglio il controller di dominio virtualizzati intero processo di clonazione.  
  
-   [Ripristino sicuro di Controller di dominio virtualizzati](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCSafeRestore)  
  
    Illustra in dettaglio le verifiche effettuate durante il ripristino sicuro di controller di dominio virtualizzati.  
  
## <a name="BKMK_InstallConsiderations"></a>Considerazioni sull'installazione di  
Esiste un ruolo speciale o l'installazione delle funzionalità per il controller di dominio virtualizzati. tutti i controller di dominio contengono automaticamente funzionalità di ripristino di clonazione e sicuro. È possibile rimuovere o disabilitare queste funzionalità.  
  
Uso di Windows Server 2012 controller di dominio richiede un AD DS Schema versione 56 o successiva di Windows Server 2012 e livello di funzionalità foresta uguale o superiore a Windows Server 2003 nativo.  
  
Entrambi i controller di dominio scrivibili e di sola lettura supportano tutti gli aspetti dei controller di dominio virtualizzati, come i cataloghi globali e dei ruoli FSMO.  
  
> [!IMPORTANT]  
> Titolare del ruolo FSMO dell'emulatore PDC deve essere online quando viene avviata la clonazione.  
  
### <a name="BKMK_PlatformReqs"></a>Requisiti della piattaforma  
La clonazione del Controller di dominio virtualizzati richiede:  
  
-   Ruolo FSMO dell'emulatore PDC ospitato in un controller di dominio di Windows Server 2012  
  
-   Emulatore PDC disponibile durante le operazioni di clonazione  
  
Sia clonazione e il ripristino sicuro richiedono:  
  
-   Guest virtualizzati di Windows Server 2012  
  
-   Piattaforma host di virtualizzazione supporta l'ID di generazione VM (VMGID)  
  
Esaminare la tabella seguente per prodotti di virtualizzazione e se supportano o meno i controller di dominio virtualizzati e ID di generazione VM.  
  
|||  
|-|-|  
|**Prodotto di virtualizzazione**|**Supporto dei controller di dominio virtualizzati e VMGID**|  
|**Server Microsoft Windows Server 2012 con funzionalità di Hyper-V**|Sì|  
|**Microsoft Windows Server 2012 Hyper-V Server**|Sì|  
|**Funzionalità di Microsoft Windows 8 con Hyper-V Client**|Sì|  
|**Windows Server 2008 R2 e Windows Server 2008**|No|  
|**Soluzioni di virtualizzazione non Microsoft**|Contattare il fornitore|  
  
Anche se Microsoft supporta Windows 7 Virtual PC, Virtual PC 2007, Virtual PC 2004 e Virtual Server 2005, non possono eseguire guest a 64 bit né supportano l'ID di generazione VM.  
  
Per informazioni sui prodotti di virtualizzazione di terze parti e orientamento loro supporto con controller di dominio virtualizzati, contattare direttamente il fornitore.  
  
Per ulteriori informazioni, vedere i criteri di supporto per [software Microsoft in esecuzione in software di virtualizzazione hardware non Microsoft](https://support.microsoft.com/kb/897615).  
  
### <a name="critical-caveats"></a>Avvertenze importanti  
I controller di dominio virtualizzati *non* supportano il ripristino sicuro delle operazioni seguenti:  
  
-   File VHD e VHDX copiati manualmente su file VHD esistenti  
  
-   File VHD e VHDX ripristinati con software di backup di file o di backup completo del disco  
  
> [!NOTE]  
> I file VHDX sono una novità per Windows Server 2012 Hyper-V.  
  
Nessuna di queste operazioni è inclusa nella semantica dell'ID di generazione VM e pertanto non cambia l'ID di generazione VM. Ripristino dominio controller tramite questi metodi può comportare un rollback degli USN e uno in quarantena il controller di dominio o introdurre oggetti residui e l'esigenza di operazioni di pulizia a livello di foresta.  
  
> [!WARNING]  
> Ripristino sicuro di controller di dominio virtualizzati non è intendersi come sostituto di backup dello stato del sistema e il Cestino per Active Directory.  
>   
> Dopo il ripristino di uno snapshot, i delta delle modifiche in precedenza non replicate provenienti dal controller di dominio dopo lo snapshot vanno persi definitivamente. Ripristino sicuro implementa il ripristino non autorevole automatizzato per evitare di quarantena di controller di dominio accidentali *solo*.  
  
Per ulteriori informazioni su bolle USN e oggetti residui, vedere [operazioni di risoluzione dei problemi di Active Directory non riuscite con errore 8606: "attributi insufficienti assegnati per creare un oggetto"](https://support.microsoft.com/kb/2028495).  
  
## <a name="BKMK_VDCCloning"></a>Dominio virtualizzati la clonazione del Controller  
Esistono una serie di passaggi per la clonazione del controller di dominio virtualizzati, sia che si usino strumenti grafici o Windows PowerShell e fasi. In generale, le tre fasi sono:  
  
**Preparare l'ambiente di**  
  
-   Passaggio 1: Verificare che l'hypervisor supporti l'ID di generazione VM e quindi la clonazione  
  
-   Passaggio 2: Verificare che il ruolo emulatore PDC è ospitato da un controller di dominio che esegue Windows Server 2012 e che sia online e raggiungibile dal controller di dominio clonato durante la clonazione.  
  
**Preparare il controller di dominio di origine**  
  
-   Passaggio 3: Autorizzare il controller di dominio di origine per la clonazione  
  
-   Passaggio 4: Rimuovere servizi o programmi incompatibili o aggiungerli al file CustomDCCloneAllowList.xml.  
  
-   Passaggio 5: Creare il file DCCloneConfig.xml  
  
-   Passaggio 6: Portare offline il controller di dominio di origine  
  
**Creare il controller di dominio clonato**  
  
-   Passaggio 7: Copiare o esportare la macchina virtuale di origine e aggiungere il XML, se non è già stato copiato  
  
-   Passaggio 8: Creare una nuova macchina virtuale dalla copia  
  
-   Passaggio 9: Avviare la nuova macchina virtuale per iniziare la clonazione  
  
Non esistono differenze procedurale nell'operazione quando si utilizzano strumenti grafici, ad esempio la Console di gestione di Hyper-V o strumenti da riga di comando come Windows PowerShell, in modo che i passaggi vengono presentati solo una volta con entrambe le interfacce. In questo argomento vengono forniti esempi di Windows PowerShell per consentire di esplorare l'automazione end-to-end del processo di clonazione; non sono necessari per i passaggi. Esiste nessuno strumento di gestione grafica per i controller di dominio virtualizzati incluso in Windows Server 2012.  
  
Ci sono diversi punti della procedura in cui sono disponibili opzioni per la creazione del computer clonato e come aggiungere i file xml; questi passaggi sono illustrati in dettaglio di seguito. Il processo non è modificabile.  
  
Il diagramma seguente illustra il processo clonazione controller dominio virtualizzato, in cui il dominio esiste già.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_CloningProcessFlow.png)  
  
### <a name="step-1---validate-the-hypervisor"></a>Passaggio 1 - convalidare l'Hypervisor  
Assicurarsi che il controller di dominio di origine è in esecuzione in un hypervisor supportato consultando la documentazione del fornitore. Controller di dominio virtualizzati sono indipendenti dall'hypervisor e non richiedono Hyper-V.  
  
Se l'hypervisor è Microsoft Hyper-V, assicurarsi che sia in esecuzione in Windows Server 2012. È possibile convalidare questo mediante la gestione dei dispositivi  
  
Apri **Devmgmt.msc** ed esaminare **dispositivi di sistema** per installare Microsoft Hyper-V dispositivi e driver. Il dispositivo di sistema specifico necessario per un controller di dominio virtualizzati è il **Microsoft Hyper-V Generation Counter** (driver: vmgencounter.sys).  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVVMGenIDCounter.png)  
  
### <a name="step-2---verify-the-pdce-fsmo-role"></a>Passaggio 2: verificare il ruolo FSMO PDCE  
Prima di tentare di clonare un controller di dominio, è necessario verificare che il controller di dominio che ospita il FSMO dell'emulatore Controller di dominio primario esegue Windows Server 2012. L'emulatore PDC (emulatore PDC) è necessario per diversi motivi:  
  
1.  L'emulatore PDC crea speciale **controller di dominio clonabili** di gruppo e imposta la relativa autorizzazione sulla radice del dominio per consentire a un controller di dominio eseguire l'autoclonazione.  
  
2.  Il controller di dominio clonazione contatta l'emulatore PDC direttamente tramite il protocollo DRSUAPI RPC, per creare oggetti computer per il controller di dominio clone.  
  
    > [!NOTE]  
    > Windows Server 2012 estende esistente Directory Replication Service (DRS) Remote Protocol (UUID **E3514235-4B06-11D1-AB04-00C04FC2DCD2**) per includere un nuovo metodo RPC **IDL_DRSAddCloneDC** (Opnum **28**). Il **IDL_DRSAddCloneDC** metodo crea un nuovo oggetto controller di dominio copiando gli attributi di un oggetto controller di dominio esistente.  
    >   
    > Gli stati di un controller di dominio sono costituiti da computer, server, impostazioni NTDS, servizio Replica file, DFSR e connessione, gli oggetti gestiti per ogni controller di dominio. Quando Duplica un oggetto, questo metodo RPC sostituisce tutti i riferimenti al controller di dominio originale con oggetti corrispondenti del nuovo controller di dominio. Il chiamante deve avere il controllo di accesso destra DS-Clone-Domain-Controller nel contesto dei nomi di dominio.  
    >   
    > Utilizzo di questo nuovo metodo richiede sempre l'accesso diretto al controller di dominio dell'emulatore PDC da parte del chiamante.  
    >   
    > Poiché questo metodo RPC è nuovo, il software di analisi di rete richiede parser aggiornati che includano i campi per la nuova voce Opnum 28 nell'UUID E3514235-4B06 - 11D di esistente 1-AB04-00C04FC2DCD2. In caso contrario, è possibile analizzare il traffico.  
    >   
    > Per ulteriori informazioni, vedere [4.1.29 IDL_DRSAddCloneDC (Opnum 28)](https://msdn.microsoft.com/en-us/library/hh554213(v=prot.13).aspx).  
  
***Ciò significa anche quando si utilizzano le reti non completamente instradate, la clonazione di controller di dominio virtualizzati richiede segmenti di rete con accesso all'emulatore PDC***. È accettabile spostare un controller di dominio clonato in una rete diversa dopo la clonazione, come per i controller di dominio fisici, purché si presti attenzione ad aggiornare le informazioni sul sito logico di Active Directory.  
  
> [!IMPORTANT]  
> Quando si clona un dominio che contiene solo un singolo controller di dominio, è necessario assicurarsi che di controller di dominio di origine è tornata in linea prima di iniziare le copie del clone. Un dominio di produzione dovrebbe sempre contenere almeno due controller di dominio.  
  
#### <a name="active-directory-users-and-computers-method"></a>Metodo utenti di Active Directory e computer  
  
1.  Utilizzando lo snap-in Dsa.msc, fare clic con il pulsante destro del dominio e fare clic su **master operazioni**. Si noti il controller di dominio denominato nella scheda PDC e chiudere la finestra di dialogo.  
  
2.  Fare doppio clic su oggetto computer del controller di dominio e fare clic su **proprietà**, quindi verificare le informazioni del sistema operativo.  
  
#### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
È possibile combinare i seguenti cmdlet di modulo di Active Directory Windows PowerShell per restituire la versione dell'emulatore PDC:  
  
```  
Get-adddomaincontroller  
Get-adcomputer  
```  
  
Se non viene specificato il dominio, questi cmdlet considerano il dominio del computer in cui è stato eseguito.  
  
Il comando seguente restituisce l'emulatore PDC e sul sistema operativo:  
  
```  
get-adcomputer(Get-ADDomainController -Discover -Service "PrimaryDC").name -property * | format-list dnshostname,operatingsystem,operatingsystemversion  
```  
  
L'esempio seguente viene illustrato che specifica il nome di dominio e filtrare le proprietà restituite prima della pipeline di Windows PowerShell:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PDCOSInfo.png)  
  
### <a name="step-3---authorize-a-source-dc"></a>Passaggio 3 - autorizzare un controller di dominio di origine  
Il controller di dominio di origine deve avere il controllo diritto di accesso (Auto) **consentire un controller di dominio di creare un clone di se stesso** al vertice del contesto dei nomi di dominio. Per impostazione predefinita, il ben noto gruppo **controller di dominio clonabili** ha questa autorizzazione e non contiene membri. L'emulatore PDC crea questo gruppo quando il ruolo FSMO viene trasferito a un controller di dominio di Windows Server 2012.  
  
#### <a name="active-directory-administrative-center-method"></a>Metodo centro di amministrazione di Active Directory  
  
1.  Avviare Dsac.exe e passare al controller di dominio di origine, quindi aprire la pagina dei dettagli.  
  
2.  Nel **membro di** sezione, aggiungere il **controller di dominio clonabili** gruppo per tale dominio.  
  
#### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
È possibile combinare i cmdlet del modulo di Active Directory Windows PowerShell seguenti **get-adcomputer** e **aggiungere-adgroupmember** per aggiungere un controller di dominio per il **controller di dominio clonabili** gruppo:  
  
```  
Get-adcomputer <dc name> | %{add-adgroupmember "cloneable domain controllers" $_.samaccountname}  
```  
  
Ad esempio, questo aggiunge DC1 server al gruppo, senza la necessità di specificare il nome distinto del membro del gruppo:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_AddDcToGroup.png)  
  
#### <a name="rebuilding-default-permissions"></a>Ricreare le autorizzazioni predefinite  
Se si rimuove questa autorizzazione dall'intestazione di dominio, la clonazione non riesce. È possibile ricreare l'autorizzazione utilizzando il centro di amministrazione di Active Directory o Windows PowerShell.  
  
##### <a name="active-directory-administrative-center-method"></a>Metodo centro di amministrazione di Active Directory  
  
1.  Apri **centro di amministrazione di Active Directory**, fare doppio clic su nell'intestazione di dominio, fare clic su **proprietà**, fare clic su di **estensioni** scheda, fare clic su **sicurezza**e quindi fare clic su **avanzate**. Fare clic su **solo questo oggetto**.  
  
2.  Fare clic su **Aggiungi**, in **immettere il nome dell'oggetto da selezionare**, digitare il nome del gruppo **controller di dominio clonabili.**  
  
3.  In autorizzazioni fare clic su **consentire un controller di dominio di creare un clone di se stesso**, quindi fare clic su **OK**.  
  
> [!NOTE]  
> È inoltre possibile rimuovere l'autorizzazione predefinita e aggiungere singoli controller di dominio. In questo modo è probabile che causano problemi di manutenzione, tuttavia, in cui vengono informati di questa personalizzazione nuovi amministratori. Modifica l'impostazione predefinita non aumenta la sicurezza ed è sconsigliata.  
  
##### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
Utilizzare i comandi seguenti in un prompt di console di Windows PowerShell con privilegi elevati di amministratore. Questi comandi rilevano il nome di dominio e aggiungono di nuovo le autorizzazioni predefinite:  
  
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
  
In alternativa, eseguire il codice di esempio [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms) in una console di Windows PowerShell, avviata come amministratore con privilegi elevato in un controller di dominio del dominio interessato. Le autorizzazioni vengono impostate automaticamente. L'esempio si trova nell'appendice di questo modulo.  
  
### <a name="step-4---remove-incompatible-applications-or-services-if-not-using-customdccloneallowlistxml"></a>Passaggio 4 - rimuovere applicazioni o servizi incompatibili (se non si usa CustomDCCloneAllowList.xml)  
Tutti i programmi o servizi restituiti in precedenza da Get-ADDCCloningExcludedApplicationList - *e non aggiunti a CustomDCCloneAllowList.xml* -deve essere rimossa prima della clonazione. Disinstallazione dell'applicazione o servizio è il metodo consigliato.  
  
> [!WARNING]  
> Qualsiasi programma incompatibile o servizio non disinstallati né aggiunti a CustomDCCloneAllowList.xml impedisce la clonazione.  
  
Utilizzare il cmdlet Get-AdComputerServiceAccount per individuare eventuali account del servizio gestito autonomo (MSAs) nel dominio e se il computer utilizza uno di essi. Se è installato eventuali account Microsoft, utilizzare il cmdlet Uninstall-ADServiceAccount per rimuovere l'account di servizio installato in locale. Dopo aver portato offline il controller di dominio di origine nel passaggio 6, è possibile aggiungere nuovamente l'account del servizio gestito usando Install-ADServiceAccount quando il server è tornata in linea. Per ulteriori informazioni, vedere [Uninstall-ADServiceAccount](https://technet.microsoft.com/en-us/library/hh852310).  
  
> [!IMPORTANT]  
> Servizio gestito autonomi e - primo rilasciate in Windows Server 2008 R2 - sono stati sostituiti in Windows Server 2012 con questi account. Questi account supportano la clonazione.  
  
### <a name="step-5---create-dccloneconfigxml"></a>Passaggio 5: creare il file DCCloneConfig.xml  
Il file DcCloneConfig.xml è necessario per la clonazione di controller di dominio. Il relativo contenuto consente di specificare dettagli univoci, come il nuovo nome computer e l'indirizzo IP.  
  
Il file CustomDCCloneAllowList.xml è facoltativo, a meno che non si installano applicazioni o servizi Windows potenzialmente incompatibili nel controller di dominio di origine. I file richiedono precise di denominazione, formattazione e posizionamento; in caso contrario, la clonazione ha esito negativo.  
  
Per questo motivo, utilizzare sempre i cmdlet di Windows PowerShell per creare i file XML e posizionarli nel percorso corretto.  
  
#### <a name="generating-with-new-addccloneconfigfile"></a>Generazione con New-ADDCCloneConfigFile  
Il modulo di Windows PowerShell per Active Directory contiene un nuovo cmdlet in Windows Server 2012:  
  
```  
New-ADDCCloneConfigFile  
```  
  
Eseguire il cmdlet nel controller di dominio proposto che si intende clonare. Il cmdlet supporta più argomenti e se usato, sottopone sempre a test il computer e l'ambiente in cui viene eseguito a meno che non si specifica l'argomento - offline.  
  
||||  
|-|-|-|  
|**Active Directory**<br /><br />**Cmdlet**|**Argomenti**|**Spiegazione**|  
|**New-ADDCCloneConfigFile**|*<no argument specified>*|Crea un file DcCloneConfig.xml file vuoto nella Directory di lavoro DSA (impostazione predefinita: systemroot%\ntds)|  
||-CloneComputerName|Specifica il nome del computer controller di dominio clone. Tipo di dati stringa.|  
||-Path|Specifica la cartella per creare il file DcCloneConfig.xml. Se non specificato, scrive in Directory di lavoro DSA (impostazione predefinita: systemroot%\ntds). Tipo di dati stringa.|  
||-SiteName|Specifica il nome di sito logico di Active Directory da aggiungere durante la creazione dell'account del computer clonato. Tipo di dati stringa.|  
||-IPv4Address|Specifica l'indirizzo IPv4 statico del computer clonato. Tipo di dati stringa.|  
||-IPv4SubnetMask|Specifica la subnet mask IPv4 statica del computer clonato. Tipo di dati stringa.|  
||-IPv4DefaultGateway|Specifica l'indirizzo gateway predefinito IPv4 statico del computer clonato. Tipo di dati stringa.|  
||-IPv4DNSResolver|Specifica le voci DNS IPv4 statiche del computer clonato in un elenco delimitato da virgole. Tipo di dati matrice. Fino a quattro voci possono essere fornite.|  
||-PreferredWINSServer|Specifica l'indirizzo IPv4 statico del server WINS primario. Tipo di dati stringa.|  
||-AlternateWINSServer|Specifica l'indirizzo IPv4 statico del server WINS secondario. Tipo di dati stringa.|  
||-IPv6DNSResolver|Specifica le voci DNS IPv6 statiche del computer clonato in un elenco delimitato da virgole. Non è possibile impostare informazioni statiche Ipv6 nella clonazione del controller di dominio virtualizzati. Tipo di dati matrice.|  
||-Non in linea|Non esegue i test di convalida e sovrascrive eventuali file dccloneconfig.xml esistenti. Non ha parametri. Per ulteriori informazioni, vedere [esegue New-ADDCCloneConfigFile in modalità offline](../../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode).|  
||*-Statico*|Obbligatorio se si specificano gli argomenti IP statici IPv4SubnetMask, IPv4SubnetMask o IPv4DefaultGateway. Non ha parametri.|  
  
I test eseguiti durante l'esecuzione in modalità online:  
  
-   Emulatore PDC è Windows Server 2012 o versione successiva  
  
-   Il controller di dominio di origine è un membro del gruppo controller di dominio clonabili  
  
-   Controller di dominio di origine non include eventuali applicazioni o servizi esclusi  
  
-   Controller di dominio di origine non contiene già un file DcCloneConfig.xml nel percorso specificato  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewDCCloneConfig.png)  
  
### <a name="step-6---take-the-source-domain-controller-offline"></a>Passaggio 6: portare Offline il Controller di dominio di origine  
Non è possibile copiare una controller di dominio; di origine in esecuzione è necessario arrestarlo normalmente. Non clonare un controller di dominio arrestato da interruzione di corrente anomala.  
  
#### <a name="graphical-method"></a>Metodo grafico  
Utilizzare il pulsante di arresto all'interno del controller di dominio in esecuzione o il pulsante di arresto del sistema di gestione di Hyper-V.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_Shutdown.png)  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVShutdown.png)  
  
#### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
È possibile arrestare una macchina virtuale utilizzando uno dei cmdlet seguenti:  
  
```  
Stop-computer  
Stop-vm  
```  
  
Stop-computer è un cmdlet che supporta l'arresto di computer indipendentemente dalla virtualizzazione ed è analogo all'utilità legacy Shutdown.exe. Stop-vm è un nuovo cmdlet del modulo di Windows PowerShell per Windows Server 2012 Hyper-V ed è equivalente a opzioni di risparmio energia di gestione di Hyper-V. Quest'ultimo è utile in ambienti di laboratorio in cui il controller di dominio opera spesso in una rete privata virtualizzata.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopComputer2.png)  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopVM.png)  
  
### <a name="step-7---copy-disks"></a>Passaggio 7 - copiare i dischi  
Nella fase di copia è necessaria una scelta a livello amministrativo:  
  
-   Copiare i dischi manualmente, senza Hyper-V  
  
-   Esportare la macchina virtuale con Hyper-V  
  
-   Esportare i dischi uniti con Hyper-V  
  
Tutti i dischi della macchina virtuale deve essere copiati, non solo l'unità del sistema. Se il controller di dominio di origine Usa dischi differenze e si prevede di spostare il controller di dominio clonato in un altro host Hyper-V, è necessario esportare.  
  
Copia manuale dei dischi è consigliato se il controller di dominio di origine ha solo *uno* unità. Esportazione/importazione è consigliata per le macchine virtuali con *più* unità o altre personalizzazioni hardware virtualizzate e complesse, ad esempio più schede di rete.  
  
Se la copia dei file manualmente, eliminare eventuali snapshot prima di iniziare. Se esporti la macchina virtuale, eliminare gli snapshot prima dell'esportazione o eliminarli dalla nuova macchina virtuale dopo l'importazione.  
  
> [!WARNING]  
> Gli snapshot sono dischi differenze che possono restituire un controller di dominio per lo stato precedente. Se fosse necessario clonare un controller di dominio e quindi ripristinare il relativo snapshot pre-clonazione, farebbe con controller di dominio duplicati nella foresta. Non esiste alcun valore negli snapshot precedenti in un controller di dominio appena clonato.  
  
#### <a name="manually-copying-disks"></a>Copia manuale dei dischi  
  
##### <a name="hyper-v-manager-method"></a>Metodo di gestione di Hyper-V  
Utilizzare lo snap-in Gestione di Hyper-V per determinare quali dischi sono associati con il controller di dominio di origine. Usare l'opzione controlla per verificare se il controller di dominio Usa dischi differenze (che è necessario copiare il disco padre anche)  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVInspect.png)  
  
Per eliminare gli snapshot, selezionare una macchina virtuale ed eliminare il sottoalbero snapshot.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDeleteSnapshot.gif)  
  
È quindi possibile copiare manualmente i file VHD o VHDX usando Esplora risorse, Xcopy.exe o Robocopy.exe. Effettuare alcuna operazione particolare non sono necessari. È consigliabile modificare i nomi dei file anche se lo spostamento in un'altra cartella.  
  
> [!NOTE]  
> Se la copia tra computer host su una LAN (1 GB o superiore), il **Xcopy.exe /J** opzione copia i file VHD/VHDX notevolmente più veloce di qualsiasi altro strumento, ma con molto maggiore utilizzo della larghezza di banda.  
  
##### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
Per determinare i dischi usando Windows PowerShell, usare i moduli Hyper-V:  
  
```  
Get-vmidecontroller  
Get-vmscsicontroller  
Get-vmfibrechannelhba  
Get-vmharddiskdrive  
```  
  
Ad esempio, è possibile restituire tutti i dischi rigidi IDE da una macchina virtuale denominata **DC2** con l'esempio seguente:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_ReturnIDE.png)  
  
Se il percorso del disco punta a un file AVHD o AVHDX, si tratta di uno snapshot. Per eliminare gli snapshot associati a un disco e unirli nel VHD o VHDX reale, utilizzare i cmdlet:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Ad esempio, per eliminare tutti gli snapshot da una VM chiamata DC2-SOURCECLONE:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_DelSnapshots.png)  
  
Per copiare i file utilizzando Windows PowerShell, usare il cmdlet seguente:  
  
```  
Copy-Item  
```  
  
Combinare i cmdlet VM in pipeline per supportare l'automazione. La pipeline è un canale usato tra più cmdlet per passare i dati. Ad esempio, per copiare l'unità di un controller di dominio di origine offline chiamata DC2-SOURCECLONE in un nuovo disco c:\temp\copy.vhd senza la necessità di conoscere il percorso esatto l'unità di sistema denominato:  
  
```  
Get-VMIdeController dc2-sourceclone | Get-VMHardDiskDrive | select-Object {copy-item -path $_.path -destination c:\temp\copy.vhd}  
```  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSCopyDrive.png)  
  
> [!IMPORTANT]  
> È possibile utilizzare dischi passthrough con la clonazione, che non usano un file di disco virtuale ma un disco rigido fisico.  
  
> [!NOTE]  
> Per ulteriori informazioni su ulteriori operazioni di Windows PowerShell con le pipeline, vedere [Piping e Pipeline in Windows PowerShell](https://technet.microsoft.com/en-us/library/ee176927.aspx).  
  
#### <a name="exporting-the-vm"></a>Esportazione della macchina virtuale  
In alternativa a copiare i dischi, è possibile esportare l'intera macchina virtuale Hyper-V come copia. L'esportazione crea automaticamente una cartella denominata per la macchina virtuale e che contiene tutti i dischi e le informazioni di configurazione.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVExport.png)  
  
##### <a name="hyper-v-manager-method"></a>Metodo di gestione di Hyper-V  
Per esportare una macchina virtuale con Hyper-V Manager:  
  
1.  Fare doppio clic su controller di dominio e fare clic su **esportare**.  
  
2.  Selezionare una cartella esistente come contenitore di esportazione.  
  
3.  Attendere la colonna Stato interrompere la visualizzazione **esportazione**.  
  
##### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
Per esportare una macchina virtuale con il modulo Hyper-V di Windows PowerShell, usare il cmdlet:  
  
```  
Export-vm  
```  
  
Ad esempio, per esportare una VM chiamata DC2-SOURCECLONE in una cartella denominata C:\VM:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSExport.png)  
  
> [!NOTE]  
> Windows Server 2012 Hyper-V supporta nuove Esporta e Importa funzionalità che esulano dall'ambito di questa esercitazione. Per ulteriori informazioni, vedere TechNet.  
  
#### <a name="exporting-merged-disks-using-hyper-v"></a>Esportare i dischi uniti con Hyper-V  
L'opzione finale consiste nell'usare le opzioni di unione e conversione di dischi all'interno di Hyper-V. Questi consentono di effettuare una copia di una struttura del disco esistente, anche se i file AVHD/AVHDX di snapshot, in un nuovo disco singolo. Come nello scenario di copia manuale dei dischi, questo è destinato principalmente più semplice macchine virtuali che utilizzano solo una singola unità, ad esempio C:\\. L'unico vantaggio è che, a differenza della copia manuale, non richiede di eliminare prima gli snapshot. Questa operazione è necessariamente inferiore semplicemente l'eliminazione degli snapshot e copia dei dischi.  
  
##### <a name="hyper-v-manager-method"></a>Metodo di gestione di Hyper-V  
Per creare un disco unito con gestione di Hyper-V:  
  
1.  Fare clic su **modifica disco**.  
  
2.  Cercare il disco figlio minore. Ad esempio, se si utilizza un disco differenze, il disco figlio è il figlio minore. Se la macchina virtuale ha uno snapshot (o più di uno), lo snapshot attualmente selezionato è il disco figlio minore.  
  
3.  Selezionare il **unire** opzione per creare un singolo disco dalla struttura padre-figlio intero.  
  
4.  Selezionare un nuovo disco rigido virtuale e specificare un percorso. I file VHD/VHDX vengono riconciliati in una nuova unità portatile che non sono a rischio di ripristinare precedenti snapshot.  
  
##### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
Per creare un disco unito da un insieme complesso di elementi padre con il modulo Hyper-V di Windows PowerShell, usare il cmdlet:  
  
```  
Convert-vm  
```  
  
Ad esempio, per esportare l'intera catena di snapshot di dischi della macchina virtuale (questa volta senza includere dischi differenze) e padre del disco in un nuovo disco singolo denominato DC4-CLONED. VHDX:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSConvertVhd.png)  
  
#### <a name="BKMK_Offline"></a>Aggiunta di XML al disco di sistema Offline  
Se è stato copiato il file Dccloneconfig.xml per il controller di dominio di origine in esecuzione, è necessario copiare il file dccloneconfig.xml aggiornato al disco di sistema copiato/esportato offline ora. A seconda delle applicazioni installate rilevate con Get-ADDCCloningExcludedApplicationList in precedenza, è necessario copiare il file CustomDCCloneAllowList.xml nel disco.  
  
I percorsi seguenti possono contenere il file DcCloneConfig.xml:  
  
1.  Directory di lavoro DSA  
  
2.  %windir%\NTDS  
  
3.  Supporto rimovibile di lettura/scrittura, in ordine di lettera di unità, alla radice dell'unità  
  
Questi percorsi non sono configurabili. Dopo che viene avviata la clonazione, i controlli di clonazione queste posizioni nell'ordine specifico e Usa il file DcCloneConfig.xml primo file trovato, indipendentemente dal contenuto della cartella.  
  
I percorsi seguenti possono contenere il file CustomDCCloneAllowList.xml:  
  
1.  Hkey_local_machine\system\currentcontrolset\services\ntds\parameters.  
  
    AllowListFolder (*REG_SZ*)  
  
2.  Directory di lavoro DSA  
  
3.  %windir%\NTDS  
  
4.  Supporto rimovibile di lettura/scrittura, in ordine di lettera di unità, alla radice dell'unità  
  
È possibile eseguire New-ADDCCloneConfigFile con il **-offline** argomento (noto anche come modalità) per creare il file DcCloneConfig.xml e posizionarlo nel percorso corretto. Gli esempi seguenti illustrano come eseguire New-ADDCCloneConfigFile in modalità offline.  
  
Per creare un controller di dominio clone denominato CloneDC1 in modalità offline, in un sito denominato "REDMOND" con indirizzo IPv4 statico, digitare:  
  
```  
New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS  
```  
  
Per creare un controller di dominio clone denominato Clone2 in modalità offline con impostazioni IPv4 statiche e statiche IPv6, tipo:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS  
```  
  
Per creare un controller di dominio clone in modalità offline con impostazioni IPv4 statiche e dinamiche IPv6 e specificare più server DNS per le impostazioni del resolver DNS, digitare:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS   
```  
  
Per creare un controller di dominio clone denominato Clone1 in modalità offline con impostazioni IPv4 dinamiche e statiche IPv6, tipo:  
  
```  
New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS  
```  
  
Per creare un controller di dominio clone in modalità offline con impostazioni IPv4 dinamiche e dinamico IPv6, digitare:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS  
  
```  
  
##### <a name="windows-explorer-method"></a>Metodo Esplora risorse  
Windows Server 2012 offre ora un'opzione grafica per montare file VHD e VHDX. È necessario installare la funzionalità esperienza Desktop in Windows Server 2012.  
  
1.  Fare clic sul file VHD/VHDX appena copiato che contiene l'unità del sistema del controller di dominio di origine o cartella del percorso della Directory di lavoro DSA, quindi fare clic su **montare** dal **strumenti immagini disco** menu.  
  
2.  Nell'unità appena montata, copiare i file XML in un percorso valido. Potrebbe essere richiesto per le autorizzazioni per la cartella.  
  
3.  Fare clic sull'unità montata e fare clic su **Eject** dal **disco strumenti** menu.  
  
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
  
In questo modo il controllo completo sul processo. Ad esempio, può essere l'unità montate con una lettera di unità specifica, copiare il file e smontare l'unità.  
  
```  
mount-vhd <disk path> -passthru -nodriveletter | get-disk | get -partition | get-volume | get-partition | where {$_.partition number -eq 2} | Add-PartitionAccessPath -accesspath <drive letter>  
  
copy-item <xml file path><destination path>\dccloneconfig.xml  
  
dismount-vhd <disk path>  
```  
  
Per esempio:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSMountVHD.png)  
  
In alternativa, è possibile utilizzare il nuovo **Mount-DiskImage** cmdlet per montare un file disco rigido virtuale (o ISO).  
  
### <a name="step-8---create-the-new-virtual-machine"></a>Passaggio 8 - creare la nuova macchina virtuale  
Il passaggio di configurazione finale prima di avviare il processo di clonazione consiste nel creare una nuova macchina virtuale che utilizza i dischi dal controller di dominio di origine copiato. In base alla selezione effettuata nella fase di copia dei dischi, sono disponibili due opzioni:  
  
1.  Associare una nuova macchina virtuale al disco copiato  
  
2.  Importare la VM esportata  
  
#### <a name="associating-a-new-vm-with-copied-disks"></a>Associazione di una nuova macchina virtuale ai dischi copiati  
Se il disco di sistema è stato copiato manualmente, è necessario creare una nuova macchina virtuale usando il disco copiato. L'hypervisor imposta automaticamente l'ID di generazione VM quando viene creata una nuova macchina virtuale; modifiche alla configurazione non sono necessari nell'host di macchina virtuale o di Hyper-V.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVConnectVHD.gif)  
  
##### <a name="hyper-v-manager-method"></a>Metodo di gestione di Hyper-V  
  
1.  Creare una nuova macchina virtuale.  
  
2.  Specificare il nome, memoria e rete VM.  
  
3.  Nella pagina Connessione disco rigido virtuale, specificare il disco di sistema copiato.  
  
4.  Completare la procedura guidata per creare la macchina virtuale.  
  
Se ci sono più dischi, schede di rete o altre personalizzazioni, configurarli prima di avviare il controller di dominio. Il metodo "Esportazione-importazione" copia dei dischi è consigliato per le macchine virtuali complesse.  
  
##### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
È possibile utilizzare il modulo Hyper-V di Windows PowerShell per automatizzare la creazione della macchina virtuale in Windows Server 2012, usando il cmdlet seguente:  
  
```  
New-VM  
```  
  
Ad esempio, di seguito la VM DC4-CLONEDFROMDC2 viene creata, con 1GB di RAM, l'avvio dal file c:\vm\dc4-systemdrive-clonedfromdc2.vhd e uso della rete virtuale 10.0:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewVM.png)  
  
#### <a name="import-vm"></a>Importare VM  
Se in precedenza è stata esportata la VM, è necessario importare nuovamente come copia. Utilizza il file XML esportato per ricreare il computer con tutte le impostazioni precedenti, unità, reti e le impostazioni della memoria.  
  
Se si intende creare copie aggiuntive da della stessa VM esportata, apportare le copie della VM esportata necessarie. Quindi usare Importa per ogni copia.  
  
> [!IMPORTANT]  
> È importante usare il **copia** opzione, quanto l'esportazione preserva tutte le informazioni dall'origine; l'importazione del server con **spostare** o **In posizione** causa collisioni delle informazioni se eseguita nello stesso server host Hyper-V.  
  
##### <a name="hyper-v-manager-method"></a>Metodo di gestione di Hyper-V  
Per importare con lo snap-in Gestione di Hyper-V:  
  
1.  Fare clic su **Importa macchina virtuale**  
  
2.  Nel **individua cartella** pagina, selezionare il file di definizione VM esportato usando il pulsante Sfoglia  
  
3.  Nel **seleziona macchina virtuale** pagina, fare clic sul computer di origine.  
  
4.  Nel **Scegli tipo di importazione** pagina, fare clic su **copia macchina virtuale (Crea un nuovo ID univoco)**, quindi fare clic su **fine**.  
  
5.  Rinominare la VM importata, se l'importazione nello stesso host Hyper-V; avrà lo stesso nome del controller di dominio di origine esportato.  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportLocateFolder.png)  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportSelectVM.png)  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportChooseType.gif)  
  
Ricordarsi di rimuovere eventuali snapshot importati, utilizzando lo snap-in Gestione Hyper-V:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportDelSnap.gif)  
  
> [!WARNING]  
> L'eliminazione di eventuali snapshot importati è di importanza critica; Se applicato, sarebbe restituiscono il controller di dominio clonato allo stato di un controller di dominio precedente, possibilmente attivo - causando errori di replica, informazioni IP duplicate e altre interruzioni.  
  
##### <a name="windows-powershell-method"></a>Metodo Windows PowerShell  
È possibile utilizzare il modulo Hyper-V di Windows PowerShell per automatizzare l'importazione di VM in Windows Server 2012, usando i cmdlet seguenti:  
  
```  
Import-VM  
Rename-VM  
```  
  
Ad esempio, qui esportato la VM DC2-CLONED viene importato utilizzando il file XML determinato automaticamente, quindi viene immediatamente rinominata con il nuovo nome VM DC5-CLONEDFROMDC2:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSImportVM.png)  
  
Ricordarsi di rimuovere eventuali snapshot importati, usando i cmdlet seguenti:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Per esempio:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSGetVMSnap.png)  
  
> [!WARNING]  
> Assicurarsi che, quando si importa il computer, indirizzi MAC statici non assegnati al controller di dominio di origine. Se viene clonato un computer di origine con un indirizzo MAC statico, i computer copiati non correttamente invierà o riceverà traffico di rete. In questo caso, impostare un nuovo univoco statico o dinamico indirizzo MAC. È possibile vedere se una VM Usa indirizzi MAC statici con il comando:  
>   
> **Get-VM - Name**   
>  ***test-vm* | Get-VMNetworkAdapter | fl \ ***  
  
### <a name="step-9---clone-the-new-virtual-machine"></a>Passaggio 9 - creare la nuova macchina virtuale  
Facoltativamente, prima di iniziare la clonazione, riavviare il controller di dominio di origine clone non in linea. Assicurarsi che l'emulatore PDC sia online, indipendentemente dal fatto.  
  
Per iniziare la clonazione, avviare semplicemente la nuova macchina virtuale. Il processo inizia automaticamente e il controller di dominio viene riavviato automaticamente al termine della clonazione.  
  
> [!IMPORTANT]  
> Non è consigliabile mantenere i controller di dominio disattivati per un lungo periodo di tempo e se il clone viene aggiunto il controller di dominio di origine nello stesso sito la topologia di replica tra siti intra iniziale potrebbe richiedere più tempo per creare se il controller di dominio di origine è offline.  
  
Se si usa Windows PowerShell per avviare una macchina virtuale, è il nuovo cmdlet modulo Hyper-V:  
  
```  
Start-VM  
```  
  
Per esempio:  
  
![Distribuzione di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSStartVM.png)  
  
Dopo il riavvio del computer al termine della clonazione, è un controller di dominio ed è possibile effettuare l'accesso normalmente per verificarne il regolare funzionamento. Se sono presenti errori, il server è impostato per l'avvio in modalità ripristino servizi Directory per l'analisi.  
  
## <a name="BKMK_VDCSafeRestore"></a>Misure di sicurezza della virtualizzazione  
A differenza di clonazione del controller di dominio virtualizzati, misure di sicurezza di Windows Server 2012 non è alcun passaggio di configurazione. La funzionalità funziona senza l'intervento, purché vengano rispettate alcune semplici condizioni:  
  
-   L'hypervisor supporti l'ID di generazione VM  
  
-   Esiste un controller di dominio partner valido che un controller di dominio ripristinato può replicare le modifiche in modo non autorevole.  
  
### <a name="validate-the-hypervisor"></a>Convalidare l'Hypervisor  
Assicurarsi che il controller di dominio di origine è in esecuzione in un hypervisor supportato consultando la documentazione del fornitore. Controller di dominio virtualizzati sono indipendenti dall'hypervisor e non richiedono Hyper-V.  
  
Rivedere i precedenti [requisiti della piattaforma](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_PlatformReqs) sezione per supporto noto dell'ID di generazione VM.  
  
Se si esegue la migrazione di macchine virtuali da un hypervisor di origine a un hypervisor di destinazione diversa, misure di sicurezza della virtualizzazione può o meno attivate a seconda che i hypervisor supportino ID di generazione VM, come illustrato nella tabella seguente.  
  
|Hypervisor di origine|Hypervisor di destinazione|Risultato|  
|---------------------|---------------------|----------|  
|Supporta l'ID di generazione VM|Non supporta l'ID di generazione VM|Misure di sicurezza non attivate (se è presente un file DCCloneConfigFile.xml, controller di dominio verrà avviato in modalità ripristino servizi directory)|  
|Non supporta l'ID di generazione VM|Supporta l'ID di generazione VM|Misure di sicurezza attivate|  
|Supporta l'ID di generazione VM|Supporta l'ID di generazione VM|Misure di sicurezza non attivate, perché la definizione della VM non è stato modificato, ovvero in modo che l'ID di generazione VM rimane invariato|  
  
### <a name="validate-the-replication-topology"></a>Convalidare la topologia di replica  
Misure di sicurezza della virtualizzazione avviano una replica in ingresso non autorevole per il delta della replica di Active Directory, nonché la risincronizzazione non autorevole di tutto il contenuto SYSVOL. In questo modo il controller di dominio restituito da uno snapshot con la piena funzionalità ed è alla fine coerente con il resto dell'ambiente.  
  
Con questa nuova funzionalità prevede diversi requisiti e limitazioni:  
  
-   Un controller di dominio ripristinato deve essere in grado di contattare un controller di dominio scrivibile  
  
-   Tutti i controller di dominio in un dominio non devono essere ripristinati contemporaneamente  
  
-   Le eventuali modifiche derivanti da un controller di dominio ripristinato che non sono state replicate in uscita perché la creazione dello snapshot vanno perse per sempre  
  
Mentre la sezione sulla risoluzione dei problemi illustra questi scenari, i dettagli seguenti assicurano che non viene creata una topologia che potrebbe causare problemi.  
  
#### <a name="writable-domain-controller-availability"></a>Disponibilità del Controller di dominio scrivibile  
Se ripristinato, un controller di dominio deve avere connettività con un controller di dominio scrivibile. un controller di dominio di sola lettura non può inviare il delta degli aggiornamenti. La topologia è probabile che già corretta per questo, come controller di dominio scrivibile necessita sempre di un partner scrivibile. Tuttavia, se tutti i controller di dominio scrivibili vengono ripristinati simultaneamente, nessuno di essi può trovare un'origine valida. Lo stesso vale se i controller di dominio scrivibili sono offline per manutenzione o altrimenti irraggiungibili tramite la rete.  
  
#### <a name="simultaneous-restore"></a>Ripristino simultaneo  
Non ripristinare tutti i controller di dominio in un singolo dominio contemporaneamente. Se tutti gli snapshot vengono ripristinati contemporaneamente, funzionamento della replica di Active Directory normalmente, ma si interrompe la replica di SYSVOL. L'architettura di ripristino di FRS e replica DFS richiede l'impostazione della relativa istanza di replica alla modalità di sincronizzazione non autorevole. Se tutti i controller di dominio ripristino contemporaneamente e ogni controller di dominio di essi si contrassegna non autorevole per SYSVOL, perché tutti tenterà quindi sincronizzare criteri di gruppo e script di un partner autorevole; a questo punto, però, tutti i partner sono anche non autorevole.  
  
> [!IMPORTANT]  
> Se tutti i controller di dominio vengono ripristinati contemporaneamente, consultare gli articoli seguenti per impostare un controller di dominio, in genere l'emulatore PDC - come autorevole, in modo che gli altri controller di dominio è possono tornare alla normale funzionamento:  
>   
> [Set di repliche utilizzando la chiave del Registro di sistema BurFlags per reinizializzare il servizio Replica File](https://support.microsoft.com/kb/290762)  
>   
> [Come forzare una sincronizzazione autorevole e non autorevole per SYSVOL con replica DFS (come "D4/D2" per il servizio Replica file)](https://support.microsoft.com/kb/2218556)  
  
> [!WARNING]  
> Impossibile eseguire tutti i controller di dominio in un dominio o foresta nello stesso host hypervisor. Introduce un singolo punto di errore che danneggia servizi di dominio Active Directory, Exchange, SQL e altre operazioni aziendali ogni volta che l'hypervisor passa offline. Si tratta non diversa dall'uso di un solo controller di dominio per un intero dominio o foresta. Più controller di dominio su più piattaforme assicurano ridondanza e tolleranza di errore.  
  
#### <a name="post-snapshot-replication"></a>Replica post-Snapshot  
Non ripristinare gli snapshot finché tutte le modifiche di origine locale ed effettuate dopo la creazione di snapshot sono state replicate in uscita. Le modifiche di origine vanno perse per sempre se altri controller di dominio non ha già ricevuto li tramite la replica.  
  
Usare Repadmin.exe per mostrare eventuali modifiche non replicate in uscita tra un controller di dominio e i relativi partner:  
  
1.  Restituire partner del controller di dominio, i nomi e GUID oggetto DSA con:  
  
    ```  
    Repadmin.exe /showrepl <DC Name of the partner> /repsto  
    ```  
  
2.  Restituire la replica in ingresso in sospeso del controller di dominio partner al controller di dominio da ripristinare:  
  
    ```  
    Repadmin.exe /showchanges < Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare>  
    ```  
  
In alternativa, è solo per visualizzare il numero di modifiche non replicate:  
  
```  
Repadmin.exe /showchanges <Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare> /statistics  
```  
  
Ad esempio (con l'output modificato per migliorare la leggibilità e le voci importanti ***in corsivo***), qui si esamina i partner di replica di DC4:  
  
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
  
Ora sai che esegue la replica con DC2 e DC3. Viene quindi mostrato l'elenco delle modifiche che DC2 dichiara ancora non hanno da DC4 e verificare che è disponibile un nuovo gruppo:  
  
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
  
Viene inoltre testato l'altro partner per assicurarsi che è stato già replicato.  
  
In alternativa, se non è importante sapere quali oggetti sono stati replicati e solo se ci sono che tutti gli oggetti sono stati in sospeso, è possibile utilizzare il **/statistics** opzione:  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com /statistics  
  
***********************************************  
********* Grand total *************************  
Packets:              1  
Objects:              1Object Additions:     1Object Modifications: 0Object Deletions:     0Object Moves:         0Attributes:           12Values:               13  
```  
  
> [!IMPORTANT]  
> Testare tutti i partner scrivibili se eventuali errori o repliche in sospeso. Purché almeno uno sia convergente, è in genere possibile ripristinare lo snapshot, come la replica transitiva alla fine Riconcilia gli altri server.  
>   
> Assicurarsi di prendere nota di eventuali errori di replica mostrati da /showchanges e non continuare finché saranno risolti.  
  
### <a name="windows-powershell-snapshot-cmdlets"></a>Cmdlet di Windows PowerShell per Snapshot  
I seguenti cmdlet del modulo Hyper-V di Windows PowerShell forniscono funzionalità di snapshot in Windows Server 2012:  
  
```  
Checkpoint-VM  
Export-VMSnapshot  
Get-VMSnapshot  
Remove-VMSnapshot  
Rename-VMSnapshot  
Restore-VMSnapshot  
```  
  


