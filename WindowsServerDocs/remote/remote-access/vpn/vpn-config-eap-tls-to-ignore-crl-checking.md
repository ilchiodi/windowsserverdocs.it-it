---
title: Configurare EAP-TLS per ignorare il controllo dell'elenco di revoche di certificati (CRL)
description: Un client EAP-TLS non è in grado di connettersi a meno che il server dei criteri di rete non completi un controllo di revoca della catena di certificati (incluso il certificato radice) del client e verifica che i certificati siano stati revocati.
services: active-directory
ms.prod: windows-server
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: lizross
author: eross-msft
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: c58386467d632d77fdc96bf3bc18b5a85d1ecdaf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80307700"
---
# <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checking"></a>Passaggio 7.1. Configurare EAP-TLS per ignorare il controllo dell'elenco di revoche di certificati (CRL)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Passaggio 7. Opzionale Accesso condizionale per la connettività VPN con Azure AD](ad-ca-vpn-connectivity-windows10.md)
- [Passaggio **successivo:** Passaggio 7,2. Creare certificati radice per l'autenticazione VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>Se si verifica un errore durante l'implementazione di questa modifica del registro di sistema, le connessioni IKEv2 che usano i certificati cloud con PEAP avranno esito negativo, ma le connessioni IKEv2 che usano i certificati di autenticazione client emessi dalla CA locale continueranno a funzionare.

In questo passaggio è possibile aggiungere **IgnoreNoRevocationCheck** e impostarlo in modo da consentire l'autenticazione dei client quando il certificato non include i punti di distribuzione CRL. Per impostazione predefinita, IgnoreNoRevocationCheck è impostato su 0 (disabilitato).

>[!NOTE]
>Se un server di routing e accesso remoto (RRAS) di Windows utilizza il server dei criteri di gruppo per il proxy delle chiamate RADIUS a un secondo server dei criteri di gruppo, è necessario impostare **IgnoreNoRevocationCheck = 1** in entrambi i server.

Un client EAP-TLS non è in grado di connettersi a meno che il server dei criteri di rete non completi un controllo di revoca della catena di certificati, incluso il certificato radice. I certificati cloud rilasciati all'utente da Azure AD non dispongono di un CRL perché sono certificati di breve durata con una durata di un'ora. EAP in NPS deve essere configurato per ignorare l'assenza di un CRL. Per impostazione predefinita, IgnoreNoRevocationCheck è impostato su 0 (disabilitato). Aggiungere IgnoreNoRevocationCheck e impostarlo su 1 per consentire l'autenticazione dei client quando il certificato non include i punti di distribuzione CRL. 

Poiché il metodo di autenticazione è EAP-TLS, questo valore del registro di sistema è necessario solo in EAP\13. Se vengono usati altri metodi di autenticazione EAP, è necessario aggiungere anche il valore del registro di sistema. 

**Procedura**

1. Aprire **Regedit. exe** nel server NPS.

2. Passare a **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\rasman\ppp\eap\13**.

3. Selezionare **modifica > nuovo** e selezionare **valore DWORD (32-bit)** e immettere **IgnoreNoRevocationCheck**.

4. Fare doppio clic su **IgnoreNoRevocationCheck** e impostare i dati del valore su **1**.

5. Selezionare **OK** e riavviare il server. Il riavvio dei servizi RRAS e NPS non è sufficiente.

Per ulteriori informazioni, vedere [come abilitare o disabilitare il controllo delle revoche di certificati (CRL) sui client](https://technet.microsoft.com/library/bb680540.aspx).


|Percorso del Registro di sistema  |Estensione EAP  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-MSCHAP v2         |

## <a name="next-steps"></a>Passaggi successivi

[Passaggio 7,2. Creare certificati radice per l'autenticazione VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md): in questo passaggio vengono configurati i certificati radice di accesso condizionale per l'autenticazione VPN con Azure ad, che crea automaticamente un'app Cloud Server VPN nel tenant.
