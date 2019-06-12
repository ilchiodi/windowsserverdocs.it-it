---
title: ftp
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 758335e1-fd8d-448c-a654-993126239dd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3639626962e295a54c9068c9731f1d30754a9472
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438375"
---
# <a name="ftp"></a>ftp

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Trasferimento di file da e verso un computer che esegue un servizio del server File Transfer Protocol (ftp). **FTP** può essere usato in modo interattivo o in modalità batch per l'elaborazione dei file di testo ASCII. 
## <a name="syntax"></a>Sintassi
```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<FileName>] [-a] [-A] [-x:<SendBuffer>] [-r:<RecvBuffer>] [-b:<AsyncBuffers>][-w:<WindowsSize>]  [-?] [<Host>]
```
### <a name="parameters"></a>Parametri

|     Parametro     |                                                                                                                                                      Descrizione                                                                                                                                                      |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        -v         |                                                                                                                                    Elimina visualizzazione delle risposte del server remoto.                                                                                                                                     |
|        -d         |                                                                                                               Attiva il debug, la visualizzazione di tutti i comandi passati tra il client FTP e il server FTP.                                                                                                                |
|        -i         |                                                                                                                            Disabilita la richiesta interattiva durante il trasferimento di più file.                                                                                                                             |
|        -n         |                                                                                                                                    Disattiva accesso automatico al momento della connessione iniziale.                                                                                                                                     |
|        -g         |                                         Disabilita il glob dei nomi di file.  **Glob** consenta l'utilizzo dell'asterisco (\*) e un punto interrogativo (?) come caratteri jolly nei nomi di file e percorso locali. Per altre informazioni, vedere [riferimenti aggiuntivi](ftp.md#BKMK_additionalRef).                                          |
|   -s:<FileName>   | Specifica un file di testo contenente **ftp** comandi. Questi comandi vengono eseguiti automaticamente dopo **ftp** viene avviato. Questo parametro non consente spazi. Utilizzare questo parametro anziché il reindirizzamento ( **<** ). **Nota:** In Windows 8 e Windows Server 2012 o versioni successive, il file di testo deve essere scritto in formato UTF-8. |
|        -a         |                                                                                                                 Specifica che è possibile utilizzare qualsiasi interfaccia locale durante l'associazione della connessione dati ftp.                                                                                                                  |
|        -A         |                                                                                                                                        Log al server ftp come anonimo.                                                                                                                                         |
|  -x:<SendBuffer>  |                                                                                                                                     Esegue l'override di quelle predefinite SO_SNDBUF di 8192.                                                                                                                                     |
|  -r:<RecvBuffer>  |                                                                                                                                     Esegue l'override di quelle predefinite SO_RCVBUF di 8192.                                                                                                                                     |
| -b:<AsyncBuffers> |                                                                                                                                    Sostituisce il numero di buffer predefinito async 3.                                                                                                                                     |
| -w:<WindowsSize>  |                                                                                                                   Specifica la dimensione del buffer di trasferimento. La dimensione predefinita è 4096 byte.                                                                                                                   |
|        -?         |                                                                                                                                         Visualizza la guida al prompt dei comandi.                                                                                                                                          |
|      <host>       |                                                                    Specifica il nome del computer, un indirizzo IP o un indirizzo IPv6 del server ftp a cui connettersi. Il nome host o l'indirizzo, se specificato, deve essere l'ultimo parametro della riga.                                                                    |

## <a name="remarks"></a>Note
- per altre informazioni sulle **ftp** comandi in Windows Server 2003, vedere [ftp](https://technet.microsoft.com/library/cc756013(v=ws.10).aspx).
- **FTP** parametri della riga di comando sono tra maiuscole e minuscole.
- Questo comando è disponibile solo se il **protocollo Internet (TCP/IP)** è installato come un componente nelle proprietà di una scheda di rete in connessioni di rete.
- **FTP** può essere utilizzato in modo interattivo. Dopo l'avvio, **ftp** Crea un ambiente secondario in cui è possibile utilizzare **ftp** comandi. È possibile tornare al prompt dei comandi digitando il **chiudere** comando. Quando il **ftp** ambiente secondario è in esecuzione, è indicata dal **ftp >** prompt dei comandi. Per ulteriori informazioni vedere il **ftp** comandi.
- **FTP** supporta l'utilizzo di IPv6 quando è installato il protocollo IPv6. Per altre informazioni, vedere [riferimenti aggiuntivi](ftp.md#BKMK_additionalRef).
  ## <a name="BKMK_Examples"></a>Esempi
  Per accedere al server ftp denominato Microsoft.com, digitare:
  ```
  ftp ftp.example.microsoft.com
  ```
  Per accedere al server ftp denominato Microsoft.com ed eseguire la **ftp** comandi contenuti in un file denominato Resync. txt, tipo:
  ```
  ftp -s:resync.txt ftp.example.microsoft.com
  ```
  ## <a name="BKMK_additionalRef"></a>Riferimenti aggiuntivi
- [IP versione 6](https://technet.microsoft.com/library/cc738636(v=ws.10).aspx)
- [Applicazioni IPv6](https://technet.microsoft.com/library/cc782509(v=ws.10).aspx)
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
