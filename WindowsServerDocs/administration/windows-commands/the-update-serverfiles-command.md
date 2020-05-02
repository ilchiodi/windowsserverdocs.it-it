---
title: Update-ServerFiles
description: Argomento di riferimento per Update-ServerFiles, che aggiorna i file nella cartella condivisa reminst usando i file più recenti archiviati nella cartella%Windir%\System32\RemInst del server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0005d8e198300c4aad9fdfc772957b460d6fee74
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721378"
---
# <a name="update-serverfiles"></a>Update-ServerFiles

Aggiorna i file nella cartella condivisa REMINST utilizzando i file più recenti che sono archiviati nella cartella %Windir%\System32\RemInst del server. Per garantire la validità dell'installazione di servizi di distribuzione Windows, è necessario eseguire questo comando dopo ogni aggiornamento del server, installazione del service pack o aggiornamenti ai file di servizi di distribuzione Windows.

## <a name="syntax"></a>Sintassi

```
WDSUTIL [Options] /Update-ServerFiles [/Server:<Server name>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[/Server:\<nome server>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|

## <a name="examples"></a>Esempi

Per aggiornare i file, digitare uno dei seguenti:
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)