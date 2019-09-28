---
title: Passaggio 2 pianificare la distribuzione del server RADIUS
description: Questo argomento fa parte della Guida distribuire accesso remoto con l'autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d6ad863-02a5-49b0-9aff-d189e78b2b80
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a991b312a0938a3809acd2b94c00aa678f5b41da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404398"
---
# <a name="step-2-plan-the-radius-server-deployment"></a>Passaggio 2 pianificare la distribuzione del server RADIUS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Dopo la distribuzione di un singolo server di accesso remoto, pianificare il server di autenticazione OTP (One-time password).  
  
|Attività|Descrizione|  
|----|--------|  
|2,1 pianificare il server RADIUS|Per il server di autenticazione OTP, l'accesso remoto in Windows Server 2016 e Windows Server 2012 supporta qualsiasi server OTP abilitato per RADIUS che supporta il protocollo PAP (Password Authentication Protocol).|  
  
## <a name="BKMK_1.1"></a>2,1 pianificare il server RADIUS  
Quando si pianifica un server RADIUS per l'autenticazione OTP, tenere presente quanto segue:  
  
-   Per la maggior parte dei tipi di distribuzioni OTP, è necessario configurare il server di accesso remoto come agente RADIUS. Per ulteriori informazioni, vedere la documentazione del fornitore OTP.  
  
-   Per tutte le distribuzioni OTP, è necessario sincronizzare gli utenti Active Directory con il server RADIUS.  
  
-   Non è necessario che il server RADIUS sia un membro del dominio.  
  
-   Quando si distribuisce il server RADIUS, si configura un segreto condiviso e il numero di porta per il traffico RADIUS. Prendere nota di questi dettagli; sono necessarie quando si configura il server di accesso remoto.  
  
È possibile visualizzare una guida al Lab di test di esempio che configura l'autenticazione OTP con un server RSA SecurID in [Test Lab guide: Dimostrazione di DirectAccess con autenticazione OTP e RSA SecurID @ no__t-0.  
  
  
  


