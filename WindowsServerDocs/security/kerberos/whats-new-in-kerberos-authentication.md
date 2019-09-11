---
title: What's New in Kerberos Authentication
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 35274147dcee9d31751d8ca61033244bd37a1759
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870271"
---
# <a name="whats-new-in-kerberos-authentication"></a>What's New in Kerberos Authentication

>Si applica a: Windows Server 2016 e Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>Supporto KDC per l'autenticazione client basata sull'attendibilità della chiave pubblica

A partire da Windows Server 2016, KDC supporta una modalità di mapping delle chiavi pubbliche. Se viene eseguito il provisioning della chiave pubblica per un account, il KDC supporta PKInit Kerberos in modo esplicito usando tale chiave. Poiché non è presente alcuna convalida del certificato, i certificati autofirmati sono supportati e la garanzia del meccanismo di autenticazione non è supportata.

L'attendibilità della chiave è preferibile quando configurato per un account indipendentemente dall'impostazione di UseSubjectAltName.

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>Supporto del client Kerberos e del KDC per l'estensione di aggiornamento PKInit RFC 8070

A partire da Windows 10, versione 1607 e Windows Server 2016, i client Kerberos tentano l' [estensione di aggiornamento PKInit RFC 8070](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/) per gli accessi basati su chiave pubblica. 

A partire da Windows Server 2016, KDC può supportare l'estensione di aggiornamento PKInit. Per impostazione predefinita, KDC non offre l'estensione di aggiornamento PKInit. Per abilitarla, usare l'impostazione dei criteri dei modelli amministrativi KDC del supporto KDC per l'estensione PKInit per tutti i controller di dominio nel dominio. Una volta configurate, le opzioni seguenti sono supportate quando il dominio è il livello di funzionalità del dominio Windows Server 2016 (DFL):

- **Disattivata**: Il KDC non offre mai l'estensione di aggiornamento PKInit e accetta richieste di autenticazione valide senza verificarne l'aggiornamento. Gli utenti non riceveranno mai il SID di identità della chiave pubblica aggiornata.
- **Supportato**: L'estensione di aggiornamento PKInit è supportata su richiesta. I client Kerberos che eseguono l'autenticazione con l'estensione di aggiornamento PKInit ricevono il SID di identità della chiave pubblica aggiornata.
- **Richiesto**: Per l'autenticazione corretta, è necessaria l'estensione di aggiornamento PKInit. I client Kerberos che non supportano l'estensione di aggiornamento PKInit avranno sempre esito negativo quando si utilizzano le credenziali della chiave pubblica.

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Supporto dei dispositivi aggiunti a un dominio per l'autenticazione con chiave pubblica

A partire da Windows 10 versione 1507 e Windows Server 2016, se un dispositivo aggiunto a un dominio è in grado di registrare la chiave pubblica associata con un controller di dominio di Windows Server 2016 (DC), il dispositivo può eseguire l'autenticazione con la chiave pubblica usando l'autenticazione Kerberos per un controller di dominio Windows Server 2016. Per altre informazioni, vedere [autenticazione con chiave pubblica del dispositivo aggiunto](Domain-joined-Device-Public-Key-Authentication.md) a un dominio

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>I client Kerberos consentono i nomi host IPv4 e IPv6 nei nomi dell'entità servizio (SPN)

A partire da Windows 10 versione 1507 e Windows Server 2016, i client Kerberos possono essere configurati per supportare i nomi host IPv4 e IPv6 nei nomi SPN. 

Percorso del registro di sistema:

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

Per configurare il supporto per i nomi host degli indirizzi IP nei nomi SPN, creare una voce TryIPSPN. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. Se non è configurato, i nomi host degli indirizzi IP non vengono tentati.

Se il nome SPN è registrato in Active Directory, l'autenticazione ha esito positivo con Kerberos. 

Per altre informazioni, vedere il documento [configurazione di Kerberos per gli indirizzi IP](configuring-kerberos-over-ip.md).

## <a name="kdc-support-for-key-trust-account-mapping"></a>Supporto KDC per il mapping dell'account attendibilità della chiave

A partire da Windows Server 2016, i controller di dominio dispongono del supporto per il mapping dell'account attendibilità della chiave, nonché il fallback a AltSecID esistenti e al nome dell'entità utente (UPN) nel comportamento SAN. Quando UseSubjectAltName è impostato su:

- 0: Il mapping esplicito è obbligatorio. Devono quindi essere presenti:
    - Trust chiave (nuovo con Windows Server 2016)
    - ExplicitAltSecID
- 1: Il mapping implicito è consentito (impostazione predefinita):
    1. Se il trust della chiave è configurato per l'account, viene usato per il mapping (nuovo con Windows Server 2016).
    2. Se non è presente alcun UPN nella SAN, viene tentato il mapping di AltSecID.
    3. Se è presente un UPN nella rete SAN, si tenta di eseguire il mapping dell'UPN.

## <a name="see-also"></a>Vedere anche

- [Panoramica dell'autenticazione Kerberos](kerberos-authentication-overview.md)
