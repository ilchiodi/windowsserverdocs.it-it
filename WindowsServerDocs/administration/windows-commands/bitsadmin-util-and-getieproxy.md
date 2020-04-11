---
title: Bitsadmin util e GETIEPROXY
description: Windows Commands Topic for **Bitsadmin util and GETIEPROXY**, che recupera l'utilizzo del proxy per l'account del servizio specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 22b24c4f9c0941c88c70b488a82de47c7901bd8b
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122477"
---
# <a name="bitsadmin-util-and-getieproxy"></a>Bitsadmin util e GETIEPROXY

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera l'utilizzo di proxy per l'account di servizio. Questo comando Visualizza il valore per ogni utilizzo di proxy, non solo l'utilizzo del proxy specificate per l'account del servizio. Per informazioni dettagliate sull'impostazione dell'utilizzo del proxy per account del servizio specifici, vedere il comando [Bitsadmin util e SETIEPROXY](bitsadmin-util-and-setieproxy.md) .

## <a name="syntax"></a>Sintassi

```
bitsadmin /util /getieproxy <account> [/conn <connectionname>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ---------- |
| autorizzato | Specifica l'account del servizio cui si desidera recuperare le impostazioni di proxy. I valori possibili sono:<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>LocalService.</li></ul> |
| ConnectionName | Facoltativa. Usato con il parametro **/conn** per specificare quale connessione modem usare. Se non si specifica il parametro **/conn** , BITS usa la connessione LAN. |

## <a name="examples"></a>Esempi

L'esempio seguente mostra l'utilizzo di proxy per l'account del SERVIZIO di RETE.

```
C:\>bitsadmin /util /getieproxy NETWORKSERVICE
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
