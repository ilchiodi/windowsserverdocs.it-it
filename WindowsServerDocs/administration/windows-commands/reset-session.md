---
title: reset session
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 5a0991c76ba890bb94b0dcf258df6207ed228e72
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441790"
---
# <a name="reset-session"></a>reset session

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di reimpostare (eliminare) una sessione in un server Host sessione Desktop remoto (Host sessione rd).  
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).  

> [!NOTE]  
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.  

## <a name="syntax"></a>Sintassi  
```  
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]  
```  

## <a name="parameters"></a>Parametri  

|Parametro|Descrizione|  
|-------|--------|  
|\<SessionName >|Specifica il nome della sessione che si desidera reimpostare. Per determinare il nome della sessione, utilizzare il **query sessione** comando.|  
|\<SessionID >|Specifica l'ID della sessione da reimpostare.|  
|/server:\<ServerName>|Specifica il server terminal contenente la sessione che si desidera reimpostare. In caso contrario, viene usato il server Host sessione Desktop remoto corrente.|  
|/v|Consente di visualizzare informazioni sulle azioni eseguibili in esecuzione.|  
|/?|Visualizza la guida al prompt dei comandi.|  

## <a name="remarks"></a>Note  
-   È sempre possibile reimpostare le proprie sessioni, ma è necessario disporre dell'autorizzazione controllo completo per reimpostare la sessione di un altro utente.  
-   Tenere presente che la reimpostazione di una sessione utente senza avvisare l'utente può comportare la perdita di dati nella sessione.  
-   È consigliabile reimpostare una sessione solo quando non viene eseguita correttamente o risulta non rispondono.  
-   Il **/server** parametro è obbligatorio solo se si utilizza **reimpostazione sessione** da un server remoto.  

## <a name="BKMK_examples"></a>Esempi  
- Per reimpostare la sessione rdp-tcp #6, digitare:  
  ```  
  reset session rdp-tcp#6  
  ```  
- Per reimpostare la sessione che Usa sessione ID 3, digitare:  
  ```  
  reset session 3  
  ```  

#### <a name="additional-references"></a>Altri riferimenti  
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
[Servizi Desktop remoto &#40;servizi Terminal&#41; Guida comandi](remote-desktop-services-terminal-services-command-reference.md)  
