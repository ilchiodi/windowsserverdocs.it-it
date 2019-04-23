---
title: tracerpt
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d4d943da40793be0680d6ea6a4d9de0960f65c72
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861702"
---
# <a name="tracerpt"></a>tracerpt



Il **tracerpt** comando può essere utilizzato per analizzare i registri eventi di traccia, file di log generati da Performance Monitor e provider di traccia di eventi in tempo reale. Genera file di dump, file di report e gli schemi di report.

Per esempi di come utilizzare **tracerpt**, vedere [esempi](#BKMK_EXAMPLES).

## <a name="syntax"></a>Sintassi

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

## <a name="options"></a>Opzioni

|Flag di opzione|Descrizione|
|-----------|-----------|
|-?|Vengono visualizzate sensibile al contesto della Guida.|
|-config \<filename>|Caricare un file di impostazioni che contiene le opzioni di comando.|
|-y|Rispondere Sì a tutte le domande senza chiedere conferma.|
|-f \<XML | HTML>|Definire il formato di report.|
|-di \<CSV | EVTX | XML>|Definire il formato dump. Valore predefinito è XML.|
|-df \<nomefile >|Creare una specifica di Microsoft il conteggio e alla dichiarazione di file di schema.|
|-int \<nomefile >|Scarica la struttura dell'evento interpretate nel file specificato.|
|-rts|Timestamp non elaborato di report nell'intestazione della traccia eventi. Può essere utilizzato solo con -o, non - report o - riepilogo.|
|-tmf \<filename>|Specificare un file di definizione del formato dei messaggi di traccia.|
|-tp \<valore >|Specificare il percorso di ricerca file TMF. Possono essere utilizzati più percorsi, separati da un punto e virgola (;).|
|-i \<value>|Specificare il percorso dell'immagine del provider. Il file PDB corrispondente si troverà nel Server di simboli. Più percorsi possono essere utilizzati, separati da un punto e virgola (;).|
|-pdb \<valore >|Specificare il percorso del server di simboli. Più percorsi possono essere utilizzati, separati da un punto e virgola (;).|
|-ora di Greenwich|Convertire i timestamp payload WPP ora di Greenwich.|
|-rl \<valore >|Definire a livello di Report di sistema da 1 a 5. Valore predefinito è 1.|
|-Riepilogo [nomefile]|Generare un file di testo di riepilogo. Nome del file se non specificato è Summary. txt.|
|-o [nomefile]|Generare un file di output di testo. Nome del file se non specificato è dumpfile. Xml.|
|-report [nomefile]|Generare un file di report di output di testo. Nome del file se non specificato è Workload.|
|-lr|Specificare "meno restrittiva". Usa impegno per gli eventi che non corrispondono allo schema di eventi.|
|-esportare [nomefile]|Generare un file di esportazione dello Schema di eventi. Nome del file se non specificato è man.|
|[-l] \<value [value […]]>|Specificare il file di log traccia eventi per l'elaborazione.|
|-rt \<session_name [session_name [...]] >|Specificare origini dati di sessione di traccia di eventi in tempo reale.|

## <a name="BKMK_EXAMPLES"></a>Esempi

-   Questo esempio viene creato un report basato su due registri eventi **logfile1.etl** e **logfile2.etl** e crea il file di dump **logdump.xml** in formato XML.  
    ```
    tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
    ```  
-   Questo esempio viene creato un report basato sul registro eventi di **logfile.etl**, crea il file di dump **logdmp.xml** in formato XML, impegno viene utilizzato per identificare gli eventi non nello schema, produce un file di report di riepilogo **logdump.txt**, e produce il file di report **logrpt.xml**.  
    ```
    tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
    ```  
-   In questo esempio utilizza i registri eventi di due **logfile1.etl** e **logfile2.etl** per produrre un file di dump e un file di report con i nomi di file predefinito.  
    ```
    tracerpt logfile1.etl logfile2.etl -o -report
    ```  
-   In questo esempio utilizza il registro eventi **logfile.etl** e nel registro delle prestazioni **counterfile.blg** per generare il file di report **logrpt.xml** e il file di schema XML specifica Microsoft **schema**.  
    ```
    tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
    ```  
-   In questo esempio legge la sessione di traccia di eventi in tempo reale "Logger di Kernel NT" e genera il file di dump **fileregistro. csv** in formato CSV.  
    ```
    tracerpt -rt "NT Kernel Logger" -o logfile.csv -of CSV
    ```