---
title: Aggiungere servizi MultiPoint a un dominio (facoltativo)
Description: Viene descritta la procedura per aggiungere servizi MultiPoint al dominio
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 623b7c21-dcbb-402e-8b5a-8e434cd225bd
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 03b60b1d3a7a776448eaa18d926a87a00f56fc30
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820244"
---
# <a name="join-the-multipoint-services-computer-to-a-domain-optional"></a>Aggiungere il computer Servizi MultiPoint a un dominio (facoltativo)
Se si accede al computer Servizi MultiPoint tramite un dominio Active Directory, il passaggio successivo consiste nell'aggiungere il computer al dominio.  
  
> [!IMPORTANT]  
> Prima di aggiungere il computer a un dominio, è necessario verificare il fuso orario. Per istruzioni, vedere [impostare la data, l'ora e il fuso orario](Set-the-date--time--and-time-zone.md).  
   
1.  Dalla schermata **Start** aprire il **Pannello di controllo**. Fare clic su **sistema e sicurezza**, quindi su **sistema**.  
  
2.  In **Impostazioni relative a nome computer, dominio e gruppo di lavoro** fare clic su **Cambia impostazioni**.  
  
3.  Nella scheda **nome computer** fare clic su **Cambia**.  
  
4.  Nella finestra di dialogo **Cambiamenti dominio/nome computer** selezionare **dominio**, immettere il nome del dominio e fare clic su **OK**, quindi seguire i passaggi della procedura guidata per completare il processo.  
  
5.  Dopo il riavvio del computer, accedere come amministratore e attendere l'apertura di gestione MultiPoint.  
  
> [!IMPORTANT]  
> Per assicurarsi che la distribuzione del dominio MultiPoint Services funzioni correttamente, sarà necessario configurare un paio di criteri di gruppo e aggiornare il registro di sistema. Per informazioni, vedere [configurare criteri di gruppo per la distribuzione di un dominio](https://technet.microsoft.com/library/dn265982.aspx).  