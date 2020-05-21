---
title: doskey
description: Argomento di riferimento per il comando DOSKEY e DOSKEY. exe, che richiama i comandi della riga di comando immessi in precedenza, modifica le righe di comando e crea macro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4874fd43-d5ea-45f3-ae24-388ae925ed76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 96a47a40463c5cd6af16ab637f96382228f7d0f8
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437026"
---
# <a name="doskey"></a>doskey

Chiama DOSKEY. exe, che richiama i comandi della riga di comando immessi in precedenza, modifica le righe di comando e crea le macro.

## <a name="syntax"></a>Sintassi

```
doskey [/reinstall] [/listsize=<size>] [/macros:[all | <exename>] [/history] [/insert | /overstrike] [/exename=<exename>] [/macrofile=<filename>] [<macroname>=[<text>]]
```

### <a name="parameters"></a>Parametri

| Parametro | Description |
| --------- | ----------- |
| /REINSTALL | Installa una nuova copia di Doskey.exe e cancella il buffer della cronologia dei comandi. |
| /LISTSIZE =`<size>` | Specifica il numero massimo di comandi nel buffer. |
| /Macros | Visualizza un elenco di tutti **doskey** macro. È possibile utilizzare il simbolo di reindirizzamento (`>`) con **/macros** per reindirizzare l'elenco in un file. È possibile abbreviare **/macros** a **/m**. |
| /Macros:all | Consente di visualizzare **doskey** macro per tutti i file eseguibili. |
| /Macros`<exename>` | Visualizza le macro **DOSKEY** per il file eseguibile specificato da *exename*. |
| /History | Visualizza tutti i comandi che vengono archiviati nella memoria. È possibile utilizzare il simbolo di reindirizzamento (`>`) con **/history** per reindirizzare l'elenco in un file. È possibile abbreviare **/history** come **/h**. |
| /Insert | Specifica che il nuovo testo digitato viene inserito nel testo precedente. |
| /overstrike | Specifica che il nuovo testo sovrascrive il testo precedente. |
| /EXEName =`<exename>` | Specifica il programma (vale a dire eseguibile) in cui il **doskey** macro viene eseguita. |
| /MACROFILE =`<filename>` | Specifica un file che contiene le macro che si desiderano installare. |
| `<macroname>`=[`<text>`] | Crea una macro che esegue i comandi specificati da *testo*. *Nomemacro* Specifica il nome da assegnare alla macro. *Testo* Specifica i comandi che si desidera registrare. Se *testo* viene lasciato vuoto, *nomemacro* vengono cancellati tutti i comandi assegnati. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Alcuni programmi basati su caratteri e interattivi, ad esempio debugger o programmi di trasferimento di file (FTP) utilizzano automaticamente Doskey.exe. Per utilizzare Doskey.exe, un programma deve essere un processo di console e utilizzare memorizzato nel buffer di input. Eseguire l'override di tasti programma **doskey** chiave di assegnazioni. Ad esempio, se il programma utilizza F7 per una funzione, è possibile ottenere un **doskey** comando cronologia in una finestra popup.

- È possibile utilizzare DOSKEY. exe per modificare la riga di comando corrente, ma non è possibile utilizzare le opzioni della riga di comando dal prompt dei comandi di un programma. È necessario eseguire **doskey** Opzioni della riga di comando prima di avviare un programma. Se si utilizza Doskey.exe all'interno di un programma, le assegnazioni di tasti del programma che hanno la precedenza e alcuni Doskey.exe tasti di modifica potrebbe non funzionare.

- Con Doskey.exe, è possibile mantenere una cronologia dei comandi per ogni programma che si avvia o ripetere. È possibile modificare i precedenti comandi al prompt dei comandi del programma e avviare **doskey** macro create per il programma. Se si chiude e quindi riavviarla un programma dalla stessa finestra del prompt dei comandi, la cronologia dei comandi della sessione precedente programma è disponibile.

- Per richiamare un comando, è possibile utilizzare una delle seguenti chiavi dopo l'avvio di DOSKEY. exe:

  | Chiave | Descrizione |
  | --- | ----------- |
  | FRECCIA SU | Richiama il comando che è stato utilizzato prima di quello che viene visualizzato. |
  | Freccia GIÙ | Richiama il comando che è stato utilizzato successiva a quella che viene visualizzato. |
  | PGSU | Richiama il primo comando che è stato utilizzato nella sessione corrente. |
  | PGGIÙ | Richiama il comando più recente che è stato utilizzato nella sessione corrente. |

