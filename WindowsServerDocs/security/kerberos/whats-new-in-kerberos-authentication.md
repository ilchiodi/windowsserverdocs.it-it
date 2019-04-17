---
title: "Novità dell'autenticazione Kerberos"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 7b046490c606cdf9e1436f503bf46a9cd4280ea9
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2018
---
# <a name="whats-new-in-kerberos-authentication"></a>Novità dell'autenticazione Kerberos

>Si applica a: Windows Server 2016 e Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>Supporto KDC per l'autenticazione client basata su Trust chiave pubblica

A partire da Windows Server 2016, il KDC supportano una modalità di mapping di chiave pubblica. Se la chiave pubblica viene eseguito il provisioning di un account, il KDC supporta Kerberos PKInit in modo esplicito tramite tale chiave. Poiché non esiste alcuna convalida del certificato, sono supportati i certificati autofirmati e verifica del meccanismo di autenticazione non è supportato.

Chiave di Trust è preferibile quando configurato per un account indipendentemente dall'impostazione UseSubjectAltName.

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>Supporto client Kerberos e KDC per RFC 8070 PKInit validità estensione

A partire da Windows 10 versione 1607 e Windows Server 2016, i client Kerberos tentano di [estensione validità RFC 8070 PKInit](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/) per la chiave pubblica in base ai punti di accesso. 

A partire da Windows Server 2016, i KDC possono supportare l'estensione della validità PKInit. Per impostazione predefinita, i KDC non offrono l'estensione della validità PKInit. Per abilitarlo, usare il nuovo supporto KDC per l'impostazione di criteri di modelli amministrativi KDC di estensione validità PKInit in tutti i controller di dominio nel dominio. Quando è configurato, le opzioni seguenti sono supportate quando il dominio a livello funzionale del dominio Windows Server 2016 (livello):

- **Disabilitato**: il KDC non offre l'estensione della validità PKInit e accetta le richieste di autenticazione valido senza verificare la validità. Gli utenti non riceveranno mai l'identità di chiave pubblica nuovo SID.
- **Supportato**: PKInit validità estensione è supportato su richiesta. I client Kerberos correttamente l'autenticazione con l'estensione della validità PKInit ricevono l'identità di chiave pubblica nuovo SID.
- **Obbligatorio**: validità PKInit l'estensione è necessaria per l'autenticazione. I client Kerberos che non supportano l'estensione della validità PKInit avrà sempre esito negativo quando si utilizzano credenziali a chiave pubblica.

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Supporto dei dispositivi appartenenti a un dominio per l'autenticazione mediante chiave pubblica

A partire da Windows 10 versione 1507 e Windows Server 2016, se un dispositivo aggiunto al dominio è in grado di registrare la propria chiave pubblica associata con un controller di dominio (DC) di Windows Server 2016, quindi il dispositivo può eseguire l'autenticazione con la chiave pubblica con l'autenticazione Kerberos per un controller di dominio di Windows Server 2016. Per ulteriori informazioni, vedere [appartenenti a un dominio pubblico chiave di autenticazione del dispositivo](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>I client Kerberos consentire nomi host di indirizzi IPv4 e IPv6 in nomi dell'entità servizio (SPN)

A partire da Windows 10 versione 1507 e Windows Server 2016, i client Kerberos possono essere configurati per supportare nomi host IPv4 e IPv6 in SPN. 

Percorso del Registro di sistema:

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

Per configurare il supporto per nomi host di indirizzi IP in nomi SPN, creare una voce TryIPSPN. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. Se non è configurato, nomi host di indirizzo IP non sono tentato.

Se il SPN sia registrato in Active Directory, l'autenticazione ha esito positivo con il protocollo Kerberos. 

## <a name="kdc-support-for-key-trust-account-mapping"></a>Supporto KDC per il mapping account Trust chiave

A partire da Windows Server 2016, i controller di dominio includono il supporto per il mapping account Trust chiave, nonché fallback esistente AltSecID e nome dell'entità utente (UPN), il comportamento di SAN. Quando UseSubjectAltName è impostato su:

- 0: mappatura esplicita è obbligatorio. Quindi deve essere presente uno:
    - Chiave attendibile (novità di Windows Server 2016)
    - ExplicitAltSecID
- 1: mappatura implicita è consentita (impostazione predefinita):
    1. Se chiave attendibile è configurato per l'account, quindi viene usato per il mapping (novità di Windows Server 2016).
    2. Se non esiste alcun UPN nel SAN, viene tentata AltSecID per il mapping.
    3. Se è presente un nome UPN nel SAN, viene tentata UPN per il mapping.

## <a name="see-also"></a>Vedere anche

- [Panoramica dell'autenticazione Kerberos](kerberos-authentication-overview.md)
