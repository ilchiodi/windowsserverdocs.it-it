---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: Configurare un server federativo con Device Registration Service
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c7775801940faeba07ad91aa81434a34c97eb6bc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855514"
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>Configurare un server federativo con Device Registration Service

È possibile abilitare il servizio registrazione dispositivo \(DRS\) nel server federativo dopo aver completato le procedure descritte in [passaggio 4: configurare un server federativo](https://technet.microsoft.com/library/dn303424.aspx). Il servizio Registrazione dispositivi fornisce un meccanismo di onboarding per l'autenticazione a due fattori trasparente, un Single Sign\-permanente su \(\)SSO e l'accesso condizionale ai consumer che richiedono l'accesso alle risorse aziendali. Per altre informazioni su DRS, vedere [accedere a una rete aziendale da qualsiasi dispositivo per SSO e l'autenticazione a due fattori trasparente per tutte le applicazioni aziendali](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>Preparare la foresta Active Directory per supportare i dispositivi  
  
> [!NOTE]  
> Si tratta di un'operazione\-tempo che è necessario eseguire per preparare la foresta Active Directory a supportare i dispositivi. Per completare questa procedura, è necessario effettuare l'accesso con le autorizzazioni di amministratore dell'organizzazione e la foresta Active Directory deve avere lo schema di Windows Server 2012 R2.  
>   
> Inoltre, DRS richiede che sia presente almeno un server di catalogo globale nel dominio radice della foresta. Il server di catalogo globale è necessario per eseguire Initialize\-ADDeviceRegistration e durante l'autenticazione AD FS. AD FS Inizializza un in\-rappresentazione di memoria dell'oggetto di configurazione DRS in ogni richiesta di autenticazione e se l'oggetto di configurazione DRS non è stato trovato in un controller di dominio nel dominio corrente, la richiesta viene tentata rispetto al GC in cui è stato effettuato il provisioning degli oggetti DRS durante l'inizializzazione\-ADDeviceRegistration.  
  
#### <a name="to-prepare-the-active-directory-forest"></a>Per preparare la foresta Active Directory  
  
1.  Nel server federativo aprire una finestra di comando di Windows PowerShell e digitare:  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  Quando viene richiesto di specificare ServiceAccountName, immettere il nome dell'account del servizio selezionato come account del servizio per ADFS.  Se si tratta di un account gMSA, immettere l'account nel **dominio\\AccountName $** format. Per un account di dominio, utilizzare il formato **dominio\\AccountName**.  
  
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
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>Per abilitare l'autenticazione a due fattori trasparente, l'accesso Single Sign\-permanente nei\) \(SSO e l'accesso condizionale per i dispositivi aggiunti all'area di lavoro  
  
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
  
## <a name="see-also"></a>Vedi anche 

[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una server farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

