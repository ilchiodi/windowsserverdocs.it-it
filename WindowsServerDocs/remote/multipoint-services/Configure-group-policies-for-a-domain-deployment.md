---
title: Configurare i criteri di gruppo per la distribuzione di un dominio
description: Informazioni su come configurare i criteri di gruppo in servizi MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13e5fa90-d330-4155-a6b8-78eb650cbbfa
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: f661fbdc40fd7dd2562d51756bc7642c8e9a4a82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888042"
---
# <a name="configure-group-policies-for-a-domain-deployment"></a>Configurare i criteri di gruppo per la distribuzione di un dominio
Per garantire il corretto funzionamento della distribuzione di dominio di servizi MultiPoint, si applicano le seguenti impostazioni dei criteri di gruppo per l'account utente WMSshell su un sistema MultiPoint Services.  
  
> [!IMPORTANT]  
> Alcune impostazioni dei criteri di gruppo possono impedire le impostazioni di configurazione necessari vengano applicati ai servizi MultiPoint. Assicurarsi di comprendere e definire le impostazioni dei criteri di gruppo in modo che funzionino correttamente in MultiPoint Services. Ad esempio, un'impostazione di criteri di gruppo che impedisce l'accesso automatico può presentare problemi con il comportamento di accesso di MultiPoint Services.  
  
## <a name="update-group-policies-for-the-wmsshell-user-account"></a>Aggiornare i criteri di gruppo per l'account utente WMSshell 
L'account utente WMSshell è un account di sistema servizi MultiPoint viene utilizzato per effettuare l'accesso alla console in cui vengono create le stazioni actuall. Questo account non è mento essere gestito da Gestione MultiPoint.
  
> [!NOTE]  
> Per scoprire come aggiornare i criteri di gruppo, vedere [Editor criteri di gruppo locali](https://technet.microsoft.com/library/dn265982.aspx).  
  
**CRITERI:** Configurazione utente > modelli amministrativi > Pannello di controllo > **personalizzazione**  
  
Assegnare i valori seguenti:  
  
|Impostazione|Valori|  
|-----------|----------|  
|Abilita lo screen saver|Disabled|  
|Timeout screen saver|Disabled<br /><br />Secondi: xxx|  
|Proteggi screen saver con password|Disabled|  
  
**CRITERI:** Configurazione computer > Impostazioni di Windows > Impostazioni di sicurezza > Criteri locali > Assegnazione diritti utente > **Consenti accesso locale**  
  
|Impostazione|Valori|  
|-----------|----------|  
|Consenti accesso locale|Assicurarsi che l'elenco degli account includa l'account WMSshell.<br /><br />**Nota:** Per impostazione predefinita, l'account WMSshell è un membro del gruppo di utenti. Se è presente il gruppo di utenti nell'elenco e WMSshell è un membro del gruppo di utenti, non devi aggiungere l'account WMSshell all'elenco.|  
  
> [!IMPORTANT]  
> Quando si impostano criteri di gruppo, assicurarsi che i criteri non interferiscono con gli aggiornamenti automatici e segnalazione errori Windows errore sui server MultiPoint. Queste ultime vengono impostate il **installa gli aggiornamenti automaticamente** e **segnalazione errori Windows automatica** impostazioni che sono state selezionate durante l'installazione di Windows MultiPoint Server, configurata in MultiPoint Gestione tramite **modificare le impostazioni del server**, o configurati negli aggiornamenti pianificati per la protezione disco.  
  
## <a name="update-the-registry"></a>Aggiornare il Registro di sistema  
Per una distribuzione di dominio di servizi MultiPoint, è necessario aggiornare le seguenti sottochiavi del Registro di sistema.  
  
> [!IMPORTANT]  
> La modifica non corretta del Registro di sistema potrebbe danneggiare gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.  
  
#### <a name="to-update-registry-subkeys-for-a-domain-deployment-of-multipoint-services"></a>Per aggiornare le sottochiavi del Registro di sistema per la distribuzione di un dominio di servizi MultiPoint  
  
1.  Aprire l'editor del Registro di sistema. (Un prompt dei comandi, digitare **regedit.exe**, quindi premere INVIO.)  
  
2.  Nel riquadro sinistro, individuare e quindi selezionare la sottochiave del Registro di sistema seguente:  
  
    HKEY_USERS\<SIDofWMSshell > \Software\Policies\Microsoft\Windows\Control Panel\Desktop  
  
    dove '<SIDofWMSshell>' è l'identificatore di sicurezza (SID) per l'account WMSshell. Per scoprire come identificare il SID, vedere [come associare un nome utente con un ID di sicurezza (SID)](https://support.microsoft.com/kb/154599).  
  
3.  Nell'elenco a destra, aggiornare le seguenti sottochiavi.  
  
    |Sottochiave|Nome del valore|Dati valore|  
    |----------|--------------|--------------|  
    |ScreenSaveActive|REG_SZ|0 (zero)|  
    |ScreenSaveTimeout|REG_SZ|120|  
    |ScreenSaverIsSecure|REG_SZ|0 (zero)|  
  
    Per aggiornare una sottochiave del Registro di sistema:  
  
    1.  Con la chiave del Registro di sistema selezionata nel riquadro sinistro, destro la sottochiave nel riquadro di destra e quindi fare clic su **Modify**.  
  
    2.  Nella finestra di dialogo Modifica stringa, digitare un nuovo valore in **dati valore**, quindi fare clic su **OK**.  
  
4.  Dopo aver completato l'aggiornamento di sottochiavi del Registro di sistema, riavviare il computer per attivare le modifiche. 