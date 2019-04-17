---
title: Determinazione dell'elenco Consenti-Nega e l'inventario delle applicazioni per criteri di restrizione Software
description: Protezione di Windows Server
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>Determinazione dell'elenco Consenti-Nega e l'inventario delle applicazioni per criteri di restrizione Software

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento, destinato ai professionisti IT fornisce istruzioni come creare un Consenti e Nega elenco per le applicazioni da gestire con inizio Software criteri di restrizione con Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introduzione
Software di criteri di restrizione è una funzionalità basata su criteri di gruppo che identifica i programmi software in esecuzione nel computer in un dominio e controlla la possibilità di eseguire tali programmi. Utilizzare criteri di restrizione software per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate eseguire. Questi sono integrati in servizi di dominio Microsoft Active Directory e criteri di gruppo, ma possono anche essere configurati in computer autonomi. Per un punto di partenza per criteri di restrizione software, vedere il [criteri di restrizione Software](software-restriction-policies.md).

A partire da Windows Server 2008 R2 e Windows 7, Windows AppLocker può essere utilizzato invece di o in combinazione con criteri di restrizione software per una parte della strategia di controllo dell'applicazione.

Per informazioni su come eseguire attività specifiche utilizzando criteri di restrizione software, vedere gli argomenti seguenti:

-   [Utilizzo delle regole dei criteri di restrizione Software](work-with-software-restriction-policies-rules.md)

-   [Utilizzare criteri di restrizione Software per proteggere il Computer da un Virus di posta elettronica](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>La regola predefinita per scegliere: consentire o negare
Criteri di restrizione software possono essere distribuiti in una delle due modalità che costituiscono la base della regola predefinita: elenco Consenti o Nega. È possibile creare un criterio che identifica ogni applicazione può essere eseguita nell'ambiente; la regola predefinita all'interno del criterio è Restricted e verrà bloccare tutte le applicazioni che non si consente in modo esplicito per l'esecuzione. Oppure è possibile creare un criterio che identifica ogni applicazione che non possa eseguire. la regola predefinita è senza restrizioni e limita solo le applicazioni che sono stati elencati in modo esplicito.

> [!IMPORTANT]
> La modalità di negare l'elenco potrebbe essere una strategia di manutenzione di elevato dell'organizzazione relative al controllo delle applicazioni. Creazione e gestione di un elenco in continua evoluzione che impedisce a tutti i malware e altre applicazioni problematiche sarebbe molto tempo e soggetta a errori.

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>Creare un inventario delle applicazioni per un elenco Consenti
Per utilizzare al meglio la regola predefinita Consenti, è necessario determinare esattamente quali applicazioni sono necessarie nell'organizzazione. Sono disponibili strumenti progettati per produrre un inventario delle applicazioni, ad esempio l'agente di raccolta inventario in Microsoft Application Compatibility Toolkit. Ma criteri di restrizione software è una funzionalità di registrazione avanzata per aiutarti a comprendere esattamente quali applicazioni sono in esecuzione nel proprio ambiente.

##### <a name="to-discover-which-applications-to-allow"></a>Per individuare le applicazioni da consentire

1.  In un ambiente di test, distribuire i criteri di restrizione Software con il set di regole predefinite per senza restrizioni e rimuovere tutte le regole aggiuntive. Se si abilita criteri di restrizione software senza imporre per limitare le applicazioni, SPR sarà in grado di controllare quali applicazioni vengono eseguiti.

2.  Creare il seguente valore del Registro di sistema per abilitare la funzionalità di registrazione avanzata e impostare il percorso in cui il file di registro deve essere scritti.

    **"HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Safer\ CodeIdentifiers"**

    Il valore di stringa: *percorso NameLogFile NameLogFile*

    Poiché i criteri di restrizione software sta valutando tutte le applicazioni durante l'esecuzione, viene scritta una voce nel file di log *NameLogFile* ogni volta che viene eseguita l'applicazione.

3.  Valutare il file di registro

    Ogni voce del registro quanto segue:

    -   Il chiamante di criteri di restrizione software e l'ID processo (PID) del processo chiamante

    -   La destinazione in fase di valutazione

    -   La regola di restrizione software che si è verificata durante l'esecuzione dell'applicazione

    -   Un identificatore per la regola di restrizione software.

    Un esempio di output scritti in un file di registro:

**Explorer.exe (PID = 4728) identifiedC:\Windows\system32\onenote.exe come regola usingpath senza restrizioni, Guid = {320bd852-aa7c-4674-82c5-9a80321670a3}** tutte le applicazioni e codice associato che Criteri di restrizione software controlla e impostato per bloccare verrà indicati nel file di registro, è quindi possibile utilizzare per determinare quali file eseguibili devono essere considerati per l'elenco consentito.


