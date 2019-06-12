---
title: Riferimento ai comandi di Servizi Desktop remoto (servizi Terminal)
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f371848-5c48-470c-908c-afbc95d3a805
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7524973c50b597b8fd8798c5268746dd1d991d7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441830"
---
# <a name="remote-desktop-services-terminal-services-command-reference"></a>Riferimento ai comandi di Servizi Desktop remoto (servizi Terminal)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Di seguito è riportato un elenco di strumenti della riga di comando di Servizi Desktop remoto.
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
> 
> |                 Comando                 |                                                      Descrizione                                                       |
> |-----------------------------------------|------------------------------------------------------------------------------------------------------------------------|
> |           [change](change.md)           | Modifica le impostazioni di server Host sessione Desktop remoto (Host sessione rd) per gli accessi, i mapping delle porte COM e la modalità di installazione. |
> |     [change logon](change-logon.md)     |    Abilita o disabilita gli accessi da sessioni client in un server Host sessione Desktop remoto o Visualizza lo stato di accesso corrente.     |
> |      [change port](change-port.md)      |                   Elenca o modifica i mapping delle porte COM per garantire la compatibilità con le applicazioni MS-DOS.                    |
> |      [change user](change-user.md)      |                                modifica la modalità di installazione per il server Host sessione Desktop remoto.                                |
> |         [chglogon](chglogon.md)         |    Abilita o disabilita gli accessi da sessioni client in un server Host sessione Desktop remoto o Visualizza lo stato di accesso corrente.     |
> |          [chgport](chgport.md)          |                   Elenca o modifica i mapping delle porte COM per garantire la compatibilità con le applicazioni MS-DOS.                    |
> |           [chgusr](chgusr.md)           |                                modifica la modalità di installazione per il server Host sessione Desktop remoto.                                |
> |         [flattemp](flattemp.md)         |                                      Abilita o disabilita le cartelle temporanee.                                       |
> |           [logoff](logoff.md)           |          Disconnette un utente da una sessione in un server Host sessione Desktop remoto ed elimina la sessione dal server.          |
> |              [msg](msg.md)              |                                Invia un messaggio a un utente in un server Host sessione Desktop remoto.                                 |
> |            [mstsc](mstsc.md)            |                       Crea connessioni al server Host sessione Desktop remoto o ad altri computer remoti.                        |
> |          [qappsrv](qappsrv.md)          |                             Visualizza un elenco di tutti i server Host sessione Desktop remoto sulla rete.                             |
> |         [qprocess](qprocess.md)         |                  Visualizza informazioni sui processi in esecuzione in un server Host sessione Desktop remoto.                   |
> |            [query](query.md)            |                      Visualizza informazioni sui processi, sessioni e i server Host sessione Desktop remoto.                      |
> |    [processo di query](query-process.md)    |                  Visualizza informazioni sui processi in esecuzione in un server Host sessione Desktop remoto.                   |
> |    [sessione di query](query-session.md)    |                           Visualizza le informazioni sulle sessioni in un server Host sessione Desktop remoto.                            |
> | [eseguire una query termserver](query-termserver.md) |                             Visualizza un elenco di tutti i server Host sessione Desktop remoto sulla rete.                             |
> |       [query utente](query-user.md)       |                         Visualizza le informazioni sulle sessioni utente in un server Host sessione Desktop remoto.                         |
> |            [quser](quser.md)            |                         Visualizza le informazioni sulle sessioni utente in un server Host sessione Desktop remoto.                         |
> |          [qwinsta](qwinsta.md)          |                           Visualizza le informazioni sulle sessioni in un server Host sessione Desktop remoto.                            |
> |          [rdpsign](rdpsign.md)          |                          Consente di firmare digitalmente un file Remote Desktop Protocol (RDP).                          |
> |    [reset session](reset-session.md)    |                         Consente di reimpostare (eliminare) una sessione in un server Host sessione Desktop remoto.                          |
> |          [rwinsta](rwinsta.md)          |                         Consente di reimpostare (eliminare) una sessione in un server Host sessione Desktop remoto.                          |
> |           [shadow](shadow.md)           |            Consente di controllare in remoto una sessione attiva di un altro utente in un server Host sessione Desktop remoto.             |
> |            [tscon](tscon.md)            |                               Si connette a un'altra sessione in un server Host sessione Desktop remoto.                                |
> |         [tsdiscon](tsdiscon.md)         |                                 Disconnette una sessione da un server Host sessione Desktop remoto.                                  |
> |           [tskill](tskill.md)           |                           Termina un processo in esecuzione in una sessione in un server Host sessione Desktop remoto.                            |
> |           [tsprof](tsprof.md)           |              Copia le informazioni di configurazione utente di Servizi Desktop remoto da un utente a un altro.               |
