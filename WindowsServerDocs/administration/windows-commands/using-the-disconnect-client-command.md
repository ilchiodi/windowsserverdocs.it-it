---
title: Utilizzando il comando di disconnessione Client
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b89f6c2ff6d41230afd0a2b251ad6982dfa235b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841752"
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
|/ClientId:\<Client ID>|Specifica l'ID del client deve essere disconnessa. Per visualizzare l'ID di un client, digitare **WDSUTIL /get-multicasttransmission /show:clients**.|
|[/ Server:\<nome Server >]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|[/Force]|Interrompe l'installazione completamente e non utilizza un metodo di fallback. Si noti che Wdsmcast.exe non supporta alcun meccanismo di fallback. Se non si utilizza questa opzione, il comportamento predefinito è come segue:</br>-Se si utilizza il client di servizi di distribuzione Windows, il client continua l'installazione tramite unicasting.</br>-Se non si utilizza il client di servizi di distribuzione Windows, l'installazione non riesce.</br>Importante: È consigliabile usare questa opzione con cautela perché l'installazione avrà esito negativo e il computer potrebbe trovarsi in uno stato inutilizzabile.|

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

[Chiave sintassi della riga di comando](command-line-syntax-key.md)