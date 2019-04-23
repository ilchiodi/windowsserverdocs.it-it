---
title: Pianificare l'accesso remoto con l'autenticazione OTP
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
ms.assetid: 762bc463-eead-46ac-8b90-32355743c27c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fafc41ef5dbd2cd7f98fa2552426a97622ec8634
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863222"
---
# <a name="plan-remote-access-with-otp-authentication"></a>Pianificare l'accesso remoto con l'autenticazione OTP

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

 Windows Server 2016 e Windows Server 2012 combina DirectAccess e Routing e accesso remoto (RRAS) VPN in un unico ruolo Accesso remoto. Questa panoramica fornisce un'introduzione ai passaggi di configurazione necessari per distribuire una distribuzione multisito singola accesso remoto di Windows Server 2012 o Windows Server 2016.  
  
  
-  Passaggio 1: [Distribuire un Server DirectAccess singolo con impostazioni avanzate](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Questo passaggio consente di pianificare l'infrastruttura necessaria per distribuire un server singolo. Include la pianificazione per la rete e le impostazioni del server, i requisiti dei certificati, le impostazioni DNS, distribuzione server del percorso di rete, server di Gestione DirectAccess, le impostazioni di Active Directory e oggetti Criteri di gruppo (GPO).  
  
-   [Passaggio 2: Pianificare la distribuzione di server RADIUS](Step-2-Plan-the-RADIUS-Server-Deployment.md)  
  
-   [Passaggio 3: Pianificare la distribuzione del certificato OTP](Step-3-Plan-OTP-Certificate-Deployment.md)  
  
-   [Passaggio 4: Piano per OTP sul server di accesso remoto](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  
Dopo aver completato questi passaggi di pianificazione, vedere [configurare l'accesso remoto con l'autenticazione OTP](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/configure/configure-ra-with-otp-authentication). Per informazioni su come configurare una distribuzione multisito come un modello di verifica in un ambiente lab, vedere [Guida al Lab di Test: Dimostrazione di DirectAccess con autenticazione OTP e SecurID RSA](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid).  
  


