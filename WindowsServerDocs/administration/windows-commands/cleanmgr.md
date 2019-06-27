---
title: Cleanmgr
description: Informazioni su come usare le opzioni della riga di comando per configurare lo strumento di pulitura disco (Cleanmgr.exe) per pulire automaticamente determinati file.
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: fd722dda8476891860773126c84ed10f125715f0
ms.sourcegitcommit: 545dcfc23a81943e129565d0ad188263092d85f6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/27/2019
ms.locfileid: "67407888"
---
# <a name="cleanmgr"></a>Cleanmgr

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2, Windows Server (canale semestrale)

Cancella i file non necessari dal disco rigido del computer. È possibile usare le opzioni della riga di comando per specificare che Cleanmgr pulisce i file Temp, i file Internet, i file scaricati e file della cartella bin Cestino. È quindi possibile pianificare l'attività da eseguire in un momento specifico tramite lo strumento attività pianificate.

Per esempi di utilizzo di questo comando, vedere [Esempi](#examples).

## <a name="syntax"></a>Sintassi

```
cleanmgr [/d <driveletter>] [/sageset:n]  [/sagerun:n] [/TUNEUP:n] [/LOWDISK] [/VERYLOWDISK]
```

## <a name="parameters"></a>Parametri

|      Parametro      |    Descrizione     |
| ------------------- | ------------------ |
|  /d \<driveletter>          | Specifica l'unità in cui si desidera eseguire Pulitura disco.<br>**Nota:** L'opzione /d non viene usata con /sagerun:n. |
| /sageset:n | Consente di visualizzare la finestra di dialogo Impostazioni di pulitura del disco e crea anche una chiave del Registro di sistema per archiviare le impostazioni selezionate. Il `n` valore, che viene archiviata nel Registro di sistema, che consente di specificare le attività per eseguire la pulitura disco. Il `n` valore può essere qualsiasi valore intero compreso tra 0 e 65535. Per ottenere tutte le opzioni disponibili quando si usa l'opzione /sageset, specificare l'unità in cui è installato Windows.  |
|  /sagerun:n  |  Esegue le attività specificate che vengano assegnate al valore di n Se si seleziona l'ozione \sageset. Vengono enumerate tutte le unità nel computer e il profilo selezionato viene eseguito su ogni unità.           |
| /TUNEUP:n    | Eseguire /sageset e /sagerun per lo stesso `n` . |
| /LOWDISK     | Eseguire con le impostazioni predefinite. |
| /VERYLOWDISK | Eseguire con le impostazioni predefinite, nessun utente viene richiesto. |
| /?           | Visualizzare la Guida. |

## <a name="options"></a>Opzioni

Le opzioni per i file che è possibile specificare per la pulizia del disco tramite /sageset e /sagerun includono:

- **File di installazione temporanea** -questi sono i file creati dal programma di installazione non è più in esecuzione.

- **I file di programma scaricati** : file di programma scaricati sono i controlli ActiveX e Java programmi che vengono scaricati automaticamente da Internet quando si visualizzano alcune pagine. Questi file vengono temporaneamente archiviati nella cartella programmi scaricati sul disco rigido. Questa opzione include un pulsante di visualizzare i file in modo che è possibile visualizzare i file prima che questi vengono rimossi dalla Pulitura disco. Il pulsante viene aperta la cartella C:\Winnt\Downloaded Program Files.

- **File Internet temporanei** -cartella di file temporanei Internet contiene le pagine Web che vengono archiviate nel disco rigido per la visualizzazione rapida. Pulitura disco rimuove queste pagine, ma lascia invariate le impostazioni personalizzate per le pagine Web. Questa opzione include anche un pulsante, i file di visualizzazione che consente di aprire la cartella C:\Documents and Settings\Username\Local locali\File Internet Files\Content.IE5. 

- **I file obsoleti Chkdsk** -Chkdsk quando si verifica un disco per gli errori, potrebbe salvare frammenti persi i file come file nella cartella radice sul disco. Questi file non sono necessari.

- **Cestino** -Cestino contiene i file che sono stati eliminati dal computer. Questi file non vengono rimossi in modo permanente fino a quando non si svuota il Cestino. Questa opzione include un pulsante di visualizzare i file che si apre il Cestino. **Nota:** Un Cestino possono essere in più unità, ad esempio, non solo in % SystemRoot %.

- **I file temporanei** -programmi talvolta archiviano le informazioni temporanee in una cartella temporanea. Prima della chiusura di un programma, vengono automaticamente eliminati in genere queste informazioni. È possibile eliminare in modo sicuro i file temporanei che non sono stati modificati nell'ultima settimana.

- **File temporanei** -file temporanei sono copie locali dei file di rete usati di recente. Questi file vengono automaticamente memorizzate nella cache in modo che sia possibile usarle dopo la disconnessione dalla rete. Un pulsante Visualizza file consente di aprire la cartella di file Offline.

- **I file offline** -file non in linea sono copie locali dei file di rete che si desidera avere disponibile offline in modo che sia possibile usarle dopo la disconnessione dalla rete in modo specifico. Un pulsante Visualizza file consente di aprire la cartella di file Offline.

- **Comprimere i file obsoleti** -Windows possibile comprimere i file non usati di recente. La compressione dei file di risparmiare spazio su disco, ma è comunque possibile usare i file. Nessun file viene eliminato. Poiché i file vengono compressi con frequenze diverse, la quantità di spazio su disco che si potrà usufruire è approssimativa. Un pulsante di opzioni consente di specificare il numero di giorni di attesa prima di pulitura disco comprime un file non usato.

- **File di catalogo per l'indicizzatore contenuto** -il servizio di indicizzazione velocizza e migliora la ricerca di file tramite la gestione di un indice dei file presenti sul disco. Questi file di catalogo rimangono da un'operazione di indicizzazione precedente e possono essere eliminati in modo sicuro. **Nota:** File di catalogo apparire in più unità, ad esempio, non solo in % SystemRoot %.

**Nota** se si specifica pulizia dell'unità che contiene l'installazione di Windows, tutte queste opzioni sono disponibili nella scheda Pulitura disco. Se si specifica qualsiasi altra unità, solo il Cestino e i file di catalogo per le opzioni di indice di contenuto sono disponibili nella scheda Pulitura disco. 

## <a name="examples"></a>Esempi

Per eseguire l'app Pulitura disco in modo che è possibile usare la finestra di dialogo per specificare le opzioni per l'uso in un secondo momento, il salvataggio delle impostazioni per il set **1**, digitare quanto segue:

```
cleanmgr /sageset:1
```

Per eseguire Pulitura disco e includere le opzioni che è stato specificato con il comando /sageset:1 cleanmgr, digitare:

```
cleanmgr /sagerun:1
```

Per eseguire ```cleanmgr /sageset:1``` e ```cleanmgr /sagerun:1``` insieme, digitare:

```
cleanmgr /tuneup:1
```

## <a name="additional-references"></a>Altri riferimenti

[Liberare spazio nell'unità in Windows 10](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)
