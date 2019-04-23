---
title: Passaggio 2 pianificare la distribuzione di Server RADIUS
description: Questo argomento fa parte della Guida alla distribuzione di accesso remoto con autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d6ad863-02a5-49b0-9aff-d189e78b2b80
ms.author: pashort
author: shortpatti
ms.openlocfilehash: faf3f0b7c691edfb2c41e7b568e0791a3cad76b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859212"
---
# <a name="step-2-plan-the-radius-server-deployment"></a>Passaggio 2 pianificare la distribuzione di Server RADIUS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Dopo aver distribuito un singolo server di accesso remoto, pianificare il server di autenticazione One Time password (OTP).  
  
|Attività|Descrizione|  
|----|--------|  
|2.1 pianificare il server RADIUS|Per il server di autenticazione OTP, accesso remoto in Windows Server 2016 e Windows Server 2012 supporta qualsiasi server OTP abilitato per RADIUS che supporta il protocollo di autenticazione di password (Authentication Protocol PAP).|  
  
## <a name="BKMK_1.1"></a>2.1 pianificare il server RADIUS  
Quando si pianifica un server RADIUS per l'autenticazione OTP, tenere presente quanto segue:  
  
-   Per la maggior parte dei tipi di distribuzione di OTP, è necessario configurare il server di accesso remoto come agente di RADIUS. Per altre informazioni, vedere la documentazione del fornitore OTP.  
  
-   Per tutte le distribuzioni di OTP, è necessario sincronizzare gli utenti di Active Directory con il server RADIUS.  
  
-   Il server RADIUS non è necessario essere un membro del dominio.  
  
-   Quando si distribuisce il server RADIUS, si configura un segreto condiviso e il numero di porta per il traffico RADIUS. Prendere nota di questi dettagli; sono necessarie quando si configura il server di accesso remoto.  
  
È possibile visualizzare una Guida del lab di test di esempio che configura l'autenticazione OTP con un server RSA SecurID in [Guida al Lab di Test: Dimostrazione di DirectAccess con autenticazione OTP e SecurID RSA](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid).  
  
  
  


