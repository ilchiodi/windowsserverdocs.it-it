---
title: detach vdisk
description: Argomento di riferimento per il comando Scollega vdisk, che consente di arrestare il disco rigido virtuale (VHD) selezionato come unità disco rigido locale nel computer host.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 427b27630341589f3ff6dd422667e1247f5b64ec
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993075"
---
# <a name="detach-vdisk"></a>detach vdisk

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Arresta la visualizzazione del disco rigido virtuale (VHD) selezionato come unità disco rigido locale nel computer host. Quando viene reso non visibile, il VHD può essere copiato in percorsi diversi. Prima di iniziare, è necessario selezionare un disco rigido virtuale affinché questa operazione abbia esito positivo. Utilizzare il [Selezionare vdisk](select-vdisk.md) comando per selezionare un disco rigido Virtuale e spostare lo stato attivo a esso.


## <a name="syntax"></a>Sintassi

```
detach vdisk [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="examples"></a>Esempi

Per scollegare il disco rigido Virtuale selezionato, digitare:

```
detach vdisk
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Connetti vdisk](attach-vdisk.md)

- [comando Compact vdisk](compact-vdisk.md)

- [comando vdisk dettaglio](detail-vdisk.md)

- [Espandi comando vdisk](expand-vdisk.md)

- [Comando merge vdisk](merge-vdisk.md)

- [Selezionare il comando vdisk](select-vdisk.md)

- [comando list](list.md)
