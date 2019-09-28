---
title: Bitsadmin util e GETIEPROXY
description: "Windows Commands Topic for **Bitsadmin util and GETIEPROXY** : Recupera l'utilizzo del proxy per l'account del servizio specificato."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6936e088ddcf467b5a8f931bc8217ba9da4662c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380266"
---
# <a name="bitsadmin-util-and-getieproxy"></a>Bitsadmin util e GETIEPROXY

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera l'utilizzo di proxy per l'account di servizio.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Util /GetIEProxy <Account> [/Conn <ConnectionName>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|Account|Specifica l'account del servizio cui si desidera recuperare le impostazioni di proxy. I valori possibili sono:<br /><br />-LOCALSYSTEM<br />-SERVIZIO DI RETE<br />-LOCALSERVICE|
|ConnectionName|Facoltativo usato con il parametro **/conn** per specificare la connessione modem da usare. Se non si specifica il parametro **/conn** , BITS utilizzerà la connessione LAN. Specificare il nome della connessione modem immediatamente dopo il **/conn** parametro.|

## <a name="remarks"></a>Note

Questa opzione Mostra il valore per ogni utilizzo del proxy, non solo l'utilizzo del proxy specificato per l'account del servizio. Per informazioni dettagliate sull'impostazione dell'utilizzo del proxy per gli account del servizio, vedere l'opzione [Bitsadmin util e SETIEPROXY](bitsadmin-util-and-setieproxy.md) .

## <a name="BKMK_examples"></a>Esempi

L'esempio seguente mostra l'utilizzo di proxy per l'account del SERVIZIO di RETE.

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
