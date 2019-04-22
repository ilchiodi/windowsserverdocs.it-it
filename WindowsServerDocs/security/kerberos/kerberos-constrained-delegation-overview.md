---
title: Kerberos Constrained Delegation Overview
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51923b0a-0c1a-47b2-93a0-d36f8e295589
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: d77dd6f6f310ae71a4d9e2d52b44cc9b1bef6401
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821932"
---
# <a name="kerberos-constrained-delegation-overview"></a>Kerberos Constrained Delegation Overview

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento di panoramica per i professionisti IT descrive le nuove funzionalità per la delega vincolata Kerberos in Windows Server 2012 R2 e Windows Server 2012.

**Descrizione funzionalità**

La delega vincolata Kerberos è stata introdotta in Windows Server 2003 per offrire un tipo di delega più sicura utilizzabile dai servizi. Dopo la configurazione, la delega vincolata limita i servizi su cui può agire il server specificato per conto di un utente. Sono necessari privilegi di amministratore del dominio per configurare un account di dominio per un servizio e limitare l'account a un solo dominio. Nelle aziende attuali, servizi front-end non sono progettati per limitarsi all'integrazione con i soli servizi presenti nel medesimo dominio.

Nei sistemi operativi precedenti, in cui il servizio è configurato dall'amministratore del dominio, l'amministratore del servizio non dispone di un modo utile per scoprire quali servizi front-end sono delegati ai servizi relativi alle risorse di loro proprietà. Qualsiasi servizio front-end che può delegare a un servizio relativo alle risorse rappresenta quindi un potenziale punto di attacco. In caso di compromissione di un server che ospita un servizio front-end e se il server è configurato per delegare i servizi relativi alle risorse, anche questi servizi potrebbero risultare compromessi.

In Windows Server 2012 R2 e Windows Server 2012, possibilità di configurare la delega vincolata per il servizio è stata trasferita dall'amministratore di dominio all'amministratore del servizio. In questo modo, l'amministratore del servizio back-end può consentire o negare i servizi front-end.

