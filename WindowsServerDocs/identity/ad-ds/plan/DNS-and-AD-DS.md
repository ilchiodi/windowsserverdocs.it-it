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
ms.openlocfilehash: 0eaddb6e94011a14b352b813cf508062116001f1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822724"
---
# <a name="dns-and-ad-ds"></a>DNS e Active Directory Domain Services

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Domain Services (AD DS) utilizza i servizi di risoluzione dei nomi Domain Name System (DNS) per consentire ai client di individuare i controller di dominio e i controller di dominio che ospitano il servizio directory per comunicare tra loro.  
  
Servizi di dominio Active Directory consente un'integrazione semplificata dello spazio dei nomi Active Directory in uno spazio dei nomi DNS esistente. Le funzionalità come le zone DNS integrate Active Directory rendono più semplice la distribuzione di DNS eliminando la necessità di configurare le zone secondarie e quindi di configurare i trasferimenti di zona.  
  
Per informazioni sul supporto di servizi di dominio Active Directory in DNS, vedere la sezione [supporto DNS per Active Directory riferimento tecnico](https://go.microsoft.com/fwlink/?LinkID=48147).  
  
> [!NOTE]  
> Se si implementa uno spazio dei nomi non contiguo in cui il nome di dominio di servizi di dominio Active Directory differisce dal suffisso DNS primario utilizzato dai client, l'integrazione di servizi di dominio Active Directory con DNS è più complessa. Per ulteriori informazioni, vedere [spazio dei nomi non contiguo](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
- [Posizione del controller di dominio](../../ad-ds/plan/Domain-Controller-Location.md)  
- [Zone DNS integrate in Active Directory](../../ad-ds/plan/Active-Directory-Integrated-DNS-Zones.md)  
- [Denominazione dei computer](../../ad-ds/plan/Computer-Naming.md)  
- [Spazio dei nomi indipendente](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)  
