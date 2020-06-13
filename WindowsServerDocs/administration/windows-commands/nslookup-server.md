---
title: nslookup server
description: Argomento di riferimento per il comando del server nslookup, che consente di modificare il server predefinito nel dominio del Domain Name System (DNS) specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1a153bb39e3c7c4114334e7fa16b0f287b8b7fe8
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721621"
---
# <a name="nslookup-server"></a>nslookup server

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il server predefinito sul dominio di Domain Name System (DNS) specificato.

Questo comando usa il server predefinito corrente per cercare le informazioni sul dominio DSN specificato. Se si desidera eseguire la ricerca delle informazioni utilizzando il server iniziale, utilizzare il comando [nslookup lserver](nslookup-lserver.md) .

## <a name="syntax"></a>Sintassi

```
server <DNSdomain>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<DNSdomain>` | Specifica il dominio DNS per il server predefinito. |
| /? | Visualizza la guida al prompt dei comandi. |
| /help | Visualizza la guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [nslookup lserver](nslookup-lserver.md)
