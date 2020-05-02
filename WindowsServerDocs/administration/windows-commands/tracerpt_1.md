---
title: tracerpt
description: Argomento di riferimento per tracerpt, che consente di analizzare i log di traccia eventi, i file di log generati da performance monitor e i provider di traccia eventi in tempo reale.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 79eac1db4d91bfcfe52cf71cbab683b7489d5287
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721307"
---
# <a name="tracerpt"></a>tracerpt

Il **tracerpt** comando può essere utilizzato per analizzare i registri eventi di traccia, file di log generati da Performance Monitor e provider di traccia di eventi in tempo reale. Genera file di dump, file di report e gli schemi di report.

## <a name="syntax"></a>Sintassi

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

## <a name="options"></a>Opzioni

|              Flag di opzione               |                                                                    Descrizione                                                                    |
|----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
|                   -?                   |                                                         Vengono visualizzate sensibile al contesto della Guida.                                                          |
|          -config \<nomefile>           |                                                 Caricare un file di impostazioni che contiene le opzioni di comando.                                                  |
|                   -y                   |                                                  Rispondere Sì a tutte le domande senza chiedere conferma.                                                   |
|            -f \<XML\|> HTML             |                                                                  Formato del report.                                                                   |
|         -di \<CSV\|evtx\|XML>          |                                                         Formato di dump, il valore predefinito è XML.                                                          |
|            -DF \<nomefile>             |                                            Creare una specifica di Microsoft il conteggio e alla dichiarazione di file di schema.                                            |
|            -int \<nomefile>            |                                            Scarica la struttura dell'evento interpretate nel file specificato.                                            |
|                  -rts                  |                        Timestamp non elaborato di report nell'intestazione della traccia eventi. Può essere utilizzato solo con -o, non - report o - riepilogo.                         |
|            -MDF \<nomefile>            |                                                  Specificare un file di definizione del formato dei messaggi di traccia.                                                  |
|              -valore \<TP>              |                            Specificare il percorso di ricerca file TMF. Possono essere utilizzati più percorsi, separati da un punto e virgola (;).                            |
|              -i \<valore>               | Specificare il percorso dell'immagine del provider. Il file PDB corrispondente si troverà nel Server di simboli. Più percorsi possono essere utilizzati, separati da un punto e virgola (;). |
|             -> \<valore PDB              |                             Specificare il percorso del server di simboli. Più percorsi possono essere utilizzati, separati da un punto e virgola (;).                             |
|                  -ora di Greenwich                  |                                              Convertire i timestamp payload WPP ora di Greenwich.                                               |
|              -> \<valore RL              |                                               Definire a livello di Report di sistema da 1 a 5. Il valore predefinito è 1.                                               |
|          -Riepilogo [nomefile]           |                                  Generare un file di testo di riepilogo. Nome del file se non specificato è Summary. txt.                                   |
|             -o [nomefile]              |                                      Generare un file di output di testo. Nome del file se non specificato è dumpfile. Xml.                                      |
|           -report [nomefile]           |                                  Generare un file di report di output di testo. Nome del file se non specificato è Workload.                                   |
|                  -lr                   |                        Specificare meno restrittivo. Usa impegno per gli eventi che non corrispondono allo schema di eventi.                         |
|           -esportare [nomefile]           |                                  Generare un file di esportazione dello Schema di eventi. Nome del file se non specificato è man.                                   |
|       [-l] \<valore [valore [...]] >        |                                                   Specificare il file di log traccia eventi per l'elaborazione.                                                    |
| -RT \<session_name [session_name [...]] > |                                                Specificare origini dati di sessione di traccia di eventi in tempo reale.                                                |

## <a name="examples"></a>Esempi

- Questo esempio viene creato un report basato su due registri eventi **logfile1.etl** e **logfile2.etl** e crea il file di dump **logdump.xml** in formato XML.  
  ```
  tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
  ```  
- Questo esempio viene creato un report basato sul registro eventi di **logfile.etl**, crea il file di dump **logdmp.xml** in formato XML, impegno viene utilizzato per identificare gli eventi non nello schema, produce un file di report di riepilogo **logdump.txt**, e produce il file di report **logrpt.xml**.  
  ```
  tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
  ```  
- In questo esempio utilizza i registri eventi di due **logfile1.etl** e **logfile2.etl** per produrre un file di dump e un file di report con i nomi di file predefinito.  
  ```
  tracerpt logfile1.etl logfile2.etl -o -report
  ```  
- In questo esempio utilizza il registro eventi **logfile.etl** e nel registro delle prestazioni **counterfile.blg** per generare il file di report **logrpt.xml** e il file di schema XML specifica Microsoft **schema**.  
  ```
  tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
  ```  
- Questo esempio legge la sessione di traccia di eventi in tempo reale e genera il file di dump **logfile. csv** in formato CSV.  
  ```
  tracerpt -rt NT Kernel Logger -o logfile.csv -of CSV
  ```
