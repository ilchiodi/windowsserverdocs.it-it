---
title: copy
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9624d4a1-349a-4693-ad00-1d1d4e59e9ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d593fbdbffd2a5ee4e4dfb4a817ad4708162160a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853772"
---
# <a name="copy"></a>copy



Copia di uno o più file da una posizione a un'altra.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
copy [/d] [/v] [/n] [/y | /-y] [/z] [/a | /b] <Source> [/a | /b] [+<Source> [/a | /b] [+ ...]] [<Destination> [/a | /b]]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/d|Consente i file crittografati vengono copiati per essere salvato come file decrittografati nella destinazione.|
|/v|Verifica che i nuovi file vengono copiati correttamente.|
|/n|Utilizza un nome di file brevi, se disponibile, quando si copia un file con un nome più di otto caratteri, o con un'estensione di file più di tre caratteri.|
|/y|Evita la visualizzazione verrà richiesto di confermare che si desidera sovrascrivere un file di destinazione esistente.|
|/-y|Viene richiesto di confermare che si desidera sovrascrivere un file di destinazione esistente.|
|/z|Copia i file in rete in modalità riavviabile.|
|/a|Indica un file di testo ASCII.|
|/ b|Indica un file binario.|
|\<origine >|Obbligatorio. Specifica il percorso da cui si desidera copiare un file o un set di file. *Origine* può essere costituito da una lettera di unità e i due punti, un nome di directory, un nome di file o una combinazione di questi.|
|\<Destinazione >|Obbligatorio. Specifica il percorso in cui si desidera copiare un file o un set di file. *Destinazione* può essere costituito da una lettera di unità e i due punti, un nome di directory, un nome di file o una combinazione di questi.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   È possibile copiare un file di testo ASCII che viene utilizzato un carattere di fine del file (CTRL + Z) per indicare la fine del file.
-   Utilizzando **/a**

    Quando **/a** precede o segue un elenco di file nella riga di comando, si applica a tutti i file elencati fino **Copia** rileva **/b**. In questo caso, **/b** si applica al file precedente **/b**.

    L'effetto di **/a** dipende dalla relativa posizione nella stringa della riga di comando. Quando **/a** segue *origine*, **Copia** gestisce il file come file ASCII e copia i dati che precede il primo carattere di fine del file (CTRL + Z).

    Quando **/a** segue *destinazione*, **Copia** aggiunge un carattere di fine del file (CTRL + Z) come l'ultimo carattere del file.
-   Utilizzando **/b**

    **/ b** indirizza l'interprete dei comandi per leggere il numero di byte specificato dalla dimensione del file nella directory. **/ b** è il valore predefinito per **Copia**, a meno che **Copia** combina i file.

    Quando **/b** precede o segue un elenco di file nella riga di comando, si applica a tutti i file elencati fino **Copia** rileva **/a**. In questo caso, **/a** si applica al file precedente **/a**.

    L'effetto di **/b** dipende dalla relativa posizione nella stringa di riga di comando. Quando **/b** segue *origine*, **Copia** Copia l'intero file, incluso il carattere di fine file (CTRL + Z).

    Quando **/b** segue *destinazione*, **Copia** aggiunge un carattere di fine del file (CTRL + Z).
-   Utilizzando **/v**

    Se non è verificata un'operazione di scrittura che viene visualizzato un messaggio di errore. Anche se si verificano raramente errori di registrazione con **Copia**, è possibile utilizzare **/v** per verificare che i dati critici sono stati registrati correttamente. Il **/v** opzione della riga di comando rallenta il **Copia** comando, in quanto ciascun settore registrato sul disco deve essere selezionata.
-   Utilizzando **/y** e **/-y**

    Se **/y** è preimpostato nella variabile di ambiente COPYCMD, è possibile ignorare questa impostazione utilizzando **/-y** nella riga di comando. Per impostazione predefinita, viene richiesto quando si sostituisce questa impostazione, a meno che il **Copia** comando viene eseguito in uno script batch.
-   Aggiunta di file

    Per aggiungere file, specificare un singolo file per *destinazione*, ma i file più *origine* (usare caratteri jolly o *File1*+*File2*+*File3* formato).
-   Utilizzando **/z**

    Se la connessione viene persa durante la fase di copia (ad esempio, se il server si interrompe la connessione), **copiare /z** riprende dopo la connessione viene ristabilita. **/z** inoltre visualizza la percentuale dell'operazione di copia che viene completata per ogni file.
