---
title: Distribuire spazi di archiviazione in un server autonomo
description: Viene descritto come distribuire spazi di archiviazione in un server autonomo basato su Windows Server 2012.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-spaces
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 52e6a4d53271a73bc0913e2ac500c4328f2e7009
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393725"
---
# <a name="deploy-storage-spaces-on-a-stand-alone-server"></a>Distribuire spazi di archiviazione in un server autonomo

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento descrive come distribuire spazi di archiviazione in un server autonomo. Per informazioni su come creare uno spazio di archiviazione in cluster, vedere [distribuire un cluster di spazi di archiviazione in Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>).

Per creare uno spazio di archiviazione, creare prima di tutto uno o più pool di archiviazione. Un pool di archiviazione è una raccolta di dischi fisici che consente l'aggregazione dell'archiviazione, l'espansione flessibile della capacità e l'amministrazione delegata.

Da un pool di archiviazione si possono creare uno o più dischi virtuali. Questi dischi virtuali vengono anche chiamati *spazi di archiviazione*. Uno spazio di archiviazione viene visualizzato nel sistema operativo Windows come un normale disco da cui è possibile creare volumi formattati. Quando si crea un disco virtuale dall'interfaccia utente Servizi file e archiviazione, è possibile configurare il tipo di resilienza (semplice, mirror o parità), il tipo di provisioning (thin o fisso) e le dimensioni. Con Windows PowerShell si possono aggiungere altri parametri, ad esempio il numero di colonne, il valore di interfoliazione e i dischi fisici del pool da usare. Per informazioni su questi parametri aggiuntivi, vedere [New-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/new-virtualdisk?view=win10-ps) e [Cosa sono le colonne e come viene stabilito quante usarne in Spazi di archiviazione?](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx%23what_are_columns_and_how_does_storage_spaces_decide_how_many_to_use) nelle domande frequenti su Spazi di archiviazione.

>[!NOTE]
>Non è possibile usare uno spazio di archiviazione per ospitare il sistema operativo Windows.

