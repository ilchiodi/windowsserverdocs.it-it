---
title: lodctr
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 96a2818110b766071eb83822abd34d8c0d00132d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374565"
---
# <a name="lodctr"></a>lodctr

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di registrare o salvare di impostazioni del Registro di sistema e nome del contatore delle prestazioni in un file e definire servizi attendibili.
## <a name="syntax"></a>Sintassi
```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```
### <a name="parameters"></a>Parametri

|    Parametro     |                                                                                                                                         Descrizione                                                                                                                                          |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <filename>    |                                                                                          Registra le impostazioni del nome del contatore delle prestazioni e la descrizione fornita nel file di inizializzazione nome file.                                                                                          |
|  /s: <filename>   |                                                                                                       Prestazioni consente di salvare le impostazioni del Registro di sistema del contatore e testo esplicativo per file <filename>.                                                                                                       |
|        /r        |                                Ripristina le impostazioni del Registro di sistema dei contatori e il testo esplicativo dai file memorizzati nella cache delle prestazioni correlati nel Registro di sistema e le impostazioni del Registro di sistema corrente.<br /><br />Questa opzione è disponibile solo nel sistema operativo Windows Server 2003.                                |
|  /r: <filename>   | Ripristini prestazioni del contatore delle impostazioni del Registro di sistema e il testo esplicativo dal file <filename>. **Avviso:** se si usa il comando **lodctr/r** , si sovrascriveranno tutte le impostazioni del registro di sistema del contatore delle prestazioni e il testo esplicativo, sostituendolo con la configurazione definita nel file specificato. |
| /t: <servicename> |                                                                                                                       Indica il servizio <servicename> è attendibile.                                                                                                                       |
|        /?        |                                                                                                                             Visualizza la guida al prompt dei comandi.                                                                                                                             |

## <a name="remarks"></a>Note
Se le informazioni fornite contengono spazi, utilizzare il testo tra virgolette (ad esempio, "<filename>").
## <a name="BKMK_Examples"></a>Esempi
Per salvare le impostazioni del Registro di sistema corrente e contatore testo esplicativo file **prestazioni backup prest1. txt**:
```
lodctr /s:"perf backup1.txt"
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

