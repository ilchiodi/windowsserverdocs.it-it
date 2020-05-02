---
title: Script ed esempi di DiskPart
description: Argomento di riferimento per gli script e gli esempi di DiskPart su come automatizzare le attività correlate al disco, ad esempio la creazione di volumi o la conversione di dischi in dischi dinamici.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 319c0795-11df-47c8-b203-eadb0577ee0d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 74cefe87fbbeaa1f2bacccbe4addeff952b2c432
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719479"
---
# <a name="diskpart-scripts-and-examples"></a>Script ed esempi di DiskPart

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Usare DiskPart `/s` per eseguire script che automatizzano le attività relative al disco, ad esempio la creazione di volumi o la conversione di dischi in dischi dinamici. Lo scripting di queste attività è utile se si distribuisce Windows utilizzando l'installazione automatica o lo strumento Sysprep, che non supporta la creazione di volumi diversi dal volume di avvio.  
  
-   Per creare uno script DiskPart, creare un file di testo contenente i comandi DiskPart che si desidera eseguire, con un comando per riga e senza righe vuote. È possibile avviare una riga con `rem` per impostare come commento la riga.  
  
    ad esempio, di seguito è riportato uno script che cancella un disco e quindi crea una partizione di 300 MB per ambiente ripristino Windows:  
  
    ```  
    select disk 0  
    clean  
    convert gpt  
    create partition primary size=300  
    format quick fs=ntfs label=Windows RE tools  
    assign letter=T  
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
> Quando si usa il comando **DiskPart** come parte di uno script, si consiglia di completare tutte le operazioni DiskPart insieme come parte di un singolo script DiskPart. È possibile eseguire script DiskPart consecutivi, ma è necessario consentire almeno 15 secondi tra ogni script per un arresto completo dell'esecuzione precedente prima di eseguire di nuovo il comando **DiskPart** negli script successivi. In caso contrario, gli script successivi potrebbero non riuscire. È possibile aggiungere una pausa tra script DiskPart consecutivi aggiungendo il `timeout /t 15` comando al file batch insieme agli script DiskPart.  
  
Quando viene avviato DiskPart, il nome del computer e la versione DiskPart vengono visualizzati al prompt dei comandi. Per impostazione predefinita, se DiskPart rileva un errore durante il tentativo di eseguire un'attività con script, DiskPart interrompe l'elaborazione dello script e visualizza un codice \(di errore a meno che non sia stato specificato il parametro **noerr** \)noerr. Tuttavia, DiskPart restituisce sempre errori quando rileva errori di sintassi, indipendentemente dal fatto che sia stato usato il parametro **noerr** . Il parametro **noerr** consente di eseguire attività utili, ad esempio l'uso di un singolo script per eliminare tutte le partizioni in tutti i dischi, indipendentemente dal numero totale di dischi.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi
  
- [Esempio: configurare partizioni\/del\-disco rigido basate su UEFI GPT usando Windows PE e Diskpart](https://technet.microsoft.com/library/hh825686.aspx)  
- [Esempio: configurare partizioni\/del\-disco rigido basate su MBR BIOS usando Windows PE e Diskpart](https://technet.microsoft.com/library/hh825677.aspx)  
- [Cmdlet di archiviazione in Windows PowerShell](https://technet.microsoft.com/library/hh848705.aspx)  
  

