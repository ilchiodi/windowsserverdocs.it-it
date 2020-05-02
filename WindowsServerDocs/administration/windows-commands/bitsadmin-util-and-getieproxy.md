---
title: bitsadmin util e getieproxy
description: Argomento di riferimento per il comando Bitsadmin util e GETIEPROXY, che consente di recuperare l'utilizzo del proxy per l'account del servizio specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 576f308bc7fb9a4e448638d06621f95eebef0cd0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707667"
---
# <a name="bitsadmin-util-and-getieproxy"></a>bitsadmin util e getieproxy

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera l'utilizzo di proxy per l'account di servizio. Questo comando Visualizza il valore per ogni utilizzo di proxy, non solo l'utilizzo del proxy specificate per l'account del servizio. Per informazioni dettagliate sull'impostazione dell'utilizzo del proxy per account del servizio specifici, vedere il comando [Bitsadmin util e SETIEPROXY](bitsadmin-util-and-setieproxy.md) .

## <a name="syntax"></a>Sintassi

```
bitsadmin /util /getieproxy <account> [/conn <connectionname>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ---------- |
| account | Specifica l'account del servizio cui si desidera recuperare le impostazioni di proxy. I valori possibili sono:<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>LocalService.</li></ul> |
| ConnectionName | Facoltativa. Usato con il parametro **/conn** per specificare quale connessione modem usare. Se non si specifica il parametro **/conn** , BITS usa la connessione LAN. |

## <a name="examples"></a>Esempi

Per visualizzare l'utilizzo del proxy per l'account servizio di rete:

```
bitsadmin /util /getieproxy NETWORKSERVICE
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando util Bitsadmin](bitsadmin-util.md)

- [comando Bitsadmin](bitsadmin.md)
