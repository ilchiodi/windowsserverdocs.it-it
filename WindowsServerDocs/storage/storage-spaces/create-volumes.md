---
ms.assetid: a9f229eb-bef4-4231-97d0-0899e17cef32
title: Creazione di volumi in Spazi di archiviazione diretta
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 01/11/2017
ms.localizationpriority: medium
ms.openlocfilehash: 277a676d8e53a7847d54039aab6607be8e5a78c5
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "1833433"
---
# <a name="creating-volumes-in-storage-spaces-direct"></a>Creazione di volumi in Spazi di archiviazione diretta

>Si applica a: Windows Server 2016

In questo argomento viene descritto come creare volumi in Spazi di archiviazione diretta usando PowerShell o Gestione cluster di failover.

   >[!TIP]
   >  Se non lo hai già fatto, consulta prima [Pianificazione dei volumi in Spazi di archiviazione diretta](plan-volumes.md).

## <a name="create-volumes-using-powershell"></a>Creare volumi con PowerShell

Ti consigliamo di usare il cmdlet **New-Volume** per creare i volumi per Spazi di archiviazione diretta. Offre l'esperienza più semplice e rapida. Questo cmdlet singolo crea automaticamente il disco virtuale, lo divide in partizioni e lo formatta, crea il volume con il nome corrispondente e lo aggiunge ai volumi condivisi del cluster, tutto in un unico, semplice passaggio.

Il cmdlet **New-Volume** prevede quattro parametri che dovrai sempre fornire:

- **FriendlyName:** qualsiasi stringa, ad esempio *"Volume1"*
- **FileSystem:** **CSVFS_ReFS** (scelta consigliata) o **CSVFS_NTFS**
- **StoragePoolFriendlyName:** nome del pool di archiviazione, ad esempio *"S2D in NomeCluster"*
- **Size:** le dimensioni del volume, ad esempio *"10TB"*

   >[!NOTE]
   >  Windows, incluso PowerShell, conta con numeri binari (in base 2), mentre le unità sono spesso etichettate con numeri decimali (in base 10). Questo spiega perché un'unità da "un terabyte", definita come 1.000.000.000.000 byte, viene visualizzata in Windows come circa "909 GB". Si tratta di un comportamento previsto. Durante la creazione di volumi tramite **New-Volume**, devi specificare il parametro **Size** in numeri binari (in base 2). Ad esempio, se specifichi "909GB" o "0,909495TB", verrà creato un volume di circa 1.000.000.000.000 byte.

### <a name="example-with-2-or-3-servers"></a>Esempio: con 2 o 3 server

Per semplificare le cose, se la distribuzione include solo due server, Spazi di archiviazione diretta userà automaticamente il mirroring a 2 vie per la resilienza. Se la distribuzione include solo tre server, userà automaticamente il mirroring a 3 vie.

```
New-Volume -FriendlyName "Volume1" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB
```

### <a name="example-with-4-servers"></a>Esempio: con 4 server e più

Se disponi di quattro o più server, puoi utilizzare il parametro facoltativo **ResiliencySettingName** per scegliere il tipo di resilienza.

-   **ResiliencySettingName:** **Mirror** o **Parity**.

Nell'esempio seguente, *"Volume2"* usa il mirroring a 3 vie e *"Volume3"* usa la doppia parità (spesso detta "codifica di cancellazione").

```
New-Volume -FriendlyName "Volume2" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Mirror
New-Volume -FriendlyName "Volume3" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Parity
```

### <a name="example-using-storage-tiers"></a>Esempio: utilizzo di livelli di archiviazione

Nelle distribuzioni con tre tipi di unità, un volume può comprendere i livelli SSD e HDD per risiedere parzialmente su ciascuno. Analogamente, nelle distribuzioni con quattro o più server, un volume può combinare mirroring e doppia parità per risiedere parzialmente su ciascuno.

Per facilitare la creazione di tali volumi, Spazi di archiviazione diretta fornisce modelli di livello predefiniti denominati *Performance* e *Capacity*. Questi modelli incapsulano le definizioni per il mirroring a 3 vie nelle unità di capacità più veloci (ove applicabile) e la doppia parità nelle unità di capacità più lente (ove applicabile).

Puoi vederli eseguendo il cmdlet **Get-StorageTier**.

```
Get-StorageTier | Select FriendlyName, ResiliencySettingName, PhysicalDiskRedundancy
```

![Screenshot di PowerShell con livelli di archiviazione](media/creating-volumes/storage-tiers-screenshot.png)

Per creare volumi a livelli, fai riferimento a questi modelli di livello usando i parametri **StorageTierFriendlyNames** e **StorageTierSizes** del cmdlet **New-Volume**. Ad esempio, il cmdlet seguente crea un solo volume che combina il mirroring a 3 vie e la doppia parità in proporzioni di 30:70.

```
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

### <a name="step-2-create-volume"></a>Passaggio 2: Creare il volume

La *Creazione guidata Nuovo volume* verrà aperta.

7. Seleziona il disco virtuale appena creato e fai clic su **Avanti**.
8. Specifica le dimensioni del volume (impostazione predefinita: le stesse dimensioni del disco virtuale) e fai clic su **Avanti**. 
9. Assegna il volume a una lettera di unità o scegli **Non assegnare a una lettera di unità** e fai clic su **Avanti**.
10. Specifica il file system da utilizzare, lascia la dimensione dell'unità di allocazione come *Predefinita*, assegna un nome al volume e fai clic su **Avanti**.
11. Rivedi le selezioni e fai clic su **Crea**, quindi su **Chiudi**.

### <a name="step-3-add-to-cluster-shared-volumes"></a>Passaggio 3: Aggiungere ai volumi condivisi del cluster

![Aggiungere ai volumi condivisi del cluster](media/creating-volumes/GUI-Step-2.png)

12. In Gestione cluster di failover, passa a **Archiviazione** -> **Dischi**.
13. Seleziona il disco virtuale appena creato e seleziona **Aggiungi a volumi condivisi cluster** dal riquadro Azioni a destra o fai clic con il pulsante destro del mouse sul disco virtuale e seleziona **Aggiungi a volumi condivisi cluster**.

Ecco fatto! Ripeti il numero di volte necessario per creare altri volumi.

## <a name="see-also"></a>Vedi anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
- [Pianificazione dei volumi in Spazi di archiviazione diretta](plan-volumes.md)
