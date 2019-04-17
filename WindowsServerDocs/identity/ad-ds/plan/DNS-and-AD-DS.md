---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS e Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8e7ee494c157396a7d58e9fd1b4b80060c4d99fc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="dns-and-ad-ds"></a>DNS e Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servizi di dominio Active Directory (AD DS) utilizza servizi di risoluzione nome sistema DNS (Domain Name) per consentire ai client di individuare i controller di dominio e per i controller di dominio che ospitano il servizio directory per comunicare tra loro.  
  
Servizi di dominio Active Directory consente una facile integrazione dello spazio dei nomi Active Directory in uno spazio dei nomi DNS. Funzionalità ad esempio, zone DNS integrate in Active Directory rendono più semplice per la distribuzione DNS, eliminando la necessità di impostare le zone secondarie e quindi configurare i trasferimenti di zona.  
  
Per informazioni su come DNS supporta servizi di dominio Active Directory, vedere il supporto di DNS per Active Directory Technical Reference ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147)).  
  
> [!NOTE]  
> Se si implementa uno spazio dei nomi indipendente in cui il nome di dominio Active Directory è diverso dal suffisso DNS primario utilizzati dai client, l'integrazione di dominio Active Directory con DNS è più complesso. Per ulteriori informazioni, vedere [indipendenti Namespace](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md).  
  
## <a name="in-this-section"></a>In questa sezione  
  
-   [Individuazione dei Controller di dominio](../../ad-ds/plan/Domain-Controller-Location.md)  
  
-   [Attiva le zone DNS integrate nella Directory](../../ad-ds/plan/Active-Directory-Integrated-DNS-Zones.md)  
  
-   [La denominazione di computer](../../ad-ds/plan/Computer-Naming.md)  
  
-   [Namespace indipendente](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)  
  


