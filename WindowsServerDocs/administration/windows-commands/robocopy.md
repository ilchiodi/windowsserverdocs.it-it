---
title: robocopy
description: Informazioni su come usare il comando Robocopy in Windows e Windows Server per copiare i file
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f675f66eaafbfd79ac6b452a92417159d8ebb28c
ms.sourcegitcommit: 51e0b575ef43cd16b2dab2db31c1d416e66eebe8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76259033"
---
# <a name="robocopy"></a>robocopy

Copia i dati del file.

## <a name="syntax"></a>Sintassi

```
robocopy <Source> <Destination> [<File>[ ...]] [<Options>]
```

## <a name="parameters"></a>Parametri

|   Parametro    |                                                                                            Descrizione                                                                                           |
|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   > origine \<    |                                                                            Specifica il percorso della directory di origine.                                                                           |
| > di destinazione \< |                                                                          Specifica il percorso della directory di destinazione.                                                                        |
|    File di \<     | Specifica il file o i file da copiare. Se lo si desidera, è **&#42;** possibile utilizzare caratteri jolly (o **?** ). Se il parametro **file** non è specificato, viene utilizzato **\*.\*** come valore predefinito. |
|   Opzioni di \<   |                                                                    Specifica le opzioni da utilizzare con il comando **Robocopy** .                                                                   |

### <a name="copy-options"></a>Opzioni di copia

