---
title: Vssadmin delete ombreggiature
description: Descrizione del comando vssadmin Elimina ombreggiature.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 71119174c7fc5929eb4e1982183ba0aed7eb1735
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082256"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin delete ombreggiature

>Si applica a: 10 Windows, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Elimina copia shadow del volume specificato.

## <a name="syntax"></a>Sintassi

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---|---|
|/ for = \ < PerVolumeSpecificato >|Specifica copia shadow del volume che verrà eliminata.|
|/ meno recente|Elimina la copia shadow meno recente.|
|/all|Consente di eliminare tutte le copie shadow del volume specificato.|
|/ ombreggiatura = \ < IDShadow >|Elimina la copia shadow specificata da IDShadow. Per ottenere l'ID di una copia shadow, utilizzare il comando **ombreggiature elenco vssadmin** . Quando si immette un ID di una copia shadow, utilizzare il formato seguente, dove ogni *X* rappresenta un carattere esadecimale:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|
|/ non interattiva|Specifica che il comando non verrà visualizzati i messaggi mentre è in esecuzione.|

## <a name="remarks"></a>Osservazioni

È possibile eliminare solo le copie shadow con il tipo di client accessibile.

## <a name="examples"></a>Esempi

Per eliminare il meno recente copia shadow del volume C, immettere il seguente comando:

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>Ulteriori riferimenti

* [Chiave sintassi della riga di comando](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [Ombreggiature elenco vssadmin](vssadmin-list-shadows.md)