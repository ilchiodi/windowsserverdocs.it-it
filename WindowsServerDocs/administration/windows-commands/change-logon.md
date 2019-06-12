---
title: change logon
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41466260-aee9-4333-bcb6-178112c22afd Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45c171a1b14cf69abf039d57697cad933a2dd87b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434574"
---
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="change-logon"></a>change logon
Abilita o disabilita gli accessi da sessioni client Visualizza lo stato di accesso corrente.
Questa utilità è utile per la manutenzione del sistema.
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
> ## <a name="syntax"></a>Sintassi
> ```
> change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
> ```
> ## <a name="parameters"></a>Parametri
> 
> |     Parametro      |                                                       Descrizione                                                        |
> |--------------------|--------------------------------------------------------------------------------------------------------------------------|
> |       /query       |                             Visualizza lo stato di accesso corrente, se abilitato o disabilitato.                              |
> |      /Enable       |                              Consente gli accessi da sessioni client, ma non dalla console.                              |
> |      /Disable      |  Disabilita gli accessi successivi da sessioni client, ma non da console. Non influisce sugli utenti attualmente connesso.   |
> |       /drain       |                 Disabilita gli accessi da sessioni client nuovo, ma consente di riconnessione a sessioni esistenti.                 |
> | /drainuntilrestart | Disabilita gli accessi da nuove sessioni client fino a quando il computer viene riavviato, ma consente di riconnessione a sessioni esistenti. |
> |         /?         |                                           Visualizza la guida al prompt dei comandi.                                           |
> 
> ## <a name="remarks"></a>Note
> - Solo gli amministratori possono utilizzare il **modificare accesso** comando.
> - Gli accessi vengono riattivati quando si riavvia il sistema. Se si è connessi al server Host sessione Desktop remoto (Host sessione rd) da una sessione client e disabilitare gli accessi e quindi disconnettersi prima di riabilitare gli accessi, non sarà in grado di riconnettersi alla sessione corrente. Per riabilitare gli accessi da sessioni client, accedere alla console.
>   ## <a name="BKMK_examples"></a>Esempi
> - Per visualizzare lo stato di accesso corrente, digitare:
>   ```
>   change logon /query
>   ```
> - Per abilitare gli accessi da sessioni client, digitare:
>   ```
>   change logon /enable
>   ```
> - Per disabilitare gli accessi client, digitare:
>   ```
>   change logon /disable
>   ```
>   #### <a name="additional-references"></a>Riferimenti aggiuntivi
>   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
>   [modificare](change.md)
>   [Remote Desktop Services &#40;servizi Terminal&#41; riferimenti ai comandi](remote-desktop-services-terminal-services-command-reference.md)
