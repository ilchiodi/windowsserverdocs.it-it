---
title: Rimozione di server in Spazi di archiviazione diretta
ms.assetid: 9d8499a7-1307-473d-9f00-8a051164fad2
ms.prod: windows-server
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
description: Come rimuovere i server da un cluster di Spazi di archiviazione diretta in Windows Server.
ms.date: 2/5/2017
ms.localizationpriority: medium
ms.openlocfilehash: ce8caef2b51279c97cc012045750b7a73d97a4ba
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402802"
---
# <a name="removing-servers-in-storage-spaces-direct"></a>Rimozione di server in Spazi di archiviazione diretta

>Si applica a: Windows Server 2019, Windows Server 2016

In questo argomento viene descritto come rimuovere i server in [Spazi di archiviazione diretta](storage-spaces-direct-overview.md) tramite PowerShell.

## <a name="remove-a-server-but-leave-its-drives"></a>Rimuovere un server, ma lasciare le relative unità

Se prevedi di riaggiungere il server nel cluster a breve o intendi mantenere le relative unità spostandole in un altro server, puoi rimuovere il server dal cluster *senza* rimuovere le relative unità dal pool di archiviazione. Questo è il comportamento predefinito se utilizzi Gestione cluster di failover per rimuovere il server.

Utilizza il cmdlet [Remove-ClusterNode](https://technet.microsoft.com/library/hh847251.aspx) di PowerShell:

```PowerShell
Remove-ClusterNode <Name>
```

Questo cmdlet ha esito positivo rapidamente, indipendentemente dalle eventuali considerazioni sulla capacità, perché il pool di archiviazione "memorizza" le unità mancanti e prevede che verranno reinserite. Non avviene nessuno spostamento di dati dalle unità mancanti. Mentre sono mancanti, il loro **OperationalStatus** verrà visualizzato come "Comunicazione interrotta" e i volumi visualizzeranno "Incompleto".

Al reinserimento delle unità, queste vengono automaticamente rilevate e riassociate al pool, anche se sono ora in un nuovo server.

   >[!WARNING]
   > Non distribuire le unità con i dati del pool da un server a più altri server. Ad esempio, se un server con dieci unità ha problemi (ad esempio per un guasto della scheda madre o dell'unità di avvio), **puoi** spostare tutte e dieci le unità in un nuovo server, ma **non puoi** spostare ciascuna separatamente in diversi altri server.

## <a name="remove-a-server-and-its-drives"></a>Rimuovere un server e le relative unità

Se vuoi rimuovere un server dal cluster in modo permanente (operazione talvolta detta ridimensionamento), puoi rimuovere il server dal cluster *e* rimuovere le relative unità dal pool di archiviazione.

Utilizza il cmdlet **Remove-ClusterNode** con il flag facoltativo **- CleanUpDisks**:

```PowerShell
Remove-ClusterNode <Name> -CleanUpDisks
```

L'esecuzione di questo cmdlet potrebbe richiedere molto tempo (a volte molte ore) perché Windows deve spostare tutti i dati archiviati nel server su altri server del cluster. Al termine, le unità vengono rimosse in modo definitivo dal pool di archiviazione, riportando i volumi interessati a uno stato di integrità.

### <a name="requirements"></a>Requisiti

Per il ridimensionamento permanente (rimozione di un server *e* delle relative unità), il cluster deve soddisfare i due requisiti seguenti. In caso contrario, il cmdlet **Remove-ClusterNode -CleanUpDisks** restituirà un errore immediatamente, prima di iniziare qualsiasi spostamento di dati, per ridurre al minimo le interruzioni.

#### <a name="enough-capacity"></a>Capacità sufficiente

Prima di tutto, è necessario disporre di una capacità di archiviazione sufficiente nei server rimanenti per supportare tutti i volumi.

Ad esempio, se disponi di quattro server, ognuno con 10 unità da 1 TB, hai 40 TB di capacità di archiviazione fisica totale. Dopo la rimozione di un server e tutte le relative unità, disporrai di 30 TB di capacità rimasta. Se i footprint dei volumi ammontano a più di 30 TB nel complesso, non potranno essere contenuti nei server rimanenti, quindi il cmdlet restituirà un errore e non sposterà alcun dato.

#### <a name="enough-fault-domains"></a>Domini di errore sufficienti

In secondo luogo, devi disporre di domini di errore (in genere server) sufficienti a fornire la resilienza dei volumi.

Ad esempio, se i volumi utilizzano il mirroring a tre vie a livello di server per la resilienza, non possono essere completamente integri a meno che tu non disponga di almeno tre server. Se disponi di esattamente tre server e tenti di rimuoverne uno e tutte le relative unità, il cmdlet restituirà un errore e non sposterà alcun dato.

Questa tabella mostra il numero minimo di domini di errore necessari per ogni tipo di resilienza.

|    Resilienza          |    Numero minimo di domini di errore necessari   |
|------------------------|-------------------------------------|
|    Mirroring a 2 vie      |    2                                |
|    Mirroring a 3 vie    |    3                                |
|    Doppia parità         |    4                                |

   >[!NOTE]
   > È possibile disporre di meno server per un breve periodo, ad esempio durante guasti o manutenzione. Tuttavia, per ripristinare uno stato completamente integro per i volumi, devi disporre del numero minimo di server elencati sopra.

## <a name="see-also"></a>Vedere anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
