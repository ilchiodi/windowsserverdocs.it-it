---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: Configurare un server federativo con Device Registration Service
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6d4285816993ffd277df471348149b3b54039939
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359775"
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>Configurare un server federativo con Device Registration Service

È possibile abilitare il servizio \(registrazione dispositivo DRS\) nel server federativo dopo aver completato le procedure descritte [nel passaggio 4: Configurare un server](https://technet.microsoft.com/library/dn303424.aspx)federativo. Il servizio Registrazione dispositivi fornisce un meccanismo di onboarding per l'autenticazione a due fattori trasparente,\-Single \(Sign\)-on permanente SSO e accesso condizionale ai consumer che richiedono l'accesso all'azienda risorse. Per altre informazioni su DRS, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e l'autenticazione a due fattori trasparente per tutte le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>Preparare la foresta Active Directory per supportare i dispositivi  
  
> [!NOTE]  
> Si tratta di un'\-operazione che è necessario eseguire una sola volta per preparare la foresta Active Directory per supportare i dispositivi. Per completare questa procedura, è necessario effettuare l'accesso con le autorizzazioni di amministratore dell'organizzazione e la foresta Active Directory deve avere lo schema di Windows Server 2012 R2.  
>   
> Inoltre, DRS richiede che sia presente almeno un server di catalogo globale nel dominio radice della foresta. Il server di catalogo globale è necessario per eseguire Initialize\-ADDeviceRegistration e durante l'autenticazione ad FS. AD FS Inizializza una rappresentazione\-in memoria dell'oggetto di configurazione DRS in ogni richiesta di autenticazione e se l'oggetto di configurazione DRS non è stato trovato in un controller di dominio nel dominio corrente, viene tentata la richiesta rispetto al GC in cui si trovano gli oggetti DRS provisioning eseguito durante\-l'inizializzazione di ADDeviceRegistration.  
  
#### <a name="to-prepare-the-active-directory-forest"></a>Per preparare la foresta Active Directory  
  
1.  Nel server federativo aprire una finestra di comando di Windows PowerShell e digitare:  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  Quando viene richiesto di ServiceAccountName, immettere il nome dell'account di servizio selezionato come account del servizio per AD FS.  Se si tratta di un account gMSA, immettere l'account nel **formato\\Domain AccountName $** . Per un account di dominio, utilizzare il **formato\\Domain AccountName**.  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>Abilitare il servizio registrazione dispositivo in un nodo di server farm di Federazione  
  
> [!NOTE]  
> Per completare questa procedura, è necessario essere connessi con le autorizzazioni di amministratore di dominio.  
  
#### <a name="to-enable-device-registration-service"></a>Per abilitare il servizio registrazione dispositivo  
  
1.  Nel server federativo aprire una finestra di comando di Windows PowerShell e digitare:  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  Ripetere questo passaggio in ogni nodo della farm della Federazione della AD FS farm.  
  
## <a name="enable-seamless-second-factor-authentication"></a>Abilitare l'autenticazione a due fattori trasparente  
L'autenticazione a due fattori trasparente è un miglioramento in AD FS che fornisce un livello aggiuntivo di protezione dell'accesso alle risorse e alle applicazioni aziendali da dispositivi esterni che tentano di accedervi. Quando un dispositivo personale è aggiunto all'area di lavoro, diventa un dispositivo noto e gli amministratori possono usare queste informazioni per guidare l'accesso condizionale e l'accesso alle risorse.  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>Per abilitare l'autenticazione a due fattori trasparente, Single\-Sign \(-\) on permanente SSO e accesso condizionale per i dispositivi aggiunti all'area di lavoro  
  
1.  Nella console di gestione di AD FS passare a criteri di autenticazione. Selezionare Modifica autenticazione primaria globale. Selezionare la casella di controllo accanto a Abilita autenticazione dispositivo e quindi fare clic su OK.  
  
## <a name="update-the-web-application-proxy-configuration"></a>Aggiornare la configurazione del proxy dell'applicazione Web  
  
> [!IMPORTANT]  
> Non è necessario pubblicare il servizio Device Registration sul proxy applicazione Web.  Il servizio registrazione dispositivo sarà disponibile tramite il proxy applicazione Web dopo che è stato abilitato in un server federativo.  Potrebbe essere necessario completare questa procedura per aggiornare la configurazione del proxy dell'applicazione Web se è stata distribuita prima di abilitare il servizio registrazione dispositivo.  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>Per aggiornare la configurazione del proxy dell'applicazione Web  
  
1.  Nel server proxy applicazione Web aprire una finestra di comando di Windows PowerShell e digitare  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  Quando vengono richieste le credenziali, immettere le credenziali di un account con diritti amministrativi per i server federativi.  
  
## <a name="see-also"></a>Vedere anche 

[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una server farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

