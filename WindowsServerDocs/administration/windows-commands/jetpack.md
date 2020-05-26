---
title: jetpack
description: Argomento di riferimento per il comando Jetpack, che consente di compattare un database WINS (Windows Internet Name Service) o Dynamic Host Configuration Protocol (DHCP).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82a2b7ef-0db5-4575-a028-8acb0bf6c7ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d77f9c964f5820fc7a44b803bb765e94cb35637
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818251"
---
# <a name="jetpack"></a>jetpack

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Compatta un database di Windows Internet Name Service (WINS) o Dynamic Host Configuration Protocol (DHCP). Si consiglia di compattare il database WINS ogni volta che si avvicina a 30 MB.

Jetpack. exe compatta il database per:

1. Copia delle informazioni del database in un file di database temporaneo.

2. Eliminazione del file di database originale, ovvero WINS o DHCP.

3. Rinomina i file di database temporaneo per il nome del file originale.

## <a name="syntax"></a>Sintassi

```
jetpack.exe <database_name> <temp_database_name>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| `<database_name>` | Specifica il nome del file di database originale. |
| `<temp_database_name>` | Specifica il nome del file di database temporaneo che verrà creato da Jetpack. exe.<p>Nota: questo file temporaneo viene rimosso al termine del processo di compattazione. Per il corretto funzionamento di questo comando, è necessario assicurarsi che il nome del file temporaneo sia univoco e che non esista già un file con lo stesso nome. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="examples"></a>Esempi

Per compattare il database WINS, in cui **tmp. mdb** è un database temporaneo e **WINS. mdb** è il database WINS, digitare:

```
cd %SYSTEMROOT%\SYSTEM32\WINS
NET STOP WINS
jetpack Wins.mdb Tmp.mdb
NET start WINS
```

Per compattare il database DHCP, dove **tmp. mdb** è un database temporaneo e **DHCP. mdb** è il database DHCP, digitare:

```
cd %SYSTEMROOT%\SYSTEM32\DHCP
NET STOP DHCPSERVER
jetpack Dhcp.mdb Tmp.mdb
NET start DHCPSERVER
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
