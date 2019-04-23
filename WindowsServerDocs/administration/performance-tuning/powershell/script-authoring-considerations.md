---
title: Considerazioni sulle prestazioni script PowerShell
description: Creazione di script per le prestazioni in PowerShell
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: df406c7e382907ca32006ec4dae6537a140a91cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830372"
---
# <a name="powershell-scripting-performance-considerations"></a>Considerazioni sulle prestazioni script PowerShell

Script di PowerShell che sfruttano .NET direttamente ed evitare la pipeline tendono a essere più veloce di PowerShell idiomatico. PowerShell idiomatico Usa in genere i cmdlet e funzioni di PowerShell, molto spesso sfruttando la pipeline e lo spostamento verso il basso in .NET solo quando è necessario.

>[!Note] 
> Molte delle tecniche descritte di seguito non sono idiomatico PowerShell e può ridurne la leggibilità di uno script di PowerShell. Gli autori di script sono invitati a usare PowerShell idiomatico a meno che non determina la prestazioni in caso contrario.

## <a name="suppressing-output"></a>Eliminazione di Output

Esistono molti modi per evitare la scrittura di oggetti alla pipeline:

```PowerShell
$null = $arrayList.Add($item)
[void]$arrayList.Add($item)
```

Assegnazione a `$null` o eseguire il cast a `[void]` sono all'incirca equivalenti e in genere devono essere preferito in cui è importante delle prestazioni.

```PowerShell
$arrayList.Add($item) > $null
```

Il reindirizzamento del file `$null` è quasi efficace alternativa precedente, più script sarebbe impercettibile la differenza.
A seconda dello scenario, il reindirizzamento del file presentano tuttavia un po' di sovraccarico.

```PowerShell
$arrayList.Add($item) | Out-Null
```

Inviare tramite pipe a `Out-Null` presenta un overhead significativo rispetto alle alternative.
Dovrebbe essere evitando nel codice sensibile delle prestazioni.

```PowerShell
$null = . {
    $arrayList.Add($item)
    $arrayList.Add(42)
}
```

Introduzione a un blocco di script e nel chiamarlo (utilizzando la notazione punto determinazione dell'origine o in caso contrario), quindi assegnare il risultato a `$null` è una tecnica utile per l'eliminazione di output di un blocco di script di grandi dimensioni.
Questa tecnica consente di eseguire più o meno, nonché di invio tramite pipe a `Out-Null` e devono essere evitate negli script sensibile delle prestazioni.
Il sovraccarico aggiuntivo in questo esempio deriva dalla creazione delle per richiamare un blocco di script che si trovava precedentemente script inline.


## <a name="array-addition"></a>Aggiunta di matrice

Generazione di un elenco di elementi viene spesso eseguita utilizzando una matrice con l'operatore di addizione:

```PowerShell
$results = @()
$results += Do-Something
$results += Do-SomethingElse
$results
```

Può trattarsi di inefficent molto poiché le matrici non sono modificabili.
Ogni aggiunta alla matrice effettivamente crea una nuova matrice sufficientemente grande per contenere tutti gli elementi di entrambi gli operandi sinistro e destro, quindi copia gli elementi di entrambi gli operandi nella nuova matrice.
Per le raccolte di piccole dimensioni, questo sovraccarico potrebbe non essere rilevante.
Per le raccolte di grandi dimensioni, può essere sicuramente un problema.

Esistono un paio di alternative.
Se non è in realtà una richiesta a una matrice, considerare invece l'uso su un ArrayList:

```PowerShell
$results = [System.Collections.ArrayList]::new()
$results.AddRange((Do-Something))
$results.AddRange((Do-SomethingElse))
$results
```

Se è necessaria una matrice, è possibile usare il proprio `ArrayList` semplicemente chiamare `ArrayList.ToArray` quando si desidera che la matrice.
In alternativa, è possibile lasciare che PowerShell crea la `ArrayList` e `Array` automaticamente:

```PowerShell
$results = @(
    Do-Something
    Do-SomethingElse
)
```

In questo esempio di PowerShell crea un `ArrayList` per contenere i risultati scritti la pipeline all'interno dell'espressione di matrice.
Prima di assegnare `$results`, PowerShell converte il `ArrayList` a un `object[]`.

## <a name="processing-large-files"></a>L'elaborazione dei file di grandi dimensioni

Il modo idiomatico per elaborare un file di PowerShell potrebbe essere simile a:

```PowerShell
Get-Content $path | Where-Object { $_.Length -gt 10 }
```

Questo può essere quasi un ordine di grandezza inferiore usando direttamente le API .NET:

```PowerShell
try
{
    $stream = [System.IO.StreamReader]::new($path)
    while ($line = $stream.ReadLine())
    {
        if ($line.Length -gt 10)
        {
            $line
        }
    }
}
finally
{
    $stream.Dispose()
}
```

## <a name="avoid-write-host"></a>Evitare di Write-Host

In genere viene considerata una procedura soddisfacente per scrivere l'output direttamente alla console, ma quando sarà opportuno, molti script usano `Write-Host`.

Se è necessario scrivere numerosi messaggi nella console `Write-Host` può essere più lento rispetto a un ordine di grandezza `[Console]::WriteLine()`. Tuttavia, tenere presente che `[Console]::WriteLine()` è solo un'alternativa adatta per gli host specifici, ad esempio powershell.exe o powershell_ise.exe: non è necessariamente funzionare in tutti gli host.

Invece di usare `Write-Host`, è consigliabile usare [Write-Output](/powershell/module/Microsoft.PowerShell.Utility/Write-Output?view=powershell-5.1).

