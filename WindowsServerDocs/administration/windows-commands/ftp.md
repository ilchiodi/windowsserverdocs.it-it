---
title: ftp
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 758335e1-fd8d-448c-a654-993126239dd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8fa956124e0c227d048d4c6eec844154187d5861
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725050"
---
# <a name="ftp"></a>ftp

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Trasferisce i file da e verso un computer che esegue un servizio server FTP (File Transfer Protocol). **FTP** può essere utilizzato in modo interattivo o in modalità batch elaborando file di testo ASCII. 
## <a name="syntax"></a>Sintassi
```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<FileName>] [-a] [-A] [-x:<SendBuffer>] [-r:<RecvBuffer>] [-b:<AsyncBuffers>][-w:<WindowsSize>]  [-?] [<Host>]
```
#### <a name="parameters"></a>Parametri

|     Parametro     |                                                                                                                                                      Descrizione                                                                                                                                                      |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        -v         |                                                                                                                                    Elimina visualizzazione delle risposte del server remoto.                                                                                                                                     |
|        -d         |                                                                                                               Abilita il debug, visualizzando tutti i comandi passati tra il client FTP e il server FTP.                                                                                                                |
|        -i         |                                                                                                                            Disabilita la richiesta interattiva durante i trasferimenti di più file.                                                                                                                             |
|        -n         |                                                                                                                                    Disattiva accesso automatico al momento della connessione iniziale.                                                                                                                                     |
|        -g         |                                         Disabilita il glob dei nomi di file.  Il **glob** consente di usare l'asterisco (\*) e il punto interrogativo (?) come caratteri jolly nei nomi di file e percorsi locali. Per ulteriori informazioni, vedere [riferimenti aggiuntivi](ftp.md#BKMK_additionalRef).                                          |
|   s<FileName>   | Specifica un file di testo contenente **ftp** comandi. Questi comandi vengono eseguiti automaticamente dopo **ftp** viene avviato. Questo parametro non consente spazi. Utilizzare questo parametro anziché il reindirizzamento (**<**). **Nota:** In Windows 8 e Windows Server 2012 o versioni successive, il file di testo deve essere scritto in formato UTF-8. |
|        -a         |                                                                                                                 Specifica che è possibile utilizzare qualsiasi interfaccia locale quando si associa la connessione dati FTP.                                                                                                                  |
|        -A         |                                                                                                                                        Accede al server FTP come anonimo.                                                                                                                                         |
|  x<SendBuffer>  |                                                                                                                                     Esegue l'override di quelle predefinite SO_SNDBUF di 8192.                                                                                                                                     |
|  r<RecvBuffer>  |                                                                                                                                     Esegue l'override di quelle predefinite SO_RCVBUF di 8192.                                                                                                                                     |
| b<AsyncBuffers> |                                                                                                                                    Sostituisce il numero di buffer predefinito async 3.                                                                                                                                     |
| w<WindowsSize>  |                                                                                                                   Specifica la dimensione del buffer di trasferimento. La dimensione predefinita è 4096 byte.                                                                                                                   |
|        -?         |                                                                                                                                         Visualizza la guida al prompt dei comandi.                                                                                                                                          |
|      <host>       |                                                                    Specifica il nome del computer, l'indirizzo IP o l'indirizzo IPv6 del server FTP a cui connettersi. Il nome host o l'indirizzo, se specificato, deve essere l'ultimo parametro della riga.                                                                    |

## <a name="remarks"></a>Osservazioni
- Per ulteriori informazioni sui comandi **FTP** in Windows Server 2003, vedere [FTP](https://technet.microsoft.com/library/cc756013(v=ws.10).aspx).
- i parametri della riga di comando **FTP** fanno distinzione tra maiuscole e minuscole.
- Questo comando è disponibile solo se il **protocollo Internet (TCP/IP)** è installato come un componente nelle proprietà di una scheda di rete in connessioni di rete.
- **FTP** può essere utilizzato in modo interattivo. Dopo l'avvio, **ftp** Crea un ambiente secondario in cui è possibile utilizzare **ftp** comandi. È possibile tornare al prompt dei comandi digitando il **chiudere** comando. Quando il **ftp** ambiente secondario è in esecuzione, è indicata dal **ftp >** prompt dei comandi. Per ulteriori informazioni vedere il **ftp** comandi.
- **FTP** supporta l'utilizzo di IPv6 quando il protocollo IPv6 è installato. Per ulteriori informazioni, vedere [riferimenti aggiuntivi](ftp.md#BKMK_additionalRef).
  ## <a name="examples"></a>Esempi
  Per accedere al server FTP denominato ftp.example.microsoft.com, digitare:
  ```
  ftp ftp.example.microsoft.com
  ```
  Per accedere al server FTP denominato ftp.example.microsoft.com ed eseguire i comandi **FTP** contenuti in un file denominato Resync. txt, digitare:
  ```
  ftp -s:resync.txt ftp.example.microsoft.com
  ```
  ## <a name="additional-references"></a><a name=BKMK_additionalRef></a>riferimenti aggiuntivi
- [IP versione 6](https://technet.microsoft.com/library/cc738636(v=ws.10).aspx)
- [Applicazioni IPv6](https://technet.microsoft.com/library/cc782509(v=ws.10).aspx)
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
