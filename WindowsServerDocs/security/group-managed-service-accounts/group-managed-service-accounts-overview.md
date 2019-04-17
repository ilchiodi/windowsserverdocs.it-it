---
title: Panoramica degli account del servizio gestito del gruppo
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cef0693c-f861-48a7-a1c0-8d1bc06143ce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 4912ae273e603b4a3362c4984da710f780c3e8b3
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/12/2018
---
# <a name="group-managed-service-accounts-overview"></a>Panoramica degli account del servizio gestito del gruppo

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento per i professionisti IT illustra l'Account del servizio gestito di gruppo di applicazioni pratiche, modifica nell'implementazione di Microsoft e i requisiti hardware e software.


## <a name="BKMK_OVER"></a>Descrizione delle funzionalità
Account del servizio gestito, introdotti in Windows Server 2008 R2 e Windows 7, sono account di dominio gestiti che forniscono la gestione automatica delle password e la gestione SPN semplificata, inclusa la delega della gestione ad altri amministratori.

Il gruppo di Account dei servizi gestiti offre le stesse funzionalità all'interno del dominio ma estende tali funzionalità su più server. Quando ci si connette a un servizio ospitato in una server farm, ad esempio bilanciamento carico di rete, i protocolli di autenticazione supportano l'autenticazione reciproca richiedono che tutte le istanze dei servizi utilizzino la stessa entità. Quando l'Account del servizio gestito di gruppo vengono utilizzati come entità di servizio, il sistema operativo Windows gestisce la password dell'account anziché affidarsi all'amministratore di gestire la password.

\(Kdssvc.dll\) il servizio distribuzione chiavi Microsoft fornisce il meccanismo per ottenere in modo sicuro la chiave più recente o una chiave specifica con un identificatore chiave per un account di Active Directory. Il servizio distribuzione chiavi condivide un segreto che viene utilizzato per creare le chiavi per l'account. Queste chiavi vengono modificate periodicamente. Per un Account del servizio gestito del gruppo controller di dominio calcola la password sulla chiave fornita dai servizi di distribuzione di chiave, inoltre, per gli altri attributi dell'Account del servizio gestito di gruppo.  Gli host membri possono ottenere i valori correnti e precedenti password contattando un controller di dominio.

## <a name="BKMK_APP"></a>Applicazioni pratiche
Account del servizio gestito del gruppo offrono una soluzione singola identità per i servizi in esecuzione in una server farm o nei sistemi sottostanti a bilanciamento carico di rete. Fornendo una soluzione MSA del gruppo, i servizi possono essere configurati per la nuova entità MSA del gruppo e la gestione delle password è gestita da Windows.

Utilizzare un gruppo di Account del servizio gestito, servizi o gli amministratori del servizio non è necessario gestire la sincronizzazione delle password tra istanze dei servizi. Il gruppo di Account del servizio gestito supporta gli host in cui vengono mantenuti offline per un periodo di tempo prolungato e la gestione di host membri per tutte le istanze di un servizio. Ciò significa che è possibile distribuire una server farm che supporta una singola identità a cui i computer client esistenti possono autenticarsi senza che sia nota l'istanza del servizio a cui si stanno connettendo.

Questi account non supportano i cluster di failover. Tuttavia, servizi eseguiti il servizio Cluster possono usare un gestito o un (autonomo) se sono un servizio di Windows, un pool di applicazioni, un'attività pianificata o supportano in modo nativo gestito o (autonomo).

## <a name="BKMK_SOFT"></a>Requisiti software

Un'architettura a 64 bit è necessario per eseguire i comandi di Windows PowerShell che vengono utilizzati per amministrare account del servizio gestito del gruppo.

Un account del servizio gestito è dipendente da Kerberos supportati i tipi di crittografia. Quando un computer client esegue l'autenticazione a un server utilizzando Kerberos, il controller di dominio crea un ticket di servizio Kerberos protetto con crittografia supporta controller di dominio e server. Il controller di dominio utilizza l'attributo msDS\-SupportedEncryptionTypes dell'account per determinare cosa crittografia supportato dal server e, se è presente alcun attributo, si presuppone che il computer client non supporta i tipi di crittografia più avanzati. Se l'host è configurato per non supportare la crittografia RC4, quindi l'autenticazione avrà sempre esito negativo. Per questo motivo, AES deve sempre essere configurato in modo esplicito account Microsoft.

> [!NOTE]
> A partire da Windows Server 2008 R2, la crittografia DES è disabilitata per impostazione predefinita. Per ulteriori informazioni sui tipi di crittografia supportati, vedere [modifiche all'autenticazione Kerberos](https://technet.microsoft.com/library/dd560670(WS.10).aspx).

Account del servizio gestito del gruppo non sono applicabili ai sistemi operativi Windows precedenti a Windows Server 2012.

## <a name="server-manager-information"></a>Informazioni su Server Manager
Sono necessari per implementare MSA e MSA del gruppo utilizzando Server Manager o il cmdlet Install\-WindowsFeature procedure di configurazione.

## <a name="BKMK_LINKS"></a>Vedere anche
Nella tabella seguente vengono forniti collegamenti a risorse aggiuntive relative agli account del servizio gestito e account del servizio gestito del gruppo.

|Tipo di contenuto|Riferimenti|
|--------|-------|
|**Valutazione del prodotto**|[Novità per gli account del servizio gestito](what-s-new-for-managed-service-accounts.md)<br /><br />[Account del servizio gestiti documentazione per Windows 7 e Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[Account del servizio Step\-by-Step Guide](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**Pianificazione**|Non è ancora disponibile|
|**Distribuzione**|Non è ancora disponibile|
|**Operazioni**|[Account del servizio in Active Directory gestiti](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**Risoluzione dei problemi**|Non è ancora disponibile|
|**Valutazione**|[Guida introduttiva a gruppo di account dei servizi gestiti](getting-started-with-group-managed-service-accounts.md)|
|**Strumenti e impostazioni**|[Account del servizio in servizi di dominio Active Directory gestiti](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**Risorse della community**|[Account dei servizi gestiti: Informazioni, implementazione, procedure consigliate e risoluzione dei problemi](http://blogs.technet.com/b/askds/archive/2009/09/10/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)|
|**Tecnologie correlate**|[Panoramica di servizi di dominio Active Directory](active-directory-domain-services-overview.md)|


