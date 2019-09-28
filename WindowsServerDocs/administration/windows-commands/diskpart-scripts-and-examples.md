---
title: Script ed esempi di DiskPart
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9c40ce79664795297af4369e35cbda7422617e6e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377849"
---
# <a name="diskpart-scripts-and-examples"></a>Script ed esempi di DiskPart

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Usare DiskPart `/s` per eseguire script che automatizzano le attività disk @ no__t-1related, ad esempio la creazione di volumi o la conversione di dischi in dischi dinamici. Lo scripting di queste attività è utile se si distribuisce Windows utilizzando l'installazione automatica o lo strumento Sysprep, che non supporta la creazione di volumi diversi dal volume di avvio.  
  
-   Per creare uno script DiskPart, creare un file di testo contenente i comandi DiskPart che si desidera eseguire, con un comando per riga e senza righe vuote. È possibile avviare una riga con `rem` per impostare la riga come commento.  
  
    ad esempio, di seguito è riportato uno script che cancella un disco e quindi crea una partizione di 300 MB per ambiente ripristino Windows:  
  
    ```  
    select disk 0  
    clean  
    convert gpt  
    create partition primary size=300  
    format quick fs=ntfs label="Windows RE tools"  
    assign letter="T"  
    ```  
  
-   Per eseguire uno script DiskPart, al prompt dei comandi digitare il comando seguente, dove *scriptname* è il nome del file di testo che contiene lo script.  
  
    ```  
    diskpart /s scriptname.txt  
    ```  
  
-   Per reindirizzare l'output di scripting di DiskPart a un file, digitare il comando seguente, dove *logfile* è il nome del file di testo in cui DiskPart scrive l'output.  
  
    ```  
    diskpart /s scriptname.txt > logfile.txt  
    ```  
  
> [!IMPORTANT]  
> Quando si usa il comando **DiskPart** come parte di uno script, si consiglia di completare tutte le operazioni DiskPart insieme come parte di un singolo script DiskPart. È possibile eseguire script DiskPart consecutivi, ma è necessario consentire almeno 15 secondi tra ogni script per un arresto completo dell'esecuzione precedente prima di eseguire di nuovo il comando **DiskPart** negli script successivi. In caso contrario, gli script successivi potrebbero non riuscire. È possibile aggiungere una pausa tra script DiskPart consecutivi aggiungendo il comando `timeout /t 15` al file batch insieme agli script DiskPart.  
  
Quando viene avviato DiskPart, il nome del computer e la versione DiskPart vengono visualizzati al prompt dei comandi. Per impostazione predefinita, se DiskPart rileva un errore durante il tentativo di eseguire un'attività con script, DiskPart interrompe l'elaborazione dello script e visualizza un codice di errore \(unless è stato specificato il parametro **noerr** @ no__t-2. Tuttavia, DiskPart restituisce sempre errori quando rileva errori di sintassi, indipendentemente dal fatto che sia stato usato il parametro **noerr** . Il parametro **noerr** consente di eseguire attività utili, ad esempio l'uso di un singolo script per eliminare tutte le partizioni in tutti i dischi, indipendentemente dal numero totale di dischi.  
  
## <a name="see-also"></a>Vedere anche  
[Esempio: Configurare le partizioni del disco rigido UEFI @ no__t-0gpt @ no__t-1Basato usando Windows PE e DiskPart @ no__t-2  
[Esempio: Configurare BIOS @ no__t-0MBR @ no__t-1Basato partizioni disco rigido usando Windows PE e DiskPart @ no__t-2  
[Cmdlet di archiviazione in Windows PowerShell](https://technet.microsoft.com/library/hh848705.aspx)  
  

