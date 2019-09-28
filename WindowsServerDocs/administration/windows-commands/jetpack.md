---
title: jetpack
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82a2b7ef-0db5-4575-a028-8acb0bf6c7ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b011658c6a745d62707cf88404379b17b0e05eef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375317"
---
# <a name="jetpack"></a>jetpack

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

compatta un database WINS (Windows Internet Name Service) o Dynamic Host Configuration Protocol (DHCP). Si consiglia di compattare il database ogni volta che si avvicina 30 MB. 

## <a name="syntax"></a>Sintassi
```
jetpack.EXE <database name> <temp database name>
```

### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|<database name>|Specifica il file di database originale.|
|<temp database name>|Specifica il file di database temporaneo.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_Examples"></a>Esempi
Per compattare il database WINS:
```
cd %SYSTEMROOT%\SYSTEM32\WINS
NET STOP WINS
jetpack WINS.MDB TMP.MDB
NET start WINS
```
Per compattare il database DHCP:
```
cd %SYSTEMROOT%\SYSTEM32\DHCP
NET STOP DHCPSERver
jetpack DHCP.MDB TMP.MDB
NET start DHCPSERver
```
Negli esempi precedenti **tmp. mdb** è un database temporaneo usato da Jetpack. exe. **WINS** è il database WINS. **MDB** è il database DHCP.
Jetpack. exe compatta il database WINS o DHCP eseguendo le operazioni seguenti:
1.  Le copie del database le informazioni in un file di database temporaneo denominato **TMP**.
2.  Elimina il file di database originale, **WINS. mdb** o **DHCP. mdb**.
3.  Rinomina i file di database temporanei nel nome file originale.

> [!NOTE]
> Durante il processo di compattazione, jetpack. exe crea un file temporaneo con il nome specificato dal parametro del *nome del database temporaneo* . Una volta completato il processo di compattazione, viene rimosso il file temporaneo. Assicurarsi che non è un file già esistente in WINS o DHCP con lo stesso nome di quello specificato nella cartella di *nome database temporaneo* parametro.

## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
