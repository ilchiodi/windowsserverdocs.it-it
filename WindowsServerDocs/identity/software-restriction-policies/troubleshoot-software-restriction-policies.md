---
title: Risolvere i problemi relativi ai criteri di restrizione software
description: Sicurezza di Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872382"
---
# <a name="troubleshoot-software-restriction-policies"></a>Risolvere i problemi relativi ai criteri di restrizione software

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento descrive problemi comuni e relative soluzioni per risolvere il problema che inizia criteri restrizione Software (SRP) con Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introduzione
I criteri di restrizione software sono una funzionalità basata su Criteri di gruppo che identifica i programmi software in esecuzione nei computer di un dominio e controlla la possibilità di tali programmi di essere eseguiti. È possibile utilizzare i criteri di restrizione software per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate. Questi sono integrati con Microsoft Active Directory Domain Services e criteri di gruppo, ma può anche essere configurati in computer autonomi. Per altre informazioni sui criteri di restrizione software, vedere la [criteri di restrizione Software](software-restriction-policies.md).

A partire da Windows Server 2008 R2 e Windows 7, Windows AppLocker utilizzabile al posto di o in concerto con criteri di restrizione software per una parte della strategia di controllo dell'applicazione.

### <a name="windows-cannot-open-a-program"></a>Windows non è possibile aprire un programma
Gli utenti ricevono un messaggio con la dicitura "Windows non è possibile aprire questo programma perché è stata impedita da un criterio di restrizione software. Per altre informazioni, aprire il Visualizzatore eventi o contattare l'amministratore di sistema." In alternativa, nella riga di comando, un messaggio indicante "il sistema non è possibile eseguire il programma specificato".

**Causa:** Il livello di sicurezza predefinito (o una regola) è stata creata in modo che il programma software viene impostato come **vietate**, e di conseguenza non verrà avviata.

**Soluzione:** Esaminare il registro eventi per una descrizione dettagliata del messaggio. Messaggio del registro eventi indica quale programma software viene impostato come **vietate** e quale regola viene applicata al programma.

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>I criteri di restrizione software modificato non hanno effetto
**Causa:** Criteri di restrizione software che vengono specificati in un dominio mediante criteri di gruppo l'override delle impostazioni dei criteri sono configurate in locale. Ciò potrebbe implicare che vi sia un'impostazione dei criteri del dominio che esegue l'override delle impostazione dei criteri.

**Causa:** Criteri di gruppo potrebbero non sono aggiornati le impostazioni dei criteri. Criteri di gruppo applica le modifiche alle impostazioni dei criteri periodicamente; Pertanto, è probabile che le modifiche apportate nella directory non sono ancora state aggiornate.

**Soluzioni:**

1.  Il computer in cui si modifica criteri di restrizione software per la rete deve essere in grado di contattare un controller di dominio. Verificare che il computer può contattare un controller di dominio.

2.  Aggiornare i criteri di accesso all'esterno della rete e quindi nuovamente l'accesso alla rete. Se eventuali criteri vengono applicati tramite criteri di gruppo, riaccedendo aggiornerà tali criteri.

3.  È possibile aggiornare le impostazioni di criteri con l'utilità della riga di comando gpupdate o per la disconnessione e poi riconnettendosi nuovamente al computer. Per ottenere risultati ottimali, eseguire gpupdate e quindi disconnettersi e accedere nuovamente al computer in uso. Le impostazioni di sicurezza a livello generale, vengono aggiornate ogni 90 minuti in una workstation o server e ogni 5 minuti in un controller di dominio. Le impostazioni vengono inoltre aggiornate ogni 16 ore, indipendentemente che siano state apportate modifiche. Si tratta di impostazioni configurabili in modo che gli intervalli di aggiornamento potrebbero essere diversi in ogni dominio.

4.  Controllare i criteri da applicare. Controllare i criteri a livello di dominio per **non sovrascrivere** impostazioni.

5.  Criteri di restrizione software che vengono specificati in un dominio mediante criteri di gruppo sostituiscono eventuali criteri configurati in locale. Usare lo strumento da riga di comando Gpresult per determinare quale sia l'effetto dei criteri. Ciò potrebbe implicare che vi sia un criterio del dominio che esegue l'override di impostazioni locali.

6.  Se le impostazioni di criteri di AppLocker e criteri di restrizione software sono nello stesso GPO, impostazioni di AppLocker avranno la precedenza su Windows 7, Windows Server 2008 R2 e versioni successive. È consigliabile inserire le impostazioni di criteri restrizione software e AppLocker in oggetti Criteri di gruppo diverso.

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>Dopo l'aggiunta di una regola tramite criteri di restrizione software, non è possibile accedere al computer in uso
**Causa:** All'avvio del computer accede a molti programmi e file. Si potrebbe inavvertitamente configurato uno di questi programmi o file da **vietate**. Poiché il computer non è possibile accedere al programma o file, non è possibile avviare correttamente.

**Soluzione:** Avviare il computer in modalità provvisoria, accedere come amministratore locale e quindi modificare i criteri di restrizione software per consentire al programma o un file da eseguire.

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>Una nuova impostazione dei criteri non verrà applicati a un'estensione nome file specifico
**Causa:** L'estensione non è presente nell'elenco dei tipi di file supportati.

**Soluzione:** Aggiungere l'estensione del file all'elenco dei tipi di file supportati dal provider di risorse Condivise.

Criteri di restrizione software risolvono il problema della regolazione codice sconosciuto o non attendibile. Criteri di restrizione software sono le impostazioni di sicurezza per identificare il software e controllare la possibilità di eseguire in un computer locale, in un sito, dominio o unità Organizzativa e possono essere implementati tramite un oggetto Criteri di gruppo.

### <a name="a-default-rule-is-not-restricting-as-expected"></a>Una regola predefinita non consiste nel limita come previsto
**Causa:** Regole che vengono applicate in un determinato ordine che può causare le regole predefinite da sottoporre a override da regole specifiche. Criteri di restrizione software si applica le regole nell'ordine seguente (più specifico al generale):

1.  Regole hash

2.  Regole certificati

3.  Regole di percorso

4.  Regole area Internet

5.  Regole predefinite

**Soluzione:** Valutare le regole che limitano l'applicazione e, se appropriato, rimuovere tutto tranne la regola predefinita.

### <a name="unable-to-discover-which-restrictions-are-applied"></a>Non è possibile individuare le restrizioni vengono applicate
**Causa:** Non vi è alcuna causa apparente per il comportamento imprevisto e aggiornamento di oggetti Criteri di gruppo non ha risolto il problema non è necessaria un'ulteriore analisi.

**Soluzioni:**

1.  Analizzare il registro eventi di sistema, il filtro sull'origine di "Criteri di restrizione Software". Le voci di dichiarare in modo esplicito la regola che viene implementata per ogni applicazione.

2.  Abilitare la registrazione avanzata. Visualizzare [inventario delle applicazioni per i criteri di restrizione Software e determinare elenco Consenti-Nega](software-restriction-policies.md) per altre informazioni.


