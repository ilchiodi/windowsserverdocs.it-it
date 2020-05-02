---
title: fc
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 485fc3d8-b7c5-496d-87be-0011112f27d5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 372e8b6a605bd96f6287a005004fd2f1532dfe4f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725641"
---
# <a name="fc"></a>fc



Confronta due file o gruppi di file e visualizza le differenze esistenti tra di essi.



## <a name="syntax"></a>Sintassi

```
fc /a [/c] [/l] [/lb<N>] [/n] [/off[line]] [/t] [/u] [/w] [/<NNNN>] [<Drive1>:][<Path1>]<FileName1> [<Drive2>:][<Path2>]<FileName2>
fc /b [<Drive1:>][<Path1>]<FileName1> [<Drive2:>][<Path2>]<FileName2>
```

### <a name="parameters"></a>Parametri

|            Parametro             |                                                                                                                                     Descrizione                                                                                                                                      |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                /a                |                                                 Abbrevia l'output di un confronto ASCII. Invece di visualizzare tutte le righe che sono diverse, **fc** viene visualizzata solo la riga e il cognome di ogni gruppo di differenze.                                                  |
|                / b                |             Confronta i due file in modalità binaria, byte per byte e non tenta di sincronizzare nuovamente i file dopo aver trovato una mancata corrispondenza. Si tratta della modalità predefinita per il confronto tra i file con le seguenti estensioni: .exe,. com, sys, obj,. lib o bin.              |
|                /C                |                                                                                                                               Ignora le maiuscole/minuscole.                                                                                                                               |
|                /l                |               Confronta i file in modalità ASCII, riga per riga e tenta di sincronizzare nuovamente i file dopo aver trovato una mancata corrispondenza. Si tratta della modalità predefinita per il confronto dei file, ad eccezione dei file con le seguenti estensioni: .exe,. com, sys, obj,. lib o bin.                |
|             /LB\<N>              |                         Imposta il numero di righe per il buffer di riga interno su *N*. La lunghezza predefinita del buffer di riga è 100 righe. Se i file che si desidera confrontare più di 100 righe diverse volte consecutive, **fc** annullerà il confronto.                         |
|                /n                |                                                                                                                Visualizza i numeri di riga durante un confronto ASCII.                                                                                                                 |
|            / [offline]            |                                                                                                               Non ignorare i file che sono impostato l'attributo non in linea.                                                                                                               |
|                /t                |                                                                    Impedisce **fc** dalla conversione tabulazioni in spazi. Il comportamento predefinito consiste nel considerare schede come spazi, con interruzioni ogni otto caratteri.                                                                    |
|                /U                |                                                                                                                        Confronta i file come file di testo Unicode.                                                                                                                         |
|                /W                |         Comprime gli spazi vuoti (vale a dire, schede e spazi) durante il confronto. Se una riga contiene molti spazi consecutivi o nelle schede, **/w** li considererà come spazio singolo. Se utilizzato con **/w**, **fc** Ignora gli spazi vuoti all'inizio e fine riga.         |
|             /\<> NNNN             | Specifica il numero di righe consecutive che deve corrispondere in seguito una mancata corrispondenza prima **fc** considera i file essere risincronizzata. Se il numero di righe corrispondenti nei file è minore di *NNNN*, **fc** righe saranno visualizzate come differenze. Il valore predefinito è 2. |
| [\<Unità1>:] [<Path1>]<FileName1> |                                                                                        Specifica il percorso e nome del primo file o set di file da confrontare. *FileName1* è obbligatorio.                                                                                        |
| [\<Unità2>:] [<Path2>]<FileName2> |                                                                                       Specifica il percorso e nome del secondo file o set di file da confrontare. *FileName2* è obbligatorio.                                                                                        |
|                /?                |                                                                                                                         Visualizza la guida al prompt dei comandi.                                                                                                                         |

## <a name="remarks"></a>Osservazioni

-   Questo comando è implemeted da c:\WINDOWS\fc.exe. È possibile usare questo comando in PowerShell, ma assicurarsi di definire il file eseguibile completo (FC. exe) perché "FC" è un alias per Format-Custom.

-   Segnalazione delle differenze tra i file per un confronto ASCII

    Quando si usa **FC** per un confronto ASCII, **FC** Visualizza le differenze tra due file nell'ordine seguente:  
    -   Nome del primo file
    -   Righe *FileName1* che differiscono tra i file
    -   Prima riga che corrisponde in entrambi i file
    -   Nome del secondo file
    -   Righe *FileName2* che differiscono
    -   Prima riga che corrisponde
-   Uso di **/b** per i confronti binari

    **/b** Visualizza le mancate corrispondenze rilevate durante un confronto binario nella sintassi seguente:

    `\<XXXXXXXX: YY ZZ>`

    Il valore di *xxxxxxxx* specifica l'indirizzo esadecimale relativo per la coppia di byte, misurato a partire dall'inizio del file. Gli indirizzi iniziano a 00000000. Valore esadecimale per *AA* e *ZZ* rappresentano i byte non corrispondenti da *FileName1* e *FileName2*, rispettivamente.
-   Utilizzo di caratteri jolly

    È possibile usare caratteri jolly (**&#42;** e **?**) in *filename1* e *filename2*. Se si utilizza un carattere jolly nel *FileName1*, **fc** Confronta tutti i file specificati nel file o un set di file specificato da *FileName2*. Se si utilizza un carattere jolly nel *FileName2*, **fc** utilizza il valore corrispondente dal *FileName1*.
-   Uso della memoria

    Quando si confrontano i file ASCII, **FC** usa un buffer interno (sufficientemente grande per ospitare 100 righe) come risorsa di archiviazione. Se i file sono maggiori del buffer, **fc** Confronta ciò che è possibile caricare nel buffer. Se **FC** non trova una corrispondenza nelle parti caricate dei file, viene arrestata e viene visualizzato il messaggio seguente:

    `Resynch failed. Files are too different.`

    Quando si confrontano file binari di dimensioni maggiori della memoria disponibile, **FC** confronta completamente entrambi i file, sovrapponendo le parti in memoria con le parti successive dal disco. L'output è uguale a quello per i file che rientrano completamente in memoria.

## <a name="examples"></a>Esempi

Per eseguire un confronto di due file di testo, mensile e vendite. rpt, ASCII e visualizzare i risultati in formato abbreviato, digitare:
```
fc /a monthly.rpt sales.rpt 
```
Per eseguire un confronto binario tra due file batch, ricavi, digitare:
```
fc /b profits.bat earnings.bat
```
Risultati simili a visualizzato il seguente:
```
00000002: 72 43
00000004: 65 3A
0000000E: 56 92
...
...
...
000005E8: 00 6E
FC: Earnings.bat longer than Profits.bat
```
Se sono identici, i file ricavi **fc** Visualizza il messaggio seguente:
```
Comparing files Profits.bat and Earnings.bat
FC: no differences encountered
```
Per confrontare ogni file con estensione bat nella directory corrente con il file nuovo. bat, digitare:
```
fc *.bat new.bat
```
Per confrontare il file nuovo. bat sull'unità C con il file nuovo. bat sull'unità D, digitare:
```
fc c:new.bat d:*.bat
```
Per confrontare ogni file batch nella directory radice dell'unità C per il file con lo stesso nome nella directory radice dell'unità D, digitare:
```
fc c:*.bat d:*.bat
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
