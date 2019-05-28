---
title: Distribuire spazi di archiviazione in un server autonomo
description: Viene descritto come distribuire spazi di archiviazione in un server autonomo basato su Windows Server 2012.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-spaces
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7090657a0936aed0f4b2e79007f69d7b082b0b8f
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/24/2019
ms.locfileid: "63750656"
---
# <a name="deploy-storage-spaces-on-a-stand-alone-server"></a>Distribuire spazi di archiviazione in un server autonomo

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene descritto come distribuire spazi di archiviazione in un server autonomo. Per informazioni su come creare uno spazio di archiviazione in cluster, vedere [distribuire un cluster di spazi di archiviazione in Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>).

Per creare uno spazio di archiviazione, creare prima di tutto uno o più pool di archiviazione. Un pool di archiviazione è una raccolta di dischi fisici che consente l'aggregazione dell'archiviazione, l'espansione flessibile della capacità e l'amministrazione delegata.

Da un pool di archiviazione si possono creare uno o più dischi virtuali. Questi dischi virtuali vengono anche chiamati *spazi di archiviazione*. Uno spazio di archiviazione viene visualizzato nel sistema operativo Windows come un normale disco da cui è possibile creare volumi formattati. Quando si crea un disco virtuale dall'interfaccia utente Servizi file e archiviazione, è possibile configurare il tipo di resilienza (semplice, mirror o parità), il tipo di provisioning (thin o fisso) e le dimensioni. Con Windows PowerShell si possono aggiungere altri parametri, ad esempio il numero di colonne, il valore di interfoliazione e i dischi fisici del pool da usare. Per informazioni su questi parametri aggiuntivi, vedere [New-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/new-virtualdisk?view=win10-ps) e [Cosa sono le colonne e come viene stabilito quante usarne in Spazi di archiviazione?](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx%23what_are_columns_and_how_does_storage_spaces_decide_how_many_to_use) nelle domande frequenti su Spazi di archiviazione.

>[!NOTE]
>È possibile usare uno spazio di archiviazione per ospitare il sistema operativo Windows.

Da un disco virtuale si possono creare uno o più volumi. Quando si crea un volume, è possibile configurare la dimensione lettera di unità o cartella, file system (file system NTFS o Resilient File System (ReFS)), dimensione unità di allocazione e un'etichetta di volume facoltativa.

Nella seguente figura è illustrato il flusso di lavoro per gli spazi di archiviazione.

