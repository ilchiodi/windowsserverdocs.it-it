---
title: Bitsadmin util e GETIEPROXY
description: Windows Commands Topic for Bitsadmin util and GETIEPROXY, che recupera l'utilizzo del proxy per l'account del servizio specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0d2a8634f3b42d655632a280cb9b998111c800b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848974"
---
# <a name="bitsadmin-util-and-getieproxy"></a>Bitsadmin util e GETIEPROXY

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera l'utilizzo di proxy per l'account di servizio.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Util /GetIEProxy <Account> [/Conn <ConnectionName>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|Account|Specifica l'account del servizio cui si desidera recuperare le impostazioni di proxy. I valori possibili sono:<p>-LOCALSYSTEM<br />-SERVIZIO DI RETE<br />-LOCALSERVICE|
|ConnectionName|Facoltativo usato con il parametro **/conn** per specificare la connessione modem da usare. Se non si specifica il parametro **/conn** , BITS utilizzerà la connessione LAN. Specificare il nome della connessione modem immediatamente dopo il **/conn** parametro.|

## <a name="remarks"></a>Note

Questa opzione Mostra il valore per ogni utilizzo del proxy, non solo l'utilizzo del proxy specificato per l'account del servizio. Per informazioni dettagliate sull'impostazione dell'utilizzo del proxy per gli account del servizio, vedere l'opzione [Bitsadmin util e SETIEPROXY](bitsadmin-util-and-setieproxy.md) .

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

L'esempio seguente mostra l'utilizzo di proxy per l'account del SERVIZIO di RETE.

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
