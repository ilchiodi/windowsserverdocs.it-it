---
title: query utente
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a670fb78-c055-464a-b61d-3a85632c52c5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c6e0d936e03e14e134c7bfd450a878fda1a796c4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442006"
---
# <a name="query-user"></a>query utente

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni sulle sessioni utente in un server Host sessione Desktop remoto (Host sessione rd).
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
> ## <a name="syntax"></a>Sintassi
> ```
> query user [<UserName> | <SessionName> | <SessionID>] [/server:<ServerName>]
> ```
> ## <a name="parameters"></a>Parametri
> 
> |      Parametro       |                                                     Descrizione                                                     |
> |----------------------|---------------------------------------------------------------------------------------------------------------------|
> |      <UserName>      |                            Specifica il nome di accesso dell'utente che si desidera eseguire una query.                             |
> |    <SessionName>     |                              Specifica il nome della sessione che si desidera eseguire una query.                              |
> |     <SessionID>      |                               Specifica l'ID della sessione che si desidera eseguire una query.                               |
> | /server:<ServerName> | Specifica il server Host sessione Desktop remoto che si desidera eseguire una query. In caso contrario, viene usato il server Host sessione Desktop remoto corrente. |
> |          /?          |                                        Visualizza la guida al prompt dei comandi.                                         |
> 
> ## <a name="remarks"></a>Note
> - È possibile utilizzare questo comando per sapere se un utente specifico è connesso a un server Host sessione Desktop remoto specifico. **query utente** restituisce le informazioni seguenti:
>   -   Il nome dell'utente
>   -   Il nome della sessione nel server Host sessione Desktop remoto
>   -   L'ID di sessione
>   -   Lo stato della sessione (attivo o disconnesse)
>   -   Il tempo di inattività (il numero di minuti dopo l'ultima sequenza di tasti o movimento del mouse nella sessione)
>   -   Data e ora l'utente connesso
> - Per utilizzare **query utente**, è necessario disporre dell'autorizzazione controllo completo o informazioni speciali autorizzazioni di accesso di query.
> - Se si usa **query utente** senza specificare <*UserName*>, <*SessionName*>, o <*SessionID*>, un elenco di tutti gli utenti sono connessi al server viene restituito. In alternativa, è anche possibile usare **query sessione** per visualizzare un elenco di tutte le sessioni in un server.
> - Quando **query utente** restituisce informazioni, un simbolo di maggiore (>) viene visualizzate prima della sessione corrente.
> - Il **/server** parametro è obbligatorio solo se si usa **query utente** da un server remoto.
>   ## <a name="BKMK_examples"></a>Esempi
> - Per visualizzare informazioni su tutti gli utenti registrati nel sistema, digitare:
>   ```
>   query user
>   ```
> - Per visualizzare informazioni sull'utente USER1 nel server denominato SERver1, digitare:
>   ```
>   query user USER1 /server:SERver1
>   ```
>   #### <a name="additional-references"></a>Riferimenti aggiuntivi
>   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
>   [query](query.md)
>   [Remote Desktop Services &#40;servizi Terminal&#41; riferimenti ai comandi](remote-desktop-services-terminal-services-command-reference.md)
