---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS e Active Directory Domain Services
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f6d75a78119d76a0f8380967292b1d0abc720597
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813142"
---
# <a name="dns-and-ad-ds"></a>DNS e Active Directory Domain Services

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servizi di dominio Active Directory (AD DS) Usa servizi di risoluzione dei nomi di sistema DNS (Domain Name) per rendere possibili per i client individuare i controller di dominio e per i controller di dominio che ospitano il servizio directory per comunicare tra loro.  
  
Active Directory Domain Services consente una facile integrazione dello spazio dei nomi Active Directory in uno spazio dei nomi DNS. Le funzionalità, ad esempio le zone DNS integrate in Active Directory semplificano la distribuzione DNS, eliminando la necessità di configurare le zone secondarie e quindi configurare i trasferimenti di zona.  
  
Per informazioni sul DNS supporta Active Directory Domain Services, vedere la sezione [supporto DNS per Active Directory Technical Reference](https://go.microsoft.com/fwlink/?LinkID=48147).  
  
> [!NOTE]  
> Se si implementa uno spazio dei nomi non contiguo in cui il nome di dominio Active Directory Domain Services è diverso dal suffisso DNS primario che i client usano, integrazione di Active Directory Domain Services con il servizio DNS è più complessa. Per altre informazioni, vedere [Namespace non contigue](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
- [Individuazione dei Controller di dominio](../../ad-ds/plan/Domain-Controller-Location.md)  
- [Attiva le zone DNS integrate in Directory](../../ad-ds/plan/Active-Directory-Integrated-DNS-Zones.md)  
- [Denominazione di computer](../../ad-ds/plan/Computer-Naming.md)  
- [Namespace non contiguo](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)  
