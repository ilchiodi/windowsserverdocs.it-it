---
title: ftp
description: Argomento di riferimento per il comando FTP, che trasferisce i file da e verso un computer che esegue un servizio server FTP (File Transfer Protocol).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 758335e1-fd8d-448c-a654-993126239dd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3920306ce05aeb1b1e364c8146c461ea187f6560
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820241"
---
# <a name="ftp"></a>ftp

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Trasferisce i file da e verso un computer che esegue un servizio server FTP (File Transfer Protocol). Questo comando può essere usato in modo interattivo o in modalità batch tramite l'elaborazione di file di testo ASCII.

## <a name="syntax"></a>Sintassi

```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<filename>] [-a] [-A] [-x:<sendbuffer>] [-r:<recvbuffer>] [-b:<asyncbuffers>][-w:<windowssize>][<host>] [-?]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ----------| ----------- |
| -v | Elimina visualizzazione delle risposte del server remoto. |
| -d | Abilita il debug, visualizzando tutti i comandi passati tra il client FTP e il server FTP. |
| -i | Disabilita la richiesta interattiva durante i trasferimenti di più file. |
| -n | Disattiva accesso automatico al momento della connessione iniziale. |
| -g | Disabilita il glob dei nomi di file.  **Glob** consente di utilizzare l'asterisco (*) e un punto interrogativo (?) come caratteri jolly nei nomi di file e percorso locali. |
| s`<filename>` | Specifica un file di testo contenente **ftp** comandi. Questi comandi vengono eseguiti automaticamente dopo **ftp** viene avviato. Questo parametro non consente spazi. Utilizzare questo parametro anziché il reindirizzamento (`<`). **Nota:** In Windows 8 e Windows Server 2012 o versioni successive, il file di testo deve essere scritto in formato UTF-8. |
| -a | Specifica che è possibile utilizzare qualsiasi interfaccia locale quando si associa la connessione dati FTP. |
| -A | Accede al server FTP come anonimo. |
| x`<sendbuffer> `| Esegue l'override di quelle predefinite SO_SNDBUF di 8192. |
| r`<recvbuffer>` | Esegue l'override di quelle predefinite SO_RCVBUF di 8192. |
| b`<asyncbuffers>` | Sostituisce il numero di buffer predefinito async 3. |
| w`<windowssize>` | Specifica la dimensione del buffer di trasferimento. La dimensione predefinita è 4096 byte. |
| `<host>` | Specifica il nome del computer, l'indirizzo IP o l'indirizzo IPv6 del server FTP a cui connettersi. Il nome host o l'indirizzo, se specificato, deve essere l'ultimo parametro della riga. |
| -? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- I parametri della riga di comando **FTP** fanno distinzione tra maiuscole e minuscole.

- Questo comando è disponibile solo se il **protocollo Internet (TCP/IP)** è installato come un componente nelle proprietà di una scheda di rete in connessioni di rete.

- Il comando **FTP** può essere utilizzato in modo interattivo. Dopo l'avvio, **ftp** Crea un ambiente secondario in cui è possibile utilizzare **ftp** comandi. È possibile tornare al prompt dei comandi digitando il **chiudere** comando. Quando l'ambiente secondario **FTP** è in esecuzione, viene indicato dal prompt dei `ftp >` comandi. Per ulteriori informazioni, vedere i comandi **FTP** .

- Il comando **FTP** supporta l'utilizzo di IPv6 quando il protocollo IPv6 è installato.

### <a name="examples"></a>Esempi

Per accedere al server FTP denominato `ftp.example.microsoft.com` , digitare:

```
ftp ftp.example.microsoft.com
```

Per accedere al server FTP denominato `ftp.example.microsoft.com` ed eseguire i comandi **FTP** contenuti in un file denominato *Resync. txt*, digitare:

```
ftp -s:resync.txt ftp.example.microsoft.com
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))

- [IP versione 6](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc738636(v=ws.10))

- [Applicazioni IPv6](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc782509(v=ws.10))
