---
title: Limitare l'accesso al server
description: Informazioni su come concedere o negare l'accesso ai servizi MultiPoint per gli utenti e gruppi
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cabd4f1-a764-4be6-bc6e-0a5f5566390c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 1466e19152847a6c7d88f77162c50ec73a5a7d27
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830812"
---
# <a name="limit-users-access-to-the-multipoint-server"></a>Limitare l'accesso degli utenti al server Multipoint
Se si aggiungere server MultiPoint a un dominio di Active Directory o si usano l'account utente locali, tutti gli utenti dispongono dell'accesso al server MultiPoint per impostazione predefinita. Prima di consentire agli utenti di accedere a stazioni nel proprio ambiente di servizi MultiPoint, è consigliabile limitare l'accesso al server.  
  
Qualsiasi utente del gruppo utenti Desktop remoto può accedere al server MultiPoint. Per impostazione predefinita, l'utente il gruppo Everyone è membro del gruppo utenti Desktop remoto e pertanto ogni utente locale e utente di dominio possono accedere al Server MultiPoint. Per limitare l'accesso al Server MultiPoint, rimuovere il tutti gli utenti dal gruppo utenti Desktop remoto di gruppo e quindi aggiungere utenti o gruppi specifici per il gruppo utenti Desktop remoto.  
  
## <a name="add-or-remove-users-or-groups-to-the-remote-desktop-users-group"></a>Aggiungere o rimuovere utenti o gruppi al gruppo utenti Desktop remoto  
  
1.  Dal **avviare** schermata, aprire **Gestione Computer**.  
  
2.  Nell'albero della console, sotto **utenti e gruppi locali**, fare clic su **gruppi**.  
  
3.  Fare doppio clic su **Remote Desktop Users**e seguire le istruzioni per aggiungere o rimuovere utenti.  
  
    -   Per limitare l'accesso generale per il server, rimuovere il gruppo Everyone.  
  
    -   Per assegnare i server MultiPoint agli utenti l'accesso alle stazioni, aggiungere ogni account locale o ogni account utente o gruppo di dominio al gruppo utenti Desktop remoto.  