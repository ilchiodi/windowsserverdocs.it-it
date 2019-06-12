---
title: Configurazione di Kerberos per l'indirizzo IP
description: Supporto di Kerberos per i nomi SPN basato su IP
ms.openlocfilehash: 30741f7a0f1978fcaa6ac83c98a54c07e1ef25c5
ms.sourcegitcommit: c6acac3622e5d34714ca5c569805931681f98779
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391522"
---
# <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>I client Kerberos consentono i nomi host indirizzo di IPv4 e IPv6 in nomi dell'entità servizio (SPN)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

A partire da Windows 10 versione 1507 e Windows Server 2016, client Kerberos possono essere configurati per supportare i nomi host IPv4 e IPv6 nei nomi dell'entità servizio.

Per impostazione predefinita Windows non tenterà l'autenticazione Kerberos per un host se il nome host è un indirizzo IP. Eseguirà il fallback per altri protocolli di autenticazione abilitate, ad esempio NTLM. Tuttavia, le applicazioni sono talvolta hardcoded per l'uso di indirizzi IP che indica che l'applicazione verrà reimpostata come NTLM e non usa Kerberos. Ciò può causare problemi di compatibilità come spostare gli ambienti disabilitare NTLM.

Per ridurre l'impatto della disabilitazione NTLM una nuova funzionalità è stata introdotta che consente agli amministratori di usare gli indirizzi IP come nomi host nei nomi delle entità servizio. Questa funzionalità è abilitata nel client tramite un valore della chiave del Registro di sistema.

```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters" /v TryIPSPN /t REG_DWORD /d 1 /f
```

Per configurare il supporto per nomi host di indirizzi IP nell'entità servizio, creare una voce TryIPSPN. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. Questo valore del Registro di sistema dovrà essere impostata in ogni computer client che richiede l'accesso a risorse protette da Kerberos in base all'indirizzo IP.

## <a name="configuring-a-service-principal-name-as-ip-address"></a>Configurazione di un nome SPN come indirizzo IP

Un nome dell'entità servizio è un identificatore univoco utilizzato durante l'autenticazione Kerberos per identificare un servizio di rete. Un nome SPN è costituito da un servizio, nome host e, facoltativamente, una porta nel formato `service/hostname[:port]` , ad esempio `host/fs.contoso.com`. Windows registra più nomi SPN per un oggetto computer quando un computer viene aggiunto ad Active Directory.

Indirizzi IP non vengono usati in genere al posto di nomi host perché gli indirizzi IP sono spesso temporanei. Questo può causare conflitti ed errori di autenticazione come scadono e rinnovano di lease di indirizzi. Di conseguenza la registrazione di un SPN basato su indirizzi IP è un processo manuale e deve essere usata solo quando non è possibile passare a un nome host basati su DNS.

L'approccio consigliato consiste nell'usare la [Setspn.exe](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)) dello strumento. Si noti che un nome SPN può essere registrato solo a un singolo account in Active Directory in un momento pertanto è consigliabile che gli indirizzi IP con lease statici se si utilizza DHCP.

```
Setspn -s <service>/ip.address> <domain-user-account>  
```

Esempio:

```
Setspn -s host/192.168.1.1 server01
```
