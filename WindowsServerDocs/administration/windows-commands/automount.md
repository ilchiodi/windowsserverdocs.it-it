---
title: automount
description: Argomento di riferimento per il comando automount, che Abilita o Disabilita la funzionalità di montaggio automatico.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4635fc91-a477-4f17-8dcc-aa08854bfe45
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a3ff8782b2110dd1b8039477c0b748dc4ab8f44
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718724"
---
# <a name="automount"></a>automount

Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

> [!IMPORTANT]
> Nelle configurazioni SAN (Storage Area Network), la disabilitazione del montaggio automatico impedisce a Windows di montare o assegnare automaticamente le lettere di unità a tutti i nuovi volumi di base visibili al sistema.

## <a name="syntax"></a>Sintassi

montaggio automatico [{Enable | Disable | scrub}] [noerr]

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| abilitare | Consente a Windows di montare automaticamente nuovi volumi di base e dinamici aggiunti al sistema e di assegnare loro lettere di unità. |
| disabilitare | Impedisce a Windows di montare automaticamente i nuovi volumi di base e dinamici aggiunti al sistema.<p>**Nota**: la disabilitazione del montaggio automatico può causare la mancata riuscita della parte di archiviazione della convalida guidata configurazione per i cluster di failover. |
| scrub | Rimuove le directory del punto di montaggio del volume e le impostazioni del registro di sistema per i volumi che non sono più presenti nel sistema. In questo modo si impedisce che i volumi precedentemente montati nel sistema vengano montati automaticamente e che i relativi punti di montaggio del volume precedenti vengano aggiunti al sistema. |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="examples"></a>Esempi

Per verificare se la funzionalità di montaggio automatico è abilitata, digitare i comandi seguenti all'interno del comando DiskPart:

```
automount
```

Per abilitare la funzionalità di montaggio automatico, digitare:

```
automount enable
```

Per disabilitare la funzionalità di montaggio automatico, digitare:

```
automount disable
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comandi DiskPart](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc770877(v%3dws.11))
