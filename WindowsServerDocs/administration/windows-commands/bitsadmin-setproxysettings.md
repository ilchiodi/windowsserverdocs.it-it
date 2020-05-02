---
title: bitsadmin setproxysettings
description: Argomento di riferimento per il comando Bitsadmin setproxysettings, che consente di impostare le impostazioni proxy per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7f7c54b3081c85756735d921fb70f726ba60d833
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720487"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings

Imposta le impostazioni proxy per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setproxysettings <job> <usage> [list] [bypass]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| utilizzo | Imposta l'utilizzo del proxy, tra cui:<ul><li>**Preconfigurazione.** Utilizzare le impostazioni predefinite di Internet Explorer del proprietario.</li><li>**NO_PROXY.** Non usare un server proxy.</li><li>**OVERRIDE.** Usare un elenco di proxy esplicito e un elenco di bypass. Le informazioni relative all'elenco di proxy e al bypass del proxy devono essere seguite.</li><li>**Rilevamento automatico.** Rileva automaticamente le impostazioni del proxy.</li></ul> |
| list | Utilizzato quando il parametro *Usage* è impostato su override. Deve contenere un elenco delimitato da virgole di server proxy da usare. |
| ignorare | Utilizzato quando il parametro *Usage* è impostato su override. Deve contenere un elenco delimitato da spazi di nomi host o indirizzi IP, o entrambi, per i quali i trasferimenti non devono essere instradati tramite un proxy. Può trattarsi di `<local>` per fare riferimento a tutti i server alla stessa rete LAN. I valori NULL possono essere utilizzati per un elenco di bypass proxy vuoto. |

## <a name="examples"></a>Esempi

Per impostare le impostazioni proxy usando le varie opzioni di utilizzo per il processo denominato *myDownloadJob*:

```
bitsadmin /setproxysettings myDownloadJob PRECONFIG
```

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
```
```
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80
```

```
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
