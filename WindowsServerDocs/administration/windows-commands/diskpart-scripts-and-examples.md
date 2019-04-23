---
title: Esempi e script di Diskpart
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 319c0795-11df-47c8-b203-eadb0577ee0d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8bd0dfab98ca705cf64766d66285c69bd3d3129
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862792"
---
# <a name="diskpart-scripts-and-examples"></a>Esempi e script di Diskpart

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Utilizzare Diskpart `/s` per eseguire gli script che automatizzano disco\-relative attività, quali la creazione di volumi o la conversione di dischi a dischi dinamici. Queste attività di scripting è utile se si distribuisce Windows mediante l'installazione automatica o lo strumento Sysprep, che non supportano la creazione di volumi diverso dal volume di avvio.  
  
-   Per creare uno script di Diskpart, creare un file di testo che contiene i comandi che si desidera eseguire, con un unico comando per ogni riga e le righe non vuote. È possibile avviare una riga con `rem` per scrivere un commento di riga.  
  
    ad esempio, qui s uno script che cancella un disco e quindi crea una partizione di 300 MB di ambiente ripristino Windows:  
  
    ```  
    select disk 0  
    clean  
    convert gpt  
    create partition primary size=300  
    format quick fs=ntfs label="Windows RE tools"  
    assign letter="T"  
    ```  
  
-   Per eseguire uno script di DiskPart, al prompt dei comandi, digitare il comando seguente, dove *NomeScript* è il nome del file di testo che contiene lo script.  
  
    ```  
    diskpart /s scriptname.txt  
    ```  
  
-   Per reindirizzare l'output di uno script di DiskPart per un file, digitare il comando seguente, dove *logfile* è il nome del file di testo in cui DiskPart scrive l'output.  
  
    ```  
    diskpart /s scriptname.txt > logfile.txt  
    ```  
  
> [!IMPORTANT]  
> Quando si usa la **DiskPart** comando come parte di uno script, si consiglia di completare tutte le operazioni di DiskPart insieme come parte di un unico script. È possibile eseguire gli script di DiskPart consecutivi, ma è necessario consentire almeno 15 secondi tra ogni script per un arresto completo dell'esecuzione precedente prima di eseguire la **DiskPart** comando nuovamente negli script successivi. In caso contrario, gli script successivi potrebbero non riuscire. È possibile aggiungere una pausa tra gli script di DiskPart consecutivi aggiungendo il `timeout /t 15` comando per il file batch che contiene gli script.  
  
All'avvio DiskPart, DiskPart versione e computer di nome visualizzazione al prompt dei comandi. Per impostazione predefinita, se DiskPart rileva un errore durante il tentativo di eseguire un'attività tramite script, DiskPart interrompe lo script di elaborazione e viene visualizzato un codice di errore \(a meno che non è stato specificato il **noerr** parametro\). Tuttavia, DiskPart restituisce sempre gli errori quando si verificano errori di sintassi, indipendentemente dal fatto se è stata usata la **noerr** parametro. Il **noerr** parametro consente di eseguire operazioni utili, ad esempio usando un unico script per eliminare tutte le partizioni in tutti i dischi indipendentemente dal numero totale di dischi.  
  
## <a name="see-also"></a>Vedere anche  
[Esempio: Configurare il firmware UEFI\/gpt\-basati su disco rigido unità partizioni usando Windows PE e DiskPart](https://technet.microsoft.com/library/hh825686.aspx)  
[Esempio: Configurare BIOS\/MBR\-basato su partizioni del disco rigido usando Windows PE e DiskPart](https://technet.microsoft.com/library/hh825677.aspx)  
[Cmdlet di archiviazione in Windows PowerShell](https://technet.microsoft.com/library/hh848705.aspx)  
  

