---
title: utente query
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a670fb78-c055-464a-b61d-3a85632c52c5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6624c559bc85263da955f993ae7e4ad7e8b9ee2d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836804"
---
# <a name="query-user"></a>utente query

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni sulle sessioni utente in un server Host sessione Desktop remoto (host sessione Desktop remoto).
per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).
> [!NOTE]
> In Windows Server 2008 R2 Servizi terminal è stato rinomato Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
> ## <a name="syntax"></a>Sintassi
> ```
> query user [<UserName> | <SessionName> | <SessionID>] [/server:<ServerName>]
> ```
> ### <a name="parameters"></a>Parametri
> 
> |      Parametro       |                                                     Descrizione                                                     |
> |----------------------|---------------------------------------------------------------------------------------------------------------------|
> |      <UserName>      |                            Specifica il nome di accesso dell'utente su cui si desidera eseguire la query.                             |
> |    <SessionName>     |                              Specifica il nome della sessione su cui si desidera eseguire una query.                              |
> |     <SessionID>      |                               Specifica l'ID della sessione su cui si desidera eseguire una query.                               |
> | /server:<ServerName> | Specifica il server Host sessione Desktop remoto su cui si desidera eseguire la query. In caso contrario, viene utilizzato il server Host sessione Desktop remoto corrente. |
> |          /?          |                                        Visualizza la guida al prompt dei comandi.                                         |
> 
> ## <a name="remarks"></a>Note
> - È possibile utilizzare questo comando per verificare se un utente specifico è connesso a un server Host sessione Desktop remoto specifico. **query user** restituisce le informazioni seguenti:
>   -   Il nome dell'utente
>   -   Nome della sessione nel server Host sessione Desktop remoto
>   -   ID sessione
>   -   Stato della sessione (attivo o disconnesso)
>   -   Tempo di inattività (il numero di minuti trascorsi dall'ultimo movimento di tasto o del mouse nella sessione)
>   -   Data e ora in cui l'utente ha effettuato l'accesso
> - Per utilizzare l' **utente di query**, è necessario disporre dell'autorizzazione controllo completo o delle informazioni di query speciali di accesso.
> - Se si utilizza **query utente** senza specificare <*UserName*>, <*sessionname*> o <*SessionID*>, viene restituito un elenco di tutti gli utenti connessi al server. In alternativa, è possibile usare anche la **sessione di query** per visualizzare un elenco di tutte le sessioni in un server.
> - Quando l' **utente di query** restituisce informazioni, viene visualizzato un simbolo di maggiore (>) prima della sessione corrente.
> - Il parametro **/Server** è obbligatorio solo se si usa **query utente** da un server remoto.
>   ## <a name="examples"></a><a name=BKMK_examples></a>Esempi
> - Per visualizzare informazioni su tutti gli utenti connessi al sistema, digitare:
>   ```
>   query user
>   ```
> - Per visualizzare informazioni sull'utente USER1 sul server SERver1, digitare:
>   ```
>   query user USER1 /server:SERver1
>   ```
>   ## <a name="additional-references"></a>Altre informazioni di riferimento
>   - Guida di riferimento ai comandi della [sintassi della riga di comando](command-line-syntax-key.md)
>   [query](query.md)
>   [Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
