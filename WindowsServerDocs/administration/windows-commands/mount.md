---
title: montare
description: Argomento di riferimento per il comando di montaggio, che consente di montare condivisioni di rete NFS (Network File System).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dd9d7ecb-ef00-4aaa-bcd0-423fa636e34a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 823b88b8ab1168776c25e05e3dbf5ec08d784724
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354561"
---
# <a name="mount"></a>montare

Utilità da riga di comando che monta le condivisioni di rete NFS (Network File System). Se usato senza opzioni o argomenti, **Mount** Visualizza le informazioni su tutti i file System NFS montati.

> [!NOTE]
> Questa utilità è disponibile solo se è installato **client per NFS** .

## <a name="syntax"></a>Sintassi

```
mount [-o <option>[...]] [-u:<username>] [-p:{<password> | *}] {\\<computername>\<sharename> | <computername>:/<sharename>} {<devicename> | *}
```

### <a name="parameters"></a>Parametri

| Parametro  | Descrizione |
| ---------- | ----------- |
| -o rsize. =`<buffersize>` | Imposta le dimensioni in kilobyte del buffer di lettura. I valori accettabili sono 1, 2, 4, 8, 16 e 32; il valore predefinito è 32 KB. |
| -o wsize =`<buffersize>` | Imposta le dimensioni in kilobyte del buffer di scrittura. I valori accettabili sono 1, 2, 4, 8, 16 e 32; il valore predefinito è 32 KB. |
| -o timeout =`<seconds>` | Imposta il valore di timeout in secondi per una chiamata di procedura remota (RPC). I valori accettabili sono 0,8, 0,9 e qualsiasi numero intero compreso nell'intervallo 1-60; il valore predefinito è 0,8. |
| -o nuovo tentativo =`<number>` | Imposta il numero di tentativi per un montaggio soft. I valori accettabili sono numeri interi compresi nell'intervallo 1-10; il valore predefinito è 1. |
| -o mtype =`{soft|hard}` | Imposta il tipo di montaggio per la condivisione NFS. Per impostazione predefinita, Windows usa un montaggio soft. Il timeout del montaggio Soft è più semplice quando si verificano problemi di connessione; Tuttavia, per ridurre l'interferenza di I/O durante il riavvio del server NFS, è consigliabile usare un hard mount.|
| -o anon | Viene montato come utente anonimo. |
| -o NOLOCK | Disabilita il blocco (il valore predefinito è **abilitato**). |
| -o CaseSensitive | Impone la distinzione tra maiuscole e minuscole per le ricerche di file nel server. |
| -o FileAccess =`<mode>` | Specifica la modalità di autorizzazione predefinita dei nuovi file creati nella condivisione NFS. Specificare *mode* come numero a tre cifre nel formato *PGT*, dove *o*, *g*e *w* sono ciascuna cifra che rappresenta l'accesso concesso rispettivamente al proprietario del file, al gruppo e al mondo. Le cifre devono essere comprese nell'intervallo 0-7, tra cui:<ul><li>**0:** Nessun accesso</li><li>**1:** x (esecuzione dell'accesso)</li><li>**2:** w (accesso in scrittura)</li><li>**3:** WX (accesso in scrittura ed esecuzione)</li><li>**4:** r (accesso in lettura)</li><li>**5:** RX (accesso in lettura ed esecuzione)</li><li>**6:** RW (accesso in lettura e scrittura)</li><li>**7:** rwx (accesso in lettura, scrittura ed esecuzione)</li></ul> |
| -o lang =`{euc-jp|euc-tw|euc-kr|shift-jis|Big5|Ksc5601|Gb2312-80|Ansi)` | Specifica la codifica della lingua da configurare in una condivisione NFS. È possibile utilizzare una sola lingua nella condivisione. Questo valore può includere uno dei valori seguenti:<ul><li>**EUC-JP:** Giapponese</li><li>**EUC-TW:** Cinese</li><li>**EUC-KR:** Coreano</li><li>**MAIUSC-JIS:** Giapponese</li><li>**Big5:** Cinese</li><li>**Ksc5601:** Coreano</li><li>**GB2312-80:** Cinese semplificato</li><li>**ANSI:** Con codifica ANSI</li></ul> |
| u`<username>` | Specifica il nome utente da utilizzare per il montaggio della condivisione. Se il *nome utente* non è preceduto da una barra rovesciata (* *\** ), viene considerato come un nome utente UNIX. |
| p`<password>` | Password da utilizzare per il montaggio della condivisione. Se si usa un asterisco (**&#42;**), verrà richiesto di immettere la password. |
| `<computername>` | Specifica il nome del server NFS. |
| `<sharename>` | Specifica il nome del file system. |
| `<devicename>` | Specifica la lettera di unità e il nome del dispositivo. Se si utilizza un asterisco (**&#42;**), questo valore rappresenta la prima lettera di driver disponibile. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
