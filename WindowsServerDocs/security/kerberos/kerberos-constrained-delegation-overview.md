---
title: Panoramica della delega vincolata Kerberos
description: Protezione di Windows Server
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
ms.openlocfilehash: e90aa5e395fa43a3f94cccb13444fd6991ac1074
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="kerberos-constrained-delegation-overview"></a>Panoramica della delega vincolata Kerberos

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questa panoramica per i professionisti IT descrive le nuove funzionalità per la delega vincolata Kerberos in Windows Server 2012 R2 e Windows Server 2012.

## <a name="feature-description"></a>Descrizione delle funzionalità
La delega vincolata Kerberos è stata introdotta in Windows Server 2003 per offrire un tipo di delega che potrebbe essere usato dai servizi più sicura. Quando viene configurato, la delega vincolata limita i servizi a cui il server specificato può agire per conto di un utente. Ciò richiede privilegi di amministratore di dominio per configurare un account di dominio per un servizio e limitare l'account a un singolo dominio. Nelle aziende attuali, servizi front-end non sono progettati per limitarsi all'integrazione con i soli servizi presenti nel medesimo dominio.

Nei sistemi operativi precedenti in cui l'amministratore di dominio configurato il servizio, l'amministratore del servizio non potevano utile sapere quali servizi front-end delegati a servizi relativi alle risorse che loro proprietà. E qualsiasi servizio front-end che può delegare a un servizio di risorsa rappresentato un potenziale punto di attacco. Se è stato compromesso un server che ospita un servizio front-end e che è stato configurato per delegare servizi relativi alle risorse, servizi relativi alle risorse anche potrebbero essere compromessa.

In Windows Server 2012 R2 e Windows Server 2012, possibilità di configurare la delega vincolata per il servizio è stata trasferita dall'amministratore del dominio all'amministratore del servizio. In questo modo, l'amministratore del servizio back-end può consentire o negare i servizi front-end.

Per informazioni dettagliate sulla delega vincolata forma introdotta in Windows Server 2003, vedere [transizione al protocollo Kerberos e delega vincolata](https://technet.microsoft.com/library/cc739587(v=ws.10)).

L'implementazione di Windows Server 2012 R2 e Windows Server 2012 del protocollo Kerberos include estensioni specifiche per la delega vincolata.  Il servizio per User to Proxy (S4U2Proxy) consente a un servizio di usare il ticket di servizio Kerberos per un utente ottenga un ticket di servizio dal centro distribuzione chiavi (KDC) a un servizio back-end. Queste estensioni consentono la delega vincolata deve essere configurata nell'account del servizio back-end, che può essere in un altro dominio. Per ulteriori informazioni su queste estensioni, vedere [\[MS-SFU\]: estensioni del protocollo Kerberos: il servizio per utenti e specifica del protocollo di delega vincolata](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx) in MSDN Library.

## <a name="practical-applications"></a>Applicazioni pratiche
La delega vincolata offre agli amministratori di servizi la possibilità di specificare e applicare limiti di trust per applicazioni, limitando l'ambito in cui i servizi delle applicazioni possono agire per conto dell'utente. Gli amministratori del servizio è possono configurare quali gli account del servizio front-end possono delegare ai servizi back-end.

Grazie al supporto della delega vincolata tra domini in Windows Server 2012 R2 e Windows Server 2012, servizi front-end come Microsoft Internet Security e Acceleration (ISA) Server, Microsoft Forefront Threat Management Gateway, Microsoft Exchange Outlook Web Access (OWA) e Microsoft SharePoint Server possono essere configurati per utilizzare la delega vincolata per l'autenticazione nei server in altri domini. Questo offre supporto per domini soluzioni di servizio tramite un'infrastruttura Kerberos esistente. La delega vincolata Kerberos può essere gestita dagli amministratori del dominio o gli amministratori del servizio.

## <a name="new-and-changed-functionality"></a>Funzionalità nuove e modificate
**Delega vincolata basata su risorse tra domini**

La delega vincolata Kerberos può essere utilizzata per fornire la delega vincolata quando il servizio front-end e servizi relativi alle risorse non si trovano nello stesso dominio. Gli amministratori del servizio sono in grado di configurare la nuova delega specificando gli account di dominio dei servizi front-end che possono rappresentare gli utenti di oggetti account dei servizi relativi alle risorse.

**Valore aggiunto da queste modifiche?**

Grazie al supporto della delega vincolata tra domini, i servizi è configurabile per usare la delega vincolata per l'autenticazione nei server in altri domini, anziché usare la delega vincolata. Ciò fornisce supporto dell'autenticazione per tra soluzioni di servizi di dominio utilizzando un'infrastruttura Kerberos esistente senza la necessità di considerare attendibili i servizi front-end per la delega di qualsiasi servizio.

**Differenze di funzionamento**

Una modifica nel protocollo sottostante consente la delega vincolata tra domini. L'implementazione di Windows Server 2012 R2 e Windows Server 2012 del protocollo Kerberos include estensioni per Service for User to protocollo Proxy (S4U2Proxy). Questo è un set di estensioni del protocollo Kerberos che consente a un servizio di usare il ticket di servizio Kerberos per un utente ottenga un ticket di servizio dal centro distribuzione chiavi (KDC) a un servizio back-end.

Per informazioni sull'implementazione su queste estensioni, vedere [\[MS-SFU\]: estensioni del protocollo Kerberos: il servizio per utenti e specifica del protocollo di delega vincolata](https://msdn.microsoft.com/library/cc246071(PROT.10).aspx) in MSDN.

Per ulteriori informazioni sulla sequenza di messaggi di base per la delega Kerberos con un ticket di concessione ticket (TGT) rispetto al servizio per estensioni User (S4U), vedere sezione [1.3.3 Panoramica del protocollo](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx) nel documento [MS-SFU]: estensioni del protocollo Kerberos: il servizio per utenti e specifica del protocollo di delega vincolata.

Per configurare un servizio di risorsa per consentire l'accesso a un servizio front-end per conto degli utenti, utilizzare i cmdlet di Windows PowerShell.

-   Per recuperare un elenco di entità, utilizzare il **Get-ADComputer**, **Get-ADServiceAccount**, e **Get-ADUser** cmdlet con il **Properties PrincipalsAllowedToDelegateToAccount** parametro.

-   Per configurare il servizio di risorsa, usare il **New-ADComputer**, **New-ADServiceAccount**, **New-ADUser**, **Set-ADComputer**, **Set-ADServiceAccount**, e **Set-ADUser** cmdlet con il **PrincipalsAllowedToDelegateToAccount** parametro.

## <a name="BKMK_SOFT"></a>Requisiti software
La delega vincolata basata sulle risorse può essere configurata solo in un controller di dominio che eseguono Windows Server 2012 R2 e Windows Server 2012, ma può essere applicata in una foresta in modalità mista.

È necessario applicare l'hotfix seguente a tutti i controller di dominio che esegue Windows Server 2012 utente domini di account nel percorso di riferimento tra i domini front-end e back-end che eseguono sistemi operativi precedenti a Windows Server: errore KDC_ERR_POLICY in ambienti con controller di dominio basati su Windows Server 2008 R2 delega vincolata basata su risorse.
