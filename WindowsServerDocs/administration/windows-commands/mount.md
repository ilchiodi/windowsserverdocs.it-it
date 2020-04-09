---
title: montare
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dd9d7ecb-ef00-4aaa-bcd0-423fa636e34a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72239a3dc55bf88638004c5885da6e1da70bcd02
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839394"
---
# <a name="mount"></a>montare



È possibile utilizzare **Mount** per montare le condivisioni di rete NFS (Network File System).

## <a name="syntax"></a>Sintassi

```
mount [-o <Option>[...]] [-u:<UserName>] [-p:{<Password> | *}] {\\<ComputerName>\<ShareName> | <ComputerName>:/<ShareName>} {<DeviceName> | *}
```

## <a name="description"></a>Descrizione

L'utilità da riga di comando **Mount** monta il file System identificato da *ShareName* esportato dal server NFS identificato da *ComputerName* e lo associa alla lettera di unità specificata da *DeviceName* o, se viene usato **&#42;** un asterisco (), dalla prima lettera di driver disponibile. Gli utenti possono quindi accedere ai file system esportati come se si trattasse di un'unità nel computer locale. Se usato senza opzioni o argomenti, **Mount** Visualizza le informazioni su tutti i file System NFS montati.

L'utilità di **montaggio** è disponibile solo se è installato client per NFS.

Con l'utilità di **montaggio** è possibile utilizzare le opzioni e gli argomenti seguenti.


|          Termine          |                                                                                                                                                                                                                                                Definizione                                                                                                                                                                                                                                                |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -o rsize. =\<bufferSize > |                                                                                                                                                                                            Imposta le dimensioni in kilobyte del buffer di lettura. I valori accettabili sono 1, 2, 4, 8, 16 e 32; il valore predefinito è 32 KB.                                                                                                                                                                                            |
| -o wsize =\<bufferSize > |                                                                                                                                                                                           Imposta le dimensioni in kilobyte del buffer di scrittura. I valori accettabili sono 1, 2, 4, 8, 16 e 32; il valore predefinito è 32 KB.                                                                                                                                                                                            |
| -o timeout =\<secondi >  |                                                                                                                                                                       Imposta il valore di timeout in secondi per una chiamata di procedura remota (RPC). I valori accettabili sono 0,8, 0,9 e qualsiasi numero intero compreso nell'intervallo 1-60; il valore predefinito è 0,8.                                                                                                                                                                       |
|   -o nuovo tentativo =\<numero >   |                                                                                                                                                                                             Imposta il numero di tentativi per un montaggio soft. I valori accettabili sono numeri interi compresi nell'intervallo 1-10; il valore predefinito è 1.                                                                                                                                                                                             |
|     -o mtype = {soft     |                                                                                                                                                                                                                                                  rigido                                                                                                                                                                                                                                                   |
|        -o anon         |                                                                                                                                                                                                                                       Viene montato come utente anonimo.                                                                                                                                                                                                                                       |
|       -o NOLOCK        |                                                                                                                                                                                                                                Disabilita il blocco (il valore predefinito è **abilitato**).                                                                                                                                                                                                                                |
|    -o CaseSensitive    |                                                                                                                                                                                                                         Impone la distinzione tra maiuscole e minuscole per le ricerche di file nel server.                                                                                                                                                                                                                          |
| -o FileAccess = modalità\<>  | Specifica la modalità di autorizzazione predefinita dei nuovi file creati nella condivisione NFS. Specificare *mode* come numero a tre cifre nel formato *PGT*, dove *o*, *g*e *w* sono ciascuna cifra che rappresenta l'accesso concesso rispettivamente al proprietario del file, al gruppo e al mondo. Le cifre devono essere comprese nell'intervallo 0-7 con il significato seguente:</br>-0: nessun accesso</br>-1: x (esecuzione dell'accesso)</br>-2: w (accesso in scrittura)</br>-3: WX</br>-4: r (accesso in lettura)</br>-5: RX</br>-6: RW</br>-7: rwx |
|    -o lang = {EUC-JP     |                                                                                                                                                                                                                                                  EUC-TW                                                                                                                                                                                                                                                  |
|     -u:\<nome utente >     |                                                                                                                                                                             Specifica il nome utente da utilizzare per il montaggio della condivisione. Se il *nome utente* non è preceduto da una barra rovesciata ( **\\** ), viene considerato come nome utente UNIX.                                                                                                                                                                             |
|     -p:\<password >     |                                                                                                                                                                                          Password da utilizzare per il montaggio della condivisione. Se si utilizza un asterisco ( **&#42;** ), verrà richiesto di immettere la password.                                                                                                                                                                                          |

> [!NOTE]
