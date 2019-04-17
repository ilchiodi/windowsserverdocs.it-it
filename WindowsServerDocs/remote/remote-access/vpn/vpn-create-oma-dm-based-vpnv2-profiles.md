---
title: Creare profili VPNv2 basati su OMA-DM in dispositivi Windows 10
description: 'È possibile utilizzare uno dei due metodi per creare OMA-DM in base i profili VPNv2. '
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
ms.openlocfilehash: 1ce20d09c304b26e3708429cc45da06d020e5809
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031295"
---
# Passaggio 7.5. Creare OMA-DM in base i profili VPNv2 per i dispositivi Windows 10

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Precedente:** passaggio 7.4. Distribuire i certificati radice di accesso condizionale locale AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)<br>
& #187; [ **Successivo:** informazioni di accesso condizionale come per la VPN funziona](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

In questo passaggio, è possibile creare OMA-DM in base i profili VPNv2 con Intune per distribuire un criterio di configurazione dei dispositivi VPN. Se vuoi usare script SCCM o PowerShell per creare profili VPNv2, vedi [le impostazioni CSP VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) per altri dettagli. 

## Distribuzione gestita tramite Intune

Tutti gli elementi descritti in questa sezione è il requisito minimo necessario per far VPN funzionare con accesso condizionale. Non copre Split Tunneling, con Windows Information Protection, la creazione di profili di configurazione dispositivo Intune personalizzati per ottenere AutoVPN lavorando o SSO. Integrare le impostazioni di sotto il profilo VPN creati in precedenza nel passaggio 5 [. Configurare i Client Windows 10 sempre per le connessioni VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md).In questo esempio, abbiamo stiamo la loro integrazione nel criterio [Configura il client VPN tramite Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune) . 

**Prerequisito:**<p>
Computer client Windows 10 è già stato configurato con una connessione VPN con Intune.   


**Procedura:**

1. Nel portale di Azure, fai clic su **Intune** > **Configurazione dei dispositivi** > **profili** e seleziona il profilo VPN è stato creato in precedenza in [Configura il client VPN tramite Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune).
    
2. Nell'editor dei criteri, scegli **proprietà** > **Impostazioni** > **VPN di Base**. Estendere l' **Xml EAP** esistenti per includere un filtro che fornisce la logica che ha bisogno di recuperare il certificato di accesso condizionale AAD dall'archivio dei certificati dell'utente anziché lasciandolo di probabilità in modo che il certificato primo di usare il client VPN individuati.

    >[!NOTE]
    >In caso contrario, il client VPN può recuperare il certificato utente emesso dall'autorità di certificazione in locale, generando una connessione VPN non riuscita.

    ![Portale di Intune](../../media/Always-On-Vpn/intune-eap-xml.png)

3. Individua la sezione che termina con **\</AcceptServerName>\</EapType>** e inserire la stringa seguente tra queste due valori per fornire il client VPN con la logica per selezionare il certificato di accesso condizionale di AAD:

    ```XML
    <TLSExtensions xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. Selezionare il pannello di **Accesso condizionale** e toogle **accesso condizionale per la connessione VPN** **abilitato**.<p>L'abilitazione di questa impostazione viene sostituito il **\<DeviceCompliance>\<Enabled>true\</Enabled>** impostazione nel file XML del profilo VPNv2.

    ![Accesso condizionale per la VPN - proprietà Always On](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

6. Scegli **OK**.

6. Selezionare **le assegnazioni di**gruppo Includi, fare clic su **Seleziona gruppi da includere**.

7. Seleziona il gruppo di **Utenti VPN** che riceve questo criterio e fare clic su **Salva**.

    ![LIMITE per gli utenti della VPN Auto - le assegnazioni](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## Forzare la sincronizzazione dei criteri MDM nel Client
Se il profilo VPN non viene visualizzato nel dispositivo client, in Settings\\Network & Internet\\VPN, è possibile forzare il criterio MDM per la sincronizzazione.

1. Accedi a un computer client appartenenti al dominio in qualità di membro del gruppo di **Utenti VPN** .

2. Nel menu Start, digita **l'account**e premi INVIO.

3.  Nel riquadro di spostamento a sinistra fai clic su **Accedi all'azienda o all'istituto di istruzione**.

5.  In Accedi all'azienda o all'istituto di istruzione, fai clic su **connesso a <\domain> MDM** e fai clic su **Info**.

6.  Fai clic su **sincronizzazione** e verificare che il profilo VPN venga visualizzato sotto Settings\\Network & Internet\\VPN.


## Passaggio successivo
Hai finito la configurazione del profilo VPN per l'uso di accesso condizionale di Azure AD. 

|Se si desidera...  |Vedi quindi...  |
|---------|---------|
|Altre informazioni su come condizionale funziona l'accesso con le reti VPN  |[VPN e accesso condizionale](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): questa pagina fornisce ulteriori informazioni sull'accesso condizionale come funziona con le reti VPN.      |
|Altre informazioni sulle funzionalità avanzate di VPN  |[Funzionalità avanzate di VPN](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features): questa pagina fornisce indicazioni su come abilitare filtri del traffico VPN, come configurare le connessioni VPN automatica mediante trigger di App e come configurare dei criteri di rete per consentire solo le connessioni VPN da client che utilizzano i certificati rilasciati da Azure ACTIVE DIRECTORY.        |


---

## Argomenti correlati
- [CSP VPNv2](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp): in questo argomento offre una panoramica del CSP VPNv2. Il provider di servizi di configurazione VPNv2 consente al server di gestione dispositivi mobili configurare il profilo VPN del dispositivo.

- [Configurare Windows 10 Client sempre su connessioni VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): in questo argomento fornisce informazioni sulle opzioni ProfileXML e lo schema e come creare la VPN ProfileXML. Dopo aver configurato l'infrastruttura del server, è necessario configurare i computer client Windows 10 per comunicare con tale infrastruttura con una connessione VPN. 

- [Configurare il client VPN tramite Intune](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections#configure-the-vpn-client-by-using-intune): in questo argomento fornisce informazioni su come distribuire i profili di Windows 10 remoto accesso VPN Always On. A questo punto, Intune Usa i gruppi di Azure AD. Se Azure AD Connect sincronizzato con il gruppo di utenti VPN da locale ad Azure AD, quindi non è necessario per la configurazione del client VPN con Intune.

---
