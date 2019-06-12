---
title: Novità dell'autenticazione Kerberos
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 90107bd49268f232fd6d532c304c2fdd050bcbf5
ms.sourcegitcommit: c6acac3622e5d34714ca5c569805931681f98779
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391501"
---
# <a name="whats-new-in-kerberos-authentication"></a>What's New in Kerberos Authentication

>Si applica a: Windows Server 2016 e Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>Supporto KDC per l'autenticazione client basata su attendibilità chiavi pubbliche

A partire da Windows Server 2016, KDC supportare una modalità di mapping di chiave pubblica. Se la chiave pubblica viene eseguito il provisioning di un account, il KDC supporta Kerberos PKInit in modo esplicito usando tale chiave. Poiché non esiste alcuna convalida del certificato, sono supportati i certificati autofirmati e verifica del meccanismo di autenticazione non è supportato.

Attendibilità chiavi è preferito quando configurato per un account indipendentemente dall'impostazione UseSubjectAltName.

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>Supporto client Kerberos e KDC per RFC 8070 PKInit Freshness estensione

A partire da Windows 10, versione 1607 e Windows Server 2016, i client Kerberos tentano il [estensione freshness RFC 8070 PKInit](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/) per la chiave pubblica basata su punti di accesso. 

A partire da Windows Server 2016, i KDC può supportare l'estensione freshness PKInit. Per impostazione predefinita, i KDC non è disponibile l'estensione freshness PKInit. Per abilitarla, usare il nuovo supporto KDC per l'impostazione di criteri modelli amministrativi KDC di estensione Freshness PKInit su tutti i controller di dominio nel dominio. Quando è configurato, le opzioni seguenti sono supportate quando il dominio è livello funzionale del dominio Windows Server 2016 (funzionalità del dominio):

- **Disattivata**: Il KDC non offre l'estensione Freshness PKInit e accetta le richieste di autenticazione valido senza verificare la validità. Gli utenti non riceveranno mai l'identità di chiave pubblica nuova SID.
- **Supportato**: Estensione Freshness PKInit è supportato su richiesta. I client Kerberos correttamente l'autenticazione con l'estensione Freshness PKInit ricevono l'identità di chiave pubblica nuova SID.
- **Richiesto**: L'estensione Freshness PKInit è necessaria per l'autenticazione ha esito positivo. I client Kerberos che non supportano l'estensione Freshness PKInit avrà sempre esito negativo quando si usano le credenziali tramite chiave pubbliche.

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Supporto dei dispositivi aggiunti a un dominio per l'autenticazione tramite chiave pubblica

A partire da Windows 10 versione 1507 e Windows Server 2016, se un dispositivo aggiunto al dominio è in grado di registrare la propria chiave pubblica associata con un controller di dominio (DC) di Windows Server 2016, quindi il dispositivo può eseguire l'autenticazione con la chiave pubblica usando l'autenticazione Kerberos un controller di dominio Windows Server 2016. Per altre informazioni, vedere [appartenenti a un dominio pubblico chiave l'autenticazione del dispositivo](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>I client Kerberos consentono i nomi host indirizzo di IPv4 e IPv6 in nomi dell'entità servizio (SPN)

A partire da Windows 10 versione 1507 e Windows Server 2016, client Kerberos possono essere configurati per supportare i nomi host IPv4 e IPv6 nei nomi dell'entità servizio. 

Percorso del Registro di sistema:

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

Per configurare il supporto per nomi host di indirizzi IP nell'entità servizio, creare una voce TryIPSPN. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. Se non è configurato, i nomi host indirizzo IP non vengono tentate.

Se il nome SPN viene registrato in Active Directory, l'autenticazione ha esito positivo con Kerberos. 

Per ulteriori informazioni consultare il documento [configurazione di Kerberos per gli indirizzi IP](configuring-kerberos-over-ip.md).

## <a name="kdc-support-for-key-trust-account-mapping"></a>Supporto KDC per attendibilità chiavi il mapping di account

A partire da Windows Server 2016, i controller di dominio dispongono di supporto per il mapping di account di attendibilità chiavi, nonché il fallback su esistente AltSecID e del nome dell'entità utente (UPN) nel comportamento SAN. UseSubjectAltName quando è impostata su:

- 0: Mapping esplicito è obbligatorio. Deve essere presente uno:
    - Attendibilità chiavi (una novità di Windows Server 2016)
    - ExplicitAltSecID
- 1: Mapping implicito è consentito (impostazione predefinita):
    1. Se attendibilità chiavi è configurato per l'account, quindi viene utilizzato per il mapping (novità di Windows Server 2016).
    2. Se è presente alcun UPN nel SAN, viene tentata AltSecID per il mapping.
    3. Se è presente un UPN nel SAN, viene tentata UPN per il mapping.

## <a name="see-also"></a>Vedere anche

- [Panoramica dell'autenticazione Kerberos](kerberos-authentication-overview.md)
