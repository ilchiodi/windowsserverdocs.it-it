---
title: sessione di query
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abc0ace8-0b74-4b6e-a937-a78bb4b61a1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bde9a246f2c46eaa466f2863c2cfc3c28a3a04eb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384907"
---
# <a name="query-session"></a>sessione di query

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni sulle sessioni in un server di host sessione Desktop remoto (host sessione Desktop remoto).
L'elenco include informazioni non solo sulle sessioni attive, ma anche su altre sessioni eseguite dal server.
per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
> ## <a name="syntax"></a>Sintassi
> ```
> query session [<SessionName> | <UserName> | <SessionID>] [/server:<ServerName>] [/mode] [/flow] [/connect] [/counter]
> ```
> ## <a name="parameters"></a>Parametri
> 
> |      Parametro       |                                                      Descrizione                                                      |
> |----------------------|-----------------------------------------------------------------------------------------------------------------------|
> |    <SessionName>     |                               Specifica il nome della sessione su cui si desidera eseguire una query.                               |
> |      <UserName>      |                           Specifica il nome dell'utente di cui si desidera eseguire una query per le sessioni.                            |
> |     <SessionID>      |                                Specifica l'ID della sessione su cui si desidera eseguire una query.                                |
> | /server:<ServerName> |                  Identifica il server Host sessione Desktop remoto su cui eseguire la query. Il valore predefinito è il server corrente.                   |
> |        /Mode         |                                            Visualizza le impostazioni correnti della riga.                                            |
> |        /flow         |                                        Visualizza le impostazioni di controllo del flusso corrente.                                        |
> |       /Connect       |                                          Visualizza le impostazioni di connessione correnti.                                           |
> |       /Counter       | Visualizza le informazioni sui contatori correnti, incluso il numero totale di sessioni create, disconnesse e riconnesse. |
> |          /?          |                                         Visualizza la guida al prompt dei comandi.                                          |
> 
> ## <a name="remarks"></a>Osservazioni
> - Un utente può sempre eseguire una query sulla sessione a cui l'utente è attualmente connesso. Per eseguire una query su altre sessioni, l'utente deve disporre dell'autorizzazione di accesso speciale per informazioni sulle query.
> - Se non si specifica una sessione usando <*sessionname*>, <*UserName*> o <*SessionID*>, **Session query** Visualizza le informazioni su tutte le sessioni attive nel sistema.
> - Quando la **sessione di query** restituisce informazioni, viene visualizzato un simbolo di maggiore (>) prima della sessione corrente. Di seguito è riportato l'output di esempio per la **sessione di query**:
>   ```
>   C:\>query session
>    SESSIONNAME    USERNAME       ID STATE  TYPE   DEVICE
>   console        Administrator1  0 active wdcon
>    rdp-tcp#1      User1           1 active wdtshare
>    rdp-tcp                        2 listen wdtshare
>                                   4 idle
>                                   5 idle
>   ```
>   Il simbolo di maggiore di (>) indica la sessione corrente. SESSIONname specifica il nome assegnato alla sessione. USERNAME indica il nome utente dell'utente connesso alla sessione. LO stato fornisce informazioni sullo stato corrente della sessione. Il tipo indica il tipo di sessione. Il dispositivo, che non è presente per la console o le sessioni connesse alla rete, è il nome del dispositivo assegnato alla sessione. Il commento che segue le informazioni di sessione viene dal profilo della sessione. Tutte le sessioni in cui lo stato iniziale è configurato come DISABILITAto non vengono visualizzate nell'elenco della **sessione di query** finché non vengono abilitate.
>   ## <a name="BKMK_examples"></a>Esempi
> - Per visualizzare informazioni su tutte le sessioni attive nel server SERver2, digitare:
>   ```
>   query session /server:SERver2
>   ```
> - Per visualizzare le informazioni sulla modeM02 della sessione attiva, digitare:
>   ```
>   query session modeM02
>   ```
>   #### <a name="additional-references"></a>riferimenti aggiuntivi
>   [Sintassi della riga di comando chiave](command-line-syntax-key.md)
>   [query](query.md)
>   [Servizi Desktop remoto &#40;riferimento&#41; ai comandi di Servizi terminal](remote-desktop-services-terminal-services-command-reference.md)
