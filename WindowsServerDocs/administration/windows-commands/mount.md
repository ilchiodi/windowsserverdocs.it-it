---
title: montare
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dd9d7ecb-ef00-4aaa-bcd0-423fa636e34a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65083ce4b7d04129ff630a7f314c458d7ee378f4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437243"
---
# <a name="mount"></a>montare



È possibile usare **montare** montare le condivisioni di rete di Network File System (NFS).

## <a name="syntax"></a>Sintassi

```
mount [-o <Option>[...]] [-u:<UserName>] [-p:{<Password> | *}] {\\<ComputerName>\<ShareName> | <ComputerName>:/<ShareName>} {<DeviceName> | *}
```

## <a name="description"></a>Descrizione

Il **montare** utilità della riga di comando consente di montare file system identificato dal *nomecondivisione* esportato dal server NFS identificata dalla *ComputerName* e lo associa al lettera di unità specificata da *DeviceName* oppure, se un asterisco ( **&#42;** ) viene utilizzato, per la prima lettera di unità disponibile. Gli utenti possono quindi accedere al sistema di file esportato come se fosse un'unità nel computer locale. Quando viene utilizzata senza le opzioni o argomenti **montare** Visualizza informazioni su tutti i sistemi di file montati NFS.

Il **montare** utilità è disponibile solo se l'installazione del Client per NFS.

Le opzioni e gli argomenti seguenti sono utilizzabili con il **montare** utilità.


|          Nome          |                                                                                                                                                                                                                                                Definizione                                                                                                                                                                                                                                                |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -o rsize =\<buffersize > |                                                                                                                                                                                            Imposta le dimensioni in kilobyte del buffer di lettura. I valori accettabili sono 1, 2, 4, 8, 16 e 32; il valore predefinito è 32 KB.                                                                                                                                                                                            |
| -o wsize=\<buffersize> |                                                                                                                                                                                           Imposta le dimensioni in kilobyte del buffer di scrittura. I valori accettabili sono 1, 2, 4, 8, 16 e 32; il valore predefinito è 32 KB.                                                                                                                                                                                            |
| o - timeout =\<secondi >  |                                                                                                                                                                       Imposta il valore di timeout in secondi per una chiamata di procedura remota (RPC). I valori accettabili sono 0.8 0.9 e qualsiasi numero intero compreso tra 1 e 60; il valore predefinito è 0,8.                                                                                                                                                                       |
|   nuovo tentativo -o =\<numero >   |                                                                                                                                                                                             Imposta il numero di tentativi per un soft mount. I valori accettabili sono numeri interi nell'intervallo 1-10; il valore predefinito è 1.                                                                                                                                                                                             |
|     -o mtype={soft     |                                                                                                                                                                                                                                                  disco rigido}                                                                                                                                                                                                                                                   |
|        -o anonima         |                                                                                                                                                                                                                                       Punti di montaggio come utente anonimo.                                                                                                                                                                                                                                       |
|       nolock -o        |                                                                                                                                                                                                                                Disabilita il blocco (valore predefinito è **abilitato**).                                                                                                                                                                                                                                |
|    -o casesensitive    |                                                                                                                                                                                                                         Forza le ricerche del file nel server per essere maiuscole e minuscole.                                                                                                                                                                                                                          |
| -o fileaccess =\<modalità >  | Specifica la modalità di autorizzazione predefiniti dei nuovi file creati nella condivisione NFS. Specificare *modalità* come numero a tre cifre nel formato *PGT*, dove *o*, *g*, e *w* è ognuno una cifra che rappresenta l'accesso concesso proprietario del file, il gruppo e tutto il mondo, rispettivamente. Le cifre devono essere compreso nell'intervallo da 0 a 7 con il significato seguente:</br>-   0: Nessun accesso</br>-1: x (accesso in esecuzione)</br>-2: w (accesso in scrittura)</br>-3: wx</br>-4: r (accesso in lettura)</br>-   5: rx</br>-6: rw</br>-7: rwx |
|    -o lang = {euc-jp     |                                                                                                                                                                                                                                                  euc-tw                                                                                                                                                                                                                                                  |
|     -u:\<UserName >     |                                                                                                                                                                             Specifica il nome utente da usare per montare la condivisione. Se *nomeutente* non è preceduto da una barra rovesciata ( **\\** ), viene considerato come un nome utente di UNIX.                                                                                                                                                                             |
|     -p:\<Password>     |                                                                                                                                                                                          Password da usare per montare la condivisione. Se si usa un asterisco ( **&#42;** ), verrà richiesto di specificarla.                                                                                                                                                                                          |

> [!NOTE]
