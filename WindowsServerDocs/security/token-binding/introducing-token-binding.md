---
title: Introduzione all'associazione di token
ms.prod: windows-server
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: d067db04fe881193143104ce9f75a0c9932907e7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855894"
---
# <a name="introducing-token-binding"></a>Introduzione all'associazione di token

>Si applica a: Windows Server 2016 e Windows 10

Il protocollo di associazione dei token consente alle applicazioni e ai servizi di associare in modo crittografico i propri token di sicurezza al livello TLS per attenuare il furto dei token e gli attacchi di riproduzione. Le associazioni TLS [RFC5246] di lunga durata e univocamente identificabili possono estendersi su più sessioni e connessioni TLS.

Supporto della versione:

- Windows 10, versione 1507-disattivato per impostazione predefinita
    - Token binding Protocol aggiunto [[draft-ietf-tokbind-Protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet & HTTP. Supporto SYS dell'associazione di token su HTTP [[draft-ietf-tokbind-HTTPS-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10, versioni 1511 e 1607 e Windows Server 2016-on per impostazione predefinita
    - Aggiornamento del protocollo di binding del token [[draft-ietf-tokbind-Protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - Estensione TLS per la negoziazione dell'associazione di token aggiunta [[Draft-Popov-tokbind-Negotiate-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet & HTTP. Supporto SYS dell'associazione di token su HTTP aggiornato [[draft-ietf-tokbind-HTTPS-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10, versione 1507 con aggiornamento del servizio [KB4034668](https://support.microsoft.com/kb/KB4034668), Windows 10, versione 1511 con aggiornamento di manutenzione [KB4034660](https://support.microsoft.com/kb/KB4034660), windows 10, versione 1607 e Windows Server 2016 con aggiornamento del servizio [KB4034658](https://support.microsoft.com/kb/KB4034658) support token binding Protocol versione 0,10-on per impostazione predefinita
    - Aggiornamento del protocollo di binding del token [[draft-ietf-tokbind-Protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Estensione TLS per la negoziazione dell'associazione di token aggiunta [[draft-ietf-tokbind-Negotiation-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Supporto SYS dell'associazione di token su HTTP aggiornato [[draft-ietf-tokbind-HTTPS-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows 10, versione 1703 supporta token binding Protocol versione 0,10-on per impostazione predefinita
    - Aggiornamento del protocollo di binding del token [[draft-ietf-tokbind-Protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Estensione TLS per la negoziazione dell'associazione di token aggiunta [[draft-ietf-tokbind-Negotiation-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Supporto SYS dell'associazione di token su HTTP aggiornato [[draft-ietf-tokbind-HTTPS-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - I dispositivi Windows con sicurezza basata sulla virtualizzazione abilitata manterranno le chiavi di associazione dei token in un ambiente protetto isolato dal sistema operativo in esecuzione

Informazioni sul supporto per ASP .NET sono disponibili nell' [.NET Framework origine riferimento](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170). 

Per informazioni su .NET Framework, vedere gli argomenti seguenti:

- [Miglioramenti di rete](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [Classe di token .NET](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
