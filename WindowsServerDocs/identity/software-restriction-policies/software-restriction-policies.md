---
title: Criteri di restrizione software
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c0befad-07c3-4262-b418-372d01850305
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: ab44013b947d33adc12c54b527415bf16c46a4c6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="software-restriction-policies"></a>Criteri di restrizione software

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento, destinato ai professionisti IT descrive criteri (restrizione Software) in Windows Server 2012 e Windows 8 e vengono forniti collegamenti alle informazioni tecniche su inizio criteri di restrizione software con Windows Server 2003.

Per procedure e suggerimenti sulla risoluzione dei problemi, vedere [amministrare i criteri di restrizione Software](administer-software-restriction-policies.md) e [risoluzione dei problemi di criteri di restrizione Software](troubleshoot-software-restriction-policies.md).

## <a name="BKMK_OVER"></a>Descrizione di criteri di restrizione software
Software di criteri di restrizione è una funzionalità basata su criteri di gruppo che identifica i programmi software in esecuzione nel computer in un dominio e controlla la possibilità di eseguire tali programmi. Criteri di restrizione software fanno parte di Microsoft security strategia di gestione e per aiutare le aziende ad aumentare l'affidabilità, integrità e la gestibilità dei computer.

È inoltre possibile utilizzare criteri di restrizione software per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate eseguire. Criteri di restrizione software sono integrati in Microsoft Active Directory e criteri di gruppo. È inoltre possibile creare criteri di restrizione software in computer autonomi. Criteri di restrizione software sono criteri di attendibilità, ovvero normative configurate dall'amministratore per limitare gli script e altro codice che non è completamente attendibile in esecuzione.

È possibile definire questi criteri con l'estensione di criteri di restrizione Software dell'Editor criteri di gruppo locali o lo snap-in Criteri di sicurezza locali di Microsoft Management Console (MMC).

Per informazioni dettagliate su criteri di restrizione software, vedere il [Software Restriction Policies Technical Overview](software-restriction-policies-technical-overview.md).

## <a name="BKMK_APP"></a>Applicazioni pratiche
Gli amministratori possono utilizzare criteri di restrizione software per le attività seguenti:

-   Definire il codice attendibile

-   Progettazione di criteri di gruppo flessibili per definire script, file eseguibili e controlli ActiveX

Criteri di restrizione software vengono applicati dal sistema operativo e dalle applicazioni (ad esempio applicazioni di scripting) conformi ai criteri di restrizione software.

In particolare, gli amministratori possono utilizzare criteri di restrizione software per gli scopi seguenti:

-   Specificare il tipo di software (file eseguibili) possono essere eseguiti nei client

-   Impedire agli utenti di eseguire determinati programmi nei computer condivisi

-   Specificare chi può aggiungere autori attendibili ai client

-   Impostare l'ambito dei criteri di restrizione software (specificare se i criteri interessano tutti gli utenti o solo un sottoinsieme di utenti nei client)

-   Impedire che i file eseguibili in esecuzione nel computer locale, unità organizzativa (OU), sito o dominio. Questa funzionalità è utile quando non si utilizza criteri di restrizione software per risolvere i problemi potenziali a utenti malintenzionati.

## <a name="BKMK_NEW"></a>Funzionalità nuove e modificate
Sono state apportate modifiche delle funzionalità di criteri restrizione Software.

## <a name="BKMK_DEP"></a>Funzionalità rimosse o deprecate
Non esiste alcuna funzionalità rimosse o deprecate per criteri di restrizione Software.

## <a name="BKMK_SOFT"></a>Requisiti software
L'estensione di criteri di restrizione Software a Editor criteri di gruppo locali sono accessibili tramite la console MMC.

Per creare e gestire criteri di restrizione software nel computer locale sono necessarie le seguenti funzionalità:

-   Editor criteri di gruppo locali

-   Windows Installer

-   Authenticode e WinVerifyTrust

Se si progettano chiamate per la distribuzione di dominio di questi criteri, oltre a sopra elencati, sono necessarie le seguenti funzionalità:

-   Servizi di dominio Active Directory

-   Criteri di gruppo

## <a name="BKMK_INSTALL"></a>Informazioni su Server Manager
Criteri di restrizione software è un'estensione dell'Editor criteri di gruppo locali e non viene installato tramite Server Manager, Aggiungi ruoli e funzionalità.

## <a name="BKMK_LINKS"></a>Vedere anche
Nella tabella seguente vengono forniti collegamenti a risorse utili per comprendere e usare criteri di restrizione software.

|Tipo di contenuto|Riferimenti|
|--------|-------|
|**Valutazione del prodotto**|[Blocco dell'applicazione con criteri di restrizione Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|
|**Pianificazione**|[Software Restriction Policies Technical Overview](software-restriction-policies-technical-overview.md) (Windows Server 2012)<br /><br />[Riferimento tecnico criteri di restrizione software](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx) (Windows Server 2003)|
|**Distribuzione**|Nessuna risorsa disponibile.|
|**Operazioni**|[Amministrare i criteri di restrizione Software](administer-software-restriction-policies.md) (Windows Server 2012)<br /><br />[Guida del prodotto criteri restrizione software](https://technet.microsoft.com/library/cc779607(v=WS.10).aspx) (Windows Server 2003)|
|**Risoluzione dei problemi**|[Risolvere i problemi relativi a criteri di restrizione Software](troubleshoot-software-restriction-policies.md) (Windows Server 2012)<br /><br />[Risoluzione dei problemi di criteri di restrizione software](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx) (Windows Server 2003)|
|**Protezione**|[Minacce e contromisure per restrizione Software criteri](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx) (Windows Server 2008)<br /><br />[Minacce e contromisure per restrizione Software criteri](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx) (Windows Server 2008 R2)|
|**Strumenti e impostazioni**|[Criteri di restrizione software strumenti e impostazioni](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx) (Windows Server 2003)|
|**Risorse della community**|[Blocco dell'applicazione con criteri di restrizione Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|