Per informazioni dettagliate sulla delega vincolata nella forma introdotta in Windows Server 2003, vedere la pagina relativa alla [transizione al protocollo Kerberos e la delega vincolata](https://technet.microsoft.com/library/cc739587(v=ws.10)).

L'implementazione di Windows Server 2012 R2 e Windows Server 2012 del protocollo Kerberos include estensioni specifiche per la delega vincolata.  L'estensione S4U2Proxy (Service for User to Proxy) consente a un servizio di usare il ticket del servizio Kerberos per fare in modo che un utente ottenga un ticket di servizio dal Centro distribuzione chiavi (KDC) a un servizio back-end. Queste estensioni consentono la delega vincolata essere configurate per l'account del servizio back-end, che può essere in un altro dominio. Per altre informazioni su queste estensioni, vedere [ \[MS-SFU\]: estensioni del protocollo Kerberos: Servizio per utente e la specifica del protocollo di delega vincolata](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx) in MSDN Library.

**Applicazioni pratiche**

La delega vincolata offre agli amministratori di servizi la possibilità di specificare e applicare limiti di attendibilità dell'applicazione, limitando l'ambito in cui i servizi delle applicazioni possono agire per conto dell'utente. Gli amministratori del servizio possono configurare gli account di servizio front-end che possono delegare ai servizi back-end.

Dal supporto della delega vincolata tra domini in Windows Server 2012 R2 e Windows Server 2012, servizi front-end come Microsoft Internet Security and Acceleration (ISA) Server, Microsoft Forefront Threat Management Gateway, Microsoft Exchange Outlook Web Access (OWA) e Microsoft SharePoint Server può essere configurato per usare la delega vincolata per l'autenticazione nei server in altri domini. Vengono in tal modo supportate soluzioni di servizio tra dominio tramite un'infrastruttura Kerberos esistente. La delega vincolata Kerberos può essere gestita dagli amministratori di dominio o dagli amministratori di servizio.

## <a name="resource-based-constrained-delegation-across-domains"></a>Delega vincolata basata su risorse tra domini

È possibile usare la delega vincolata Kerberos per offrire la delega vincolata quando il servizio front-end e i servizi relativi alle risorse non si trovano nello stesso dominio. Gli amministratori del servizio possono configurare la nuova delega specificando gli account di dominio dei servizi front-end che possono rappresentare gli utenti negli oggetti account dei servizi relativi alle risorse.

**Valore aggiunto da queste modifiche**

Tramite il supporto della delega vincolata tra domini, i servizi possono essere configurati per l'utilizzo della delega vincolata per l'autenticazione nei server in altri domini, anziché usare la delega vincolata. Ciò rende disponibile il supporto dell'autenticazione per le soluzioni di servizio tra domini usando un'infrastruttura Kerberos esistente, senza dover stabilire una relazione di trust con i servizi front-end per la delega di qualsiasi servizio.

Questo passa anche la decisione del fatto che un server deve considerare attendibile l'origine di un'identità delegata da parte dell'amministratore di dominio da delega al proprietario della risorsa.

**Differenze di funzionamento**

Una modifica nel protocollo sottostante consente la delega vincolata tra domini. L'implementazione di Windows Server 2012 R2 e Windows Server 2012 del protocollo Kerberos include estensioni per Service for User to protocollo Proxy (S4U2Proxy). Si tratta di un set di estensioni per il protocollo Kerberos che consente a un servizio di usare il ticket del servizio Kerberos per fare in modo che un utente ottenga un ticket di servizio dal Centro distribuzione chiavi (KDC) a un servizio back-end.

Per informazioni sull'implementazione su queste estensioni, vedere [ \[MS-SFU\]: estensioni del protocollo Kerberos: Servizio per utente e la specifica del protocollo di delega vincolata](https://msdn.microsoft.com/library/cc246071(PROT.10).aspx) in MSDN.

Per altre informazioni sulla sequenza di messaggi di base per la delega Kerberos con un ticket di concessione ticket (TGT) inoltrato a confronto con le estensioni S4U (Service for User), vedere la sezione [1.3.3 della panoramica del protocollo](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx) nel documento [MS-SFU] sulle estensioni del protocollo Kerberos: specifica del protocollo per il servizio per utenti e la delega vincolata.

**Implicazioni di sicurezza della delega vincolata basata sulle risorse**

Basata sulle risorse vincolate controllo inserisce la delega di delega nelle mani dell'amministratore proprietario della risorsa a cui si accede. A seconda degli attributi del servizio di risorse anziché il servizio verrà considerato attendibile per delegare. Di conseguenza, la delega vincolata basata sulle risorse non è possibile usare il bit attendibile-a-eseguire l'autenticazione-per-delega controllate in precedenza la transizione di protocollo. Il KDC consente sempre la transizione di protocollo durante l'esecuzione basata su risorse per la delega vincolata come se fosse impostato il bit.

Poiché il KDC non limita la transizione di protocollo, sono stati introdotti due nuovi SID noto per fornire questo controllo per l'amministratore delle risorse.  I SID identificare se si è verificata la transizione di protocollo e utilizzabile con elenchi di controllo di accesso standard per consentire o limitare l'accesso in base alle esigenze.

|SID|Descrizione|
|-------|--------|
|AUTHENTICATION_AUTHORITY_ASSERTED_IDENTITY<br />S-1-18-1|Un SID che significa che l'identità del client viene dichiarato da un'autorità di autenticazione basata su prova di possesso delle credenziali client.|
|SERVICE_ASSERTED_IDENTITY<br />S-1-18-2|Un SID che significa che l'identità del client viene dichiarato da un servizio.|

Un servizio back-end è possibile usare espressioni standard di ACL per determinare la modalità in cui è stato autenticato l'utente.

**Come si configura la delega vincolata basata sulle risorse?**

Per configurare un servizio relativo alle risorse per consentire l'accesso a un servizio front-end per conto degli utenti, usare i cmdlet di Windows PowerShell.

-   Per recuperare un elenco di entità, usare il **Get-ADComputer**, **Get-ADServiceAccount**, e **Get-ADUser** cmdlet con il **proprietà PrincipalsAllowedToDelegateToAccount** parametro.

-   Per configurare il servizio di risorse, usare il **New-ADComputer**, **New-ADServiceAccount**, **New-ADUser**, **Set-ADComputer**,  **Set-ADServiceAccount**, e **Set-ADUser** cmdlet con il **PrincipalsAllowedToDelegateToAccount** parametro.

## <a name="BKMK_SOFT"></a>Requisiti software
La delega vincolata basata sulle risorse può essere configurata solo in un controller di dominio che eseguono Windows Server 2012 R2 e Windows Server 2012, ma può essere applicata in una foresta in modalità mista.

È necessario applicare l'hotfix seguente a tutti i controller di dominio che eseguono Windows Server 2012 in utente domini di account nel percorso di riferimento tra i domini front-end e back-end che eseguono sistemi operativi precedenti a Windows Server:  Errore KDC_ERR_POLICY per la delega vincolata basata sulle risorse in ambienti con controller di dominio basati su Windows Server 2008 R2.
