---
title: Risolvere i problemi relativi ai criteri di restrizione software
description: Sicurezza di Windows Server
ms.prod: windows-server
ms.technology: security-software-restriction-policies
ms.topic: article
ms.assetid: 4fd53736-03e7-4bf9-ba90-d1212d93e19a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: c6b3a475f21925b506d073bd3618d78e2ee0c1d7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819724"
---
# <a name="troubleshoot-software-restriction-policies"></a>Risolvere i problemi relativi ai criteri di restrizione software

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono descritti i problemi comuni e le relative soluzioni durante la risoluzione dei problemi relativi ai criteri di restrizione software a partire da Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introduzione
I criteri di restrizione software sono una funzionalità basata su Criteri di gruppo che identifica i programmi software in esecuzione nei computer di un dominio e controlla la possibilità di tali programmi di essere eseguiti. È possibile utilizzare i criteri di restrizione software per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate. Queste sono integrate con Microsoft Active Directory Domain Services e Criteri di gruppo ma possono essere configurate anche nei computer autonomi. Per ulteriori informazioni su SRP, vedere [criteri di restrizione software](software-restriction-policies.md).

A partire da Windows Server 2008 R2 e Windows 7, è possibile utilizzare Windows AppLocker anziché o in concerto con SRP per una parte della strategia di controllo delle applicazioni.

### <a name="windows-cannot-open-a-program"></a>Windows non è in grado di aprire un programma
Gli utenti ricevono un messaggio che indica che "Windows non è in grado di aprire il programma perché è stato impedito da un criterio di restrizione software. Per ulteriori informazioni, aprire Visualizzatore eventi o contattare l'amministratore di sistema. " In alternativa, nella riga di comando viene visualizzato un messaggio che indica che il sistema non è in grado di eseguire il programma specificato.

**Motivo:** Il livello di sicurezza predefinito (o una regola) è stato creato in modo che il programma software sia impostato come non **consentito**e, di conseguenza, non verrà avviato.

**Soluzione:** Cercare nel registro eventi una descrizione dettagliata del messaggio. Il messaggio del registro eventi indica il programma software impostato come non **consentito** e la regola applicata al programma.

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>I criteri di restrizione software modificati non diventano effettivi
**Motivo:** I criteri di restrizione software specificati in un dominio tramite Criteri di gruppo sostituiscono le impostazioni dei criteri configurate localmente. Questo potrebbe implicare l'impostazione di un criterio dal dominio che esegue l'override dell'impostazione dei criteri.

**Motivo:** È possibile che Criteri di gruppo non abbia aggiornato le impostazioni dei criteri. Criteri di gruppo applica le modifiche alle impostazioni dei criteri periodicamente; Pertanto, è probabile che le modifiche ai criteri apportate nella directory non siano ancora state aggiornate.

**Soluzioni**

1.  Il computer in cui si modificano i criteri di restrizione software per la rete deve essere in grado di contattare un controller di dominio. Verificare che il computer sia in grado di contattare un controller di dominio.

2.  Aggiornare i criteri disconnettendosi dalla rete e quindi riconnettendosi alla rete. Se viene applicato un criterio tramite Criteri di gruppo, l'accesso a questi criteri verrà aggiornato.

3.  È possibile aggiornare le impostazioni dei criteri con l'utilità da riga di comando gpupdate o disconnettendosi da e quindi riconnettendosi al computer. Per ottenere risultati ottimali, eseguire gpupdate, quindi disconnettersi da e accedere di nuovo al computer. Le impostazioni di sicurezza vengono in genere aggiornate ogni 90 minuti su una workstation o un server e ogni 5 minuti in un controller di dominio. Le impostazioni vengono inoltre aggiornate ogni 16 ore, indipendentemente dal fatto che siano state apportate modifiche o meno. Si tratta di impostazioni configurabili. gli intervalli di aggiornamento potrebbero essere diversi in ogni dominio.

4.  Verificare i criteri applicati. Controllare i criteri a livello di dominio senza impostazioni di **sostituzione** .

5.  I criteri di restrizione software specificati in un dominio tramite Criteri di gruppo eseguono l'override di tutti i criteri configurati localmente. Utilizzare lo strumento da riga di comando gpresult per determinare l'effetto netto dei criteri. Questo potrebbe implicare l'esistenza di un criterio dal dominio che esegue l'override dell'impostazione locale.

6.  Se le impostazioni dei criteri SRP e AppLocker si trovano nello stesso oggetto Criteri di gruppo, le impostazioni di AppLocker avranno la precedenza su Windows 7, Windows Server 2008 R2 e versioni successive. È consigliabile inserire le impostazioni dei criteri SRP e AppLocker in oggetti Criteri di gruppo diversi.

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>Dopo aver aggiunto una regola tramite SRP, non è possibile accedere al computer
**Motivo:** Il computer accede a molti programmi e file all'avvio. È possibile che uno di questi programmi o file sia stato inavvertitamente impostato su non **consentito**. Poiché il computer non è in grado di accedere al programma o al file, non è possibile avviarlo correttamente.

**Soluzione:** Avviare il computer in modalità provvisoria, accedere come amministratore locale, quindi modificare i criteri di restrizione software per consentire l'esecuzione del programma o del file.

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>Una nuova impostazione di criteri non viene applicata a un'estensione di file specifica
**Motivo:** L'estensione del nome file non è presente nell'elenco dei tipi di file supportati.

**Soluzione:** Aggiungere l'estensione filename all'elenco dei tipi di file supportati da SRP.

I criteri di restrizione software risolvono il problema della regolazione di codice sconosciuto o non attendibile. I criteri di restrizione software sono impostazioni di sicurezza per identificare il software e controllarne la capacità di esecuzione in un computer locale, in un sito, un dominio o un'unità organizzativa e possono essere implementati tramite un oggetto Criteri di gruppo.

### <a name="a-default-rule-is-not-restricting-as-expected"></a>Una regola predefinita non limita come previsto
**Motivo:** Regole applicate in un ordine particolare che può causare l'override delle regole predefinite da specifiche regole. Il SRP applica le regole nell'ordine seguente (più specifico per generale):

1.  Regole hash

2.  Regole certificati

3.  Regole di percorso

4.  Regole dell'area Internet

5.  Regole predefinite

**Soluzione:** Valutare le regole che limitano l'applicazione e, se necessario, rimuovere tutte le regole tranne quelle predefinite.

### <a name="unable-to-discover-which-restrictions-are-applied"></a>Non è possibile individuare le restrizioni applicate
**Motivo:** Non esiste alcuna causa apparente del comportamento imprevisto e l'aggiornamento degli oggetti Criteri di gruppo non ha risolto il problema, quindi è necessaria un'ulteriore analisi.

**Soluzioni**

1.  Esaminare il registro eventi di sistema, filtrando l'origine dei "criteri di restrizione software". Le voci dichiarano in modo esplicito la regola implementata per ogni applicazione.

2.  Abilitare la registrazione avanzata. Per ulteriori informazioni, vedere [determinare l'elenco Consenti-Nega e l'inventario delle applicazioni per i criteri di restrizione software](software-restriction-policies.md) .


