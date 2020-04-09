---
title: finger
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 907ea637-5c6c-4752-84c2-46bbf2a68a33
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78313fc4980b32e3aeb6d1611ef80d7eb6831fc1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844604"
---
# <a name="finger"></a>finger

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le informazioni su un utente o su un computer remoto specificato (in genere un computer che esegue UNIX) che esegue il servizio Finger o daemon. Il computer remoto specifica il formato e l'output della visualizzazione di informazioni dell'utente. Se utilizzato senza parametri, **dito** Visualizza la Guida. 
## <a name="syntax"></a>Sintassi
```
finger [-l] [<User>] [@<Host>] [...]
```
#### <a name="parameters"></a>Parametri

| Parametro |                                                                            Descrizione                                                                            |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    -l     |                                                          Visualizza le informazioni utente in formato lungo.                                                           |
|  <User>   | Specifica l'utente sul quale si desiderano informazioni. Se si omette il *utente* parametro **dito** Visualizza informazioni su tutti gli utenti sul computer specificato. |
|  @<Host>  |        Specifica il computer remoto che esegue il servizio Finger in cui si stanno cercando le informazioni sull'utente. È possibile specificare un indirizzo IP o il nome del computer.        |
|    /?     |                                                               Visualizza la guida al prompt dei comandi.                                                                |

## <a name="remarks"></a>Note
È possibile specificare più parametri User@Host.
È necessario anteporre **dito** parametri con un trattino (-) anziché una barra (/).
Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.
Windows Server 2003 non fornisce un servizio con un dito.
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi
Per visualizzare informazioni per user1 nel computer utenti.microsoft.com, digitare:
```
finger user1@users.microsoft.com
```
Per visualizzare informazioni per tutti gli utenti del computer utenti.microsoft.com, digitare:
```
finger @users.microsoft.com
```
## <a name="additional-references"></a>Altre informazioni di riferimento
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
