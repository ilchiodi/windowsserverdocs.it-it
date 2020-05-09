---
title: dir
description: Argomento di riferimento per il comando dir, che visualizza un elenco dei file e delle sottodirectory di una directory.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: edcbf69b-eaa4-466e-b210-3dd8892f4d93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 067b4961aecc6149d6ff68872123acb2cfe99305
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992536"
---
# <a name="dir"></a>dir

Visualizza un elenco di file e le sottodirectory della directory. Se usato senza parametri, questo comando Visualizza l'etichetta del volume e il numero di serie del disco, seguiti da un elenco di directory e file sul disco (inclusi i relativi nomi e la data e l'ora dell'Ultima modifica di ogni). Per i file, questo comando Visualizza l'estensione del nome e le dimensioni in byte. Questo comando Visualizza anche il numero totale di file e directory elencati, le relative dimensioni cumulative e lo spazio disponibile (in byte) rimanente sul disco.

Il comando **dir** può essere eseguito anche dalla Console ripristino Windows utilizzando parametri diversi. Per ulteriori informazioni, vedere [ambiente ripristino Windows (WinRE)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintassi

```
dir [<drive>:][<path>][<filename>] [...] [/p] [/q] [/w] [/d] [/a[[:]<attributes>]][/o[[:]<sortorder>]] [/t[[:]<timefield>]] [/s] [/b] [/l] [/n] [/x] [/c] [/4]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `[<drive>:][<path>]` | Specifica l'unità e directory per il quale si desidera visualizzare un elenco. |
| `[<filename>]` | Specifica un file specifico o un gruppo di file per il quale si desidera visualizzare un elenco. |
| / p | Visualizza una schermata dell'elenco alla volta. Per visualizzare la schermata successiva, premere un tasto qualsiasi. |
| /q | Visualizza informazioni sulla proprietà di file. |
| /W | Visualizza l'elenco in formato esteso, con un massimo di cinque nomi di file o i nomi di directory in ogni riga. |
| /d | Visualizza l'elenco nello stesso formato **/w**, ma i file vengono ordinati per colonna. |
| /a[[:]`<attributes>`] | Visualizza solo i nomi delle directory e dei file con gli attributi specificati. Se non si usa questo parametro, il comando Visualizza i nomi di tutti i file tranne quelli nascosti e i file di sistema. Se si usa questo parametro senza specificare alcun *attributo*, il comando Visualizza i nomi di tutti i file, inclusi i file nascosti e di sistema. L'elenco dei valori possibili degli *attributi* è il seguente:<ul><li>Directory **d**</li><li>file nascosti **h**</li><li>**s** -file di sistema</li><li>**l** -reparse point</li><li>**r** -file di sola lettura</li><li>**a** -file pronti per l'archiviazione</li><li>**i-non** contenuti nei file indicizzati</li></ul>È possibile usare qualsiasi combinazione di questi valori, ma non separare i valori usando spazi. Facoltativamente, è possibile usare i due punti (:) separatore oppure è possibile usare un trattino (-) come prefisso per indicare "not". Se ad esempio si usa l'attributo **-s** , non verranno visualizzati i file di sistema. |
| /o[[:]`<sortorder>`] | Ordina l'output in base a *SortOrder*, che può essere qualsiasi combinazione dei valori seguenti:<ul><li>**n** -alfabeticamente per nome</li><li>**e** -alfabeticamente per estensione</li><li>prima directory del gruppo **g**</li><li>**s** -per dimensione, prima più piccola</li><li>**d** -per data/ora, prima prima</li><li>Utilizzare il **-** prefisso per invertire l'ordinamento</li></ul>Più valori vengono elaborati nell'ordine in cui sono elencate. Non separare più valori con spazi, ma è possibile usare facoltativamente i due punti (:).<p>Se *SortOrder* non è specificato, **dir/o** elenca le directory in ordine alfabetico, seguite dai file, anch ' essi ordinati alfabeticamente. |
| /t[[:]`<timefield>`] | Specifica il campo orario da visualizzare o da usare per l'ordinamento. I valori *TimeField* disponibili sono:<ul><li>creazione di **c**</li><li>**a** -ultimo accesso</li><li>**w** -ultima scrittura</li></ul> |
| /s | Elenca tutte le occorrenze del nome del file specificato in una directory specificata e tutte le sottodirectory. |
| / b | Visualizza un elenco di directory e file, con nessuna informazione aggiuntiva bare. Il parametro **/b** esegue l'override di **/w**. |
| /l | Consente di visualizzare i nomi di file e di directory non ordinati, usando caratteri minuscoli. |
| /n | Visualizza in formato lungo con nomi di file all'estrema destra della schermata. |
| /x | Visualizza i nomi brevi generati per i nomi di file non 8dot3. Il display è uguale a quella del **/n**, ma il nome breve viene inserito prima il nome lungo. |
| /C | Visualizza il separatore delle migliaia nelle dimensioni dei file. Questo è il comportamento predefinito. Utilizzare **/c** per nascondere i separatori. |
| /4 | Visualizzazione degli anni nel formato a quattro cifre. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Per usare più parametri *filename* , separare ogni nome file con uno spazio, una virgola o un punto e virgola.

- È possibile utilizzare caratteri jolly (**&#42;** o **?**) per rappresentare uno o più caratteri di un nome file e per visualizzare un subset di file o sottodirectory.

- È possibile utilizzare il carattere jolly, **&#42;**, per sostituire qualsiasi stringa di caratteri, ad esempio:

  - `dir *.txt`Elenca tutti i file nella directory corrente con estensioni che iniziano con txt, ad esempio. txt,. txt1,. txt_old.

  - `dir read *.txt`Elenca tutti i file nella directory corrente che iniziano con Read e con le estensioni che iniziano con txt, ad esempio. txt,. txt1 o. txt_old.

  - `dir read *.*`Elenca tutti i file nella directory corrente che iniziano con Read con qualsiasi estensione.

  Il carattere jolly asterisco sempre utilizzato mapping nomi file brevi, si potrebbero ottenere risultati imprevisti. Ad esempio, la directory seguente contiene due file (t.txt2 e t97.txt):

  ```
  C:\test>dir /x
  Volume in drive C has no label.
  Volume Serial Number is B86A-EF32

  Directory of C:\test

  11/30/2004  01:40 PM <DIR>  .
  11/30/2004  01:40 PM <DIR> ..
  11/30/2004  11:05 AM 0 T97B4~1.TXT t.txt2
  11/30/2004  01:16 PM 0 t97.txt
  ```

  Ci si potrebbe aspettare che `dir t97\*` la digitazione restituisca il file t97. txt. Tuttavia, digitando `dir t97\*` restituisce entrambi i file, perché il carattere jolly asterisco corrisponde al file t. txt2 a T97. txt usando la mappa nome breve *T97B4 ~ 1. txt*. Analogamente, `del t97\*` la digitazione eliminerà entrambi i file.

- È possibile usare il punto interrogativo (?) come sostituto di un singolo carattere in un nome. Ad esempio, digitando `dir read???.txt` vengono elencati tutti i file nella directory corrente con l'estensione txt che iniziano con Read e sono seguiti da un massimo di tre caratteri. Sono inclusi Read.txt, Read1.txt, Read12.txt, Read123.txt e Readme1.txt, ma non Readme12.txt.

- Se si usa **/a** con più di un valore negli *attributi*, questo comando Visualizza i nomi dei soli file con tutti gli attributi specificati. Se ad esempio si usa **/a** con **r** e **-h** come attributi (usando `/a:r-h` o `/ar-h`), questo comando visualizzerà solo i nomi dei file di sola lettura non nascosti.

- Se si specifica più di un valore *SortOrder* , questo comando Ordina i nomi di file in base al primo criterio, quindi in base al secondo criterio e così via. Se, ad esempio, si usa **/o** con **i parametri e e** **-s** per *SortOrder* (usando `/o:e-s` o `/oe-s`), questo comando Ordina i nomi delle directory e dei file per estensione, con il primo più grande, quindi Visualizza il risultato finale. In ordine alfabetico per estensione determina i nomi di file con estensioni non vengono visualizzati per primi, quindi i nomi di directory e i nomi di file con estensioni.

- Se si`>`usa il simbolo di reindirizzamento () per inviare l'output di questo comando a un file o se si usa una pipe (`|`) per inviare l'output di questo comando a un altro comando, è necessario `/a:-d` usare e **/b** per elencare solo i nomi di file. È possibile utilizzare *filename* con **/b** e **/s** per specificare che questo comando deve eseguire la ricerca nella directory corrente e nelle relative sottodirectory per tutti i nomi file che corrispondono al *nome*file. Questo comando elenca solo la lettera di unità, il nome della directory, il nome del file e l'estensione del nome di file (un percorso per riga), per ogni nome di file trovato. Prima di usare una pipe per inviare l'output di questo comando a un altro comando, è necessario impostare la variabile di ambiente *Temp* nel file Autoexec. NT.

## <a name="examples"></a>Esempi

Per visualizzare tutte le directory, uno dopo l'altro, in ordine alfabetico, nel formato grande e pause tra le schermate, assicurarsi che la directory radice è la directory corrente e quindi digitare:

```
dir /s/w/o/p
```

Nell'output sono elencate la directory radice, le sottodirectory e i file nella directory radice, incluse le estensioni. Questo comando elenca anche i nomi delle sottodirectory e i nomi di file in ogni sottodirectory dell'albero.

Per modificare l'esempio precedente in modo che **dir** Visualizza i nomi di file e le estensioni, ma non i nomi delle directory, tipo:

```
dir /s/w/o/p/a:-d
```

Per stampare un elenco di directory, digitare:

```
dir > prn
```

Quando si specifica **prn**, l'elenco di directory viene inviato alla stampante collegata alla porta LPT1. Se la stampante è collegata a una porta diversa, è necessario sostituire **prn** con il nome della porta corretta.

È possibile reindirizzare l'output del **dir** comando in un file sostituendo **prn** con un nome file. È anche possibile digitare un percorso. Ad esempio, una diretta **dir** per il file Dir. doc nella directory di record, tipo di output:

```
dir > \records\dir.doc
```

Se dir. doc non esiste, **dir** lo crea, a meno che la directory dei **record** non esista. In tal caso, viene visualizzato il messaggio seguente:

```
File creation error
```

Per visualizzare un elenco di tutti i nomi di file con estensione txt in tutte le directory sull'unità C, digitare:

```
dir c:\*.txt /w/o/s/p
```

Il comando **dir** Visualizza, in formato esteso, un elenco in ordine alfabetico dei nomi file corrispondenti in ogni directory e viene sospeso ogni volta che lo schermo si riempie fino a quando non si preme un tasto qualsiasi per continuare.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
