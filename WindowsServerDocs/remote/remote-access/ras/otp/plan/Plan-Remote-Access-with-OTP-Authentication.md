---
title: Pianificare l'accesso remoto con l'autenticazione OTP
description: Questo argomento fa parte della Guida distribuire accesso remoto con l'autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 762bc463-eead-46ac-8b90-32355743c27c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ddcfa2898f4b90bf724a547bb16244cfad4ab3ff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858194"
---
# <a name="plan-remote-access-with-otp-authentication"></a>Pianificare l'accesso remoto con l'autenticazione OTP

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

 Windows Server 2016 e Windows Server 2012 combinano DirectAccess e la VPN del servizio Routing e accesso remoto (RRAS) in un singolo ruolo accesso remoto. Questa panoramica offre un'introduzione ai passaggi di configurazione necessari per distribuire una singola distribuzione multisito di accesso remoto di Windows Server 2016 o Windows Server 2012.  
  
  
-  Passaggio 1: [distribuire un server DirectAccess singolo con impostazioni avanzate](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Questo passaggio include la pianificazione dell'infrastruttura necessaria per distribuire un singolo server. Include la pianificazione delle impostazioni di rete e del server, i requisiti dei certificati, le impostazioni DNS, la distribuzione del server dei percorsi di rete, i server di Gestione DirectAccess, le impostazioni di Active Directory e gli oggetti di Criteri di gruppo (GPO).  
  
-   [Passaggio 2: pianificare la distribuzione del server RADIUS](Step-2-Plan-the-RADIUS-Server-Deployment.md)  
  
-   [Passaggio 3: pianificare la distribuzione di certificati OTP](Step-3-Plan-OTP-Certificate-Deployment.md)  
  
-   [Passaggio 4: pianificare l'OTP sul server di accesso remoto](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  
Dopo aver completato questi passaggi di pianificazione, vedere [configurare l'accesso remoto con l'autenticazione OTP](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/configure/configure-ra-with-otp-authentication). Per informazioni sulla configurazione di una distribuzione multisito come modello di prova in un ambiente lab, vedere [Guida al Lab di test: dimostrazione di DirectAccess con autenticazione OTP e RSA SecurID](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid).  
  


