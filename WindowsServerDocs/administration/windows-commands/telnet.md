---
title: telnet
description: Argomento comandi di Windows per Telnet, che comunica con un computer che esegue il servizio Server Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b70a6156-9413-4300-84ce-a34c467e2b4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b59a3891dd276c6ab0b8e7a8a0a2d11a6b6b55c0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833134"
---
# <a name="telnet"></a>telnet

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comunica con un computer che esegue il servizio Server Telnet.
 
## <a name="syntax"></a>Sintassi
```
telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]
```
#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/a|tentativo di accesso automatico. Equivale all'opzione/l eccetto che usa il nome dell'utente attualmente connesso.|
|/e \<EscapeChar >|Carattere di escape utilizzato per immettere il prompt del client Telnet.|
|/f \<nomefile >|Nome file utilizzato per la registrazione lato client.|
|/l \<nome utente >|Specifica il nome utente con cui eseguire l'accesso nel computer remoto.|
|/t {VT100 &#124; VT52 &#124; ANSI &#124; VTNT}|Specifica il tipo di terminale. I tipi di terminali supportati sono VT100, VT52, ANSI e VTNT.|
|> host \<[> porta\<]|Specifica il nome host o l'indirizzo IP del computer remoto a cui connettersi e, facoltativamente, la porta TCP da usare (il valore predefinito è la porta TCP 23).|
|/?|Visualizza la guida al prompt dei comandi. In alternativa, è possibile digitare/h.|

## <a name="remarks"></a>Note
-   Prima di poter eseguire questo comando, è necessario installare il software client Telnet. Per ulteriori informazioni, vedere [installazione di Telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx).
-   È possibile eseguire Telnet senza parametri per immettere il contesto Telnet, indicato dal prompt dei comandi Telnet (**Microsoft telnet >** ). Dal prompt di Telnet è possibile utilizzare i comandi Telnet per gestire il computer che esegue il client Telnet.

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi
Utilizzare Telnet per connettersi al computer in cui è in esecuzione il servizio Server Telnet in telnet.microsoft.com.
```
telnet telnet.microsoft.com
```
Utilizzare Telnet per connettersi al computer in cui è in esecuzione il servizio Server Telnet in telnet.microsoft.com sulla porta TCP 44 e registrare l'attività della sessione in un file locale denominato telnetlog. txt.
```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>Altre informazioni di riferimento
-   [Installazione di Telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)
-   [Riferimento tecnico per Telnet](https://technet.microsoft.com/library/cc754987(v=ws.10).aspx)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
