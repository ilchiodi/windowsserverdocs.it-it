---
title: Configurare EAP-TLS per ignorare il controllo dell'elenco di revoche di certificati (CRL)
description: Un client EAP-TLS non riesce a connettersi a meno che il server dei criteri di rete viene completato un controllo di revoca della catena di certificati (incluso il certificato radice) del client e verifica che i certificati sono stati revocati.
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
ms.openlocfilehash: ac59c554c69a6138a106a648c3fab3ed4fe05b7b
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067373"
---
# Passaggio 7.1. Configurare EAP-TLS per ignorare il controllo dell'elenco di revoche di certificati (CRL)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Precedente:** passaggio 7. (Facoltativo) Accesso condizionale per la connettività VPN con Azure AD](ad-ca-vpn-connectivity-windows10.md)<br>
& #187; [ **Successivo:** passaggio 7.2. Creare certificati radice per l'autenticazione VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>Per implementare questa modifica del Registro di sistema, avranno esito connessioni IKEv2 utilizzo dei certificati cloud con PEAP esito negativo, ma le connessioni IKEv2 usando i certificati di autenticazione Client rilasciati dall'autorità di certificazione locale continui a funzionare.

In questo passaggio, puoi aggiungere **IgnoreNoRevocationCheck** e impostarla per consentire l'autenticazione dei client quando il certificato non includere punti di distribuzione CRL. Per impostazione predefinita, IgnoreNoRevocationCheck è impostato su 0 (disabilitato).

>[!NOTE]
>Se un Server di accesso remoto (RRAS) e di Routing di Windows Usa dei criteri di rete per proxy RADIUS le chiamate a una seconda dei criteri di rete, allora è necessario impostare **IgnoreNoRevocationCheck = 1** su entrambi i server.

Un client EAP-TLS non riesce a connettersi a meno che il server dei criteri di rete viene completato un controllo di revoca della catena di certificati (incluso il certificato radice). I certificati di cloud rilasciati da Azure AD all'utente non hanno un CRL quanto sono inclusi i certificati di breve durati con una durata di un'ora. EAP in Criteri di rete deve essere configurato per ignorare l'assenza di un CRL. Per impostazione predefinita, IgnoreNoRevocationCheck è impostato su 0 (disabilitato). Aggiungi IgnoreNoRevocationCheck e Impostala su 1 per consentire l'autenticazione dei client quando il certificato non includere punti di distribuzione CRL. 

Poiché il metodo di autenticazione EAP-TLS, questo valore del Registro di sistema è necessaria solo in EAP\13. Se vengono utilizzati altri metodi di autenticazione EAP, quindi il valore del Registro di sistema deve essere aggiunto in quelli anche. 

**Procedura**

1. Apri **regedit.exe** nel server dei criteri di rete.

2. Passa alla **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13**.

3. Fai clic su **Modifica > Nuovo** e selezionare **valore DWORD (32-bit)** e digitare **IgnoreNoRevocationCheck**.

4. Fai doppio clic su **IgnoreNoRevocationCheck** e imposta i dati del valore su **1**.

5. Fai clic su **OK** e riavviare il server. Non è sufficiente riavviare i servizi RRAS e dei criteri di rete.

Per altre informazioni, vedi [come abilitare o disabilitare il controllo di revoche certificati (CRL) nei client](https://technet.microsoft.com/library/bb680540.aspx).


|Percorso del Registro di sistema  |Estensione EAP  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-MS-CHAP v2         |

## Passaggio successivo

Passaggio [7.2. Creare certificati radice per l'autenticazione VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md): In questo passaggio, configurare i certificati radice di accesso condizionale per l'autenticazione VPN con Azure AD, che crea automaticamente un'app cloud Server VPN nel tenant. 

---