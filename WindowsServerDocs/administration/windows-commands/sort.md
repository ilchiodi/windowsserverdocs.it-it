---
title: sort
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 65b091a6de4f20ce94389ed39f4fe645c72b3560
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383947"
---
# <a name="sort"></a>sort



Legge l'input, Ordina i dati e scrive i risultati sullo schermo, in un file o in un altro dispositivo.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
sort [/r] [/+<N>] [/m <Kilobytes>] [/l <Locale>] [/rec <Characters>] [[<Drive1>:][<Path1>]<FileName1>] [/t [<Drive2>:][<Path2>]] [/o [<Drive3>:][<Path3>]<FileName3>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/r|Inverte l'ordinamento, ovvero ordina da Z a A e da 9 a 0.|
|/+ @ NO__T-1N >|Specifica il numero di posizione del carattere in cui l' **ordinamento** inizierà ogni confronto. *N* può essere qualsiasi numero intero valido.|
|/m \<Kilobytes >|Specifica la quantità di memoria principale da usare per l'ordinamento in kilobyte (KB).|
|/l \<Locale >|Esegue l'override dell'ordinamento dei caratteri definiti dalle impostazioni locali predefinite del sistema, ovvero la lingua e il paese selezionati durante l'installazione.|
|/REC \<Characters >|Specifica il numero massimo di caratteri in un record o una riga del file di input (il valore predefinito è 4.096 e il valore massimo è 65.535).|
|[\<Drive1 >:] [\<Path1 >] \<FileName1 >|Specifica il file da ordinare. Se non viene specificato alcun nome file, l'input standard viene ordinato. La specifica del file di input è più veloce rispetto al reindirizzamento dello stesso file come input standard.|
|/t [\<Drive2 >:] [\<Path2 >]|Specifica il percorso della directory in cui memorizzare l'archiviazione funzionante del comando di **ordinamento** se i dati non rientrano nella memoria principale. Per impostazione predefinita, viene usata la directory temporanea di sistema.|
|/o [\<Drive3 >:] [\<Path3 >] \<FileName3 >|Specifica il file in cui deve essere archiviato l'input ordinato. Se non specificato, i dati vengono scritti nell'output standard. La specifica del file di output è più veloce rispetto al reindirizzamento dell'output standard allo stesso file.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Uso dell'opzione della riga di comando **/+**

    Per impostazione predefinita, i confronti iniziano in corrispondenza del primo carattere di ogni riga. L'opzione della riga di comando **/+** avvia i confronti con il carattere specificato da *N*. Ad esempio, `/+3` indica che ogni confronto deve iniziare in corrispondenza del terzo carattere di ogni riga. Le righe con meno di *N* caratteri vengono confrontate prima di altre righe.
-   Uso dell'opzione della riga di comando **/m**

    La memoria utilizzata è sempre un minimo di 160 KB. Se viene specificata la dimensione della memoria, per l'ordinamento viene utilizzata l'esatta quantità specificata (deve essere almeno 160 KB), indipendentemente dalla quantità di memoria disponibile.

    Le dimensioni massime predefinite della memoria se non si specifica alcuna dimensione sono pari al 90% della memoria principale disponibile se l'input e l'output sono file o il 45% della memoria principale in caso contrario. L'impostazione predefinita offre in genere le prestazioni migliori.
-   Utilizzo dell'opzione della riga di comando **/l**

    Attualmente, l'unica alternativa alle impostazioni locali predefinite è costituita dalle impostazioni locali "C", più veloci rispetto all'ordinamento del linguaggio naturale (Ordina i caratteri in base alle relative codifiche binarie).
-   Utilizzo di simboli di reindirizzamento con il comando **Sort**

    È possibile usare il simbolo di barra verticale ( **|** ) per indirizzare i dati di input al comando **Sort** da un altro comando o per indirizzare l'output ordinato a un altro comando. È possibile specificare i file di input e di output usando i simboli di reindirizzamento ( **<** o **>** ). Può essere più veloce ed efficiente (specialmente con file di grandi dimensioni) per specificare il file di input direttamente (come definito da *filename1* nella sintassi del comando), quindi specificare il file di output usando il parametro **/o** .
-   Distinzione maiuscole/minuscole

    Il comando **Sort** non distingue tra lettere maiuscole e minuscole.
-   Limiti sulle dimensioni dei file

    Il comando **Sort** non prevede alcun limite per le dimensioni del file.
-   Sequenza di fascicolazione

    Il programma di ordinamento usa la tabella COLLATE-sequenza che corrisponde al codice paese e alle impostazioni della tabella codici. I caratteri maggiori del codice ASCII 127 vengono ordinati in base alle informazioni contenute nel file Country. sys o in un file alternativo specificato dal comando **Country** nel file config. NT.
-   Utilizzo della memoria

    Se l'ordinamento rientra nelle dimensioni massime della memoria (impostate per impostazione predefinita o come specificato dal parametro **/m** ), l'ordinamento viene eseguito in un singolo passaggio. In caso contrario, l'ordinamento viene eseguito in due passaggi di ordinamento e merge distinti e le quantità di memoria utilizzata per entrambi i passaggi sono uguali. Quando vengono eseguite due sessioni, i dati parzialmente ordinati vengono archiviati in un file temporaneo su disco. Se la memoria disponibile non è sufficiente per eseguire l'ordinamento in due passaggi, viene generato un errore di run-time. Se l'opzione della riga di comando **/m** viene utilizzata per specificare una quantità di memoria superiore a quella effettivamente disponibile, è possibile che si verifichi un calo delle prestazioni o un errore in fase di esecuzione.

## <a name="BKMK_examples"></a>Esempi

**Ordinamento di un file**

Per ordinare e visualizzare in ordine inverso le righe in un file denominato expenses. txt, digitare:

`sort /r expenses.txt`

**Ordinamento dell'output di un comando**

Per eseguire la ricerca in un file di grandi dimensioni denominato Mailer. txt per il testo "Jones" e per ordinare i risultati della ricerca, utilizzare la pipe (|) per indirizzare l'output di un comando **Find** al comando **Sort** , come indicato di seguito:

`find "Jones" maillist.txt | sort`

Il comando genera un elenco ordinato di righe che contengono il testo specificato.

**Ordinamento degli input da tastiera**

Per ordinare l'input da tastiera e visualizzare i risultati in ordine alfabetico sullo schermo, è possibile usare prima il comando **Sort** senza parametri, come indicato di seguito:

`sort`

Digitare quindi il testo che si desidera ordinare e premere INVIO alla fine di ogni riga. Al termine della digitazione del testo, premere CTRL + Z, quindi premere INVIO. Il comando **Sort** Visualizza il testo digitato, ordinata alfabeticamente.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)