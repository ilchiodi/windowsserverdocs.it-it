---
title: sessione di query
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1b96f7e6a6252b40e5a32910d161b1fa0ff94e11
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868892"
---
# <a name="query-session"></a>sessione di query

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni sulle sessioni in un server Host sessione Desktop remoto (Host sessione rd).
L'elenco include le informazioni non solo sulle sessioni attive, ma anche su altre sessioni che il server in esecuzione.
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
## <a name="syntax"></a>Sintassi
```
query session [<SessionName> | <UserName> | <SessionID>] [/server:<ServerName>] [/mode] [/flow] [/connect] [/counter]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|<SessionName>|Specifica il nome della sessione che si desidera eseguire una query.|
|<UserName>|Specifica il nome dell'utente il cui si desidera eseguire una query per le sessioni.|
|<SessionID>|Specifica l'ID della sessione che si desidera eseguire una query.|
|/server:<ServerName>|Identifica il server Host sessione Desktop remoto alla query. Il valore predefinito è il server corrente.|
|/Mode|Consente di visualizzare le impostazioni della riga corrente.|
|/flow|Consente di visualizzare le impostazioni di controllo del flusso corrente.|
|/ connect|Impostazioni di connessione consente di visualizzare corrente.|
|/counter|Consente di visualizzare informazioni sui contatori corrente, incluso il numero totale di sessioni creato, disconnesso e riconnesso.|
|/?|Visualizza la guida al prompt dei comandi.|
## <a name="remarks"></a>Note
-   Un utente può richiedere sempre la sessione a cui l'utente attualmente connesso. Per eseguire query su altre sessioni, l'utente deve disporre dell'autorizzazione di accesso speciali di informazioni di query.
-   Se non si specifica una sessione con <*SessionName*>, <*UserName*>, o <*SessionID*>, **query sessione** Visualizza informazioni su tutte le sessioni attive nel sistema.
-   Quando **query sessione** restituisce informazioni, un simbolo di maggiore (>) viene visualizzate prima della sessione corrente. Di seguito è l'output di esempio per **query sessione**:
    ```
    C:\>query session
     SESSIONNAME    USERNAME       ID STATE  TYPE   DEVICE
    >console        Administrator1  0 active wdcon
     rdp-tcp#1      User1           1 active wdtshare
     rdp-tcp                        2 listen wdtshare
                                    4 idle
                                    5 idle
    ```
    Il simbolo di maggiore (>) indica la sessione corrente. Nome sessione specifica il nome assegnato alla sessione. Nome utente indica il nome utente dell'utente connesso alla sessione. STATO fornisce informazioni sullo stato corrente della sessione. TIPO indica il tipo di sessione. DISPOSITIVO, che non è presente per la console o le sessioni connesse tramite rete, è il nome del dispositivo assegnato alla sessione. Il commento che segue le informazioni sulla sessione è nel profilo della sessione. Tutte le sessioni in cui lo stato iniziale è configurato come disattivato non vengono visualizzati nei **query sessione** elencare fino a quando non sono abilitati.
## <a name="BKMK_examples"></a>Esempi
-   Per visualizzare informazioni su tutte le sessioni attive nel server, SERver2, digitare:
    ```
    query session /server:SERver2
    ```
-   Per visualizzare informazioni sulle sessioni attive modeM02, digitare:
    ```
    query session modeM02
    ```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[query](query.md)
[Remote Desktop Services &#40;servizi Terminal&#41; riferimenti ai comandi](remote-desktop-services-terminal-services-command-reference.md)
