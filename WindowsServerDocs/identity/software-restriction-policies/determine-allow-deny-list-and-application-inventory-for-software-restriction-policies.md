---
title: Determinare l'elenco Consenti-Nega e l'inventario delle applicazioni per i criteri di restrizione software
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0abb73b6-b5d8-4505-8ab1-2f29e4bf0411
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 60e78912284715649938567d66ffb90b9890b1b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847292"
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>Determinare l'elenco Consenti-Nega e l'inventario delle applicazioni per i criteri di restrizione software

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento destinato ai professionisti IT fornisce indicazioni creare un Consenti e Nega elenco per le applicazioni da gestire con inizio criteri restrizione Software (SRP) con Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introduzione
I criteri di restrizione software sono una funzionalità basata su Criteri di gruppo che identifica i programmi software in esecuzione nei computer di un dominio e controlla la possibilità di tali programmi di essere eseguiti. È possibile utilizzare i criteri di restrizione software per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate. Questi sono integrati con Microsoft Active Directory Domain Services e criteri di gruppo, ma può anche essere configurati in computer autonomi. Per un punto di partenza per criteri di restrizione software, vedere la [criteri di restrizione Software](software-restriction-policies.md).

A partire da Windows Server 2008 R2 e Windows 7, Windows AppLocker utilizzabile al posto di o in concerto con criteri di restrizione software per una parte della strategia di controllo dell'applicazione.

Per informazioni su come eseguire attività specifiche utilizzando criteri di restrizione software, vedere gli argomenti seguenti:

-   [Lavorare con le regole di criteri di restrizione Software](work-with-software-restriction-policies-rules.md)

-   [Usare i criteri di restrizione Software per proteggere il Computer da un Virus di posta elettronica](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>Scegliere quali regola predefinita: Consentire o negare
Criteri di restrizione software possono essere distribuiti in una delle due modalità che costituiscono la base della regola predefinita: Elenco Consenti o Nega elenco. È possibile creare un criterio che identifica tutte le applicazioni che può essere eseguita nell'ambiente in uso; la regola predefinita all'interno del criterio è limitato e verrà bloccare tutte le applicazioni che non si consente in modo esplicito per l'esecuzione. Oppure è possibile creare un criterio che identifica tutte le applicazioni che non è possibile eseguire; la regola predefinita è senza restrizioni e limita solo le applicazioni che siano stati elencati anche in modo esplicito.

> [!IMPORTANT]
> La modalità elenco potrebbe essere una strategia di maggiore manutenzione dell'organizzazione relative al controllo delle applicazioni. Creare e gestire un elenco in continua evoluzione che impedisce a tutti i malware e altre applicazioni problematici sarebbe molto tempo e soggette a errori.

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>Creare un inventario delle applicazioni per l'elenco Consenti
Per usare in modo efficace la regola di assenso predefinito, è necessario determinare esattamente quali applicazioni sono necessari nell'organizzazione. Sono disponibili gli strumenti progettati per produrre un inventario delle applicazioni, ad esempio l'agente di raccolta inventario in Microsoft Application Compatibility Toolkit. Tuttavia, criteri di restrizione software è una funzionalità di registrazione avanzate che consentono di comprendere esattamente quali applicazioni sono in esecuzione nell'ambiente in uso.

##### <a name="to-discover-which-applications-to-allow"></a>Per individuare le applicazioni da consentire

1.  In un ambiente di test, distribuire i criteri di restrizione Software con il set di regole predefinito su senza restrizioni e rimuovere tutte le regole aggiuntive. Se si abilita criteri di restrizione software senza forzarlo per limitare tutte le applicazioni, SPR sarà in grado di controllare quali applicazioni vengono eseguiti.

2.  Creare il valore del Registro di sistema seguente per abilitare la funzionalità di registrazione avanzate e impostare il percorso in cui deve essere scritto il file di log.

    **"HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Safer\ CodeIdentifiers"**

    Valore stringa: *Percorso NameLogFile NameLogFile*

    Poiché i criteri di restrizione software è la valutazione di tutte le applicazioni durante l'esecuzione, nel file di log viene scritta una voce *NameLogFile* ogni volta che viene eseguita l'applicazione.

3.  Valutare il file di log

    Ogni voce di log quanto segue:

    -   il chiamante di criteri di restrizione software e l'ID processo (PID) del processo chiamante

    -   la destinazione valutata

    -   la regola di restrizione software che è stata rilevata durante l'esecuzione dell'applicazione

    -   un identificatore per la regola di restrizione software.

    Un esempio dell'output scritto in un file di log:

**Explorer.exe (PID = 4728) identifiedC:\Windows\system32\onenote.exe quello regola usingpath senza restrizioni, Guid = {320bd852-aa7c-4674-82c5-9a80321670a3}** tutte le applicazioni e codice associato che Criteri di restrizione software controlla e impostato per bloccare è indicati nel registro file che è quindi possibile usare per determinare quali file eseguibili da considerare per l'elenco consentiti.


