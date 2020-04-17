---
title: Determinare l'elenco Consenti-Nega e l'inventario delle applicazioni per i criteri di restrizione software
description: Sicurezza di Windows Server
ms.prod: windows-server
ms.technology: security-software-restriction-policies
ms.topic: article
ms.assetid: 0abb73b6-b5d8-4505-8ab1-2f29e4bf0411
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: c0fdb5c1d7c4b03610a173c6cd0575d39646a7d0
ms.sourcegitcommit: af1cf89632d62a94943d3ad9f6b5234b88499278
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2020
ms.locfileid: "81524906"
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>Determinare l'elenco Consenti-Nega e l'inventario delle applicazioni per i criteri di restrizione software

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento per i professionisti IT fornisce indicazioni su come creare un elenco di accesso consentito e negato per la gestione delle applicazioni da parte di criteri di restrizione software a partire da Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introduzione
I criteri di restrizione software sono una funzionalità basata su Criteri di gruppo che identifica i programmi software in esecuzione nei computer di un dominio e controlla la possibilità di tali programmi di essere eseguiti. È possibile utilizzare i criteri di restrizione software per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate. Queste sono integrate con Microsoft Active Directory Domain Services e Criteri di gruppo ma possono essere configurate anche nei computer autonomi. Per un punto di partenza per SRP, vedere [criteri di restrizione software](software-restriction-policies.md).

A partire da Windows Server 2008 R2 e Windows 7, è possibile utilizzare Windows AppLocker anziché o in concerto con SRP per una parte della strategia di controllo delle applicazioni.

Per informazioni su come eseguire attività specifiche utilizzando il software di RESTRIzione software, vedere gli argomenti seguenti:

-   [Usare le regole dei criteri di restrizione software](work-with-software-restriction-policies-rules.md)

-   [Usare i criteri di restrizione software per proteggere il computer da un virus di posta elettronica](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>Quale regola predefinita scegliere: Consenti o nega
I criteri di restrizione software possono essere distribuiti in una delle due modalità che costituiscono la base della regola predefinita: Consenti elenco o nega elenco. È possibile creare un criterio che identifichi tutte le applicazioni consentite per l'esecuzione nell'ambiente in uso. la regola predefinita nel criterio è limitata e blocca tutte le applicazioni che non sono consentite in modo esplicito per l'esecuzione. In alternativa, è possibile creare un criterio che identifichi tutte le applicazioni che non possono essere eseguite; la regola predefinita è senza restrizioni e limita solo le applicazioni elencate in modo esplicito.

> [!IMPORTANT]
> La modalità elenco di negazione potrebbe essere una strategia di manutenzione elevata per la propria organizzazione in merito al controllo delle applicazioni. La creazione e la gestione di un elenco in continua evoluzione che impedisce a tutti i malware e ad altre applicazioni problematiche potrebbero richiedere molto tempo ed essere suscettibili agli errori.

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>Creare un inventario delle applicazioni per l'elenco Consenti
Per usare in modo efficace la regola Consenti impostazioni predefinite, è necessario determinare esattamente quali applicazioni sono necessarie nell'organizzazione. Sono disponibili strumenti progettati per produrre un inventario delle applicazioni, ad esempio Inventory Collector in Microsoft Application Compatibility Toolkit. Tuttavia, il servizio SRP dispone di una funzionalità di registrazione avanzata che consente di comprendere esattamente quali applicazioni sono in esecuzione nell'ambiente.

##### <a name="to-discover-which-applications-to-allow"></a>Per individuare le applicazioni da consentire

1.  In un ambiente di test, distribuire i criteri di restrizione software con la regola predefinita impostata su senza restrizioni e rimuovere eventuali regole aggiuntive. Se si Abilita SRP senza forzarlo a limitare le applicazioni, SPR sarà in grado di monitorare le applicazioni in esecuzione.

2.  Creare il seguente valore del registro di sistema per abilitare la funzionalità di registrazione avanzata e impostare il percorso in cui deve essere scritto il file di log.

    **"HKEY_LOCAL_MACHINE \SOFTWARE\Policies\Microsoft\Windows\Safer\CodeIdentifiers"**

    Valore stringa: *LogFileName percorso di LogFileName*

    Poiché il SRP sta valutando tutte le applicazioni quando vengono eseguite, una voce viene scritta nel file di log *NameLogFile* ogni volta che viene eseguita l'applicazione.

3.  Valutare il file di log

    Ogni voce di log indica:

    -   il chiamante dei criteri di restrizione software e l'ID del processo (PID) del processo chiamante

    -   destinazione valutata

    -   regola SRP che è stata rilevata durante l'esecuzione dell'applicazione

    -   Identificatore della regola SRP.

    Esempio di output scritto in un file di log:

**Explorer. exe (PID = 4728) identifiedC: \ Windows\system32\onenote.exe come regola usingpath senza restrizioni, Guid = {320bd852-aa7c-4674-82C5-9a80321670a3}**    Nel file di log verranno indicate tutte le applicazioni e il codice associato che le verifiche del rollup e l'impostazione del blocco verranno riportate nel file di log, che sarà quindi possibile utilizzare per determinare quali file eseguibili devono essere presi in considerazione per l'elenco degli

