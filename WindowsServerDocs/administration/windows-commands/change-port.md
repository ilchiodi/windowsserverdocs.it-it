---
title: change port
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ced5b9f0198179ab8b388f56aaea848b7a966081
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434499"
---
# <a name="change-port"></a>change port

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elenca o modifica i mapping delle porte COM per garantire la compatibilità con le applicazioni MS-DOS.
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
> ## <a name="syntax"></a>Sintassi
> ```
> change port [<PortX>=<PortY> | /d <PortX> | /query]
> ```
> ## <a name="parameters"></a>Parametri
> 
> |    Parametro    |              Descrizione               |
> |-----------------|----------------------------------------|
> | <PortX>=<PortY> |    Esegue il mapping di COM <*PortaX*> per <*portay*>.    |
> |   /d <PortX>    | Elimina il mapping per COM <*PortaX*>. |
> |     /query      |  Consente di visualizzare i mapping delle porte corrente.   |
> |       /?        |  Visualizza la guida al prompt dei comandi.  |
> 
> ## <a name="remarks"></a>Note
> - La maggior parte delle applicazioni di MS-DOS supportano solo COM1 a porte seriali COM4. Il **cambiare porta** comando esegue il mapping di una porta seriale per un numero di porta diverso, consentendo alle applicazioni che non supportano numeri alti COM le porte per accedere alla porta seriale. modifica del mapping funziona solo per la sessione corrente e non verrà conservato se si esegue la disconnessione da una sessione e quindi accedere di nuovo.
> - Uso **cambiare porta** senza parametri per visualizzare le porte COM disponibili e i relativi mapping corrente.
>   ## <a name="BKMK_examples"></a>Esempi
> - Per eseguire il mapping COM12 COM1 per l'uso da un'applicazione basata su MS-DOS, digitare:
>   ```
>   change port com12=com1
>   ```
> - Per visualizzare i mapping delle porte corrente, digitare:
>   ```
>   change port /query
>   ```
>   #### <a name="additional-references"></a>Riferimenti aggiuntivi
>   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
>   [modificare](change.md)
>   [Remote Desktop Services &#40;servizi Terminal&#41; riferimenti ai comandi](remote-desktop-services-terminal-services-command-reference.md)
