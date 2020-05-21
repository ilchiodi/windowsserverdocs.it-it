---
title: FC
description: Argomento di riferimento per il comando FC, che confronta due file o set di file e visualizza le differenze tra di essi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 485fc3d8-b7c5-496d-87be-0011112f27d5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 146ae51334f40284e15c2a4564de8dd04660bf25
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437156"
---
# <a name="fc"></a>FC

Confronta due file o gruppi di file e visualizza le differenze esistenti tra di essi.

## <a name="syntax"></a>Sintassi

```
fc /a [/c] [/l] [/lb<n>] [/n] [/off[line]] [/t] [/u] [/w] [/<nnnn>] [<drive1>:][<path1>]<filename1> [<drive2>:][<path2>]<filename2>
fc /b [<drive1:>][<path1>]<filename1> [<drive2:>][<path2>]<filename2>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /a | Abbrevia l'output di un confronto ASCII. Invece di visualizzare tutte le righe che sono diverse, **fc** viene visualizzata solo la riga e il cognome di ogni gruppo di differenze. |
| / b | Confronta i due file in modalità binaria, byte per byte e non tenta di sincronizzare nuovamente i file dopo aver trovato una mancata corrispondenza. Si tratta della modalità predefinita per il confronto tra i file con le seguenti estensioni: .exe,. com, sys, obj,. lib o bin. |
| /C | Ignora le maiuscole/minuscole. |
| /l | Confronta i file in modalità ASCII, riga per riga e tenta di sincronizzare nuovamente i file dopo aver trovato una mancata corrispondenza. Si tratta della modalità predefinita per il confronto dei file, ad eccezione dei file con le seguenti estensioni: .exe,. com, sys, obj,. lib o bin. |
| /lb`<n>` | Imposta il numero di righe per il buffer di riga interno su *N*. La lunghezza predefinita del buffer di riga è 100 righe. Se i file che si desidera confrontare più di 100 righe diverse volte consecutive, **fc** annullerà il confronto. |
| /n | Visualizza i numeri di riga durante un confronto ASCII. |
| / [offline] | Non ignora i file in cui è impostato l'attributo offline. |
| /t | Impedisce **fc** dalla conversione tabulazioni in spazi. Il comportamento predefinito consiste nel considerare schede come spazi, con interruzioni ogni otto caratteri. |
| /U | Confronta i file come file di testo Unicode. |
| /W | Comprime gli spazi vuoti (vale a dire, schede e spazi) durante il confronto. Se una riga contiene molti spazi consecutivi o nelle schede, **/w** li considererà come spazio singolo. Se utilizzato con **/w**, **fc** Ignora gli spazi vuoti all'inizio e fine riga. |
| /`<nnnn>` | Specifica il numero di righe consecutive che deve corrispondere in seguito una mancata corrispondenza prima **fc** considera i file essere risincronizzata. Se il numero di righe corrispondenti nei file è inferiore a *nnnn*, **FC** Visualizza le righe corrispondenti come differenze. Il valore predefinito è 2. |
| `[<drive1>:][<path1>]<filename1>` | Specifica il percorso e nome del primo file o set di file da confrontare. *filename1* è obbligatorio. |
| `[<drive2>:][<path2>]<filename2>` | Specifica il percorso e nome del secondo file o set di file da confrontare. *filename2* è obbligatorio. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Questo comando è implemeted da c:\WINDOWS\fc.exe. È possibile usare questo comando in PowerShell, ma assicurarsi di definire il file eseguibile completo (FC. exe) perché "FC" è anche un alias per Format-Custom.

- Quando si usa **FC** per un confronto ASCII, **FC** Visualizza le differenze tra due file nell'ordine seguente:

  - Nome del primo file

  - Righe da *filename1* che differiscono tra i file

  - Prima riga che corrisponde in entrambi i file

  - Nome del secondo file

  - Righe da *filename2* diverse

  - Prima riga che corrisponde

- **/b** Visualizza le mancate corrispondenze rilevate durante un confronto binario nella sintassi seguente:

    `\<XXXXXXXX: YY ZZ>`

    Il valore di *xxxxxxxx* specifica l'indirizzo esadecimale relativo per la coppia di byte, misurato a partire dall'inizio del file. Gli indirizzi iniziano a 00000000. I valori esadecimali per *YY* e *ZZ* rappresentano i byte non corrispondenti rispettivamente da *filename1* e *filename2*.

- È possibile usare caratteri jolly (**&#42;** e **?**) in *filename1* e *filename2*. Se si usa un carattere jolly in *filename1*, **FC** confronta tutti i file specificati con il file o il set di file specificato da *filename2*. Se si usa un carattere jolly in *filename2*, **FC** usa il valore corrispondente di *filename1*.

- Quando si confrontano i file ASCII, **FC** usa un buffer interno (sufficientemente grande per ospitare 100 righe) come risorsa di archiviazione. Se i file sono maggiori del buffer, **fc** Confronta ciò che è possibile caricare nel buffer. Se **FC** non trova una corrispondenza nelle parti caricate dei file, viene arrestata e viene visualizzato il messaggio seguente:

    `Resynch failed. Files are too different.`

    Quando si confrontano file binari di dimensioni maggiori della memoria disponibile, **FC** confronta completamente entrambi i file, sovrapponendo le parti in memoria con le parti successive dal disco. L'output è uguale a quello per i file che rientrano completamente in memoria.

### <a name="examples"></a>Esempi

Per eseguire un confronto ASCII tra due file di testo, *Monthly. rpt* e *Sales. rpt*, e visualizzare i risultati in formato abbreviato, digitare:

```
fc /a monthly.rpt sales.rpt
```

Per eseguire un confronto binario tra due file batch, *profits. bat* e *guadagni. bat*, digitare:

```
fc /b profits.bat earnings.bat
```

Risultati simili a visualizzato il seguente:

```
00000002: 72 43
00000004: 65 3A
0000000E: 56 92
000005E8: 00 6E
FC: earnings.bat longer than profits.bat
```

Se i file profit. bat e Earnings. bat sono identici, **FC** Visualizza il messaggio seguente:

```
Comparing files profits.bat and earnings.bat
FC: no differences encountered
```

Per confrontare ogni file con estensione bat nella directory corrente con il file *nuovo. bat*, digitare:

```
fc *.bat new.bat
```

Per confrontare il file *nuovo. bat* sull'unità C con il file *nuovo. bat* sull'unità D, digitare:

```
fc c:new.bat d:*.bat
```

Per confrontare ogni file batch nella directory radice dell'unità C per il file con lo stesso nome nella directory radice dell'unità D, digitare:

```
fc c:*.bat d:*.bat
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
