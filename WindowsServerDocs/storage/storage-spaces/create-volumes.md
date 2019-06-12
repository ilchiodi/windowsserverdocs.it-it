---
title: Creazione di volumi in Spazi di archiviazione diretta
description: Come creare i volumi in spazi di archiviazione diretta tramite Windows Admin Center e PowerShell.
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: cosmosdarwin
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.date: 06/06/2019
ms.openlocfilehash: 85eca06a5d8c103851596055099876cb53a902ad
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810560"
---
# <a name="creating-volumes-in-storage-spaces-direct"></a>Creazione di volumi in Spazi di archiviazione diretta

> Si applica a: Windows Server 2019, Windows Server 2016

In questo argomento viene descritto come creare i volumi in un cluster di spazi di archiviazione diretta tramite Windows Admin Center, PowerShell o gestione Cluster di Failover.

> [!TIP]
> Se non lo hai già fatto, consulta prima [Pianificazione dei volumi in Spazi di archiviazione diretta](plan-volumes.md).

## <a name="create-a-three-way-mirror-volume"></a>Creare un volume con mirroring a tre vie

Per creare un volume con mirroring a tre vie in Windows Admin Center: 

1. In Windows Admin Center, connettersi a un cluster di spazi di archiviazione diretta e quindi selezionare **volumi** dalle **Tools** riquadro.
2. Nella pagina di volumi, selezionare la **inventario** scheda e quindi selezionare **Crea volume**.
3. Nel **Crea volume** riquadro, immettere un nome per il volume e lasciare **resilienza** come **vie**.
4. Nelle **dimensioni in unità disco rigido**, specificare le dimensioni del volume. Ad esempio, 5 TB (terabyte).
5. Selezionare **Create**.

A seconda delle dimensioni, la creazione del volume può richiedere alcuni minuti. Le notifiche in alto a destra consente di sapere quando viene creato il volume. Il nuovo volume viene visualizzata nell'elenco dell'inventario.

Guardare un video rapido su come creare un volume con mirroring a tre vie.

