---
title: Configurare EAP-TLS per ignorare il controllo dell'elenco di revoche di certificati (CRL)
description: Un client di EAP-TLS non può connettersi, a meno che il server NPS esegue una verifica delle revoche di certificati della catena di certificati (incluso il certificato radice) del client e verifica che i certificati sono stati revocati.
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
ms.openlocfilehash: 781239f45b9b260b7d374c2a6972cdb8faad2879
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749598"
---
# <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checking"></a>Passaggio 7.1. Configurare EAP-TLS per ignorare il controllo dell'elenco di revoche di certificati (CRL)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Passaggio 7. (Facoltativo) Accesso condizionale per la connettività VPN con Azure AD](ad-ca-vpn-connectivity-windows10.md)
- [**prossimo:** Passaggio 7.2. Creare certificati radice per l'autenticazione VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>Se non si implementa questa modifica del Registro di sistema causerà connessioni IKEv2, uso di certificati di cloud con PEAP per avere esito negativo, ma le connessioni IKEv2 usando i certificati di autenticazione Client emessi dall'autorità di certificazione locale continuerà a funzionare.

In questo passaggio, è possibile aggiungere **IgnoreNoRevocationCheck** e impostarlo per consentire l'autenticazione dei client quando il certificato non include i punti di distribuzione CRL. Per impostazione predefinita, IgnoreNoRevocationCheck è impostata su 0 (disabilitato).

>[!NOTE]
>Se un Server accesso remoto (RRAS) e Windows Routing utilizza criteri di rete al proxy RADIUS le chiamate a un secondo server NPS, quindi è necessario impostare **IgnoreNoRevocationCheck = 1** in entrambi i server.

Non può connettersi un client di EAP-TLS, a meno che il server dei criteri di rete ha completato un controllo delle revoche della catena di certificati (incluso il certificato radice). Certificati cloud rilasciati all'utente da Azure AD non è un elenco CRL perché sono i certificati di breve durati con una durata di un'ora. EAP in Criteri di rete deve essere configurato per ignorare l'assenza di un CRL. Per impostazione predefinita, IgnoreNoRevocationCheck è impostata su 0 (disabilitato). Aggiungere IgnoreNoRevocationCheck e impostarla su 1 per consentire l'autenticazione dei client quando il certificato non include i punti di distribuzione CRL. 

Poiché il metodo di autenticazione EAP-TLS, questo valore del Registro di sistema è necessario solo in EAP\13. Se vengono utilizzati altri metodi di autenticazione EAP, quindi il valore del Registro di sistema deve essere aggiunti nelle quelli anche. 

**Procedure**

1. Aprire **regedit.exe** nel server NPS.

2. Passare a **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13**.

3. Selezionare **Modifica > New** e selezionare **valore DWORD (32 bit)** e immettere **IgnoreNoRevocationCheck**.

4. Fare doppio clic su **IgnoreNoRevocationCheck** e impostare il valore **1**.

5. Selezionare **OK** e riavviare il server. Riavviare i servizi RRAS e dei criteri di rete non è sufficiente.

Per altre informazioni, vedere [come abilitare o disabilitare la verifica della revoca dei certificati (CRL) nei client](https://technet.microsoft.com/library/bb680540.aspx).


|Percorso del Registro di sistema  |Estensione EAP  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |Protocollo EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-MSCHAP v2         |

## <a name="next-steps"></a>Passaggi successivi

[Passaggio 7.2. Creare i certificati radice per l'autenticazione VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md): In questo passaggio si configura i certificati radice di accesso condizionale per l'autenticazione VPN con Azure AD, che crea automaticamente un'app cloud di Server VPN nel tenant.
