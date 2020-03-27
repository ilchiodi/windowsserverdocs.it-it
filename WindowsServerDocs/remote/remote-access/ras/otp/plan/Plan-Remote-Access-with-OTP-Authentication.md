---
title: Pianificare l'accesso remoto con l'autenticazione OTP
description: Questo argomento fa parte della Guida distribuire accesso remoto con l'autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 762bc463-eead-46ac-8b90-32355743c27c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1f0b030667b0b10a22b4e90d1ddff87086c04aa0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313603"
---
# <a name="plan-remote-access-with-otp-authentication"></a>Pianificare l'accesso remoto con l'autenticazione OTP

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

 Windows Server 2016 e Windows Server 2012 combinano DirectAccess e la VPN del servizio Routing e accesso remoto (RRAS) in un singolo ruolo accesso remoto. Questa panoramica offre un'introduzione ai passaggi di configurazione necessari per distribuire una singola distribuzione multisito di accesso remoto di Windows Server 2016 o Windows Server 2012.  
  
  
-  Passaggio 1: [distribuire un server DirectAccess singolo con impostazioni avanzate](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Questo passaggio include la pianificazione dell'infrastruttura necessaria per distribuire un singolo server. Include la pianificazione delle impostazioni di rete e del server, i requisiti dei certificati, le impostazioni DNS, la distribuzione del server dei percorsi di rete, i server di Gestione DirectAccess, le impostazioni di Active Directory e gli oggetti di Criteri di gruppo (GPO).  
  
-   [Passaggio 2: pianificare la distribuzione del server RADIUS](Step-2-Plan-the-RADIUS-Server-Deployment.md)  
  
-   [Passaggio 3: pianificare la distribuzione di certificati OTP](Step-3-Plan-OTP-Certificate-Deployment.md)  
  
-   [Passaggio 4: pianificare l'OTP sul server di accesso remoto](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  
Dopo aver completato questi passaggi di pianificazione, vedere [configurare l'accesso remoto con l'autenticazione OTP](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/configure/configure-ra-with-otp-authentication). Per informazioni sulla configurazione di una distribuzione multisito come modello di prova in un ambiente lab, vedere [Guida al Lab di test: dimostrazione di DirectAccess con autenticazione OTP e RSA SecurID](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid).  
  


