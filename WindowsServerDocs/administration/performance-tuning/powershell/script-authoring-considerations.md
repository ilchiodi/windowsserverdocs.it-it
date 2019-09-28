---
title: Considerazioni sulle prestazioni di scripting di PowerShell
description: Creazione di script per le prestazioni in PowerShell
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: 2898cf5ee965da77c9f6a3473e55c1cee6b53f2b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71354982"
---
# <a name="powershell-scripting-performance-considerations"></a>Considerazioni sulle prestazioni di scripting di PowerShell

Gli script di PowerShell che sfruttano .NET direttamente ed evitano la pipeline tendono a essere più veloci rispetto a PowerShell idiomatiche. In genere, PowerShell usa i cmdlet e le funzioni di PowerShell, spesso sfruttando la pipeline e rilasciando .NET solo quando necessario.

>[!Note] 
> Molte delle tecniche descritte in questo articolo non sono di PowerShell idiomatiche e possono ridurre la leggibilità di uno script di PowerShell. Si consiglia agli autori di script di usare PowerShell idiomatiche, a meno che le prestazioni non vengano stabilite diversamente.

## <a name="suppressing-output"></a>Eliminazione dell'output

Esistono diversi modi per evitare di scrivere oggetti nella pipeline:

```PowerShell
$null = $arrayList.Add($item)
[void]$arrayList.Add($item)
```

L'assegnazione a `$null` o il cast al `[void]` sono approssimativamente equivalenti e dovrebbero in genere essere preferiti laddove le prestazioni sono importanti.

```PowerShell
$arrayList.Add($item) > $null
```

Il reindirizzamento di file a `$null` è quasi altrettanto valido delle alternative precedenti, la maggior parte degli script non si noterà mai la differenza.
A seconda dello scenario, il reindirizzamento dei file introduce un po' di sovraccarico.

```PowerShell
$arrayList.Add($item) | Out-Null
```

Il piping a `Out-Null` comporta un sovraccarico significativo rispetto alle alternative.
Evitando il codice sensibile alle prestazioni.

```PowerShell
$null = . {
    $arrayList.Add($item)
    $arrayList.Add(42)
}
```

Introducendo un blocco di script e chiamandolo (usando l'origine del punto o altro), l'assegnazione del risultato a `$null` è una tecnica utile per l'eliminazione dell'output di un blocco di script di grandi dimensioni.
Questa tecnica viene eseguita approssimativamente oltre che tramite pipe a `Out-Null` e deve essere evitata in uno script con distinzione delle prestazioni.
L'overhead aggiuntivo in questo esempio deriva dalla creazione e dalla chiamata di un blocco di script che in precedenza era uno script inline.


## <a name="array-addition"></a>Aggiunta di matrici

La generazione di un elenco di elementi viene spesso eseguita utilizzando una matrice con l'operatore di addizione:

```PowerShell
$results = @()
$results += Do-Something
$results += Do-SomethingElse
$results
```

Questo può essere molto inefficent perché le matrici non sono modificabili.
Ogni aggiunta all'array crea effettivamente una nuova matrice sufficientemente grande da consentire l'inserimento di tutti gli elementi degli operandi sinistro e destro, quindi copia gli elementi di entrambi gli operandi nella nuova matrice.
Per le raccolte di piccole dimensioni, questo sovraccarico potrebbe non essere rilevante.
Per le raccolte di grandi dimensioni, questo può costituire sicuramente un problema.

Esistono un paio di alternative.
Se non è effettivamente necessario un array, provare a usare un ArrayList:

```PowerShell
$results = [System.Collections.ArrayList]::new()
$results.AddRange((Do-Something))
$results.AddRange((Do-SomethingElse))
$results
```

Se è necessaria una matrice, è possibile utilizzare il proprio `ArrayList` e chiamare semplicemente `ArrayList.ToArray` quando si desidera la matrice.
In alternativa, è possibile consentire a PowerShell di creare il `ArrayList` e `Array` per l'utente:

```PowerShell
$results = @(
    Do-Something
    Do-SomethingElse
)
```

In questo esempio, tramite PowerShell viene creato un `ArrayList` per conservare i risultati scritti nella pipeline all'interno dell'espressione di matrice.
Appena prima di assegnare a `$results`, PowerShell converte il `ArrayList` in un `object[]`.

## <a name="processing-large-files"></a>Elaborazione file di grandi dimensioni

Il modo in cui è possibile elaborare un file in PowerShell potrebbe essere simile al seguente:

```PowerShell
Get-Content $path | Where-Object { $_.Length -gt 10 }
```

Questo può essere quasi un ordine di grandezza più lento rispetto all'uso diretto delle API .NET:

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

## <a name="avoid-write-host"></a>Evitare write-host

In genere, è consigliabile scrivere l'output direttamente nella console, ma quando è sensato, molti script utilizzano `Write-Host`.

Se è necessario scrivere molti messaggi nella console, `Write-Host` può essere un ordine di grandezza più lento rispetto a `[Console]::WriteLine()`. Tuttavia, tenere presente che `[Console]::WriteLine()` è solo un'alternativa appropriata per host specifici come PowerShell. exe o powershell_ise. exe. non è garantito che funzioni in tutti gli host.

Anziché usare `Write-Host`, provare a usare [Write-Output](/powershell/module/Microsoft.PowerShell.Utility/Write-Output?view=powershell-5.1).

