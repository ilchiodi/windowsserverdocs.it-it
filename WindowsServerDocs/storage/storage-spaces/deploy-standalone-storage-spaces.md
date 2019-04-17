---
title: Distribuire spazi di archiviazione in un server autonomo
description: Descrive come distribuire spazi di archiviazione in un server autonomo basato su Windows Server 2012.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-spaces
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 40265e767ac9aca05386c0893def259aca3a5633
ms.sourcegitcommit: e73fbe1046a8bd2bf4f24ccffc11465ad8dfab1d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/07/2019
ms.locfileid: "8992534"
---
# Distribuire spazi di archiviazione in un server autonomo

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento descrive come distribuire spazi di archiviazione in un server autonomo. Per informazioni su come creare uno spazio di archiviazione del cluster, vedere [distribuire un cluster di spazi di archiviazione in Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>).

Per creare uno spazio di archiviazione, devi prima creare uno o più pool di archiviazione. Un pool di archiviazione è una raccolta di dischi fisici. Un pool di archiviazione consente di aggregazione di archiviazione, espansione elastico capacità e l'amministrazione delegata.

Da un pool di archiviazione, è possibile creare uno o più dischi virtuali. I dischi virtuali sono anche noti come *spazi di archiviazione*. Come un normale disco da cui è possibile creare formattata volumi, viene visualizzato uno spazio di archiviazione del sistema operativo Windows. Quando crei un disco virtuale tramite l'interfaccia utente di servizi File e archiviazione, è possibile configurare il tipo di resilienza (semplici, mirror o parità), il tipo di provisioning (thin o fisso) e le dimensioni. Tramite Windows PowerShell, è possibile impostare parametri aggiuntivi, ad esempio il numero di colonne, il valore interfoliazione e dischi quali fisici nel pool di usare. Per informazioni su questi parametri aggiuntivi, vedi [New-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/new-virtualdisk?view=win10-ps) e [quali sono le colonne e come spazi di archiviazione decidere quante usare?](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx%23what_are_columns_and_how_does_storage_spaces_decide_how_many_to_use) nell'archiviazione spazi domande frequenti (FAQ).

>[!NOTE]
>Non puoi usare uno spazio di archiviazione per ospitare il sistema operativo Windows.

Da un disco virtuale, è possibile creare uno o più volumi. Quando si crea un volume, è possibile configurare le dimensioni, lettera di unità o cartelle, file system (file system NTFS o Resilient File System (ReFS)), la dimensione dell'unità di allocazione e un'etichetta di volume facoltativo.

La figura seguente mostra il flusso di lavoro di spazi di archiviazione.

