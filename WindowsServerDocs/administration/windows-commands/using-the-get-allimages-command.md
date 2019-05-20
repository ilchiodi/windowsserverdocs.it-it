---
title: Utilizzando il comando get-AllImages
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 57b81dd3dd3a24876c4401e80d08130ed5243888
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872562"
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
|/: Mostra {Avvio &#124; Installa &#124; LegacyRis &#124; All}|-   **Avvio** restituisce solo le immagini di avvio.<br />-   **Installare** restituisce installare immagini, nonché informazioni sui gruppi di immagini che li contengono.<br />-   **LegacyRis** restituisce solo le immagini di servizi di installazione (RIS) remoto.<br />-   **Tutti i** restituisce informazioni sulle immagini RIS, informazioni sulle immagini di installazione (incluse le informazioni sui gruppi di immagini) e le informazioni sulle immagini di avvio.|
|[/ dettagliate]|Indica che tutti i metadati delle immagini da ogni immagine devono essere restituito. Se si omette questa opzione, il comportamento predefinito è per restituire solo il nome dell'immagine, descrizione e nome file.|
## <a name="BKMK_examples"></a>Esempi
Per visualizzare informazioni sulle immagini, digitare uno dei seguenti:
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando add-immagine](using-the-add-image-command.md)
[utilizzando il comando Copia immagine](using-the-copy-image-command.md)
[tramite il Comando Export-Image](using-the-export-image-command.md)
[utilizzando il comando remove-immagine](using-the-remove-image-command.md)
[usando l'immagine di sostituzione comando](using-the-replace-image-command.md) 
 [Sottocomando: set-immagine](subcommand-set-image.md)
