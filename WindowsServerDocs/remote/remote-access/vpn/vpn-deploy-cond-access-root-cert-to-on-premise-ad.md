---
title: Distribuire i certificati radice di accesso condizionale ad Active Directory locale
description: ''
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 210540846f5d62dfc74a2e629a6b7675ccf9894d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837372"
---
# <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>Passaggio 7.4. Distribuire i certificati radice di accesso condizionale ai servizi locali Active Directory

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

In questo passaggio si distribuisce il certificato radice di accesso condizionale come certificato radice attendibile per l'autenticazione VPN per on-premises AD.

&#171;  [**Precedente:** Passaggio 7.3. Configurare i criteri di accesso condizionale](vpn-config-conditional-access-policy.md)<br>
&#187; [ **Next:** Passaggio 7.5. Creare OMA-DM basati su profili VPNv2 per i dispositivi Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. Nel **connettività VPN** pagina, fare clic su **Scarica certificato**. 
   
    ![Scaricare il certificato per l'accesso condizionale](../../media/Always-On-Vpn/06.png)

    >[!NOTE]
    >Il **Scarica certificato base64** opzione è disponibile per alcune configurazioni che richiedono certificati base64 per la distribuzione. 

2. Accedere a un computer aggiunto al dominio con diritti di amministratore dell'organizzazione ed eseguire questi comandi da un prompt dei comandi di amministratore per aggiungere cloud radice di uno o più certificati nel *Enterprise NTauth* archiviare:

    >[!NOTE]
    >Per gli ambienti in cui il server VPN non viene aggiunto al dominio di Active Directory, è necessario aggiungere i certificati radice cloud per la _autorità di certificazione radice attendibili_ archiviare manualmente.

    |Comando  |Descrizione  |  
    |---------|-------------| 
    |`certutil -dspublish -f VpnCert.cer RootCA`     |Vengono creati due **radice VPN Microsoft generazione CA 1** contenitori sotto il **CN = AIA** e **CN = autorità di certificazione** contenitori e pubblica ogni certificato radice come un valore in il _cACertificate_ attributo entrambi **radice VPN Microsoft generazione CA 1** contenitori.|  
    |`certutil -dspublish -f VpnCert.cer NTAuthCA`   |Crea uno **CN = NTAuthCertificates** contenitore all'interno di **CN = AIA** e **CN = autorità di certificazione** contenitori e lo pubblica di ogni certificato radice come un valore in il _cACertificate_ attributo il **CN = NTAuthCertificates** contenitore. |  
    |`gpupdate /force`     |Consente di accelerare l'aggiunta di certificati radice per i computer client e server Windows.  |

3.  Verificare che siano presenti nell'archivio NTauth Enterprise e vengono indicate come attendibili i certificati radice:

    a.  Accedere a un server con diritti di amministratore dell'organizzazione che ha il **gli strumenti di gestione di certificati dell'autorità** installato.

    >[!NOTE]
    >Per impostazione predefinita il **gli strumenti di gestione di certificati dell'autorità** sono installati i server di autorità di certificazione. Possono essere installati su altri server membri come parte del **strumenti di amministrazione ruoli** in Server Manager.

    b.  Nel server VPN, nel menu Start, digitare **PKIView** per aprire la finestra di dialogo di infrastruttura a chiave pubblica dell'organizzazione.

    c.  Dal menu Start, digitare **PKIView** per aprire la finestra di dialogo di infrastruttura a chiave pubblica dell'organizzazione.

    d.  Fare doppio clic su **PKI aziendale** e selezionare **Gestisci AD contenitori**.

    d.  Verificare che sia presente in ogni certificato di generazione 1 autorità di certificazione radice Microsoft VPN:<ul><li>NTAuthCertificates</li><li>Contenitore AIA</li><li>Contenitore Autorità di certificati</li></ul>

    
## <a name="next-step"></a>Passaggio successivo
[Passaggio 7.5. Creare OMA-DM basati su profili VPNv2 per i dispositivi Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md): In questo passaggio, è possibile creare OMA-DM basati su profili VPNv2 usando Intune per distribuire un criterio di configurazione del dispositivo VPN. Se vuole SCCM o Script di PowerShell per creare i profili di VPNv2, vedere [impostazioni VPNv2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) per altri dettagli.

---
