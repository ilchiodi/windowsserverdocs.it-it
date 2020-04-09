---
title: typeperf
description: Windows Commands Topic for Typeperf, che scrive i dati sulle prestazioni nella finestra di comando o in un file di log.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac5f7def37939a472eb8f47cf65edf184a2fe2fc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832364"
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

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|contatore \<[...]] >|Specifica i contatori delle prestazioni da monitorare.|

> [!NOTE]
> **\<contatore >** è il nome completo di un contatore delle prestazioni nel formato *\\\\Computer\Object (istanza) \Contatore* , ad esempio **\\\\Server1\Processor (0)\% tempo utente**.

## <a name="options"></a>Opzioni

|                   Opzione                   |                                                         Descrizione                                                          |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|                     -?                     |                                               Vengono visualizzate sensibile al contesto della Guida.                                               |
| -f \<CSV&verbar;TSV&verbar;BIN&verbar;SQL > |                                    Specifica il formato del file di output. Il valore predefinito è CSV.                                     |
|              -CF \<filename >               |              Specifica un file contenente un elenco di contatori delle prestazioni da monitorare, con un contatore per riga.               |
|             -si < [[hh:] mm:] ss >             |                                  Specifica l'intervallo di campionamento. Il valore predefinito è un secondo.                                   |
|               -o \<nomefile >               |     Specifica il percorso del file di output o del database SQL. Il valore predefinito è STDOUT (scritto nella finestra di comando).      |
|                -q [oggetto]                 | Visualizza un elenco di contatori installati (nessuna istanza). Per elencare i contatori per un oggetto, includere il nome dell'oggetto. ESEMPIO di \*\*\* |
|                -QX [oggetto]                |        Visualizza un elenco di contatori installati con le istanze. Per elencare i contatori per un oggetto, includere il nome dell'oggetto.        |
|               -SC \<esempi >               |             Specifica il numero di campioni da raccogliere. Per impostazione predefinita, vengono raccolti i dati fino a quando non si preme CTRL + C.              |
|            -config \<filename >             |                                    Specifica un file di impostazioni che contiene le opzioni di comando.                                     |
|            -s \<computer_name >             |                   Specifica un computer remoto per il monitoraggio se non è specificato alcun computer nel percorso del contatore.                    |
|                     -y                     |                                        Rispondere Sì a tutte le domande senza chiedere conferma.                                        |

## <a name="examples"></a><a name=BKMK_EXAMPLES></a>Esempi

- Nell'esempio seguente vengono scritti i valori per il contatore delle prestazioni del computer locale **\\processore \\(_Total)\% tempo processore** alla finestra di comando in un intervallo di campionamento predefinito di 1 secondo fino a quando non si preme CTRL + C.  
  ```
  typeperf \Processor(_Total)\% Processor Time
  ```  
- Nell'esempio seguente vengono scritti i valori per l'elenco di contatori nel file **Counters. txt** nel file delimitato da tabulazione **domain2. TSV** a un intervallo di campionamento di 5 secondi fino a quando non vengono raccolti gli esempi 50.  
  ```
  typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
  ```  
- Nell'esempio seguente vengono eseguite query sui contatori installati con istanze per l'oggetto contatore **PhysicalDisk** e viene scritto l'elenco risultante nel file **Counters. txt**.  
  ```
  typeperf -qx PhysicalDisk -o counters.txt
  ```
