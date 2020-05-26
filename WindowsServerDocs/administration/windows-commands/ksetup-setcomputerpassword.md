---
title: setcomputerpassword che Ksetup
description: Argomento di riferimento per il comando che Ksetup setcomputerpassword, che consente di impostare la password per il computer locale.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1ec410cd85c13cb3a925c3fc65b8c9f86fba606a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817431"
---
# <a name="ksetup-setcomputerpassword"></a>setcomputerpassword che Ksetup

Imposta la password per il computer locale. Questo comando influisce solo sull'account computer e richiede un riavvio per rendere effettiva la modifica della password.

> [!IMPORTANT]
> La password dell'account computer non viene visualizzata nel registro di sistema o come output del comando **che Ksetup** .

## <a name="syntax"></a>Sintassi

```
ksetup /setcomputerpassword <password>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<password>` | Specifica la password specificata per impostare l'account computer nel computer locale. La password può essere impostata solo usando un account con privilegi amministrativi e la password deve essere da 1 a 156 caratteri alfanumerici o caratteri speciali. |

### <a name="examples"></a>Esempi

Per modificare la password dell'account computer nel computer locale da *IPops897* a *IPop $897!*, digitare:

```
ksetup /setcomputerpassword IPop$897!
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)
