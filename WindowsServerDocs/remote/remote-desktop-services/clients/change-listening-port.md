---
title: Modificare la porta di ascolto in Desktop remoto
description: Informazioni su come modificare la porta di ascolto per il client Desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 07/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: 5bf90010143e742f7a0c9b5c262be01e6ccf8c5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882722"
---
# <a name="change-the-listening-port-for-remote-desktop-on-your-computer"></a>Modificare la porta di ascolto per il Desktop remoto nel computer

>Si applica a: Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2, Windows Server 2008 R2

Quando ci si connette a un computer tramite il client Desktop remoto (un client Windows o Windows Server), la funzionalità Desktop remoto nel computer "Ascolta" la richiesta di connessione tramite una porta di ascolto definita (3389 per impostazione predefinita). È possibile modificare tale porta di ascolto nei computer Windows modificando il Registro di sistema.

1. Avviare l'editor del Registro di sistema. (Digitare regedit nella casella di ricerca).
2. Individuare la sottochiave del Registro di sistema seguente: HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber
3. Fare clic su **Modifica > Modify**, quindi fare clic su **decimale**.
4. Digitare il nuovo numero di porta e quindi fare clic su **OK**. 
5. Chiudere l'editor del Registro di sistema e riavviare il computer.

Al successivo che ci si connette al computer con connessione Desktop remoto, è necessario digitare la nuova porta. Se si usa un firewall, assicurarsi di configurare il firewall per consentire le connessioni per il nuovo numero di porta.
