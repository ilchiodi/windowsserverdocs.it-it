---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS e Active Directory Domain Services
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 817c6ca168e33df1a0c59e151a303c4259313743
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624299"
---
# <a name="dns-and-ad-ds"></a>DNS e Active Directory Domain Services

> Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Domain Services (AD DS) utilizza i servizi di risoluzione dei nomi Domain Name System (DNS) per consentire ai client di individuare i controller di dominio e i controller di dominio che ospitano il servizio directory per comunicare tra loro.

Servizi di dominio Active Directory consente un'integrazione semplificata dello spazio dei nomi Active Directory in uno spazio dei nomi DNS esistente. Le funzionalità come le zone DNS integrate Active Directory rendono più semplice la distribuzione di DNS eliminando la necessità di configurare le zone secondarie e quindi di configurare i trasferimenti di zona.

Per informazioni sul supporto di servizi di dominio Active Directory in DNS, vedere la sezione [supporto DNS per Active Directory riferimento tecnico](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc781627(v=ws.10)).

> [!NOTE]
> Se si implementa uno spazio dei nomi non contiguo in cui il nome di dominio di servizi di dominio Active Directory differisce dal suffisso DNS primario utilizzato dai client, l'integrazione di servizi di dominio Active Directory con DNS è più complessa. Per ulteriori informazioni, vedere [spazio dei nomi non contiguo](Disjoint-Namespace.md).

## <a name="in-this-section"></a>Contenuto della sezione

- [Posizione del controller di dominio](Domain-Controller-Location.md)
- [Zone DNS integrate in Active Directory](Active-Directory-Integrated-DNS-Zones.md)
- [Denominazione dei computer](Computer-Naming.md)
- [Spazio dei nomi indipendente](Disjoint-Namespace.md)