![Flusso di lavoro per spazi di archiviazione](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**Figura 1: Flusso di lavoro spazi di archiviazione**

>[!NOTE]
>Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per altre informazioni, vedere [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6).

## <a name="prerequisites"></a>Prerequisiti

Per usare spazi di archiviazione in un server 2012−based Server Windows autonomo, verificare che i dischi fisici che si desidera usare soddisfino i prerequisiti seguenti.

> [!IMPORTANT]
> Se si vuole imparare a distribuire spazi di archiviazione in un cluster di failover, vedere [distribuire un cluster di spazi di archiviazione in Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>). Una distribuzione di cluster di failover ha prerequisiti diversi, ad esempio tipi di bus del disco supportate, tipi di resilienza supportati e il numero minimo necessario di dischi.

|Area|Requisito|Note|
|---|---|---|
|Tipi di bus del disco|-Serial Attached SCSI (SAS)<br>-Serie Advanced Technology Attachment (SATA)<br>-iSCSI e Fibre Channel controller. |Possono essere usate anche unità USB. Non è tuttavia ottimale da usare unità USB in un ambiente server.<br>Spazi di archiviazione è supportata su iSCSI e Fibre Channel (FC) controller, purché i dischi virtuali creati in base a essi sono non resilienza (semplice con un numero qualsiasi di colonne).<br>|
|Configurazione del disco|-Dischi fisici devono essere almeno 4 GB<br>-I dischi devono essere vuoti e non formattati. Non creare volumi.||
|Considerazioni HBA|-È consigliabile usare schede bus host semplice (HBA) che non supportano la funzionalità RAID<br>-Se RAID in grado di supportare, schede bus host deve essere in modalità non RAID con tutte le funzionalità RAID disabilitate<br>-Gli adapter non devono estrarre i dischi fisici, memorizzare i dati o oscurare eventuali dispositivi collegati. Questo include i servizi enclosure forniti dai dispositivi JBOD (Just-a-Bunch-Of-Disks) collegati. |Spazi di archiviazione è compatibile con le schede HBA solo se sono state completamente disabilitate tutte le funzionalità RAID.|
|Enclosure JBOD|-Chassis JBOD sono facoltativi<br>-È consigliabile usare spazi di archiviazione certified enclosure elencate nel catalogo di Windows Server<br>-Se si usa un'enclosure JBOD, verificare con il fornitore dell'archiviazione che l'enclosure supporta gli spazi di archiviazione per garantire funzionalità complete<br>-Per determinare se l'enclosure JBOD supporta l'enclosure e l'identificazione slot, eseguire il cmdlet di Windows PowerShell seguente:<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>Se il **EnclosureNumber** e **SlotNumber** campi contengono valori e quindi lo chassis supporta queste funzionalità.||

Per pianificare il numero di dischi fisici e il tipo di resilienza desiderato per una distribuzione di server autonomi, usare le seguenti linee guida.

|Tipo di resilienza|Requisiti dei dischi|Quando utilizzarlo|
|---|---|---|
|**Semplice**<br><br>-Data vengono archiviati con striping nei dischi fisici<br>-Ottimizza la capacità del disco e aumenta la velocità effettiva<br>-Nessuna resilienza (non protegge da errori del disco)<br><br><br><br><br><br><br>|Richiede almeno un disco fisico.|Non usarlo per ospitare dati non sostituibili. Spazi semplici non offrono protezione contro errori del disco.<br><br>Usarlo per ospitare a costi ridotti dati temporanei o facilmente ricreabili.<br><br>Ideale per carichi di lavoro ad alte prestazioni dove la resilienza non è necessaria o è già fornita dall'applicazione.|
|**Mirror**<br><br>-Archivia due o tre copie dei dati nel set di dischi fisici<br>-Aumenta l'affidabilità, ma riduce la capacità. La duplicazione viene eseguita a ogni scrittura. Anche uno spazio mirror esegue l'archiviazione con striping dei dati in più unità fisiche.<br>-Maggiore velocità effettiva dei dati e una minore latenza di accesso di parità<br>-Usa dirty (DRT) per tenere traccia delle modifiche ai dischi nel pool di monitoraggio dell'area. Quando il sistema si riprende dopo un arresto non pianificato e gli spazi tornano online, DRT ripristina la coerenza dei dischi nel pool.|Richiede almeno due dischi fisici per proteggere dall'errore di un disco.<br><br>Richiede almeno cinque dischi fisici per proteggere dall'errore simultaneo di due dischi.|Usarlo nella maggior parte delle distribuzioni. Ad esempio, gli spazi mirror sono adatti per una condivisione file generica o per una libreria del disco rigido virtuale (VHD).|
|**Parità**<br><br>-Informazioni di dati e la parità vengono archiviati con striping nei dischi fisici<br>-Aumenta l'affidabilità quando viene confrontato con uno spazio semplice, ma riduce la capacità<br>-Consente di aumentare la resilienza tramite inserimento nel journal. che contribuisce a evitare il danneggiamento dei dati in caso di arresto non pianificato.|Richiede almeno tre dischi fisici per proteggere dall'errore di un disco.|Usarlo per carichi di lavoro altamente sequenziali, ad esempio archiviazione o backup.|

## <a name="step-1-create-a-storage-pool"></a>Passaggio 1: Creare un pool di archiviazione

Prima di tutto, i dischi fisici disponibili devono essere raggruppati in uno o più pool di archiviazione.

1. Nel riquadro di spostamento di Server Manager, selezionare **servizi File e archiviazione**.

2. Nel riquadro di spostamento, selezionare la **pool di archiviazione** pagina.
    
    Per impostazione predefinita, i dischi disponibili sono inclusi in un pool denominato pool *originale*. Se non sono elencati pool originali in **POOL DI ARCHIVIAZIONE**, l'archiviazione non soddisfa i requisiti per Spazi di archiviazione. Verificare che il disco soddisfi i requisiti descritti nella sezione Prerequisiti.
    
    >[!TIP]
    >Se si seleziona il pool di archiviazione **Primordial**, i dischi fisici disponibili vengono elencati in **DISCHI FISICI**.

3. Sotto **pool di archiviazione**, selezionare la **attività** elenco e quindi selezionare **nuovo Pool di archiviazione**. Verrà aperta la creazione guidata nuovo Pool di archiviazione.

4. Nel **prima di iniziare** pagina, selezionare **successivo**.

5. Nel **specificare un nome di pool di archiviazione e sottosistema** pagina, immettere un nome e una descrizione facoltativa per il pool di archiviazione, selezionare il gruppo di dischi fisici disponibili che si desidera usare e quindi selezionare **successivo**.

6. Nel **Seleziona dischi fisici del pool di archiviazione** pagina, effettuare le operazioni seguenti e quindi selezionare **successivo**:
    
    1. Selezionare la casella di controllo accanto a ogni disco fisico da includere nel pool di archiviazione.
    
    2. Se si vuole designare in uno o più dischi come riserve a caldo **allocazione**, selezionare la freccia a discesa, quindi selezionare **riserva a caldo**.

7. Nel **Conferma selezioni** pagina, verificare che le impostazioni siano corrette e quindi selezionare **crea**.

8. Nel **visualizzare i risultati** pagina, verificare che tutte le attività completate e quindi selezionare **Chiudi**.
    
    >[!NOTE]
    >Facoltativamente, per passare direttamente al passaggio successivo, è possibile selezionare la casella di controllo **Crea un disco virtuale alla chiusura della procedura guidata**.

9. In **POOL DI ARCHIVIAZIONE**, verificare che il nuovo pool di archiviazione sia presente nell'elenco.

### <a name="windows-powershell-equivalent-commands-for-creating-storage-pools"></a>Comandi equivalenti di Windows PowerShell per la creazione di pool di archiviazione

Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.

Nel seguente esempio vengono visualizzati i dischi fisici disponibili nel pool originale.

```PowerShell
Get-StoragePool -IsPrimordial $true | Get-PhysicalDisk | Where-Object CanPool -eq $True
```

L'esempio seguente crea un nuovo pool di archiviazione denominato *StoragePool1* che Usa tutti i dischi disponibili.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk –CanPool $True)
```

L'esempio seguente crea un nuovo pool di archiviazione *StoragePool1*, che usa quattro dei dischi disponibili.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk PhysicalDisk1, PhysicalDisk2, PhysicalDisk3, PhysicalDisk4)
```

La seguente sequenza di cmdlet di esempio mostra come aggiungere un disco fisico disponibile, *PhysicalDisk5*, come riserva a caldo al pool di archiviazione *StoragePool1*.

```PowerShell
$PDToAdd = Get-PhysicalDisk –FriendlyName PhysicalDisk5
Add-PhysicalDisk –StoragePoolFriendlyName StoragePool1 –PhysicalDisks $PDToAdd –Usage HotSpare
```

## <a name="step-2-create-a-virtual-disk"></a>Passaggio 2: Creare un disco virtuale

In questo passaggio è necessario creare uno o più dischi virtuali dal pool di archiviazione. Quando si crea un disco virtuale, è possibile selezionare come disporre i dati nei dischi fisici. Questa operazione ha effetti sia sull'affidabilità che sulle prestazioni. Si può anche scegliere se creare dischi con thin provisioning o provisioning fisso.

1. Se la Creazione guidata nuovo disco virtuale non è aperta, nella pagina **Pool di archiviazione** di Server Manager, in **POOL DI ARCHIVIAZIONE**, verificare che sia selezionato il pool di archiviazione desiderato.

2. Sotto **dischi VIRTUALI**, selezionare la **attività** elenco e quindi selezionare **nuovo disco virtuale**. Verrà aperta la creazione guidata disco virtuale.

3. Nel **prima di iniziare** pagina, selezionare **successivo**.

4. Nel **selezionare il pool di archiviazione** pagina, selezionare il pool di archiviazione desiderato e quindi selezionare **successivo**.

5. Nel **specificare il nome del disco virtuale** pagina, immettere un nome e una descrizione facoltativa, quindi selezionare **successivo**.

6. Nel **selezionare il layout di archiviazione** pagina, selezionare il layout desiderato, quindi selezionare **successivo**.
    
    >[!NOTE]
    >Se si seleziona un layout in cui non è sufficiente dischi fisici, si riceverà un messaggio di errore quando si seleziona **successivo**. Per informazioni sul layout da usare e i requisiti del disco, vedere [prerequisiti](#prerequisites)).

7. Se è stato selezionato **Mirror** come il layout di archiviazione e avere cinque o più dischi nel pool, il **configurare le impostazioni di resilienza** verrà visualizzata la pagina. Selezionare una delle opzioni seguenti:
    
      - **Mirroring a 2 vie**
      - **Mirroring a 3 vie**

8. Nel **specificare il tipo di provisioning** pagina, selezionare una delle opzioni seguenti, quindi selezionare **successivo**.
    
      - **Thin**
        
        Con il thin provisioning, lo spazio viene allocato in base alla necessità. In questo modo viene ottimizzato l'uso dello spazio di archiviazione disponibile. Tuttavia è necessario monitorare attentamente lo spazio su disco disponibile perché questa opzione consente di allocare lo spazio di archiviazione in eccesso.
    
      - **Fixed**
        
        Con il provisioning fisso, la capacità di archiviazione viene allocata immediatamente, durante la creazione del disco virtuale. Il provisioning fisso, quindi, usa uno spazio del pool di archiviazione pari alle dimensioni del disco virtuale.
    
    >[!TIP]
    >Con gli spazi di archiviazione è possibile creare dischi virtuali con thin provisioning e provisioning fisso nello stesso pool di archiviazione. Ad esempio, si può usare un disco virtuale con thin provisioning per ospitare un database e un disco virtuale con provisioning fisso per i file di log associati.

9. Nella pagina **Specificare le dimensioni del disco virtuale**, eseguire le operazioni seguenti:
    
    Se è stato selezionato il thin provisioning nel passaggio precedente, nelle **dimensione disco virtuale** casella, immettere una dimensione disco virtuale, selezionare le unità di misura (**MB**, **GB**, o **TB** ), quindi selezionare **successivo**.
    
    Se si seleziona il provisioning fisso nel passaggio precedente, selezionare una delle operazioni seguenti:
    
      - **Specifica dimensioni**
        
        Per specificare una dimensione, immettere un valore nel **dimensione disco virtuale** casella, quindi selezionare le unità (**MB**, **GB**, oppure **TB**).
        
        Se non si usa un layout di archiviazione di tipo semplice, il disco virtuale richiede più spazio libero rispetto alle dimensioni specificate. Per evitare possibili errori nel caso in cui le dimensioni del volume superino lo spazio disponibile nel pool di archiviazione, è possibile selezionare la casella di controllo **Creare un disco virtuale con le dimensioni massime possibili, fino alle dimensioni specificate**.
    
      - **Dimensioni massime**
        
        Selezionare questa opzione per creare un disco virtuale che usi la capacità massima del pool di archiviazione.

10. Nel **Conferma selezioni** pagina, verificare che le impostazioni siano corrette e quindi selezionare **crea**.

11. Nel **visualizzare i risultati** pagina, verificare che tutte le attività completate e quindi selezionare **Chiudi**.
    
    >[!TIP]
    >Per impostazione predefinita, la casella di controllo **Crea un volume al termine della procedura guidata** è selezionata e consente di procedere direttamente al passaggio successivo.

### <a name="windows-powershell-equivalent-commands-for-creating-virtual-disks"></a>Comandi equivalenti di Windows PowerShell per la creazione di dischi virtuali

Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.

L'esempio seguente crea un disco virtuale da 50 GB denominato *VirtualDisk1* su un pool di archiviazione denominato *StoragePool1*.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

L'esempio seguente crea un disco virtuale con mirroring denominato *VirtualDisk1* su un pool di archiviazione denominato *StoragePool1*. Il disco Usa la capacità di archiviazione massima del pool di archiviazione.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –ResiliencySettingName Mirror –UseMaximumSize
```

L'esempio seguente crea un disco virtuale da 50 GB denominato *VirtualDisk1* in un pool di archiviazione denominato *StoragePool1*. Il disco usa il tipo di provisioning thin.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB) –ProvisioningType Thin
```

L'esempio seguente crea un disco virtuale denominato *VirtualDisk1* su un pool di archiviazione denominato *StoragePool1*. Il disco virtuale usa il mirroring a tre vie e ha una dimensione fissa di 20 GB.

>[!NOTE]
>Per il funzionamento di questo cmdlet sono necessari almeno cinque dischi fisici nel pool di archiviazione. In questo numero non sono inclusi i dischi allocati come riserve a caldo.

```PowerShell
New-VirtualDisk -StoragePoolFriendlyName StoragePool1 -FriendlyName VirtualDisk1 -ResiliencySettingName Mirror -NumberOfDataCopies 3 -Size 20GB -ProvisioningType Fixed
```

## <a name="step-3-create-a-volume"></a>Passaggio 3: Creare un volume

In questo passaggio è necessario creare un volume dal disco virtuale. È possibile assegnare una lettera di unità facoltativa o una cartella, quindi formattare il volume con un file system.

1. Se la creazione guidata nuovo Volume non è già aperta, scegliere il **pool di archiviazione** pagina in Server Manager, nella sezione **dischi VIRTUALI**, fare doppio clic su disco virtuale desiderato e quindi selezionare **nuovo Volume**.
    
    Si apre la Creazione guidata nuovo volume.

2. Nel **prima di iniziare** pagina, selezionare **successivo**.

3. Nel **selezionare il server e il disco** pagina, effettuare le operazioni seguenti e quindi selezionare **successivo**.
    
    1. Nel **Server** area, selezionare il server in cui si desidera eseguire il provisioning del volume.
    
    2. Nel **disco** area, selezionare il disco virtuale in cui si desidera creare il volume.

4. Nel **specificare le dimensioni del volume** pagina, immettere una dimensione del volume, specificare le unità (**MB**, **GB**, oppure **TB**) e quindi scegliere **Successivo**.

5. Nel **assegnare a una lettera di unità o cartella** pagina, configurare l'opzione desiderata e quindi selezionare **successivo**.

6. Nel **selezionare le impostazioni file system** pagina, effettuare le operazioni seguenti e quindi selezionare **successivo**.
    
    1. Nel **File system** elencare, selezionare **NTFS** oppure **ReFS**.
    
    2. Nell'elenco **Dimensioni unità di allocazione**, lasciare l'impostazione su **Impostazione predefinita** o impostare le dimensioni dell'unità di allocazione.
        
        >[!NOTE]
        >Per altre informazioni sulle dimensioni dell'unità di allocazione, vedere [Dimensioni di cluster predefinite per NTFS, FAT ed exFAT](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat).

    
    3. Facoltativamente, nella casella **Etichetta volume**, immettere un'etichetta di volume, ad esempio **Dati RU**.

7. Nel **Conferma selezioni** pagina, verificare che le impostazioni siano corrette e quindi selezionare **crea**.

8. Nel **visualizzare i risultati** pagina, verificare che tutte le attività completate e quindi selezionare **Chiudi**.

9. Per verificare che il volume è stato creato, in Server Manager, selezionare la **volumi** pagina. Il volume viene elencato sotto il server in cui è stato creato. È anche possibile controllare che il volume si trovi in Esplora risorse.

### <a name="windows-powershell-equivalent-commands-for-creating-volumes"></a>Comandi equivalenti di Windows PowerShell per la creazione di volumi

Il cmdlet di Windows PowerShell seguente esegue la stessa funzione della procedura precedente. Immettere il comando su un'unica riga.

Nel seguente esempio vengono inizializzati i dischi per il disco virtuale *VirtualDisk1*, viene creata una partizione con una lettera di unità assegnata e viene formattato il volume con il file system NTFS predefinito.

```PowerShell
Get-VirtualDisk –FriendlyName VirtualDisk1 | Get-Disk | Initialize-Disk –Passthru | New-Partition –AssignDriveLetter –UseMaximumSize | Format-Volume
```

## <a name="additional-information"></a>Altre informazioni

- [Spazi di archiviazione](overview.md)
- [Cmdlet di archiviazione in Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/index?view=win10-ps)
- [Distribuire spazi di archiviazione in cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11))
- [Domande frequenti (FAQ sugli) spazi di archiviazione](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)
