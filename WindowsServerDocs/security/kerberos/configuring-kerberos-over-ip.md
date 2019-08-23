---
title: Configurazione di Kerberos per l'indirizzo IP
description: Supporto di Kerberos per SPN basati su IP
author: daveba
ms.author: daveba
ms.openlocfilehash: 1061364528100fe005e80f64c6315f9fca69ad98
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2019
ms.locfileid: "69980292"
---
# <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>I client Kerberos consentono i nomi host IPv4 e IPv6 nei nomi dell'entità servizio (SPN)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

A partire da Windows 10 versione 1507 e Windows Server 2016, i client Kerberos possono essere configurati per supportare i nomi host IPv4 e IPv6 nei nomi SPN.

Per impostazione predefinita, Windows non tenterà di eseguire l'autenticazione Kerberos per un host se il nome host è un indirizzo IP. Verrà eseguito il fallback ad altri protocolli di autenticazione abilitati come NTLM. Tuttavia, le applicazioni sono talvolta hardcoded per l'utilizzo di indirizzi IP, il che significa che l'applicazione eseguirà il fallback a NTLM senza utilizzare Kerberos. Questo può causare problemi di compatibilità perché gli ambienti si spostano per disabilitare NTLM.

Per ridurre l'effetto della disabilitazione di NTLM, è stata introdotta una nuova funzionalità che consente agli amministratori di utilizzare gli indirizzi IP come nomi host nei nomi delle entità servizio. Questa funzionalità è abilitata sul client tramite un valore della chiave del registro di sistema.

```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters" /v TryIPSPN /t REG_DWORD /d 1 /f
```

Per configurare il supporto per i nomi host degli indirizzi IP nei nomi SPN, creare una voce TryIPSPN. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. Questo valore del registro di sistema deve essere impostato su ogni computer client che deve accedere alle risorse protette da Kerberos in base all'indirizzo IP.

## <a name="configuring-a-service-principal-name-as-ip-address"></a>Configurazione di un nome dell'entità servizio come indirizzo IP

Un nome dell'entità servizio è un identificatore univoco usato durante l'autenticazione Kerberos per identificare un servizio nella rete. Un nome SPN è costituito da un servizio, da un nome host e, facoltativamente `service/hostname[:port]` , da `host/fs.contoso.com`una porta in formato, ad esempio. Windows registrerà più nomi SPN in un oggetto computer quando un computer viene aggiunto al Active Directory.

Gli indirizzi IP non vengono in genere utilizzati al posto dei nomi host perché gli indirizzi IP sono spesso temporanei. Questo può causare conflitti e errori di autenticazione perché i lease di indirizzi scadono e si rinnovano. La registrazione di un nome SPN basato su indirizzi IP è quindi un processo manuale e deve essere usato solo quando non è possibile passare a un nome host basato su DNS.

L'approccio consigliato consiste nell'utilizzare lo strumento [SetSPN. exe](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)) . Si noti che un nome SPN può essere registrato solo per un singolo account in Active Directory alla volta, quindi è consigliabile che gli indirizzi IP dispongano di lease statici se si utilizza DHCP.

```
Setspn -s <service>/ip.address> <domain-user-account>  
```

Esempio:

```
Setspn -s host/192.168.1.1 server01
```
