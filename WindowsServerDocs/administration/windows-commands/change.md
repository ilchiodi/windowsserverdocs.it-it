---
title: modifica
description: Argomento dei comandi di Windows per Change, che modifica host sessione Desktop remoto impostazioni server per gli accessi, i mapping delle porte COM e la modalità di installazione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90012116-0fb3-4f34-a819-cf4d4b4f8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5d91f8d0941fc96e776c761b9c7037e58588df8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847954"
---
# <a name="change"></a>modifica

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica le impostazioni del server host sessione Desktop remoto per gli accessi, i mapping delle porte COM e la modalità di installazione.

> [!NOTE]
> In Windows Server 2008 R2 Servizi terminal è stato rinomato Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi

 ```
 change logon
 change port
 change user
 ```
 
 ### <a name="parameters"></a>Parametri
 
 |            Parametro            |                                                   Descrizione                                                   |
 |---------------------------------|-----------------------------------------------------------------------------------------------------------------|
 | [change logon](change-logon.md) | Abilita o Disabilita gli accessi da sessioni client in un server Host sessione Desktop remoto o Visualizza lo stato di accesso corrente. |
 |  [change port](change-port.md)  |                elenca o modifica i mapping delle porte COM in modo che siano compatibili con le applicazioni MS-DOS.                |
 |  [change user](change-user.md)  |                            modifica la modalità di installazione per il server Host sessione Desktop remoto.                             |
 
 ## <a name="additional-references"></a>Altre informazioni di riferimento
 - Guida di riferimento alla [chiave della sintassi della riga di comando](command-line-syntax-key.md)
 [Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
