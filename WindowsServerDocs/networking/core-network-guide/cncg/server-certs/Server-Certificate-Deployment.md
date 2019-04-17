---
title: Distribuzione del certificato server
description: Questo argomento fa parte della Guida di certificati del Server di distribuzione per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4a10a9bafa6a8c9fddecac799ec8e837bf339d0e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment"></a>Distribuzione del certificato server

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Segui questi passaggi per installare un'autorità di certificazione radice aziendale (CA) e per distribuire i certificati server per l'utilizzo con PEAP ed EAP.  
  
> [!IMPORTANT]  
> Prima di installare Servizi certificati Active Directory, è necessario il nome computer, configurare il computer con un indirizzo IP statico e aggiungere il computer al dominio. Dopo aver installato Servizi certificati Active Directory, è possibile modificare il nome del computer o l'appartenenza al dominio del computer, tuttavia, se necessario, è possibile modificare l'indirizzo IP. Per ulteriori informazioni su come eseguire queste attività, vedere Windows Server&reg; 2016 [Guida alla rete Core](../../Core-Network-Guide.md).  

  
-   [Installare WEB1 il Server Web](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)  
  
-   [Creare un record alias (CNAME) in DNS per WEB1](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)  
  
-   [Configurare WEB1 per la distribuzione degli elenchi di revoche di certificati (CRL)](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)  
  
-   [Preparare il file CAPolicy inf](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)  
  
-   [Installare l'autorità di certificazione](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)  
  
-   [Configurare le estensioni CDP e AIA su CA1](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)  
  
-   [Copiare il certificato CA e CRL per la directory virtuale](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)  
  
-   [Configurare il modello di certificato server](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)  
  
-   [Configurare la registrazione automatica del certificato server](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)  
  
-   [Aggiornare criteri di gruppo](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)  
  
-   [Verificare la registrazione di Server di un certificato Server](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)  
  
> [!NOTE]  
> Le procedure descritte in questa Guida non includono istruzioni per i casi in cui il **controllo dell'Account utente** viene visualizzata la finestra di dialogo per richiedere l'autorizzazione a continuare. Se viene visualizzata questa finestra di dialogo mentre si siano eseguendo le procedure in questa Guida, se è stata aperta la finestra di dialogo in risposta alle azioni dell'utente, fare clic su **continua**.  
  


