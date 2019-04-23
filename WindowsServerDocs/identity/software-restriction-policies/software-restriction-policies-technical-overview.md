---
title: Panoramica tecnica di Criteri di restrizione software
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d007d55ced9c6a18581eaedb4edb66db9eeccab9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830852"
---
# <a name="software-restriction-policies-technical-overview"></a>Panoramica tecnica di Criteri di restrizione software

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento descrive i criteri di restrizione software, quando e come usare la funzionalità, quali modifiche sono state implementate nelle versioni precedenti e fornisce collegamenti a risorse aggiuntive che consentono di creare e distribuire criteri di restrizione software a partire da Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introduzione
Criteri di restrizione software forniscono agli amministratori un meccanismo basato sui criteri di gruppo per identificare il software e controllare la possibilità di eseguire nel computer locale. Questi criteri possono essere usati per proteggere i computer che eseguono sistemi operativi Microsoft Windows (che inizia con Windows Server 2003 e Windows XP Professional) contro i conflitti noti e proteggere i computer da minacce di sicurezza, ad esempio virus e i programmi di Trojan horse. È possibile usare i criteri di restrizione software anche per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate. I criteri di restrizione software sono integrati in Microsoft Active Directory e nei Criteri di gruppo. È anche possibile creare criteri di restrizione software in computer autonomi.

I criteri di restrizione software sono criteri di attendibilità, ovvero normative configurate dall'amministratore per limitare gli script e altro codice la cui esecuzione non è completamente attendibile. L'estensione di criteri di restrizione Software a Editor criteri di gruppo locali offre una singola interfaccia utente attraverso cui è possono gestire le impostazioni che consentono di limitare l'utilizzo delle applicazioni nel computer locale o in un dominio.

## <a name="procedures"></a>Procedure

