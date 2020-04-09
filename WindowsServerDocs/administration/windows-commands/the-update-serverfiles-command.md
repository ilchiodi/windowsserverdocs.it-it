---
title: Update-ServerFiles
description: Windows Commands Topic for Update-ServerFiles, che aggiorna i file nella cartella condivisa reminst usando i file più recenti archiviati nella cartella%Windir%\System32\RemInst del server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37cbb880246cf5e5ff6a9e007dbe720de8dd1cbe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832954"
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
|[/Server:\<nome server >]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per aggiornare i file, digitare uno dei seguenti:
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)