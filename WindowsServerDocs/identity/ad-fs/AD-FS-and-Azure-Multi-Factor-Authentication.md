---
ms.assetid: 0f7e56fe-1cbc-43ff-bb87-f4672137ed7f
title: Autenticazione multifattore ADFS e Azure
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 67ce41bbd351bedc8d87e31ddb79c48338e44de9
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: it-IT
---
# <a name="ad-fs-and-azure-multi-factor-authentication"></a>Autenticazione multifattore ADFS e Azure

>Si applica a: Windows Server 2016

AD FS in Windows Server 2016 introduce e sfrutta le funzionalità di autenticazione a più fattori che sono stati introdotti con Windows Server 2012 R2.   Provider non è mai stata più semplice grazie all'introduzione di un adattatore AMF Azure, installazione e configurazione per l'utilizzo di Azure AMF come autenticazione primaria incorporato.

## <a name="why-use-ad-fs-with-azure-mfa"></a>Perché utilizzare ADFS con Azure AMF
Attualmente esistono alcune limitazioni all'utilizzo di autenticazione avanzata con AD FS.  Con AD FS in Windows Server 2016, questi sono stati risolti e organizzazioni possono avvalersi dei usuing AMF Azure con la relativa implementazione di ADFS.  Le organizzazioni possono sfruttare le seguenti:

-   Un'azienda di ADFS può facilmente essere configurata per utilizzare autenticazione multifattore con Azure AMF senza eccessive operazioni di configurazione o un'infrastruttura aggiuntiva.

-   L'esperienza AMF incorporato è facile da installare con Windows Server 2016

-   AMF Azure può essere configurato come provider di autenticazione primaria consentendo l'utilizzo di un meccanismo di autenticazione alternative per intranet o extranet.

## <a name="how-it-works"></a>Come funziona
Affinché AMF Azure da utilizzare come provider di autenticazione principale, il provider di autenticazione verrà considerato come gli altri provider incorporati.  Ovvero, come maschere o autenticazione basata su certificati.  L'unica differenza sarà che il provider verrà implementato come provider di autenticazione esterno tramite un'interfaccia che viene esposta solo al provider di Azure AMF di implementazione.

Di seguito viene illustrato il flusso di dati tra ADFS in Windows Server 2016 e Azure AMF.

![ADFS e AMF](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_1.png)

## <a name="before-you-get-started"></a>Prima di iniziare
Di seguito è un elenco di informazioni che è opportuno conoscere prima di tentare di utilizzare Azure AMF come provider di autenticazione principale.

-   Per impostare AMF Azure come metodo di autenticazione principale è necessario disporre di Azure Active Directory.

-   Impostazione di Azure AMF come metodo di autenticazione principale avviene tramite connessione AD Azure.  È necessario utilizzare uno dei metodi descritti di seguito riportato di seguito per installare e configurare il

-   Questa funzionalità non è disponibile in modalità mista.  È l'annuncio intera farm FS deve essere aWindows Server 2016

## <a name="using-azure-ad-connect-to-setup-azure-mfa-as-primary-authentication-provider"></a>Per impostare i AMF Azure come provider di autenticazione principale utilizza Connetti AD Azure
Poiché utilizza AMF Azure come scenario ibrido è il provider di autenticazione principale, è possibile configurare e l'installazione utilizzando la procedura guidata Connetti AD Azure.  Seconda che si sono appena impostarlo o già disporre di Azure Connect di Active Directory è possibile comunque utilizzarlo per impostare e configurare ADFS per utilizzare AMF Azure.  Utilizzare le procedure riportate di seguito

### <a name="setting-up-azure-mfa-with-azure-ad-connect---new-installation-of-azure-ad-connect-and-ad-fs"></a>Impostazione di Azure AMF con Azure Connect di Active Directory - Connect di nuova installazione di Azure Active Directory e AD FS
Se si tratta di una nuova installazione, simplyIf, si tratta di una nuova installazione, è sufficiente seguire le istruzioni per l'impostazione di Azure federazione ADFS e di connessione di Active Directory che si trova [qui](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-get-started-custom/).   Quando si arriva alla schermata Opzioni di revisione, assicurarsi che sia selezionata configurare AMF Azure per ADFS e fare clic su conferma.   AMF Azure come provider di autenticazione principale verrà impostato automaticamente su automatically.When si arriva alla schermata Opzioni di revisione, assicurarsi che **configurare AMF Azure per ADFS** è selezionata e fare clic su **conferma**.   AMF Azure come provider di autenticazione principale verrà impostato automaticamente verso l'alto.

![ADFS e AMF](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_2.png)

### <a name="setting-up-azure-mfa-with-azure-ad-connect---new-installation-of-azure-ad-connect-and-existing-ad-fs"></a>Impostazione di Azure AMF con Azure Connect Active Directory - nuova installazione di Azure AD Connect e ADFS esistente
Se si dispone già dell'installazione di ADFS e sono ora ad installare Azure Connect di Active Directory, è necessario seguire le istruzioni per l'impostazione di Azure federazione ADFS e di connessione di Active Directory che si trova [qui](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-get-started-custom/) e assicurarsi di selezionare una farm di server ADFS esistente.   Quando si arriva alla schermata Opzioni di revisione, assicurarsi che **configurare AMF Azure per ADFS** è selezionata e fare clic su **conferma.** AMF Azure come provider di autenticazione principale verrà impostato automaticamente verso l'alto.  .

![ADFS e AMF](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_3.png)

### <a name="setting-up-azure-mfa-with-azure-ad-connect---existing-azure-ad-connect-and-existing-ad-fs"></a>Impostazione di Azure AMF con Azure Connect di Active Directory - Connetti AD Azure esistente e ADFS esistente
Se è già stato installato Azure Connect di Active Directory e AD FS è quindi possibile semplicemente eseguire la procedura guidata Connetti AD Azure nuovamente e le attività aggiuntive selezionare **configurare AMF Azure per la farm di ADFS** e seguire quindi completato l'esecuzione della procedura guidata.

![ADFS e AMF](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_4.png)

## <a name="setting-up-azure-mfa-with-server-manager-in-windows-server-2016"></a>Impostazione di Azure AMF con Server Manager in Windows Server 2016


