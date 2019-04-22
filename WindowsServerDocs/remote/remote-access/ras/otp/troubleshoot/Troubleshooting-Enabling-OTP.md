---
title: Risoluzione dei problemi dell'abilitazione di OTP
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
ms.assetid: b58252ca-4c1d-4664-a3c4-7301e2121517
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d789671a0425974e560e5f4a43ebcba4c1741c76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819462"
---
# <a name="troubleshooting-enabling-otp"></a>Risoluzione dei problemi dell'abilitazione di OTP

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento contiene informazioni sulla risoluzione dei problemi relativi all'abilitazione dell'autenticazione OTP di DirectAccess usando la **Enable-DAOtpAuthentication** cmdlet di PowerShell o la console Gestione accesso remoto.
  
## <a name="failed-to-enroll-the-otp-signing-certificate"></a>Non è riuscito a registrare il certificato di firma OTP  
**Errore ricevuto** (registro eventi del server). Un certificato di autenticazione OTP non può essere registrato usando il modello di certificato < OTP_signing_template_name >  
  
**Causa**  
  
Esistono tre possibili cause di questo errore:  
  
-   Il modello non esiste.  
  
-   Le autorizzazioni impostate per il modello non consente al server DirectAccess di registrarsi.  
  
-   Non è disponibile connettività di rete all'autorità di certificazione emittente (CA).  
  
**Soluzione**  
  
1.  Assicurarsi che la firma di OTP certificato modello con il nome specificato:  
  
    1.  Esista e disponga delle autorizzazioni appropriate.  
  
    2.  È impostato su vengano emessi da almeno una CA che possa emettere certificati per il server DirectAccess.  
  
2.  Se il modello non esiste, crearla come descritto nel piano 3.3 il certificato dell'autorità di registrazione o se esiste un altro modello corrisponda riconfigurare OTP di DirectAccess con il nuovo nome del modello.  
  
## <a name="failed-to-enable-directaccess-otp-when-webdav-is-installed"></a>Non è stato possibile abilitare OTP di DirectAccess quando è installato WebDAV  
**Scenario**. Durante il tentativo di applicare la configurazione di OTP di DirectAccess nella console di gestione accesso remoto o tramite il `Enable-DAOtpAuthentication` cmdlet di PowerShell, l'operazione non riesce.  
  
**Errore ricevuto** (registro eventi del server). Impossibile applicare le impostazioni OTP di DirectAccess perché l'estensione WebDAV IIS è in esecuzione nel server. Rimuovere WebDAV e applicare nuovamente le impostazioni.  
  
**Causa**  
  
Il servizio di OTP di DirectAccess non è compatibile con la funzionalità di pubblicazione WebDAV e non può essere abilitato anche se WebDAV è installato.  
  
**Soluzione**  
  
Disinstallare il ruolo WebDAV:  
  
1.  Nella console di Server Manager, nel riquadro di sinistra, fare clic su **IIS**.  
  
2.  Nel riquadro principale, scorrere fino alla **ruoli e funzionalità**.  
  
3.  Fare doppio clic su **pubblicazioni WebDAV**, quindi fare clic su **Rimuovi ruolo o funzionalità**.  
  
4.  Completare la rimozione guidata ruoli e funzionalità.  
  
5.  Applicare di nuovo la configurazione di OTP di DirectAccess.  
  
## <a name="no-templates-available-in-the-remote-access-management-console"></a>Nessun modello disponibile nella console di gestione accesso remoto  
**Scenario**. Durante la configurazione OTP o registrazione modelli di certificato di autorità mediante la console Gestione accesso remoto, alcuni o tutti i modelli non sono presenti dalle finestre di selezione.  
  
**Causa**  
  
Esistono due possibili cause di questo errore:  
  
-   Il modello non è configurato in base ai requisiti di OTP di DirectAccess e pertanto non può essere selezionato.  
  
-   Le autorità di certificazione selezionate sotto **server CA OTP** non sono configurate per emettere i modelli necessari.  
  
**Soluzione**  
  
1.  Assicurarsi che il modello di accesso OTP e il modello di certificato OTP firma siano configurati correttamente come descritto nel piano 3.2, 3.3 e il modello di certificato OTP prevede il certificato dell'autorità di registrazione.  
  
2.  Assicurarsi che il server accesso client configurato nel **server CA OTP** elenco sono configurati per i problemi i modelli rilevanti:  
  
    1.  Nel server CA, aprire la console Autorità di certificazione.  
  
    2.  Nel riquadro sinistro, espandere il server di autorità di certificazione prescelto.  
  
    3.  Fare clic su **modelli di certificato** e assicurarsi che i modelli necessari sono abilitati. In caso contrario, fare doppio clic su **modelli di certificato**, fare clic su **New**, fare clic su **modello di certificato da emettere**e quindi selezionare i modelli che si desidera abilitare.  
  
## <a name="cannot-set-renewal-period-of-otp-template-to-1-hour"></a>Non è possibile impostare il periodo di rinnovo del modello OTP a 1 ora  
**Scenario**. Quando si configura il modello di accesso di OTP di DirectAccess tramite autorità di certificazione di Windows 2003, non è possibile impostare il periodo di rinnovo del modello a 1 ora.  
  
**Causa**  
  
Lo snap-in MMC di modelli di certificato in Windows Server 2003 non consente di impostare il periodo di rinnovo di un modello a 1 ora.  
  
**Soluzione**  
  
Installare lo snap-in modelli di certificato in un server di post-Windows Server 2003 e usarla per configurare il modello di accesso OTP, vedere [installare lo Snap-In modelli di certificato](https://technet.microsoft.com/library/cc732445.aspx).  
  


