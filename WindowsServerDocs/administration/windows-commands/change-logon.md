---
title: change logon
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c04eaffe366dce079aed53351589c1b5026954e3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379640"
---
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="change-logon"></a>change logon
Abilita o Disabilita gli accessi da sessioni client o Visualizza lo stato di accesso corrente.
Questa utilità è utile per la manutenzione del sistema.
Per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
> ## <a name="syntax"></a>Sintassi
> ```
> change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
> ```
> ## <a name="parameters"></a>Parametri
> 
> |     Parametro      |                                                       Descrizione                                                        |
> |--------------------|--------------------------------------------------------------------------------------------------------------------------|
> |       /query       |                             Visualizza lo stato di accesso corrente, se abilitato o disabilitato.                              |
> |      /Enable       |                              Consente l'accesso da sessioni client, ma non dalla console di.                              |
> |      /Disable      |  Disabilita gli accessi successivi dalle sessioni client, ma non dalla console. Non influisce sugli utenti attualmente connessi.   |
> |       /drain       |                 Disabilita gli accessi dalle nuove sessioni client, ma consente la riconnessione a sessioni esistenti.                 |
> | /drainuntilrestart | Disabilita gli accessi dalle nuove sessioni client finché il computer non viene riavviato, ma consente la riconnessione a sessioni esistenti. |
> |         /?         |                                           Visualizza la guida al prompt dei comandi.                                           |
> 
> ## <a name="remarks"></a>Note
> - Solo gli amministratori possono usare il comando **Change logon** .
> - Gli accessi vengono abilitati di nuovo al riavvio del sistema. Se si è connessi al server host sessione Desktop remoto (host sessione Desktop remoto) da una sessione client e si disabilitano gli accessi e quindi si disconnette prima di riabilitare gli accessi, non sarà possibile riconnettersi alla sessione. Per riabilitare gli accessi da sessioni client, accedere alla console di.
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
>   [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
>   [modificare](change.md)
>   [ &#40;Servizi Desktop remoto&#41; riferimento ai comandi di Servizi terminal](remote-desktop-services-terminal-services-command-reference.md)
