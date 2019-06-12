---
title: Creare profili VPNv2 basati su OMA-DM in dispositivi Windows 10
description: 'È possibile usare uno dei due metodi per creare OMA-DM basati VPNv2 profili. '
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 9051a5b4dc8055885bdc1f8f727514b6e049d74d
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749510"
---
# <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devices"></a>Passaggio 7.5. Creare OMA-DM basati su profili VPNv2 per i dispositivi Windows 10

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Passaggio 7.4. Distribuire i certificati radice di accesso condizionale ai servizi locali Active Directory](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)
- [**prossimo:** Informazioni su come l'accesso condizionale per VPN works](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

In questo passaggio, è possibile creare OMA-DM basati su profili VPNv2 usando Intune per distribuire un criterio di configurazione del dispositivo VPN. Se si desidera usare script di SCCM o PowerShell per creare i profili di VPNv2, vedere [impostazioni VPNv2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) per altri dettagli. 

## <a name="managed-deployment-using-intune"></a>Distribuzione gestita con Intune

Tutte le operazioni descritte in questa sezione è il requisito minimo necessario per usare VPN con l'accesso condizionale. Non copre lo Split Tunneling, WIP, tramite la creazione di profili di configurazione dispositivo Intune personalizzati per ottenere AutoVPN utilizzo o SSO. Integrare le impostazioni sotto il profilo VPN è creata in precedenza in [passaggio 5. Configurare sempre i Client Windows 10 su connessioni VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md).  In questo esempio, si sta integrando nel [configurare il client VPN usando Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune) criteri. 

**Prerequisito:**

Computer client Windows 10 è già stato configurato con una connessione VPN usando Intune.   


**Procedura:**

1. Nel portale di Azure, selezionare **Intune** > **configurazione del dispositivo** > **profili** e selezionare il profilo VPN è stato creato in precedenza in [ Configurare il client VPN usando Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune).
    
2. Nell'editor dei criteri, selezionare **delle proprietà** > **impostazioni** > **VPN di Base**. Estendere l'oggetto esistente **Xml EAP** per includere un filtro che consente al client VPN la logica necessaria per recuperare il certificato di accesso condizionale di AAD dall'archivio certificati dell'utente anziché lasciarlo a opportunità consentendo di utilizzare il primo certificato individuato.

    >[!NOTE]
    >In caso contrario, il client VPN è stato possibile recuperare il certificato utente emesso dall'autorità di certificazione in locale, risultante in una connessione VPN non riuscita.

    ![Portale di Intune](../../media/Always-On-Vpn/intune-eap-xml.png)

3. Individuare la sezione che termina con  **\</AcceptServerName >\</EapType >** e inserire la stringa seguente tra i due valori da fornire al client VPN con la logica per selezionare il condizionale di AAD Certificato di accesso:

    ```XML
    <TLSExtensions xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. Selezionare il **accesso condizionale** pannello e toogle **accesso condizionale per questa connessione VPN** al **abilitato**.
   
   Se si abilita questa impostazione modifiche il  **\<DeviceCompliance >\<abilitato > true\<o abilitati >** impostazione nel file XML del profilo VPNv2.

    ![Accesso condizionale per VPN - proprietà Always On](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

5. Scegliere **OK**.

6. Selezionare **assegnazioni**, nel gruppo di inclusione, selezionare **selezionare i gruppi da includere**.

7. Selezionare il **gli utenti VPN** gruppo che riceve questo criterio e selezionare **salvare**.

    ![Limite di utilizzo per gli utenti VPN automatico - assegnazioni](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## <a name="force-mdm-policy-sync-on-the-client"></a>Forzare la sincronizzazione dei criteri MDM sul Client

Se il profilo VPN non viene visualizzato nel dispositivo client, in impostazioni\\rete e Internet\\VPN, è possibile imporre criteri MDM per la sincronizzazione.

1. Accedi a un computer client aggiunti al dominio come membro del **gli utenti VPN** gruppo.

2. Nel menu Start, immettere **account**, quindi premere INVIO.

3. Nel riquadro di spostamento a sinistra, selezionare **accesso azienda o dell'istituto di istruzione**.

4. In accesso aziendale o dell'istituto di istruzione, selezionare **connesso a < \domain > MDM**, quindi selezionare **Info**.

5. Selezionare **Sync** e verificare il profilo VPN viene visualizzato in impostazioni\\rete e Internet\\VPN.


## <a name="next-steps"></a>Passaggi successivi

Aver completato la configurazione del profilo VPN per usare l'accesso condizionale di Azure AD. 

|Se si vuole...  |Vedere quindi...  |
|---------|---------|
|Altre informazioni sul funzionamento dell'accesso condizionale con connessioni VPN  |[VPN e l'accesso condizionale](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Questa pagina fornisce altre informazioni sull'accesso condizionale funziona con connessioni VPN.      |
|Altre informazioni sulle funzionalità di VPN avanzate  |[Funzionalità VPN avanzate](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features): Questa pagina fornisce istruzioni su come abilitare i filtri traffico VPN, come configurare connessioni VPN automatiche tramite i trigger di App e come configurare criteri di rete per consentire solo le connessioni VPN dal client che usano i certificati rilasciati da Azure AD.        |


## <a name="related-topics"></a>Argomenti correlati

- [VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp):  In questo argomento offre una panoramica di VPNv2 CSP. Il provider di servizi di configurazione VPNv2 consente al server di gestione (MDM) dei dispositivi mobili configurare il profilo VPN del dispositivo.

- [Configurare sempre i Client Windows 10 su connessioni VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): Questo argomento vengono fornite informazioni sulle opzioni ProfileXML e dello schema e su come creare la VPN ProfileXML. Dopo aver configurato l'infrastruttura di server, è necessario configurare i computer client Windows 10 per comunicare con tale infrastruttura con una connessione VPN. 

- [Configurare il client VPN usando Intune](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections#configure-the-vpn-client-by-using-intune): In questo argomento vengono fornite informazioni su come distribuire i profili di Windows 10 Remote Access VPN Always On. Intune Usa ora i gruppi di Azure AD. Se Azure AD Connect sincronizzato il gruppo di utenti VPN da locale ad Azure AD, non è necessario per la configurazione del client VPN usando Intune.
