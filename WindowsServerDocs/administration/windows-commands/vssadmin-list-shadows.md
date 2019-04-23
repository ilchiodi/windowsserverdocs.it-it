---
title: Shadows elenco vssadmin
description: Una descrizione dell'elenco vssadmin nasconde comando.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 07da36da1473563c3236a4fafc3ceae06259981a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861152"
---
# <a name="vssadmin-list-shadows"></a>Shadows elenco vssadmin

>Si applica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Elenca tutte le copie shadow esistenti di un volume specificato. Se si usa questo comando senza parametri, Visualizza tutte le copie shadow del volume del computer nell'ordine stabilito dal **Shadow copia impostato**.

## <a name="syntax"></a>Sintassi

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---|---|
|/for=\<ForVolumeSpec>|Specifica che le copie shadow verranno elencate per il volume.|
|/shadow=\<ShadowID>|Elenca la copia shadow specificata da IDShadow. Per ottenere l'ID della copia shadow, usare il **shadows elenco vssadmin** comando. Quando si digitare un ID della copia shadow, usare il formato seguente, dove ogni *X* rappresenta un carattere esadecimale:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|

## <a name="additional-references"></a>Altri riferimenti

* [Chiave sintassi della riga di comando](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)