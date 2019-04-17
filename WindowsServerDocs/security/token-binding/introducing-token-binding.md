---
title: Introduzione a Binding di Token
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 2a0bb8c75fe3ca7f7befe0bd67eb3d363a5ad7a9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="introducing-token-binding"></a>Introduzione a Binding di Token

>Si applica a: Windows Server 2016 e Windows 10

Il protocollo Binding Token consente alle applicazioni e servizi eseguire il binding crittograficamente token di sicurezza al livello di TLS per ridurre il furto di token e attacchi di tipo replay. Le associazioni di lunga durate, identificabile in modo univoco TLS [RFC5246] possono estendersi su più sessioni TLS e connessioni.

Supporto delle versioni:

- Windows 10 versione 1507: disattivato per impostazione predefinita
    - Token di associazione di protocollo aggiunto [[bozza-ietf-tokbind-protocollo-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet & HTTP.SYS il supporto dei binding di token su HTTP [[bozza-ietf-tokbind-https-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10, versione 1511 e 1607 e Windows Server 2016-in per impostazione predefinita
    - Token di associazione di protocollo aggiornato [[bozza-ietf-tokbind-protocollo-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - Estensione TLS per la negoziazione di binding di token aggiunta [[bozza-popov-tokbind-negoziazione-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet & HTTP.SYS il supporto dei binding di token su HTTP aggiornato [[bozza ietf-tokbind-https-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10 versione 1507 con l'aggiornamento di manutenzione [KB4034668](https://support.microsoft.com/kb/KB4034668), Windows 10 versione 1511 con l'aggiornamento di manutenzione [KB4034660](https://support.microsoft.com/kb/KB4034660), Windows 10 versione 1607 e Windows Server 2016 con l'aggiornamento di manutenzione [KB4034658](https://support.microsoft.com/kb/KB4034658) supporta il protocollo di Binding Token versione 0,10 – su per impostazione predefinita
    - Token di associazione di protocollo aggiornato [[bozza-ietf-tokbind-protocollo-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Estensione TLS per la negoziazione di binding di token aggiunta [[bozza ietf tokbind-negoziazione-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP.SYS il supporto dei binding di token su HTTP aggiornato [[bozza-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows 10, versione 1703 supporta Token Binding protocollo versione 0,10-in per impostazione predefinita
    - Token di associazione di protocollo aggiornato [[bozza-ietf-tokbind-protocollo-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Estensione TLS per la negoziazione di binding di token aggiunta [[bozza ietf tokbind-negoziazione-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP.SYS il supporto dei binding di token su HTTP aggiornato [[bozza-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - Dispositivi Windows con protezione basata su virtualizzazione abilitata manterrà i token di associazione di chiavi in un ambiente protetto isolato dal sistema operativo in esecuzione

Informazioni sul supporto di ASP .NET, visitare il [origine di riferimento di .NET Framework](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170). 

Per informazioni su .NET Framework, vedere gli argomenti seguenti:

- [Miglioramenti di rete](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [Classe .NET TokenBinding](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
