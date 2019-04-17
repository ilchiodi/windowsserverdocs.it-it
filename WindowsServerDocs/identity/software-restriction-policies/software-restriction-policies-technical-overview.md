---
title: Software Restriction Policies Technical Overview
description: Protezione di Windows Server
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="software-restriction-policies-technical-overview"></a>Software Restriction Policies Technical Overview

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento vengono descritti i criteri di restrizione software quando e come usare la funzionalità, quali modifiche sono state implementate nelle versioni precedenti e vengono forniti collegamenti a risorse aggiuntive che consentono di creare e distribuire criteri di restrizione software a partire da Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introduzione
Criteri di restrizione software, gli amministratori offrono un meccanismo basato su criteri di gruppo per identificare il software e controllare la possibilità di esecuzione nel computer locale. Questi criteri possono essere utilizzati per proteggere i computer che eseguono sistemi operativi Microsoft Windows (a partire da Windows Server 2003 e Windows XP Professional) contro i conflitti noti e proteggere il computer da minacce di sicurezza, ad esempio virus e programmi Trojan horse. È inoltre possibile utilizzare criteri di restrizione software per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate eseguire. Criteri di restrizione software sono integrati in Microsoft Active Directory e criteri di gruppo. È inoltre possibile creare criteri di restrizione software in computer autonomi.

Criteri di restrizione software sono criteri di attendibilità, ovvero normative configurate dall'amministratore per limitare gli script e altro codice che non è completamente attendibile in esecuzione. L'estensione di criteri di restrizione Software a Editor criteri di gruppo locali offre una singola interfaccia utente attraverso la quale è possano gestire le impostazioni per limitare l'utilizzo di applicazioni nel computer locale o in un dominio.

## <a name="procedures"></a>Procedure

