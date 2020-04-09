---
title: Passaggio 2 pianificare la distribuzione del server RADIUS
description: Questo argomento fa parte della Guida distribuire accesso remoto con l'autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 2d6ad863-02a5-49b0-9aff-d189e78b2b80
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1722b750405ea1188d18fab6282d82434f5334ef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858174"
---
# <a name="step-2-plan-the-radius-server-deployment"></a>Passaggio 2 pianificare la distribuzione del server RADIUS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Dopo la distribuzione di un singolo server di accesso remoto, pianificare il server di autenticazione OTP (One-time password).  
  
|Attività|Descrizione|  
|----|--------|  
|2,1 pianificare il server RADIUS|Per il server di autenticazione OTP, l'accesso remoto in Windows Server 2016 e Windows Server 2012 supporta qualsiasi server OTP abilitato per RADIUS che supporta il protocollo PAP (Password Authentication Protocol).|  
  
## <a name="21-plan-the-radius-server"></a><a name="BKMK_1.1"></a>2,1 pianificare il server RADIUS  
Quando si pianifica un server RADIUS per l'autenticazione OTP, tenere presente quanto segue:  
  
-   Per la maggior parte dei tipi di distribuzioni OTP, è necessario configurare il server di accesso remoto come agente RADIUS. Per ulteriori informazioni, vedere la documentazione del fornitore OTP.  
  
-   Per tutte le distribuzioni OTP, è necessario sincronizzare gli utenti Active Directory con il server RADIUS.  
  
-   Non è necessario che il server RADIUS sia un membro del dominio.  
  
-   Quando si distribuisce il server RADIUS, si configura un segreto condiviso e il numero di porta per il traffico RADIUS. Prendere nota di questi dettagli; sono necessarie quando si configura il server di accesso remoto.  
  
È possibile visualizzare una guida al Lab di test di esempio che configura l'autenticazione OTP con un server RSA SecurID nella [Guida al Lab di test: dimostrazione di DirectAccess con autenticazione OTP e RSA SecurID](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid).  
  
  
  


