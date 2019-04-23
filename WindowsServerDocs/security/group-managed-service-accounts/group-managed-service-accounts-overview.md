---
title: Group Managed Service Accounts Overview
description: Sicurezza di Windows Server
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
ms.openlocfilehash: 24e3e3c15544de2f3bed4a7ef177b659e8095385
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836972"
---
# <a name="group-managed-service-accounts-overview"></a>Group Managed Service Accounts Overview

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento per i professionisti IT illustra l'Account del servizio gestito di gruppo applicazioni pratiche, le modifiche nell'implementazione di Microsoft e i requisiti hardware e software.


## <a name="BKMK_OVER"></a>Descrizione funzionalità
Autonomo (sMSA) Account del servizio gestito è un account di dominio gestito che offre gestione automatica delle password, gestione dei servizi semplificata del nome principale (SPN) e la possibilità di delegare la gestione ad altri amministratori. Questo tipo di account del servizio gestito (MSA) è stato introdotto in Windows Server 2008 R2 e Windows 7.

Il gruppo di Account del servizio gestito (gMSA) offre le stesse funzionalità all'interno del dominio ma estende tali funzionalità su più server. Quando ci si connette a un servizio ospitato in una farm di server, ad esempio soluzioni di rete con bilanciamento del carico, i protocolli di autenticazione che supportano l'autenticazione reciproca richiedono che tutte le istanze dei servizi utilizzino la stessa entità. Quando viene usato un account gMSA come entità servizio, il sistema operativo Windows gestisce la password dell'account anziché affidarsi all'amministratore di gestire la password.

Il servizio distribuzione chiavi Microsoft \(kdssvc. dll\) fornisce un meccanismo per ottenere in modo sicuro la chiave più recente o una chiave specifica con un identificatore di chiave per un account di Active Directory. Il Servizio distribuzione chiavi condivide un segreto che viene utilizzato per creare le chiavi per l'account. Tali chiavi vengono modificate periodicamente. Per un account gMSA il controller di dominio calcola la password sulla chiave fornita da servizio distribuzione chiavi, oltre agli altri attributi dei gMSA.  Gli host membri possono ottenere i valori di current e preceding password contattando un controller di dominio.

## <a name="BKMK_APP"></a>Applicazioni pratiche
account Gmsa offrono una soluzione unica identità per i servizi in esecuzione in una server farm o in sistemi ubicati dietro al bilanciamento del carico di rete. Fornendo una soluzione gMSA, i servizi possono essere configurati per gMSA nuove entità e la gestione delle password viene gestita da Windows.

Usando un gMSA, servizi o gli amministratori del servizio non sono necessario gestire la sincronizzazione password tra istanze del servizio. GMSA supporta gli host che rimangono disconnessi per un periodo di tempo prolungato e la gestione di host membri per tutte le istanze di un servizio. Di conseguenza, è possibile distribuire una server farm in grado di supportare una singola identità a cui tutti i computer client esistenti possono autenticarsi senza che sia nota l'istanza del servizio a cui si stanno connettendo.

I cluster di failover non supportano gli account del servizio gestiti del gruppo. Tuttavia, i servizi eseguiti sul servizio cluster possono usare un account del servizio gestito del gruppo o un account del servizi gestito (autonomo) se si tratta di un servizio Windows, di un pool di applicazioni, di un'attività pianificata o se supportano gli account del servizio gestito del gruppo o gli account del servizio gestiti (autonomi) a livello nativo.

## <a name="BKMK_SOFT"></a>Requisiti software

A 64\-architettura bit è necessario per eseguire i comandi di Windows PowerShell che vengono usati per amministrare account Gmsa.

Un account del servizio gestito dipende dai tipi di crittografia supportati da Kerberos. Quando un computer client esegue l'autenticazione a un server che utilizza Kerberos, il controller di dominio crea un ticket di servizio Kerberos con una crittografia supportata sia dal controller di dominio che dal server. Il controller di dominio utilizza msDS dell'account\-SupportedEncryptionTypes attributo per determinare quali supporta la crittografia di server e, se non esiste un attributo, si presuppone che il computer client non supporta i tipi di crittografia più avanzati. Se l'host è configurato per non supportare la crittografia RC4, quindi l'autenticazione avrà sempre esito negativo. Per questo motivo, per gli account del servizio gestiti è sempre consigliabile configurare la crittografia AES in modo esplicito.

> [!NOTE]
> A partire da Windows Server 2008 R2, la crittografia DES è disabilitata per impostazione predefinita. Per altre informazioni sui tipi di crittografia supportati, vedere [Modifiche all'autenticazione Kerberos](https://technet.microsoft.com/library/dd560670(WS.10).aspx).

Questi account non sono applicabili a sistemi operativi Windows precedenti a Windows Server 2012.

## <a name="server-manager-information"></a>Informazioni su Server Manager
Non sono previsti passaggi di configurazione necessari per implementare l'account del servizio gestito e gestito tramite Server Manager o l'installazione\-WindowsFeature cmdlet.

## <a name="BKMK_LINKS"></a>Vedere anche
Nella tabella seguente vengono riportati i collegamenti a risorse aggiuntive relative agli account del servizio gestiti e agli account del servizio gestiti del gruppo.

|Tipo di contenuto|Riferimenti|
|--------|-------|
|**Valutazione del prodotto**|[Quali sono le novità per gli account del servizio gestito](what-s-new-for-managed-service-accounts.md)<br /><br />[Documentazione per Windows 7 e Windows Server 2008 R2 account dei servizi gestiti](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[Passaggio degli account del servizio\-da\-Guida dettagliata](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**Pianificazione**|Non ancora disponibile|
|**Distribuzione**|Non ancora disponibile|
|**Operazioni**|[Account del servizio in Active Directory gestiti](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**Risoluzione dei problemi**|Non ancora disponibile|
|**Valutazione**|[Introduzione a gruppo di account del servizio gestiti](getting-started-with-group-managed-service-accounts.md)|
|**Strumenti e impostazioni**|[Account del servizio in Active Directory Domain Services gestiti](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**Risorse della community**|[Account del servizio gestiti: Informazioni, implementazione, procedure consigliate e risoluzione dei problemi](http://blogs.technet.com/b/askds/archive/2009/09/10/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)|
|**Tecnologie correlate**|[Panoramica di servizi di dominio Active Directory](active-directory-domain-services-overview.md)|


