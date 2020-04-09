---
title: Criteri di restrizione software
description: Sicurezza di Windows Server
ms.prod: windows-server
ms.technology: security-software-restriction-policies
ms.topic: article
ms.assetid: 5c0befad-07c3-4262-b418-372d01850305
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 905af608bcf5f43ea8883bc62d0344887c89baa7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855744"
---
# <a name="software-restriction-policies"></a>Criteri di restrizione software

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento per i professionisti IT descrive i criteri di restrizione software in Windows Server 2012 e Windows 8 e fornisce collegamenti a informazioni tecniche su SRP a partire da Windows Server 2003.

Per le procedure e i suggerimenti per la risoluzione dei problemi, vedere Amministrazione [dei](troubleshoot-software-restriction-policies.md) [criteri di restrizione software](administer-software-restriction-policies.md)

## <a name="software-restriction-policies-description"></a><a name="BKMK_OVER"></a>Descrizione di criteri di restrizione software
I criteri di restrizione software sono una funzionalità basata su Criteri di gruppo che identifica i programmi software in esecuzione nei computer di un dominio e controlla la possibilità di tali programmi di essere eseguiti. I criteri di restrizione software fanno parte della strategia di sicurezza e gestione Microsoft per aiutare le aziende ad aumentare l'affidabilità, l'integrità e la gestibilità dei propri computer.

È possibile usare i criteri di restrizione software anche per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate. I criteri di restrizione software sono integrati in Microsoft Active Directory e nei Criteri di gruppo. È anche possibile creare criteri di restrizione software in computer autonomi. I criteri di restrizione software sono criteri di attendibilità, ovvero normative configurate dall'amministratore per limitare gli script e altro codice la cui esecuzione non è completamente attendibile.

È possibile definire questi criteri con l'estensione Criteri di restrizione software dell'Editor Criteri di gruppo locali o con lo snap-in Criteri di sicurezza locali in Microsoft Management Console (MMC).

Per informazioni dettagliate su Criteri di restrizione software, vedere [Software Restriction Policies Technical Overview](software-restriction-policies-technical-overview.md).

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Applicazioni pratiche
Gli amministratori possono usare i criteri di restrizione software per le attività seguenti:

-   Definire il codice attendibile

-   Progettare Criteri di gruppo flessibili per definire script, file eseguibili e controlli ActiveX

I criteri di restrizione software vengono imposti dal sistema operativo e dalle applicazioni (ad esempio, le applicazioni di scripting) conformi ai criteri di restrizione software.

In particolare, gli amministratori possono usare i criteri di restrizione software per gli scopi seguenti:

-   Specificare il software (file eseguibili) che può essere eseguito nei client

-   Impedire agli utenti di eseguire determinati programmi nei computer condivisi

-   Specificare chi può aggiungere entità di pubblicazione attendibili ai client

-   Impostare l'ambito dei criteri di restrizione software (specificare se i criteri interessano tutti gli utenti o solo un sottoinsieme di utenti nei client)

-   Impedire l'esecuzione dei file eseguibili nel computer locale, nell'unità organizzativa, nel sito o nel dominio. Questa funzionalità è utile quando non si usano i criteri di restrizione software per risolvere i potenziali problemi relativi a utenti malintenzionati.

## <a name="new-and-changed-functionality"></a><a name="BKMK_NEW"></a>Funzionalità nuove e modificate
Non sono state apportate modifiche alla funzionalità di Criteri di restrizione software.

## <a name="removed-or-deprecated-functionality"></a><a name="BKMK_DEP"></a>Funzionalità rimosse o deprecate
Nessuna funzionalità di Criteri di restrizione software è stata rimossa o deprecata.

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisiti software
L'accesso all'estensione Criteri di restrizione software nell'Editor Criteri di gruppo locali può essere eseguito con MMC.

Le funzionalità seguenti sono necessarie per creare e gestire i criteri di restrizione software nel computer locale:

-   Editor Criteri di gruppo locali

-   Windows Installer

-   Authenticode e WinVerifyTrust

Se si progettano chiamate per la distribuzione di dominio di questi criteri, sono richieste le funzionalità seguenti, oltre a quelle elencate precedentemente:

-   Servizi di dominio Active Directory

-   Criteri di gruppo

## <a name="server-manager-information"></a><a name="BKMK_INSTALL"></a>Informazioni Server Manager
Criteri di restrizione software è un'estensione dell'Editor Criteri di gruppo locali e non viene installato da Server Manager, Aggiungi ruoli e funzionalità.

## <a name="see-also"></a><a name="BKMK_LINKS"></a>Vedere anche
La tabella seguente fornisce collegamenti a risorse utili per comprendere e usare Criteri di restrizione software.

|Tipo di contenuto|Riferimenti|
|--------|-------|
|**Valutazione del prodotto**|[Blocco dell'applicazione con criteri di restrizione software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|
|**Pianificazione**|[Panoramica tecnica di criteri di restrizione software](software-restriction-policies-technical-overview.md) (Windows Server 2012)<p>[Guida di riferimento tecnico di Criteri di restrizione software](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx) (Windows Server 2003)|
|**Distribuzione**|Nessuna risorsa disponibile.|
|**Operazioni**|[Amministrare i criteri di restrizione software](administer-software-restriction-policies.md) (Windows Server 2012)<p>[Guida del prodotto Criteri di restrizione software](https://technet.microsoft.com/library/cc779607(v=WS.10).aspx) (Windows Server 2003)|
|**Risoluzione dei problemi**|[Risolvere i problemi relativi ai criteri di restrizione software](troubleshoot-software-restriction-policies.md) (Windows Server 2012)<p>[Risoluzione dei problemi di Criteri di restrizione software](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx) (Windows Server 2003)|
|**Sicurezza**|[Minacce e contromisure per Criteri di restrizione software](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx) (Windows Server 2008)<p>[Minacce e contromisure per Criteri di restrizione software](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx) (Windows Server 2008 R2)|
|**Strumenti e impostazioni**|[Strumenti e impostazioni di Criteri di restrizione software](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx) (Windows Server 2003)|
|**Risorse della community**|[Blocco dell'applicazione con criteri di restrizione software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|



