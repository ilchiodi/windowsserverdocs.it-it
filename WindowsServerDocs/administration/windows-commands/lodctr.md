---
title: lodctr
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 33c14970a669d24f1cc803003e8530712311c564
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840954"
---
# <a name="lodctr"></a>lodctr

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di registrare o salvare di impostazioni del Registro di sistema e nome del contatore delle prestazioni in un file e definire servizi attendibili.
## <a name="syntax"></a>Sintassi
```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```
#### <a name="parameters"></a>Parametri

|    Parametro     |                                                                                                                                         Descrizione                                                                                                                                          |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <filename>    |                                                                                          Registra le impostazioni del nome del contatore delle prestazioni e la descrizione fornita nel file di inizializzazione nome file.                                                                                          |
|  /s:<filename>   |                                                                                                       Prestazioni consente di salvare le impostazioni del Registro di sistema del contatore e testo esplicativo per file <filename>.                                                                                                       |
|        /r        |                                Ripristina le impostazioni del Registro di sistema dei contatori e il testo esplicativo dai file memorizzati nella cache delle prestazioni correlati nel Registro di sistema e le impostazioni del Registro di sistema corrente.<p>Questa opzione è disponibile solo nel sistema operativo Windows Server 2003.                                |
|  /r:<filename>   | Ripristini prestazioni del contatore delle impostazioni del Registro di sistema e il testo esplicativo dal file <filename>. **Avviso:** se si usa il comando **lodctr/r** , si sovrascriveranno tutte le impostazioni del registro di sistema del contatore delle prestazioni e il testo esplicativo, sostituendolo con la configurazione definita nel file specificato. |
| /t:<servicename> |                                                                                                                       Indica il servizio <servicename> è attendibile.                                                                                                                       |
|        /?        |                                                                                                                             Visualizza la guida al prompt dei comandi.                                                                                                                             |

## <a name="remarks"></a>Note
Se le informazioni fornite contengono spazi, racchiudere il testo tra virgolette, ad esempio <filename>.
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi
Per salvare le impostazioni del Registro di sistema corrente e contatore testo esplicativo file **prestazioni backup prest1. txt**:
```
lodctr /s:perf backup1.txt
```
## <a name="additional-references"></a>Altre informazioni di riferimento
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

