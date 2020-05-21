---
title: termserver query
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3b89d3b4-236f-4376-90b6-939a0ec4b288
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b3ae6858a70d22b3ad928474648a3b2ee156235
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436236"
---
# <a name="query-termserver"></a>termserver query

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco di tutti i server di host sessione Desktop remoto (host sessione Desktop remoto) nella rete.

> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
> ## <a name="syntax"></a>Sintassi
> ```
> query termserver [<ServerName>] [/domain:<Domain>] [/address] [/continue]
> ```
> ### <a name="parameters"></a>Parametri
>
> |    Parametro     |                                                                        Descrizione                                                                         |
> |------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |   <ServerName>   |                                               Specifica il nome che identifica il server Host sessione Desktop remoto.                                               |
> | /Domain<Domain> | Specifica il dominio per eseguire una query per i server terminal. Non è necessario specificare un dominio se si sta eseguendo una query sul dominio in cui si sta lavorando. |
> |     /Address     |                                                  Visualizza gli indirizzi di rete e di nodo per ogni server.                                                  |
> |    /continue     |                                              Impedisce la sospensione dopo la visualizzazione di ogni schermata delle informazioni.                                               |
> |        /?        |                                                            Visualizza la guida al prompt dei comandi.                                                            |
>
>#### <a name="remarks"></a>Osservazioni
> - **query termserver** Cerca nella rete tutti i server Host sessione Desktop remoto collegati e restituisce le informazioni seguenti:
>   - Nome host del server.
>   - Rete (e indirizzo del nodo se viene utilizzata l'opzione/Address)
>     ## <a name="examples"></a>Esempi
> - Per visualizzare informazioni su tutti i server Host sessione Desktop remoto nella rete, digitare:
>   ```
>   query termserver
>   ```
> - Per visualizzare informazioni sul server Host sessione Desktop remoto denominato Server3, digitare:
>   ```
>   query termserver Server3
>   ```
> - Per visualizzare informazioni su tutti i server Host sessione Desktop remoto nel dominio CONTOSO, digitare:
>   ```
>   query termserver /domain:CONTOSO
>   ```
> - Per visualizzare la rete e l'indirizzo del nodo per il server Host sessione Desktop remoto denominato Server3, digitare:
>   ```
>   query termserver Server3 /address
>   ```
>   ## <a name="additional-references"></a>Riferimenti aggiuntivi
>   - Chiave sintassi della [riga di comando](command-line-syntax-key.md) 
>    [query](query.md) 
>    di Guida di [riferimento ai comandi servizi desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
