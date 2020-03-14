---
title: VM schermate per i tenant-creazione di dati di schermatura per definire una macchina virtuale schermata
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 49f4e84d-c1f7-45e5-9143-e7ebbb2ef052
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: 26ff5e27494e2f42a0c8e4d28e2b9820f8d19e6a
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322463"
---
# <a name="shielded-vms-for-tenants---creating-shielding-data-to-define-a-shielded-vm"></a>VM schermate per i tenant-creazione di dati di schermatura per definire una macchina virtuale schermata

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Un file di dati di schermatura (chiamato anche file di dati di provisioning o file PDK) è un file crittografato creato dal tenant o dal proprietario di una macchina virtuale per proteggere le informazioni di configurazione importanti di una macchina virtuale, ad esempio la password dell'amministratore, RDP e altri certificati di identità, le credenziali di aggiunta al dominio e così via. In questo argomento vengono fornite informazioni su come creare un file di dati di schermatura. Prima di poter creare il file, è necessario ottenere un disco modello dal provider di servizi di hosting oppure creare un disco modello come descritto in [VM schermate per i tenant-creazione di un disco modello (facoltativo)](guarded-fabric-tenant-creates-template-disk.md).

Per un elenco e un diagramma del contenuto di un file di dati di schermatura, vedere [che cos'è la schermatura dei dati e perché è necessaria?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

> [!IMPORTANT]
> I passaggi descritti in questa sezione devono essere completati in un computer separato e attendibile al di fuori dell'infrastruttura sorvegliata. In genere, il proprietario della macchina virtuale (tenant) creerebbe i dati di schermatura per le macchine virtuali, non gli amministratori dell'infrastruttura.

Per preparare la creazione di un file di dati di schermatura, seguire questa procedura:

- [Ottenere un certificato per Connessione Desktop remoto](#optional-obtain-a-certificate-for-remote-desktop-connection)
- [Creazione di un file di risposte](#create-an-answer-file)
- [Ottenere il file di catalogo della firma del volume](#get-the-volume-signature-catalog-file)
- [Seleziona infrastrutture attendibili](#select-trusted-fabrics)

Quindi, è possibile creare il file di dati di schermatura:

- [Creare un file di dati di schermatura e aggiungere tutori](#create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard)

## <a name="optional-obtain-a-certificate-for-remote-desktop-connection"></a>Opzionale Ottenere un certificato per Connessione Desktop remoto

Poiché i tenant sono in grado di connettersi alle VM schermate solo usando Connessione Desktop remoto o altri strumenti di gestione remota, è importante assicurarsi che i tenant possano verificare la connessione all'endpoint corretto (ovvero, non esiste un "uomo al centro" intercettazione della connessione.

Un modo per verificare la connessione al server previsto consiste nell'installare e configurare un certificato per la Servizi Desktop remoto da presentare quando si avvia una connessione. Il computer client che si connette al server verificherà se considera attendibile il certificato e visualizzerà un avviso in caso contrario. In genere, per garantire che il client che esegue la connessione consideri attendibile il certificato, i certificati RDP vengono emessi dall'infrastruttura a chiave pubblica del tenant. Altre informazioni sull' [uso dei certificati in Servizi Desktop remoto](https://technet.microsoft.com/library/dn781533.aspx) sono disponibili su TechNet.

 Per decidere se è necessario ottenere un certificato RDP personalizzato, tenere presente quanto segue:

- Se si stanno semplicemente testando VM schermate in un ambiente lab, **non** è necessario un certificato RDP personalizzato.
- Se la macchina virtuale è configurata per l'aggiunta a un dominio Active Directory, un certificato del computer viene in genere emesso automaticamente dall'autorità di certificazione dell'organizzazione e utilizzato per identificare il computer durante le connessioni RDP. **Non** è necessario un certificato RDP personalizzato.
- Se la macchina virtuale non è aggiunta a un dominio, ma si vuole un modo per verificare la connessione al computer corretto quando si usa Desktop remoto, è **consigliabile** usare certificati RDP personalizzati.

> [!TIP]
> Quando si seleziona un certificato RDP da includere nel file di dati di schermatura, assicurarsi di usare un certificato con caratteri jolly. Un file di dati di schermatura può essere usato per creare un numero illimitato di macchine virtuali. Poiché ogni macchina virtuale condividerà lo stesso certificato, un certificato con caratteri jolly garantisce che il certificato sarà valido indipendentemente dal nome host della macchina virtuale.

## <a name="create-an-answer-file"></a>Creare un file di risposte

Poiché il disco modello firmato in VMM è generalizzato, è necessario che i tenant forniscano un file di risposte per specializzare le macchine virtuali schermate durante il processo di provisioning. Il file di risposte (spesso denominato file di installazione automatica) può configurare la macchina virtuale per il ruolo desiderato, ovvero può installare le funzionalità di Windows, registrare il certificato RDP creato nel passaggio precedente ed eseguire altre azioni personalizzate. Fornirà inoltre le informazioni necessarie per l'installazione di Windows, tra cui la password dell'amministratore e il codice Product Key predefiniti.

Per informazioni su come ottenere e usare la funzione **New-ShieldingDataAnswerFile** per generare un file di risposte (Unattend. xml file) per la creazione di VM schermate, vedere [generare un file di risposte usando la funzione New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md). Utilizzando la funzione, è possibile generare più facilmente un file di risposte che rifletta le scelte seguenti:

- La macchina virtuale deve essere aggiunta a un dominio al termine del processo di inizializzazione?
- Si usa un contratto multilicenza o un codice Product Key specifico per VM?
- Si sta usando un indirizzo IP statico o DHCP?
- Si userà un certificato di Remote Desktop Protocol personalizzato (RDP) che verrà usato per dimostrare che la macchina virtuale appartiene all'organizzazione?
- Eseguire uno script al termine dell'inizializzazione?

I file di risposte usati nei file di dati di schermatura verranno usati in tutte le macchine virtuali create con il file di dati di schermatura. Pertanto, è necessario assicurarsi di non eseguire il codice rigido delle informazioni specifiche della macchina virtuale nel file di risposte. VMM supporta alcune stringhe di sostituzione (vedere la tabella riportata di seguito) nel file di installazione automatica per gestire i valori di specializzazione che possono passare dalla macchina virtuale alla macchina virtuale. Non è necessario usare questi. Tuttavia, se sono presenti, VMM ne approfitterà.

Quando si crea un file Unattend. XML per le macchine virtuali schermate, tenere presenti le restrizioni seguenti:

- Se si usa VMM per gestire il Data Center, il file di installazione automatica deve causare la disattivazione della macchina virtuale dopo la configurazione. Questo consente a VMM di stabilire quando deve segnalare al tenant che la macchina virtuale ha terminato il provisioning ed è pronta per l'uso. VMM rileverà automaticamente la macchina virtuale dopo che è stata rilevata mentre è stata spenta durante il provisioning.

- Assicurarsi di abilitare RDP e la regola del firewall corrispondente per poter accedere alla macchina virtuale dopo la configurazione. Non è possibile usare la console VMM per accedere alle macchine virtuali schermate, quindi è necessario il protocollo RDP per connettersi alla macchina virtuale. Se si preferisce gestire i sistemi con la comunicazione remota di Windows PowerShell, assicurarsi che WinRM sia abilitato.

- Le uniche stringhe di sostituzione supportate nei file di installazione automatica della VM schermata sono le seguenti:

    | Elemento sostituibile | Stringa di sostituzione |
    |-----------|-----------|
    | ComputerName        | @ComputerName@      |
    | TimeZone            | @TimeZone@          |
    | ProductKey          | @ProductKey@        |
    | IPAddr4-1           | @IP4Addr-1@         |
    | IPAddr6-1           | @IP6Addr-1@         |
    | MACAddr-1           | @MACAddr-1@         |
    | Prefisso-1-1          | @Prefix-1-1@        |
    | NextHop-1-1         | @NextHop-1-1@       |
    | Prefisso-1-2          | @Prefix-1-2@        |
    | NextHop-1-2         | @NextHop-1-2@       |

    Se è presente più di una scheda di interfaccia di rete, è possibile aggiungere più stringhe di sostituzione per la configurazione IP incrementando la prima cifra. Ad esempio, per impostare l'indirizzo IPv4, la subnet e il gateway per 2 schede di rete, è necessario usare le stringhe di sostituzione seguenti:

    | Stringa di sostituzione | Esempio di sostituzione |
    |---------------------|----------------------|
    | @IP4Addr-1@         | 192.168.1.10/24      |
    | @MACAddr-1@         | Ethernet             |
    | @Prefix-1-1@        | 24                   |
    | @NextHop-1-1@       | 192.168.1.254        |
    | @IP4Addr-2@         | 10.0.20.30/24        |
    | @MACAddr-2@         | Ethernet 2           |
    | @Prefix-2-1@        | 24                   |
    | @NextHop-2-1@       | 10.0.20.1            |

Quando si usano le stringhe di sostituzione, è importante assicurarsi che le stringhe vengano popolate durante il processo di provisioning della macchina virtuale. Se una stringa come @ProductKey@ non viene fornita in fase di distribuzione, lasciando vuoto il &lt;ProductKey&gt; nodo nel file di installazione automatica, il processo di specializzazione avrà esito negativo e non sarà possibile connettersi alla macchina virtuale.

Si noti inoltre che le stringhe di sostituzione correlate alla rete verso la fine della tabella vengono utilizzate solo se si utilizzano i pool di indirizzi IP statici di VMM. Il provider di servizi di hosting dovrebbe essere in grado di indicare se queste stringhe di sostituzione sono obbligatorie. Per ulteriori informazioni sugli indirizzi IP statici nei modelli VMM, vedere gli argomenti seguenti nella documentazione di VMM:

- [Linee guida per i pool di indirizzi IP](https://technet.microsoft.com/system-center-docs/vmm/plan/plan-network#guidelines-for-ip-address-pools)
- [Configurare i pool di indirizzi IP statici nell'infrastruttura di VMM](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-network-static-address-pools)

Infine, è importante notare che il processo di distribuzione delle macchine virtuali schermate eseguirà solo la crittografia dell'unità del sistema operativo. Se si distribuisce una macchina virtuale schermata con una o più unità dati, si consiglia vivamente di aggiungere un comando di installazione automatica o Criteri di gruppo impostazione nel dominio tenant per crittografare automaticamente le unità dati.

## <a name="get-the-volume-signature-catalog-file"></a>Ottenere il file di catalogo della firma del volume

I file di dati di schermatura contengono anche informazioni sui dischi del modello che un tenant considera attendibile. I tenant acquisiscono le firme del disco da dischi modello attendibili sotto forma di file VSC (volume Signature Catalog). Queste firme vengono quindi convalidate quando viene distribuita una nuova macchina virtuale. Se nessuna delle firme nel file di dati di schermatura corrisponde al disco del modello che tenta di essere distribuito con la macchina virtuale (ovvero è stato modificato o scambiato con un disco diverso potenzialmente dannoso), il processo di provisioning avrà esito negativo.

> [!IMPORTANT]
> Sebbene il VSC assicuri che un disco non sia stato alterato, è comunque importante che il tenant consideri attendibile il disco. Se si è il tenant e il disco modello viene fornito dal provider di hosting, distribuire una macchina virtuale di test usando il disco del modello ed eseguire i propri strumenti (antivirus, scanner di vulnerabilità e così via) per convalidare che il disco sia, in realtà, in uno stato attendibile.

Esistono due modi per acquisire il VSC di un disco modello:

1. Il provider di hosting (o tenant, se il tenant ha accesso a VMM) USA i cmdlet di PowerShell per VMM per salvare il VSC e assegnarlo al tenant. Questa operazione può essere eseguita su qualsiasi computer con la console VMM installata e configurata per gestire l'ambiente VMM dell'infrastruttura di hosting. I cmdlet di PowerShell per salvare VSC sono i seguenti:

    ```powershell
    $disk = Get-SCVirtualHardDisk -Name "templateDisk.vhdx"

    $vsc = Get-SCVolumeSignatureCatalog -VirtualHardDisk $disk

    $vsc.WriteToFile(".\templateDisk.vsc")
    ```

2. Il tenant può accedere al file su disco del modello. Questa situazione può verificarsi se il tenant crea un disco modello da caricare in un provider di servizi di hosting o se il tenant può scaricare il disco modello del provider di servizi di hosting. In questo caso, senza VMM nell'immagine, il tenant eseguirà il cmdlet seguente (installato con la funzionalità strumenti di VM schermate, parte del Strumenti di amministrazione remota del server):

    ```powershell
    Save-VolumeSignatureCatalog -TemplateDiskPath templateDisk.vhdx -VolumeSignatureCatalogPath templateDisk.vsc
    ```

## <a name="select-trusted-fabrics"></a>Seleziona infrastrutture attendibili

L'ultimo componente nel file di dati di schermatura è correlato al proprietario e ai tutori di una macchina virtuale. I tutori vengono usati per designare il proprietario di una macchina virtuale schermata e le infrastrutture sorvegliate sulle quali è autorizzata l'esecuzione.

Per autorizzare un'infrastruttura di hosting a eseguire una macchina virtuale schermata, è necessario ottenere i metadati del tutore dal servizio sorveglianza host del provider di servizi di hosting. Spesso il provider di servizi di hosting fornirà questi metadati tramite gli strumenti di gestione. In uno scenario aziendale, è possibile avere accesso diretto per ottenere i metadati manualmente.

L'utente o il provider di servizi di hosting può ottenere i metadati del tutore da HGS eseguendo una delle operazioni seguenti:

- Ottenere i metadati di Guardian direttamente da HGS eseguendo il comando di Windows PowerShell seguente o visitando il sito Web e salvando il file XML visualizzato:

    ```powershell
    Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml
    ```

- Ottenere i metadati di Guardian da VMM usando i cmdlet di PowerShell per VMM:

    ```powershell
    $relecloudmetadata = Get-SCGuardianConfiguration
    $relecloudmetadata.InnerXml | Out-File .\RelecloudGuardian.xml -Encoding UTF8
    ```

Ottenere i file di metadati tutor per ogni infrastruttura sorvegliata in cui si vuole autorizzare l'esecuzione delle macchine virtuali schermate prima di continuare.

## <a name="create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard"></a>Creare un file di dati di schermatura e aggiungere tutori usando la creazione guidata file di dati di schermatura

Eseguire la creazione guidata file di dati di schermatura per creare un file di dati di schermatura (PDK). Qui verranno aggiunti il certificato RDP, il file di installazione automatica, i cataloghi delle firme dei volumi, il custode del proprietario e i metadati di sorveglianza scaricati ottenuti nel passaggio precedente.

1. Installare **Strumenti di amministrazione remota del server &gt; strumenti di amministrazione delle funzionalità &gt; gli strumenti di VM schermate** nel computer usando Server Manager o il comando di Windows PowerShell seguente:

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

2. Aprire la creazione guidata file di dati di schermatura dalla sezione strumenti di amministrazione del menu Start o eseguendo l'eseguibile seguente **C:\\Windows\\System32\\ShieldingDataFileWizard. exe**.

3. Nella prima pagina usare la seconda casella di selezione dei file per scegliere un percorso e un nome file per il file di dati di schermatura. In genere, è necessario assegnare un nome a un file di dati di schermatura dopo l'entità che possiede le macchine virtuali create con i dati di schermatura (ad esempio HR, IT, Finance) e il ruolo del carico di lavoro in esecuzione, ad esempio file server, server Web o qualsiasi altro elemento configurato dal file di installazione automatica. Lasciare il pulsante di opzione impostato su **schermatura dei dati per i modelli schermati**.

    > [!NOTE]
    > Nella procedura guidata file di dati di schermatura si noteranno le due opzioni seguenti:
    >- **Schermatura dei dati per i modelli schermati**
    >- **Schermatura dei dati per le macchine virtuali esistenti e i modelli non schermati**<br>
    > La prima opzione viene usata per la creazione di nuove macchine virtuali schermate da modelli schermati. La seconda opzione consente di creare dati di schermatura che possono essere usati solo per la conversione di macchine virtuali esistenti o per la creazione di macchine virtuali schermate da modelli non schermati.

    ![Creazione guidata file di dati di schermatura, selezione file](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-01.png)

    Inoltre, è necessario scegliere se le macchine virtuali create con questo file di dati di schermatura saranno realmente schermate o configurate in modalità "crittografia supportata". Per altre informazioni su queste due opzioni, vedere [quali sono i tipi di macchine virtuali che un'infrastruttura sorvegliata può eseguire?](guarded-fabric-and-shielded-vms.md#what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run).

    > [!IMPORTANT]
    > Prestare attenzione al passaggio successivo, in quanto definisce il proprietario delle macchine virtuali schermate e quali infrastrutture saranno autorizzate per l'esecuzione delle macchine virtuali schermate.<br>Il possesso del **tutore proprietario** è necessario per modificare successivamente una macchina virtuale schermata esistente da **schermate** a **crittografia supportata** o viceversa.

4. L'obiettivo di questo passaggio è due volte:

    - Creare o selezionare un tutore del proprietario che rappresenti il proprietario della macchina virtuale

    - Importare il tutore scaricato dal servizio sorveglianza host del provider di hosting nel passaggio precedente

    Per designare un tutore proprietario esistente, selezionare il tutore appropriato dal menu a discesa. Solo i tutori installati nel computer locale con le chiavi private integre vengono visualizzati nell'elenco. È anche possibile creare un tutore proprietario selezionando **Gestisci tutori locali** nell'angolo in basso a destra e facendo clic su **Crea** e completa la procedura guidata.

    Si importano quindi i metadati di Guardian scaricati in precedenza usando la pagina **Owner e Guardians** . Selezionare **Gestisci tutori locali** nell'angolo in basso a destra. Usare la funzionalità di **importazione** per importare il file di metadati di Guardian. Dopo aver importato o aggiunto tutti i tutori necessari, fare clic su **OK** . Come procedura consigliata, denominare tutori dopo il provider di servizi di hosting o il data center aziendale che rappresentano. Infine, selezionare tutti i tutori che rappresentano i Data Center in cui è autorizzata l'esecuzione della macchina virtuale schermata. Non è necessario selezionare di nuovo il tutore del proprietario. Al termine, fare clic su **Avanti** .

    ![Creazione guidata file di dati di schermatura, proprietario e tutori](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-02.png)

5. Nella pagina qualificatori ID volume fare clic su **Aggiungi** per autorizzare un disco modello firmato nel file di dati di schermatura. Quando si seleziona un VSC nella finestra di dialogo, vengono visualizzate le informazioni relative al nome, alla versione e al certificato utilizzati per la firma del disco. Ripetere questo processo per ogni disco modello che si vuole autorizzare.

6. Nella pagina **valori di specializzazione** fare clic su **Sfoglia** per selezionare il file Unattend. XML che verrà usato per specializzare le macchine virtuali.

    Usare il pulsante **Aggiungi** nella parte inferiore per aggiungere eventuali file aggiuntivi al PDK necessario durante il processo di specializzazione. Se, ad esempio, il file di installazione automatica installa un certificato RDP nella macchina virtuale (come descritto in [generare un file di risposte tramite la funzione New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md)), è necessario aggiungere il file pfx del certificato RDP e lo script RDPCertificateConfig. ps1. Si noti che tutti i file specificati verranno copiati automaticamente in C:\\Temp\\ nella macchina virtuale creata. Il file di installazione automatica dovrebbe aspettarsi che i file si trovino in tale cartella quando vi si fa riferimento in base al percorso.

7. Controllare le selezioni effettuate nella pagina successiva, quindi fare clic su **genera**.

8. Chiudere la procedura guidata dopo che è stata completata.

## <a name="create-a-shielding-data-file-and-add-guardians-using-powershell"></a>Creare un file di dati di schermatura e aggiungere tutori usando PowerShell

In alternativa alla creazione guidata file di dati di schermatura, è possibile eseguire [New-ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/new-shieldingdatafile?view=win10-ps) per creare un file di dati di schermatura.

Tutti i file di dati di schermatura devono essere configurati con i certificati di proprietario e tutore corretti per autorizzare l'esecuzione delle macchine virtuali schermate in un'infrastruttura sorvegliata.
È possibile controllare se sono presenti tutori installati localmente eseguendo [Get-HgsGuardian](https://docs.microsoft.com/powershell/module/hgsclient/get-hgsguardian?view=win10-ps). I tutori dei proprietari hanno chiavi private, ma in genere i tutori per il Data Center.

Se è necessario creare un tutore del proprietario, eseguire il comando seguente:

```powershell
New-HgsGuardian -Name "Owner" -GenerateCertificates
```

Questo comando crea una coppia di certificati di firma e crittografia nell'archivio certificati del computer locale nella cartella "certificati locali della VM schermata".
Per evitare la schermatura di una macchina virtuale, è necessario disporre dei certificati proprietario e delle chiavi private corrispondenti, quindi assicurarsi che i certificati vengano sottoposti a backup e protetti da furti.
Un utente malintenzionato con accesso ai certificati proprietario può usarli per avviare la macchina virtuale schermata o modificare la configurazione di sicurezza.

Se è necessario importare informazioni sul tutore da un'infrastruttura sorvegliata in cui si vuole eseguire la macchina virtuale (il data center principale, i Data Center di backup e così via), eseguire il comando seguente per ogni [file di metadati recuperato dalle infrastrutture sorvegliate](#select-trusted-fabrics).

```powershell
Import-HgsGuardian -Name 'EAST-US Datacenter' -Path '.\EastUSGuardian.xml'
```

> [!TIP]
> Se sono stati usati certificati autofirmati o i certificati registrati con HGS sono scaduti, potrebbe essere necessario usare i flag `-AllowUntrustedRoot` e/o `-AllowExpired` con il comando Import-HgsGuardian per ignorare i controlli di sicurezza.

Sarà anche necessario [ottenere un catalogo di firma del volume](#get-the-volume-signature-catalog-file) per ogni disco modello che si vuole usare con questo file di dati di schermatura e un [file di risposta dei dati di schermatura](#create-an-answer-file) per consentire al sistema operativo di completare automaticamente le proprie attività di specializzazione.
Infine, decidere se si vuole che la macchina virtuale sia completamente schermata o semplicemente abilitata per vTPM.
Usare `-Policy Shielded` per una VM completamente schermata o `-Policy EncryptionSupported` per una macchina virtuale abilitata per vTPM che consente le connessioni di base della console e PowerShell Direct.

Quando tutto è pronto, eseguire il comando seguente per creare il file di dati di schermatura:

```powershell
$viq = New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\marketing-ws2016.vsc' -VersionRule Equals
New-ShieldingDataFile -ShieldingDataFilePath "C:\temp\Marketing-LBI.pdk" -Policy EncryptionSupported -Owner 'Owner' -Guardian 'EAST-US Datacenter' -VolumeIDQualifier $viq -AnswerFile 'C:\temp\marketing-ws2016-answerfile.xml'
```

> [!TIP]
> Se si usa un certificato RDP personalizzato, chiavi SSH o altri file che devono essere inclusi nel file di dati di schermatura, usare il parametro `-OtherFile` per includerli. È possibile specificare un elenco di percorsi di file delimitati da virgole, ad esempio `-OtherFile "C:\source\myRDPCert.pfx", "C:\source\RDPCertificateConfig.ps1"`

Nel comando precedente, il Guardian denominato "Owner" (ottenuto da Get-HgsGuardian) potrà modificare la configurazione di sicurezza della macchina virtuale in futuro, mentre "EAST-US datacenter" è in grado di eseguire la macchina virtuale, ma non di modificarne le impostazioni.
Se si dispone di più di un tutore, separare i nomi dei tutori con virgole come `'EAST-US Datacenter', 'EMEA Datacenter'`.
Il qualificatore ID volume specifica se si considera attendibile solo la versione esatta (uguale) del disco modello o delle versioni future (GreaterThanOrEquals).
Il nome del disco e il certificato di firma devono corrispondere esattamente per il confronto delle versioni da considerare in fase di distribuzione.
È possibile considerare attendibile più di un disco modello fornendo un elenco delimitato da virgole di qualificatori ID volume al parametro `-VolumeIDQualifier`.
Infine, se sono presenti altri file che devono accompagnare il file di risposte con la macchina virtuale, usare il parametro `-OtherFile` e fornire un elenco delimitato da virgole di percorsi di file.

Per informazioni su altri modi per configurare il file di dati di schermatura, vedere la documentazione del cmdlet [New-ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-ShieldingDataFile?view=win10-ps) e [New-VolumeIDQualifier](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-VolumeIDQualifier?view=win10-ps) .

## <a name="see-also"></a>Vedere anche

- [Distribuire macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