-   La copia verso e dai dispositivi

    È possibile sostituire un nome di dispositivo per una o più occorrenze di *origine* o *destinazione*.
-   Utilizzo o omissione **/b** quando si copia in un dispositivo

    Quando *destinazione* è un dispositivo, ad esempio Com1 o Lpt1, **/b** Copia i dati per il dispositivo in modalità binaria. In modalità binaria, **copiare/b** Copia tutti i caratteri (compresi i caratteri speciali, ad esempio CTRL + C, CTRL + S, CTRL + Z e INVIO) per il dispositivo come dati. Tuttavia, se si omette **/b**, i dati vengono copiati il dispositivo in modalità ASCII. In modalità ASCII, caratteri speciali potrebbe di combinare durante il processo di copia file.
-   Utilizzare il file di destinazione predefinito

    Se non si specifica un file di destinazione, una copia viene creata con lo stesso nome, data di modifica e modifica del file originale. La nuova copia viene archiviata nella directory corrente dell'unità corrente. Se il file di origine è l'unità corrente e nella directory corrente e non si specifica un'unità diversa o una directory del file di destinazione, il **Copia** comando Arresta e viene visualizzato il messaggio di errore seguente:

    `File cannot be copied onto itself`

    `0 File(s) copied`
-   Unione di file

    Se si specifica più di un file in *origine*, **Copia** li combina in un singolo file mediante il nome file specificato in *destinazione*. **Copia** presuppone che i file uniti siano file ASCII a meno che non si utilizza il **/b** (opzione).
-   La copia dei file di lunghezza zero

    **Copia** non copia i file che sono di 0 byte. Utilizzare **xcopy** per copiare i file.
-   Modifica di un file di data e ora

    Se si desidera assegnare l'ora corrente e la data in un file senza modificare il file, utilizzare la sintassi seguente:  
    ```
    copy /b <Source> +,,
    ```  
    Le virgole indicano l'omissione del *destinazione* parametro.
-   La copia dei file nelle sottodirectory

    Per copiare tutti i file e le sottodirectory della directory, utilizzare il **xcopy** comando.
-   Il **Copia** comando con parametri diversi, è disponibile dalla Console di ripristino.

## <a name="BKMK_examples"></a>Esempi

Per copiare un file denominato note per doc nell'unità corrente e verificare che sia un carattere di fine del file (CTRL + Z) alla fine del file copiato, digitare:
```
copy memo.doc letter.doc /a
```
Per copiare il file Cocker dall'unità corrente e directory in una directory esistente nome che si trova sull'unità C, digitare:
```
copy robin.typ c:\birds
```
Se la directory non esiste, il file Cocker viene copiato in un file con nome che si trova nella directory radice sul disco nell'unità C.

Per combinare Mar89 Apr89 e Mag89. rpt, che si trovano nella directory corrente e li inserisce in un file denominato Report (anche nella directory corrente), digitare:
```
copy mar89.rpt + apr89.rpt + may89.rpt Report
```
Quando si combinano file **Copia** Contrassegna il file di destinazione con la data e ora correnti. Se si omette *destinazione*, i file vengono combinati e archiviati con il nome del primo file nell'elenco. Ad esempio, per combinare tutti i file di Report quando esiste già un file denominato Report, digitare:
```
copy report + mar89.rpt + apr89.rpt + may89.rpt
```
Per combinare tutti i file nella directory corrente che the.txt estensione in un unico file denominato Combined.doc, tipo:
```
copy *.txt Combined.doc 
```
Se si desidera unire più file binari in un unico file utilizzando caratteri jolly, includere **/b**. In tal modo considerando CTRL + Z come carattere di fine del file. Ad esempio, digitare il comando seguente:
```
copy /b *.exe Combined.exe
```

> [!CAUTION]
> Se si combinano file binari, il file risulta potrebbe essere inutilizzabile a causa di formattazione interna.

Nell'esempio seguente, **Copia** combina ogni file con estensione txt con il corrispondente file ref. Il risultato è un file con lo stesso nome ma con estensione doc. **Copia** combina file1. txt con File1.ref a file1 e quindi **Copia** combina file2 con File2.ref form File2.doc e così via. Ad esempio, digitare il comando seguente:
```
copy *.txt + *.ref *.doc
```
Per combinare tutti i file con estensione txt e quindi combinare tutti i file con estensione ref in un file denominato Combined.doc, digitare:
```
copy *.txt + *.ref Combined.doc
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)