---
title: Spostare e ridimensionare la cache ospitata (facoltativa)
description: In questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata sul computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b0e3b6b490dead32071d99becccd9dca937f1f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406354"
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>Spostare e ridimensionare la cache ospitata \(Optional @ no__t-1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare questa procedura per spostare la cache ospitata per l'unità e la cartella che si preferisce e per specificare la quantità di spazio su disco che il server cache ospitata può utilizzare per la cache ospitata.

Questa procedura è facoltativa. Se il valore predefinito nella cache percorso \(% windir %\\ServiceProfiles\\NetworkService\\AppData\\locale\\PeerDistPub\) e dimensioni, che sono 5% di spazio su disco totale: sono appropriati per la distribuzione, non è necessario modificarle.

Per eseguire questa procedura, è necessario essere membri del gruppo Administrators.

### <a name="to-move-and-resize-the-hosted-cache"></a>Per spostare e ridimensionare la cache ospitata

1. Aprire Windows PowerShell con privilegi di amministratore.

2. Digitare il comando seguente per spostare la cache ospitata in un'altra posizione nel computer locale e quindi premere INVIO.

    > [!IMPORTANT]
    > Prima di eseguire il comando seguente, sostituire i valori di parametro, ad esempio – percorso e – MoveTo, con valori appropriati per la distribuzione.

    ``` 
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ``` 

3.  Digitare il comando seguente per ridimensionare il contenuto nella cache: specificamente datacache \- nel computer locale. Premere INVIO.

    > [!IMPORTANT]
    > Prima di eseguire il comando seguente, sostituire i valori dei parametri, ad esempio \-percentuale, con valori appropriati per la distribuzione.  

    ``` 
    Set-BCCache -Percentage 20
    ``` 

4.  Per verificare la configurazione del server cache ospitata, digitare il comando seguente e premere INVIO.

    ``` 
    Get-BCStatus
    ``` 

    I risultati dello stato di visualizzazione del comando per tutti gli aspetti dell'installazione di BranchCache. Di seguito sono riportate alcune delle impostazioni di BranchCache e il valore corretto per ogni elemento:

    -   DataCache | CacheFileDirectoryPath: Visualizza il percorso del disco rigido che corrisponde al valore fornito con il parametro – MoveTo del comando SetBCCache. Ad esempio, se è stato specificato il valore d\\datacache, di valore viene visualizzato nell'output del comando.

    -   DataCache | MaxCacheSizeAsPercentageOfDiskVolume: Visualizza il numero che corrisponde al valore specificato con il parametro – percentuale del comando SetBCCache. Ad esempio, se il valore 20, tale valore viene visualizzato nell'output del comando.

Per continuare con questa Guida, vedere [Prehash e Preload contenuto sul Server Cache ospitata & #40; facoltativo & #41;](7-Bc-Prehash-Preload.md).