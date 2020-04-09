---
title: Piano passaggio 4 per OTP sul server di accesso remoto
description: Questo argomento fa parte della Guida distribuire accesso remoto con l'autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4b97b2fd-767a-45c1-a64e-5b3edd0c8a47
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 789fd72e2f3fc1693bf4803f33dcc1e7f1b3acc3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855774"
---
# <a name="step-4-plan-for-otp-on-the-remote-access-server"></a>Piano passaggio 4 per OTP sul server di accesso remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Dopo aver pianificato il server RADIUS e le impostazioni dei certificati monouso (OTP), il passaggio finale della pianificazione di una distribuzione OTP di accesso remoto consiste nel pianificare le impostazioni OTP client nel server di accesso remoto.  
  
|Attività|Descrizione|  
|----|--------|  
|[4,1 pianificare le esenzioni dei client OTP](#bkmk_4_1_Exemptions)|Pianificare le esenzioni per gli utenti che non sono necessari per l'autenticazione tramite OTP.|  
|[4,2 piano per client Windows 7](#bkmk_4_2_Win7)|Pianificare la distribuzione di DirectAccess Connectivity Assistant (DCA) 2,0 ai computer client Windows 7.|  
|[4,3 pianificare le smart card](#BKMK_smartcard)|Pianificare l'utilizzo di smart card per un'ulteriore autorizzazione.|  
  
## <a name="41-plan-for-otp-client-exemptions"></a><a name="bkmk_4_1_Exemptions"></a>4,1 pianificare le esenzioni dei client OTP  
Quando è abilitata l'autenticazione OTP, per impostazione predefinita tutti gli utenti devono eseguire l'autenticazione utilizzando una combinazione di nome utente e password e le credenziali OTP. Tuttavia, è possibile consentire agli utenti selezionati di eseguire l'autenticazione usando solo un nome utente e una password, senza OTP. A tale scopo, creare un gruppo di sicurezza e aggiungere tutti gli utenti che si desidera esentare dall'autenticazione OTP.  
  
> [!NOTE]  
> Solo i computer client di una singola foresta possono essere esentati a causa del fatto che è possibile selezionare solo un gruppo di sicurezza per le esenzioni client.  
  
## <a name="42-plan-for-windows-7-clients"></a><a name="bkmk_4_2_Win7"></a>4,2 piano per client Windows 7  
Per impostazione predefinita, i computer client Windows 7 non possono eseguire l'autenticazione con OTP.  I computer client Windows 7 richiedono il DCA 2,0 per l'autenticazione tramite OTP in una distribuzione di accesso remoto di Windows Server 2012. Per ulteriori informazioni su DCA 2,0, vedere [DirectAccess Connectivity Assistant 2,0](https://go.microsoft.com/fwlink/?LinkId=253699) nell'area download Microsoft.  
  
## <a name="43-plan-for-smart-cards"></a><a name="BKMK_smartcard"></a>4,3 pianificare le smart card  
Quando è abilitata l'autenticazione OTP, è disponibile l'opzione per abilitare l'uso delle smart card per l'autorizzazione aggiuntiva. Creare un gruppo di sicurezza per consentire l'accesso temporaneo nel caso in cui la smart card di un utente non sia funzionante.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Vedere anche  
  
-   [Configurare DirectAccess con l'autenticazione OTP](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/deploy-ra-otp)  
  


