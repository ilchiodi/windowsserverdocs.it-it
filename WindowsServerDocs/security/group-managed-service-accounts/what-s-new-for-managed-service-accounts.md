---
title: "Novità per gli account del servizio gestito"
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f2a8b6b-c152-4c40-b712-bfabff0e408b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: cac55d04a40c84ce160eb3883d6095a7db0ef3be
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="what39s-new-for-managed-service-accounts"></a>Cosa & #39; s novità di account dei servizi gestiti

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento destinato ai professionisti IT descrive le modifiche delle funzionalità per l'account del servizio gestiti con l'introduzione del gruppo di Account del servizio gestito (gMSA) in Windows Server 2012 e Windows 8.

Account del servizio gestito è progettato per fornire servizi e le attività, ad esempio servizi di Windows e pool di applicazioni IIS per condividere i propri account di dominio, eliminando la necessità di un amministratore di amministrare manualmente le password per questi account. È un account di dominio gestiti che forniscono la gestione automatica delle password.

## <a name="versions"></a>Novità per gli account del servizio gestito in Windows Server 2012 e Windows 8
Di seguito vengono descritte le modifiche delle funzionalità apportate a MSA in Windows Server 2012 e Windows 8.

### <a name="group-managed-service-accounts"></a>Account di servizio gestito del gruppo
Quando un account di dominio è configurato per un server in un dominio, il computer client possa eseguire l'autenticazione e connettersi a tale servizio. In precedenza, solo due tipi di account sono forniti identità senza richiedere la gestione delle password. Tuttavia, questi tipi di account presentano limitazioni:

-   L'account computer è limitato a un server di dominio e le password sono gestite dal computer

-   Account del servizio gestito è limitato a un server di dominio e le password sono gestite dal computer.

Questi account non possono essere condivisi tra più sistemi. Pertanto, è necessario gestire regolarmente l'account per ogni servizio in ogni sistema per evitare che scadano le relative password.

**Valore aggiunto da queste modifiche?**

L'Account del servizio gestito del gruppo risolve questo problema in quanto la password dell'account è gestita dai controller di dominio Windows Server 2012 e può essere recuperata da più sistemi Windows Server 2012. Ciò contribuisce a ridurre il sovraccarico amministrativo di un account del servizio consentendo a Windows di gestire le password per questi account.

**Differenze di funzionamento**

Nei computer che esegue Windows Server 2012 o Windows 8, un gruppo di account Microsoft possono essere creati e gestiti tramite Gestione controllo servizi, affinché numerose istanze del servizio, ad esempio distribuite in una server farm, può essere gestito da un server. Strumenti e utilità utilizzati per amministrare account del servizio gestiti, ad esempio Gestione Pool di applicazioni di IIS, può essere utilizzata con account del servizio gestito del gruppo. Gli amministratori di dominio possono delegare la gestione dei servizi agli amministratori dei servizi che possono gestire l'intero ciclo di vita di un Account del servizio gestito o l'Account del servizio gestito di gruppo. I computer client esistenti potranno eseguire l'autenticazione per qualsiasi servizio senza conoscere l'istanza di servizio che eseguono l'autenticazione per.

### <a name="interoperability"></a>Funzionalità rimosse o deprecate
Per Windows Server 2012, l'impostazione predefinita i cmdlet di Windows PowerShell per il gruppo account dei servizi gestiti anziché gli account del servizio gestiti del server di gestione.

## <a name="see-also"></a>Vedere anche

-   [Panoramica degli account del servizio gestito del gruppo](group-managed-service-accounts-overview.md)

-   [Panoramica di servizi di dominio Active Directory](active-directory-domain-services-overview.md)

-   [Account dei servizi gestiti: Informazioni, implementazione, procedure consigliate e risoluzione dei problemi](http://blogs.technet.com/b/askds/archive/20../managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)


