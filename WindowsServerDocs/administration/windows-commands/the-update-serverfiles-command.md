---
title: Il comando Update-ServerFiles
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 93eeb0deaa527921db35f4ab955d2ccc46b57d7a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385854"
---
# <a name="the-update-serverfiles-command"></a>Il comando Update-ServerFiles



Aggiorna i file nella cartella condivisa REMINST utilizzando i file più recenti che sono archiviati nella cartella %Windir%\System32\RemInst del server. Per garantire la validità dell'installazione di servizi di distribuzione Windows, è necessario eseguire questo comando dopo ogni aggiornamento del server, installazione del service pack o aggiornamenti ai file di servizi di distribuzione Windows.

## <a name="syntax"></a>Sintassi

```
WDSUTIL [Options] /Update-ServerFiles [/Server:<Server name>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[/Server: nome \<Server >]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|

## <a name="BKMK_examples"></a>Esempi

Per aggiornare i file, digitare uno dei seguenti:
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)