---
title: Utilizzando il comando di disconnessione Client
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 876bbe6c-76ab-4de5-879b-d2066e700326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbb96d64b47ec72ff0710bfb3684257c1bda2d04
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363467"
---
# <a name="using-the-disconnect-client-command"></a>Utilizzando il comando di disconnessione Client



Disconnette un client da una trasmissione multicast o dello spazio dei nomi. Se non si specifica **/force**, il client eseguirà il fallback a un altro metodo di trasferimento (se è supportato dal client).

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/ClientId: ID \<Client >|Specifica l'ID del client deve essere disconnessa. Per visualizzare l'ID di un client, digitare **WDSUTIL /get-multicasttransmission /show:clients**.|
|[/Server: nome \<Server >]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|[/Force]|Interrompe l'installazione completamente e non utilizza un metodo di fallback. Si noti che Wdsmcast.exe non supporta alcun meccanismo di fallback. Se non si utilizza questa opzione, il comportamento predefinito è come segue:</br>-Se si utilizza il client di servizi di distribuzione Windows, il client continua l'installazione tramite unicasting.</br>-Se non si utilizza il client di servizi di distribuzione Windows, l'installazione non riesce.</br>Importante: Usare questa opzione con cautela perché l'installazione avrà esito negativo e il computer potrebbe essere lasciato in uno stato inutilizzabile.|

## <a name="BKMK_examples"></a>Esempi

Per disconnettere un client, digitare:
```
WDSUTIL /Disconnect-Client /ClientId:1
```
Per disconnettere un client e forzare l'installazione, digitare:
```
WDSUTIL /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)