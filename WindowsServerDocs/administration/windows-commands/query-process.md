---
title: processo di query
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 36ce3ffc-0092-4eb1-a374-28e6616ca946
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3248c2b1f476a1a9843f7e930b05a3dca812d694
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442119"
---
# <a name="query-process"></a>processo di query

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni sui processi in esecuzione in un server Host sessione Desktop remoto (Host sessione rd).
È possibile usare questo comando per scoprire quali programmi di un utente specifico è in esecuzione e anche gli utenti che sono in esecuzione un programma specifico.
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
> ## <a name="syntax"></a>Sintassi
> ```
> query process [* | <ProcessID> | <UserName> | <SessionName> | /id:<nn> | <ProgramName>] [/server:<ServerName>]
> ```
> ## <a name="parameters"></a>Parametri
> 
> |      Parametro       |                                                                 Descrizione                                                                  |
> |----------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
> |          \*          |                                                    Elenca i processi per tutte le sessioni.                                                     |
> |     <ProcessID>      |                                   Specifica l'ID numerico che identifica il processo che si desidera eseguire una query.                                   |
> |      <UserName>      |                                       Specifica il nome dell'utente il cui processi che si desidera elencare.                                       |
> |    <SessionName>     |                                     Specifica il nome della sessione di cui processi che si desidera elencare.                                      |
> |       /id:<nn>       |                                      Specifica l'ID della sessione di cui processi che si desidera elencare.                                       |
> |    <ProgramName>     |                     Specifica il nome del programma di cui si desidera eseguire una query di processi. L'estensione .exe è obbligatorio.                     |
> | /server:<ServerName> | Specifica il server Host sessione Desktop remoto cui processi che si desidera elencare. Se non viene specificato, viene utilizzato il server in cui attualmente connessi. |
> |          /?          |                                                     Visualizza la guida al prompt dei comandi.                                                     |
> 
> ## <a name="remarks"></a>Note
> - Gli amministratori hanno accesso completo a tutti **eseguire una query processo** funzioni.
> - Se non si specifica di <*nomeutente*>, <*SessionName*>, **/id:** <*nn*>, <*ProgramName*>, o **\\** *, parametri **query processo** Visualizza solo i processi che appartengono all'utente corrente.
> - Se si specifica una sessione, è necessario identificare una sessione attiva.
> - **query processo** restituisce le informazioni seguenti:
>   -   L'utente proprietario del processo
>   -   La sessione in cui è proprietario del processo
>   -   L'ID della sessione
>   -   Il nome del processo
>   -   L'ID del processo
> - Quando **eseguire una query processo** restituisce informazioni, un simbolo di maggiore (>) viene visualizzate prima di ogni processo a cui appartiene la sessione corrente.
>   ## <a name="BKMK_examples"></a>Esempi
> - Per visualizzare informazioni sui processi utilizzati da tutte le sessioni, digitare:
>   ```
>   query process *
>   ```
> - Per visualizzare informazioni sui processi utilizzati dalla sessione ID 2, digitare:
>   ```
>   query process /ID:2
>   ```
>   #### <a name="additional-references"></a>Riferimenti aggiuntivi
>   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
>   [query](query.md)
>   [Remote Desktop Services &#40;servizi Terminal&#41; riferimenti ai comandi](remote-desktop-services-terminal-services-command-reference.md)
