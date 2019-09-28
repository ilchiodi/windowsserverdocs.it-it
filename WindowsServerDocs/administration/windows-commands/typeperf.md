---
title: typeperf
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 087b201c51d5aec8e6f61c7469c59307d3ed8b4d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392291"
---
# <a name="typeperf"></a>typeperf



Il comando **typeperf** scrive i dati sulle prestazioni nella finestra di comando o in un file di log. Per arrestare **typeperf**, premere CTRL + C.

Per esempi relativi all'uso di **typeperf**, vedere [esempi](#BKMK_EXAMPLES).

## <a name="syntax"></a>Sintassi

```
typeperf <counter [counter ...]> [options]
typeperf -cf <filename> [options]
typeperf -q [object] [options]
typeperf -qx [object] [options]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<counter [...]] >|Specifica i contatori delle prestazioni da monitorare.|

> [!NOTE]
> **\<counter >** è il nome completo di un contatore delle prestazioni nel formato *\\ @ no__t-4Computer\Object (istanza) \Contatore* , ad esempio **\\ @ No__t-7Server1\Processor (0) @no__t tempo utente**.

## <a name="options"></a>Opzioni

|                   Opzione                   |                                                         Descrizione                                                          |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|                     -?                     |                                               Vengono visualizzate sensibile al contesto della Guida.                                               |
| -f \<CSV @ no__t-1TSV @ no__t-2BIN @ no__t-3SQL > |                                    Specifica il formato del file di output. Il valore predefinito è CSV.                                     |
|              -CF \<filename >               |              Specifica un file contenente un elenco di contatori delle prestazioni da monitorare, con un contatore per riga.               |
|             -si < [[hh:] mm:] ss >             |                                  Specifica l'intervallo di campionamento. Il valore predefinito è un secondo.                                   |
|               -o \<filename >               |     Specifica il percorso del file di output o del database SQL. Il valore predefinito è STDOUT (scritto nella finestra di comando).      |
|                -q [oggetto]                 | Visualizza un elenco di contatori installati (nessuna istanza). Per elencare i contatori per un oggetto, includere il nome dell'oggetto. \* @ NO__T-1 @ NO__T-2EXAMPLE |
|                -QX [oggetto]                |        Visualizza un elenco di contatori installati con le istanze. Per elencare i contatori per un oggetto, includere il nome dell'oggetto.        |
|               -SC \<samples >               |             Specifica il numero di campioni da raccogliere. Per impostazione predefinita, vengono raccolti i dati fino a quando non si preme CTRL + C.              |
|            -config \<nomefile >             |                                    Specifica un file di impostazioni che contiene le opzioni di comando.                                     |
|            -s \<computer_name >             |                   Specifica un computer remoto per il monitoraggio se non è specificato alcun computer nel percorso del contatore.                    |
|                     -y                     |                                        Rispondere Sì a tutte le domande senza chiedere conferma.                                        |

## <a name="BKMK_EXAMPLES"></a>Esempi

- Nell'esempio seguente vengono scritti i valori per il contatore delle prestazioni del computer locale **\\ @ no__t-2Processor (_ Total) \% tempo processore** alla finestra di comando in un intervallo di campionamento predefinito di 1 secondo fino a quando non si preme CTRL + C.  
  ```
  typeperf "\Processor(_Total)\% Processor Time"
  ```  
- Nell'esempio seguente vengono scritti i valori per l'elenco di contatori nel file **Counters. txt** nel file delimitato da tabulazione **domain2. TSV** a un intervallo di campionamento di 5 secondi fino a quando non vengono raccolti gli esempi 50.  
  ```
  typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
  ```  
- Nell'esempio seguente vengono eseguite query sui contatori installati con istanze per l'oggetto contatore **PhysicalDisk** e viene scritto l'elenco risultante nel file **Counters. txt**.  
  ```
  typeperf -qx PhysicalDisk -o counters.txt
  ```
