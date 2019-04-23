---
title: What's New for Managed Service Accounts
description: Sicurezza di Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872182"
---
# <a name="what39s-new-for-managed-service-accounts"></a>Cosa&#39;s novità di account dei servizi gestiti

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento destinato ai professionisti IT descrive le modifiche di funzionalità per gli account del servizio gestito con l'introduzione del gruppo Account del servizio gestito (gMSA) in Windows Server 2012 e Windows 8.

L'account del servizio gestito è progettato per fornire servizi e attività, ad esempio servizi di Windows e pool di applicazioni IIS per condividere i relativi account di dominio, per consentire così agli amministratori di non dover amministrare manualmente le password per tali account. Si tratta di un account di un dominio gestito che offre la gestione automatica delle password.

## <a name="versions"></a>Quali sono le novità per gli account del servizio gestiti in Windows Server 2012 e Windows 8
Di seguito descritte le modifiche delle funzionalità apportate all'account del servizio gestito in Windows Server 2012 e Windows 8.

### <a name="group-managed-service-accounts"></a>Account del servizio gestito di gruppo
Quando per un server in un dominio è configurato un account di dominio, il computer client può eseguire l'autenticazione e la connessione al servizio in questione. In precedenza, solo due tipi di account erano in grado di fornire l'identità senza richiedere la gestione delle password. Questi tipi di account presentano tuttavia alcune limitazioni:

-   L'account del computer è limitato a un server di dominio e le password sono gestite dal computer

-   L'account del servizio gestito è limitato a un server di dominio e le password sono gestite dal computer

Tali account non possono essere condivisi tra più sistemi. È pertanto necessario gestire regolarmente l'account per ogni servizio e in ogni sistema per evitare che scadano le relative password.

**Valore aggiunto da queste modifiche**

L'Account del servizio gestito del gruppo risolve questo problema, perché la password dell'account è gestita da controller di dominio di Windows Server 2012 e può essere recuperata da più sistemi Windows Server 2012. In questo modo è possibile ridurre il sovraccarico amministrativo di un account del servizio consentendo a Windows di gestire le password per tali account.

**Differenze di funzionamento**

Nei computer che eseguono Windows Server 2012 o Windows 8, un gruppo di account del servizio gestito possono essere create e gestite tramite Gestione controllo servizi, affinché numerose istanze del servizio, ad esempio distribuite in una server farm, può essere gestito da un server. Gli strumenti e le utilità utilizzati per la gestione degli account del servizio gestiti, ad esempio Gestione pool di applicazioni IIS, possono essere utilizzati anche con gli account del servizio gestiti del gruppo. Gli amministratori di dominio possono delegare la gestione dei servizi agli amministratori dei servizi che possono gestire l'intero ciclo di vita di un account del servizio gestito o di un account del servizio gestito del gruppo. I computer client esistenti potranno eseguire l'autenticazione per qualsiasi servizio senza conoscere l'istanza del servizio in cui stanno eseguendo l'autenticazione.

### <a name="interoperability"></a>Funzionalità rimosse o deprecate
Per Windows Server 2012, l'impostazione predefinita i cmdlet di Windows PowerShell per gestire il gruppo account del servizio gestito anziché l'account del servizio gestiti del server.

## <a name="see-also"></a>Vedere anche

-   [Panoramica degli account del servizio gestito del gruppo](group-managed-service-accounts-overview.md)

-   [Panoramica di servizi di dominio Active Directory](active-directory-domain-services-overview.md)

-   [Account del servizio gestiti: Informazioni, implementazione, procedure consigliate e risoluzione dei problemi](http://blogs.technet.com/b/askds/archive/20../managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)


