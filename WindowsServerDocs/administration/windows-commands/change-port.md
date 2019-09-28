---
title: change port
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d772c90-e849-4e74-b9ec-b6cae1159336 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0e587572acd1af1cc7dbd2550e1eae5244d0d1dd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379602"
---
# <a name="change-port"></a>change port

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

elenca o modifica i mapping delle porte COM in modo che siano compatibili con le applicazioni MS-DOS.
Per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
> ## <a name="syntax"></a>Sintassi
> ```
> change port [<PortX>=<PortY> | /d <PortX> | /query]
> ```
> ## <a name="parameters"></a>Parametri
> 
> |    Parametro    |              Descrizione               |
> |-----------------|----------------------------------------|
> | <PortX>=<PortY> |    Esegue il mapping di COM <*portax*> a <*PortaY*>.    |
> |   /d <PortX>    | Elimina il mapping per < COM*portax*>. |
> |     /query      |  Consente di visualizzare i mapping delle porte correnti.   |
> |       /?        |  Visualizza la guida al prompt dei comandi.  |
> 
> ## <a name="remarks"></a>Note
> - La maggior parte delle applicazioni MS-DOS supporta solo la COM1 tramite porte seriali COM4. Il comando **Change Port** esegue il mapping di una porta seriale a un numero di porta diverso, consentendo alle applicazioni che non supportano porte com con numero elevato di accedere alla porta seriale. il rimapping funziona solo per la sessione corrente e non viene mantenuto se si disconnette da una sessione di e quindi si esegue di nuovo l'accesso.
> - Utilizzare **Change Port** senza parametri per visualizzare le porte com disponibili e i relativi mapping correnti.
>   ## <a name="BKMK_examples"></a>Esempi
> - Per eseguire il mapping di COM12 a COM1 per l'uso da parte di un'applicazione basata su MS-DOS, digitare:
>   ```
>   change port com12=com1
>   ```
> - Per visualizzare i mapping delle porte correnti, digitare:
>   ```
>   change port /query
>   ```
>   #### <a name="additional-references"></a>Riferimenti aggiuntivi
>   [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
>   [modificare](change.md)
>   [ &#40;Servizi Desktop remoto&#41; riferimento ai comandi di Servizi terminal](remote-desktop-services-terminal-services-command-reference.md)