|Opzione|Descrizione|
|------|-----------|
|/s|Copia le sottodirectory. Si noti che questa opzione esclude le directory vuote.|
|/e|Copia le sottodirectory. Si noti che questa opzione include directory vuote. Per ulteriori informazioni, vedere la [sezione Osservazioni](#remarks).|
|/Lev:\<N >|Copia solo i primi *N* livelli dell'albero della directory di origine.|
|/z|Copia i file in modalità riavviabile.|
|/ b|Copia i file in modalità di backup.|
|/zb|Usa la modalità riavviabile. Se l'accesso viene negato, questa opzione usa la modalità di backup.|
|/efsraw|Copia tutti i file crittografati in modalità RAW EFS.|
|/Copy:\<CopyFlags >|Specifica le proprietà del file da copiare. Di seguito sono riportati i valori validi per questa opzione:</br>Dati **D**</br>**Attributi**</br>**Timestamp**</br>Elenco di controllo di accesso NTFS **S** (ACL)</br>Informazioni sul proprietario **O**</br>Informazioni sul controllo **U**</br>Il valore predefinito per **CopyFlags** è **dat** (data, attributi e timestamp).|
|/DCOPY:\<copyflags\>|Definisce gli elementi da copiare per le directory. Il valore predefinito è DA. Le opzioni sono D = data, A = Attributes e T = timestamps.|
|/sec|Copia i file con sicurezza (equivalente a **/Copy: dat**).|
|/copyall|Copia tutte le informazioni sui file (equivalente a **/Copy: DATSOU**).|
|/nocopy|Non copia informazioni sui file (utile con **/Purge**).|
|/secfix|Corregge la sicurezza dei file in tutti i file, anche quelli ignorati.|
|/timfix|Corregge gli orari dei file su tutti i file, anche quelli ignorati.|
|/Purge|Elimina i file e le directory di destinazione che non esistono più nell'origine. Per ulteriori informazioni, vedere la [sezione Osservazioni](#remarks).|
|/Mir|Rispecchia un albero di directory (equivalente a **/e** più **/Purge**). Per ulteriori informazioni, vedere la [sezione Osservazioni](#remarks).|
|/mov|Sposta i file e li elimina dall'origine dopo la copia.|
|/Move|Sposta i file e le directory e li elimina dall'origine dopo la copia.|
|/a +: [RASHCNET]|Aggiunge gli attributi specificati ai file copiati.|
|/a-.: [RASHCNET]|Rimuove gli attributi specificati dai file copiati.|
|/Create|Crea una struttura di directory e solo i file di lunghezza zero.|
|/fat|Crea file di destinazione usando solo nomi file FAT di 8,3 caratteri.|
|/256|Disattiva il supporto per percorsi molto lunghi (più di 256 caratteri).|
|/Mon:\<N >|Monitora l'origine e viene eseguita di nuovo quando vengono rilevate più di *N* modifiche.|
|/mot:\<M >|Monitora l'origine e viene eseguita nuovamente in *M* minuti se vengono rilevate modifiche.|
|/MT[:N]|Crea copie multithread con *N* thread. *N* deve essere un numero intero compreso tra 1 e 128. Il valore predefinito per *N* è 8.</br>Il parametro **/mt** non può essere usato con i parametri **/IPG** e **/EFSRAW** .</br>Reindirizzare l'output utilizzando l'opzione **/log** per ottenere prestazioni migliori.</br>Nota: il parametro/MT si applica a Windows Server 2008 R2 e Windows 7.|
|/RH: hhmm-HHMM|Specifica i tempi di esecuzione quando è possibile avviare nuove copie.|
|/PF|Verifica i tempi di esecuzione in base a un singolo file (non al passaggio).|
|/IPG: n|Specifica il gap tra pacchetti per la larghezza di banda disponibile sulle righe lente.|
|/SL|Non seguire i collegamenti simbolici e creare invece una copia del collegamento.|

> [!IMPORTANT]
> Quando si usa l'opzione di copia **/secfix** , specificare il tipo di informazioni di sicurezza che si vuole copiare usando anche una delle opzioni di copia aggiuntive seguenti:
>- **/COPYALL**
>- **/COPY: O**
>- **/COPY: S**
>- **/COPY: U**
>- **/SEC**

### <a name="file-selection-options"></a>Opzioni di selezione file

|Opzione|Descrizione|
|------|-----------|
|/a|Copia solo i file per i quali è impostato l'attributo **Archive** .|
|/m|Copia solo i file per i quali è impostato l'attributo **Archive** e Reimposta l'attributo **Archive** .|
|/ia:[RASHCNETO]|Include solo i file per i quali sono impostati gli attributi specificati.|
|/xa:[RASHCNETO]|Esclude i file per i quali sono impostati gli attributi specificati.|
|/XF \<nomefile > [...]|Esclude i file che corrispondono ai nomi o ai percorsi specificati. Si noti che *filename* può includere caratteri jolly **&#42;** (e **?** ).|
|/xD \<Directory > [...]|Esclude le directory che corrispondono ai nomi e ai percorsi specificati.|
|/xc|Esclude i file modificati.|
|/xn|Esclude i file più recenti.|
|/xo|Esclude i file meno recenti.|
|/xx|Esclude i file e le directory aggiuntivi.|
|/XL|Esclude i file e le directory "Lonely".|
|/is|Include gli stessi file.|
|/IT|Include file "modificati".|
|/Max:\<N >|Specifica le dimensioni massime del file (per escludere file di dimensioni superiori a *N* byte).|
|/min:\<N >|Specifica le dimensioni minime dei file (per escludere i file di dimensioni inferiori a *N* byte).|
|/MaxAge:\<N >|Specifica la validità massima del file (per escludere i file più vecchi di *N* giorni o Data).|
|/Minage:\<N >|Specifica la validità minima dei file (escludere i file più recenti di *N* giorni o Data).|
|/MAXLAD:\<N >|Specifica la data massima dell'ultimo accesso (esclude i file non utilizzati da *N*).|
|/MINLAD:\<N >|Specifica la data di ultimo accesso minima (esclude i file usati da *n*) se *n* è minore di 1900, *n* specifica il numero di giorni. In caso contrario, *N* specifica una data nel formato AAAAMMGG.|
|/xj|Esclude i punti di giunzione, che in genere sono inclusi per impostazione predefinita.|
|/fft|Presuppone i tempi dei file FAT (precisione di due secondi).|
|/dst|Compensa le differenze temporali di un'ora per l'ora legale.|
|/xjd|Esclude i punti di giunzione per le directory.|
|/xjf|Esclude i punti di giunzione per i file.|

### <a name="retry-options"></a>Opzioni di ripetizione dei tentativi

|Opzione|Descrizione|
|------|-----------|
|/r:\<N >|Specifica il numero di tentativi per le copie non riuscite. Il valore predefinito di *N* è 1 milione (1 milione tentativi).|
|/w:\<N >|Specifica il tempo di attesa tra i tentativi, in secondi. Il valore predefinito di *N* è 30 (tempo di attesa 30 secondi).|
|/reg|Salva i valori specificati nelle opzioni **/r** e **/w** come impostazioni predefinite nel registro di sistema.|
|/tbd|Specifica che il sistema attenderà la definizione dei nomi di condivisione (errore di ripetizione 67).|

### <a name="logging-options"></a>Opzioni di registrazione

|Opzione|Descrizione|
|------|-----------|
|/l|Specifica che i file devono essere elencati solo (e non copiati, eliminati o timestamp).|
|/x|Segnala tutti i file aggiuntivi, non solo quelli selezionati.|
|/v|Produce output dettagliato e Mostra tutti i file ignorati.|
|/ts|Include i timestamp del file di origine nell'output.|
|/fp|Include i nomi di percorso completo dei file nell'output.|
|/bytes|Stampa le dimensioni come byte.|
|/NS|Specifica che le dimensioni dei file non devono essere registrate.|
|/nc|Specifica che le classi di file non devono essere registrate.|
|/nfl|Specifica che i nomi dei file non devono essere inseriti nei log.|
|/ndl|Specifica che i nomi di directory non devono essere inseriti nei log.|
|/np|Specifica che lo stato dell'operazione di copia (il numero di file o directory copiati finora) non deve essere visualizzato.|
|/eta|Mostra il tempo stimato di arrivo (ETA) dei file copiati.|
|/log: > LogFile\<|Scrive l'output di stato nel file di log sovrascrivendo il file di log esistente.|
|/log +:\<LogFile >|Scrive l'output dello stato nel file di log (aggiunge l'output al file di log esistente).|
|/unicode|Visualizza l'output dello stato come testo Unicode.|
|/UNILOG: > LogFile\<|Scrive l'output dello stato nel file di log come testo Unicode, sovrascrivendo il file di log esistente.|
|/UNILOG +:\<LogFile >|Scrive l'output dello stato nel file di log come testo Unicode (aggiunge l'output al file di log esistente).|
|/tee|Scrive l'output dello stato nella finestra della console, oltre che nel file di log.|
|/njh|Specifica che non è presente alcuna intestazione di processo.|
|/njs|Specifica che non è presente alcun Riepilogo del processo.|

