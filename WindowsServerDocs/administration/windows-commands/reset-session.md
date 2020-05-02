---
title: reset session
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: df7b953e02c7339b7ed66a831955f802dd21e624
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722364"
---
# <a name="reset-session"></a>reset session

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di reimpostare (eliminare) una sessione in un server Host sessione Desktop remoto (host sessione Desktop remoto).  
  

> [!NOTE]  
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.  

## <a name="syntax"></a>Sintassi  
```  
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]  
```  

### <a name="parameters"></a>Parametri  

|Parametro|Descrizione|  
|-------|--------|  
|\<Sessionname>|Specifica il nome della sessione che si desidera reimpostare. Per determinare il nome della sessione, utilizzare il **query sessione** comando.|  
|\<> SessionID|Specifica l'ID della sessione da reimpostare.|  
|/Server:\<nomeserver>|Specifica il server terminal contenente la sessione che si desidera reimpostare. In caso contrario, viene utilizzato il server Host sessione Desktop remoto corrente.|  
|/v|Visualizza le informazioni sulle azioni eseguite.|  
|/?|Visualizza la guida al prompt dei comandi.|  

## <a name="remarks"></a>Osservazioni  
-   È sempre possibile reimpostare le proprie sessioni, ma è necessario disporre dell'autorizzazione controllo completo per reimpostare la sessione di un altro utente.  
-   Tenere presente che la reimpostazione di una sessione utente senza avvisare l'utente può comportare la perdita di dati nella sessione.  
-   È consigliabile reimpostare una sessione solo quando non viene eseguita correttamente o risulta non rispondono.  
-   Il **/server** parametro è obbligatorio solo se si utilizza **reimpostazione sessione** da un server remoto.  

## <a name="examples"></a>Esempi  
- Per reimpostare la sessione rdp-tcp #6, digitare:  
  ```  
  reset session rdp-tcp#6  
  ```  
- Per reimpostare la sessione che Usa sessione ID 3, digitare:  
  ```  
  reset session 3  
  ```  

## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
[Informazioni di riferimento sui comandi di Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)  
