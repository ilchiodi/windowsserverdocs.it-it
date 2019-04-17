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
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067309"
---
# Passaggio 7.4. Distribuire certificati radice di accesso condizionale locale AD

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

In questo passaggio, distribuire il certificato radice di accesso condizionale come certificato radice attendibile per l'autenticazione VPN per locale AD.

& #171;  [ **Precedente:** 7.3 passaggio. Configura il criterio di accesso condizionale](vpn-config-conditional-access-policy.md)<br>
& #187; [ **Successivo:** passaggio 7.5. Creare OMA-DM VPNv2 profili in base ai dispositivi Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. Nella pagina di **connettività VPN** , fai clic su **Scarica certificato**. 
   
    ![Scaricare i certificati per l'accesso condizionale](../../media/Always-On-Vpn/06.png)

    >[!NOTE]
    >La possibilità di **scaricare il certificato con codifica base64** è disponibile per alcune configurazioni che richiedono i certificati con codifica base64 per la distribuzione. 

2. Accedere a un computer appartenenti al dominio con diritti di amministratore dell'organizzazione ed Esegui i comandi seguenti da un prompt dei comandi di amministratore per aggiungere i certificati radice cloud nell'archivio *NTauth dell'organizzazione* :

    >[!NOTE]
    >Per gli ambienti in cui il server VPN non viene aggiunto al dominio di Active Directory, i certificati radice cloud devono essere aggiunti all'archivio _Autorità di certificazione radice attendibili_ manualmente.

    |Comando  |Descrizione  |  
    |---------|-------------| 
    |`certutil -dspublish -f VpnCert.cer RootCA`     |Crea due contenitori **Microsoft VPN radice generazione CA 1** con la **CN = AIA** e **CN = autorità di certificazione** contenitori e pubblica ogni certificato radice come valore nell'attributo _cACertificate_ della radice entrambi **Microsoft VPN Generazione di autorità di certificazione 1** i contenitori.|  
    |`certutil -dspublish -f VpnCert.cer NTAuthCA`   |Crea uno **CN = NTAuthCertificates** contenitore sotto il **CN = AIA** e **CN = autorità di certificazione** contenitori e pubblica ogni certificato radice come valore nell'attributo _cACertificate_ di CN il **= NTAuthCertificates** contenitore. |  
    |`gpupdate /force`     |Consente di accelerare aggiungendo i certificati radice per Windows server e i computer client.  |

3.  Verifica che siano presenti nell'archivio NTauth dell'organizzazione e Mostra come attendibili i certificati radice:

    a.  Accedi a un server con diritti di amministratore dell'organizzazione che ha installato **Gli strumenti di gestione di autorità di certificazione** .

    >[!NOTE]
    >Per impostazione predefinita, gli **Strumenti di gestione di autorità di certificazione** sono installati i server di autorità di certificazione. Può essere installati su altri server membri come parte degli **Strumenti di amministrazione ruoli** in Server Manager.

    b.  Nel server VPN, nel menu Start, digita **PKIView. msc** per aprire la finestra di dialogo di infrastruttura a chiave pubblica dell'organizzazione.

    c.  Dal menu Start, digita **PKIView. msc** per aprire la finestra di dialogo di infrastruttura a chiave pubblica dell'organizzazione.

    d.  **Infrastruttura a chiave pubblica dell'organizzazione** di mouse e selezionare **Gestisci AD contenitori**.

    d.  Verifica che ogni certificato di generazione 1 Microsoft VPN CA radice è presente sotto:<ul><li>NTAuthCertificates</li><li>Contenitore AIA</li><li>Contenitore di autorità di certificazione</li></ul>

    
## Passaggio successivo
Passaggio [7.5. Creare OMA-DM VPNv2 profili in base ai dispositivi Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md): In questo passaggio, è possibile creare OMA-DM in base i profili VPNv2 tramite Intune per distribuire un criterio di configurazione dei dispositivi VPN. Se si desidera SCCM o Script di PowerShell per creare i profili VPNv2, vedere [le impostazioni CSP VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) per altri dettagli.

---
