---
title: Aggiungere servizi MultiPoint a un dominio (facoltativo)
Description: Fornisce i passaggi per aggiungere servizi MultiPoint al dominio
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 623b7c21-dcbb-402e-8b5a-8e434cd225bd
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: af5dd1f16e011161bbcf72c21c21088721ac1243
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827042"
---
# <a name="join-the-multipoint-services-computer-to-a-domain-optional"></a>Aggiungere il computer MultiPoint Services a un dominio (facoltativo)
Se è necessario accedere nel computer di MultiPoint Services in un dominio Active Directory, il passaggio successivo consiste nell'aggiungere il computer al dominio.  
  
> [!IMPORTANT]  
> Prima di aggiungere il computer a un dominio, è necessario verificare il fuso orario. Per istruzioni, vedere [impostare la data, ora e fuso orario](Set-the-date--time--and-time-zone.md).  
   
1.  Dalla schermata **Start** aprire il **Pannello di controllo**. Fare clic su **sistema e sicurezza**, quindi fare clic su **sistema**.  
  
2.  In **Impostazioni relative a nome computer, dominio e gruppo di lavoro**fare clic su **Cambia impostazioni**.  
  
3.  Nel **nome Computer** scheda, fare clic su **modifica**.  
  
4.  Nel **cambiamenti dominio/nome Computer** finestra di dialogo **Domain**, immettere il nome del dominio e fare clic su **OK**e quindi seguire i passaggi della procedura guidata per completare il processo.  
  
5.  Dopo il riavvio del computer, accedere come amministratore e attendere selezionare Gestione MultiPoint da aprire.  
  
> [!IMPORTANT]  
> Per garantire il corretto funzionamento della distribuzione di dominio di servizi MultiPoint, dovrai configurare un paio di criteri di gruppo e aggiornare il Registro di sistema. Per informazioni, vedere [configurare i criteri di gruppo per la distribuzione di un dominio](https://technet.microsoft.com/library/dn265982.aspx).  