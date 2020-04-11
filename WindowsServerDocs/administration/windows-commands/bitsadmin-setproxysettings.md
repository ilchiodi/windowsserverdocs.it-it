---
title: bitsadmin setproxysettings
description: Windows Commands Topic for **BITSAdmin setproxysettings**, che imposta le impostazioni proxy per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ea92383d9bd09372d21d3c1da84db060b0a9958
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122758"
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
| lavoro | Nome visualizzato o GUID del processo. |
| usage | Imposta l'utilizzo del proxy, tra cui:<ul><li>**Preconfigurazione.** Utilizzare le impostazioni predefinite di Internet Explorer del proprietario.</li><li>**NO_PROXY.** Non usare un server proxy.</li><li>**OVERRIDE.** Usare un elenco di proxy esplicito e un elenco di bypass. Le informazioni relative all'elenco di proxy e al bypass del proxy devono essere seguite.</li><li>**Rilevamento automatico.** Rileva automaticamente le impostazioni del proxy.</li></ul> |
| list | Utilizzato quando il parametro *Usage* è impostato su override. Deve contenere un elenco delimitato da virgole di server proxy da usare. |
| Bypass | Utilizzato quando il parametro *Usage* è impostato su override. Deve contenere un elenco delimitato da spazi di nomi host o indirizzi IP, o entrambi, per i quali i trasferimenti non devono essere instradati tramite un proxy. Può trattarsi di `<local>` per fare riferimento a tutti i server alla stessa rete LAN. I valori NULL possono essere utilizzati per un elenco di bypass proxy vuoto. |

## <a name="examples"></a>Esempi

Nell'esempio seguente vengono impostate le impostazioni proxy per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /setproxysettings myDownloadJob PRECONFIG
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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)