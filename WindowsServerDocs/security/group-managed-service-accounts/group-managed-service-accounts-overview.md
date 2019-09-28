---
title: Group Managed Service Accounts Overview
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4e7f46739dd8def6ffc34c6cc50210c0e6999c79
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403748"
---
# <a name="group-managed-service-accounts-overview"></a>Group Managed Service Accounts Overview

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento per i professionisti IT introduce l'account del servizio gestito del gruppo descrivendo le applicazioni pratiche, le modifiche all'implementazione di Microsoft e i requisiti hardware e software.


## <a name="BKMK_OVER"></a>Descrizione della funzionalità
Un account del servizio gestito autonomo (sMSA) è un account di dominio gestito che offre la gestione automatica delle password, la gestione semplificata del nome dell'entità servizio (SPN) e la possibilità di delegare la gestione ad altri amministratori. Questo tipo di account del servizio gestito è stato introdotto in Windows Server 2008 R2 e Windows 7.

L'account del servizio gestito del gruppo (gMSA) fornisce le stesse funzionalità all'interno del dominio, ma estende anche tale funzionalità su più server. Quando ci si connette a un servizio ospitato in un server farm, ad esempio una soluzione di bilanciamento del carico di rete, i protocolli di autenticazione che supportano l'autenticazione reciproca richiedono che tutte le istanze dei servizi usino la stessa entità. Quando un gMSA viene usato come entità servizio, il sistema operativo Windows gestisce la password per l'account anziché affidarsi all'amministratore per la gestione della password.

Il servizio distribuzione chiavi Microsoft @no__t -0kdssvc. dll @ no__t-1 fornisce il meccanismo per ottenere in modo sicuro la chiave più recente o una chiave specifica con un identificatore di chiave per un account di Active Directory. Il Servizio distribuzione chiavi condivide un segreto che viene utilizzato per creare le chiavi per l'account. Tali chiavi vengono modificate periodicamente. Per un gMSA, il controller di dominio calcola la password sulla chiave fornita dai servizi di distribuzione delle chiavi, oltre ad altri attributi di gMSA.  Gli host membri possono ottenere i valori della password corrente e precedente contattando un controller di dominio.

## <a name="BKMK_APP"></a>Applicazioni pratiche
Servizi gestiti offrono una singola soluzione di gestione delle identità per i servizi in esecuzione in un server farm o sui sistemi dietro Load Balancer di rete. Fornendo una soluzione gMSA, i servizi possono essere configurati per la nuova entità gMSA e la gestione delle password viene gestita da Windows.

Con gMSA, i servizi o gli amministratori del servizio non devono gestire la sincronizzazione delle password tra le istanze del servizio. GMSA supporta gli host mantenuti offline per un periodo di tempo prolungato e la gestione di host membri per tutte le istanze di un servizio. Di conseguenza, è possibile distribuire una server farm in grado di supportare una singola identità a cui tutti i computer client esistenti possono autenticarsi senza che sia nota l'istanza del servizio a cui si stanno connettendo.

I cluster di failover non supportano gli account del servizio gestiti del gruppo. Tuttavia, i servizi eseguiti sul servizio cluster possono usare un account del servizio gestito del gruppo o un account del servizi gestito (autonomo) se si tratta di un servizio Windows, di un pool di applicazioni, di un'attività pianificata o se supportano gli account del servizio gestito del gruppo o gli account del servizio gestiti (autonomi) a livello nativo.

## <a name="BKMK_SOFT"></a>Requisiti software

Per eseguire i comandi di Windows PowerShell usati per amministrare servizi gestiti, è necessaria un'architettura di 64 @ no__t-0bit.

Un account del servizio gestito dipende dai tipi di crittografia supportati da Kerberos. Quando un computer client esegue l'autenticazione a un server che utilizza Kerberos, il controller di dominio crea un ticket di servizio Kerberos con una crittografia supportata sia dal controller di dominio che dal server. Il controller di dominio usa l'attributo msDS @ no__t-0SupportedEncryptionTypes dell'account per determinare la crittografia supportata dal server e, se non esiste alcun attributo, presuppone che il computer client non supporti i tipi di crittografia più avanzati. Se l'host è configurato per non supportare RC4, l'autenticazione avrà sempre esito negativo. Per questo motivo, per gli account del servizio gestiti è sempre consigliabile configurare la crittografia AES in modo esplicito.

> [!NOTE]
> A partire da Windows Server 2008 R2, la crittografia DES è disabilitata per impostazione predefinita. Per altre informazioni sui tipi di crittografia supportati, vedere [Modifiche all'autenticazione Kerberos](https://technet.microsoft.com/library/dd560670(WS.10).aspx).

Servizi gestiti non sono applicabili ai sistemi operativi Windows precedenti a Windows Server 2012.

## <a name="server-manager-information"></a>Informazioni su Server Manager
Non sono necessari passaggi di configurazione per implementare MSA e gMSA usando Server Manager o il cmdlet Install @ no__t-0WindowsFeature.

## <a name="BKMK_LINKS"></a>Vedere anche
Nella tabella seguente vengono riportati i collegamenti a risorse aggiuntive relative agli account del servizio gestiti e agli account del servizio gestiti del gruppo.

|Tipo di contenuto|Riferimenti|
|--------|-------|
|**Valutazione del prodotto**|[Novità per gli account dei servizi gestiti](what-s-new-for-managed-service-accounts.md)<br /><br />[Documentazione relativa agli account dei servizi gestiti per Windows 7 e Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[Service Accounts Step @ no__t-1di @ no__t-2Step guide](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**Pianificazione**|Non ancora disponibile|
|**Distribuzione**|Non ancora disponibile|
|**Operazioni**|[Account del servizio gestito in Active Directory](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**Risoluzione dei problemi**|Non ancora disponibile|
|**Valutazione**|[Guida introduttiva alla funzionalità Account del servizio gestito di gruppo](getting-started-with-group-managed-service-accounts.md)|
|**Strumenti e impostazioni**|[Account del servizio gestito in Active Directory Domain Services](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**Risorse della community**|Account del servizio [Managed: Informazioni, implementazione, procedure consigliate e risoluzione dei problemi @ no__t-0|
|**Tecnologie correlate**|[Panoramica di Active Directory Domain Services](active-directory-domain-services-overview.md)|


