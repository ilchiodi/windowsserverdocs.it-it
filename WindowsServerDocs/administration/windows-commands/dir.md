---
title: dir
description: Windows Commands Topic for dir, che visualizza un elenco dei file e delle sottodirectory di una directory.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: edcbf69b-eaa4-466e-b210-3dd8892f4d93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44e50707886df87b217f22bc04edcdaf7496b0d1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845564"
---
# <a name="dir"></a>dir

Visualizza un elenco di file e le sottodirectory della directory. Se utilizzata senza parametri, **dir** Visualizza l'etichetta di volume del disco e numero di serie, seguito da un elenco di directory e file sul disco (inclusi i relativi nomi e i data e ora dell'ultima modifica ogni). Per i file, **dir** consente di visualizzare l'estensione del nome e la dimensione in byte. **Dir** inoltre visualizza il numero totale di file e directory elencate, la quantità di memoria e lo spazio libero sul disco (in byte).

Per esempi di utilizzo di questo comando, vedere [Esempi](#examples).

## <a name="syntax"></a>Sintassi

```
dir [<Drive>:][<Path>][<FileName>] [...] [/p] [/q] [/w] [/d] [/a[[:]<Attributes>]][/o[[:]<SortOrder>]] [/t[[:]<TimeField>]] [/s] [/b] [/l] [/n] [/x] [/c] [/4]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[\<unità >:] [<Path>]|Specifica l'unità e directory per il quale si desidera visualizzare un elenco.|
|[\<nomefile >]|Specifica un file specifico o un gruppo di file per il quale si desidera visualizzare un elenco.|
|/p|Visualizza una schermata dell'elenco alla volta. Per visualizzare la schermata successiva, premere un tasto sulla tastiera.|
|/q|Visualizza informazioni sulla proprietà di file.|
|/w|Visualizza l'elenco in formato esteso, con un massimo di cinque nomi di file o i nomi di directory in ogni riga.|
|/d|Visualizza l'elenco nello stesso formato **/w**, ma i file vengono ordinati per colonna.|
|/a [[:]\<attributi >]|Visualizza solo i nomi delle directory e file con gli attributi specificati. Se si omette **/a**, **dir** Visualizza i nomi di tutti i file tranne nascosti e i file di sistema. Se si utilizza **/a** senza specificare *attributi*, **dir** Visualizza i nomi di tutti i file, tra cui nascosti e i file di sistema.</br>Nell'elenco seguente sono descritti i valori che è possibile utilizzare per *attributi*. Utilizzo di due punti (:) è facoltativo. Utilizzare una combinazione qualsiasi di questi valori e non separare i valori con spazi.</br>**d** Directory</br>**h** file nascosti</br>**s** i file di sistema</br>**l** Reparse Point</br>**r** i file di sola lettura</br>**un** pronto per l'archiviazione dei file</br>**i** non contenuti i file indicizzati</br>**-** Il prefisso non significa|
|/o [[:]\<SortOrder >]|Ordina l'output in base a *SortOrder*, che può essere qualsiasi combinazione dei valori seguenti:</br>**n** in base al nome (in ordine alfabetico)</br>**e** dall'estensione (in ordine alfabetico)</br>**g** prima alla directory di gruppo</br>**s** per dimensioni (il primo più piccolo)</br>**d** da data/ora (più recente)</br>**-** Prefisso per l'ordine inverso</br>Nota: l'uso di due punti è facoltativo. Più valori vengono elaborati nell'ordine in cui sono elencate. Non separare più valori con uno spazio.</br>Se *SortOrder* non è specificato, **/o dir** Elenca le directory in ordine alfabetico, seguito dai file, che devono essere disposti in ordine alfabetico.|
|/t [[:]\<TimeField >]|Specifica il campo ora per visualizzare o utilizzare per l'ordinamento. Nell'elenco seguente sono descritti i valori è possibile utilizzare per *ora*:</br>**c** creazione</br>**un** ultimo accesso</br>**w** ultima scrittura|
|/s|Elenca tutte le occorrenze del nome del file specificato in una directory specificata e tutte le sottodirectory.|
|/ b|Visualizza un elenco di directory e file, con nessuna informazione aggiuntiva bare. **/ b** sostituzioni **/w**.|
|/l|Visualizza i nomi di directory e dei file in minuscolo.|
|/n|Visualizza in formato lungo con nomi di file all'estrema destra della schermata.|
|/x|Visualizza i nomi brevi generati per i nomi di file non 8dot3. Il display è uguale a quella del **/n**, ma il nome breve viene inserito prima il nome lungo.|
|/c|Visualizza il separatore delle migliaia nelle dimensioni dei file. Questo è il comportamento predefinito. Utilizzare **/c** per nascondere i separatori.|
|/4|Visualizzazione degli anni nel formato a quattro cifre.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

- L'utilizzo di più *FileName* parametri, separare ogni nome di file con uno spazio, virgola o punto e virgola.
- È possibile utilizzare caratteri jolly ( **&#42;** o **?** ) per rappresentare uno o più caratteri di un nome file e per visualizzare un subset di file o sottodirectory.

  **Asterisco (\*):** utilizzare l'asterisco come sostituto di qualsiasi stringa di caratteri, ad esempio:  
  - **dir \*. txt** Elenca tutti i file nella directory corrente con le estensioni che iniziano con estensione txt, ad esempio con estensione txt, .txt1, .txt_old.
  - **dir leggere\*. txt** elenca tutti i file nella directory corrente che iniziano con Read e con le estensioni che iniziano con txt, ad esempio. txt,. txt1 o. txt_old.
  - **dir leggere\*.\*** elenca tutti i file nella directory corrente che iniziano con Read con qualsiasi estensione.

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

  Ci si potrebbe aspettare che digitando **dir t97\\** * venga restituito il file t97. txt. Tuttavia, digitando **dir t97\\** * vengono restituiti entrambi i file, perché il carattere jolly asterisco corrisponde al file t. txt2 a T97. txt usando la mappa nome breve T97B4 ~ 1. txt. Analogamente, la digitazione **del t97\\** * eliminerà entrambi i file.

  **Punto interrogativo (?):** utilizzare il punto interrogativo come sostituto di un singolo carattere in un nome. Ad esempio, digitare **dir read???. txt** elenca tutti i file nella directory corrente con l'estensione txt che iniziano con Read e sono seguiti da un massimo di tre caratteri. Sono inclusi Read.txt, Read1.txt, Read12.txt, Read123.txt e Readme1.txt, ma non Readme12.txt.
- Specifica degli attributi di visualizzazione del file

  Se si utilizza **/a** con più di un valore negli *attributi*, **dir** Visualizza i nomi dei soli file con tutti gli attributi specificati. Ad esempio, se si utilizza **/a** con **r** e **-h** come attributi (utilizzando **/a: r-h** o **/ar-h verranno**), **dir** verrà visualizzato solo i nomi dei file di sola lettura che non sono nascosti.
- Impostazione dell'ordinamento del nome file

  Se si specifica più di un valore *SortOrder* , **dir** Ordina i nomi di file in base al primo criterio, quindi in base al secondo criterio e così via. Ad esempio, se si utilizza **/o** con il **e** e **-s** i valori del parametro *SortOrder* (utilizzando **/o: e-s** o **/oe-s**), **dir** Ordina i nomi di directory e file per estensione, con il primo più grande e viene visualizzato il risultato finale. In ordine alfabetico per estensione determina i nomi di file con estensioni non vengono visualizzati per primi, quindi i nomi di directory e i nomi di file con estensioni.
- Uso di simboli di reindirizzamento e pipe

  Quando si usa il simbolo di reindirizzamento ( **>** ) per inviare l'output **dir** a un file o una pipe ( **|** ) per inviare l'output **dir** a un altro comando, usare **/a:-d** e **/b** per elencare solo i nomi file. È possibile utilizzare *FileName* con **/b** e **/s** per specificare che **dir** ricerca nella directory corrente e nelle relative sottodirectory per tutti i file che corrispondono a *FileName*. **Dir** Elenca solo la lettera di unità, nome della directory, nome file ed estensione (un percorso per riga), per ogni file denominarlo trova. Prima di utilizzare una pipe per inviare **dir** output a un altro comando, è necessario impostare posizione TEMPORANEA di variabile di ambiente nel file Autoexec.
- Il **dir** comando con parametri diversi, è disponibile dalla Console di ripristino.

## <a name="examples"></a>Esempi

Per visualizzare tutte le directory, uno dopo l'altro, in ordine alfabetico, nel formato grande e pause tra le schermate, assicurarsi che la directory radice è la directory corrente e quindi digitare:

```
dir /s/w/o/p
```

Il comando **dir** elenca la directory radice, le sottodirectory e i file nella directory radice, incluse le estensioni. Quindi, **dir** Elenca i nomi delle sottodirectory e file in ogni sottodirectory nella struttura.

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

Se non esiste Dir. doc, **dir** Crea, a meno che la directory di record non esiste. In tal caso, viene visualizzato il messaggio seguente:

`File creation error`

Per visualizzare un elenco di tutti i nomi di file con estensione txt in tutte le directory sull'unità C, digitare:

```
dir c:\*.txt /w/o/s/p
```

Il comando **dir** Visualizza, in formato esteso, un elenco in ordine alfabetico dei nomi file corrispondenti in ogni directory e viene sospeso ogni volta che lo schermo si riempie fino a quando non si preme un tasto qualsiasi per continuare.

## <a name="additional-references"></a>Altre informazioni di riferimento

- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
