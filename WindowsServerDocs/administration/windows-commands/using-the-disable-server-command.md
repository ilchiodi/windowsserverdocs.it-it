---
title: disabilitare-server
description: Argomento di riferimento per Disable-server, che Disabilita tutti i servizi per un server di servizi di distribuzione Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b69fcfe0-b744-4794-bc75-2c9218c0ba66
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5df5c7e2f18cdda2aeeea22c209881077c681f03
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720966"
---
# <a name="disable-server"></a>disabilitare-server

Disabilita tutti i servizi per un server di servizi di distribuzione Windows.

## <a name="syntax"></a>Sintassi

```
WDSUTIL [Options] /Disable-Server [/Server:<Server name>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[/Server:\<nome server>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|

## <a name="examples"></a>Esempi

Per disabilitare il server, eseguire una delle seguenti:
```
WDSUTIL /Disable-Server
WDSUTIL /Verbose /Disable-Server /Server:MyWDSServer
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

