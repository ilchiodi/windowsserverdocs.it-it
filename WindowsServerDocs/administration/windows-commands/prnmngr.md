---
title: prnmngr
description: Informazioni su come aggiungere, eliminare ed elencare le stampanti e le connessioni.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39eee1a8-4b41-4c9f-941e-486495135eb8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 12981519a1d3bfc079a58e5883bc845955b8a8c6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372074"
---
# <a name="prnmngr"></a>prnmngr

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiunge, Elimina ed elenca stampanti o connessioni stampanti, oltre a impostare e visualizzare la stampante predefinita.

## <a name="syntax"></a>Sintassi
```
cscript Prnmngr {-a | -d | -x | -g | -t | -l | -?}[c] [-s <ServerName>] 
[-p <printerName>] [-m <printermodel>] [-r <PortName>] [-u <UserName>] 
[-w <Password>]
```

## <a name="parameters"></a>Parametri

|           Parametro           |                                                                                                                                                                                        Descrizione                                                                                                                                                                                        |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -a               |                                                                                                                                                                             aggiunge una connessione alla stampante locale.                                                                                                                                                                              |
|              -d               |                                                                                                                                                                               Elimina una connessione alla stampante.                                                                                                                                                                               |
|              -x               |                                                                                                               Elimina tutte le stampanti dal server specificato con il parametro **-s** . Se non si specifica un server, Windows eliminerà tutte le stampanti nel computer locale.                                                                                                               |
|              -g               |                                                                                                                                                                               Visualizza la stampante predefinita.                                                                                                                                                                               |
|              -t               |                                                                                                                                                        Imposta la stampante predefinita sulla stampante specificata dal parametro **-p** .                                                                                                                                                         |
|              -l               |                                                                                                         elenca tutte le stampanti installate nel server specificato dal parametro **-s** . Se non si specifica un server, Windows elenca le stampanti installate nel computer locale.                                                                                                         |
|               c               |                                                                                                                                      Specifica che il parametro si applica alle connessioni della stampante. Può essere usato con i parametri **-a** e **-x** .                                                                                                                                      |
|        -s <ServerName>        |                                                                                                                  Specifica il nome del computer remoto che ospita la stampante che si desidera gestire. Se non si specifica un computer, viene usato il computer locale.                                                                                                                  |
|       -p \<PrinterName >       |                                                                                                                                                                Specifica il nome della stampante che si desidera gestire.                                                                                                                                                                 |
|     -m \<DrivermodelName >     |                                                                                                          Specifica (per nome) il driver che si desidera installare. I driver sono spesso denominati per il modello di stampante supportato. Per ulteriori informazioni, vedere la documentazione della stampante.                                                                                                           |
|        -r \<Portaname >         |                                                                         Specifica la porta a cui è connessa la stampante. Se si tratta di una porta parallela o seriale, usare l'ID della porta (ad esempio, LPT1: o COM1:). Se si tratta di una porta TCP/IP, utilizzare il nome della porta specificato quando la porta è stata aggiunta.                                                                          |
| -u \<nome utente >-w \<password > | Specifica un account con le autorizzazioni per la connessione al computer che ospita la stampante che si desidera gestire. Tutti i membri del gruppo Administrators locale del computer di destinazione dispongono di queste autorizzazioni, ma è possibile concedere anche le autorizzazioni ad altri utenti. Se non si specifica un account, è necessario effettuare l'accesso con un account con le autorizzazioni necessarie per il funzionamento del comando. |
|              /?               |                                                                                                                                                                           Visualizza la guida al prompt dei comandi.                                                                                                                                                                            |

## <a name="remarks"></a>Osservazioni
-   Il comando **prndrvr** è uno script Visual Basic che si trova nella directory%windir%\system32\. printing_Admin_Scripts\\<language>. Per usare questo comando, al prompt dei comandi digitare **cscript** seguito dal percorso completo del file **prnmngr** o passare alla cartella appropriata. Ad esempio:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr
    ```
-   Se le informazioni fornite contengono spazi, racchiudere il testo tra virgolette, ad esempio `"computer Name"`.

## <a name="BKMK_examples"></a>Esempi
Per aggiungere una stampante denominata colorprinter_2 connessa a LPT1 nel computer locale e richiede un driver della stampante denominato Color Printer Driver1, digitare:
```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```
Per eliminare la stampante denominata colorprinter_2 dal computer remoto denominato ServerRU, digitare:
```
cscript prnmngr -d -s HRServer -p colorprinter_2 
```

#### <a name="additional-references"></a>riferimenti aggiuntivi
[Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[riferimento al comando stampa](print-command-reference.md)
