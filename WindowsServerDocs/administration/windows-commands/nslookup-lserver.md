---
title: nslookup lserver
description: Argomento di riferimento per il comando nslookup lserver, che consente di modificare il server iniziale nel dominio Domain Name System (DNS) specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 868142f251d62ebc3efd7913aded8e22aa077bd3
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721592"
---
# <a name="nslookup-lserver"></a>nslookup lserver

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il server iniziale sul dominio di Domain Name System (DNS) specificato.

Questo comando usa il server iniziale per cercare le informazioni sul dominio DSN specificato. Se si desidera eseguire la ricerca delle informazioni utilizzando il server predefinito corrente, utilizzare il comando [nslookup server](nslookup-server.md) .

## <a name="syntax"></a>Sintassi

```
lserver <DNSdomain>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<DNSdomain>` | Specifica il dominio DNS per il server iniziale. |
| /? | Visualizza la guida al prompt dei comandi. |
| /help | Visualizza la guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [nslookup server](nslookup-server.md)
