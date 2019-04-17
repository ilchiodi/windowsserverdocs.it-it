---
title: Modificare la porta di ascolto di Desktop remoto
description: Scopri come modificare la porta di ascolto per client Desktop remoto.
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
ms.openlocfilehash: b70479b644f4984c93136d6493483c372703244d
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297321"
---
# Modificare la porta di ascolto per Desktop remoto nel computer

>Si applica a: Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2008 R2

Quando ti Connetti a un computer (un client di Windows o Windows Server) tramite il client Desktop remoto, la funzionalità di Desktop remoto nel computer di "Ascolta" la richiesta di connessione attraverso una porta di ascolto definita (3389 per impostazione predefinita). È possibile modificare tale porta di ascolto nel computer Windows modificando il Registro di sistema.

1. Avvia l'editor del Registro di sistema. (Tipo regedit nella casella di ricerca).
2. Individuare la sottochiave del Registro di sistema seguente: HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber
3. Fare clic su **Modifica gt _ modifica**e quindi fai clic su **decimale**.
4. Digita il nuovo numero di porta e quindi fai clic su **OK**. 
5. Chiudi l'editor del Registro di sistema e riavviare il computer.

Al successivo che si connette a questo computer tramite la connessione Desktop remoto, è necessario digitare la nuova porta. Se stai usando un firewall, assicurati di configurare il firewall per consentire le connessioni per il nuovo numero di porta.
