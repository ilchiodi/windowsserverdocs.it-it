---
title: processo query
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 36ce3ffc-0092-4eb1-a374-28e6616ca946
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81132ebf6b75115086ed7cc2ab9f73d9d06e65e4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722714"
---
# <a name="query-process"></a>processo query

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni sui processi in esecuzione in un server Host sessione Desktop remoto (host sessione Desktop remoto).
È possibile utilizzare questo comando per individuare i programmi eseguiti da un utente specifico e anche gli utenti che eseguono un programma specifico.

> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
> ## <a name="syntax"></a>Sintassi
> ```
> query process [* | <ProcessID> | <UserName> | <SessionName> | /id:<nn> | <ProgramName>] [/server:<ServerName>]
> ```
> ### <a name="parameters"></a>Parametri
> 
> |      Parametro       |                                                                 Descrizione                                                                  |
> |----------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
> |          \*          |                                                    elenca i processi per tutte le sessioni.                                                     |
> |     <ProcessID>      |                                   Specifica l'ID numerico che identifica il processo su cui si desidera eseguire la query.                                   |
> |      <UserName>      |                                       Specifica il nome dell'utente di cui si desidera visualizzare i processi.                                       |
> |    <SessionName>     |                                     Specifica il nome della sessione di cui si desidera visualizzare i processi.                                      |
> |       /ID<nn>       |                                      Specifica l'ID della sessione di cui si desidera visualizzare i processi.                                       |
> |    <ProgramName>     |                     Specifica il nome del programma di cui si desidera eseguire una query sui processi. L'estensione exe è obbligatoria.                     |
> | /server:<ServerName> | Specifica il server Host sessione Desktop remoto di cui si desidera visualizzare i processi. Se non è specificato, viene utilizzato il server in cui si è attualmente connessi. |
> |          /?          |                                                     Visualizza la guida al prompt dei comandi.                                                     |
> 
> ## <a name="remarks"></a>Osservazioni
> - Gli amministratori hanno accesso completo a tutte le funzioni di **processo di query** .
> - Se non si specifica il *nome utente* <>, <*sessionname*>, **/ID:**<*nn*>, <*ProgramName*> o **\\*** Parameters, il **processo di query** Visualizza solo i processi che appartengono all'utente corrente.
> - Se viene specificata una sessione, è necessario che identifichi una sessione attiva.
> - il **processo di query** restituisce le informazioni seguenti:
>   -   Utente proprietario del processo
>   -   La sessione proprietaria del processo
>   -   ID della sessione
>   -   Nome del processo
>   -   ID del processo
> - Quando il **processo di query** restituisce informazioni, viene visualizzato un simbolo di maggiore (>) prima di ogni processo appartenente alla sessione corrente.
>   ## <a name="examples"></a>Esempi
> - Per visualizzare informazioni sui processi utilizzati da tutte le sessioni, digitare:
>   ```
>   query process *
>   ```
> - Per visualizzare informazioni sui processi utilizzati dalla sessione con ID 2, digitare:
>   ```
>   query process /ID:2
>   ```
>   ## <a name="additional-references"></a>Riferimenti aggiuntivi
>   - [Guida di](command-line-syntax-key.md)
>   [query](query.md)
>   riferimento ai comandi della sintassi della riga di comando[Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
