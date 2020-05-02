---
title: chcp
description: Argomento di riferimento per il comando CHCP, che consente di modificare la tabella codici della console attiva.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc7b1c71-7b80-443d-9cf1-9bcf305aa1fd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f1291176ed5245b06c68491f0d5cb0ae9b0b600
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82715322"
---
# <a name="chcp"></a>chcp

Modifica la tabella codici della console attiva. Se utilizzata senza parametri, **chcp** Visualizza il numero della tabella codici della console attiva.

## <a name="syntax"></a>Sintassi

```
chcp [<nnn>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<nnn>` | Specifica la tabella codici. |
| /? | Visualizza la guida al prompt dei comandi. |

La tabella seguente elenca ogni codice supportate pagina e il paese/regione o lingua:

| Tabella codici | Paese/regione o lingua |
| --------- | -------------------------- |
| 437 | Stati Uniti |
| 850 | Multilingue (latino I) |
| 852 | Slavo (latino II) |
| 855 | Cirillico (russo) |
| 857 | Turco |
| 860 | Portoghese |
| 861 | Islandese |
| 863 | Francese (Canada) |
| 865 | Area lingue nordiche |
| 866 | Russo |
| 869 | Greco moderno |
| 936 | Cinese |

#### <a name="remarks"></a>Osservazioni

- Solo la tabella codici OEM (OEM) viene installata con Windows viene visualizzato correttamente in una finestra del prompt dei comandi che utilizza caratteri Raster. Le altre tabelle codici vengono visualizzati correttamente in modalità a schermo intero o in windows il prompt dei comandi che utilizzano tipi di carattere TrueType.

- Non è necessario preparare le tabelle codici (come in MS-DOS).

- I programmi che si avvia dopo aver assegnano una nuova pagina codice, utilizzare la nuova pagina di codice. Tuttavia, i programmi (eccetto cmd. exe) avviati prima di assegnare la nuova tabella codici continueranno a usare la tabella codici originale.

## <a name="examples"></a>Esempi

Per visualizzare l'impostazione della tabella codici attiva, digitare:

```
chcp
```

Viene visualizzato un messaggio simile al seguente:`Active code page: 437`

Per impostare la tabella codici attiva su 850 (multilingue), digitare:

```
chcp 850
```

Se la tabella codici specificata non è valida, viene visualizzato il messaggio di errore seguente:`Invalid code page`

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
