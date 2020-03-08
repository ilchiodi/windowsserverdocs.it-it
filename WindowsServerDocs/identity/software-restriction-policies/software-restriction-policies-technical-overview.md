---
title: Panoramica tecnica di Criteri di restrizione software
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7013b0-0efd-40fd-bd6d-75128adbd0b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 293239c9f746f939b06d45d6e8c1a50b59e2bc43
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371735"
---
# <a name="software-restriction-policies-technical-overview"></a>Panoramica tecnica di Criteri di restrizione software

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono descritti i criteri di restrizione software, quando e come usare la funzionalità, quali modifiche sono state implementate nelle versioni precedenti e sono disponibili collegamenti a risorse aggiuntive per semplificare la creazione e la distribuzione di criteri di restrizione software a partire da Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introduzione
I criteri di restrizione software forniscono agli amministratori un meccanismo basato su Criteri di gruppo per identificare il software e controllarne la capacità di esecuzione nel computer locale. Questi criteri possono essere utilizzati per proteggere i computer che eseguono sistemi operativi Microsoft Windows (a partire da Windows Server 2003 e Windows XP Professional) contro i conflitti noti e proteggere i computer da minacce alla sicurezza, ad esempio virus dannosi. e i programmi Trojan Horse. È possibile usare i criteri di restrizione software anche per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate. I criteri di restrizione software sono integrati in Microsoft Active Directory e nei Criteri di gruppo. È anche possibile creare criteri di restrizione software in computer autonomi.

I criteri di restrizione software sono criteri di attendibilità, ovvero normative configurate dall'amministratore per limitare gli script e altro codice la cui esecuzione non è completamente attendibile. L'estensione criteri di restrizione software per il Editor Criteri di gruppo locali fornisce una singola interfaccia utente tramite la quale è possibile gestire le impostazioni per la limitazione dell'utilizzo delle applicazioni nel computer locale o in un dominio.

## <a name="procedures"></a>Procedure

