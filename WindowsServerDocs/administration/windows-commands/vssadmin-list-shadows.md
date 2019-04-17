---
title: Ombreggiature elenco vssadmin
description: Una descrizione dell'elenco vssadmin nasconde comando.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 07da36da1473563c3236a4fafc3ceae06259981a
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082347"
---
# <a name="vssadmin-list-shadows"></a>Ombreggiature elenco vssadmin

>Si applica a: 10 Windows, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Vengono elencate tutte le copie shadow esistente di un volume specificato. Se si utilizza questo comando senza parametri, Visualizza tutte le copie shadow del volume sul computer nell'ordine stabilita dal **Set di copia Shadow**.

## <a name="syntax"></a>Sintassi

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---|---|
|/ for = \ < PerVolumeSpecificato >|Specifica che le copie shadow è elencate per il volume.|
|/ ombreggiatura = \ < IDShadow >|Elenca la copia shadow specificata da IDShadow. Per ottenere l'ID di una copia shadow, utilizzare il comando **ombreggiature elenco vssadmin** . Quando si digita un ID di una copia shadow, utilizzare il formato seguente, dove ogni *X* rappresenta un carattere esadecimale:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|

## <a name="additional-references"></a>Ulteriori riferimenti

* [Chiave sintassi della riga di comando](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)