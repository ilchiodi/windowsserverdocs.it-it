---
title: Creare profili VPNv2 basati su OMA-DM in dispositivi Windows 10
description: 'È possibile usare uno dei due metodi seguenti per creare profili VPNv2 basati su OMA-DM. '
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 07/13/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: b1316fe2feba674beb915b6ea22b1c0361ae1243
ms.sourcegitcommit: 7116460855701eed4e09d615693efa4fffc40006
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2020
ms.locfileid: "83433155"
---
# <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devices"></a>Passaggio 7.5. Creare profili VPNv2 basati su OMA-DM per i dispositivi Windows 10

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Passaggio 7,4. Distribuire i certificati radice di accesso condizionale ad Active Directory locale](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)
- [Passaggio **successivo:** Informazioni sul funzionamento dell'accesso condizionale per VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

In questo passaggio è possibile creare profili VPNv2 basati su OMA usando Intune per distribuire i criteri di configurazione del dispositivo VPN. Se si vuole usare Microsoft endpoint Configuration Manager o uno script di PowerShell per creare profili VPNv2, vedere [le impostazioni di VPNV2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) per altri dettagli. 

## <a name="managed-deployment-using-intune"></a>Distribuzione gestita con Intune

Tutti gli elementi illustrati in questa sezione sono il requisito minimo necessario per eseguire la VPN con l'accesso condizionale. Non viene trattata la suddivisione del tunneling, usando WIP, creando profili di configurazione dei dispositivi Intune personalizzati per ottenere il funzionamento di AutoVPN o SSO. Integrare le impostazioni seguenti nel profilo VPN creato in precedenza nel [passaggio 5. Configurare il client Windows 10 Always On le connessioni VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md).In questo esempio, i criteri vengono integrati nel [configurare il client VPN usando i criteri di Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune) . 

**Prerequisiti**

Il computer client Windows 10 è già stato configurato con una connessione VPN con Intune.   


**Procedura**

1. Nella portale di Azure selezionare profili di configurazione dei dispositivi di **Intune**  >  **Device Configuration**  >  **Profiles** e selezionare il profilo VPN creato in precedenza in [configurare il client VPN con Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune).
    
2. Nell'editor dei criteri selezionare **Proprietà**  >  **Impostazioni**  >  **VPN di base**. Estendere il **file XML EAP** esistente per includere un filtro che fornisce al client VPN la logica necessaria per recuperare il certificato di accesso condizionale di AAD dall'archivio certificati dell'utente, invece di lasciarlo alla possibilità di usare il primo certificato individuato.

    >[!NOTE]
    >Senza questo, il client VPN potrebbe recuperare il certificato utente emesso dall'autorità di certificazione locale, causando una connessione VPN non riuscita.

    ![Portale di Intune](../../media/Always-On-Vpn/intune-eap-xml.png)

3. Individuare la sezione che termina con ** \< /AcceptServerName>\< /EapType>** e inserire la stringa seguente tra questi due valori per fornire al client VPN la logica per selezionare il certificato di accesso condizionale di AAD:

    ```XML
    <TLSExtensions xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. Selezionare il pannello **accesso condizionale** e l' **accesso condizionale Toogle per la connessione VPN** **abilitata**.
   
   L'abilitazione di questa impostazione modifica l'impostazione di ** \< DeviceCompliance>\< abilitata>true \< /Enabled>** nel file XML del profilo VPNv2.

    ![Accesso condizionale per Always On VPN-proprietà](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

5. Seleziona **OK**.

6. Selezionare **assegnazioni**, in Includi, selezionare **Seleziona gruppi da includere**.

7. Selezionare il gruppo **utenti VPN** che riceve questo criterio e selezionare **Salva**.

    ![LIMITE per utenti VPN automatici-assegnazioni](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## <a name="force-mdm-policy-sync-on-the-client"></a>Forza la sincronizzazione dei criteri MDM sul client

Se il profilo VPN non viene visualizzato nel dispositivo client, in impostazioni \\ rete & \\ VPN Internet è possibile forzare la sincronizzazione dei criteri MDM.

1. Accedere a un computer client aggiunto al dominio come membro del gruppo **utenti VPN** .

2. Nel menu Start immettere **account**e premere INVIO.

3. Nel riquadro di spostamento a sinistra selezionare **Accedi all'ufficio o all'Istituto di istruzione**.

4. In Accedi all'ufficio o all'Istituto **di istruzione selezionare connesso a < \domain> MDM**e quindi selezionare **info**.

5. Selezionare **Sincronizza** e verificare che il profilo VPN sia visualizzato in impostazioni \\ rete & \\ VPN Internet.


## <a name="next-steps"></a>Passaggi successivi

La configurazione del profilo VPN per l'uso di Azure AD l'accesso condizionale è stata eseguita. 

|Per...  |Vedere...  |
|---------|---------|
|Altre informazioni sul funzionamento dell'accesso condizionale con VPN  |[VPN e accesso condizionale](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Questa pagina fornisce altre informazioni sul funzionamento dell'accesso condizionale con le VPN.      |
|Altre informazioni sulle funzionalità VPN avanzate  |[Funzionalità VPN avanzate](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features): Questa pagina fornisce indicazioni su come abilitare i filtri del traffico VPN, come configurare le connessioni VPN automatiche usando i trigger di app e come configurare NPS per consentire solo le connessioni VPN dai client che usano i certificati rilasciati da Azure ad.        |


## <a name="related-topics"></a>Argomenti correlati

- [VPNV2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp): in questo argomento viene fornita una panoramica di VPNv2 CSP. Il provider di servizi di configurazione VPNv2 consente al server di gestione di dispositivi mobili (MDM) di configurare il profilo VPN del dispositivo.

- [Configurare il client Windows 10 always on connessioni VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): in questo argomento vengono fornite informazioni sulle opzioni e sullo schema di ProfileXML e su come creare la VPN ProfileXML. Dopo aver configurato l'infrastruttura server di, è necessario configurare i computer client Windows 10 in modo che comunicano con tale infrastruttura con una connessione VPN. 

- [Configurare il client VPN con Intune](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections#configure-the-vpn-client-by-using-intune): questo argomento fornisce informazioni su come distribuire accesso remoto Windows 10 always on profili VPN. Intune USA ora i gruppi di Azure AD. Se Azure AD Connect sincronizzato il gruppo di utenti VPN da locale a Azure AD, non è necessario configurare il client VPN con Intune.