-   [Gestire i criteri di restrizione software](administer-software-restriction-policies.md)

    -   [Determinare l'elenco Consenti-Nega e l'inventario delle applicazioni per i criteri di restrizione software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [Usare le regole dei criteri di restrizione software](work-with-software-restriction-policies-rules.md)

    -   [Usare i criteri di restrizione software per proteggere il computer da un virus di posta elettronica](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [Risolvere i problemi relativi ai criteri di restrizione software](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>Scenari di utilizzo dei criteri di restrizione software
Gli utenti aziendali collaborano utilizzando posta elettronica, messaggistica immediata e applicazioni peer-to-peer. Poiché queste collaborazioni aumentano, soprattutto con l'utilizzo di Internet nell'informatica aziendale, è possibile eseguire le minacce dal codice dannoso, ad esempio worm, virus e minacce per utenti malintenzionati o utenti malintenzionati.

È possibile che gli utenti ricevano codice ostile in molte forme, da file eseguibili di Windows nativi (file con estensione exe), a macro nei documenti (ad esempio file con estensione doc), a script (ad esempio file con estensione vbs). Utenti malintenzionati o utenti malintenzionati usano spesso metodi di ingegneria sociale per consentire agli utenti di eseguire codice contenente virus e worm. (Ingegneria sociale è un termine per ingannare gli utenti a rivelare la propria password o una forma di informazioni di sicurezza). Se tale codice viene attivato, può generare attacchi Denial of Service sulla rete, inviare dati sensibili o privati a Internet, mettere a rischio la sicurezza del computer oppure danneggiare il contenuto dell'unità disco rigido.

Le organizzazioni e gli utenti IT devono essere in grado di determinare quale software è sicuro per l'esecuzione e non lo è. Con i numeri e i form di grandi dimensioni che possono essere accettati dal codice ostile, questo diventa un'attività difficile.

Per proteggere i computer di rete da codice ostile e software sconosciuto o non supportato, le organizzazioni possono implementare i criteri di restrizione software come parte della strategia di sicurezza globale.

Gli amministratori possono usare i criteri di restrizione software per le attività seguenti:

-   Definire il codice attendibile

-   Progettare Criteri di gruppo flessibili per definire script, file eseguibili e controlli ActiveX

I criteri di restrizione software vengono imposti dal sistema operativo e dalle applicazioni (ad esempio, le applicazioni di scripting) conformi ai criteri di restrizione software.

In particolare, gli amministratori possono usare i criteri di restrizione software per gli scopi seguenti:

-   Specificare il software (file eseguibili) che può essere eseguito nei computer client

-   Impedire agli utenti di eseguire determinati programmi nei computer condivisi

-   Specificare gli utenti che possono aggiungere autori attendibili ai computer client

-   Impostare l'ambito dei criteri di restrizione software (specificare se i criteri interessano tutti gli utenti o un sottoinsieme di utenti nei computer client)

-   Impedire l'esecuzione dei file eseguibili nel computer locale, nell'unità organizzativa, nel sito o nel dominio. Questa funzionalità è utile quando non si usano i criteri di restrizione software per risolvere i potenziali problemi relativi a utenti malintenzionati.

## <a name="BKMK_Diffs_Changes"></a>Differenze e modifiche della funzionalità
Non sono state apportate modifiche alla funzionalità di criteri di RESTRIzione software per Windows Server 2012 e Windows 8.

**Versioni supportate**

I criteri di restrizione software possono essere configurati e applicati solo a computer che eseguono almeno Windows Server 2003, tra cui Windows Server 2012 e almeno Windows XP, incluso Windows 8.

> [!NOTE]
> Alcune edizioni del sistema operativo client Windows a partire da Windows Vista non dispongono di criteri di restrizione software. I computer non gestiti in un dominio da Criteri di gruppo potrebbero non ricevere i criteri distribuiti.

**Confronto tra le funzioni di controllo delle applicazioni in Criteri restrizione software e AppLocker**

Nel seguente tabella vengono confrontate le funzionalità e le funzioni di Criteri di restrizione software e AppLocker.

|Funzione di controllo delle applicazioni|Criteri di restrizione software|AppLocker|
|----------------|----|-------|
|Ambito|I criteri del Provider di risorse condivise possono essere applicati a tutti i sistemi operativi Windows, a partire da Windows XP e Windows Server 2003.|I criteri di AppLocker si applicano solo a Windows Server 2008 R2, Windows Server 2012, Windows 7 e Windows 8.|
|Creazione dei criteri|I criteri di criteri di RESTRIzione software vengono gestiti tramite Criteri di gruppo e solo l'amministratore dell'oggetto Criteri di gruppo può aggiornare i criteri di criteri di L'amministratore del computer locale può modificare i criteri di criteri di RESTRIzione software definiti nell'oggetto Criteri di gruppo locale.|I criteri di AppLocker vengono gestiti tramite Criteri di gruppo e solo l'amministratore dell'oggetto Criteri di gruppo può aggiornare i criteri. L'amministratore del computer locale può modificare i criteri di AppLocker definiti nell'oggetto Criteri di gruppo locale.<br /><br />AppLocker consente la personalizzazione dei messaggi di errore per indirizzare gli utenti a una pagina Web di guida.|
|Manutenzione dei criteri|È necessario aggiornare i criteri di criteri di RESTRIzione software tramite lo snap-in criteri di sicurezza locali (se i criteri sono stati creati localmente) o la Console Gestione Criteri di gruppo (GPMC).|I criteri di AppLocker possono essere aggiornati tramite lo snap-in criteri di sicurezza locali (se i criteri sono stati creati localmente) o la console Gestione criteri di Windows o i cmdlet di AppLocker di Windows PowerShell.|
|Applicazione criteri|I criteri SRP vengono distribuiti tramite Criteri di gruppo.|I criteri di AppLocker vengono distribuiti tramite Criteri di gruppo.|
|Modalità di imposizione|L'opzione SRP funziona in modalità elenco di negazione in cui gli amministratori possono creare regole per i file che non vogliono consentire in questa organizzazione, mentre il resto del file può essere eseguito per impostazione predefinita.<br /><br />È inoltre possibile configurare il criteri di RESTRIzione software in modo che per impostazione predefinita tutti i file siano bloccati e che gli amministratori debbano creare regole di autorizzazione per i file che desiderano consentire.|Per impostazione predefinita, AppLocker funziona in "Consenti modalità elenco", in cui è consentita l'esecuzione solo dei file per i quali esiste una regola di consenso corrispondente.|
|Tipi di file che possono essere controllati|Il SRP può controllare i tipi di file seguenti:<br /><br />-Eseguibili<br />-Dll<br />-Script<br />-Programmi di installazione di Windows<br /><br />Il SRP non è in grado di controllare separatamente ogni tipo di file. Tutte le regole SRP si trovano in una singola raccolta di regole.|AppLocker può controllare i tipi di file seguenti:<br /><br />-Eseguibili<br />-Dll<br />-Script<br />-Programmi di installazione di Windows<br />-App e programmi di installazione in pacchetto (Windows Server 2012 e Windows 8)<br /><br />AppLocker gestisce una raccolta di regole separata per ognuno dei cinque tipi di file.|
|Tipi di file designati|SRP supporta un elenco estendibile di tipi di file considerati eseguibili. Gli amministratori possono aggiungere estensioni per i file che devono essere considerati eseguibili.|AppLocker non supporta questa operazione. AppLocker supporta attualmente le estensioni di file seguenti:<br /><br />-Eseguibili (. exe,. com)<br />-Dll (. ocx,. dll)<br />-Script (. vbs,. js,. ps1,. cmd,. bat)<br />-Programmi di installazione di Windows (con estensione msi, MST e msp)<br />-Programmi di installazione app in pacchetto (. appx)|
|Tipi di regole|Il SRP supporta quattro tipi di regole:<br /><br />-Hash<br />-Percorso<br />-Firma<br />-Area Internet|AppLocker supporta tre tipi di regole:<br /><br />-Hash<br />-Percorso<br />-Editore|
|Modifica del valore hash|Il SRP consente agli amministratori di fornire valori hash personalizzati.|AppLocker calcola il valore hash. Internamente usa l'hash Authenticode di SHA1 per i file eseguibili portabili (exe e dll) e i programmi di installazione di Windows e un hash file flat SHA1 per il resto.|
|Supporto per diversi livelli di sicurezza|Con gli amministratori SRP è possibile specificare le autorizzazioni con cui è possibile eseguire un'app. Un amministratore può quindi configurare una regola in modo che il blocco note venga sempre eseguito con autorizzazioni limitate e mai con privilegi amministrativi.<br /><br />I criteri di RESTRIzione software in Windows Vista e versioni precedenti supportano più livelli di sicurezza. In Windows 7 l'elenco era limitato a soli due livelli: non consentiti e senza restrizioni (l'utente di base si traduce in non consentito).|AppLocker non supporta i livelli di sicurezza.|
|Gestire le app in pacchetto e i programmi di installazione app in pacchetto|Impossibile|. appx è un tipo di file valido che può essere gestito da AppLocker.|
|Assegnazione di una regola a un utente o a un gruppo di utenti|Le regole SRP si applicano a tutti gli utenti di un computer specifico.|Le regole di AppLocker possono essere destinate a un utente specifico o a un gruppo di utenti.|
|Supporto per le eccezioni delle regole|Il SRP non supporta le eccezioni delle regole|Le regole di AppLocker possono avere eccezioni che consentono agli amministratori di creare regole come "Consenti tutto da Windows ad eccezione di Regedit. exe".|
|Supporto per la modalità di controllo|Il SRP non supporta la modalità di controllo. L'unico modo per testare i criteri di criteri di RESTRIzione software consiste nel configurare un ambiente di test ed eseguire alcuni esperimenti.|AppLocker supporta la modalità di controllo che consente agli amministratori di testare l'effetto dei criteri nell'ambiente di produzione reale senza influire sull'esperienza utente. Quando si è soddisfatti dei risultati, è possibile avviare l'applicazione dei criteri.|
|Supporto per l'esportazione e l'importazione di criteri|Il SRP non supporta l'importazione/esportazione dei criteri.|AppLocker supporta l'importazione e l'esportazione di criteri. In questo modo è possibile creare criteri di AppLocker in un computer di esempio, testarli e quindi esportarli nuovamente nell'oggetto Criteri di gruppo desiderato.|
|Imposizione delle regole|Internamente, l'imposizione delle regole SRP si verifica in modalità utente meno sicura.|Internamente, le regole di AppLocker per i exe e le dll vengono applicate in modalità kernel, che è più sicura rispetto all'applicazione in modalità utente.|

## <a name="system-requirements"></a>Requisiti di sistema
I criteri di restrizione software possono essere configurati e applicati solo a computer che eseguono almeno Windows Server 2003 e almeno Windows XP. Criteri di gruppo è necessario per distribuire Criteri di gruppo oggetti che contengono criteri di restrizione software.

## <a name="software-restriction-policies-components-and-architecture"></a>Componenti e architettura dei criteri di restrizione software
I criteri di restrizione software forniscono un meccanismo per il sistema operativo e le applicazioni conformi ai criteri di restrizione software per limitare l'esecuzione in fase di esecuzione dei programmi software.

A un livello elevato, i criteri di restrizione software sono costituiti dai componenti seguenti:

-   API criteri di restrizione software. Le API (Application Programming Interface) vengono usate per creare e configurare le regole che costituiscono i criteri di restrizione software. Sono disponibili anche API per i criteri di restrizione software per l'esecuzione di query, l'elaborazione e l'applicazione di criteri di restrizione software.

-   Uno strumento di gestione dei criteri di restrizione software. Questo è costituito dall'estensione **criteri di restrizione software** dello snap-in **Editor oggetti Criteri di gruppo locale** , che gli amministratori usano per creare e modificare i criteri di restrizione software.

-   Un set di API del sistema operativo e applicazioni che chiamano le API di criteri di restrizione software per fornire l'applicazione dei criteri di restrizione software in fase di esecuzione.

-   Active Directory e Criteri di gruppo. I criteri di restrizione software dipendono dall'infrastruttura Criteri di gruppo per propagare i criteri di restrizione software dal Active Directory ai client appropriati e per l'ambito e il filtraggio dell'applicazione di questi criteri nel modo appropriato computer di destinazione.

-   API di attendibilità Authenticode e WinVerify che vengono usate per elaborare i file eseguibili firmati.

-   Visualizzatore eventi. Le funzioni utilizzate dai criteri di restrizione software registrano gli eventi nei log del Visualizzatore eventi.

-   Gruppo di criteri risultante (RSoP), che può aiutare a diagnosticare i criteri effettivi che verranno applicati a un client.

Per ulteriori informazioni sull'architettura del SRP, sulle modalità di gestione delle regole, dei processi e delle interazioni tra i criteri di restrizione software, vedere funzionamento dei [criteri di restrizione software](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx) nella raccolta di documentazione tecnica su 2003 Windows

## <a name="BKMK_Best_Practices"></a>Procedure consigliate

### <a name="do-not-modify-the-default-domain-policy"></a>Non modificare i criteri di dominio predefiniti.

-   Se non si modificano i criteri di dominio predefiniti, è sempre possibile riapplicare i criteri di dominio predefiniti se si verificano problemi con i criteri di dominio personalizzati.

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>Creare un oggetto Criteri di gruppo separato per i criteri di restrizione software.

-   Se si crea un oggetto Criteri di gruppo separato (GPO) per i criteri di restrizione software, è possibile disabilitare i criteri di restrizione software in un'emergenza senza disabilitare il resto del criterio del dominio.

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>Se si verificano problemi con le impostazioni dei criteri applicati, riavviare Windows in modalità provvisoria.

-   I criteri di restrizione software non si applicano quando Windows viene avviato in modalità provvisoria. Se si blocca accidentalmente una workstation con criteri di restrizione software, riavviare il computer in modalità provvisoria, accedere come amministratore locale, modificare il criterio, eseguire **gpupdate**, riavviare il computer e accedere normalmente.

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>Prestare attenzione quando si definisce un'impostazione predefinita di non consentita.

-   Quando si definisce un'impostazione predefinita non **consentita**, tutto il software non è consentito tranne che per il software che è stato esplicitamente consentito. Tutti i file che si desidera aprire devono disporre di una regola criteri di restrizione software che ne consenta l'apertura.

-   Per impedire agli amministratori di bloccarsi dal sistema, quando il livello di sicurezza predefinito è impostato su non **consentito**, vengono create automaticamente quattro regole per il percorso del registro di sistema. È possibile eliminare o modificare queste regole del percorso del registro di sistema; Tuttavia, questa operazione non è consigliata.

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>Per una maggiore sicurezza, usare gli elenchi di controllo di accesso insieme ai criteri di restrizione software.

-   È possibile che gli utenti tenti di aggirare i criteri di restrizione software rinominando o spostando i file non consentiti oppure sovrascrivendo i file senza restrizioni. È quindi consigliabile usare gli elenchi di controllo di accesso (ACL) per negare agli utenti l'accesso necessario per eseguire queste attività.

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>Testare accuratamente le nuove impostazioni dei criteri negli ambienti di test prima di applicare le impostazioni dei criteri al dominio.

-   Le nuove impostazioni dei criteri potrebbero funzionare in modo diverso rispetto all'origine prevista. Il test diminuisce la probabilità di riscontrare un problema quando si distribuiscono le impostazioni dei criteri in rete.

-   È possibile configurare un dominio di test, separato dal dominio dell'organizzazione, in cui testare le nuove impostazioni dei criteri. È anche possibile testare le impostazioni dei criteri creando un oggetto Criteri di gruppo di test e collegarlo a un'unità organizzativa di test. Dopo aver testato accuratamente le impostazioni dei criteri con gli utenti di test, è possibile collegare l'oggetto Criteri di gruppo di test al dominio.

-   Non impostare programmi o file su non **consentito** senza test per vedere quale potrebbe essere l'effetto. Le restrizioni per determinati file possono influire seriamente sul funzionamento del computer o della rete.

-   Le informazioni immesse in modo errato o la digitazione di errori possono causare un'impostazione dei criteri che non viene eseguita come previsto. Il test delle nuove impostazioni dei criteri prima di applicarli può impedire un comportamento imprevisto.

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>Filtrare le impostazioni dei criteri utente in base all'appartenenza ai gruppi di sicurezza.

-   È possibile specificare gli utenti o i gruppi per i quali non si desidera applicare un'impostazione di criteri deselezionando le caselle di controllo **applica criteri di gruppo** e **Leggi** , che si trovano nella scheda **sicurezza** della finestra di dialogo proprietà per l'oggetto Criteri di gruppo.

-   Quando l'autorizzazione di lettura viene negata, l'impostazione dei criteri non viene scaricata dal computer. Di conseguenza, una minore larghezza di banda viene utilizzata scaricando le impostazioni dei criteri non necessarie, consentendo al tempo stesso di funzionare più rapidamente. Per negare l'autorizzazione lettura, selezionare **Nega** per la casella di controllo **lettura** , disponibile nella scheda **sicurezza** della finestra di dialogo proprietà per l'oggetto Criteri di gruppo.

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>Non collegarsi a un oggetto Criteri di gruppo in un altro dominio o sito.

-   Il collegamento a un oggetto Criteri di gruppo in un altro dominio o sito può comportare una riduzione delle prestazioni.

## <a name="additional-resources"></a>Risorse aggiuntive

|Tipo di contenuto|Riferimenti|
|--------|-------|
|**Pianificazione**|[Riferimento tecnico per i criteri di restrizione software](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)|
|**Operazioni**|[Gestire i criteri di restrizione software](administer-software-restriction-policies.md)|
|**Risoluzione dei problemi**|[Risoluzione dei problemi relativi ai criteri di restrizione software (2003)](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)|
|**Sicurezza**|[Minacce e contromisure per criteri di restrizione software (2008)](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)<br /><br />[Minacce e contromisure per criteri di restrizione software (2008 R2)](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)|
|**Strumenti e impostazioni**|[Strumenti e impostazioni di criteri di restrizione software (2003)](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)|
|**Risorse della community**|[Blocco dell'applicazione con criteri di restrizione software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|


