---
title: typeperf
description: Argomento di riferimento per Typeperf, che scrive i dati sulle prestazioni nella finestra di comando o in un file di log.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a996abc1417fdb6aa50370a942433716d8df2cbe
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721215"
---
# <a name="typeperf"></a>typeperf

Il comando **typeperf** scrive i dati sulle prestazioni nella finestra di comando o in un file di log. Per arrestare **typeperf**, premere CTRL + C.

## <a name="syntax"></a>Sintassi

```
typeperf <counter [counter ...]> [options]
typeperf -cf <filename> [options]
typeperf -q [object] [options]
typeperf -qx [object] [options]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Counter [contatore [...]] >|Specifica i contatori delle prestazioni da monitorare.|

> [!NOTE]
> Counter>è il nome completo di un contatore delle prestazioni nel * \\ \\formato Computer\Object (instance) \Contatore* , ad esempio ** \\ \\Server1\Processor (0\% ) User Time**. ** \<**

## <a name="options"></a>Opzioni

|                   Opzione                   |                                                         Descrizione                                                          |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|                     -?                     |                                               Vengono visualizzate sensibile al contesto della Guida.                                               |
| -file \<con&verbar;estensione&verbar;CSV&verbar;del file con estensione> SQL |                                    Specifica il formato del file di output. Il valore predefinito è CSV.                                     |
|              -CF \<nomefile>               |              Specifica un file contenente un elenco di contatori delle prestazioni da monitorare, con un contatore per riga.               |
|             -si < [[hh:] mm:] ss >             |                                  Specifica l'intervallo di campionamento. Il valore predefinito è un secondo.                                   |
|               -o \<nomefile>               |     Specifica il percorso del file di output o del database SQL. Il valore predefinito è STDOUT (scritto nella finestra di comando).      |
|                -q [oggetto]                 | Visualizza un elenco di contatori installati (nessuna istanza). Per elencare i contatori per un oggetto, includere il nome dell'oggetto. \*\*\*ESEMPIO |
|                -QX [oggetto]                |        Visualizza un elenco di contatori installati con le istanze. Per elencare i contatori per un oggetto, includere il nome dell'oggetto.        |
|               -SC \<samples>               |             Specifica il numero di campioni da raccogliere. Per impostazione predefinita, vengono raccolti i dati fino a quando non si preme CTRL + C.              |
|            -config \<nomefile>             |                                    Specifica un file di impostazioni che contiene le opzioni di comando.                                     |
|            -s \<computer_name>             |                   Specifica un computer remoto per il monitoraggio se non è specificato alcun computer nel percorso del contatore.                    |
|                     -y                     |                                        Rispondere Sì a tutte le domande senza chiedere conferma.                                        |

## <a name="examples"></a>Esempi

- Per scrivere i valori per il processore del contatore ** \\ \\\% ** delle prestazioni del computer locale (_Total) nella finestra di comando in un intervallo di campionamento predefinito di 1 secondo fino a quando non viene premuto CTRL + C.  
  ```
  typeperf \Processor(_Total)\% Processor Time
  ```  
- Per scrivere i valori per l'elenco dei contatori nel file **Counters. txt** nel file delimitato da tabulazione **domain2. TSV** in un intervallo di campionamento di 5 secondi fino a quando non vengono raccolti i campioni 50.  
  ```
  typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
  ```  
- Per eseguire una query sui contatori installati con istanze per l'oggetto contatore **PhysicalDisk** e scrive l'elenco risultante nel file Counters **. txt**.  
  ```
  typeperf -qx PhysicalDisk -o counters.txt
  ```
