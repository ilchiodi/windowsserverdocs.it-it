---
title: Utilizzando il comando get-AllImages
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5122a5660031d503795715c0005b404f910d6626
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363504"
---
# <a name="using-the-get-allimages-command"></a>Utilizzando il comando get-AllImages

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informazioni su tutte le immagini in un server.
## <a name="syntax"></a>Sintassi
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/: Mostra {Avvio &#124; Installa &#124; LegacyRis &#124; All}|-   **boot** restituisce solo le immagini di avvio.<br />-   **Install** restituisce le immagini di installazione e le informazioni sui gruppi di immagini che li contengono.<br />-   **LegacyRis** restituisce solo le immagini di servizi di installazione remota (RIS).<br />@no__t 0 restituisce**tutte** le informazioni sull'immagine d'avvio, installare le informazioni sulle immagini (incluse le informazioni sui gruppi di immagini) e le informazioni sull'immagine RIS.|
|[/detailed]|Indica che tutti i metadati delle immagini da ogni immagine devono essere restituito. Se si omette questa opzione, il comportamento predefinito è per restituire solo il nome dell'immagine, descrizione e nome file.|
## <a name="BKMK_examples"></a>Esempi
Per visualizzare informazioni sulle immagini, digitare uno dei seguenti:
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave della sintassi della riga di comando](command-line-syntax-key.md)
 usando il comando[add-image](using-the-add-image-command.md)
 usando il comando[Copy-Image](using-the-copy-image-command.md)
 usando il comando[Export-Image](using-the-export-image-command.md)
[usando il comando Remove-Image](using-the-remove-image-command.md)
[usando il comando Comando Replace-Image](using-the-replace-image-command.md)1[sottocomando: Set-Image](subcommand-set-image.md)
