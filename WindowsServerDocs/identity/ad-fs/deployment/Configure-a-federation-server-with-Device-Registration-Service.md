---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: Configurare un server federativo con Device Registration Service
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 511a039afd47cf7570fffdcaf17842e0eccc5683
ms.sourcegitcommit: 9278435cbfa8dbeb30d0557ed0d27832b154edd2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2018
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>Configurare un server federativo con Device Registration Service

>Si applica a: Windows Server 2012 R2

È possibile abilitare Device Registration Service \(DRS\) nel server federativo dopo aver completato le procedure descritte in [passaggio 4: configurare un Server federativo](https://technet.microsoft.com/library/dn303424.aspx). Il servizio Registrazione dispositivi di fornisce un meccanismo di onboarding trasparente secondo fattore di autenticazione, permanente singolo accesso-on \(SSO\) e l'accesso condizionale per i consumatori che richiedono l'accesso alle risorse aziendali. Per ulteriori informazioni su DRS, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tutte le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>Preparare la foresta di Active Directory per supportare i dispositivi  
  
> [!NOTE]  
> Questa è un'operazione in fase di base che è necessario eseguire per preparare la foresta di Active Directory per supportare i dispositivi. È necessario essere connessi con autorizzazioni di amministratore dell'organizzazione e della foresta di Active Directory deve avere lo schema di Windows Server 2012 R2 per completare questa procedura.  
>   
> DRS richiede inoltre di disporre di almeno un server di catalogo globale nel dominio radice della foresta. Il server di catalogo globale è necessario per eseguire ADDeviceRegistration Initialize\ e durante l'autenticazione di ADFS. AD FS Inizializza una rappresentazione di memoria attive dell'oggetto di configurazione di DRS in ogni richiesta di autenticazione e se l'oggetto di configurazione di DRS non viene trovato in un controller di dominio nel dominio corrente, la richiesta verrà tentata il catalogo globale in cui gli oggetti DRS sono eseguito il provisioning durante ADDeviceRegistration Initialize\.  
  
#### <a name="to-prepare-the-active-directory-forest"></a>Per preparare la foresta di Active Directory  
  
1.  Nel server federativo, aprire una finestra di comando di Windows PowerShell e digitare:  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  Quando viene richiesto ServiceAccountName, immettere il nome dell'account del servizio selezionato come account del servizio per ADFS.  Se un account gestito, immettere l'account nel **domain\\accountname$** formato. Per un account di dominio, usare il formato **domain\\accountname**.  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>Abilitare Device Registration Service in un nodo di farm di server federativo  
  
> [!NOTE]  
> È necessario essere connessi con autorizzazioni di amministratore di dominio per completare questa procedura.  
  
#### <a name="to-enable-device-registration-service"></a>Per abilitare Device Registration Service  
  
1.  Nel server federativo, aprire una finestra di comando di Windows PowerShell e digitare:  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  Ripetere questo passaggio in ogni nodo della farm federativo nella farm ADFS.  
  
## <a name="enable-seamless-second-factor-authentication"></a>Enable trasparente secondo fattore di autenticazione  
Trasparente secondo fattore di autenticazione è una funzionalità migliorativa di ADFS che fornisce un ulteriore livello di protezione accesso alle risorse aziendale e applicazioni da dispositivi esterni che sta tentando di accedervi. Quando un dispositivo personale viene aggiunto all'area di lavoro, diventa un dispositivo "noto" e gli amministratori possono utilizzare queste informazioni per favorire l'accesso condizionale e controllo accesso alle risorse.  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>Per abilitare trasparente secondo fattore di autenticazione, permanente singolo accesso-on \(SSO\) e l'accesso condizionale per i dispositivi aggiunti all'area di lavoro  
  
1.  Nella console di gestione di ADFS, passare a criteri di autenticazione. Selezionare Modifica autenticazione primaria globale. Selezionare la casella di controllo Abilita autenticazione dispositivo e quindi fare clic su OK.  
  
## <a name="update-the-web-application-proxy-configuration"></a>Aggiornare la configurazione del Proxy dell'applicazione Web  
  
> [!IMPORTANT]  
> Non devi pubblicare il servizio Registrazione dispositivi di Proxy applicazione Web.  Il servizio Registrazione dispositivi di sarà disponibile tramite il Proxy dell'applicazione Web quando è abilitata in un server federativo.  Potrebbe essere necessario completare questa procedura per aggiornare la configurazione di Proxy applicazione Web se è stato distribuito prima di abilitare il servizio Registrazione dispositivi.  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>Per aggiornare la configurazione del Proxy applicazione Web  
  
1.  Nel server Proxy applicazione Web, aprire una finestra di comando Windows PowerShell e digitare  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  Alla richiesta di credenziali, immettere le credenziali di un account che disponga di diritti amministrativi ai server federativi.  
  
## <a name="see-also"></a>Vedere anche 

[Distribuzione di ADFS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una Server Farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

