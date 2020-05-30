---
title: Manage-bde TPM
description: Argomento di riferimento per il comando Manage-bde TPM, che consente di configurare la Trusted Platform Module del computer (TPM).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3aa597dbd871c64efc7e718ef70ed0c69b256ab9
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222132"
---
# <a name="manage-bde-tpm"></a>Manage-bde TPM

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura il Trusted Platform Module (TPM) del computer.

## <a name="syntax"></a>Sintassi

```
manage-bde -tpm [-turnon] [-takeownership <ownerpassword>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| attivazione: | Abilita e attiva il TPM, consentendo la password del proprietario del TPM da impostare. È inoltre possibile utilizzare **-t** come una versione abbreviata di questo comando. |
| -takeownership | Acquisisce la proprietà del TPM impostando una password del proprietario. È inoltre possibile utilizzare **-o** come una versione abbreviata di questo comando. |
| `<ownerpassword>` | Rappresenta la password del proprietario specificato per il TPM. |
| -computername | Specifica che manage-bde. exe verrà utilizzato per modificare la protezione BitLocker in un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
| `<name>` | Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer. |
| -? o /? | Visualizza una breve guida al prompt dei comandi. |
| -Help o-h | Visualizza la Guida completa al prompt dei comandi. |

### <a name="examples"></a>Esempi

Per attivare il TPM, digitare:

```
manage-bde  tpm -turnon
```

Per assumere la proprietà del TPM e impostare la password del proprietario su *0wnerP@ss* , digitare:

```
manage-bde  tpm  takeownership 0wnerP@ss
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Cmdlet di gestione TPM per Windows PowerShell](https://docs.microsoft.com/powershell/module/trustedplatformmodule/)

- [comando Manage-bde](manage-bde.md)
