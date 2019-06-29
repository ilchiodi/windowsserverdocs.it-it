---
title: Distribuire i certificati radice di accesso condizionale ad Active Directory locale
description: ''
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 200d3b96ee24b5e1264b4bf2e42d636f9e07fbef
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469675"
---
# <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>Passaggio 7.4. Distribuire i certificati radice di accesso condizionale ai servizi locali Active Directory

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

In questo passaggio si distribuisce il certificato radice di accesso condizionale come certificato radice attendibile per l'autenticazione VPN per on-premises AD.

- [**Precedente:** Passaggio 7.3. Configurare i criteri di accesso condizionale](vpn-config-conditional-access-policy.md)
- [**prossimo:** Passaggio 7.5. Creare profili VPNv2 basati su OMA-DM in dispositivi Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. Nel **connettività VPN** pagina, selezionare **Scarica certificato**.

   >[!NOTE]
   >Il **Scarica certificato base64** opzione è disponibile per alcune configurazioni che richiedono certificati base64 per la distribuzione.

2. Accedere a un computer aggiunto al dominio con diritti di amministratore dell'organizzazione ed eseguire questi comandi da un prompt dei comandi di amministratore per aggiungere cloud radice di uno o più certificati nel *Enterprise NTauth* archiviare:

   >[!NOTE]
   >Per gli ambienti in cui il server VPN non viene aggiunto al dominio di Active Directory, è necessario aggiungere i certificati radice cloud per la _autorità di certificazione radice attendibili_ archiviare manualmente.

   | Comando | Descrizione |
   | --- | --- |
   | `certutil -dspublish -f VpnCert.cer RootCA` | Vengono creati due **radice VPN Microsoft generazione CA 1** contenitori sotto il **CN = AIA** e **CN = autorità di certificazione** contenitori e pubblica ogni certificato radice come un valore in il _cACertificate_ attributo entrambi **radice VPN Microsoft generazione CA 1** contenitori. |
   | `certutil -dspublish -f VpnCert.cer NTAuthCA` | Crea uno **CN = NTAuthCertificates** contenitore all'interno di **CN = AIA** e **CN = autorità di certificazione** contenitori e lo pubblica di ogni certificato radice come un valore in il _cACertificate_ attributo il **CN = NTAuthCertificates** contenitore. |
   | `gpupdate /force` | Consente di accelerare l'aggiunta di certificati radice per i computer client e server Windows. |

3. Verificare che siano presenti nell'archivio NTauth Enterprise e vengono indicate come attendibili i certificati radice:
   1. Accedere a un server con diritti di amministratore dell'organizzazione che ha il **gli strumenti di gestione di certificati dell'autorità** installato.

   >[!NOTE]
   >Per impostazione predefinita il **gli strumenti di gestione di certificati dell'autorità** sono installati i server di autorità di certificazione. Possono essere installati su altri server membri come parte del **strumenti di amministrazione ruoli** in Server Manager.

   1. Nel server VPN, nel menu Start, immettere **PKIView** per aprire la finestra di dialogo di infrastruttura a chiave pubblica dell'organizzazione.
   1. Dal menu Start, immettere **PKIView** per aprire la finestra di dialogo di infrastruttura a chiave pubblica dell'organizzazione.
   1. Fare doppio clic su **PKI aziendale** e selezionare **Gestisci AD contenitori**.
   1. Verificare che sia presente in ogni certificato di generazione 1 autorità di certificazione radice Microsoft VPN:
      - NTAuthCertificates
      - Contenitore AIA
      - Contenitore Autorità di certificati

## <a name="next-steps"></a>Passaggi successivi

[Passaggio 7.5. Creare OMA-DM basati su profili VPNv2 per i dispositivi Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md): In questo passaggio, è possibile creare OMA-DM basati su profili VPNv2 usando Intune per distribuire un criterio di configurazione del dispositivo VPN. Se vuole SCCM o Script di PowerShell per creare i profili di VPNv2, vedere [impostazioni VPNv2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) per altri dettagli.
