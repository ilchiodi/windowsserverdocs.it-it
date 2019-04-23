---
title: tscon
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 315a9793-cd10-4987-bb68-89a9d13f7fce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8267a35aefc279ff57ce3d200415e16e431775f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883062"
---
# <a name="tscon"></a>tscon

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si connette a un'altra sessione in un server Host sessione Desktop remoto (Host sessione rd).  
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).  

> [!NOTE]  
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.  

## <a name="syntax"></a>Sintassi  
```  
tscon {<SessionID> | <SessionName>} [/dest:<SessionName>] [/password:<pw> | /password:*] [/v]  
```  
## <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|\<SessionID >|Specifica l'ID della sessione a cui si desidera connettersi. Se si usa l'opzione facoltativa **/dest:**<*SessionName*> parametro, questo è l'ID della sessione a cui si desidera connettersi.|  
|\<SessionName >|Specifica il nome della sessione a cui si desidera connettersi.|  
|/dest:\<SessionName >|Specifica il nome della sessione corrente. Questa sessione verrà disconnessa quando ci si connette alla nuova sessione.|  
|/password:\<pw>|Specifica la password dell'utente che possiede la sessione a cui si desidera connettersi. Questa password è obbligatoria quando l'utente che si connette il proprietario della sessione.|  
|/password:*|richiede la password dell'utente che possiede la sessione a cui si desidera connettersi.|  
|/v|Consente di visualizzare informazioni sulle azioni eseguibili in esecuzione.|  
|/?|Visualizza la guida al prompt dei comandi.|  

## <a name="remarks"></a>Note  
-   È necessario disporre dell'autorizzazione controllo completo o l'accesso speciale autorizzazione per connettersi a un'altra sessione di connessione.  
-   Il **/dest:**<*SessionName*> parametro consente di connettere la sessione di un altro utente a un'altra sessione.  
-   Se non si specifica una password di <*Password*> parametro e la sessione di destinazione appartiene a un utente diverso da quello corrente, **tscon** ha esito negativo.  
-   Non è possibile connettersi alla sessione della console.  

## <a name="BKMK_examples"></a>Esempi  
-   Per connettersi alla sessione 12 nel server Host sessione Desktop remoto corrente e disconnettere la sessione corrente, digitare:  
    ```  
    tscon 12  
    ```  
-   Per connettersi alla sessione 23 nel server Host sessione Desktop remoto corrente, usando ad esempio la password miapass e disconnettere la sessione corrente, digitare:  
    ```  
    tscon 23 /password:mypass  
    ```  
-   Per connettere la sessione TERM03 alla sessione TERM05 e quindi disconnettere la sessione TERM05, se è connesso, digitare:  
    ```  
    tscon TERM03 /v /dest:TERM05  
    ```  
#### <a name="additional-references"></a>Altri riferimenti  
[Chiave sintassi della riga di comando](command-line-syntax-key.md)  
[Servizi Desktop remoto &#40;servizi Terminal&#41; Guida comandi](remote-desktop-services-terminal-services-command-reference.md)  
