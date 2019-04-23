---
title: telnet
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b70a6156-9413-4300-84ce-a34c467e2b4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6439a91d82d6d199666629e333d8130bf65a384b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858562"
---
# <a name="telnet"></a>telnet

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comunica con un computer che esegue il servizio Server telnet. 
## <a name="syntax"></a>Sintassi
```
telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/a|tentativo di accesso automatico. Uguale a /l opzione tranne Usa attualmente connesso in nome utente s.|
|/e \<EscapeChar >|Eseguire l'escape di caratteri utilizzato per accedere al prompt di client telnet.|
|/f \<FileName>|Nome del file usato per la registrazione lato client.|
|/l \<UserName >|Specifica il nome utente di accedere con il computer remoto.|
|/t {vt100 &#124; vt52 &#124; ansi &#124; vtnt}|Specifica il tipo di terminale. Tipi supportati terminali sono vt100 vt52, ansi e vtnt.|
|\<Host> [\<Port>]|Specifica il nome host o indirizzo IP del computer remoto per connettersi a e, facoltativamente, la porta TCP da utilizzare (valore predefinito è la porta TCP 23).|
|/?|Visualizza la guida al prompt dei comandi. In alternativa, è possibile digitare /h.|

## <a name="remarks"></a>Note
-   Prima di poter eseguire questo comando, è necessario installare il software client telnet. Per altre informazioni, vedere [installazione telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx).
-   È possibile eseguire telnet senza parametri per accedere al contesto di telnet, indicato dal prompt (**telnet Microsoft >**). Dal prompt di telnet, è possibile usare comandi telnet per gestire il computer che eseguono il client telnet.

## <a name="BKMK_Examples"></a>Esempi
Usare telnet per connettersi al computer che esegue il servizio Server telnet in telnet.microsoft.com.
```
telnet telnet.microsoft.com
```
Usare telnet per connettersi al computer che esegue il servizio Server telnet in telnet.microsoft.com sulla porta TCP 44 e log attività della sessione in un file locale denominato telnetlog.txt
```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Installare telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)
-   [Riferimento tecnico Telnet](https://technet.microsoft.com/library/cc754987(v=ws.10).aspx)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