-   [Amministrare i criteri di restrizione Software](administer-software-restriction-policies.md)

    -   [Determinazione dell'elenco Consenti-Nega e l'inventario delle applicazioni per criteri di restrizione Software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [Utilizzo delle regole dei criteri di restrizione Software](work-with-software-restriction-policies-rules.md)

    -   [Utilizzare criteri di restrizione Software per proteggere il Computer da un Virus di posta elettronica](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [Risolvere i problemi relativi a criteri di restrizione Software](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>Scenari di utilizzo dei criteri di restrizione software
Gli utenti aziendali collaborano tramite posta elettronica, messaggistica istantanea e le applicazioni peer-to-peer. Quando queste collaborazioni aumentano, soprattutto con l'uso di Internet informatica aziendale, quindi eseguire le minacce da codice dannoso, ad esempio worm, virus e l'utente malintenzionato o minacce utente malintenzionato.

Gli utenti potrebbero ricevere il codice dannoso in varie forme, compreso tra nativi file eseguibili di Windows (file .exe), e le macro dei documenti (ad esempio file con estensione doc), agli script (ad esempio i file vbs). Utenti non autorizzati o malintenzionati usano spesso i metodi di ingegneria sociale per ottenere gli utenti di eseguire codice che include virus e worm. (Ingegneria sociale è un termine per indurre gli utenti a rivelare la password o altre informazioni di sicurezza). Se tale codice è attivato, è possibile generare attacchi denial of service nella rete, inviare i dati sensibili o privati a Internet, mettere a rischio la sicurezza del computer o danneggiare il contenuto dell'unità disco rigido.

Utenti e organizzazioni IT devono essere in grado di determinare quale software è sicuro per l'esecuzione e che non è. Con le un numero elevato e i moduli che può richiedere codice dannoso, questo diventa un'attività complessa.

Per proteggere i computer di rete da codice dannoso sia software sconosciuto o non supportato, le organizzazioni possono implementare criteri di restrizione software nell'ambito della strategia di sicurezza globale.

Gli amministratori possono utilizzare criteri di restrizione software per le attività seguenti:

-   Definire il codice attendibile

-   Progettazione di criteri di gruppo flessibili per definire script, file eseguibili e controlli ActiveX

Criteri di restrizione software vengono applicati dal sistema operativo e dalle applicazioni (ad esempio applicazioni di scripting) conformi ai criteri di restrizione software.

In particolare, gli amministratori possono utilizzare criteri di restrizione software per gli scopi seguenti:

-   Specificare il tipo di software (file eseguibili) possono essere eseguiti nei computer client

-   Impedire agli utenti di eseguire determinati programmi nei computer condivisi

-   Specificare chi può aggiungere autori attendibili ai computer client

-   Impostare l'ambito dei criteri di restrizione software (specificare se i criteri interessano tutti gli utenti o solo un sottoinsieme di utenti nei computer client)

-   Impedire che i file eseguibili in esecuzione nel computer locale, unità organizzativa (OU), sito o dominio. Questa funzionalità è utile quando non si utilizza criteri di restrizione software per risolvere i problemi potenziali a utenti malintenzionati.

## <a name="BKMK_Diffs_Changes"></a>Le differenze e le modifiche apportate alle funzionalità
Sono state apportate modifiche delle funzionalità in Criteri di restrizione software per Windows Server 2012 e Windows 8.

**Versioni supportate**

Criteri di restrizione software solo possono essere configurati in e applicati ai computer che eseguono almeno Windows Server 2003, incluso Windows Server 2012 e almeno Windows XP, incluso Windows 8.

> [!NOTE]
> Alcune edizioni di inizio del sistema operativo client Windows con Windows Vista non devono di criteri di restrizione Software. I computer non gestiti in un dominio da criteri di gruppo potrebbero non ricevere criteri distribuiti.

**Confronto delle funzioni di controllo delle applicazioni in Criteri di restrizione Software e AppLocker**

Nella tabella seguente vengono confrontate le funzionalità e le funzioni delle funzionalità Software criteri di restrizione e AppLocker.

|Funzione di controllo delle applicazioni|CRITERI DI RESTRIZIONE SOFTWARE|AppLocker|
|----------------|----|-------|
|Ambito|Criteri di restrizione software possono essere applicati a tutti i sistemi operativi Windows a partire da Windows XP e Windows Server 2003.|I criteri di AppLocker si applicano solo a Windows Server 2008 R2, Windows Server 2012, Windows 7 e Windows 8.|
|Creazione dei criteri|Criteri di restrizione software vengono gestiti tramite criteri di gruppo e solo l'amministratore dell'oggetto Criteri di gruppo possa aggiornare i criteri di restrizione software. L'amministratore del computer locale è possibile modificare i criteri di restrizione software definiti nell'oggetto Criteri di gruppo locali.|I criteri di AppLocker vengono gestiti tramite criteri di gruppo e solo l'amministratore dell'oggetto Criteri di gruppo possa aggiornare i criteri. L'amministratore del computer locale è possibile modificare i criteri di AppLocker definiti nell'oggetto Criteri di gruppo locali.<br /><br />AppLocker consente la personalizzazione dei messaggi di errore per indirizzare gli utenti a una pagina Web di Guida.|
|Gestione dei criteri|Criteri di restrizione software devono essere aggiornati tramite lo snap-in Criteri di sicurezza locali (se i criteri vengono creati in locale) o la Console Gestione criteri di gruppo (GPMC).|I criteri di AppLocker possono essere aggiornati tramite criteri di sicurezza locali snap-in (se i criteri vengono creati in locale), o la console GPMC o i cmdlet di Windows PowerShell AppLocker.|
|Applicazione dei criteri|Criteri di restrizione software vengono distribuiti tramite criteri di gruppo.|I criteri di AppLocker vengono distribuiti tramite criteri di gruppo.|
|Modalità di imposizione|SRP works in the "deny list mode" where administrators can create rules for files that they do not want to allow in this Enterprise whereas the rest of the file are allowed to run by default.<br /><br />SRP can also be configured in the "allow list mode" such that the by default all files are blocked and administrators need to create allow rules for files that they want to allow.|AppLocker by default works in the "allow list mode" where only those files are allowed to run for which there is a matching allow rule.|
|Tipi di file che possono essere controllati|Criteri di restrizione software possono controllare i tipi di file seguenti:<br /><br />-File eseguibili<br />-DLL<br />-Script<br />-Windows Installer<br /><br />Criteri di restrizione software non può controllare ogni tipo di file separatamente. Tutte le regole di criteri di restrizione software sono un insieme di singola regola.|AppLocker può controllare i tipi di file seguenti:<br /><br />-File eseguibili<br />-DLL<br />-Script<br />-Windows Installer<br />-App in pacchetto e programmi di installazione (Windows Server 2012 e Windows 8)<br /><br />AppLocker mantiene una raccolta regole separato per ciascuno dei tipi di cinque file.|
|Tipi di file designati|Criteri di restrizione software supporta un elenco dei tipi di file che sono considerati eseguibile estendibile. Gli amministratori possono aggiungere le estensioni di file che devono essere considerati come eseguibile.|AppLocker non supporta questa. AppLocker supporta attualmente le estensioni di file seguenti:<br /><br />-File eseguibili (.exe,. com)<br />-DLL (OCX, DLL)<br />-Script (vbs, con estensione js, ps1, cmd, bat)<br />-Windows Installer (file con estensione msi, MST e msp)<br />-Programmi di installazione di app in pacchetto (con estensione AppX)|
|Tipi di regole|Criteri di restrizione software sono supportati quattro tipi di regole:<br /><br />-Hash<br />-Path<br />-Firma<br />-Area Internet|AppLocker supporta tre tipi di regole:<br /><br />-Hash<br />-Path<br />-Publisher|
|Modifica il valore hash|Criteri di restrizione software consente agli amministratori di specificare i valori hash personalizzato.|AppLocker calcola il valore hash se stesso. Per file eseguibili (file Exe e Dll) e i programmi di installazione di Windows e un hash di file flat SHA1 per il resto viene utilizzata internamente l'hash SHA1 Authenticode.|
|Supporto per diversi livelli di protezione|Con criteri di restrizione software, gli amministratori possono specificare le autorizzazioni con cui è possibile eseguire un'app. Pertanto, un amministratore può configurare una regola di questo tipo che blocco note viene sempre eseguito con autorizzazioni limitate e mai con privilegi amministrativi.<br /><br />Criteri di restrizione software in Windows Vista e versioni precedenti supportata più livelli di sicurezza. In Windows 7 che elenca stato limitato a due livelli: non consentito e illimitato (utente di base equivale a non consentito).|AppLocker non supporta i livelli di protezione.|
|Gestire App in pacchetto e programmi di installazione app in pacchetto|Impossibile|file con estensione AppX è un tipo di file valido che è possibile gestire AppLocker.|
|Una regola a un utente o un gruppo di utenti di destinazione|Regole di criteri di restrizione software si applicano a tutti gli utenti in un determinato computer.|Le regole di AppLocker possono essere assegnate a un utente specifico o un gruppo di utenti.|
|Supporto per le eccezioni alla regola|Criteri di restrizione software non supporta le eccezioni alla regola|AppLocker rules can have exceptions which allow administrators to create rules such as "Allow everything from Windows except for Regedit.exe".|
|Supporto per la modalità di controllo|Criteri di restrizione software non supporta la modalità di controllo. L'unico modo per testare i criteri di restrizione software consiste nell'impostare un ambiente di test ed eseguire esperimenti pochi.|AppLocker supporta la modalità di controllo che consente agli amministratori di testare l'effetto dei relativi criteri nell'ambiente di produzione reali senza alcun impatto l'esperienza utente. Una volta soddisfatto con i risultati, puoi iniziare a imporre i criteri.|
|Supporto per l'esportazione e importazione dei criteri|Criteri di restrizione software non supporta l'importazione/esportazione dei criteri.|AppLocker supporta l'importazione ed esportazione di criteri. Questo consente di creare criteri di AppLocker in un computer di esempio, eseguire il test e quindi esportare tali criteri e importarlo indietro nell'oggetto Criteri di gruppo desiderato.|
|Imposizione delle regole|Internamente, l'imposizione delle regole di criteri di restrizione software avviene in modalità utente che è meno sicura.|Internamente, vengono applicate le regole di AppLocker per i file exe e DLL in modalità kernel che è più sicura di applicarle in modalità utente.|

## <a name="system-requirements"></a>Requisiti di sistema
Criteri di restrizione software solo possono essere configurati in e applicati ai computer che eseguono almeno Windows Server 2003 e almeno Windows XP. Criteri di gruppo sono necessaria per distribuire gli oggetti Criteri di gruppo che contengono i criteri di restrizione software.

## <a name="software-restriction-policies-components-and-architecture"></a>Architettura e componenti di criteri di restrizione software
Criteri di restrizione software forniscono un meccanismo per il sistema operativo e applicazioni compatibili con criteri di restrizione software per limitare l'esecuzione di runtime di programmi software.

In generale, i criteri di restrizione software sono costituiti dai componenti seguenti:

-   Criteri di restrizione software API. Application Programming Interface (API) vengono utilizzate per creare e configurare le regole che costituiscono i criteri di restrizione software. Esistono anche API per l'esecuzione di query, l'elaborazione i criteri di restrizione software e l'applicazione di criteri restrizione software.

-   Strumento Gestione criteri restrizione software. Si tratta del **criteri di restrizione Software** estensione del **Editor oggetti Criteri di gruppo locali** snap-in, utilizzabili dagli amministratori di creare e modificare i criteri di restrizione software.

-   Un set di API del sistema operativo e applicazioni che chiama i criteri di restrizione software API per fornire l'imposizione di criteri di restrizione software in fase di esecuzione.

-   Active Directory e criteri di gruppo. Criteri di restrizione software dipendono dall'infrastruttura Criteri di gruppo per propagare i criteri di restrizione software da Active Directory per i client appropriati e di definizione dell'ambito e filtrare l'applicazione di questi criteri ai computer di destinazione appropriato.

-   Authenticode e WinVerify Trust API che vengono usate per elaborare i file eseguibili firmati.

-   Visualizzatore eventi. Le funzioni utilizzate dagli eventi di log criteri restrizione software per i registri del Visualizzatore eventi.

-   Risultante di criteri risultante (RSoP), che possono facilitare la diagnosi del criterio valido che verrà applicato a un client.

For more information about SRP architecture, how SRP manages rules, processes and interactions, see [How Software Restriction Policies Work](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx) in the Windows Server 2003 Technical Library.

## <a name="BKMK_Best_Practices"></a>Procedure consigliate

### <a name="do-not-modify-the-default-domain-policy"></a>Non modificare il criterio dominio predefinito.

-   Se non si modifica il criterio dominio predefinito, hai sempre la possibilità di riapplicare il criterio dominio predefinito se qualcosa non funziona con i criteri di dominio personalizzato.

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>Creare un oggetto Criteri di gruppo separati per i criteri di restrizione software.

-   Se si crea un separato oggetto (criteri di gruppo) per i criteri di restrizione software, è possibile disabilitare i criteri di restrizione software in caso di emergenza senza disattivare il resto dei criteri di dominio.

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>Se si verificano problemi con le impostazioni dei criteri applicate, riavvio di Windows in modalità provvisoria.

-   Non applicano i criteri di restrizione software quando Windows viene avviato in modalità provvisoria. Se si bloccano accidentalmente una workstation con criteri di restrizione software, riavviare il computer in modalità provvisoria, accedere come amministratore locale, modificare i criteri, eseguire **gpupdate**, riavviare il computer e quindi accedere normalmente.

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>Prestare attenzione quando si definiscono il valore predefinito non consentito.

-   Quando si definisce un'impostazione di **non consentito**, tutti i software non è consentito, ad eccezione di software che è stato consentito in modo esplicito. È necessario che una restrizione software criteri della regola che consente di aprire qualsiasi file che si desidera aprire.

-   Per evitare che gli amministratori che bloccano il sistema, quando il livello di sicurezza predefinito è impostato su **non consentito**, quattro regole di percorso del Registro di sistema vengono create automaticamente. È possibile eliminare o modificare queste regole di percorso del Registro di sistema. Tuttavia, questo non è consigliato.

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>Per una protezione ottimale, utilizzare gli elenchi di controllo di accesso in combinazione con criteri di restrizione software.

-   Gli utenti potrebbero tentare di aggirare i criteri di restrizione software per rinominare o spostare file non consentiti o sovrascrivere i file senza restrizioni. Di conseguenza, è consigliabile utilizzare elenchi di controllo di accesso (ACL) per negare agli utenti l'accesso necessario per eseguire queste attività.

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>Testare attentamente nuove impostazioni di criteri in ambienti di test prima di applicare le impostazioni dei criteri al dominio.

-   Nuove impostazioni di criteri potrebbero funzionare in modo diverso da quello previsto. Questa verifica riduce il rischio di problemi durante la distribuzione delle impostazioni della rete.

-   È possibile impostare un dominio di prova, separato dal dominio dell'organizzazione, in cui testare nuove impostazioni di criteri. Creazione di un oggetto Criteri di gruppo di test e collegandolo a un'unità organizzativa di test, è inoltre possibile testare le impostazioni dei criteri. Quando è stato testato accuratamente le impostazioni dei criteri con gli utenti di test, è possibile collegare l'oggetto Criteri di gruppo di test per il dominio.

-   Non impostare i programmi o file per **non consentito** senza eseguire il test per verificare che l'effetto è possibile. Restrizioni per determinati file possono comportare seri il funzionamento del computer o in rete.

-   Le informazioni immesse in modo errato o errori di digitazione può causare un'impostazione di criteri che non esegue come previsto. Test delle nuove impostazioni di criteri prima di applicarle, è possibile evitare questo problema.

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>Filtrare le impostazioni di criteri utente in base all'appartenenza a gruppi di sicurezza.

-   È possibile specificare gli utenti o gruppi per cui non si desidera un'impostazione dei criteri da applicare mediante la cancellazione di **Applica criteri di gruppo** e **lettura** caselle di controllo, che si trovano nel **sicurezza** scheda della finestra di dialogo proprietà per l'oggetto Criteri di gruppo.

-   Quando viene negata l'autorizzazione di lettura, l'impostazione di criteri non viene scaricata dal computer. Di conseguenza, minore larghezza di banda viene utilizzata dai download delle impostazioni di criteri non necessari, che consente la rete risulta più rapidamente. Per negare l'autorizzazione di lettura, selezionare **Nega** per il **lettura** casella di controllo che si trova nel **sicurezza** scheda della finestra di dialogo proprietà per l'oggetto Criteri di gruppo.

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>Non si collega a un oggetto Criteri di gruppo in un altro dominio o del sito.

-   Il collegamento a un oggetto Criteri di gruppo in un altro dominio o del sito può influire negativamente sulle prestazioni.

## <a name="additional-resources"></a>Risorse aggiuntive

|Tipo di contenuto|Riferimenti|
|--------|-------|
|**Pianificazione**|[Riferimento tecnico criteri di restrizione software](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)|
|**Operazioni**|[Amministrare i criteri di restrizione Software](administer-software-restriction-policies.md)|
|**Risoluzione dei problemi**|[Criteri di restrizione software risoluzione dei problemi (2003)](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)|
|**Protezione**|[Minacce e contromisure per restrizione Software criteri (2008)](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)<br /><br />[Minacce e contromisure per restrizione Software criteri (2008 R2)](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)|
|**Strumenti e impostazioni**|[Criteri di restrizione software strumenti e impostazioni (2003)](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)|
|**Risorse della community**|[Blocco dell'applicazione con criteri di restrizione Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|


