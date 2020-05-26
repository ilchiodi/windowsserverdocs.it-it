---
title: dumpstate che Ksetup
description: Argomento di riferimento per che Ksetup dumpstate commnand, che visualizza lo stato corrente delle impostazioni dell'area di autenticazione per tutte le aree di autenticazione definite nel computer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4ccb75ac143239d97b823fb7030f9a8020b4b4f6
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817741"
---
# <a name="ksetup-dumpstate"></a>dumpstate che Ksetup

Visualizza lo stato corrente delle impostazioni dell'area di autenticazione per tutte le aree di autenticazione definiti nel computer. Questo comando Visualizza lo stesso output del comando **che Ksetup** .

## <a name="syntax"></a>Sintassi

```
ksetup /dumpstate
```

### <a name="remarks"></a>Osservazioni

- L'output di questo comando include l'area di autenticazione predefinito (il dominio che il computer è un membro di) e tutte le aree di autenticazione definiti in questo computer. Di seguito è incluso per ogni area di autenticazione:

  - Tutti i centri di distribuzione chiavi (KDC) associati a questa area di autenticazione.

  - Tutti i flag dell'area di autenticazione **impostati** per l'area di autenticazione.

  - Password KDC.

- Questo comando non Visualizza il nome di dominio specificato dal rilevamento DNS o dal comando `ksetup /domain` .

- Questo comando non Visualizza la password del computer impostata tramite il comando `ksetup /setcomputerpassword` .

## <a name="examples"></a>Esempi

Per individuare le configurazioni dell'area di autenticazione Kerberos in un computer, digitare:

```
ksetup /dumpstate
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)