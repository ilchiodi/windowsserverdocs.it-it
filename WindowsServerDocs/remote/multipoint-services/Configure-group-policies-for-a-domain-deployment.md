---
title: Configurare i criteri di gruppo per la distribuzione di un dominio
description: Informazioni su come configurare i criteri di gruppo in MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13e5fa90-d330-4155-a6b8-78eb650cbbfa
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 5ac6524289d231d152e366d2ba750a59d27ce14f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395515"
---
# <a name="configure-group-policies-for-a-domain-deployment"></a>Configurare i criteri di gruppo per la distribuzione di un dominio
Per assicurarsi che la distribuzione del dominio di MultiPoint Services funzioni correttamente, applicare le seguenti impostazioni di criteri di gruppo all'account utente WMSshell in un sistema MultiPoint Services.  
  
> [!IMPORTANT]  
> Alcune impostazioni di criteri di gruppo possono impedire l'applicazione delle impostazioni di configurazione richieste a MultiPoint Services. Assicurarsi di aver compreso e definito le impostazioni di criteri di gruppo in modo che funzionino correttamente in MultiPoint Services. Ad esempio, un'impostazione Criteri di gruppo che impedisce l'accesso automatico può presentare problemi con il comportamento di accesso a MultiPoint Services.  
  
## <a name="update-group-policies-for-the-wmsshell-user-account"></a>Aggiornare i criteri di gruppo per l'account utente WMSshell 
L'account utente WMSshell è un account di sistema usato da MultiPoint Services per accedere alla console, in cui vengono create le stazioni effettive. Questo account non è destinato a essere gestito da Gestione MultiPoint.
  
> [!NOTE]  
> Per informazioni su come aggiornare i criteri di gruppo, vedere [Editor criteri di gruppo locali](https://technet.microsoft.com/library/dn265982.aspx).  
  
**Criterio:** Configurazione utente > modelli amministrativi > Pannello di controllo > **personalizzazione**  
  
Assegnare i valori seguenti:  
  
|Impostazione|Valori|  
|-----------|----------|  
|Abilita screen saver|Disabled|  
|Timeout screen saver|Disabled<br /><br />Secondi: xxx|  
|Proteggi screen saver con password|Disabled|  
  
**Criterio:** Configurazione computer > impostazioni di Windows > impostazioni di sicurezza > criteri locali > Assegnazione diritti utente > **Consenti accesso locale**  
  
|Impostazione|Valori|  
|-----------|----------|  
|Consenti accesso locale|Verificare che l'elenco di account includa l'account WMSshell.<br /><br />**Nota:** Per impostazione predefinita, l'account WMSshell è un membro del gruppo Users. Se il gruppo Users è presente nell'elenco e WMSshell è un membro del gruppo Users, non è necessario aggiungere l'account WMSshell all'elenco.|  
  
> [!IMPORTANT]  
> Quando si impostano criteri di gruppo, assicurarsi che i criteri non interferiscano con aggiornamenti automatici e segnalazione errori Windows nel server MultiPoint. Queste impostazioni vengono impostate dalle impostazioni di **installazione** **automatica e automatica segnalazione errori Windows** selezionate durante l'installazione di Windows MultiPoint Server, configurate in Gestione MultiPoint mediante **Modifica impostazioni server**o configurate in aggiornamenti pianificati per la protezione del disco.  
  
## <a name="update-the-registry"></a>Aggiornare il registro di sistema  
Per la distribuzione di un dominio di MultiPoint Services, è necessario aggiornare le sottochiavi del registro di sistema seguenti.  
  
> [!IMPORTANT]  
> La modifica non corretta del Registro di sistema potrebbe danneggiare gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.  
  
#### <a name="to-update-registry-subkeys-for-a-domain-deployment-of-multipoint-services"></a>Per aggiornare le sottochiavi del registro di sistema per la distribuzione di un dominio di MultiPoint Services  
  
1.  Aprire l'editor del registro di sistema. Al prompt dei comandi digitare **Regedit. exe**e premere INVIO.  
  
2.  Nel riquadro sinistro individuare e selezionare la sottochiave del registro di sistema seguente:  
  
    HKEY_USERS\<SIDofWMSshell > \Software\Policies\Microsoft\Windows\Control Panel\Desktop  
  
    dove '<SIDofWMSshell>' è l'ID di sicurezza (SID) per l'account WMSshell. Per informazioni su come identificare il SID, vedere [come associare un nome utente a un ID di sicurezza (SID)](https://support.microsoft.com/kb/154599).  
  
3.  Nell'elenco a destra aggiornare le sottochiavi riportate di seguito.  
  
    |Sottochiave|Nome del valore|Dati valore|  
    |----------|--------------|--------------|  
    |ScreenSaveActive|REG_SZ|0 (zero)|  
    |ScreenSaveTimeout|REG_SZ|120|  
    |ScreenSaverIsSecure|REG_SZ|0 (zero)|  
  
    Per aggiornare una sottochiave del registro di sistema:  
  
    1.  Con la chiave del registro di sistema selezionata nel riquadro sinistro, fare clic con il pulsante destro del mouse sulla sottochiave nel riquadro destro e quindi scegliere **modifica**.  
  
    2.  Nella finestra di dialogo Modifica stringa digitare un nuovo valore in **dati valore**, quindi fare clic su **OK**.  
  
4.  Al termine dell'aggiornamento delle sottochiavi del registro di sistema, riavviare il computer per attivare le modifiche. 
