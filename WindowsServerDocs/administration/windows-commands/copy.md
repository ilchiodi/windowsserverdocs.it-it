---
title: copy
description: Argomento di riferimento per il comando copy, che consente di copiare uno o più file da una posizione a un'altra.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9624d4a1-349a-4693-ad00-1d1d4e59e9ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 909bcdf83c87440dafe2653711c4d7e215f8d66b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719293"
---
# <a name="copy"></a>copy

Copia di uno o più file da una posizione a un'altra.

> [!NOTE]
> Dalla console di ripristino è inoltre possibile utilizzare il comando **Copy** con parametri diversi. Per ulteriori informazioni sulla console di ripristino di sistema, vedere [ambiente ripristino Windows (Windows re)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintassi

```
copy [/d] [/v] [/n] [/y | /-y] [/z] [/a | /b] <source> [/a | /b] [+<source> [/a | /b] [+ ...]] [<destination> [/a | /b]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /d | Consente i file crittografati vengono copiati per essere salvato come file decrittografati nella destinazione. |
| /v | Verifica che i nuovi file vengono copiati correttamente. |
| /n | Utilizza un nome di file brevi, se disponibile, quando si copia un file con un nome più di otto caratteri, o con un'estensione di file più di tre caratteri. |
| /y | Evita la richiesta di conferma della sovrascrittura di un file di destinazione esistente. |
| /-y | Viene richiesto di confermare che si desidera sovrascrivere un file di destinazione esistente. |
| /z | Copia i file in rete in modalità riavviabile. |
| /a | Indica un file di testo ASCII. |
| / b | Indica un file binario. |
| `<source>` | Obbligatorio. Specifica il percorso da cui si desidera copiare un file o un set di file. *Origine* può essere costituito da una lettera di unità e i due punti, un nome di directory, un nome di file o una combinazione di questi. |
| `<destination>` | Obbligatorio. Specifica il percorso in cui si desidera copiare un file o un set di file. *Destinazione* può essere costituito da una lettera di unità e i due punti, un nome di directory, un nome di file o una combinazione di questi. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- È possibile copiare un file di testo ASCII che viene utilizzato un carattere di fine del file (CTRL + Z) per indicare la fine del file.

- Se **/a** precede o segue un elenco di file nella riga di comando, si applica a tutti i file elencati fino a quando la **copia** rileva **/b**. In questo caso, **/b** si applica al file precedente **/b**.

    L'effetto di **/a** dipende dalla relativa posizione nella stringa della riga di comando:
      - Se **/a** segue *source*, il comando **Copy** considera il file come un file ASCII e copia i dati che precede il primo carattere di fine del file (CTRL + Z).
      - Se **/a** segue la *destinazione*, il comando **Copy** aggiunge un carattere di fine file (CTRL + Z) come ultimo carattere del file.

- Se **/b** indica all'interprete dei comandi di leggere il numero di byte specificato dalle dimensioni del file nella directory. **/ b** è il valore predefinito per **Copia**, a meno che **Copia** combina i file.

- Se **/b** precede o segue un elenco di file nella riga di comando, si applica a tutti i file elencati finché la **copia** non rileva **/a**. In questo caso, **/a** si applica al file precedente **/a**.

    L'effetto di **/b** dipende dalla relativa posizione nella stringa della riga di comando:-se **/b** segue *source*, il comando **Copy** copia l'intero file, incluso qualsiasi carattere di fine del file (CTRL + Z).
        -Se **/b** segue la *destinazione*, il comando **Copy** non aggiunge un carattere di fine file (CTRL + Z).

- Se non è possibile verificare un'operazione di scrittura, viene visualizzato un messaggio di errore. Anche se si verificano raramente errori di registrazione con il comando **Copy** , è possibile usare **/v** per verificare che i dati critici siano stati registrati correttamente. Il **/v** opzione della riga di comando rallenta il **Copia** comando, in quanto ciascun settore registrato sul disco deve essere selezionata.

- Se **/y** è preimpostato nella variabile di ambiente **COPYCMD** , è possibile eseguire l'override di questa impostazione usando **/-y** nella riga di comando. Per impostazione predefinita, viene richiesto quando si sostituisce questa impostazione, a meno che il **Copia** comando viene eseguito in uno script batch.
  
- Per accodare i file, specificare un singolo file per la *destinazione*, ma più file per l' *origine* (usare caratteri jolly o *file1*+*file2*+*file3* Format).

- Se la connessione viene persa durante la fase di copia, ad esempio se il server passa alla modalità offline interrompe la connessione, è possibile usare **Copy/z** per riprendere una volta ristabilita la connessione. L'opzione **/z** Visualizza anche la percentuale dell'operazione di copia completata per ogni file.

- È possibile sostituire il nome di un dispositivo con una o più occorrenze dell' *origine* o della *destinazione*.

- Se la *destinazione* è un dispositivo (ad esempio, COM1 o LPT1), l'opzione **/b** copia i dati nel dispositivo in modalità binaria. In modalità binaria, **copy/b** copia tutti i caratteri, inclusi i caratteri speciali, ad esempio CTRL + C, CTRL + S, CTRL + Z e invio, al dispositivo come dati. Tuttavia, se si omette **/b**, i dati vengono copiati nel dispositivo in modalità ASCII. In modalità ASCII, caratteri speciali potrebbe di combinare durante il processo di copia file.

- Se non si specifica un file di destinazione, viene creata una copia con lo stesso nome, la data di modifica e l'ora di modifica del file originale. La nuova copia viene archiviata nella directory corrente dell'unità corrente. Se il file di origine è l'unità corrente e nella directory corrente e non si specifica un'unità diversa o una directory del file di destinazione, il **Copia** comando Arresta e viene visualizzato il messaggio di errore seguente:

    ```
    File cannot be copied onto itself
    0 File(s) copied
    ```

- Se si specifica più di un file nell' *origine*, il comando **Copy** li combina in un unico file usando il nome file specificato nella *destinazione*. Il comando **Copy** presuppone che i file combinati siano file ASCII a meno che non si usi l'opzione **/b** .

- Per copiare i file con una lunghezza pari a 0 byte oppure per copiare tutti i file e le sottodirectory di una directory, utilizzare il [comando xcopy](xcopy.md).

- Per assegnare l'ora e la data correnti a un file senza modificare il file, usare la sintassi seguente:  

    ```
    copy /b <source> +,,
    ```

    Dove la virgola indica che il parametro di *destinazione* è stato intenzionalmente escluso.

## <a name="examples"></a>Esempi

Per copiare un file denominato *Memo. doc* in *Letter. doc* nell'unità corrente e assicurarsi che un carattere di fine file (CTRL + Z) si trovi alla fine del file copiato, digitare:

```
copy memo.doc letter.doc /a
```

Per copiare un file denominato *Robin. Typ* dall'unità e dalla directory correnti a una directory esistente denominata *Birds* che si trova sull'unità C, digitare:

```
copy robin.typ c:\birds
```

> [!NOTE]
> Se la directory *Birds* non esiste, il file *Robin. Typ* viene copiato in un file denominato *Birds* che si trova nella directory radice del disco nell'unità C.

Per combinare *mar89. rpt*, *apr89. rpt*e *May89. rpt*, che si trovano nella directory corrente e inserirli in un file denominato *report* (anche nella directory corrente), digitare:

```
copy mar89.rpt + apr89.rpt + may89.rpt Report
```

> [!NOTE]
> Se si combinano i file, il comando **Copy** contrassegna il file di destinazione con la data e l'ora correnti. Se si omette la *destinazione*, i file vengono combinati e archiviati con il nome del primo file nell'elenco.

Per combinare tutti i file nel *report*, quando un file denominato *report* esiste già, digitare:

```
copy report + mar89.rpt + apr89.rpt + may89.rpt
```

Per combinare tutti i file nella directory corrente con estensione txt in un singolo file denominato *Combined. doc*, digitare:

```
copy *.txt Combined.doc
```

Per combinare più file binari in un unico file utilizzando caratteri jolly, includere **/b**. In tal modo considerando CTRL + Z come carattere di fine del file. Ad esempio, digitare il comando seguente:

```
copy /b *.exe Combined.exe
```

> [!CAUTION]
> Se si combinano file binari, il file risulta potrebbe essere inutilizzabile a causa di formattazione interna.

- La combinazione di ogni file con estensione txt con il corrispondente file. Ref crea un file con lo stesso nome, ma con estensione doc. Il **comando copy** combina *file1. txt* con *file1. Ref* per formare *file1. doc*, quindi il comando combina *file2. txt* con file2. *ref* per formare *file2. doc*e così via. Ad esempio, digitare il comando seguente:

```
copy *.txt + *.ref *.doc
```

Per combinare tutti i file con estensione txt e quindi combinare tutti i file con estensione. Ref in un unico file denominato *Combined. doc*, digitare:

```
copy *.txt + *.ref Combined.doc
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando xcopy](xcopy.md)
