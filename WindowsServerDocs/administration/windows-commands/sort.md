---
title: sort
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 77116469-4790-4442-8a21-9fa73b65ef9f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d38beef74156d9d57b947883c542c2c7e971e00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854752"
---
# <a name="sort"></a>sort



Legge l'input, i dati vengono ordinati e scrive i risultati sullo schermo, in un file o a un altro dispositivo.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
sort [/r] [/+<N>] [/m <Kilobytes>] [/l <Locale>] [/rec <Characters>] [[<Drive1>:][<Path1>]<FileName1>] [/t [<Drive2>:][<Path2>]] [/o [<Drive3>:][<Path3>]<FileName3>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/r|Inverte l'ordine di ordinamento (vale a dire, Ordina da Z ad A e da 9 a 0).|
|/+\<N>|Specifica il numero di posizione del carattere in cui **ordinamento** inizierà ogni confronto. *N* può essere qualsiasi numero intero valido.|
|/m \<Kilobytes>|Specifica la quantità di memoria principale da usare per l'ordinamento nel kilobyte (KB).|
|/l \<Locale>|Esegue l'override dell'ordinamento dei caratteri definite dalle impostazioni locali predefinite del sistema (vale a dire, la lingua e paese/area geografica selezionata durante l'installazione).|
|/REC \<caratteri >|Specifica il numero massimo di caratteri in un record o una riga del file di input (il valore predefinito è 4.096 e il valore massimo è 65.535).|
|[\<Drive1>:][\<Path1>]\<FileName1>|Specifica il file da ordinare. Se viene specificato alcun nome file, l'input standard viene ordinato. Specifica il file di input è più veloce di reindirizzamento stesso file di input standard.|
|/t [\<Drive2>:][\<Path2>]|Specifica il percorso della directory per contenere il **ordinamento** comando lavoro dell'archiviazione se i dati non rientrano nella memoria principale. Per impostazione predefinita, viene usata la directory temporanea di sistema.|
|/o [\<Drive3>:][\<Path3>]\<FileName3>|Specifica il file in cui viene archiviato l'input ordinato. Se non specificato, i dati vengono scritti nell'output standard. Specifica il file di output è più veloce di reindirizzamento dell'output standard dello stesso file.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Usando il **/+** opzione della riga di comando

    Per impostazione predefinita, i confronti iniziano in corrispondenza del primo carattere di ogni riga. Il **/+** opzione della riga di comando consente di avviare i confronti in corrispondenza del carattere specificato da *N*. Ad esempio, `/+3` indica che ogni confronto deve iniziare in corrispondenza del terzo carattere di ogni riga. Le righe con meno *N* caratteri collate prima delle altre righe.
-   Usando il **/m** opzione della riga di comando

    La memoria utilizzata è sempre un minimo di 160 KB. Se le dimensioni della memoria viene specificata, la quantità esatta specificata viene utilizzata per l'ordinamento (deve essere almeno 160 KB), indipendentemente dalla quantità di memoria principale è disponibile.

    La dimensione massima di memoria predefinita quando non viene specificata alcuna dimensione è in caso contrario, il 90% della memoria principale disponibile se l'input e output sono file o al 45% della memoria principale. In genere l'impostazione predefinita offre prestazioni migliori.
-   Usando il **/l** opzione della riga di comando

    Attualmente, l'unica alternativa per le impostazioni locali predefinite sia le impostazioni locali "C", che è più veloce che ordinare il linguaggio naturale (ordinano i caratteri in base al relativo codice binario).
-   Utilizzo di simboli di reindirizzamento con le **ordinamento** comando

    È possibile usare il simbolo di barra verticale (**|**) per indirizzare i dati di input per il **ordinamento** comando da un altro comando o per indirizzare l'output ordinato per un altro comando. È possibile specificare file di input e output con i simboli di reindirizzamento (**<** oppure **>**). Può risultare più veloce e più efficienti (in particolare con file di grandi dimensioni) per specificare direttamente il file di input (come definito da *FileName1* nella sintassi del comando), quindi specificare il file di output usando la **/o** parametro.
-   Distinzione maiuscole/minuscole

    Il **ordinamento** comando non fa distinzione tra lettere maiuscole e minuscole.
-   Limiti sulle dimensioni del file

    Il **ordinamento** comando non presenta alcun limite sulla dimensione del file.
-   Sequenza di ordinamento

    Il programma di ordinamento utilizzata la tabella di sequenza di confronto che corrisponde alle impostazioni di code e tabelle codici paese/area geografica. Maggiore di codice ASCII 127 caratteri vengono ordinati in base alle informazioni in file country. sys o in un file alternativo specificato dal **paese** comando nel file config.
-   Utilizzo della memoria

    Se l'ordinamento rientra nelle dimensioni massime memoria massima (secondo l'impostazione predefinita o come specificato dalle **/m** parametro), l'ordinamento viene eseguito in un unico passaggio. In caso contrario, l'ordinamento viene eseguito in due passaggi distinti, ordinamento e di tipo merge e la quantità di memoria utilizzata per entrambe le sessioni sono uguali. Quando vengono eseguite due passaggi, i dati ordinati parzialmente vengono archiviati in un file temporaneo su disco. Se non vi è memoria sufficiente per eseguire l'ordinamento in due passaggi, viene generato un errore di run-time. Se il **/m** opzione della riga di comando viene utilizzato per specificare più memoria rispetto a quella effettivamente disponibile, la riduzione delle prestazioni o può verificarsi un errore di run-time.

## <a name="BKMK_examples"></a>Esempi

**Ordinamento di un file**

Per ordinare e visualizzare le righe in ordine inverso in un file denominato txt, digitare:

`sort /r expenses.txt`

**Ordinare l'output da un comando**

Per cercare un file di grandi dimensioni si per il testo "Jones" e per ordinare i risultati della ricerca, utilizzare la barra verticale (|) per indirizzare l'output di un **trovare** comando per il **ordinamento** comando, come segue:

`find "Jones" maillist.txt | sort`

Il comando genera un elenco ordinato di righe che contengono il testo specificato.

**L'ordinamento di input da tastiera**

Per ordinare gli input da tastiera e visualizzare i risultati in ordine alfabetico sullo schermo, è possibile usare la **ordinamento** comando senza parametri, come segue:

`sort`

Quindi digitare il testo che si desidera ordinati e preme INVIO alla fine di ogni riga. Al termine della digitazione di testo, premere CTRL + Z e quindi premere INVIO. Il **ordinamento** comando Visualizza il testo digitato, ordinati in ordine alfabetico.

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)