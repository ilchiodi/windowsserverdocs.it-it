---
title: chkdsk
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 62912a3c-d2cc-4ef6-9679-43709a286035
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: ed9693749cb07c76c3c845844f3556e07a39f009
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811310"
---
# <a name="chkdsk"></a>chkdsk

Controlla il file system e i metadati del file system di un volume per gli errori logici e fisici. Se utilizzata senza parametri, **chkdsk** Visualizza solo lo stato del volume e corregge gli errori. Se usato con il **/f**, **/r**, **/x**, oppure **/b** parametri, risolvere gli errori nel volume.

> [!IMPORTANT]
> Appartenenza al gruppo locale **amministratori** o gruppo equivalente, è il requisito minimo necessario per eseguire **chkdsk**. Per aprire una finestra del prompt dei comandi come amministratore, fare doppio clic su **prompt dei comandi** nel menu Start e quindi fare clic su **Esegui come amministratore**.

> [!IMPORTANT]
> Interrompere **chkdsk** non è consigliata. Tuttavia, l'annullamento o interruzione **chkdsk** non lasciare il volume più danneggiato che si trovava prima **chkdsk** è stata eseguita. Eseguire nuovamente **chkdsk** verifica e corregge eventuali danneggiamenti rimanenti nel volume.

> [!IMPORTANT]
> **Nota:** CHKDSK è utilizzabile solo per i dischi locali. Il comando non può essere usato con una lettera di unità locale che è stata reindirizzata attraverso la rete.

