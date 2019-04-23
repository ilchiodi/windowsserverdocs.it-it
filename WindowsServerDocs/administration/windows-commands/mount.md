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
ms.openlocfilehash: f9d13f5eddef80d99c11fe59c6bd5e589fa67546
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858372"
---
# <a name="mount"></a>montare



È possibile usare **montare** montare le condivisioni di rete di Network File System (NFS).

## <a name="syntax"></a>Sintassi

```
mount [-o <Option>[...]] [-u:<UserName>] [-p:{<Password> | *}] {\\<ComputerName>\<ShareName> | <ComputerName>:/<ShareName>} {<DeviceName> | *}
```

## <a name="description"></a>Descrizione

Il **montare** utilità della riga di comando consente di montare file system identificato dal *nomecondivisione* esportato dal server NFS identificata dalla *ComputerName* e lo associa al lettera di unità specificata da *DeviceName* oppure, se un asterisco (**&#42;**) viene utilizzato, per la prima lettera di unità disponibile. Gli utenti possono quindi accedere al sistema di file esportato come se fosse un'unità nel computer locale. Quando viene utilizzata senza le opzioni o argomenti **montare** Visualizza informazioni su tutti i sistemi di file montati NFS.

Il **montare** utilità è disponibile solo se l'installazione del Client per NFS.

Le opzioni e gli argomenti seguenti sono utilizzabili con il **montare** utilità.

|Nome|Definizione|
|----|----------|
|-o rsize =\<buffersize >|Imposta le dimensioni in kilobyte del buffer di lettura. I valori accettabili sono 1, 2, 4, 8, 16 e 32; il valore predefinito è 32 KB.|
|-o wsize=\<buffersize>|Imposta le dimensioni in kilobyte del buffer di scrittura. I valori accettabili sono 1, 2, 4, 8, 16 e 32; il valore predefinito è 32 KB.|
|o - timeout =\<secondi >|Imposta il valore di timeout in secondi per una chiamata di procedura remota (RPC). I valori accettabili sono 0.8 0.9 e qualsiasi numero intero compreso tra 1 e 60; il valore predefinito è 0,8.|
|nuovo tentativo -o =\<numero >|Imposta il numero di tentativi per un soft mount. I valori accettabili sono numeri interi nell'intervallo 1-10; il valore predefinito è 1.|
|-o mtype={soft | disco rigido}|Imposta il tipo di montaggio (valore predefinito è **soft**). Indipendentemente dal tipo di montaggio, **montare** verrà restituito se è possibile montare la condivisione immediatamente. Una volta che la condivisione sia stata montata, tuttavia, se il tipo di montaggio **rigido**, Client per NFS continuerà a tentare di accedere alla condivisione finché non è riuscita. Di conseguenza, se il server NFS non è disponibile, qualsiasi programma Windows tenta di accedere alla condivisione sembra bloccarsi, o un "blocco", se il tipo di montaggio **rigido**.|
|-o anonima|Punti di montaggio come utente anonimo.|
|nolock -o|Disabilita il blocco (valore predefinito è **abilitato**).|
|-o casesensitive|Forza le ricerche del file nel server per essere maiuscole e minuscole.|
|-o fileaccess =\<modalità >|Specifica la modalità di autorizzazione predefiniti dei nuovi file creati nella condivisione NFS. Specificare *modalità* come numero a tre cifre nel formato *PGT*, dove *o*, *g*, e *w* è ognuno una cifra che rappresenta l'accesso concesso proprietario del file, il gruppo e tutto il mondo, rispettivamente. Le cifre devono essere compreso nell'intervallo da 0 a 7 con il significato seguente:</br>-   0: Nessun accesso</br>-1: x (accesso in esecuzione)</br>-2: w (accesso in scrittura)</br>-3: wx</br>-4: r (accesso in lettura)</br>-   5: rx</br>-6: rw</br>-7: rwx|
|-o lang = {euc-jp|euc-tw|EUC-kr|shift-jis|big5|ksc5601|gb2312-80|ansi}|Specifica la codifica predefinita utilizzata per i nomi di file e directory e, se utilizzata, deve essere impostata su uno dei seguenti:</br>-   **ansi**</br>-   **BIG5** (cinese)</br>-   **euc-jp** (giapponese)</br>-   **EUC-kr** (coreano)</br>-   **EUC-tw** (cinese)</br>-   **gb2312 80** (cinese semplificato)</br>-   **KSC5601** (coreano)</br>-   **Shift-jis** (giapponese)</br>Se questa opzione è impostata su **ansi** nei sistemi configurati per impostazioni locali di lingua diversa dall'inglese, lo schema di codifica è impostato per lo schema di codifica predefinita per le impostazioni locali. Di seguito sono gli schemi di codifica predefinita per le impostazioni locali indicate.</br>-Giapponese: SHIFT-JIS</br>-Coreano: KS_C_5601-1987</br>-Cinese semplificato: GB2312-80</br>-Chinese traditional: BIG5|
|-u:\<UserName >|Specifica il nome utente da usare per montare la condivisione. Se *nomeutente* non è preceduto da una barra rovesciata (**\**), viene considerato come un nome utente di UNIX.|
|-p:\<Password>|Password da usare per montare la condivisione. Se si usa un asterisco (**&#42;**), verrà richiesto di specificarla.|

> [!NOTE]
