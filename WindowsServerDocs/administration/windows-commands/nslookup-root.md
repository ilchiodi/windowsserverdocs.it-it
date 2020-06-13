---
title: nslookup root
description: Argomento di riferimento per il comando nslookup root, che consente di modificare il server predefinito nel server per la radice dello spazio dei nomi di dominio Domain Name System (DNS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f1f2bbe3b71660d079a0b7c87f5be487e0ff437
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721654"
---
# <a name="nslookup-root"></a>nslookup root

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il server predefinito sul server per la radice dello spazio dei nomi di dominio di Domain Name System (DNS). Attualmente, viene usato il server dei nomi ns.nic.ddn.mil. È possibile modificare il nome del server radice utilizzando il comando [nslookup Imposta radice](nslookup-set-root.md) .

> [!NOTE]
> Questo comando è uguale a `lserver ns.nic.ddn.mil` .

## <a name="syntax"></a>Sintassi

```
root
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /? | Visualizza la guida al prompt dei comandi. |
| /help | Visualizza la guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [nslookup set root](nslookup-set-root.md)
