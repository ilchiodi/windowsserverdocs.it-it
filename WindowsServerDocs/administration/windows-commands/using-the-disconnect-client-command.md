---
title: Disconnetti-client
description: Argomento comandi di Windows per Disconnect-Client, che disconnette un client da una trasmissione multicast o uno spazio dei nomi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 876bbe6c-76ab-4de5-879b-d2066e700326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ba40a7e885cfa3e42065b939d3ddb21ead2f866
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831604"
---
# <a name="disconnect-client"></a>Disconnetti-client

Disconnette un client da una trasmissione multicast o dello spazio dei nomi. Se non si specifica **/force**, il client eseguirà il fallback a un altro metodo di trasferimento (se è supportato dal client).

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/ClientId: ID client\<>|Specifica l'ID del client deve essere disconnessa. Per visualizzare l'ID di un client, digitare **WDSUTIL /get-multicasttransmission /show:clients**.|
|[/Server:\<nome server >]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|[/Force]|Interrompe l'installazione completamente e non utilizza un metodo di fallback. Si noti che Wdsmcast.exe non supporta alcun meccanismo di fallback. Se non si utilizza questa opzione, il comportamento predefinito è come segue:</br>-Se si utilizza il client di servizi di distribuzione Windows, il client continua l'installazione tramite unicasting.</br>-Se non si utilizza il client di servizi di distribuzione Windows, l'installazione non riesce.</br>Importante: è consigliabile usare questa opzione con cautela perché l'installazione avrà esito negativo e il computer potrebbe essere lasciato in uno stato inutilizzabile.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per disconnettere un client, digitare:
```
WDSUTIL /Disconnect-Client /ClientId:1
```
Per disconnettere un client e forzare l'installazione, digitare:
```
WDSUTIL /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)