> [!VIDEO https://www.youtube-nocookie.com/embed/o66etKq70N8]

## <a name="create-a-mirror-accelerated-parity-volume"></a>Creare un volume con parità con accelerazione mirror

Parità con accelerazione mirror riduce il footprint del volume del disco rigido. Ad esempio, un volume con mirroring a tre vie significa che per ogni 10 TB di dimensione, sarà necessario 30 terabyte come footprint. Per ridurre il sovraccarico dovuto alla superficie, creare un volume con parità con accelerazione mirror. Questo riduce il footprint da 30 terabyte fino a terabyte appena 22, anche con solo 4 server, il mirroring il 20% più attivi di dati e con parità, ovvero lo spazio più efficiente, per archiviare il resto. È possibile regolare questo rapporto di parità e mirroring per verificare le prestazioni rispetto a compromesso capacità ottimale per il carico di lavoro. Mirror di parità e del 10% al 90%, ad esempio, produce prestazioni minori ma semplifica ulteriormente l'impatto.

Per creare un volume con parità con accelerazione mirror in Windows Admin Center:

1. In Windows Admin Center, connettersi a un cluster di spazi di archiviazione diretta e quindi selezionare **volumi** dalle **Tools** riquadro.
2. Nella pagina di volumi, selezionare la **inventario** scheda e quindi selezionare **Crea volume**.
3. Nel **Crea volume** riquadro, immettere un nome per il volume.
4. Nelle **resilienza**, selezionare **parità con accelerazione Mirror**.
5. Nelle **percentuale di parità**, selezionare la percentuale di parità.
6. Selezionare **Create**.

Guardare un video rapido su come creare un volume con accelerazione mirror parità.

> [!VIDEO https://www.youtube-nocookie.com/embed/R72QHudqWpE]

## <a name="open-volume-and-add-files"></a>Aprire il volume e aggiungere i file

Per aprire un volume e aggiungere i file nel volume in Windows Admin Center:

1. In Windows Admin Center, connettersi a un cluster di spazi di archiviazione diretta e quindi selezionare **volumi** dalle **Tools** riquadro.
2. Nella pagina di volumi, selezionare la **inventario** scheda.
2. Nell'elenco di volumi, selezionare il nome del volume che si desidera aprire.

    Nella pagina dei dettagli di volume, è possibile visualizzare il percorso per il volume.

4. Nella parte superiore della pagina, selezionare **aperto**. Verrà avviato lo strumento di file in Windows Admin Center.
5. Passare al percorso del volume. Qui è possibile cercare i file nel volume.
6. Selezionare **caricare**e quindi selezionare un file da caricare.
7. Utilizzare il visualizzatore **nuovamente** per tornare al riquadro degli strumenti di Windows Admin Center.

Guardare un video rapido su come aprire un volume e aggiungere i file.

> [!VIDEO https://www.youtube-nocookie.com/embed/j59z7ulohs4]

## <a name="turn-on-deduplication-and-compression"></a>Attivare la deduplicazione e compressione

Deduplicazione e compressione viene gestito per ogni volume. Deduplicazione e compressione Usa un modello di post-elaborazione, il che significa che non siano visibili risparmi fino a quando non è in esecuzione. Quando questo accade, corretto su tutti i file, anche quelli che sono state effettuate dalla prima.

1. In Windows Admin Center, connettersi a un cluster di spazi di archiviazione diretta e quindi selezionare **volumi** dalle **Tools** riquadro.
2. Nella pagina di volumi, selezionare la **inventario** scheda.
3. Nell'elenco di volumi, selezionare il nome del volume che si desidera gestire.
4. Nella pagina dei dettagli di volume, selezionare l'opzione etichettato **deduplicazione e compressione**.
5. Nel riquadro abilita la deduplicazione, selezionare la modalità di deduplicazione.

    Anziché le impostazioni complicate, Windows Admin Center consente di scegliere tra i profili pronte all'uso per carichi di lavoro diversi. Se non si è certi, usare l'impostazione predefinita.

6. Seleziona **Abilita**.

Guardare un video rapido su come attivare la deduplicazione e compressione.

> [!VIDEO https://www.youtube-nocookie.com/embed/PRibTacyKko]

## <a name="create-volumes-using-powershell"></a>Creare volumi con PowerShell

Ti consigliamo di usare il cmdlet **New-Volume** per creare i volumi per Spazi di archiviazione diretta. Offre l'esperienza più semplice e rapida. Questo cmdlet singolo crea automaticamente il disco virtuale, lo divide in partizioni e lo formatta, crea il volume con il nome corrispondente e lo aggiunge ai volumi condivisi del cluster, tutto in un unico, semplice passaggio.

Il cmdlet **New-Volume** prevede quattro parametri che dovrai sempre fornire:

- **FriendlyName:** Qualsiasi stringa desiderata, ad esempio *"Volume1"*
- **FileSystem:** Entrambi **CSVFS_ReFS** (scelta consigliata) o **CSVFS_NTFS**
- **StoragePoolFriendlyName:** Il nome dello spazio di archiviazione del pool, ad esempio *"S2D in ClusterName"*
- **Dimensioni:** Le dimensioni del volume, ad esempio *"10 TB"*

   > [!NOTE]
   > Windows, incluso PowerShell, conta con numeri binari (in base 2), mentre le unità sono spesso etichettate con numeri decimali (in base 10). Questo spiega perché un'unità da "un terabyte", definita come 1.000.000.000.000 byte, viene visualizzata in Windows come circa "909 GB". Si tratta di un comportamento previsto. Durante la creazione di volumi tramite **New-Volume**, devi specificare il parametro **Size** in numeri binari (in base 2). Ad esempio, se specifichi "909GB" o "0,909495TB", verrà creato un volume di circa 1.000.000.000.000 byte.

### <a name="example-with-2-or-3-servers"></a>Esempio: Con server 2 o 3

Per semplificare le cose, se la distribuzione include solo due server, Spazi di archiviazione diretta userà automaticamente il mirroring a 2 vie per la resilienza. Se la distribuzione include solo tre server, userà automaticamente il mirroring a 3 vie.

```PowerShell
New-Volume -FriendlyName "Volume1" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB
```

### <a name="example-with-4-servers"></a>Esempio: Con 4 + server

Se disponi di quattro o più server, puoi utilizzare il parametro facoltativo **ResiliencySettingName** per scegliere il tipo di resilienza.

-   **ResiliencySettingName:** Entrambi **Mirror** oppure **parità**.

Nell'esempio seguente, *"Volume2"* usa il mirroring a 3 vie e *"Volume3"* usa la doppia parità (spesso detta "codifica di cancellazione").

```PowerShell
New-Volume -FriendlyName "Volume2" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Mirror
New-Volume -FriendlyName "Volume3" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Parity
```

### <a name="example-using-storage-tiers"></a>Esempio: Utilizzo dei livelli di archiviazione

Nelle distribuzioni con tre tipi di unità, un volume può comprendere i livelli SSD e HDD per risiedere parzialmente su ciascuno. Analogamente, nelle distribuzioni con quattro o più server, un volume può combinare mirroring e doppia parità per risiedere parzialmente su ciascuno.

Per facilitare la creazione di tali volumi, Spazi di archiviazione diretta fornisce modelli di livello predefiniti denominati *Performance* e *Capacity*. Questi modelli incapsulano le definizioni per il mirroring a 3 vie nelle unità di capacità più veloci (ove applicabile) e la doppia parità nelle unità di capacità più lente (ove applicabile).

Puoi vederli eseguendo il cmdlet **Get-StorageTier**.

```PowerShell
Get-StorageTier | Select FriendlyName, ResiliencySettingName, PhysicalDiskRedundancy
```

![Screenshot di PowerShell con livelli di archiviazione](media/creating-volumes/storage-tiers-screenshot.png)

Per creare volumi a livelli, fai riferimento a questi modelli di livello usando i parametri **StorageTierFriendlyNames** e **StorageTierSizes** del cmdlet **New-Volume**. Ad esempio, il cmdlet seguente crea un solo volume che combina il mirroring a 3 vie e la doppia parità in proporzioni di 30:70.

```PowerShell
New-Volume -FriendlyName "Volume4" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 300GB, 700GB
```

## <a name="create-volumes-using-failover-cluster-manager"></a>Creare volumi tramite Gestione cluster di failover

Puoi inoltre creare volumi tramite la *Creazione guidata nuovo disco virtuale (Spazi di archiviazione diretta)* seguita dalla *Creazione guidata Nuovo volume* da Gestione cluster di failover, anche se il flusso di lavoro include molti più passaggi manuali e non è consigliato.

Ci sono tre passaggi principali:

### <a name="step-1-create-virtual-disk"></a>Passaggio 1: Creare un disco virtuale

![Nuovo disco virtuale](media/creating-volumes/GUI-Step-1.png)

1. In Gestione cluster di failover, passa a **Archiviazione** -> **Pool**.
2. Seleziona **Nuovo disco virtuale** dal riquadro Azioni a destra o fai clic con il pulsante destro del mouse sul pool e seleziona **Nuovo disco virtuale**.
3. Seleziona il pool di archiviazione e fai clic su **OK**. La *Creazione guidata nuovo disco virtuale (Spazi di archiviazione diretta)* verrà aperta.
4. Utilizza la procedura guidata per denominare il disco virtuale e specificarne le dimensioni.
5. Rivedi le selezioni e fai clic su **Crea**.
6. Assicurati di selezionare la casella contrassegnata **Crea un volume al termine della procedura guidata** prima della chiusura.

### <a name="step-2-create-volume"></a>Passaggio 2: Creare volume

La *Creazione guidata Nuovo volume* verrà aperta.

7. Seleziona il disco virtuale appena creato e fai clic su **Avanti**.
8. Specifica le dimensioni del volume (impostazione predefinita: le stesse dimensioni del disco virtuale) e fai clic su **Avanti**. 
9. Assegna il volume a una lettera di unità o scegli **Non assegnare a una lettera di unità** e fai clic su **Avanti**.
10. Specifica il file system da utilizzare, lascia la dimensione dell'unità di allocazione come *Predefinita*, assegna un nome al volume e fai clic su **Avanti**.
11. Rivedi le selezioni e fai clic su **Crea**, quindi su **Chiudi**.

### <a name="step-3-add-to-cluster-shared-volumes"></a>Passaggio 3: Aggiungi a volumi condivisi del cluster

![Aggiungi a volumi condivisi cluster](media/creating-volumes/GUI-Step-2.png)

12. In Gestione cluster di failover, passa a **Archiviazione** -> **Dischi**.
13. Seleziona il disco virtuale appena creato e seleziona **Aggiungi a volumi condivisi cluster** dal riquadro Azioni a destra o fai clic con il pulsante destro del mouse sul disco virtuale e seleziona **Aggiungi a volumi condivisi cluster**.

Non c'è altro da fare. Ripeti il numero di volte necessario per creare altri volumi.

## <a name="see-also"></a>Vedere anche

- [Panoramica di spazi diretti di archiviazione](storage-spaces-direct-overview.md)
- [Pianificazione di volumi in spazi di archiviazione diretta](plan-volumes.md)
- [Estensione dei volumi in spazi di archiviazione diretta](resize-volumes.md)
- [Eliminazione dei volumi in spazi di archiviazione diretta](delete-volumes.md)