Per esempi di utilizzo di questo comando, vedere [Esempi](#examples).

## <a name="syntax"></a>Sintassi

```
chkdsk [<Volume>[[<Path>]<FileName>]] [/f] [/v] [/r] [/x] [/i] [/c] [/l[:<Size>]] [/b]  
```

## <a name="parameters"></a>Parametri

|      Parametro      |                                                                                                                      Descrizione                                                                                                                       |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      \<Volume >      |                                                                                     Specifica la lettera di unità (seguita da due punti), punto di montaggio o il nome del volume.                                                                                     |
| [\<Path>]<FileName> | Utilizzo con tabella di allocazione file (FAT) e solo FAT32. Specifica il percorso e nome di un file o un set di file che si desidera **chkdsk** per controllare la frammentazione. È possibile utilizzare il **?** e **&#42;** caratteri jolly per specificare più file. |
|         /f          |                             Consente di correggere gli errori sul disco. Il disco deve essere bloccato. Se **chkdsk** il blocco viene visualizzata l'unità, un messaggio che chiede se si desidera controllare l'unità successiva ora è possibile riavviare il computer.                             |
|         /v          |                                                                                       Visualizza il nome di ogni file in ogni directory come il controllo del disco.                                                                                        |
|         /r          |                                   Individua i settori danneggiati e recupera le informazioni leggibili. Il disco deve essere bloccato. **/r** include le funzionalità di **/f**, con ulteriori analisi degli errori del disco fisico.                                   |
|         /x          |                                                  Forza il volume da smontare in primo luogo, se necessario. Vengono invalidati tutti gli handle aperti per l'unità. **/x** include anche le funzionalità di **/f**.                                                   |
|         /i          |                                                           Utilizzare solo con NTFS. Esegue una verifica meno approfondita delle voci di indice, che riduce la quantità di tempo necessaria per eseguire **chkdsk**.                                                            |
|         /c          |                                                          Utilizzare solo con NTFS. Non verifica cicli all'interno della struttura di cartelle, che riduce la quantità di tempo necessaria per eseguire **chkdsk**.                                                           |
|    /l[:\<Size>]     |                                                         Utilizzare solo con NTFS. Modifica la dimensione del file di registro per la dimensione digitata. Se si omette il parametro di dimensione **/l** Visualizza le dimensioni correnti.                                                          |
|         / b          |           Solo NTFS: Cancella l'elenco di cluster danneggiati nel volume e aggiungerà tutti i cluster allocati e disponibile per gli errori. **/ b** include le funzionalità di **/r**. Usare questo parametro dopo la creazione dell'immagine di un volume in una nuova unità disco rigido.            |
|         /?          |                                                                                                          Visualizza la guida al prompt dei comandi.                                                                                                          |

## <a name="remarks"></a>Note

- Ignorare i controlli volume

  Il **/i** oppure **/c** commutatore riduce la quantità di tempo necessaria per eseguire **chkdsk** ignorando alcuni volume controlla.
- Controllo delle unità bloccata al momento del riavvio

  Se si desidera **chkdsk** per correggere gli errori di disco, non è possibile avere file aperti nell'unità. Se sono aperti i file, viene visualizzato il messaggio di errore seguente:

  ```
  Chkdsk cannot run because the volume is in use by another process. Would you like to schedule this volume to be checked the next time the system restarts? (Y/N)  
  ``` 

  Se si sceglie di controllare l'unità al successivo riavvio del computer, **chkdsk** Controlla l'unità e corregge automaticamente gli errori quando si riavvia il computer. Se la partizione dell'unità è una partizione di avvio, **chkdsk** Riavvia automaticamente il computer dopo aver controllato l'unità.

  È anche possibile usare la **/c chkntfs** comando per pianificare il volume da sottoporre al successivo riavvio del computer. Usare la **fsutil dirty set** comando per impostare il volume del dirty bit (che indica il danneggiamento), in modo che Windows esegue **chkdsk** quando il computer viene riavviato.
- Segnalazione di errori del disco

  Si consiglia di utilizzare **chkdsk** occasionalmente nel file system FAT e NTFS per controllare gli errori del disco. **CHKDSK** esamina lo spazio su disco e spazio su disco e fornisce un report di stato specifico di ogni file di sistema. Il report di stato Mostra errori trovati nel file system. Se si esegue **chkdsk** senza il **/f** parametro su una partizione attiva, potrebbe segnalare errori non corretti perché Impossibile bloccare l'unità.
- Correzione degli errori del disco logico

  **CHKDSK** corregge gli errori di disco logico solo se si specifica il **/f** parametro. **CHKDSK** deve essere in grado di bloccare l'unità per correggere gli errori.

  Poiché riparazioni nel file system FAT in genere la tabella di allocazione file del disco e talvolta causare una perdita di dati, **chkdsk** potrebbe essere visualizzato un messaggio di conferma simile al seguente:

  ```
  10 lost allocation units found in 3 chains.  
  Convert lost chains to files?  
  ``` 

  Se si preme **Y**, Windows Salva ogni concatenamento perso nella directory radice come file con un nome nel formato di File\<nnnn > chk. Quando **chkdsk** al termine, è possibile controllare questi file per vedere se contengono dati necessari. Se si preme **N**, Windows consente di correggere il disco, ma non salva il contenuto delle unità di allocazione perse.

  Se non si usa la **/f** parametro **chkdsk** viene visualizzato un messaggio che il file deve essere corretto, ma non la correzione gli errori.

  Se si usa **chkdsk /f** su un disco di dimensioni molto grande o con un numero molto elevato di file (ad esempio, milioni di file), **chkdsk /f** potrebbe richiedere molto tempo per il completamento.

- Ricerca di errori del disco fisico

  Usare la **/r** parametro per individuare gli errori del disco fisico del file System e tentare di ripristinare i dati da qualsiasi interessato settori del disco.

- Utilizzando **chkdsk** con file aperti

  Se si specifica la **/f** parametro **chkdsk** viene visualizzato un messaggio di errore se sono presenti file aperti sul disco. Se non si specifica la **/f** parametro e aprire file esiste, **chkdsk** potrebbe segnalare l'unità di allocazione perse sul disco. Questo problema può verificarsi se aprire i file non sono ancora stati registrati nella tabella di allocazione file. Se **chkdsk** riporta la perdita di un numero elevato di unità di allocazione, si consiglia di riparare il disco.

- Utilizzando **chkdsk** con le copie Shadow per cartelle condivise

  Poiché le copie Shadow per volume di origine di cartelle condivise non possono essere bloccate durante le copie Shadow per cartelle condivise è abilitata, in esecuzione **chkdsk** sull'origine volume potrebbe segnalare errori false o causare **chkdsk** la chiusura imprevista. È tuttavia possibile, controllare le copie shadow per errori eseguendo **chkdsk** in modalità di sola lettura (senza parametri) per controllare le copie Shadow per volume di archiviazione di cartelle condivise.

- Codici di uscita di conoscenza

  La tabella seguente elenca i codici di uscita che **chkdsk** segnala al termine.  

  | Codice di uscita |                                                   Descrizione                                                    |
  |-----------|------------------------------------------------------------------------------------------------------------------|
  |     0     |                                              Non sono stati rilevati errori.                                               |
  |     1     |                                           Gli errori rilevati e corretti.                                           |
  |     2     | Eseguire Pulitura disco (ad esempio, l'operazione di garbage collection) o Impossibile eseguire la pulizia in quanto **/f** non è stato specificato. |
  |     3     | Non è stato possibile controllare il disco, gli errori potrebbero non essere corretti o errori non corretti perché **/f** non è stato specificato.  |


- Il **chkdsk** comando con parametri diversi, è disponibile dalla Console di ripristino.
- Nei server che non vengono riavviati frequentemente, è consigliabile utilizzare il **chkntfs** o **fsutil dirty query** comandi per determinare se il volume's dirty bit è già impostano prima dell'esecuzione di chkdsk.

## <a name="examples"></a>Esempi

Se si desidera controllare il disco nell'unità D e disporre di correggere gli errori di Windows, digitare:

```
chkdsk d: /f  
```

Se si verificano errori, **chkdsk** sospende e vengono visualizzati dei messaggi. **CHKDSK** Termina visualizzando un report che elenca lo stato del disco. Non è possibile aprire qualsiasi file nell'unità specificata fino a **chkdsk** Termina.

Per controllare tutti i file in un disco FAT nella directory corrente per i blocchi non contigui, digitare:

```
chkdsk *.*  
```

**CHKDSK** Visualizza un rapporto di stato e quindi vengono elencati i file che soddisfano le specifiche dei file blocchi non contigui.
#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)