---
title: Macchine virtuali schermate per i tenant - creazione di dati di schermatura per definire una macchina virtuale schermata
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 49f4e84d-c1f7-45e5-9143-e7ebbb2ef052
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: 25ed17d964f12c2f497ccde443dad9f8bc253b20
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034677"
---
# <a name="shielded-vms-for-tenants---creating-shielding-data-to-define-a-shielded-vm"></a>Macchine virtuali schermate per i tenant - creazione di dati di schermatura per definire una macchina virtuale schermata

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Un file di dati di schermatura (chiamato anche file di dati di provisioning o file PDK) è un file crittografato creato dal tenant o dal proprietario di una macchina virtuale per proteggere le informazioni di configurazione importanti di una macchina virtuale, ad esempio la password dell'amministratore, RDP e altri certificati di identità, le credenziali di aggiunta al dominio e così via. In questo argomento vengono fornite informazioni su come creare un file di dati di schermatura. Prima di creare il file, è necessario ottenere un disco modello dal provider di servizi di hosting oppure creare un disco modello, come descritto in [schermato macchine virtuali per tenant - creazione di un disco modello (facoltativo)](guarded-fabric-tenant-creates-template-disk.md).

Per un elenco e un diagramma del contenuto di un file di dati di schermatura, vedere [quali sia i dati di schermatura e perché sono necessari?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

> [!IMPORTANT]
> I passaggi in questa sezione devono essere completati in un computer tenant che esegue Windows Server 2016. Tale macchina non deve far parte di un'infrastruttura sorvegliata (vale a dire, non devono essere configurati per usare un cluster del servizio HGS).

Per prepararsi alla creazione di un file di dati di schermatura, procedere come segue:

- [Ottenere un certificato per la connessione Desktop remoto](#obtain-a-certificate-for-remote-desktop-connection)
- [Creare un file di risposte](#create-an-answer-file)
- [Ottenere il file di catalogo di firma di volume](#get-the-volume-signature-catalog-file)
- [Selezionare le infrastrutture attendibile](#select-trusted-fabrics)

È quindi possibile creare il file di dati di schermatura:

- [Creare un file di dati di schermatura e aggiungere guardians](#create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard)


## <a name="obtain-a-certificate-for-remote-desktop-connection"></a>Ottenere un certificato per la connessione Desktop remoto

Poiché i tenant sono solo in grado di connettersi alle proprie macchine virtuali schermate tramite connessione Desktop remoto o altri strumenti di gestione remota, è importante garantire che i tenant possono verificare che si connette all'endpoint corretto (vale a dire, non c'è un "man in the middle" intercettazione della connessione).

Un modo per verificare che ci si connette al server desiderato è installare e configurare un certificato per Servizi Desktop remoto presentare quando si avvia una connessione. Nel computer client la connessione al server controllerà se considera attendibile il certificato e Mostra un avviso se non esiste. In generale, per assicurarsi che il client di connessione consideri attendibile il certificato, RDP vengono emessi da PKI del tenant. Altre informazioni sulle [usando i certificati in Servizi Desktop remoto](https://technet.microsoft.com/library/dn781533.aspx) sono disponibili su TechNet.

<!-- The previous link comes from Windows 2012 R2 content, but as of Sept 2016, there isn't a more recent link that covers the same information. -->

> [!NOTE]
> Quando si seleziona un certificato RDP da includere nel file di dati di schermatura, assicurarsi di usare un certificato con caratteri jolly. Un file di dati di schermatura può essere utilizzato per creare un numero illimitato di macchine virtuali. Poiché ogni macchina virtuale condivideranno lo stesso certificato, un certificato con caratteri jolly assicura che il certificato sarà valido indipendentemente dal nome host della macchina virtuale.

Se si siano valutando le macchine virtuali schermate e non sono ancora pronti per richiedere un certificato dall'autorità di certificazione, è possibile creare un certificato autofirmato nel computer tenant eseguendo il comando di Windows PowerShell seguente (dove *contoso.com*  è il dominio del tenant):

``` powershell
$rdpCertificate = New-SelfSignedCertificate -DnsName '\*.contoso.com'
$password = ConvertTo-SecureString -AsPlainText 'Password1' -Force
Export-PfxCertificate -Cert $RdpCertificate -FilePath .\rdpCert.pfx -Password $password
```

## <a name="create-an-answer-file"></a>Creare un file di risposte

Poiché il disco modello firmato in VMM è generalizzato, i tenant sono necessari per fornire un file di risposte per specializzare le proprie macchine virtuali schermate durante il processo di provisioning. Il file di risposte (spesso chiamato file di installazione automatica) possa configurare la macchina virtuale per il ruolo previsto: vale a dire, possono installare le funzionalità di Windows, registrare il certificato RDP creato nel passaggio precedente ed eseguire altre azioni personalizzate. Fornirà anche informazioni necessarie per l'installazione di Windows, tra cui chiave di prodotto e password dell'amministratore predefinito.

Per informazioni su come ottenere e usare la **New-ShieldingDataAnswerFile** funzione per generare un file di risposte (Unattend. XML file) per la creazione di macchine virtuali schermate, vedere [generare un file di risposte tramite il Funzione nuovo-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md). Utilizzo della funzione, è possibile generare più facilmente un file di risposte che rifletta le scelte simile al seguente:

- La macchina virtuale deve appartenere al dominio al termine del processo di inizializzazione?
- Si userà un contratto multilicenza o codice product key specifico per ogni macchina virtuale?
- Si usa DHCP o indirizzo IP statico?
- Si userà un certificato di Remote Desktop Protocol (RDP) che verrà usato per dimostrare che la macchina virtuale appartenga all'organizzazione?
- Si desidera eseguire uno script alla fine dell'inizializzazione?
- Si sta utilizzando un server di Desired State Configuration (DSC) per ampliare la configurazione?

I file di risposte utilizzati nel file di dati di schermatura verranno utilizzati in ogni macchina virtuale creata utilizzando tale file di dati di schermatura. Pertanto, assicurarsi che non codificare le informazioni specifiche delle macchine Virtuali nel file di risposte. VMM supporta alcune stringhe di sostituzione (vedere la tabella riportata di seguito) nel file di installazione automatica per gestire i valori di specializzazione che potrebbero modificare dalla macchina virtuale a macchina virtuale. Non si deve usare queste; Tuttavia, se sono presenti VMM sarà possibile avvalersi di essi.

Quando si crea un file Unattend. XML per le macchine virtuali schermate, tenere presenti le restrizioni seguenti:

-   Il file di installazione automatica deve comportare la macchina virtuale è spenta dopo che è stata configurata. Si tratta per consentire a VMM sapere quando viene segnalato al tenant che la macchina virtuale completato il provisioning e sia pronta per l'uso. VMM verranno automaticamente accendere la macchina virtuale nuovamente una volta che rileva che è stata disattivata durante il provisioning.

-   È consigliabile configurare un certificato RDP per verificare che ci si connette a destra della macchina virtuale e non un altro computer configurato per un attacco man-in-the-middle.

-   Assicurarsi di abilitare RDP e la regola del firewall corrispondenti in modo che è possibile accedere alla macchina virtuale dopo che è stata configurata. È possibile utilizzare la console VMM per le macchine virtuali schermata di accesso, pertanto è necessario il protocollo RDP per connettersi alla macchina virtuale. Se si preferisce gestire i sistemi con la comunicazione remota di Windows PowerShell, verificare che WinRM sia abilitato, troppo.

-   Le stringhe di sostituzione solo supportate in file di installazione automatica di macchine Virtuali schermati sono i seguenti:

| Elemento sostituibile | Stringa di sostituzione |
|-----------|-----------|
| ComputerName        | @ComputerName@      |
| Fuso orario            | @TimeZone@          |
| ProductKey          | @ProductKey@        |
| IPAddr4-1           | @IP4Addr-1@         |
| IPAddr6-1           | @IP6Addr-1@         |
| MACAddr-1           | @MACAddr-1@         |
| Prefix-1-1          | @Prefix-1-1@        |
| NextHop-1-1         | @NextHop-1-1@       |
| Prefix-1-2          | @Prefix-1-2@        |
| NextHop-1 o 2         | @NextHop-1-2@       |

Quando si usano le stringhe di sostituzione, è importante garantire che le stringhe verranno popolate durante il processo di provisioning di VM. Se una stringa, ad esempio @ProductKey@ non viene fornito in fase di distribuzione, lasciando il &lt;ProductKey&gt; nodo nel file di installazione automatica vuoto, il processo di specializzazione avrà esito negativo e non sarà possibile connettersi alla macchina virtuale.

Si noti inoltre che le stringhe di sostituzione correlata alla rete verso la fine della tabella vengono usate solo se si stanno sfruttando i pool di indirizzi IP statici a VMM. Il provider di servizi di hosting deve essere in grado di indicare se sono necessarie queste stringhe di sostituzione. Per altre informazioni sugli indirizzi IP statici nei modelli di VMM, vedere quanto segue nella documentazione di VMM:

- [Linee guida per i pool di indirizzi IP](https://technet.microsoft.com/system-center-docs/vmm/plan/plan-network#guidelines-for-ip-address-pools) 
- [Configurare pool di indirizzi IP statici nell'infrastruttura di VMM](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-network-static-address-pools)

Infine, è importante notare che il processo di distribuzione di VM schermato solo crittograferà l'unità del sistema operativo. Se si distribuisce una macchina virtuale schermata con una o più unità di dati, è consigliabile aggiungere un comando di installazione automatica o l'impostazione criteri di gruppo nel dominio tenant per crittografare automaticamente le unità dati.

## <a name="get-the-volume-signature-catalog-file"></a>Ottenere il file di catalogo di firma di volume

File di dati di schermatura contengono anche informazioni sui dischi modello considera attendibile un tenant. Tenant di acquisire le firme dei dischi da dischi modello attendibili sotto forma di un file di catalogo (VSC) di firma di volume. Tali firme vengono convalidate quando viene distribuita una nuova macchina virtuale. Se nessuna delle firme nel file di dati di schermatura corrisponde il disco modello tentando di essere distribuito con la macchina virtuale (vale a dire che è stato modificato o scambiato con un disco diverso, potenzialmente dannoso), il processo di provisioning avrà esito negativo.

> [!IMPORTANT]
> Mentre il VSC assicura che un disco non sia stato manomesso, è comunque importante per il tenant di considerare attendibile il disco in primo luogo. Se si è il tenant e il disco modello viene fornito dal provider di servizi di hosting, distribuire una macchina virtuale usando tale disco modello di test ed eseguire strumenti personalizzati (antivirus, scanner delle vulnerabilità e così via) per convalidare il disco è, in realtà, in uno stato attendibile.

Esistono due modi per acquisire il VSC di un disco modello:

-  L'host (o tenant, se il tenant può accedere a VMM) usa i cmdlet di PowerShell di VMM per salvare il VSC e assegna al tenant. Questa operazione può essere eseguita in qualsiasi computer con la console VMM installato e configurato per gestire l'ambiente di hosting dell'infrastruttura VMM. I cmdlet di PowerShell per salvare il VSC sono:

        $disk = Get-SCVirtualHardDisk -Name "templateDisk.vhdx"
    
        $vsc = Get-SCVolumeSignatureCatalog -VirtualHardDisk $disk
    
        $vsc.WriteToFile(".\templateDisk.vsc")

-  Il tenant ha accesso al file del disco modello. Se il tenant viene creato un disco modello caricato da un provider di servizi o se il tenant è possibile scaricare il disco di modello di hosting potrebbe essere la scelta. In questo caso, senza VMM nell'immagine, il tenant potrebbe eseguire il cmdlet seguente (installato con la funzionalità strumenti macchina virtuale schermata, parte di strumenti di amministrazione remota del Server):

        Save-VolumeSignatureCatalog -TemplateDiskPath templateDisk.vhdx -VolumeSignatureCatalogPath templateDisk.vsc

## <a name="select-trusted-fabrics"></a>Selezionare le infrastrutture attendibile

L'ultima parte del file di dati di schermatura è correlato al proprietario e ai tutori di una macchina virtuale. Guardians vengono utilizzate per designare sia il proprietario di una macchina virtuale schermata e le infrastrutture sorvegliate in cui è autorizzato a eseguire.

Per autorizzare un'infrastruttura di hosting per l'esecuzione di una macchina virtuale schermata, è necessario ottenere i metadati di sorveglianza dal servizio sorveglianza Host del provider di servizi di hosting. Spesso, il provider di servizi di hosting fornirà i metadati tramite strumenti di gestione. In uno scenario aziendale, è possibile accedere direttamente a ottenere i metadati manualmente.

Sia il provider di servizi di hosting per ottenere i metadati di sorveglianza dal servizio HGS, effettuando una delle azioni seguenti:

-  Per ottenere i metadati di sorveglianza direttamente dal servizio HGS, eseguendo il comando seguente di Windows PowerShell, o esplorando il sito Web e il salvataggio del file XML che viene visualizzato:

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

-  Ottenere i metadati di sorveglianza da VMM tramite i cmdlet di PowerShell di VMM:

        $relecloudmetadata = Get-SCGuardianConfiguration

        $relecloudmetadata.InnerXml | Out-File .\RelecloudGuardian.xml -Encoding UTF8

<!-- Note that the VMM PowerShell cmdlets aren't Windows PowerShell, so "VMM PowerShell" is the correct terminology for them. -->

Ottenere i file di metadati di sorveglianza per ogni infrastruttura sorvegliata che si vuole autorizzare le macchine virtuali schermate esecuzione prima di continuare.

## <a name="create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard"></a>Creare un file di dati di schermatura e aggiungere guardians usando la procedura guidata File di dati di schermatura

Eseguire la procedura guidata File di dati di schermatura per creare un file di dati (con estensione PDK) di schermatura. In questo caso, è possibile aggiungere il certificato RDP,. ini, cataloghi di firma di volume, sorveglianza del proprietario e i metadati di sorveglianza scaricato ottenuto nel passaggio precedente.

1.  Installare **strumenti di amministrazione remota del Server &gt; strumenti di amministrazione funzionalità &gt; strumenti macchina virtuale schermata** nel computer usando Server Manager o il comando Windows PowerShell seguente:

        Install-WindowsFeature RSAT-Shielded-VM-Tools

2.  Aprire la procedura guidata File di dati di schermatura dalla sezione Strumenti di amministrazione nel menu Start o eseguendo il seguente eseguibile **c:\\Windows\\System32\\ShieldingDataFileWizard.exe**.

3.  Nella prima pagina, utilizzare la seconda casella di selezione file per scegliere un nome file e percorso per il file di dati di schermatura. In genere, si potrebbe denominare un file di dati di schermatura dopo aver creato l'entità che possiede tutte le macchine virtuali con i dati di schermatura (ad esempio, risorse Umane, IT, Finance) e il ruolo del carico di lavoro è in esecuzione (ad esempio, file server, server web o qualsiasi altro elemento configurato dal file di installazione automatica). Lasciare il pulsante di opzione impostato su **dei dati per i modelli di schermate di schermatura**.

    > [!NOTE]
    > Nella procedura guidata File di dati di schermatura è possibile notare due opzioni seguenti:
    >- **Dati di schermatura per i modelli di schermate**
    >- **Dati di schermatura per le macchine virtuali esistenti e i modelli non schermato**<br>
    > La prima opzione è utilizzata durante la creazione di nuove macchine virtuali schermate dai modelli schermati. La seconda opzione consente di creare dati di schermatura che possono essere utilizzati solo durante la conversione VM esistenti o creazione di macchine virtuali schermate dai modelli non schermato.

    ![Creazione guidata File di dati di schermatura, selezione dei file](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-01.png)

       Inoltre, è necessario scegliere se le macchine virtuali create utilizzando questo file di dati di schermatura verrà effettivamente schermata o configurato in modalità "crittografia supportata". Per altre informazioni su queste due opzioni, vedere [quali sono i tipi di macchine virtuali che è possibile eseguire un'infrastruttura sorvegliata?](guarded-fabric-and-shielded-vms.md#what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run).

    > [!IMPORTANT]
    > Prestare particolare attenzione al passaggio successivo, perché il proprietario delle VM schermate e che definisce le infrastrutture di VM schermate sarà autorizzata a eseguire in.<br>Il possesso di **sorveglianza del proprietario** è necessario intervenire per in seguito si modifica una macchina virtuale schermata esistente dal **schermate** a **crittografia supportata** o viceversa.
    
4.  L'obiettivo in questo passaggio prevede due fasi:

    - Creare o selezionare un sorvegliante proprietario si rappresenta il proprietario della macchina virtuale

    - Importare il sorvegliante scaricato dal provider di hosting (o il proprio) il servizio sorveglianza Host nel passaggio precedente

    Per designare un tutore proprietario esistente, selezionare la sorveglianza appropriato dal menu a discesa. Solo guardians installato nel computer locale con le chiavi private intatte verrà visualizzata nell'elenco. È anche possibile creare il proprio sorveglianza del proprietario facendo clic **gestire locale Guardians** in basso a destra e scegliendo **crea** e completare la procedura guidata.

    Successivamente, è stato possibile importare i metadati di sorveglianza scaricato in precedenza usando il **proprietario e ai tutori** pagina. Selezionare **gestire locale Guardians** dall'angolo inferiore destro. Usare la **importare** funzionalità per importare il file di metadati di sorveglianza. Fare clic su **OK** dopo che sono stati importati o tutti dei sorveglianti necessari inseriti. Come procedura consigliata, assegnare un nome guardians dopo l'hosting del servizio provider di centro dati aziendale o che rappresentano. Selezionare infine tutti sorveglianti che rappresentano i Data Center in cui la macchina virtuale schermata è autorizzata a eseguire. Non devi selezionare di nuovo la sorveglianza del proprietario. Fare clic su **successivo** una volta terminato.

    ![Creazione guidata File di dati, il proprietario e ai tutori di schermatura](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-02.png)

5.  Nella pagina di qualificatori di ID di Volume, fare clic su **Add** per autorizzare un disco modello firmato nel file di dati di schermatura. Quando si seleziona un VSC nella finestra di dialogo, verrà visualizzate informazioni sul nome del disco, versione e il certificato usato per la firma. Ripetere questo processo per ogni disco di modello che si desidera autorizzare.

6.  Nel **i valori di specializzazione** pagina, fare clic su **Sfoglia** per selezionare il file Unattend. XML che verrà usato per specializzare le macchine virtuali.

    Usare la **Add** nella parte inferiore per aggiungere eventuali file aggiuntivi per il PDK necessari durante il processo di specializzazione. Ad esempio, se il file di installazione automatica viene installato un certificato RDP alla macchina virtuale (come descritto in [generare un file di risposte usando la funzione New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md)), è necessario aggiungere il file RDPCert.pfx a cui fa riferimento il In questo caso i file di installazione automatica. Si noti che tutti i file specificati qui verranno automaticamente copiati in c:\\temp\\ nella macchina virtuale creata. Il file di installazione automatica è necessario prevedere che i file in tale cartella quando si fa riferimento in base al percorso.

7.  Rivedere le selezioni nella pagina successiva e quindi fare clic su **genera**.

8.  Chiudere la procedura guidata dopo il completamento.

## <a name="create-a-shielding-data-file-and-add-guardians-using-powershell"></a>Creare un file di dati di schermatura e aggiungere guardians usando PowerShell

Come alternativa alla procedura guidata File di dati di schermatura, è possibile eseguire [New-ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/new-shieldingdatafile?view=win10-ps) per creare un file di dati di schermatura.

Tutti i file di dati di schermatura devono essere configurata con il proprietario del corretto e i certificati di sorveglianza per autorizzare le macchine virtuali schermate per l'esecuzione su un'infrastruttura sorvegliata.
È possibile verificare la presenza di eventuali guardians installato in locale eseguendo [Get-HgsGuardian](https://docs.microsoft.com/en-us/powershell/module/hgsclient/get-hgsguardian?view=win10-ps). Guardians proprietario dispongono di chiavi private mentre guardians per il tuo Data Center in genere non lo è.

Se è necessario creare una sorveglianza del proprietario, eseguire il comando seguente:

```powershell
New-HgsGuardian -Name "Owner" -GenerateCertificates
```

Questo comando crea una coppia di certificati di firma e crittografia nell'archivio certificati del computer locale nella cartella "Certificati locale della macchina virtuale schermato".
Saranno necessari i certificati del proprietario e le relative chiavi private per unshield una macchina virtuale, pertanto è necessario verificare questi certificati vengono sottoposti a backup e protetti dal rischio di furto.
Un utente malintenzionato con accesso ai certificati proprietario possibile utilizzarli per la macchina virtuale schermata di avvio o modificarne la configurazione di sicurezza.

Se si desidera importare le informazioni di sorveglianza da un'infrastruttura sorvegliata in cui si desidera eseguire la macchina virtuale (il Data Center primario, i Data Center di backup e così via), eseguire il comando seguente per ogni [file di metadati recuperati dalle infrastrutture sorvegliate ](#select-trusted-fabrics).

```powershell
Import-HgsGuardian -Name 'EAST-US Datacenter' -Path '.\EastUSGuardian.xml'
```

> [!TIP]
> Se sono utilizzati i certificati autofirmati o certificati registrati con HGS è scaduto, potrebbe essere necessario usare il `-AllowUntrustedRoot` e/o `-AllowExpired` flag con il comando Import-HgsGuardian per ignorare i controlli di sicurezza.

È necessario anche per [ottenere un catalogo delle firme del volume](#get-the-volume-signature-catalog-file) per ogni disco di modello da usare con questo file di dati di schermatura e un [file di risposte i dati di schermatura](#create-an-answer-file) per consentire al sistema operativo completare il specializzazione attività automaticamente.
Infine, decidere se la macchina virtuale deve essere completamente schermate o semplicemente abilitate per vTPM.
Uso `-Policy Shielded` per una macchina virtuale schermata completamente o `-Policy EncryptionSupported` per macchina virtuale che consente le connessioni di console di base e PowerShell Direct è abilitato un vTPM.

Quando tutto è pronto, eseguire il comando seguente per creare il file di dati di schermatura:

```powershell
$viq = New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\marketing-ws2016.vsc' -VersionRule Equals
New-ShieldingDataFile -ShieldingDataFilePath "C:\temp\Marketing-LBI.pdk" -Policy EncryptionSupported -Owner 'Owner' -Guardian 'EAST-US Datacenter' -VolumeIDQualifier $viq -AnswerFile 'C:\temp\marketing-ws2016-answerfile.xml'
```

Nel comando precedente, sarà in grado di modificare la configurazione della sicurezza della macchina virtuale in futuro, mentre 'Data Center degli Stati Uniti ORIENTALI' possono eseguire la macchina virtuale ma non modificare le impostazioni di sorveglianza denominata "Owner" (ottenuto da Get-HgsGuardian).
Se si dispone di più di un tutore, separati i nomi dei sorveglianti con virgole come `'EAST-US Datacenter', 'EMEA Datacenter'`.
Il qualificatore ID del volume consente di specificare se si considera attendibile solo la versione esatta (Equals) del disco modello o nelle versioni future (GreaterThanOrEquals) nonché.
Il nome del disco e un certificato di firma devono corrispondere esattamente per il confronto tra le versioni prendere in considerazione in fase di distribuzione.
Si può considerare attendibili più di un disco modello, fornendo i qualificatori di ID per un elenco delimitato da virgole del volume di `-VolumeIDQualifier` parametro.
Infine, se si dispone di altri file che devono accompagnare il file di risposte con la macchina virtuale, usare il `-OtherFile` parametro e specificare un elenco delimitato da virgole di percorsi di file.

Vedere la documentazione del cmdlet relativa [New-ShieldingDataFile](https://docs.microsoft.com/en-us/powershell/module/shieldedvmdatafile/New-ShieldingDataFile?view=win10-ps) e [New-VolumeIDQualifier](https://docs.microsoft.com/en-us/powershell/module/shieldedvmdatafile/New-VolumeIDQualifier?view=win10-ps) per scoprire altri modi per configurare il file di dati di schermatura.

## <a name="see-also"></a>Vedere anche

- [Distribuire macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
