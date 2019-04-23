---
title: Distribuzione di certificati server
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 751c5c5958b3d06ae0f4b701e4d6e10a7fef19dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858492"
---
# <a name="server-certificate-deployment"></a>Distribuzione di certificati server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Seguire questi passaggi per installare un'autorità di certificazione radice aziendale (CA) e per distribuire i certificati server da utilizzare con PEAP ed EAP.  
  
> [!IMPORTANT]  
> Prima di installare Servizi certificati Active Directory, è necessario assegnare il nome del computer, configurare il computer con un indirizzo IP statico e aggiungere il computer al dominio. Dopo aver installato Servizi certificati Active Directory, è possibile modificare il nome del computer o l'appartenenza al dominio del computer, tuttavia, se necessario, è possibile modificare l'indirizzo IP. Per ulteriori informazioni su come eseguire queste attività, vedere Windows Server&reg; 2016 [Guida alla rete Core](../../Core-Network-Guide.md).  

  
-   [Installare WEB1 il Server Web](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)  
  
-   [Creare un record alias (CNAME) in DNS per WEB1](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)  
  
-   [Configurare WEB1 per la distribuzione degli elenchi di revoche di certificati (CRL)](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)  
  
-   [Preparare il file CAPolicy inf](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)  
  
-   [Installare l'autorità di certificazione](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)  
  
-   [Configurare le estensioni CDP e AIA su CA1](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)  
  
-   [Copiare il certificato della CA e CRL nella directory virtuale](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)  
  
-   [Configurare il modello di certificato server](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)  
  
-   [Configurare la registrazione automatica del certificato server](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)  
  
-   [Aggiornare criteri di gruppo](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)  
  
-   [Verificare la registrazione del Server di un certificato Server](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)  
  
> [!NOTE]  
> Le procedure illustrate in questa guida non includono istruzioni relative ai casi in cui la finestra di dialogo **Controllo dell'account utente** viene visualizzata per richiedere l'autorizzazione a continuare. Se si apre questa finestra di dialogo mentre si eseguono le procedure descritte in questa Guida, se è stata aperta la finestra di dialogo in risposta alle azioni dell'utente, fare clic su **Continue**.  
  