-   [Amministrare criteri di restrizione Software](administer-software-restriction-policies.md)

    -   [Determinare elenco Consenti-Nega e l'inventario delle applicazioni per i criteri di restrizione Software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [Lavorare con le regole di criteri di restrizione Software](work-with-software-restriction-policies-rules.md)

    -   [Usare i criteri di restrizione Software per proteggere il Computer da un Virus di posta elettronica](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [Risolvere i problemi di criteri di restrizione Software](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>Scenari di utilizzo dei criteri di restrizione software
Gli utenti aziendali collaborano tramite posta elettronica, messaggistica istantanea e le applicazioni peer-to-peer. Queste collaborazioni con l'aumentare, in particolare con l'uso di Internet nei sistemi informatici aziendali, quindi, eseguire le minacce da codice dannoso, ad esempio worm, virus e utenti malintenzionati o minacce autore dell'attacco.

Gli utenti potrebbero ricevere un codice dannoso in molte forme, che vanno dai file eseguibili nativi Windows (file .exe), alle macro nei documenti (ad esempio i file doc), agli script (ad esempio i file con estensione vbs). Gli utenti malintenzionati o attacchi usano spesso i metodi di ingegneria sociale per ottenere gli utenti di eseguire codice che include virus e worm. (L'ingegneria sociale è un termine per indurre gli utenti a rivelare la password o altri tipi di informazioni di sicurezza). Se tale codice è attivato, è possibile generare gli attacchi denial of service nella rete, inviare i dati sensibili o riservati a Internet, mettere a repentaglio la sicurezza del computer o danneggiare il contenuto dell'unità disco rigido.

Gli utenti e organizzazioni IT devono essere in grado di determinare quale software è sicuro per l'esecuzione e che non corrisponde. Il numero elevato e moduli che può eseguire codice dannoso, questo diventa un'attività complessa.

Per proteggere i computer di rete da codice dannoso e software sconosciuto o non supportato, le organizzazioni possono implementare i criteri di restrizione software come parte della propria strategia di sicurezza complessiva.

Gli amministratori possono usare i criteri di restrizione software per le attività seguenti:

-   Definire il codice attendibile

-   Progettare Criteri di gruppo flessibili per definire script, file eseguibili e controlli ActiveX

I criteri di restrizione software vengono imposti dal sistema operativo e dalle applicazioni (ad esempio, le applicazioni di scripting) conformi ai criteri di restrizione software.

In particolare, gli amministratori possono usare i criteri di restrizione software per gli scopi seguenti:

-   Specificare il tipo di software (file eseguibili) è possibile eseguire nei computer client

-   Impedire agli utenti di eseguire determinati programmi nei computer condivisi

-   Specificare chi può aggiungere autori attendibili ai computer client

-   Impostare l'ambito dei criteri di restrizione software (specificare se i criteri interessano tutti gli utenti o solo un sottoinsieme di utenti nei computer client)

-   Impedire l'esecuzione dei file eseguibili nel computer locale, nell'unità organizzativa, nel sito o nel dominio. Questa funzionalità è utile quando non si usano i criteri di restrizione software per risolvere i potenziali problemi relativi a utenti malintenzionati.

## <a name="BKMK_Diffs_Changes"></a>Modifiche apportate alle funzionalità e differenze
Sono state apportate modifiche di funzionalità in Criteri di restrizione software per Windows Server 2012 e Windows 8.

**Versioni supportate**

Criteri di restrizione software solo possono essere configurati in e applicati ai computer che eseguono almeno Windows Server 2003, tra cui Windows Server 2012 e almeno Windows XP, tra cui Windows 8.

> [!NOTE]
> Alcune edizioni di inizio del sistema operativo client Windows con Windows Vista non hanno i criteri di restrizione Software. I computer non gestiti in un dominio da criteri di gruppo potrebbero non ricevere i criteri distribuiti.

**Confronto tra funzioni di controllo delle applicazioni in Criteri restrizione Software e AppLocker**

Nel seguente tabella vengono confrontate le funzionalità e le funzioni di Criteri di restrizione software e AppLocker.

|Funzione di controllo delle applicazioni|Criteri di restrizione software|AppLocker|
|----------------|----|-------|
|Ambito|I criteri del Provider di risorse condivise possono essere applicati a tutti i sistemi operativi Windows, a partire da Windows XP e Windows Server 2003.|I criteri di AppLocker si applicano solo a Windows Server 2008 R2, Windows Server 2012, Windows 7 e Windows 8.|
|Creazione dei criteri|Criteri di restrizione software sono gestiti tramite criteri di gruppo e solo l'amministratore del GPO può aggiornare i criteri di restrizione software. L'amministratore nel computer locale è possibile modificare i criteri di restrizione software definiti nell'oggetto Criteri di gruppo locali.|I criteri di AppLocker vengono gestiti tramite criteri di gruppo e solo l'amministratore del GPO può aggiornare i criteri. L'amministratore nel computer locale è possibile modificare i criteri di AppLocker nell'oggetto Criteri di gruppo locali.<br /><br />AppLocker consente la personalizzazione dei messaggi di errore per indirizzare gli utenti a una pagina Web di guida.|
|Manutenzione dei criteri|Criteri di restrizione software devono essere aggiornati tramite lo snap-in Criteri di sicurezza locali (se i criteri vengono creati in locale) o la Console Gestione criteri di gruppo (GPMC).|I criteri di AppLocker possono essere aggiornati con i criteri di sicurezza locali snap-in (se i criteri vengono creati in locale), o console GPMC o i cmdlet di AppLocker di Windows PowerShell.|
|Applicazione dei criteri|Criteri di restrizione software vengono distribuiti tramite criteri di gruppo.|I criteri di AppLocker vengono distribuiti tramite criteri di gruppo.|
|Modalità di imposizione|Criteri di restrizione software è l'impostazione "Impedisci modalità elenco" in cui gli amministratori possono creare regole per i file che non desiderano consentire questa azienda, mentre il resto del file possono essere eseguiti per impostazione predefinita.<br /><br />Criteri di restrizione software può essere configurato anche nella sezione "modalità elenco Consenti" in modo che il per impostazione predefinita tutti i file sono bloccati e gli amministratori devono creare le regole per i file che si vuole consentire.|AppLocker predefinito funziona nella sezione "Consenti la modalità elenco" in cui sono consentiti solo i file per l'esecuzione per il quale vi è una corrispondenza regola di assenso.|
|Tipi di file che possono essere controllati|Criteri di restrizione software possono controllare i tipi di file seguenti:<br /><br />-File eseguibili<br />-DLL<br />-Script<br />-I programmi di installazione di Windows<br /><br />Criteri di restrizione software non è possibile controllare i tipi di file separatamente. Tutte le regole di criteri di restrizione software sono in una raccolta singola regola.|AppLocker può controllare i tipi di file seguenti:<br /><br />-File eseguibili<br />-DLL<br />-Script<br />-I programmi di installazione di Windows<br />-App in pacchetto e programmi di installazione (Windows Server 2012 e Windows 8)<br /><br />AppLocker gestisce una raccolta di regola separata per ciascuno dei tipi di cinque file.|
|Tipi di file designati|Criteri di restrizione software supporta un elenco dei tipi di file che sono considerati eseguibile estendibile. Gli amministratori possono aggiungere le estensioni di file che devono essere considerati come eseguibile.|AppLocker non la supportano. AppLocker supporta attualmente le estensioni di file seguenti:<br /><br />-File eseguibili (.exe,. com)<br />-DLL (OCX, DLL)<br />-Script (con estensione vbs,. js, ps1,. cmd,. bat)<br />-Programmi di Windows installazione (con estensione msi, MST, msp)<br />-Programmi di installazione di app nel pacchetto (con estensione AppX)|
|Tipi di regole|Criteri di restrizione software supporta quattro tipi di regole:<br /><br />-Hash<br />-   Path<br />-Signature<br />-Area Internet|AppLocker supporta tre tipi di regole:<br /><br />-Hash<br />-   Path<br />-   Publisher|
|Modifica il valore hash|Criteri di restrizione software consente agli amministratori di fornire i valori hash personalizzato.|AppLocker calcola il valore hash stesso. Internamente Usa l'hash SHA1 Authenticode per file eseguibili portabili (file Exe e Dll) e i programmi di installazione di Windows e un hash SHA1 del file flat per il resto.|
|Supporto per diversi livelli di sicurezza|Con criteri di restrizione software, gli amministratori possono specificare le autorizzazioni con cui è possibile eseguire un'app. Pertanto, un amministratore può configurare una regola di questo tipo di blocco note viene sempre eseguito con autorizzazioni limitate e mai con privilegi amministrativi.<br /><br />Criteri di restrizione software in Windows Vista e versioni precedenti supportati più livelli di sicurezza. In Windows 7 tale elenco è limitato a due livelli: Non consentita e illimitato (utente di base viene convertita in non consentito).|AppLocker non supporta i livelli di sicurezza.|
|Gestire le App in pacchetto e programmi di installazione app in pacchetto|Non è possibile|con estensione AppX è un tipo di file valido che può gestire AppLocker.|
|Destinato a una regola a un utente o un gruppo di utenti|Regole di criteri di restrizione software si applicano a tutti gli utenti in un computer specifico.|Le regole di AppLocker possono essere destinate a un utente specifico o un gruppo di utenti.|
|Supporto per le eccezioni alla regola|Criteri di restrizione software non supporta le eccezioni alla regola|Le regole di AppLocker possono avere le eccezioni che consentono agli amministratori di creare regole, ad esempio "Consenti tutti gli elementi da Windows ad eccezione di Regedit.exe".|
|Supporto per la modalità di controllo|Criteri di restrizione software non supporta la modalità di controllo. L'unico modo per testare i criteri di restrizione software è configurare un ambiente di test ed eseguire alcuni esperimenti.|AppLocker supporta la modalità di controllo che consente agli amministratori di testare l'effetto dei criteri nell'ambiente di produzione reale senza alcun impatto sull'esperienza utente. Quando si è soddisfatti dei risultati, è possibile avviare l'applicazione di criteri.|
|Supporto per l'esportazione e importazione di criteri|Criteri di restrizione software non supporta l'importazione/esportazione dei criteri.|AppLocker supporta l'importazione ed esportazione dei criteri. Questo consente di creare criteri di AppLocker in un computer di esempio, testarlo e quindi esportare tali criteri e importarlo nuovamente nell'oggetto Criteri di gruppo desiderato.|
|Imposizione delle regole|Internamente, imposizione delle regole di criteri di restrizione software viene eseguito in modalità utente che è meno sicura.|Internamente, vengono applicate le regole di AppLocker per file eseguibili e DLL in modalità kernel che è più sicura rispetto alla applicandoli in modalità utente.|

## <a name="system-requirements"></a>Requisiti di sistema
Criteri di restrizione software solo possono essere configurati in e applicati ai computer che eseguono almeno Windows Server 2003 e almeno Windows XP. Criteri di gruppo sono necessario per distribuire oggetti Criteri di gruppo che contengono criteri restrizione software.

## <a name="software-restriction-policies-components-and-architecture"></a>Architettura e componenti dei criteri di restrizione software
Criteri di restrizione software forniscono un meccanismo per il sistema operativo e le applicazioni siano conformi ai criteri di restrizione software per limitare l'esecuzione di runtime di programmi software.

A livello generale, i criteri di restrizione software sono i seguenti componenti:

-   Criteri di restrizione software API. Application Programming Interface (API) vengono utilizzate per creare e configurare le regole che costituiscono i criteri di restrizione software. Sono disponibili anche i criteri di restrizione software le API per l'esecuzione di query, elaborazione e l'applicazione di criteri restrizione software.

-   Strumento Gestione criteri restrizione software. Si tratta del **criteri di restrizione Software** estensione del **Editor oggetti Criteri di gruppo locali** snap-in, gli amministratori che consente di creare e modificare i criteri di restrizione software.

-   Un set di API del sistema operativo e le applicazioni che chiamano i criteri di restrizione software le API per fornire l'imposizione di criteri di restrizione software in fase di esecuzione.

-   Active Directory e criteri di gruppo. Criteri di restrizione software variano in base l'infrastruttura di criteri di gruppo per propagare i criteri di restrizione software da Active Directory per i client appropriati e per la definizione dell'ambito e il filtro l'applicazione di questi criteri appropriati computer di destinazione.

-   API di attendibilità WinVerify che vengono usati per elaborare i file eseguibili firmati e Authenticode.

-   Visualizzatore eventi. Le funzioni utilizzate dagli eventi di log criteri restrizione software per il log del Visualizzatore eventi.

-   Risultante di criteri risultante (RSoP), che possono essere d'aiuto per la diagnosi del criterio valido che verrà applicato a un client.

Per altre informazioni sull'architettura di criteri di restrizione software, come criteri di restrizione software consente di gestire le regole, i processi e interazioni, vedere [come funzionano di criteri di restrizione Software](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx) nella libreria tecnica di Windows Server 2003.

## <a name="BKMK_Best_Practices"></a>Le procedure consigliate

### <a name="do-not-modify-the-default-domain-policy"></a>Non modificare il criterio dominio predefinito.

-   Se non si modifica il criterio dominio predefinito, sempre possibile riapplicare i criteri dominio predefiniti, se si verificano problemi con i criteri del dominio personalizzato.

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>Creare un oggetto Criteri di gruppo separati per i criteri di restrizione software.

-   Se si crea un separato oggetto (criteri di gruppo) per i criteri di restrizione software, è possibile disabilitare i criteri di restrizione software in caso di emergenza senza disabilitare il resto dei criteri di dominio.

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>Se si verificano problemi con le impostazioni dei criteri applicate, riavviare Windows in modalità provvisoria.

-   Criteri di restrizione software non si applicano quando Windows viene avviato in modalità provvisoria. Se si blocca accidentalmente una workstation con criteri di restrizione software, riavviare il computer in modalità provvisoria, accedere come amministratore locale, modificare i criteri, eseguire **gpupdate**, riavviare il computer e quindi eseguire l'accesso normalmente.

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>Prestare attenzione quando si definisce un'impostazione predefinita non consentito.

-   Quando si definisce un'impostazione predefinita **vietate**, tutto il software non è consentito, ad eccezione del software che è stato esplicitamente concesso. Deve avere una restrizione software i criteri della regola che consente di aprire qualsiasi file che si desidera aprire.

-   Per evitare che gli amministratori che bloccano il sistema, quando il livello di sicurezza predefinite è impostato su **vietate**, quattro regole di percorso del Registro di sistema vengono create automaticamente. È possibile eliminare o modificare queste regole di percorso del Registro di sistema; Tuttavia, questa operazione è sconsigliata.

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>Per una sicurezza ottimale, usare gli elenchi di controllo di accesso in combinazione con i criteri di restrizione software.

-   Gli utenti possono provare ad aggirare i criteri di restrizione software per la ridenominazione o spostamento di file non consentiti o sovrascrivendo i file senza restrizioni. Di conseguenza, è consigliabile utilizzare elenchi di controllo di accesso (ACL) per negare agli utenti l'accesso necessario per eseguire queste attività.

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>Testare accuratamente nuove impostazioni dei criteri in ambienti di test prima di applicare le impostazioni dei criteri per il dominio.

-   Nuove impostazioni dei criteri potrebbero comportarsi in modo diverso da quello previsto. Questa verifica riduce le probabilità che si verifichi un problema quando si distribuiscono le impostazioni dei criteri in tutta la rete di.

-   È possibile impostare un dominio di prova, separato dal dominio dell'organizzazione, in cui eseguire il test delle nuove impostazioni dei criteri. È anche possibile testare le impostazioni dei criteri tramite la creazione di un test di oggetto Criteri di gruppo e collegarlo a un'unità organizzativa di prova. Quando sono stati testati accuratamente le impostazioni dei criteri con gli utenti di test, è possibile collegare il test di oggetto Criteri di gruppo al dominio.

-   Non impostare i programmi o file da **vietate** senza eseguire il test per vedere quale potrebbe essere l'effetto. Restrizioni per determinati file possono gravemente compromettere il funzionamento del computer o della rete.

-   Le informazioni immesse in modo non corretto o errori di digitazione può comportare un'impostazione di criteri che non funzionare come previsto. Nuove impostazioni dei criteri di test prima di applicarle può impedire un comportamento imprevisto.

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>Filtrare le impostazioni di criteri utente basate sull'appartenenza a gruppi di sicurezza.

-   È possibile specificare gli utenti o gruppi per cui non si desidera un'impostazione di criteri da applicare, deselezionando il **Applica criteri di gruppo** e **lettura** caselle di controllo, che si trovano nel **sicurezza**scheda della finestra di dialogo proprietà per l'oggetto Criteri di gruppo.

-   Quando viene negata l'autorizzazione di lettura, l'impostazione dei criteri non viene scaricata dal computer. Di conseguenza, viene utilizzato meno larghezza di banda grazie al download impostazioni di criteri non necessari, che consente alla rete di funzione più rapidamente. Per negare l'autorizzazione di lettura, selezionare **Deny** per il **lettura** casella di controllo, che si trova sul **sicurezza** scheda della finestra di dialogo proprietà per l'oggetto Criteri di gruppo.

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>Non si collega a un oggetto Criteri di gruppo in un altro dominio o del sito.

-   Collegamento a un oggetto Criteri di gruppo in un altro dominio o del sito può comportare una riduzione delle prestazioni.

## <a name="additional-resources"></a>Risorse aggiuntive

|Tipo di contenuto|Riferimenti|
|--------|-------|
|**Pianificazione**|[Riferimento tecnico dei criteri di restrizione software](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)|
|**Operazioni**|[Amministrare criteri di restrizione Software](administer-software-restriction-policies.md)|
|**Risoluzione dei problemi**|[Criteri di restrizione software, la risoluzione dei problemi (2003)](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)|
|**Sicurezza**|[Minacce e contromisure per la restrizione Software criteri (2008)](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)<br /><br />[Minacce e contromisure per la restrizione Software criteri (2008 R2)](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)|
|**Strumenti e impostazioni**|[Criteri di restrizione software strumenti e impostazioni (2003)](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)|
|**Risorse della community**|[Blocco dell'applicazione con criteri di restrizione Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|


