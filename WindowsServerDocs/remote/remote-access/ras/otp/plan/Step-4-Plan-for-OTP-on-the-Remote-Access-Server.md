---
title: Pianificare il passaggio 4 di OTP in Server di accesso remoto
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
ms.assetid: 4b97b2fd-767a-45c1-a64e-5b3edd0c8a47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 470eebc82177a21985afb8d0bf143427a33d65fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825442"
---
# <a name="step-4-plan-for-otp-on-the-remote-access-server"></a>Pianificare il passaggio 4 di OTP in Server di accesso remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Dopo aver pianificato la password monouso server RADIUS (OTP) e le impostazioni del certificato, il passaggio finale della pianificazione di una distribuzione OTP accesso remoto consiste nel pianificare per le impostazioni di OTP client nel server di accesso remoto.  
  
|Attività|Descrizione|  
|----|--------|  
|[4.1 piano per esenzioni OTP client](#bkmk_4_1_Exemptions)|Pianificare le esenzioni per gli utenti che non sono necessarie per l'autenticazione utilizzando OTP.|  
|[4.2 pianificare per i client Windows 7](#bkmk_4_2_Win7)|Pianificare la distribuzione di DirectAccess Connectivity Assistant (DCA) 2.0 per i computer client Windows 7.|  
|[4.3 piano per le smart card](#BKMK_smartcard)|Piano per l'uso di smart card per fornire autorizzazioni aggiuntive.|  
  
## <a name="bkmk_4_1_Exemptions"></a>4.1 piano per esenzioni OTP client  
Quando è abilitata l'autenticazione OTP, per impostazione predefinita tutti gli utenti devono eseguire l'autenticazione usando una combinazione di nome utente e password e le credenziali OTP. Tuttavia, è possibile consentire agli utenti selezionati di eseguire l'autenticazione usando un nome utente e password, solo senza OTP. A tale scopo, creare un gruppo di sicurezza e aggiungere tutti gli utenti che si desiderano per esenti dall'autenticazione OTP.  
  
> [!NOTE]  
> Solo i computer client da una singola foresta possono essere esentati dovuto al fatto che solo un gruppo di sicurezza può essere selezionato per esenzioni client.  
  
## <a name="bkmk_4_2_Win7"></a>4.2 pianificare per i client Windows 7  
Per impostazione predefinita, i computer client Windows 7 non è possibile eseguire l'autenticazione utilizzando OTP.  I computer client Windows 7 richiedono DCA 2.0 per l'autenticazione con OTP in una distribuzione di accesso remoto di Windows Server 2012. Per altre informazioni su DCA 2.0, vedere [DirectAccess Connectivity Assistant 2.0](https://go.microsoft.com/fwlink/?LinkId=253699) nel Microsoft Download Center.  
  
## <a name="BKMK_smartcard"></a>4.3 piano per le smart card  
Quando è abilitata l'autenticazione OTP, è disponibile l'opzione per consentire l'uso di smart card per fornire autorizzazioni aggiuntive. Creare un gruppo di sicurezza per consentire l'accesso temporaneo nel caso in cui una smart card utente non è funzionale.  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   [Configurare DirectAccess con l'autenticazione OTP](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/deploy-ra-otp)  
  


