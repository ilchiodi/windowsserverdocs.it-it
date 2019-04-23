---
title: Introduzione a associazione dei Token
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 2a0bb8c75fe3ca7f7befe0bd67eb3d363a5ad7a9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884232"
---
# <a name="introducing-token-binding"></a>Introduzione a associazione dei Token

>Si applica a: Windows Server 2016 e Windows 10

Il Token Binding protocol consente alle applicazioni e servizi da associare a livello di crittografia token di sicurezza al livello di TLS per ridurre il furto di token e attacchi di tipo replay. Le associazioni [RFC5246] TLS di lunga durate, identificabile in modo univoco possono estendersi su più connessioni e sessioni TLS.

Supporto della versione:

- Windows 10 versione 1507: disattivato per impostazione predefinita
    - Token Binding Protocol aggiunto [[draft-ietf-tokbind-protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet & HTTP. Il supporto dell'associazione dei token tramite HTTP SYS [[draft-ietf-tokbind-https-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10, versione 1607 e 1511 e Windows Server 2016-in per impostazione predefinita
    - Token Binding Protocol aggiornato [[draft-ietf-tokbind-protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - Estensione TLS per la negoziazione di token di associazione aggiunta [[bozza-popov-tokbind-negoziazione-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet & HTTP. Il supporto dell'associazione dei token tramite HTTP aggiornato SYS [[draft-ietf-tokbind-https-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10 versione 1507 con aggiornamento di manutenzione [KB4034668](https://support.microsoft.com/kb/KB4034668), Windows 10 versione 1511 con aggiornamento di manutenzione [KB4034660](https://support.microsoft.com/kb/KB4034660), Windows 10, versione 1607 e Windows Server 2016 con aggiornamento deiservizi[KB4034658](https://support.microsoft.com/kb/KB4034658) supporta Token Binding Protocol versione 0.10 – in per impostazione predefinita
    - Token Binding Protocol aggiornato [[draft-ietf-tokbind-protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Estensione TLS per la negoziazione di token di associazione aggiunta [[draft-ietf-tokbind-negoziazione-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Il supporto dell'associazione dei token tramite HTTP aggiornato SYS [[draft-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows 10, versione 1703 supporta Token Binding Protocol versione 0.10 – per impostazione predefinita
    - Token Binding Protocol aggiornato [[draft-ietf-tokbind-protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Estensione TLS per la negoziazione di token di associazione aggiunta [[draft-ietf-tokbind-negoziazione-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Il supporto dell'associazione dei token tramite HTTP aggiornato SYS [[draft-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - I dispositivi Windows con protezione basata sulla virtualizzazione abilitata manterrà le chiavi di associazione dei token in un ambiente protetto isolato dal sistema operativo in esecuzione

Informazioni sul supporto di ASP .NET sono reperibile nel [origine riferimento di .NET Framework](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170). 

Per informazioni su .NET Framework, vedere gli argomenti seguenti:

- [Miglioramenti di rete](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [Classe TokenBinding .NET](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
