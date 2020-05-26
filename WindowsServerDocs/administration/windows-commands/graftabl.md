---
title: graftabl
description: Argomento di riferimento per il comando graftabl, che consente ai sistemi operativi Windows di visualizzare un set di caratteri estesi in modalità grafica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 149ae92db534cef66c966462e51906304588b042
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818691"
---
# <a name="graftabl"></a>graftabl

Consente ai sistemi operativi Windows visualizzare un carattere esteso impostato in modalità grafica. Se utilizzata senza parametri, **graftabl** Visualizza precedente e la tabella codici corrente.

## <a name="syntax"></a>Sintassi

```
graftabl <codepage>
graftabl /status
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<codepage>` | Specifica una tabella codici per definire l'aspetto dei caratteri estesi in modalità grafica. Numeri di identificazione di pagina di codice validi sono:<ul><li>**437** -Stati Uniti</li><li>**850** -multilingue (alfabeto latino)</li><li>**852** -slavo (Latino II)</li><li>**855** -alfabeto cirillico (Russo)</li><li>**857** -turco</li><li>**860** -portoghese</li><li>**861** -islandese</li><li>**863** -Canada-francese</li><li>**865** -Nordic</li><li>**866** -russo</li><li>**869** -greco moderno</li></ul> |
| /status | Visualizza la tabella codici corrente utilizzata da questo comando. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Il comando **graftabl** influiscono solo sulla visualizzazione dei caratteri estesi della tabella codici specificata. Non modifica la tabella codici effettiva di input della console. Per modificare la tabella codici di input della console, utilizzare il [modalità](mode.md) o [chcp](chcp.md) comando.

- Ogni codice di uscita e una breve descrizione:

    | Codice di uscita | Descrizione |
    | --------- | ----------- |
    | 0 | Set di caratteri è stato caricato correttamente. Nessuna pagina di codice precedente è stato caricato. |
    | 1 | È stato specificato un parametro errato. Nessuna azione effettuata. |
    | 2 | Si è verificato un errore di file. |

- È possibile utilizzare la variabile di ambiente ERRORLEVEL in un file batch per elaborare i codici di uscita restituiti da **graftabl**.

### <a name="examples"></a>Esempi

Per visualizzare la tabella codici corrente utilizzata dal **graftabl**, tipo:

```
graftabl /status
```

Per caricare il set di caratteri grafici per la tabella codici 437 (Stati Uniti) in memoria, digitare:

```
graftabl 437
```

Per caricare il grafica del set di caratteri per la tabella codici 850 (multilingue) in memoria, digitare:

```
graftabl 850
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando freedisk](freedisk.md)

- [comando Mode](mode.md)

- [comando CHCP](chcp.md)
