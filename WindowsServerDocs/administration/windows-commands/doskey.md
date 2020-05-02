---
title: doskey
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4874fd43-d5ea-45f3-ae24-388ae925ed76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fca89bd2e99b6139b13a5bd45481ae0ec2248574
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720813"
---
# <a name="doskey"></a>doskey



Chiama Doskey.exe (quali richiami precedentemente immesso i comandi della riga di comando), modifica le righe di comando e crea macro.



## <a name="syntax"></a>Sintassi

```
doskey [/reinstall] [/listsize=<Size>] [/macros:[all | <ExeName>] [/history] [/insert | /overstrike] [/exename=<ExeName>] [/macrofile=<FileName>] [<MacroName>=[<Text>]]
```

### <a name="parameters"></a>Parametri

|       Parametro        |                                                                                                                          Descrizione                                                                                                                           |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /REINSTALL       |                                                                                            Installa una nuova copia di Doskey.exe e cancella il buffer della cronologia dei comandi.                                                                                            |
|   /LISTSIZE =\<dimensioni>    |                                                                                                Specifica il numero massimo di comandi nel buffer.                                                                                                 |
|        /Macros         |                                        Visualizza un elenco di tutti **doskey** macro. È possibile utilizzare il simbolo di reindirizzamento (**>**) con **/Macros** per reindirizzare l'elenco a un file. È possibile abbreviare **/macros** a **/m**.                                         |
|      /Macros:all       |                                                                                                        Consente di visualizzare **doskey** macro per tutti i file eseguibili.                                                                                                         |
|   /Macros:\<exename>   |                                                                                             Consente di visualizzare **doskey** macro per l'eseguibile specificato da *ExeName*.                                                                                              |
|        /History        |                                    Visualizza tutti i comandi che vengono archiviati nella memoria. È possibile utilizzare il simbolo di reindirizzamento (**>**) con **/History** per reindirizzare l'elenco a un file. È possibile abbreviare **/history** come **/h**.                                    |
| /Insert | Specifica che il nuovo testo digitato viene inserito nel testo precedente. |
| /overstrike | Specifica che il nuovo testo sovrascrive il testo precedente. |
|  /EXENAME =\<exename>   |                                                                                        Specifica il programma (vale a dire eseguibile) in cui il **doskey** macro viene eseguita.                                                                                         |
| /MACROFILE =\<filename> |                                                                                              Specifica un file che contiene le macro che si desiderano installare.                                                                                               |
| \<Macroname>= [\<testo>]  | Crea una macro che esegue i comandi specificati da *testo*. *Nomemacro* Specifica il nome da assegnare alla macro. *Testo* Specifica i comandi che si desidera registrare. Se *testo* viene lasciato vuoto, *nomemacro* vengono cancellati tutti i comandi assegnati. |
|           /?           |                                                                                                              Visualizza la guida al prompt dei comandi.                                                                                                              |

## <a name="remarks"></a>Osservazioni

- Utilizzo Doskey.exe

  Doskey.exe è sempre disponibile per i programmi basati su caratteri, interattivi (ad esempio debugger o i programmi di trasferimento file) e mantiene un buffer della cronologia dei comandi e le macro per ogni programma che viene avviato. Non è possibile utilizzare **doskey** Opzioni della riga di comando da un programma. È necessario eseguire **doskey** Opzioni della riga di comando prima di avviare un programma. Eseguire l'override di tasti programma **doskey** chiave di assegnazioni.
- Richiamo di un comando

  Per richiamare un comando, è possibile utilizzare i tasti seguenti dopo l'avvio di Doskey.exe. Se si utilizza Doskey.exe all'interno di un programma, le assegnazioni di tasti del programma che hanno la precedenza.  

  |    Chiave     |                              Descrizione                              |
  |------------|-----------------------------------------------------------------------|
  |  FRECCIA SU  |  Richiama il comando che è stato utilizzato prima di quello che viene visualizzato.  |
  | Freccia GIÙ |  Richiama il comando che è stato utilizzato successiva a quella che viene visualizzato.   |
  |  PGSU   |    Richiama il primo comando che è stato utilizzato nella sessione corrente.    |
  | PGGIÙ  | Richiama il comando più recente che è stato utilizzato nella sessione corrente. |


