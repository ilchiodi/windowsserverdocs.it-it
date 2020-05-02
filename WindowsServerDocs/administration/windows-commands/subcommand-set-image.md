---
title: Set di sottocomandi-immagine
description: Argomento di riferimento per sottocomando set-Image, che modifica gli attributi di un'immagine.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ae03c86-7a13-4e38-9182-32e55fffd504
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e24b20093a726e7553474871ef25e6877223e21f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721706"
---
# <a name="subcommand-set-image"></a>Sottocomando: set-immagine

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica gli attributi di un'immagine.

## <a name="syntax"></a>Sintassi
per le immagini di avvio:
```
wdsutil /Set-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>] [/Name:<Name>] 
[/Description:<Description>] [/Enabled:{Yes | No}]
```
per le immagini di installazione:
```
wdsutil /Set-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:InstallmediaGroup:<Image group name>]
     [/Filename:<File name>]
     [/Name:<Name>]
     [/Description:<Description>]
     [/UserFilter:<SDDL>]
     [/Enabled:{Yes | No}]
     [/UnattendFile:<Unattend file path>]
         [/OverwriteUnattend:{Yes | No}]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
media<Image name>|Specifica il nome dell'immagine.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
MediaType: {boot &#124; install}|Specifica il tipo di immagine.|
|/ Architettura: {x86 & #124; ia64 & #124; x64}|Specifica l'architettura dell'immagine. Perché è possibile avere lo stesso nome di immagine per immagini di avvio diverse in diverse architetture, specificando l'architettura si garantisce che viene modificato l'immagine corretta.|
|[/Filename:<File name>]|Se l'immagine non può essere identificata in modo univoco in base al nome, è necessario utilizzare questa opzione per specificare il nome del file.|
|[/Name]|Specifica il nome dell'immagine.|
|/Description<Description>]|Imposta la descrizione dell'immagine.|
|[O abilitati: {Sì & #124; No}]|Abilita o disabilita l'immagine.|
|\mediaGroup:<Image group name>]|Specifica il gruppo di immagini che contiene l'immagine. Se viene specificato alcun nome di gruppo di immagini e il gruppo solo un'immagine esistente sul server, verrà utilizzato tale gruppo di immagini. Se è presente più di un gruppo di immagini nel server, è necessario utilizzare questa opzione per specificare il gruppo di immagini.|
|[/ UserFilter:<SDDL>]|Imposta il filtro utente nell'immagine. La stringa di filtro deve essere nel formato Security Descriptor Definition Language (SDDL). Si noti che, a differenza di **/Security** opzione per gruppi di immagini, questa opzione limita solo chi può visualizzare la definizione dell'immagine e non le risorse del file immagine effettiva. Per limitare l'accesso alle risorse di file e quindi accedere a tutte le immagini all'interno di un gruppo di immagini, è necessario impostare la protezione per il gruppo di immagini.|
|[/UnattendFile:<Unattend file path>]|Imposta il percorso completo al file di installazione automatica per essere associato all'immagine. Ad esempio: **D:\Files\Unattend\Img1Unattend.xml**|
|[/ OverwriteUnattend: {Sì & #124; No}]|È possibile specificare **/overwrite** per sovrascrivere il file di installazione automatica, se esiste già un file di installazione automatica associato all'immagine. Si noti che l'impostazione predefinita è **n**.|
## <a name="examples"></a>Esempi
Per impostare i valori per un'immagine di avvio, digitare uno dei seguenti:
```
wdsutil /Set-Imagmedia:WinPE boot imagemediatype:Boot /Architecture:x86 /Description:New description
wdsutil /verbose /Set-Imagmedia:WinPE boot image /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim 
/Name:New Name /Description:New Description /Enabled:Yes
```
Per impostare i valori per un'immagine di installazione, digitare uno dei seguenti:
```
wdsutil /Set-Imagmedia:Windows Vista with Officemediatype:Install /Description:New description 
wdsutil /verbose /Set-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /Name:New name /Description:New description /UserFilter:O:BAG:DUD:AI(A;ID;FA;;;SY)(A;ID;FA;;;BA)(A;ID;0x1200a9;;;AU) /Enabled:Yes /UnattendFile:\\server\share\unattend.xml /OverwriteUnattend:Yes
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando usando il comando[Add-immagine](using-the-add-image-command.md)
usando il comando[Copy-Image](using-the-copy-image-command.md)
[usando il comando Export-Image](using-the-export-image-command.md)
usando il comando[Get-](using-the-get-image-command.md)
Image usando[il comando Remove-Image](using-the-remove-image-command.md)
usando[il comando Replace-](using-the-replace-image-command.md) image
