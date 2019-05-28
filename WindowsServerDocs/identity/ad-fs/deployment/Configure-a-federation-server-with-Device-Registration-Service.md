---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: Configurare un server federativo con Device Registration Service
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f1367f03ea8a9ba96bfe4bae1c324deff92576f0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192265"
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>Configurare un server federativo con Device Registration Service

È possibile abilitare Device Registration Service \(DRS\) nel server federativo dopo aver completato le procedure descritte in [passaggio 4: Configurare un Server federativo](https://technet.microsoft.com/library/dn303424.aspx). Il servizio registrazione dispositivo fornisce un meccanismo di onboarding per trasparente l'autenticazione fattore, persistente di single sign\-sul \(SSO\)e l'accesso condizionale per i consumer che richiedono l'accesso alla società risorse. Per altre informazioni su DRS, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tra le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>Preparare la foresta di Active Directory per supportare i dispositivi  
  
> [!NOTE]  
> Si tratta di uno\-ora l'operazione è necessario eseguire per preparare la foresta di Active Directory per supportare i dispositivi. È necessario essere connessi con autorizzazioni di amministratore dell'organizzazione e la foresta di Active Directory deve avere lo schema di Windows Server 2012 R2 per completare questa procedura.  
>   
> DRS richiede inoltre di avere almeno un server di catalogo globale nel dominio radice foresta. Il server di catalogo globale è necessario per eseguire l'inizializzazione\-ADDeviceRegistration e durante l'autenticazione di AD FS. AD FS Inizializza un in\-rappresentazione memoria dei file di configurazione di DRS oggetto a ogni richiesta di autenticazione e se l'oggetto di configurazione di DRS non viene trovato un controller di dominio nel dominio corrente, la richiesta viene eseguito un tentativo di catalogo globale in cui sono stati gli oggetti servizio DRS il provisioning durante l'inizializzazione\-ADDeviceRegistration.  
  
#### <a name="to-prepare-the-active-directory-forest"></a>Per preparare la foresta di Active Directory  
  
1.  Nel server federativo, aprire una finestra di comando di Windows PowerShell e digitare:  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  Quando viene richiesto ServiceAccountName, immettere il nome dell'account del servizio selezionato come account del servizio per AD FS.  Se si tratta di un account gMSA, immettere l'account nel **domain\\NomeAccount$** formato. Per un account di dominio, usare il formato **domain\\NomeAccount**.  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>Abilitare il servizio registrazione dispositivo su un nodo della server farm federativo  
  
> [!NOTE]  
> È necessario essere connessi con autorizzazioni di amministratore di dominio per completare questa procedura.  
  
#### <a name="to-enable-device-registration-service"></a>Per abilitare il servizio registrazione dispositivo  
  
1.  Nel server federativo, aprire una finestra di comando di Windows PowerShell e digitare:  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  Ripetere questo passaggio in ogni nodo della farm federativo nella farm ADFS...  
  
## <a name="enable-seamless-second-factor-authentication"></a>Abilita trasparente secondo fattore di autenticazione  
Trasparente secondo fattore di autenticazione è un miglioramento in AD FS che fornisce un ulteriore livello di protezione dell'accesso a risorse aziendali e applicazioni da dispositivi esterni che sta tentando di accedere a tali. Quando un dispositivo personale è aggiunto all'area di lavoro, questa diventa un dispositivo 'noto' e gli amministratori possono utilizzare queste informazioni per gestire l'accesso condizionale e controllare l'accesso alle risorse.  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>Per abilitare senza problemi in secondo luogo l'autenticazione fattore, permanente l'accesso single sign\-sul \(SSO\) e l'accesso condizionale per i dispositivi con Workplace join  
  
1.  Nella console di gestione di AD FS, passare a criteri di autenticazione. Selezionare Modifica autenticazione primaria globale. Selezionare la casella di controllo accanto a Abilita autenticazione dispositivo e quindi fare clic su OK.  
  
## <a name="update-the-web-application-proxy-configuration"></a>Aggiornare la configurazione del Proxy applicazione Web  
  
> [!IMPORTANT]  
> Non è necessario pubblicare il servizio registrazione dispositivo per il Proxy applicazione Web.  Il servizio registrazione dispositivo sarà disponibile tramite il Proxy applicazione Web dopo essere stata attivata in un server federativo.  Si potrebbe essere necessario completare questa procedura per aggiornare la configurazione del Proxy applicazione Web se è stata distribuita prima di abilitare il servizio registrazione dispositivo.  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>Per aggiornare la configurazione del Proxy applicazione Web  
  
1.  Nel server Proxy applicazione Web, aprire una finestra di comando di Windows PowerShell e digitare  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  Alla richiesta delle credenziali, immettere le credenziali di un account che disponga dei diritti amministrativi per i server federativi.  
  
## <a name="see-also"></a>Vedere anche 

[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una server farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

