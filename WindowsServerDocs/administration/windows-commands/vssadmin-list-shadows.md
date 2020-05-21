---
title: Ombreggiatura elenco Vssadmin
description: Descrizione del comando vssadmin list shadows.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 38eda178b6c9e34fec1d63ed6c59f01023b859c4
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436675"
---
# <a name="vssadmin-list-shadows"></a>Ombreggiatura elenco Vssadmin

> Si applica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Elenca tutte le copie shadow esistenti di un volume specificato. Se si usa questo comando senza parametri, vengono visualizzate tutte le copie shadow del volume nel computer nell'ordine **indicato dal set di copie shadow**.

## <a name="syntax"></a>Sintassi

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---|---|
|/for = \< pervolumespecificato>|Specifica il volume per cui verranno elencate le copie shadow.|
|/Shadow = \< idshadow>|Elenca la copia shadow specificata da IDShadow. Per ottenere l'ID della copia shadow, usare il comando **vssadmin list shadows** . Quando si digita un ID copia shadow, usare il formato seguente, dove ogni *X* rappresenta un carattere esadecimale:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|

## <a name="additional-references"></a>Riferimenti aggiuntivi

* [Chiave sintassi della riga di comando](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)