---
title: typeperf
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1337066f547367b381a531dbab6627ad78d280ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848452"
---
# <a name="typeperf"></a>typeperf



Il **typeperf** comando scrive i dati sulle prestazioni nella finestra di comando o in un file di log. Per arrestare **typeperf**, premere CTRL + C.

Per esempi d'uso **typeperf**, vedere [esempi](#BKMK_EXAMPLES).

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
|\<contatori [contatore [...]] >|Specifica i contatori delle prestazioni da monitorare.|

> [!NOTE]
> **\<contatore >** è il nome completo di un contatore delle prestazioni nel  *\\ \\Computer\Object (istanza) \Counter* formato, ad esempio  **\\ \\Server1\ Processor(0)\% tempo utente**.

## <a name="options"></a>Opzioni

|Opzione|Descrizione|
|---------|-----------|
|-?|Vengono visualizzate sensibile al contesto della Guida.|
|-f \<CSV&verbar;TSV&verbar;BIN&verbar;SQL>|Specifica il formato di file di output. Il valore predefinito è CSV.|
|-cf \<nomefile >|Specifica un file contenente un elenco di contatori delle prestazioni da monitorare, con un contatore per ogni riga.|
|-si < [[hh:] mm:] ss >|Specifica l'intervallo di campionamento. Il valore predefinito è 1 secondo.|
|-o \<nomefile >|Specifica il percorso per il file di output o il database SQL. Il valore predefinito è STDOUT (scritto nella finestra di comando).|
|-q [object]|Visualizzare un elenco di contatori installati (non istanze). Per elencare i contatori per un oggetto, includere il nome dell'oggetto. ESEMPIO|
|-qx [object]|Visualizzare un elenco di contatori installati con le istanze. Per elencare i contatori per un oggetto, includere il nome dell'oggetto.|
|sc - \<esempi >|Specifica il numero di campioni da raccogliere. Il valore predefinito è per raccogliere i dati fino a quando non viene premuto CTRL + C.|
|-config \<filename>|Specifica un file di impostazioni che contiene le opzioni di comando.|
|-s \<nome_computer >|Specifica un computer remoto per il monitoraggio se non viene specificato alcun computer nel percorso del contatore.|
|-y|Rispondere Sì a tutte le domande senza chiedere conferma.|

## <a name="BKMK_EXAMPLES"></a>Esempi

-   Nell'esempio seguente scrive i valori per il contatore delle prestazioni del computer locale  **\\ \\processore (totale)\% tempo processore** alla finestra di comando in un intervallo di campionamento predefinita pari a 1 secondo fino a quando non viene premuto CTRL + C.  
    ```
    typeperf "\Processor)_Total)\% Processor Time"
    ```  
-   L'esempio seguente scrive i valori per l'elenco di contatori nel file **Counters** del file delimitato da tabulazione **domain2.tsv** in un intervallo di campionamento pari a 5 secondi fino a quando non sono stati raccolti esempi di 50.  
    ```
    typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
    ```  
-   Le query di esempio seguenti installati contatori con le istanze per l'oggetto contatore **PhysicalDisk** e scrive l'elenco risultante al file **Counters**.  
    ```
    typeperf -qx PhysicalDisk -o counters.txt
    ```
