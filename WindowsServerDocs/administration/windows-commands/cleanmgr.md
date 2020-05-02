---
title: cleanmgr
description: Configurare lo strumento pulizia dischi (cleanmgr. exe) per pulire automaticamente determinati file.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: 49d85fe0c8ec1bbba810a502724fd7aac0c2f55d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712844"
---
# <a name="cleanmgr"></a>cleanmgr

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2, Windows Server (canale semestrale)

Cancella i file non necessari dal disco rigido del computer. È possibile usare le opzioni della riga di comando per specificare che **cleanmgr** pulisce i file temporanei, i file Internet, i file scaricati e i file Cestino. È quindi possibile pianificare l'esecuzione dell'attività in un momento specifico utilizzando lo strumento **attività pianificate** .

## <a name="syntax"></a>Sintassi

```
cleanmgr [/d <driveletter>] [/sageset:n]  [/sagerun:n] [/TUNEUP:n] [/LOWDISK] [/VERYLOWDISK]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /d`<driveletter>` | Specifica l'unità che si desidera venga pulita dal pulitura disco.<p>**Nota:** L'opzione **/d** non viene utilizzata con `/sagerun:n`. |
| /sageset: n | Consente di visualizzare la finestra di dialogo **Impostazioni pulitura disco** e di creare anche una chiave del registro di sistema per archiviare le impostazioni selezionate. Il `n` valore, archiviato nel registro di sistema, consente di specificare le attività per l'esecuzione della pulitura del disco. Il `n` valore può essere qualsiasi valore intero compreso tra 0 e 65535. |
| /sagerun: n | Esegue le attività specificate assegnate al valore n se si usa l'opzione **\sageset** . Tutte le unità del computer vengono enumerate e il profilo selezionato viene eseguito in ogni unità. |
| /TuneUp: n | Eseguire **/sageset** e **/sagerun** per lo stesso `n` . |
| /lowdisk | Eseguire con le impostazioni predefinite. |
| /verylowdisk | Eseguire con le impostazioni predefinite, nessuna richiesta da parte dell'utente. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="options"></a>Opzioni

Le opzioni per i file che è possibile specificare per la pulizia del disco con **/sageset** e **/sagerun** includono:

- **File di installazione temporanea** : si tratta di file creati da un programma di installazione che non è più in esecuzione.

- Programmi **scaricati** : i file di programma scaricati sono controlli ActiveX e programmi Java che vengono scaricati automaticamente da Internet quando si visualizzano determinate pagine. Questi file vengono archiviati temporaneamente nella cartella programmi scaricati sul disco rigido. Questa opzione include un pulsante Visualizza file in cui è possibile visualizzare i file prima che vengano rimossi dalla pulitura disco. Il pulsante apre la cartella programmi C:\Winnt\Downloaded.

- **File temporanei Internet** : la cartella file temporanei Internet contiene le pagine Web archiviate sul disco rigido per la visualizzazione rapida. Pulitura disco rimuove questa pagina lasciando le impostazioni personalizzate per le pagine Web intatte. Questa opzione include anche un pulsante Visualizza file, che consente di aprire la cartella C:\Documents and Settings\nomeutente\impostazioni Locali\temporary Internet Files\Content.IE5

- **File CHKDSK precedenti** : Quando Chkdsk controlla un disco per individuare eventuali errori, CHKDSK potrebbe salvare i frammenti di file perduti come file nella cartella radice sul disco. Questi file non sono necessari.

- **Cestino** : il Cestino contiene i file che sono stati eliminati dal computer. Questi file non vengono rimossi definitivamente fino a quando non viene svuotato il Cestino. Questa opzione include un pulsante Visualizza file per l'apertura del cestino.<p>**Nota:** Un cestino può essere visualizzato in più di un'unità, ad esempio, non solo in% SystemRoot%.

- **File temporanei: i** programmi talvolta archiviano informazioni temporanee in una cartella temporanea. Prima che un programma venga chiuso, il programma in genere elimina queste informazioni. È possibile eliminare in modo sicuro i file temporanei che non sono stati modificati nell'ultima settimana.

- I file **temporanei file offline temporanei** non in linea sono copie locali dei file di rete usati di recente. Questi file vengono automaticamente memorizzati nella cache, in modo da poterli usare dopo la disconnessione dalla rete. Il pulsante **Visualizza file** apre la cartella file offline.

- **File offline** : i file offline sono copie locali dei file di rete che si desidera siano disponibili in modalità offline, in modo da poterli usare dopo la disconnessione dalla rete. Il pulsante **Visualizza file** apre la cartella file offline.

- **Comprimi file obsoleti** : Windows può comprimere i file che non sono stati usati di recente. La compressione dei file consente di risparmiare spazio su disco, ma è comunque possibile utilizzare i file. Nessun file viene eliminato. Poiché i file vengono compressi a frequenze diverse, la quantità visualizzata di spazio su disco che si otterrà sarà approssimativa. Un pulsante Opzioni consente di specificare il numero di giorni di attesa prima che la pulizia del disco Commuti un file inutilizzato.

- **File di catalogo per l'indicizzatore di contenuto** : il servizio di indicizzazione velocizza e migliora le ricerche di file mantenendo un indice dei file presenti sul disco. Questi file di catalogo rimangono da un'operazione di indicizzazione precedente e possono essere eliminati in modo sicuro.<p>**Nota:** Il file di catalogo può essere visualizzato in più di un'unità, ad esempio, `%SystemRoot%`non solo in.

>[!NOTE]
> Se si specifica la pulizia dell'unità che contiene l'installazione di Windows, tutte queste opzioni sono disponibili nella scheda **Pulitura disco** . Se si specifica un'altra unità, nella scheda **Pulitura disco** sono disponibili solo i file del cestino e i file di catalogo per le opzioni relative agli indici di contenuto.

## <a name="examples"></a>Esempi

Per eseguire l'app pulitura disco in modo che sia possibile usare la finestra di dialogo per specificare le opzioni da usare in un secondo momento, salvando le impostazioni nel set **1**, digitare quanto segue:

```
cleanmgr /sageset:1
```

Per eseguire Pulitura disco e includere le opzioni specificate con il comando cleanmgr/sageset: 1, digitare:

```
cleanmgr /sagerun:1
```

Per eseguire `cleanmgr /sageset:1` e `cleanmgr /sagerun:1` insieme, digitare:

```
cleanmgr /tuneup:1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Liberare spazio sull'unità in Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