![Flusso di lavoro spazi di archiviazione](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**Figura 1: Spazi di archiviazione flusso di lavoro**

>[!NOTE]
>Questo argomento include i cmdlet di Windows PowerShell di esempio che puoi usare per automatizzare alcune delle procedure descritte. Per altre informazioni, vedi [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6).

## Prerequisiti

Per usare spazi di archiviazione in un server autonomo di 2012−based Windows Server, assicurati che i dischi fisici che si desiderano utilizzare soddisfano i prerequisiti seguenti.

> [!IMPORTANT]
> Se vuoi informazioni su come distribuire spazi di archiviazione in un cluster di failover, vedere [distribuire un cluster di spazi di archiviazione in Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>). Distribuzione di un cluster di failover prevede alcuni prerequisiti diversi, ad esempio i tipi di bus dischi supportati, tipi di resilienza supportati e il numero minimo richiesto di dischi.

|Area|Requisito|Note|
|---|---|---|
|Tipi di bus disco|-Serial Attached SCSI (SAS)<br>-Serie Advanced Technology Attachment (SATA)<br>-iSCSI e Fibre Channel controller. |È inoltre possibile utilizzare le unità USB. Tuttavia, non è ottimale per utilizzare le unità USB in un ambiente server.<br>Spazi di archiviazione è supportata in iSCSI e controller FC (Fibre Channel), purché i dischi virtuali creati sopra li sono non resiliente (semplice con qualsiasi numero di colonne).<br>|
|Configurazione del disco|-I dischi fisici devono essere almeno 4 GB<br>-Dischi devono essere vuoto e non formattato. Non creare volumi.||
|Considerazioni sulla HBA|-Schede bus semplice host (HBA) che non supportano la funzionalità RAID sono consigliate<br>-Se che supportano RAID, HBA devono essere in modalità non RAID con tutte le funzionalità di RAID disabilitata<br>-Schede non devono astrarre i dischi fisici, i dati nella cache, o nascondere le periferiche collegate. Sono inclusi i servizi enclosure fornite dai dispositivi (JBOD) just-un-gruppo-di-dischi collegati. |Spazi di archiviazione è compatibile solo con HBA in cui è possibile disabilitare completamente tutte le funzionalità di RAID.|
|Enclosure JBOD|-Le enclosure JBOD sono facoltative<br>-Consigliato usare spazi di archiviazione certificati enclosure elencato nel catalogo di Windows Server<br>-Se stai usando una enclosure JBOD, verifica con il tuo fornitore di archiviazione che le enclosure supporta spazi di archiviazione per garantire la funzionalità completa<br>-Per determinare se le enclosure JBOD supporta l'identificazione della enclosure e slot, eseguire il cmdlet Windows PowerShell seguente:<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>Se i campi **EnclosureNumber** e **SlotNumber** contengono i valori, le enclosure supporta queste funzionalità.||

Per pianificare il numero di dischi fisici e il tipo di resilienza desiderata per la distribuzione di un server autonomo, Usa le linee guida seguenti.

|Tipo di resilienza|Requisiti di disco|Quando usare|
|---|---|---|
|**Semplicità**<br><br>-Strisce dati su dischi fisici<br>-Ottimizza la capacità del disco e aumenta la velocità effettiva<br>-Nessun resilienza (non protegge da un errore del disco)<br><br><br><br><br><br><br>|Richiede almeno un disco fisico.|Non usare per ospitare i dati insostituibili. Spazi semplice non protegge da un errore del disco.<br><br>Usa per ospitare i dati temporanei o facilmente ricreata a costi ridotti.<br><br>Adatta ai carichi di lavoro ad alte prestazioni in cui la resilienza non è necessario o viene già fornita dall'applicazione.|
|**Mirror**<br><br>-Archivia due o tre copie dei dati tra il set di dischi fisici<br>-Aumenta l'affidabilità, ma riduce la capacità. Duplicazione si verifica con ogni operazione di scrittura. Uno spazio mirror strisce anche i dati su più unità fisiche.<br>-Maggiore velocità effettiva dei dati e latenza di accesso inferiori rispetto a parità<br>-Usa dirty area geografica (DRT) per tenere traccia delle modifiche per i dischi nel pool di monitoraggio. Quando il sistema riprende da un arresto non pianificato e gli spazi vengono portati online indietro, DRT rende i dischi nel pool di coerenti tra loro.|Richiede almeno due dischi fisici per proteggere da un errore del disco singolo.<br><br>Richiede almeno cinque dischi fisici per proteggere da due errori disco simultanei.|Utilizzare per la maggior parte delle distribuzioni. Ad esempio, mirroring spazi servono per una condivisione file generico o una libreria di disco rigido virtuale (VHD).|
|**Parità**<br><br>-Informazioni di dati e la parità strisce su dischi fisici<br>-Aumenta affidabilità quando viene confrontato con uno spazio semplice, ma piuttosto riduce capacità<br>-Aumenta la resilienza tramite registrazione frame. Questo consente di evitare il danneggiamento dei dati se si verifica un arresto non pianificato.|Richiede almeno tre dischi fisici per proteggere da un errore del disco singolo.|Utilizzare per i carichi di lavoro sono altamente sequenziali, ad esempio archivio o backup.|

## Passaggio 1: Creare un pool di archiviazione

È necessario dischi fisici primi gruppo disponibili in uno o più pool di archiviazione.

1. Nel riquadro di spostamento di Server Manager, seleziona **servizi File e archiviazione**.

2. Nel riquadro di spostamento, selezionare la pagina **Di pool di archiviazione** .
    
    Per impostazione predefinita, i dischi disponibili sono inclusi in un pool di è denominato *primordial* pool. Se nessun pool primordial è elencato nel **Pool di archiviazione**, indica che lo spazio di archiviazione non soddisfa i requisiti per spazi di archiviazione. Assicurati che i dischi soddisfano i requisiti sono descritte nella sezione dei prerequisiti.
    
    >[!TIP]
    >Se si seleziona pool di archiviazione **Primordial** , i dischi fisici disponibili sono elencati in **Dischi fisici**.

3. In **Pool di archiviazione**, seleziona l'elenco delle **attività** e quindi seleziona **Nuovo Pool di archiviazione**. Verrà aperta la creazione guidata nuovo Pool di archiviazione.

4. Nella pagina **prima di iniziare** , seleziona **successivo**.

5. Nella pagina **specificare un nome di pool di archiviazione e sottosistema** , Immetti un nome e una descrizione facoltativa per il pool di archiviazione, seleziona il gruppo di dischi fisici disponibili che vuoi usare e quindi seleziona **successivo**.

6. Nella pagina **Seleziona dischi fisici per il pool di archiviazione** , eseguire le seguenti e quindi seleziona **quindi**:
    
    1. Seleziona la casella di controllo accanto a ogni disco fisico che si desidera includere nel pool di archiviazione.
    
    2. Se si desidera designare una o più dischi come a caldo, sotto l' **allocazione**, seleziona la freccia a discesa, quindi seleziona **Riserva ad accesso frequente**.

7. Nella pagina **Conferma selezioni** , verifica che le impostazioni siano corrette e quindi selezionare **Crea**.

8. Nella pagina **dei risultati di visualizzazione** , verifica che tutte le attività di completamento e quindi seleziona **Chiudi**.
    
    >[!NOTE]
    >Facoltativamente, per continuare direttamente al passaggio successivo, puoi selezionare la casella di controllo **Crea un disco virtuale chiusura della procedura guidata** .

9. In **Pool di archiviazione**, verificare che il nuovo pool di archiviazione è elencato.

### Comandi equivalenti di Windows PowerShell per la creazione di pool di archiviazione

Il seguente cmdlet di Windows PowerShell o i cmdlet di eseguono la stessa funzione della procedura precedente. Immetti ogni cmdlet su una singola riga, anche se a causa di vincoli di formattazione possano essere visualizzati ritorno a capo automatico tra più righe qui.

L'esempio seguente mostra quali dischi fisici sono disponibili nel pool di primordial.

```PowerShell
Get-StoragePool -IsPrimordial $true | Get-PhysicalDisk | Where-Object CanPool -eq $True
```

L'esempio seguente crea un nuovo pool di archiviazione denominato *StoragePool1* che Usa tutti i dischi disponibili.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk –CanPool $True)
```

L'esempio seguente crea un nuovo pool di archiviazione, *StoragePool1*, che usa quattro dei dischi disponibili.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk PhysicalDisk1, PhysicalDisk2, PhysicalDisk3, PhysicalDisk4)
```

La sequenza di cmdlet di esempio seguente mostra come aggiungere un disco fisico disponibile *PhysicalDisk5* come una riserva al pool di archiviazione *StoragePool1*.

```PowerShell
$PDToAdd = Get-PhysicalDisk –FriendlyName PhysicalDisk5
Add-PhysicalDisk –StoragePoolFriendlyName StoragePool1 –PhysicalDisks $PDToAdd –Usage HotSpare
```

## Passaggio 2: Creare un disco virtuale

Successivamente, devi creare uno o più dischi virtuali dal pool di archiviazione. Quando crei un disco virtuale, è possibile selezionare come i dati sono disposte tra i dischi fisici. Ciò ha prestazioni e affidabilità. Puoi anche selezionare se si desidera creare dischi con thin o fissa-provisioning.

1. Se la creazione guidata disco virtuale non è già aperta, nella pagina **Di pool di archiviazione** in Server Manager in **Pool di archiviazione**, assicurati che sia selezionato il pool di archiviazione desiderato.

2. In **Dischi VIRTUALI**, seleziona l'elenco delle **attività** e quindi seleziona **Nuovo disco virtuale**. Verrà aperta la creazione guidata disco virtuale.

3. Nella pagina **prima di iniziare** , seleziona **successivo**.

4. Nella pagina **Seleziona pool di archiviazione** , seleziona il pool di archiviazione desiderato e quindi seleziona **successivo**.

5. Nella pagina **specificare il nome del disco virtuale** , Immetti un nome e una descrizione facoltativa e quindi seleziona **successivo**.

6. Nella pagina **Seleziona il layout di archiviazione** , seleziona il layout desiderato e quindi seleziona **successivo**.
    
    >[!NOTE]
    >Se selezioni un layout in cui non è sufficiente dischi fisici, riceverai un messaggio di errore quando si seleziona **successivo**. Per informazioni su quali layout da utilizzare e i requisiti del disco, vedere la sezione [Prerequisiti](#prerequisites)).

7. Se hai selezionato **mirroring** come il layout di archiviazione e hai cinque o più dischi nel pool di, verrà visualizzata la pagina **Configura le impostazioni di resilienza** . Seleziona una delle opzioni seguenti:
    
      - **Mirroring a 2 vie**
      - **Mirroring a 3 vie**

8. Nella pagina, **Specifica il tipo di provisioning** seleziona una delle opzioni seguenti e quindi seleziona **successivo**.
    
      - **Thin**
        
        Con thin provisioning, è allocato in base alle esigenze. Ciò consente di ottimizzare l'utilizzo di risorse di archiviazione disponibile. Tuttavia, poiché ciò consente di eccessiva allocazione dell'archiviazione, è necessario con attenzione monitorare quanto spazio su disco è disponibile.
    
      - **Fissa**
        
        Con provisioning fissa, viene allocata immediatamente, la capacità di archiviazione durante la creazione di un disco virtuale. Pertanto, corretti provisioning lo spazio viene utilizzato dal pool di archiviazione che è uguale alla dimensione del disco virtuale.
    
    >[!TIP]
    >Con spazi di archiviazione, è possibile creare entrambi i dischi virtuali con thin e fissa-provisioning dello stesso pool di archiviazione. Ad esempio, è possibile utilizzare un disco virtuale con thin provisioning per ospitare un database e un disco virtuale con provisioning fissa per ospitare i file di log associato.

9. Nella pagina **specificare le dimensioni del disco virtuale** , eseguire le operazioni seguenti:
    
    Se hai selezionato thin provisioning nel passaggio precedente, nella casella di **dimensioni disco virtuale** , Immetti una dimensione del disco virtuale, selezionare le unità (**MB**, **GB**o **TB**), quindi selezionare **Avanti**.
    
    Se hai selezionato il provisioning fissa nel passaggio precedente, seleziona una delle operazioni seguenti:
    
      - **Specificare le dimensioni**
        
        Per specificare una dimensione, Immetti un valore nella casella di **dimensioni disco virtuale** e quindi seleziona l'unità (**MB**, **GB**o **TB**).
        
        Se usi un layout di archiviazione diverso da semplice, il disco virtuale utilizza più spazio rispetto alle dimensioni che specifichi. Per evitare un errore di potenziale in cui le dimensioni del volume superano lo spazio libero pool di archiviazione, puoi selezionare la casella di controllo di **creare il disco virtuale più grande possibile, fino a dimensioni specificate** .
    
      - **Dimensioni massime**
        
        Seleziona questa opzione per creare un disco virtuale che usa la capacità massima del pool di archiviazione.

10. Nella pagina **Conferma selezioni** , verifica che le impostazioni siano corrette e quindi selezionare **Crea**.

11. Nella pagina **dei risultati di visualizzazione** , verifica che tutte le attività di completamento e quindi seleziona **Chiudi**.
    
    >[!TIP]
    >Per impostazione predefinita, viene selezionata la casella di controllo **Crea un volume di chiusura della procedura guidata** . Verrà visualizzata direttamente al passaggio successivo.

### Comandi equivalenti di Windows PowerShell per la creazione di dischi virtuali

Il seguente cmdlet di Windows PowerShell o i cmdlet di eseguono la stessa funzione della procedura precedente. Immetti ogni cmdlet su una singola riga, anche se a causa di vincoli di formattazione possano essere visualizzati ritorno a capo automatico tra più righe qui.

L'esempio seguente crea un disco virtuale a 50 GB denominato *VirtualDisk1* in un pool di archiviazione denominato *StoragePool1*.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

L'esempio seguente crea un disco virtuale con mirroring denominato *VirtualDisk1* in un pool di archiviazione denominato *StoragePool1*. Il disco Usa capacità di archiviazione massima del pool di archiviazione.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –ResiliencySettingName Mirror –UseMaximumSize
```

L'esempio seguente crea un disco virtuale a 50 GB denominato *VirtualDisk1* in un pool di archiviazione è denominato *StoragePool1*. Il disco Usa il tipo thin provisioning.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB) –ProvisioningType Thin
```

L'esempio seguente crea un disco virtuale denominato *VirtualDisk1* in un pool di archiviazione denominato *StoragePool1*. Il disco virtuale utilizza vie ed è una dimensione fissa di 20 GB.

>[!NOTE]
>È necessario disporre almeno cinque dischi fisici nel pool di archiviazione di questo cmdlet funzionare. (Questo non include tutti i dischi che vengono allocati come a caldo.)

```PowerShell
New-VirtualDisk -StoragePoolFriendlyName StoragePool1 -FriendlyName VirtualDisk1 -ResiliencySettingName Mirror -NumberOfDataCopies 3 -Size 20GB -ProvisioningType Fixed
```

## Passaggio 3: Creare un volume

Successivamente, devi creare un volume dal disco virtuale. È possibile assegnare una lettera di unità facoltativa o una cartella, quindi formattare il volume con un file system.

1. Se la creazione guidata nuovo Volume non è già aperta, nella pagina **Di pool di archiviazione** in Server Manager, nell'area i **Dischi VIRTUALI**destro del mouse sul disco virtuale desiderato e quindi seleziona **Nuovo Volume**.
    
    Si apre la creazione guidata nuovo Volume.

2. Nella pagina **prima di iniziare** , seleziona **successivo**.

3. Nella pagina **Seleziona il server e un disco** , fare le seguenti e quindi seleziona **successivamente**.
    
    1. Nell'area del **Server** , selezionare il server in cui vuoi effettuare il provisioning del volume.
    
    2. Nell'area del **disco** , seleziona il disco virtuale in cui si desidera creare il volume.

4. Nella pagina **specificare le dimensioni del volume** , Immetti una dimensione di volume, specificare le unità (**MB**, **GB**o **TB**) e quindi seleziona **successivo**.

5. Nella pagina **assegnare a una lettera di unità o una cartella** , l'opzione che desideri configurare e quindi seleziona **successivo**.

6. Nella pagina **Seleziona impostazioni del file system** , eseguire le seguenti e quindi seleziona **quindi**.
    
    1. Nell'elenco **File system** , selezionare **NTFS** o **ReFS**.
    
    2. Nell'elenco **dimensione unità di allocazione** , è possibile lasciare l'impostazione **predefinita** oppure impostare la dimensione dell'unità di allocazione.
        
        >[!NOTE]
        >Per ulteriori informazioni sulla dimensione dell'unità di allocazione, vedere [predefiniti dimensione del cluster per NTFS, FAT ed exFAT](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat).

    
    3. Facoltativamente, nella casella di **etichetta di Volume** , Immetti un nome di etichetta di volume, ad esempio **Dati RU**.

7. Nella pagina **Conferma selezioni** , verifica che le impostazioni siano corrette e quindi selezionare **Crea**.

8. Nella pagina **dei risultati di visualizzazione** , verifica che tutte le attività di completamento e quindi seleziona **Chiudi**.

9. Per verificare che il volume è stato creato, in Server Manager, selezionare la pagina di **volumi** . Il volume è elencato nel server in cui è stato creato. È inoltre possibile verificare che il volume è in Esplora risorse.

### Comandi equivalenti di Windows PowerShell per la creazione di volumi

Il cmdlet Windows PowerShell seguente esegue la stessa funzione procedura precedente. Immetti il comando su una singola riga.

L'esempio seguente inizializza i dischi per il disco virtuale *VirtualDisk1*, crea una partizione con una lettera di unità e quindi formatta il volume con il file system NTFS predefinito.

```PowerShell
Get-VirtualDisk –FriendlyName VirtualDisk1 | Get-Disk | Initialize-Disk –Passthru | New-Partition –AssignDriveLetter –UseMaximumSize | Format-Volume
```

## Altre informazioni

- [Spazi di archiviazione](overview.md)
- [Cmdlet di archiviazione in Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/index?view=win10-ps)
- [Distribuire spazi di archiviazione del cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11))
- [Spazi di archiviazione frequenti domande frequenti](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)