### <a name="job-options"></a>Opzioni processo

|Opzione|Descrizione|
|------|-----------|
|/minuto:\<JobName >|Specifica che i parametri devono essere derivati dal file del processo denominato.|
|/Save:\<JobName >|Specifica che i parametri devono essere salvati nel file di processo denominato.|
|/Quit|Viene chiuso dopo l'elaborazione della riga di comando (per visualizzare i parametri).|
|/nosd|Indica che non è specificata alcuna directory di origine.|
|/nodd|Indica che non è specificata alcuna directory di destinazione.|
|/If|Include i file specificati.|

### <a name="exit-return-codes"></a>Codici di uscita (ritorno)

Value | Descrizione
-- | --
0 | Nessun file copiato. Non è stato rilevato alcun errore.  Nessun file non corrispondente. I file sono già presenti nella directory di destinazione. Pertanto, l'operazione di copia è stata ignorata.
1 | Copia di tutti i file completata.
2 | Nella directory di destinazione sono presenti altri file che non sono presenti nella directory di origine. Nessun file copiato.
3 | Alcuni file sono stati copiati. Sono presenti file aggiuntivi. Non è stato rilevato alcun errore.
5 | Alcuni file sono stati copiati. Alcuni file non corrispondono. Non è stato rilevato alcun errore.
6 | Sono presenti file aggiuntivi e file non corrispondenti. Non sono stati copiati file e non sono stati rilevati errori. Ciò significa che i file sono già presenti nella directory di destinazione.
7 | I file sono stati copiati, è presente una mancata corrispondenza dei file e sono presenti file aggiuntivi.
8 | Non è stato copiato più di un file.

> [!NOTE]
> Qualsiasi valore maggiore di 8 indica che si è verificato almeno un errore durante l'operazione di copia.

### <a name="remarks"></a>Osservazioni

-   L'opzione **/Mir** è equivalente alle opzioni **/e** Plus **/Purge** con una piccola differenza nel comportamento:  
    -   Con le opzioni **/e** Plus **/Purge** , se la directory di destinazione esiste, le impostazioni di sicurezza della directory di destinazione non vengono sovrascritte.
    -   Con l'opzione **/Mir** , se la directory di destinazione esiste, le impostazioni di sicurezza della directory di destinazione vengono sovrascritte.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
