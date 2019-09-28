---
title: reset session
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 67a4e910ba87209c9700f2242f7859a6cc9e725f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384533"
---
# <a name="reset-session"></a>reset session

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di reimpostare (eliminare) una sessione in un server Host sessione Desktop remoto (host sessione Desktop remoto).  
Per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).  

> [!NOTE]  
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.  

## <a name="syntax"></a>Sintassi  
```  
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]  
```  

## <a name="parameters"></a>Parametri  

|Parametro|Descrizione|  
|-------|--------|  
|\<SessionName >|Specifica il nome della sessione che si desidera reimpostare. Per determinare il nome della sessione, utilizzare il **query sessione** comando.|  
|\<SessionID >|Specifica l'ID della sessione da reimpostare.|  
|/Server: \<ServerName >|Specifica il server terminal contenente la sessione che si desidera reimpostare. In caso contrario, viene utilizzato il server Host sessione Desktop remoto corrente.|  
|/v|Visualizza le informazioni sulle azioni eseguite.|  
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
[Guida &#40;di riferimento&#41; ai comandi di Servizi Desktop remoto Servizi terminal](remote-desktop-services-terminal-services-command-reference.md)  
