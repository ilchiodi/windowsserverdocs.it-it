---
title: Risoluzione dei problemi dell'abilitazione di OTP
description: Questo argomento fa parte della Guida distribuire accesso remoto con l'autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: b58252ca-4c1d-4664-a3c4-7301e2121517
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9be7ef4c4d07b522f683a403e46a11e109dbd226
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853644"
---
# <a name="troubleshooting-enabling-otp"></a>Risoluzione dei problemi dell'abilitazione di OTP

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento contiene informazioni sulla risoluzione dei problemi relativi all'abilitazione dell'autenticazione OTP di DirectAccess tramite il cmdlet **Enable-DAOtpAuthentication di** PowerShell o la console di gestione accesso remoto.
  
## <a name="failed-to-enroll-the-otp-signing-certificate"></a>Non è stato possibile registrare il certificato di firma OTP  
**Errore ricevuto** (registro eventi del server). Non è possibile registrare un certificato di firma OTP usando un modello di certificato < OTP_signing_template_name >  
  
**Causa**  
  
Questo errore può essere dovuto a tre cause:  
  
-   Il modello non esiste.  
  
-   Le autorizzazioni impostate per il modello non consentono la registrazione del server DirectAccess.  
  
-   Non è disponibile connettività di rete all'autorità di certificazione emittente (CA).  
  
**Soluzione**  
  
1.  Verificare che il modello di certificato di firma OTP con il nome specificato:  
  
    1.  Esiste e dispone delle autorizzazioni appropriate.  
  
    2.  È impostato per essere emesso da almeno una CA che può emettere certificati per il server DirectAccess.  
  
2.  Se il modello non esiste, crearlo come descritto in 3,3 pianificare il certificato dell'autorità di registrazione o se è presente un altro modello corrispondente riconfigurare OTP DirectAccess con il nome del nuovo modello.  
  
## <a name="failed-to-enable-directaccess-otp-when-webdav-is-installed"></a>Non è stato possibile abilitare OTP DirectAccess quando è installato WebDAV  
**Scenario**. Quando si tenta di applicare la configurazione OTP di DirectAccess nella console di gestione accesso remoto o usando il cmdlet `Enable-DAOtpAuthentication` PowerShell, l'operazione ha esito negativo.  
  
**Errore ricevuto** (registro eventi del server). Impossibile applicare le impostazioni OTP di DirectAccess perché l'estensione IIS WebDAV è in esecuzione nel server. Rimuovere WebDAV e applicare di nuovo le impostazioni.  
  
**Causa**  
  
Il servizio OTP DirectAccess non è compatibile con la funzionalità di pubblicazione WebDAV e non può essere abilitato mentre WebDAV è installato.  
  
**Soluzione**  
  
Disinstallare il ruolo WebDAV:  
  
1.  Nel riquadro sinistro della console di Server Manager fare clic su **IIS**.  
  
2.  Nel riquadro principale scorrere fino a **ruoli e funzionalità**.  
  
3.  Fare clic con il pulsante destro del mouse su **WebDAV publishing**, quindi scegliere **Rimuovi ruolo o funzionalità**.  
  
4.  Completare la rimozione guidata ruoli e funzionalità.  
  
5.  Applicare nuovamente la configurazione OTP di DirectAccess.  
  
## <a name="no-templates-available-in-the-remote-access-management-console"></a>Nessun modello disponibile nella console di gestione accesso remoto  
**Scenario**. Durante la configurazione dei modelli di certificato dell'autorità di registrazione o OTP utilizzando la console di gestione accesso remoto, alcuni o tutti i modelli non sono presenti nelle finestre di selezione.  
  
**Causa**  
  
Questo errore può essere dovuto a due cause:  
  
-   Il modello non è configurato in base ai requisiti OTP di DirectAccess e pertanto non può essere selezionato.  
  
-   Le autorità di certificazione selezionate in **server CA OTP** non sono configurate per emettere i modelli richiesti.  
  
**Soluzione**  
  
1.  Verificare che il modello di accesso OTP e il modello di certificato di firma OTP siano configurati correttamente come descritto in 3,2 pianificare il modello di certificato OTP e 3,3 pianificare il certificato dell'autorità di registrazione.  
  
2.  Verificare che le autorità di certificazione configurate nell'elenco **server CA OTP** siano configurate per l'emissione dei modelli pertinenti:  
  
    1.  Nel server CA aprire la console autorità di certificazione.  
  
    2.  Nel riquadro sinistro espandere il server CA scelto.  
  
    3.  Fare clic su **modelli di certificato** e verificare che i modelli richiesti siano abilitati. In caso contrario, fare clic con il pulsante destro del mouse su **modelli di certificato**, scegliere **nuovo**, fare clic su **modello di certificato da emettere**, quindi selezionare i modelli che si desidera abilitare.  
  
## <a name="cannot-set-renewal-period-of-otp-template-to-1-hour"></a>Impossibile impostare il periodo di rinnovo del modello OTP su un'ora  
**Scenario**. Quando si configura il modello di accesso OTP di DirectAccess utilizzando la CA Windows 2003, non è possibile impostare il periodo di rinnovo del modello su 1 ora.  
  
**Causa**  
  
Lo snap-in MMC modelli di certificato in Windows Server 2003 non consente di impostare il periodo di rinnovo di un modello su 1 ora.  
  
**Soluzione**  
  
Installare lo snap-in modelli di certificato in un server Post-Windows Server 2003 e usarlo per configurare il modello di accesso OTP. vedere [installare lo snap-in modelli di certificato](https://technet.microsoft.com/library/cc732445.aspx).  
  


