---
title: lpr
description: Argomento di riferimento per il comando LPR, che invia un file a un computer o a un dispositivo di condivisione della stampante che esegue il servizio LPD (Line Printer Daemon) in preparazione alla stampa.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: afc8790b-8b52-45c4-acdf-be0ffa9da534
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d11b72ee807869613505052cfb51e80a89a80d70
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84223027"
---
# <a name="lpr"></a>lpr

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia un file a un computer o a un dispositivo di condivisione stampanti che esegue il servizio LPD (Line Printer Daemon) in preparazione alla stampa.

## <a name="syntax"></a>Sintassi

```
lpr [-S <servername>] -P <printername> [-C <bannercontent>] [-J <jobname>] [-o | -o l] [-x] [-d] <filename>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -S`<servername>` | Specifica, tramite nome o indirizzo IP, il computer o dispositivo che ospita la coda di stampa LPD con cui si desidera visualizzare lo stato di condivisione della stampante.  Questo parametro è obbligatorio e deve essere in maiuscolo. |
| -P`<printername> `| Specifica (nome) della stampante per la coda di stampa con uno stato che si desidera visualizzare. Per trovare il nome della stampante, aprire la cartella **stampanti** . Questo parametro è obbligatorio e deve essere in maiuscolo. |
| -C`<bannercontent>` | Specifica il contenuto per la stampa su pagina di intestazione del processo di stampa. Se non si include questo parametro, il nome del computer da cui è stato inviato il processo di stampa viene visualizzato nella pagina del banner. Questo parametro deve essere in maiuscolo. |
| -J`<jobname>` | Specifica il nome di processo di stampa che verrà stampato sulla pagina di intestazione. Se non si include questo parametro, il nome del file stampato viene visualizzato nella pagina del banner. Questo parametro deve essere in maiuscolo. |
| `[-o | -o l]` | Specifica il tipo di file che si desidera stampare. Il parametro **-o** Specifica che si desidera stampare un file di testo. Il parametro **-o l** specifica che si desidera stampare un file binario (ad esempio, un file PostScript). |
| -d | Specifica che il file di dati deve essere inviato prima il file di controllo. Utilizzare questo parametro se la stampante in uso richiede il file di dati inviati per primi. Per ulteriori informazioni, vedere la documentazione della stampante. |
| -X | Specifica che il comando **LPR** deve essere compatibile con il sistema operativo Sun Microsystems (denominato SunOS) per le versioni fino a 4.1.4_u1. |
| `<filename>` | Specifica (nome) il file da stampare. Questo parametro è obbligatorio. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="examples"></a>Esempi

Per stampare il file di testo *Document. txt* nella coda della stampante *stampa laserprinter1* in un host LPD in *10.0.0.45*, digitare:

```
lpr -S 10.0.0.45 -P Laserprinter1 -o Document.txt
```

Per stampare il file *PostScript_file. PS* Adobe PostScript nella coda della stampante *stampa laserprinter1* in un host LPD in *10.0.0.45*, digitare:

```
lpr -S 10.0.0.45 -P Laserprinter1 -o l PostScript_file.ps
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Informazioni di riferimento sui comandi di stampa](print-command-reference.md)
