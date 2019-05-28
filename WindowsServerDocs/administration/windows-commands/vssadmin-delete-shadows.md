---
title: Shadows delete vssadmin
description: Descrizione del comando vssadmin delete ombreggiature.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 83074017e4ae412cf0aec654f6ab5901ad8039e2
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/24/2019
ms.locfileid: "63706621"
---
# <a name="vssadmin-delete-shadows"></a>Shadows delete vssadmin

>Si applica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Elimina le copie shadow del volume specificato.

## <a name="syntax"></a>Sintassi

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---|---|
|/for=\<ForVolumeSpec>|Specifica la copia shadow del volume che verrà eliminata.|
|/oldest|Elimina solo la copia shadow meno recente.|
|/all|Elimina tutte le copie shadow del volume specificato.|
|/shadow=\<ShadowID>|Elimina la copia shadow specificata da IDShadow. Per ottenere l'ID della copia shadow, usare il **shadows elenco vssadmin** comando. Quando si immette un ID della copia shadow, usare il formato seguente, dove ogni *X* rappresenta un carattere esadecimale:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|
|/quiet|Specifica che il comando non visualizzerà messaggi durante l'esecuzione.|

## <a name="remarks"></a>Note

È possibile eliminare le copie shadow solo con il tipo accessibili dal client.

## <a name="examples"></a>Esempi

Per eliminare la copia shadow meno recente del volume C, immettere questo comando:

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>Altri riferimenti

* [Chiave sintassi della riga di comando](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [Shadows elenco vssadmin](vssadmin-list-shadows.md)