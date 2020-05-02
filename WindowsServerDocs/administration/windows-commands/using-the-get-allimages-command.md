---
title: Get-AllImages
description: Argomento di riferimento per Get-AllImages, che consente di recuperare informazioni su tutte le immagini in un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1f32a1789b22d04b7b61979d0ea49d91f0cf157
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720023"
---
# <a name="get-allimages"></a>Get-AllImages

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informazioni su tutte le immagini in un server.

## <a name="syntax"></a>Sintassi
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/: Mostra {Avvio & #124; Installa & #124; LegacyRis & #124; All}|-   L' **avvio** restituisce solo le immagini di avvio.<br />-   **Install** restituisce le immagini di installazione e le informazioni sui gruppi di immagini che li contengono.<br />-   **LegacyRis** restituisce solo le immagini di servizi di installazione remota (RIS).<br />-   **Tutte** le informazioni sull'immagine di avvio vengono restituite, installare le informazioni sulle immagini (incluse le informazioni sui gruppi di immagini) e le informazioni sull'immagine di RIS.|
|[/detailed]|Indica che tutti i metadati delle immagini da ogni immagine devono essere restituito. Se si omette questa opzione, il comportamento predefinito è per restituire solo il nome dell'immagine, descrizione e nome file.|
## <a name="examples"></a>Esempi
Per visualizzare informazioni sulle immagini, digitare uno dei seguenti:
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando usando il comando[Add-Image](using-the-add-image-command.md)
usando
[il comando copy-](using-the-copy-image-command.md)Image[usando il comando Export-Image](using-the-export-image-command.md)
usando il[comando](using-the-remove-image-command.md)
Remove-Image[usando il comando](using-the-replace-image-command.md)
Replace-Image[sottocomando: Set-Image](subcommand-set-image.md)
