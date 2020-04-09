---
title: tscon
description: Windows Commands argomento per tscon, che si connette a un'altra sessione in un host sessione Desktop remoto server (host sessione Desktop remoto).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 315a9793-cd10-4987-bb68-89a9d13f7fce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1666e3fd47fab89b63efccef43489d15930cd6de
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832534"
---
# <a name="tscon"></a>tscon

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si connette a un'altra sessione in un server host sessione Desktop remoto.  

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).  

> [!NOTE]  
> In Windows Server 2008 R2 Servizi terminal è stato rinomato Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.  

## <a name="syntax"></a>Sintassi  
```  
tscon {<SessionID> | <SessionName>} [/dest:<SessionName>] [/password:<pw> | /password:*] [/v]  
```  
### <a name="parameters"></a>Parametri  

|Parametro|Descrizione|  
|-------|--------|  
|\<SessionID >|Specifica l'ID della sessione a cui si desidera connettersi. Se si usa il parametro facoltativo **/dest:** <*sessionname*>, si tratta dell'ID della sessione a cui si vuole connettersi.|  
|\<sessionname >|Specifica il nome della sessione a cui si desidera connettersi.|  
|/dest:\<sessionname >|Specifica il nome della sessione corrente. Questa sessione si disconnetterà quando ci si connette alla nuova sessione.|  
|/password:\<PW >|Consente di specificare la password dell'utente proprietario della sessione a cui si desidera connettersi. Questa password è obbligatoria quando l'utente che esegue la connessione non è il proprietario della sessione.|  
|/password: *|richiede la password dell'utente proprietario della sessione a cui si desidera connettersi.|  
|/v|Visualizza le informazioni sulle azioni eseguite.|  
|/?|Visualizza la guida al prompt dei comandi.|  

## <a name="remarks"></a>Note  
-   Per connettersi a un'altra sessione, è necessario disporre dell'autorizzazione controllo completo o della connessione di accesso speciale.  
-   Il parametro **/dest:** <*sessionname*> consente di connettere la sessione di un altro utente a una sessione diversa.  
-   Se non si specifica una password nel parametro <*password*> e la sessione di destinazione appartiene a un utente diverso da quello corrente, **tscon** ha esito negativo.  
-   Non è possibile connettersi alla sessione della console.  

## <a name="examples"></a><a name=BKMK_examples></a>Esempi  
- Per connettersi alla sessione 12 sul server Host sessione Desktop remoto corrente e disconnettere la sessione corrente, digitare:  
  ```  
  tscon 12  
  ```  
- Per connettersi alla sessione 23 nel server Host sessione Desktop remoto corrente, usando il pass-through della password e disconnettere la sessione corrente, digitare:  
  ```  
  tscon 23 /password:mypass  
  ```  
- Per connettere la sessione denominata TERM03 alla sessione denominata TERM05, quindi disconnettere la sessione TERM05, se è connessa, digitare:  
  ```  
  tscon TERM03 /v /dest:TERM05  
  ```  
  ## <a name="additional-references"></a>Altre informazioni di riferimento  
  - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  [Informazioni di riferimento sui comandi di Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)  
