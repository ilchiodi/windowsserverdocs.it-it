---
title: finger
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 907ea637-5c6c-4752-84c2-46bbf2a68a33
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec8040480a7cb75a5a42e051393e3db4a47f8e2f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725610"
---
# <a name="finger"></a>finger

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="remarks"></a>Osservazioni
È User@Host possibile specificare più parametri.
È necessario anteporre **dito** parametri con un trattino (-) anziché una barra (/).
Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.
Windows Server 2003 non fornisce un servizio con un dito.
## <a name="examples"></a>Esempi
Per visualizzare informazioni per user1 nel computer utenti.microsoft.com, digitare:
```
finger user1@users.microsoft.com
```
Per visualizzare informazioni per tutti gli utenti del computer utenti.microsoft.com, digitare:
```
finger @users.microsoft.com
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