- Nella tabella seguente sono elencate le chiavi di modifica di **DOSKEY** e le relative funzioni:

  | Tasto o della combinazione | Descrizione |
  | ---------------------- | ----------- |
  | FRECCIA SINISTRA | Sposta il punto di inserimento indietro di un carattere. |
  | FRECCIA DESTRA | Sposta l'inserimento carattere successivo. |
  | CTRL+freccia SINISTRA | Sposta il cursore sulla precedente parola. |
  | CTRL+freccia DESTRA | Sposta il cursore Avanti uno sulla parola. |
  | HOME | Sposta il punto di inserimento all'inizio della riga. |
  | FINE | Sposta il punto di inserimento alla fine della riga. |
  | ESC | Cancella il comando dalla visualizzazione. |
  | F1 | Copia un carattere di una colonna nel modello per la stessa colonna nella finestra del prompt dei comandi. (Il modello è un buffer di memoria che contiene l'ultimo comando digitato). |
  | F2 | Cerca in avanti nel modello per la chiave successiva dopo aver digitato preme F2. Doskey.exe inserisce il testo dal modello, ovvero a, ma non include, il carattere specificate. |
  | F3 | Copia il resto del modello per la riga di comando. Doskey.exe inizia la copia dei caratteri dalla posizione nel modello che corrisponde alla posizione indicata dal punto di inserimento nella riga di comando. |
  | F4 | Elimina che tutti i caratteri dall'inserimento corrente scegliere posizione fino a, ma non includerlo, l'occorrenza successiva del carattere che segue si preme F4. |
  | F5 | Copia il modello nella riga di comando corrente. |
  | F6 | Posiziona un carattere di fine del file (CTRL + Z) in corrispondenza del punto di inserimento corrente. |
  | F7 | Consente di visualizzare (in una finestra di dialogo) tutti i comandi per il programma che vengono archiviati nella memoria. Utilizzare il tasto FRECCIA SU e FRECCIA GIÙ per selezionare il comando desiderato e premere INVIO per eseguire il comando. È inoltre possibile annotare il numero di sequenza che precede il comando e utilizzare questo numero in combinazione con il tasto F9. |
  | ALT+F7 | Elimina tutti i comandi presenti in memoria per il buffer corrente. |
  | F8 | Visualizza tutti i comandi nel buffer che iniziano con i caratteri nel comando corrente. |
  | F9 | Richiede un numero di comandi di buffer della cronologia e quindi Visualizza il comando associato al numero specificato. Premere INVIO per eseguire il comando. Per visualizzare tutti i numeri e i relativi comandi, premere F7. |
  | ALT + F10 | Elimina tutte le definizioni di macro. |

- Se si preme il tasto INS, è possibile digitare testo nel **doskey** nel testo esistente senza sostituire il testo della riga di comando. Tuttavia, dopo aver premuto INVIO, DOSKEY. exe restituisce la tastiera per la modalità di **sostituzione** . È necessario premere di nuovo Inserisci per tornare alla modalità di **inserimento** .

- Il cursore cambia forma quando si utilizza il tasto INS per modificare da una modalità a altra.

- Se si desidera creare e personalizzare il funzionamento di Doskey.exe con un programma **doskey** macro per il programma, è possibile creare un file batch che modifica Doskey.exe e avvia il programma.

- È possibile utilizzare Doskey.exe per creare macro che eseguono uno o più comandi. Nella tabella seguente sono elencati i caratteri speciali che è possibile utilizzare per controllare le operazioni di comando quando si definisce una macro.

  | Carattere | Descrizione |
  |---------- | ----------- |
  | `$G` o `$g` | Reindirizza l'output. Utilizzare uno di questi caratteri speciali per inviare l'output a un dispositivo o un file anziché sullo schermo. Questo carattere corrisponde al simbolo di reindirizzamento dell'output (`>`). |
  | `$G$G` o `$g$g` | Aggiunge alla fine di un file di output. Utilizzare uno di questi caratteri doppi per Accoda output a un file esistente anziché sostituire i dati nel file. Questi caratteri doppi sono equivalenti per il simbolo di reindirizzamento append (`>>`). |
  | `$L` o `$l` | Reindirizza l'input. Utilizzare uno di questi caratteri speciali per leggere l'input da un dispositivo o un file anziché dalla tastiera. Questo carattere corrisponde al simbolo di reindirizzamento dell'input (`<`). |
  | `$B` o `$b` | Invia l'output (macro) a un comando. Questi caratteri speciali sono equivalenti all'uso della pipe `(` e di `*` . |
  | `$T` o `$t` | Consente di separare i comandi. Utilizzare uno di questi caratteri speciali per separare i comandi quando si creano macro o digitare i comandi di **doskey** riga di comando. Questi caratteri speciali sono equivalenti a utilizzare la e commerciale (`&`) nella riga di comando. |
  | `$$` | Specifica il carattere di segno di dollaro (`$`). |
  | `$1`attraverso`$9` | Rappresentano le informazioni della riga di comando per specificare quando si esegue la macro. I caratteri speciali `$1` tramite `$9` sono parametri batch che consentono di utilizzare dati diversi nella riga di comando ogni volta che si esegue la macro. Il `$1` carattere in un comando **DOSKEY** è simile al `%1` carattere in un programma batch. |
  | `$*` | Rappresenta tutte le informazioni della riga di comando che si desidera specificare quando si digita il nome della macro. Il carattere speciale `$*` è un parametro sostituibile che è simile ai parametri del batch `$1` tramite `$9` , con una differenza importante: tutto ciò che si digita nella riga di comando dopo che il nome della macro viene sostituito `$*` nella macro. |

- Per eseguire una macro, digitare il nome della macro al prompt dei comandi, a partire dalla posizione del primo. Se la macro è stata definita con `$*` o uno dei parametri di batch `$1` tramite `$9` , utilizzare uno spazio per separare i parametri. Non è possibile eseguire un **doskey** (macro) da un file batch.

- Se si utilizza sempre un determinato comando con le opzioni della riga di comando specifiche, è possibile creare una macro che ha lo stesso nome del comando. Per specificare se si desidera eseguire il comando o la macro, attenersi alle seguenti indicazioni:

  - Per eseguire la macro, digitare il nome della macro al prompt dei comandi. Non aggiungere uno spazio prima il nome della macro.

  - Per eseguire il comando, inserire uno o più spazi al prompt dei comandi e digitare il nome del comando.

### <a name="examples"></a>Esempi

Il **/macros** e **/history** Opzioni della riga di comando sono utili per la creazione di programmi di batch per salvare le macro e i comandi. Ad esempio, per archiviare tutti corrente **doskey** macro, digitare:

```
doskey /macros > macinit
```

Per utilizzare le macro memorizzate in Macinit, digitare:

```
doskey /macrofile=macinit
```

Per creare un batch programma denominato tmp contenente recentemente utilizzati comandi, tipo:

```
doskey /history> tmp.bat
```

Per definire una macro con più comandi, usare `$t` per separare i comandi, come indicato di seguito:

```
doskey tx=cd temp$tdir/w $*
```

Nell'esempio precedente, la macro TX cambia la directory corrente alla posizione temporanea e quindi visualizza un elenco in formato esteso di directory. È possibile usare `$*` alla fine della macro per aggiungere altre opzioni della riga di comando a **dir** quando si esegue l'opzione TX.

La macro seguente utilizza un parametro di batch per un nuovo nome di directory:

```
doskey mc=md $1$tcd $1
```

La macro crea una nuova directory e quindi diventa la nuova directory dalla directory corrente.

Per utilizzare la macro precedente per creare e passare a una directory denominata *books*, digitare:

```
mc books
```

Per creare una macro **DOSKEY** per un programma denominato *FTP. exe*, includere **/exename** nel modo seguente:

```
doskey /exename=ftp.exe go=open 172.27.1.100$tmget *.TXT c:\reports$tbye
```

Per utilizzare la macro precedente, avviare FTP. Al prompt dei comandi FTP, digitare:

```
go
```

FTP viene eseguito il **aprire**, **mget**, e **bye** comandi.

Per creare una macro che rapidamente e in modo incondizionato formatta un disco, digitare:

```
doskey qf=format $1 /q /u
```

Per formattare un disco nell'unità e in modo non condizionale, digitare:

```
qf a:
```

Per eliminare una macro denominata *Vlist*, digitare:

```
doskey vlist =
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
