---
title: tscon
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6e53ea2888b66b9e4fbf026f752acf9803270fc2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392362"
---
# <a name="tscon"></a>tscon

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si connette a un'altra sessione in un server di host sessione Desktop remoto (host sessione Desktop remoto).  
Per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).  

> [!NOTE]  
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.  

## <a name="syntax"></a>Sintassi  
```  
tscon {<SessionID> | <SessionName>} [/dest:<SessionName>] [/password:<pw> | /password:*] [/v]  
```  
## <a name="parameters"></a>Parametri  

|Parametro|Descrizione|  
|-------|--------|  
|\<SessionID >|Specifica l'ID della sessione a cui si desidera connettersi. Se si usa il parametro facoltativo **/dest:** <*sessionname*>, si tratta dell'ID della sessione a cui si vuole connettersi.|  
|\<SessionName >|Specifica il nome della sessione a cui si desidera connettersi.|  
|/dest: > \<SessionName|Specifica il nome della sessione corrente. Questa sessione si disconnetterà quando ci si connette alla nuova sessione.|  
|/password: > \<PW|Consente di specificare la password dell'utente proprietario della sessione a cui si desidera connettersi. Questa password è obbligatoria quando l'utente che esegue la connessione non è il proprietario della sessione.|  
|/password: *|richiede la password dell'utente proprietario della sessione a cui si desidera connettersi.|  
|/v|Visualizza le informazioni sulle azioni eseguite.|  
|/?|Visualizza la guida al prompt dei comandi.|  

## <a name="remarks"></a>Note  
-   Per connettersi a un'altra sessione, è necessario disporre dell'autorizzazione controllo completo o della connessione di accesso speciale.  
-   Il parametro **/dest:** <*sessionname*> consente di connettere la sessione di un altro utente a una sessione diversa.  
-   Se non si specifica una password nel parametro <*password*> e la sessione di destinazione appartiene a un utente diverso da quello corrente, **tscon** ha esito negativo.  
-   Non è possibile connettersi alla sessione della console.  

## <a name="BKMK_examples"></a>Esempi  
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
  #### <a name="additional-references"></a>Altri riferimenti  
  [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  [Guida &#40;di riferimento&#41; ai comandi di Servizi Desktop remoto Servizi terminal](remote-desktop-services-terminal-services-command-reference.md)  