- La modifica della riga di comando

  Con Doskey.exe, è possibile modificare la riga di comando corrente. Se si utilizza Doskey.exe all'interno di un programma, le assegnazioni di tasti del programma che hanno la precedenza e alcuni Doskey.exe tasti di modifica potrebbe non funzionare.

  La tabella seguente elenca **doskey** Modifica di chiavi e le relative funzioni.  

  | Tasto o della combinazione |                                                                                                                                                       Descrizione                                                                                                                                                       |
  |------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |       FRECCIA SINISTRA       |                                                                                                                                      Sposta il punto di inserimento indietro di un carattere.                                                                                                                                      |
  |      FRECCIA DESTRA       |                                                                                                                                    Sposta l'inserimento carattere successivo.                                                                                                                                     |
  |    CTRL+freccia SINISTRA     |                                                                                                                                        Sposta il cursore sulla precedente parola.                                                                                                                                         |
  |    CTRL+freccia DESTRA    |                                                                                                                                       Sposta il cursore Avanti uno sulla parola.                                                                                                                                       |
  |          HOME          |                                                                                                                                 Sposta il punto di inserimento all'inizio della riga.                                                                                                                                 |
  |          END           |                                                                                                                                    Sposta il punto di inserimento alla fine della riga.                                                                                                                                    |
  |          ESC           |                                                                                                                                          Cancella il comando dalla visualizzazione.                                                                                                                                           |
  |           F1           |                                                                      Copia un carattere di una colonna nel modello per la stessa colonna nella finestra del prompt dei comandi. (Il modello è un buffer di memoria che contiene l'ultimo comando digitato).                                                                       |
  |           F2           |                                                                 Cerca in avanti nel modello per la chiave successiva dopo aver digitato preme F2. Doskey.exe inserisce il testo dal modello, ovvero a, ma non include, il carattere specificate.                                                                  |
  |           F3           |                                                 Copia il resto del modello per la riga di comando. Doskey.exe inizia la copia dei caratteri dalla posizione nel modello che corrisponde alla posizione indicata dal punto di inserimento nella riga di comando.                                                 |
  |           F4           |                                                                            Elimina che tutti i caratteri dall'inserimento corrente scegliere posizione fino a, ma non includerlo, l'occorrenza successiva del carattere che segue si preme F4.                                                                            |
  |           F5           |                                                                                                                                   Copia il modello nella riga di comando corrente.                                                                                                                                    |
  |           F6           |                                                                                                                    Posiziona un carattere di fine del file (CTRL + Z) in corrispondenza del punto di inserimento corrente.                                                                                                                    |
  |           F7           | Consente di visualizzare (in una finestra di dialogo) tutti i comandi per il programma che vengono archiviati nella memoria. Utilizzare il tasto FRECCIA SU e FRECCIA GIÙ per selezionare il comando desiderato e premere INVIO per eseguire il comando. È inoltre possibile annotare il numero di sequenza che precede il comando e utilizzare questo numero in combinazione con il tasto F9. |
  |         ALT+F7         |                                                                                                                          Elimina tutti i comandi presenti in memoria per il buffer corrente.                                                                                                                          |
  |           F8           |                                                                                                           Visualizza tutti i comandi nel buffer che iniziano con i caratteri nel comando corrente.                                                                                                            |
  |           F9           |                                             Richiede un numero di comandi di buffer della cronologia e quindi Visualizza il comando associato al numero specificato. Premere INVIO per eseguire il comando. Per visualizzare tutti i numeri e i relativi comandi, premere F7.                                             |
  |        ALT + F10         |                                                                                                                                             Elimina tutte le definizioni di macro.                                                                                                                                              |


- Utilizzando **doskey** all'interno di un programma

  Alcuni programmi basati su caratteri e interattivi, ad esempio debugger o programmi di trasferimento di file (FTP) utilizzano automaticamente Doskey.exe. Per utilizzare Doskey.exe, un programma deve essere un processo di console e utilizzare memorizzato nel buffer di input. Eseguire l'override di tasti programma **doskey** chiave di assegnazioni. Ad esempio, se il programma utilizza F7 per una funzione, è possibile ottenere un **doskey** comando cronologia in una finestra popup.

  Con Doskey.exe, è possibile mantenere una cronologia dei comandi per ogni programma che si avvia o ripetere. È possibile modificare i precedenti comandi al prompt dei comandi del programma e avviare **doskey** macro create per il programma. Se si chiude e quindi riavviarla un programma dalla stessa finestra del prompt dei comandi, la cronologia dei comandi della sessione precedente programma è disponibile.

  È necessario eseguire Doskey.exe prima di avviare un programma. Non è possibile utilizzare **doskey** Opzioni della riga di comando da un comando prompt dei comandi, anche se il programma dispone di un comando della shell.

  Se si desidera creare e personalizzare il funzionamento di Doskey.exe con un programma **doskey** macro per il programma, è possibile creare un file batch che modifica Doskey.exe e avvia il programma.
- Specifica una modalità di inserimento predefinita

  Se si preme il tasto INS, è possibile digitare testo nel **doskey** nel testo esistente senza sostituire il testo della riga di comando. Tuttavia, dopo aver premuto INVIO, Doskey.exe restituisce la tastiera per modalità di sostituzione. È necessario premere INS per tornare alla modalità di inserimento.

  Utilizzare **/inserire** Attiva la modalità di inserimento ogni volta che si preme INVIO. Rimane in modo efficace in modalità di inserimento fino a quando non si utilizza tastiera **/overstrike**. È possibile tornare alla modalità di sostituire temporaneamente premendo il tasto INS, ma dopo aver premuto INVIO, Doskey.exe restituisce la tastiera per la modalità di inserimento.

  Il cursore cambia forma quando si utilizza il tasto INS per modificare da una modalità a altra.
- Creazione di una macro

  È possibile utilizzare Doskey.exe per creare macro che eseguono uno o più comandi. Nella tabella seguente sono elencati i caratteri speciali che è possibile utilizzare per controllare le operazioni di comando quando si definisce una macro.  

  |   Carattere   |                                                                                                                                                                               Descrizione                                                                                                                                                                               |
  |---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |   $G o $g    |                                                                                   Reindirizza l'output. Utilizzare uno di questi caratteri speciali per inviare l'output a un dispositivo o un file anziché sullo schermo. Questo carattere è equivalente al simbolo di reindirizzamento per output (**>**).                                                                                    |
  | $G$ g $G o $g  |                                                         Aggiunge alla fine di un file di output. Utilizzare uno di questi caratteri doppi per Accoda output a un file esistente anziché sostituire i dati nel file. Questi caratteri doppi sono equivalenti al simbolo di reindirizzamento di Accodamento per l'**>>** output ().                                                         |
  |   $L o $l    |                                                                                  Reindirizza l'input. Utilizzare uno di questi caratteri speciali per leggere l'input da un dispositivo o un file anziché dalla tastiera. Questo carattere è equivalente al simbolo di reindirizzamento per input (**<**).                                                                                  |
  |   $B o $b    |                                                                                                                                    Invia l'output (macro) a un comando. Questi caratteri speciali sono equivalenti all'uso della pipe (\*\*                                                                                                                                     |
  |   $T o $t    |                                                            Consente di separare i comandi. Utilizzare uno di questi caratteri speciali per separare i comandi quando si creano macro o digitare i comandi di **doskey** riga di comando. Questi caratteri speciali sono equivalenti a utilizzare la e**&** commerciale () in una riga di comando.                                                            |
  |      $$       |                                                                                                                                                              Specifica il carattere di segno di dollaro**$**().                                                                                                                                                               |
  | $1 e $9 |             Rappresentano le informazioni della riga di comando per specificare quando si esegue la macro. I caratteri speciali **$1** tramite **$9** sono parametri batch che consentono di utilizzare dati diversi nella riga di comando ogni volta che si esegue la macro. Il **$1** carattere in un **doskey** comando è simile al **%1** carattere in un file batch.             |
  |      $\*      | Rappresenta tutte le informazioni della riga di comando che si desidera specificare quando si digita il nome della macro. ** $ ** Il carattere \* speciale è un parametro sostituibile che è simile ai parametri del batch da **$1** a **$9**, con una differenza importante: tutto ciò che si digita nella riga di comando dopo che il nome della macro viene sostituito ** $ ** \* nella macro. |


- Esecuzione di un **doskey** (macro)

  Per eseguire una macro, digitare il nome della macro al prompt dei comandi, a partire dalla posizione del primo. Se la macro è stata definita ** $ **con * o uno dei parametri del batch da **$1** a **$9**, usare uno spazio per separare i parametri. Non è possibile eseguire un **doskey** (macro) da un file batch.
- Creazione di una macro con lo stesso nome di un comando della famiglia Windows Server 2003

  Se si utilizza sempre un determinato comando con le opzioni della riga di comando specifiche, è possibile creare una macro che ha lo stesso nome del comando. Per specificare se si desidera eseguire il comando o la macro, attenersi alle seguenti indicazioni:  
  -   Per eseguire la macro, digitare il nome della macro al prompt dei comandi. Non aggiungere uno spazio prima il nome della macro.
  -   Per eseguire il comando, inserire uno o più spazi al prompt dei comandi e digitare il nome del comando.
- Eliminazione di una macro

  Per eliminare una macro, digitare:  
  ```
  doskey <MacroName> =
  ```

## <a name="examples"></a>Esempi

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
Per definire una macro con più comandi, utilizzare **$t** per separare i comandi, come indicato di seguito:
```
doskey tx=cd temp$tdir/w $*
```
Nell'esempio precedente, la macro TX cambia la directory corrente alla posizione temporanea e quindi visualizza un elenco in formato esteso di directory. È possibile usare ** $ *** alla fine della macro per aggiungere altre opzioni della riga di comando a **dir** quando si esegue TX.

La macro seguente utilizza un parametro di batch per un nuovo nome di directory:
```
doskey mc=md $1$tcd $1
```
La macro crea una nuova directory e quindi diventa la nuova directory dalla directory corrente.

Per utilizzare la macro precedente per creare e modificare la directory libri, digitare:
```
mc books
```
Per creare un **doskey** macro per un programma denominato Ftp.exe, includere **/exename** come indicato di seguito:
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
Per eliminare una macro denominata vlist, digitare:
```
doskey vlist =
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
