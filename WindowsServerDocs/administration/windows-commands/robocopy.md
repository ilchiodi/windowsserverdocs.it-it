---
title: robocopy
description: Informazioni su come usare il comando robocopy in Windows e Windows Server per copiare i file
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4c6e8e9-fcb3-4a4a-9d04-2d8c367b6354
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 07/25/2018
ms.openlocfilehash: 7ab2eff32b105916d979a954275e9c9122a06903
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441718"
---
# <a name="robocopy"></a>robocopy

Copia i dati di file.

## <a name="syntax"></a>Sintassi

```
robocopy <Source> <Destination> [<File>[ ...]] [<Options>]
```

## <a name="parameters"></a>Parametri

|   Parametro    |                                                                                            Descrizione                                                                                             |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   \<origine >    |                                                                            Specifica il percorso alla directory di origine.                                                                             |
| \<Destinazione > |                                                                          Specifica il percorso della directory di destinazione.                                                                          |
|    \<File>     | Specifica i file da copiare. È possibile usare caratteri jolly ( **&#42;** oppure **?** ), se si desidera. Se il **File** parametro non viene specificato, **\*.\\** \* viene usato come valore predefinito. |
|   \<Opzioni >   |                                                                    Specifica le opzioni da usare con il **robocopy** comando.                                                                     |

### <a name="copy-options"></a>Opzioni copia

|Opzione|Descrizione|
|------|-----------|
|/s|Sottodirectory di copie. Si noti che questa opzione consente di escludere le directory vuote.|
|/e|Sottodirectory di copie. Si noti che questa opzione include le directory vuote. Per altre informazioni, vedere [osservazioni](#remarks).|
|/lev:\<N>|Copia solo i primi *N* livelli dell'albero di directory di origine.|
|/z|Copia i file in modalità riavviabile.|
|/ b|Copia i file in modalità Backup.|
|/zb|Usa modalità riavviabile. Se l'accesso è negato, questa opzione Usa la modalità di Backup.|
|/EFSRAW|Copia tutti i file crittografati EFS raw-modalità.|
|/copy:\<CopyFlags>|Specifica le proprietà di file da copiare. Di seguito è i valori validi per questa opzione:</br>**1!d** dati</br>**Oggetto** attributi</br>**T** timestamp</br>**S** elenco di controllo di accesso (ACL) alle NTFS</br>**U** informazioni sul proprietario</br>**U** le informazioni di controllo</br>Il valore predefinito per **flag copia** viene **DAT** (dati, attributi e timbri data / ora).|
|/dcopy:\<copyflags\>|Definisce gli elementi da copiare per le directory. Il valore predefinito è DA. Le opzioni sono D = i dati, A = attributi e T = timestamp.|
|/sec|Copia i file con protezione (equivalente a **/Copy: DATS**).|
|/COPYALL|Copia tutte le informazioni di file (equivalente a **datsou**).|
|/NOCOPY|Non copiata alcuna informazione di file (con utili **/purge**).|
|/secfix|Correzioni di sicurezza dei file su tutti i file, anche ignorata quelli.|
|/timfix|Correzioni di tempi di file su tutti i file, anche ignorato quelle.|
|/purge|Elimina i file di destinazione e le directory che non esistono più nell'origine. Per altre informazioni, vedere [osservazioni](#remarks).|
|/mir|Riflette un albero di directory (equivalente a **/e** plus **/purge**). Per altre informazioni, vedere [osservazioni](#remarks).|
|/mov|Sposta i file e li elimina dall'origine dopo averli copiati.|
|/Move|Sposta file e directory e li elimina dall'origine dopo averli copiati.|
|/a+:[RASHCNET]|Aggiunge gli attributi specificati per i file copiati.|
|/a-:[RASHCNET]|Rimuove gli attributi specificati dal file copiati.|
|/create|Crea un albero di directory e solo i file di lunghezza zero.|
|/fat|Crea file di destinazione usando solo i nomi di file system FAT di lunghezza dei caratteri in formato 8.3.|
|/256|Consente di disattivare il supporto dei percorsi molto lunghi (più di 256 caratteri).|
|/mon:\<N>|Consente di monitorare l'origine e viene eseguito di nuovo quando oltre *N* vengono rilevate modifiche.|
|/Mot:\<M >|Esegue il monitoraggio di origine e viene eseguita nuovamente in *M* minuti se vengono rilevate modifiche.|
|/MT[:N]|Crea le copie con multithread *N* thread. *N* deve essere un numero intero compreso tra 1 e 128. Il valore predefinito per *N* è 8.</br>Il **/MT** parametro non può essere utilizzato con il **/IPG** e **/EFSRAW** parametri.</br>Reindirizza l'output utilizzando **/log** opzione per ottenere prestazioni migliori.</br>Nota: Il parametro /MT si applica a Windows Server 2008 R2 e Windows 7.|
|/rh:hhmm-hhmm|Specifica i tempi di esecuzione quando nuove copie potrebbero essere state avviate.|
|/pf|Volte in cui vengono eseguiti controlli su una base (non per i pass) per ogni file.|
|/ipg:n|Specifica il gap tra pacchetti per liberare la larghezza di banda su righe lente.|
|/sl|Non seguire i collegamenti simbolici e invece creare una copia del collegamento.|

> [!IMPORTANT]
> Quando si usa la **/SECFIX** copiare opzione, specificare il tipo di informazioni di sicurezza che si desidera copiare anche con una di queste opzioni di copia aggiuntiva:
>- **/COPYALL**
>- **/COPY:O**
>- **/COPY:S**
>- **/COPY:U**
>- **/SEC**

### <a name="file-selection-options"></a>Opzioni di selezione file

|Opzione|Descrizione|
|------|-----------|
|/a|Copia solo i file per il quale il **Archive** attributo è impostato.|
|/m|Copia solo i file per il quale il **Archive** attributo è impostato e reimposta il **Archive** attributo.|
|/ia:[RASHCNETO]|Include solo i file per il quale gli attributi specificati sono impostate.|
|/xa:[RASHCNETO]|Esclude i file per il quale gli attributi specificati sono impostate.|
|/XF \<FileName > [...]|Esclude il file che soddisfano i percorsi o i nomi specificati. Si noti che *nomefile* può includere caratteri jolly ( **&#42;** e **?** ).|
|/XD \<directory > [...]|Esclude le directory che corrispondono ai nomi specificati e i percorsi.|
|/xc|Esclude i file modificati.|
|/xn|Esclude i file più recenti.|
|/xo|Esclude i file meno recenti.|
|/xx|Esclude le directory e file aggiuntivi.|
|/xl|Esclude le directory e file "tristi".|
|/is|Include gli stessi file.|
|/IT|Include "modificati" i file.|
|/max:\<N>|Specifica le dimensioni massime del file (per escludere i file più grandi rispetto *N* byte).|
|/min:\<N>|Specifica la dimensione minima del file (per escludere file minori *N* byte).|
|/MAXAGE:\<N >|Specifica la durata massima di file (per escludere i file più vecchi *N* giorni o data).|
|/minAge:\<N >|Specifica la durata minima del file (escludere i file più recente *N* giorni o data).|
|/maxlad:\<N>|Specifica il numero massimo data dell'ultimo accesso (esclusi i file non utilizzati *N*).|
|/minlad:\<N>|Specifica il numero minimo di ultima data di accesso (esclude i file utilizzati a partire *N*) se *N* è minore di 1900 *N* specifica il numero di giorni. In caso contrario, *N* specifica una data nel formato aaaammgg.|
|/xj|Consente di escludere i punti di giunzione, che sono in genere inclusi per impostazione predefinita.|
|/fft|Si presuppone che il file system FAT volte (precisione di due secondi).|
|/dst|Compensare le differenze di orario dell'ora legale di un'ora.|
|/xjd|Consente di escludere i punti di giunzione per le directory.|
|/xjf|Consente di escludere i punti di giunzione per i file.|

### <a name="retry-options"></a>Le opzioni di ripetizione

|Opzione|Descrizione|
|------|-----------|
|/r:\<N>|Specifica il numero di tentativi su copie non riuscite. Il valore predefinito *N* è pari a 1.000.000 (un milione di tentativi).|
|/w:\<N>|Specifica il tempo di attesa tra i tentativi, in secondi. Il valore predefinito *N* è 30 (30 secondi di tempo di attesa).|
|/reg|Salva i valori specificati nel **/r** e **/w** opzioni come impostazioni predefinite nel Registro di sistema.|
|/tbd|Specifica che il sistema attenderà per definire i nomi di condivisione (nuovo tentativo errore 67).|

### <a name="logging-options"></a>Opzioni di registrazione

|Opzione|Descrizione|
|------|-----------|
|/l|Specifica che devono essere elencati solo i file (e non viene copiato, eliminati, o con indicazione di data ora).|
|/x|Segnala tutti i file aggiuntivi, non solo quelli selezionati.|
|/v|Produce un output dettagliato e Mostra tutti i file ignorati.|
|/ts|Include i timbri data / ora di file di origine nell'output.|
|/fp|Include i nomi di percorso completo dei file nell'output.|
|/bytes|Stampa le dimensioni, come byte.|
|/NS|Specifica che le dimensioni dei file non devono essere registrate.|
|/nc|Specifica che le classi di file non devono essere registrate.|
|/nfl|Specifica che i nomi di file non devono essere registrate.|
|/ndl|Specifica che i nomi di directory non devono essere registrate.|
|/np|Specifica che lo stato di avanzamento dell'operazione di copia (il numero di file o directory finora copiate) non verrà visualizzato.|
|/eta|Visualizza il tempo stimato di arrivo (ETA) dei file copiati.|
|/log:\<LogFile>|Scrive l'output di stato nel file di log (sovrascrive il file di log esistente).|
|/log+:\<LogFile>|Scrive l'output di stato nel file di log (Accoda output a file di log esistente).|
|/unicode|Visualizza l'output di stato come testo Unicode.|
|/unilog:\<LogFile>|Scrive lo stato di output nel file di log come testo Unicode (sovrascrive il file di log esistente).|
|/unilog+:\<LogFile>|Scrive lo stato di output nel file di log come testo Unicode (Accoda output a file di log esistente).|
|/Tee|Scrive l'output di stato alla finestra della console, nonché nel file di log.|
|/njh|Non specifica che non esiste alcuna intestazione di processo.|
|/njs|Non specifica che non esiste alcun riepilogo del processo.|

### <a name="job-options"></a>Opzioni di processo

|Opzione|Descrizione|
|------|-----------|
|/job:\<JobName>|Specifica che i parametri devono essere derivate dal file processo con un nome.|
|/save:\<JobName>|Specifica che i parametri devono essere salvate nel file processo con un nome.|
|/quit|Viene chiuso dopo la riga di comando di elaborazione (per visualizzare i parametri).|
|/nosd|Indica che viene specificata alcuna directory di origine.|
|/nodd|Indica che viene specificata alcuna directory di destinazione.|
|/if|Include i file specificati.|

### <a name="exit-return-codes"></a>Codici di uscita (return)

Value | Descrizione
-- | --
0 | Nessun file copiato. Si è verificato alcun errore.  Nessun file sono stati corrispondenti. I file esistono già nella directory di destinazione; Pertanto, l'operazione di copia è stato ignorato.
1 | Tutti i file sono stati copiati correttamente.
2 | Esistono alcuni file aggiuntivi nella directory di destinazione che non sono presenti nella directory di origine. Nessun file copiato.
3 | Alcuni file sono stati copiati. Sono presenti file aggiuntivi. Si è verificato alcun errore.
5 | Alcuni file sono stati copiati. Alcuni file sono stati corrispondenti. Si è verificato alcun errore.
6 | File non corrispondenti e altri file esistano. Nessun file sono stato copiato e non vengono visualizzati errori rilevati. Ciò significa che i file esistano già nella directory di destinazione.
7 | Sono stati copiati i file, era presente una mancata corrispondenza tra file e file aggiuntivi presenti.
8 | Diversi file non è stato copiato.

> [!NOTE]
> Qualsiasi valore maggiore di 8 indica che si è verificato almeno un errore durante l'operazione di copia.

### <a name="remarks"></a>Note

-   Il **/mir** opzione è equivalente al **/e** plus **/purge** opzioni con una piccola differenza nel comportamento:  
    -   Con il **/e** plus **/purge** opzioni, se è presente la directory di destinazione, le impostazioni di sicurezza directory di destinazione non vengono sovrascritte.
    -   Con il **/mir** opzione, se è presente la directory di destinazione, le impostazioni di sicurezza directory di destinazione verranno sovrascritte.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
