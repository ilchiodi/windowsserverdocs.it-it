---
title: Riferimento ai comandi di Servizi Desktop remoto (servizi Terminal)
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2f371848-5c48-470c-908c-afbc95d3a805
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 630b2274e96e446af07f7630d95056b40858209f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722437"
---
# <a name="remote-desktop-services-terminal-services-command-reference"></a>Riferimento ai comandi di Servizi Desktop remoto (servizi Terminal)

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Di seguito è riportato un elenco di strumenti della riga di comando di Servizi Desktop remoto.
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
> 
> |                 Comando                 |                                                      Descrizione                                                       |
> |-----------------------------------------|------------------------------------------------------------------------------------------------------------------------|
> |           [change](change.md)           | modifica le impostazioni del server host sessione Desktop remoto (host sessione Desktop remoto) per gli accessi, i mapping delle porte COM e la modalità di installazione. |
> |     [change logon](change-logon.md)     |    Abilita o Disabilita gli accessi da sessioni client in un server Host sessione Desktop remoto o Visualizza lo stato di accesso corrente.     |
> |      [change port](change-port.md)      |                   elenca o modifica i mapping delle porte COM in modo che siano compatibili con le applicazioni MS-DOS.                    |
> |      [change user](change-user.md)      |                                modifica la modalità di installazione per il server Host sessione Desktop remoto.                                |
> |         [chglogon](chglogon.md)         |    Abilita o Disabilita gli accessi da sessioni client in un server Host sessione Desktop remoto o Visualizza lo stato di accesso corrente.     |
> |          [chgport](chgport.md)          |                   elenca o modifica i mapping delle porte COM in modo che siano compatibili con le applicazioni MS-DOS.                    |
> |           [chgusr](chgusr.md)           |                                modifica la modalità di installazione per il server Host sessione Desktop remoto.                                |
> |         [flattemp](flattemp.md)         |                                      Abilita o disabilita le cartelle temporanee.                                       |
> |           [logoff](logoff.md)           |          Disconnette un utente da una sessione in un server Host sessione Desktop remoto ed elimina la sessione dal server.          |
> |              [msg](msg.md)              |                                Invia un messaggio a un utente in un server Host sessione Desktop remoto.                                 |
> |            [mstsc](mstsc.md)            |                       Crea connessioni ai server Host sessione Desktop remoto o ad altri computer remoti.                        |
> |          [qappsrv](qappsrv.md)          |                             Visualizza un elenco di tutti i server Host sessione Desktop remoto nella rete.                             |
> |         [qprocess](qprocess.md)         |                  Visualizza informazioni sui processi in esecuzione in un server Host sessione Desktop remoto.                   |
> |            [query](query.md)            |                      Visualizza informazioni su processi, sessioni e server Host sessione Desktop remoto.                      |
> |    [processo query](query-process.md)    |                  Visualizza informazioni sui processi in esecuzione in un server Host sessione Desktop remoto.                   |
> |    [sessione di query](query-session.md)    |                           Visualizza informazioni sulle sessioni in un server Host sessione Desktop remoto.                            |
> | [termserver query](query-termserver.md) |                             Visualizza un elenco di tutti i server Host sessione Desktop remoto nella rete.                             |
> |       [utente query](query-user.md)       |                         Visualizza informazioni sulle sessioni utente in un server Host sessione Desktop remoto.                         |
> |            [quser](quser.md)            |                         Visualizza informazioni sulle sessioni utente in un server Host sessione Desktop remoto.                         |
> |          [qwinsta](qwinsta.md)          |                           Visualizza informazioni sulle sessioni in un server Host sessione Desktop remoto.                            |
> |          [rdpsign](rdpsign.md)          |                          Consente di firmare digitalmente un file Remote Desktop Protocol (RDP).                          |
> |    [reset session](reset-session.md)    |                         Consente di reimpostare (eliminare) una sessione in un server Host sessione Desktop remoto.                          |
> |          [rwinsta](rwinsta.md)          |                         Consente di reimpostare (eliminare) una sessione in un server Host sessione Desktop remoto.                          |
> |           [shadow](shadow.md)           |            Consente di controllare in modalità remota una sessione attiva di un altro utente in un server Host sessione Desktop remoto.             |
> |            [tscon](tscon.md)            |                               Consente di connettersi a un'altra sessione in un server Host sessione Desktop remoto.                                |
> |         [tsdiscon](tsdiscon.md)         |                                 Disconnette una sessione da un server Host sessione Desktop remoto.                                  |
> |           [tskill](tskill.md)           |                           Termina un processo in esecuzione in una sessione in un server Host sessione Desktop remoto.                            |
> |           [tsprof](tsprof.md)           |              Copia le informazioni di configurazione utente di Servizi Desktop remoto da un utente a un altro.               |
