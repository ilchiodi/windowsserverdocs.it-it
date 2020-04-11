---
title: Cambiare la porta di ascolto in Desktop remoto
description: Informazioni su come cambiare la porta di ascolto per il client Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 07/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: 818ae5217d0144b2a4ec6e45f3a7757455cfabf1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854664"
---
# <a name="change-the-listening-port-for-remote-desktop-on-your-computer"></a>Cambiare la porta di ascolto per Desktop remoto nel computer

>Si applica a: Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2008 R2

Quando ti connetti a un computer (un client Windows o Windows Server) tramite il client Desktop remoto, la funzionalità Desktop remoto nel computer "ascolta" la richiesta di connessione tramite una porta di ascolto definita, che per impostazione predefinita è la porta 3389. Puoi cambiare la porta di ascolto nei computer Windows modificando il Registro di sistema.

1. Avvia l'editor del Registro di sistema digitando regedit nella casella di ricerca.
2. Passa alla sottochiave del Registro di sistema seguente: HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber
3. Fai clic su **Modifica > Modifica** e quindi su **Decimale**.
4. Digita il nuovo numero di porta e quindi fai clic su **OK**. 
5. Chiudi l'editor del Registro di sistema e riavvia il computer.

Alla successiva connessione al computer con connessione Desktop remoto, sarà necessario digitare la nuova porta. Se usi un firewall, assicurati di configurarlo in modo da consentire le connessioni al nuovo numero di porta.
