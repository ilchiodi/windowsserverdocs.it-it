---
title: Distribuire i certificati radice di accesso condizionale ad Active Directory locale
description: ''
services: active-directory
ms.prod: windows-server
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: lizross
author: eross-msft
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: aeb35d8c3eafd436330b66ca7d5be68d78045df0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80307843"
---
# <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>Passaggio 7.4. Distribuire i certificati radice di accesso condizionale ad Active Directory locale

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

In questo passaggio si distribuisce il certificato radice di accesso condizionale come certificato radice attendibile per l'autenticazione VPN in Active Directory locale.

- [**Precedente:** Passaggio 7,3. Configurare i criteri di accesso condizionale](vpn-config-conditional-access-policy.md)
- [Passaggio **successivo:** Passaggio 7,5. Creare profili VPNv2 basati su OMA-DM per i dispositivi Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. Nella pagina **connettività VPN** selezionare **Scarica certificato**.

   >[!NOTE]
   >L'opzione **Scarica certificato Base64** è disponibile per alcune configurazioni che richiedono certificati Base64 per la distribuzione.

2. Accedere a un computer aggiunto a un dominio con diritti di amministratore dell'organizzazione ed eseguire questi comandi da un prompt dei comandi dell'amministratore per aggiungere i certificati radice cloud nell'archivio *NTauth aziendale* :

   >[!NOTE]
   >Per gli ambienti in cui il server VPN non è aggiunto al dominio di Active Directory, i certificati radice cloud devono essere aggiunti manualmente all'archivio _autorità di certificazione radice attendibili_ .

   | Comando | Descrizione |
   | --- | --- |
   | `certutil -dspublish -f VpnCert.cer RootCA` | Crea due **contenitori CA gen 1 della radice VPN Microsoft** nei contenitori **CN = Aia** e **CN = Certification authoritys** e pubblica ogni certificato radice come valore dell'attributo _Cacertificate_ di entrambi i contenitori **CA della radice VPN Microsoft** . |
   | `certutil -dspublish -f VpnCert.cer NTAuthCA` | Crea un contenitore **CN = NTAuthCertificates** nei contenitori **CN = Aia** e **CN = Certification authoritys** e pubblica ogni certificato radice come valore dell'attributo _Cacertificate_ del contenitore **CN = NTAuthCertificates** . |
   | `gpupdate /force` | Velocizza l'aggiunta dei certificati radice ai computer Windows Server e client. |

3. Verificare che i certificati radice siano presenti nell'archivio NTauth aziendale e mostrare come attendibile:
   1. Accedere a un server con diritti di amministratore dell'organizzazione per i quali sono installati gli **strumenti di gestione dell'autorità di certificazione** .

   >[!NOTE]
   >Per impostazione predefinita, gli **strumenti di gestione dell'autorità di certificazione** sono server di autorità di certificazione installati. Possono essere installati in altri server membri come parte degli strumenti di **amministrazione ruoli** in Server Manager.

   1. Nel server VPN, nel menu Start, immettere **PKIView. msc** per aprire la finestra di dialogo infrastruttura a chiave pubblica (PKI) aziendale.
   1. Dal menu Start immettere **PKIView. msc** per aprire la finestra di dialogo infrastruttura a chiave pubblica (PKI) aziendale.
   1. Fare clic con il pulsante destro del **Manage AD Containers**mouse su **infrastruttura a chiave pubblica (PKI) Enterprise**
   1. Verificare che sia presente il certificato CA gen 1 radice VPN Microsoft in:
      - NTAuthCertificates
      - Contenitore AIA
      - Contenitore autorità di certificazione

## <a name="next-steps"></a>Passaggi successivi

[Passaggio 7,5. Creare profili VPNv2 basati su OMA-DM per i dispositivi Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md): in questo passaggio è possibile creare profili VPNv2 basati su OMA-DM usando Intune per distribuire i criteri di configurazione del dispositivo VPN. Se si vuole usare Microsoft endpoint Configuration Manager o uno script di PowerShell per creare profili VPNv2, vedere [le impostazioni di VPNV2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) per altri dettagli.
