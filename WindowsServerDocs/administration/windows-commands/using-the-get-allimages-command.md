---
title: Get-AllImages
description: Windows Commands argomento per Get-AllImages, che consente di recuperare informazioni su tutte le immagini in un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1358d7ae4a86b6439b9a304e10e3aa569112d5a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831304"
---
# <a name="get-allimages"></a>Get-AllImages

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informazioni su tutte le immagini in un server.

## <a name="syntax"></a>Sintassi
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/: Mostra {Avvio & #124; Installa & #124; LegacyRis & #124; All}|-   **boot** restituisce solo le immagini di avvio.<br />-   **Install** restituisce le immagini di installazione e le informazioni sui gruppi di immagini che li contengono.<br />-   **LegacyRis** restituisce solo le immagini dei servizi di installazione remota (RIS).<br />-   restituisce tutte le informazioni sull'immagine **d'** avvio, installare le informazioni sulle immagini (incluse le informazioni sui gruppi di immagini) e le informazioni sull'immagine di RIS.|
|[/detailed]|Indica che tutti i metadati delle immagini da ogni immagine devono essere restituito. Se si omette questa opzione, il comportamento predefinito è per restituire solo il nome dell'immagine, descrizione e nome file.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per visualizzare informazioni sulle immagini, digitare uno dei seguenti:
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
usando il comando [add-](using-the-add-image-command.md) image
[usando il comando copy-image](using-the-copy-image-command.md)
usando il comando [Export-](using-the-export-image-command.md) image
usando il comando [Remove-Image](using-the-remove-image-command.md)
[usando il comando Replace-Image](using-the-replace-image-command.md)
[sottocomando: Set-Image](subcommand-set-image.md)
