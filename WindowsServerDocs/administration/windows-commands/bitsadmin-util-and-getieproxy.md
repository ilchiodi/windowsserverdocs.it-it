---
title: GETIEPROXY e bitsadmin util
description: Argomento i comandi di Windows per **util bitsadmin e getieproxy** -recupera l'utilizzo di proxy per l'account di servizio.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: de4f86340b1163c4d8e3286d9c86c9df794a21c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876872"
---
# <a name="bitsadmin-util-and-getieproxy"></a>GETIEPROXY e bitsadmin util

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
|ConnectionName|Facoltativo utilizzato con il **/Conn** parametro per specificare la connessione modem da utilizzare. Se non si specifica la **/Conn** parametro, BITS utilizza la connessione LAN. Specificare il nome della connessione modem immediatamente dopo il **/conn** parametro.|

## <a name="remarks"></a>Note

Questa opzione Visualizza il valore per ogni utilizzo di proxy, non solo l'utilizzo del proxy specificate per l'account del servizio. Per informazioni dettagliate su come impostare l'utilizzo di proxy per gli account del servizio, vedere la [util bitsadmin e setieproxy](bitsadmin-util-and-setieproxy.md) passare.

## <a name="BKMK_examples"></a>Esempi

L'esempio seguente mostra l'utilizzo di proxy per l'account del SERVIZIO di RETE.

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
