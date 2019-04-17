---
title: Risolvere i problemi relativi a criteri di restrizione Software
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fd53736-03e7-4bf9-ba90-d1212d93e19a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: abf592930f5347fa10e83c644671f68f9dc1a3f6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-software-restriction-policies"></a>Risolvere i problemi relativi a criteri di restrizione Software

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento descrive i problemi comuni e le relative soluzioni quando la risoluzione dei problemi a partire da Windows Server 2008 e Windows Vista Software criteri di restrizione.

## <a name="introduction"></a>Introduzione
Software di criteri di restrizione è una funzionalità basata su criteri di gruppo che identifica i programmi software in esecuzione nel computer in un dominio e controlla la possibilità di eseguire tali programmi. Utilizzare criteri di restrizione software per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate eseguire. Questi sono integrati in servizi di dominio Microsoft Active Directory e criteri di gruppo, ma possono anche essere configurati in computer autonomi. Per ulteriori informazioni su criteri di restrizione software, vedere il [criteri di restrizione Software](software-restriction-policies.md).

A partire da Windows Server 2008 R2 e Windows 7, Windows AppLocker può essere utilizzato invece di o in combinazione con criteri di restrizione software per una parte della strategia di controllo dell'applicazione.

### <a name="windows-cannot-open-a-program"></a>Impossibile aprire un programma
Gli utenti ricevono un messaggio che dice "Impossibile aprire questo programma perché impedisce da criteri di restrizione software. Per ulteriori informazioni, aprire il Visualizzatore eventi o contattare l'amministratore del sistema." In alternativa, nella riga di comando, un messaggio "il sistema non può eseguire il programma specificato".

**Causa:** il livello di sicurezza predefinito (o una regola) è stata creata in modo che il programma software viene impostato come **non consentito**, e di conseguenza non verrà avviata.

**Soluzione:** cercare nel registro eventi per una descrizione dettagliata del messaggio. Messaggio del registro eventi indica quale programma software viene impostato come **non consentito** e la regola viene applicata al programma.

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>I criteri di restrizione software modificato non vengono applicati
**Causa:** criteri di restrizione Software sono specificate in un dominio tramite criteri di gruppo sostituire le impostazioni di criteri che sono configurate in locale. Ciò potrebbe implica che non esiste un'impostazione di criteri di dominio che esegue l'override delle impostazioni dei criteri del.

**Causa:** aggiornato criteri di gruppo potrebbero non avere le impostazioni dei criteri. Criteri di gruppo applicano le modifiche alle impostazioni dei criteri periodicamente; Pertanto, è probabile che le modifiche di criteri che sono state apportate nella directory non è sono ancora aggiornate.

**Soluzioni:**

1.  Il computer in cui si modifica criteri di restrizione software per la rete deve essere in grado di contattare un controller di dominio. Assicurarsi che il computer in grado di contattare un controller di dominio.

2.  Aggiornare i criteri di registrazione esterno della rete e quindi nuovamente l'accesso alla rete. Se qualsiasi criterio viene applicato tramite criteri di gruppo, registrazione nella aggiornerà tali criteri.

3.  È possibile aggiornare le impostazioni dei criteri con gpupdate l'utilità della riga di comando o la disconnessione e quindi accedere nuovamente al computer. Per ottenere risultati ottimali, eseguire gpupdate e quindi disconnettersi e accedere nuovamente al computer. In genere, le impostazioni di protezione vengono aggiornate ogni 90 minuti in un server o workstation e ogni 5 minuti in un controller di dominio. Le impostazioni vengono inoltre aggiornate ogni 16 ore, sono state apportate modifiche o meno. Queste sono impostazioni configurabili in modo che gli intervalli di aggiornamento potrebbero essere diversi in ogni dominio.

4.  Controlla i criteri da applicare. Controllare i criteri a livello di dominio per **non sovrascrivere** impostazioni.

5.  Criteri di restrizione software sono specificate in un dominio tramite criteri di gruppo ignorare eventuali criteri configurati localmente. Utilizzare lo strumento da riga di comando Gpresult per determinare l'effetto dei criteri. Ciò potrebbe implica che non esiste un criterio di dominio che esegue l'override delle impostazioni locali.

6.  Se le impostazioni dei criteri di restrizione software e AppLocker nell'oggetto Criteri di gruppo stesso, impostazioni di AppLocker avranno la priorità in Windows 7, Windows Server 2008 R2 e versioni successive. È consigliabile inserire le impostazioni dei criteri di restrizione software e AppLocker in oggetti Criteri di gruppo diverso.

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>Dopo l'aggiunta di una regola tramite criteri di restrizione software, è possibile accedere al computer
**Causa:** computer accede a molti programmi e file all'avvio. Potrebbe inavvertitamente impostati uno di questi programmi o file **non consentito**. Poiché il computer non può accedere il programma o un file, non viene avviato correttamente.

**Soluzione:** avviare il computer in modalità provvisoria, accedere come amministratore locale e quindi modificare i criteri di restrizione software per consentire al programma o un file da eseguire.

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>Una nuova impostazione dei criteri non è valida per un'estensione nome file specifico
**Causa:** l'estensione del file non è presente nell'elenco dei tipi di file supportati.

**Soluzione:** aggiungere l'estensione nome file all'elenco dei tipi di file supportati da criteri di restrizione software.

Criteri di restrizione software risolvono il problema della gestione codice sconosciuto o non attendibile. Criteri di restrizione software sono impostazioni di sicurezza per identificare il software e controllare la possibilità di esecuzione in un computer locale, in un sito, dominio o unità Organizzativa e possono essere implementati tramite un oggetto Criteri di gruppo.

### <a name="a-default-rule-is-not-restricting-as-expected"></a>Una regola predefinita non consiste nel limita come previsto
**Causa:** regole che vengono applicate in un ordine specifico che può generare regole predefinite eseguire l'override di regole specifiche. Criteri di restrizione software si applica regole nell'ordine seguente (più specifico al generale):

1.  Regole hash

2.  Regole certificati

3.  Regole di percorso

4.  Regole area Internet

5.  Regole predefinite

**Soluzione:** valutare le regole che limitano l'applicazione e, se necessario, rimuovere tutti tranne la regola predefinita.

### <a name="unable-to-discover-which-restrictions-are-applied"></a>Impossibile individuare le restrizioni applicate
**Causa:** aggiornamento oggetto Criteri di gruppo non ha risolto il problema per cui è necessario analizzare ulteriormente il problema e non esiste alcuna causa apparente per il comportamento imprevisto.

**Soluzioni:**

1.  Investigate the System Event Log, filtering on source of "Software Restriction Policy." Le voci dello stato in modo esplicito la regola è implementata per ogni applicazione.

2.  Abilitare la registrazione avanzata. Vedere [determinare elenco Consenti-Nega e l'inventario delle applicazioni per criteri di restrizione Software](software-restriction-policies.md) per ulteriori informazioni.