Da un disco virtuale si possono creare uno o più volumi. Quando si crea un volume, è possibile configurare le dimensioni, la lettera di unità o la cartella, file system (NTFS file system o ReFS (Resilient file System), le dimensioni dell'unità di allocazione e un'etichetta del volume facoltativa.

Nella seguente figura è illustrato il flusso di lavoro per gli spazi di archiviazione.

![Flusso di lavoro per spazi di archiviazione](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**Figura 1: flusso di lavoro di spazi di archiviazione**

>[!NOTE]
>Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per altre informazioni, vedere [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6).

## <a name="prerequisites"></a>Prerequisiti

Per usare spazi di archiviazione in un server autonomo basato su Windows Server 2012, assicurarsi che i dischi fisici da usare soddisfino i seguenti prerequisiti.

> [!IMPORTANT]
> Per informazioni su come distribuire spazi di archiviazione in un cluster di failover, vedere [distribuire un cluster di spazi di archiviazione in Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>). Una distribuzione di cluster di failover presenta prerequisiti diversi, ad esempio i tipi di bus di disco supportati, i tipi di resilienza supportati e il numero minimo necessario di dischi.

|Area|Requisito|Note|
|---|---|---|
|Tipi di bus del disco|-SAS (Serial Attached SCSI)<br>-SATA (Serial Advanced Technology Attachment)<br>-Controller iSCSI e Fibre Channel. |Possono essere usate anche unità USB. Tuttavia, non è ottimale usare unità USB in un ambiente server.<br>Spazi di archiviazione è supportato nei controller iSCSI e Fibre Channel (FC) a condizione che i dischi virtuali creati sopra di essi non siano resilienti (semplice con un numero qualsiasi di colonne).<br>|
|Configurazione del disco|-I dischi fisici devono essere almeno di 4 GB<br>-I dischi devono essere vuoti e non formattati. Non creare volumi.||
|Considerazioni HBA|-Le schede bus host (HBA) semplici che non supportano la funzionalità RAID sono consigliate<br>-Se supporta RAID, le schede HBA devono essere in modalità non RAID con tutte le funzionalità RAID disabilitate<br>-Gli adapter non devono estrarre i dischi fisici, memorizzare i dati nella cache o nascondere eventuali dispositivi collegati. Questo include i servizi enclosure forniti dai dispositivi JBOD (Just-a-Bunch-Of-Disks) collegati. |Spazi di archiviazione è compatibile con le schede HBA solo se sono state completamente disabilitate tutte le funzionalità RAID.|
|Enclosure JBOD|-Le enclosure JBOD sono facoltative<br>-È consigliabile usare le enclosure certificate per spazi di archiviazione elencate nel catalogo di Windows Server<br>-Se si usa un'enclosure JBOD, verificare con il fornitore dell'archiviazione che l'enclosure supporti gli spazi di archiviazione per garantire la completa funzionalità<br>-Per determinare se l'enclosure JBOD supporta l'enclosure e l'identificazione degli slot, eseguire il cmdlet di Windows PowerShell seguente:<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>Se i campi **EnclosureNumber** e **SlotNumber** contengono valori, l'enclosure supporta queste funzionalità.||

Per pianificare il numero di dischi fisici e il tipo di resilienza desiderato per una distribuzione di server autonomi, usare le seguenti linee guida.

|Tipo di resilienza|Requisiti dei dischi|Quando utilizzarlo|
|---|---|---|
|**Semplice**<br><br>-Esegue lo striping dei dati tra dischi fisici<br>-Massimizza la capacità del disco e aumenta la velocità effettiva<br>-Nessuna resilienza (non protegge da un errore del disco)<br><br><br><br><br><br><br>|Richiede almeno un disco fisico.|Non usarlo per ospitare dati non sostituibili. Gli spazi semplici non proteggono da errori del disco.<br><br>Usarlo per ospitare a costi ridotti dati temporanei o facilmente ricreabili.<br><br>Adatto per carichi di lavoro a prestazioni elevate in cui la resilienza non è richiesta o è già fornita dall'applicazione.|
|**Mirror**<br><br>-Archivia due o tre copie dei dati nel set di dischi fisici<br>-Aumenta l'affidabilità, ma riduce la capacità. La duplicazione viene eseguita a ogni scrittura. Anche uno spazio mirror esegue l'archiviazione con striping dei dati in più unità fisiche.<br>-Maggiore velocità effettiva dei dati e latenza di accesso inferiore rispetto alla parità<br>-Usa il rilevamento dell'area dirty (DRT) per tenere traccia delle modifiche apportate ai dischi nel pool. Quando il sistema si riprende dopo un arresto non pianificato e gli spazi tornano online, DRT ripristina la coerenza dei dischi nel pool.|Richiede almeno due dischi fisici per proteggere dall'errore di un disco.<br><br>Richiede almeno cinque dischi fisici per proteggere dall'errore simultaneo di due dischi.|Usarlo nella maggior parte delle distribuzioni. Ad esempio, gli spazi mirror sono adatti per una condivisione file generica o per una libreria del disco rigido virtuale (VHD).|
|**Parità**<br><br>-Esegue lo striping dei dati e delle informazioni di parità tra dischi fisici<br>-Aumenta l'affidabilità rispetto a uno spazio semplice, ma riduce la capacità<br>-Aumenta la resilienza tramite l'inserimento nel journal. che contribuisce a evitare il danneggiamento dei dati in caso di arresto non pianificato.|Richiede almeno tre dischi fisici per proteggere dall'errore di un disco.|Usarlo per carichi di lavoro altamente sequenziali, ad esempio archiviazione o backup.|

## <a name="step-1-create-a-storage-pool"></a>Passaggio 1: Creare un pool di archiviazione

Prima di tutto, i dischi fisici disponibili devono essere raggruppati in uno o più pool di archiviazione.

1. Nel riquadro di spostamento Server Manager selezionare **Servizi file e archiviazione**.

2. Nel riquadro di spostamento selezionare la pagina **pool di archiviazione** .
    
    Per impostazione predefinita, i dischi disponibili sono inclusi in un pool denominato pool *originale*. Se non sono elencati pool originali in **POOL DI ARCHIVIAZIONE**, l'archiviazione non soddisfa i requisiti per Spazi di archiviazione. Verificare che il disco soddisfi i requisiti descritti nella sezione Prerequisiti.
    
    >[!TIP]
    >Se si seleziona il pool di archiviazione **Primordial**, i dischi fisici disponibili vengono elencati in **DISCHI FISICI**.

3. In **pool di archiviazione**selezionare l'elenco **attività** , quindi selezionare **nuovo pool di archiviazione**. Verrà visualizzata la procedura guidata nuovo pool di archiviazione.

4. Nella pagina **prima di iniziare** selezionare **Avanti**.

5. Nella pagina **specificare un nome e un sottosistema di pool di archiviazione** immettere un nome e una descrizione facoltativa per il pool di archiviazione, selezionare il gruppo di dischi fisici disponibili da usare e quindi fare clic su **Avanti**.

6. Nella pagina **Seleziona dischi fisici per il pool di archiviazione** eseguire le operazioni seguenti e quindi selezionare **Avanti**:
    
    1. Selezionare la casella di controllo accanto a ogni disco fisico da includere nel pool di archiviazione.
    
    2. Se si desidera designare uno o più dischi come riserve a caldo, in **allocazione**selezionare la freccia a discesa, quindi selezionare **riserva a caldo**.

7. Nella pagina **Conferma selezioni** verificare che le impostazioni siano corrette e quindi selezionare **Crea**.

8. Nella pagina **Visualizza risultati** verificare che tutte le attività siano state completate e quindi selezionare **Chiudi**.
    
    >[!NOTE]
    >Facoltativamente, per passare direttamente al passaggio successivo, è possibile selezionare la casella di controllo **Crea un disco virtuale alla chiusura della procedura guidata**.

9. In **POOL DI ARCHIVIAZIONE**, verificare che il nuovo pool di archiviazione sia presente nell'elenco.

### <a name="windows-powershell-equivalent-commands-for-creating-storage-pools"></a>Comandi equivalenti di Windows PowerShell per la creazione di pool di archiviazione

Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.

Nel seguente esempio vengono visualizzati i dischi fisici disponibili nel pool originale.

```PowerShell
Get-StoragePool -IsPrimordial $true | Get-PhysicalDisk | Where-Object CanPool -eq $True
```

Nell'esempio seguente viene creato un nuovo pool di archiviazione denominato *StoragePool1* che usa tutti i dischi disponibili.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk –CanPool $True)
```

Nell'esempio seguente viene creato un nuovo pool di archiviazione, *StoragePool1*, che usa quattro dei dischi disponibili.

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

2. In **dischi virtuali**selezionare l'elenco **attività** , quindi selezionare **nuovo disco virtuale**. Verrà aperta la creazione guidata nuovo disco virtuale.

3. Nella pagina **prima di iniziare** selezionare **Avanti**.

4. Nella pagina **selezionare il pool di archiviazione** selezionare il pool di archiviazione desiderato e quindi fare clic su **Avanti**.

5. Nella pagina **specificare il nome del disco virtuale** immettere un nome e una descrizione facoltativa, quindi fare clic su **Avanti**.

6. Nella pagina **selezionare il layout di archiviazione** selezionare il layout desiderato, quindi fare clic su **Avanti**.
    
    >[!NOTE]
    >Se si seleziona un layout in cui non è presente un numero sufficiente di dischi fisici, quando si seleziona **Avanti**verrà visualizzato un messaggio di errore. Per informazioni sul layout da usare e sui requisiti del disco, vedere [prerequisiti](#prerequisites)).

7. Se è stato selezionato **mirror** come layout di archiviazione e sono presenti cinque o più dischi nel pool, viene visualizzata la pagina **Configura impostazioni resilienza** . Selezionare una delle opzioni seguenti:
    
      - **Mirroring a 2 vie**
      - **Mirroring a 3 vie**

8. Nella pagina **specificare il tipo di provisioning** , selezionare una delle opzioni seguenti, quindi fare clic su **Avanti**.
    
   - **Thin**
        
     Con il thin provisioning, lo spazio viene allocato in base alla necessità. In questo modo viene ottimizzato l'uso dello spazio di archiviazione disponibile. Tuttavia è necessario monitorare attentamente lo spazio su disco disponibile perché questa opzione consente di allocare lo spazio di archiviazione in eccesso.
    
   - **Fissa**
        
     Con il provisioning fisso, la capacità di archiviazione viene allocata immediatamente, durante la creazione del disco virtuale. Il provisioning fisso, quindi, usa uno spazio del pool di archiviazione pari alle dimensioni del disco virtuale.
    
     >[!TIP]
     >Con gli spazi di archiviazione è possibile creare dischi virtuali con thin provisioning e provisioning fisso nello stesso pool di archiviazione. Ad esempio, si può usare un disco virtuale con thin provisioning per ospitare un database e un disco virtuale con provisioning fisso per i file di log associati.

9. Nella pagina **Specificare le dimensioni del disco virtuale**, eseguire le operazioni seguenti:
    
    Se è stato selezionato il thin provisioning nel passaggio precedente, nella casella **dimensione disco virtuale** immettere le dimensioni di un disco virtuale, selezionare le unità **(MB**, **GB**o **TB**), quindi fare clic su **Avanti**.
    
    Se è stato selezionato il provisioning fisso nel passaggio precedente, selezionare una delle opzioni seguenti:
    
      - **Specifica dimensioni**
        
        Per specificare le dimensioni, immettere un valore nella casella **dimensione disco virtuale** , quindi selezionare le unità (**MB**, **GB**o **TB**).
        
        Se non si usa un layout di archiviazione di tipo semplice, il disco virtuale richiede più spazio libero rispetto alle dimensioni specificate. Per evitare possibili errori nel caso in cui le dimensioni del volume superino lo spazio disponibile nel pool di archiviazione, è possibile selezionare la casella di controllo **Creare un disco virtuale con le dimensioni massime possibili, fino alle dimensioni specificate**.
    
      - **Dimensioni massime**
        
        Selezionare questa opzione per creare un disco virtuale che usi la capacità massima del pool di archiviazione.

10. Nella pagina **Conferma selezioni** verificare che le impostazioni siano corrette e quindi selezionare **Crea**.

11. Nella pagina **Visualizza risultati** verificare che tutte le attività siano state completate e quindi selezionare **Chiudi**.
    
    >[!TIP]
    >Per impostazione predefinita, la casella di controllo **Crea un volume al termine della procedura guidata** è selezionata e consente di procedere direttamente al passaggio successivo.

### <a name="windows-powershell-equivalent-commands-for-creating-virtual-disks"></a>Comandi equivalenti di Windows PowerShell per la creazione di dischi virtuali

Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.

Nell'esempio seguente viene creato un disco virtuale da 50 GB denominato *VirtualDisk1* in un pool di archiviazione denominato *StoragePool1*.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

Nell'esempio seguente viene creato un disco virtuale con mirroring denominato *VirtualDisk1* in un pool di archiviazione denominato *StoragePool1*. Il disco usa la capacità di archiviazione massima del pool di archiviazione.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –ResiliencySettingName Mirror –UseMaximumSize
```

Nell'esempio seguente viene creato un disco virtuale da 50 GB denominato *VirtualDisk1* in un pool di archiviazione denominato *StoragePool1*. Il disco usa il tipo di provisioning thin.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB) –ProvisioningType Thin
```

Nell'esempio seguente viene creato un disco virtuale denominato *VirtualDisk1* in un pool di archiviazione denominato *StoragePool1*. Il disco virtuale usa il mirroring a tre vie e ha una dimensione fissa di 20 GB.

>[!NOTE]
>Per il funzionamento di questo cmdlet sono necessari almeno cinque dischi fisici nel pool di archiviazione. In questo numero non sono inclusi i dischi allocati come riserve a caldo.

```PowerShell
New-VirtualDisk -StoragePoolFriendlyName StoragePool1 -FriendlyName VirtualDisk1 -ResiliencySettingName Mirror -NumberOfDataCopies 3 -Size 20GB -ProvisioningType Fixed
```

## <a name="step-3-create-a-volume"></a>Passaggio 3: Creare un volume

In questo passaggio è necessario creare un volume dal disco virtuale. È possibile assegnare una lettera di unità o una cartella facoltativa, quindi formattare il volume con un file system.

1. Se la creazione guidata nuovo volume non è ancora aperta, nella pagina **pool di archiviazione** in Server Manager, in **dischi virtuali**, fare clic con il pulsante destro del mouse sul disco virtuale desiderato, quindi scegliere **nuovo volume**.
    
    Si apre la Creazione guidata nuovo volume.

2. Nella pagina **prima di iniziare** selezionare **Avanti**.

3. Nella pagina **selezionare il server e il disco** , eseguire le operazioni seguenti e quindi fare clic su **Avanti**.
    
    1. Nell'area **Server** selezionare il server in cui si desidera eseguire il provisioning del volume.
    
    2. Nell'area **disco** selezionare il disco virtuale in cui si desidera creare il volume.

4. Nella pagina **specificare le dimensioni del volume** immettere le dimensioni di un volume, specificare le unità (**MB**, **GB**o **TB**), quindi fare clic su **Avanti**.

5. Nella pagina **assegna a una lettera di unità o cartella** , configurare l'opzione desiderata, quindi fare clic su **Avanti**.

6. Nella pagina **Seleziona impostazioni file System** eseguire le operazioni seguenti e quindi fare clic su **Avanti**.
    
    1. Nell'elenco **file System** selezionare **NTFS** o **refs**.
    
    2. Nell'elenco **Dimensioni unità di allocazione**, lasciare l'impostazione su **Impostazione predefinita** o impostare le dimensioni dell'unità di allocazione.
        
        >[!NOTE]
        >Per altre informazioni sulle dimensioni dell'unità di allocazione, vedere [Dimensioni di cluster predefinite per NTFS, FAT ed exFAT](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat).

    
    3. Facoltativamente, nella casella **Etichetta volume**, immettere un'etichetta di volume, ad esempio **Dati RU**.

7. Nella pagina **Conferma selezioni** verificare che le impostazioni siano corrette e quindi selezionare **Crea**.

8. Nella pagina **Visualizza risultati** verificare che tutte le attività siano state completate e quindi selezionare **Chiudi**.

9. Per verificare che il volume sia stato creato, in Server Manager selezionare la pagina **volumi** . Il volume viene elencato sotto il server in cui è stato creato. È anche possibile controllare che il volume si trovi in Esplora risorse.

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
- [Domande frequenti su spazi di archiviazione](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)
