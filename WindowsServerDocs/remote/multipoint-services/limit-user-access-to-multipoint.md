---
title: Limitare l'accesso utente al server
description: Informazioni su come concedere o negare l'accesso a MultiPoint Services per utenti e gruppi
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 4cabd4f1-a764-4be6-bc6e-0a5f5566390c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: ddc852eea3ed17cd354cc87c79d82066f989bd5d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820254"
---
# <a name="limit-users-access-to-the-multipoint-server"></a>Limitare l'accesso degli utenti al server multipoint
Se si aggiunge MultiPoint Server a un dominio Active Directory o si usano account utente locali, tutti gli utenti hanno accesso a MultiPoint Server per impostazione predefinita. Prima di consentire agli utenti di accedere alle stazioni nell'ambiente MultiPoint Services, è necessario limitare l'accesso al server.  
  
Qualsiasi utente nel gruppo Desktop remoto Users può accedere a MultiPoint Server. Per impostazione predefinita, il gruppo di utenti Everyone è un membro del gruppo Desktop remoto Users, quindi ogni utente locale e utente di dominio può accedere al server MultiPoint. Per limitare l'accesso a MultiPoint Server, rimuovere il gruppo utenti Everyone dal gruppo Desktop remoto Users, quindi aggiungere utenti o gruppi specifici al gruppo Desktop remoto Users.  
  
## <a name="add-or-remove-users-or-groups-to-the-remote-desktop-users-group"></a>Aggiunta o rimozione di utenti o gruppi al gruppo Desktop remoto Users  
  
1.  Dalla schermata **Start** aprire **Gestione computer**.  
  
2.  Nell'albero della console, in **utenti e gruppi locali**, fare clic su **gruppi**.  
  
3.  Fare doppio clic su **Desktop remoto utenti**e seguire le istruzioni per l'aggiunta o la rimozione di utenti.  
  
    -   Per limitare l'accesso generale al server, rimuovere il gruppo Everyone.  
  
    -   Per consentire agli utenti di MultiPoint Server di accedere alle stazioni, aggiungere ogni account locale o account di gruppo o utente di dominio al gruppo Desktop remoto Users.  