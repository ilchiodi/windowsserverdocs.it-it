---
title: prnjobs
description: Informazioni su come gestire i processi di stampa dalla riga di comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5ad34199-7a5a-40c1-8053-bccd5929df43
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 002098ba64e97c243d1cb53813b9fa858d32c752
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821241"
---
# <a name="prnjobs"></a>prnjobs

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sospende, riprende, Annulla ed elenca i processi di stampa.

## <a name="syntax"></a>Sintassi
```
cscript Prnjobs {-z | -m | -x | -l | -?} [-s <ServerName>]
[-p <printerName>] [-j <JobID>] [-u <UserName>] [-w <Password>]
```

### <a name="parameters"></a>Parametri

|          Parametro           |                                                                                                                                                                                        Descrizione                                                                                                                                                                                        |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -Z              |                                                                                                                                                                 sospende il processo di stampa specificato con il parametro **-j** .                                                                                                                                                                 |
|              -M              |                                                                                                                                                                Riprende il processo di stampa specificato con il parametro **-j** .                                                                                                                                                                 |
|              -X              |                                                                                                                                                                Annulla il processo di stampa specificato con il parametro **-j** .                                                                                                                                                                 |
|              -l              |                                                                                                                                                                        Elenca tutti i processi di stampa in una coda di stampa.                                                                                                                                                                         |
|       -s \< nomeserver>       |                                                                                                                  Specifica il nome del computer remoto che ospita la stampante che si desidera gestire. Se non si specifica un computer, viene usato il computer locale.                                                                                                                  |
|      -p \< printername>       |                                                                                                                                                           Specifica il nome della stampante che si desidera gestire. Obbligatorio.                                                                                                                                                            |
|         -j \< JobID>          |                                                                                                                                                                Specifica (in base al numero ID) il processo di stampa che si desidera annullare.                                                                                                                                                                 |
| -u \< nomeutente>-w<Password> | Specifica un account con le autorizzazioni per la connessione al computer che ospita la stampante che si desidera gestire. Tutti i membri del gruppo Administrators locale del computer di destinazione dispongono di queste autorizzazioni, ma è possibile concedere anche le autorizzazioni ad altri utenti. Se non si specifica un account, è necessario effettuare l'accesso con un account con le autorizzazioni necessarie per il funzionamento del comando. |
|              /?              |                                                                                                                                                                           Visualizza la guida al prompt dei comandi.                                                                                                                                                                            |

## <a name="remarks"></a>Osservazioni
-   Il comando **prnjobs** è uno script Visual Basic che si trova nella directory%windir%\system32\. printing_Admin_Scripts \\ <language> . Per usare questo comando, al prompt dei comandi digitare **cscript** seguito dal percorso completo del file prnjobs o passare alla cartella appropriata. Ad esempio:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs.vbs
    ```
-   Se le informazioni fornite contengono spazi, racchiudere il testo tra virgolette (ad esempio, `"computer Name"` ).

## <a name="examples"></a><a name="BKMK_examples"></a>Esempi
Per sospendere un processo di stampa con ID di processo 27 inviato al computer remoto denominato ServerRU per la stampa sulla stampante denominata colorprinter, digitare:
```
cscript prnjobs.vbs -z -s HRServer -p colorprinter -j 27
```
Per elencare tutti i processi di stampa correnti nella coda per la stampante locale denominata colorprinter_2, digitare:
```
cscript prnjobs.vbs -l -p colorprinter_2
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Informazioni di riferimento sui comandi di stampa](print-command-reference.md)
