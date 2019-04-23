---
ms.assetid: 249ba1be-b0d3-4a77-99af-3699074a2b6e
title: Risoluzione dei problemi dei controller di dominio virtualizzati
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ca58d36599be76e4d196d91f298b9841884e7a36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863662"
---
# <a name="virtualized-domain-controller-troubleshooting"></a>Risoluzione dei problemi dei controller di dominio virtualizzati

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento fornisce una metodologia dettagliata per la risoluzione dei problemi della funzionalità dei controller di dominio virtualizzati.  
  
-   [Risoluzione dei problemi di virtualizzare la clonazione di controller di dominio](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCCloning)  
  
-   [Risoluzione dei problemi di ripristino sicuro di controller di dominio virtualizzati](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCSafeRestore)  
  
## <a name="BKMK_Intro"></a>Introduzione  
Per migliorare le proprie competenze nella risoluzione dei problemi, è molto importante creare un laboratorio di test ed esaminare in modo rigoroso normali scenari operativi. Gli eventuali errori riscontrati saranno più ovvi e facili da interpretare, in quanto a quel punto si avranno solide nozioni di base sul funzionamento della promozione dei controller di dominio. Sarà anche possibile consolidare le proprie competenze sull'analisi, in particolare della rete. Questo vale per tutte le tecnologie di sistemi distribuiti, non solo per la distribuzione di controller di dominio.  
  
Gli elementi cruciali per la risoluzione avanzata dei problemi di configurazione dei controller di dominio sono i seguenti:  
  
1.  Analisi lineare abbinata a una particolare attenzione per i dettagli  
  
2.  Analisi dell'acquisizione di rete  
  
3.  Interpretazione dei log predefiniti  
  
Il primo e il secondo elemento esulano dall'abito di questo argomento, mentre il terzo può essere illustrato in maggior dettaglio. La risoluzione dei problemi dei controller di dominio virtualizzati richiede un metodo logico e lineare. La chiave consiste nell'affrontare il problema usando i dati forniti e ricorrere a strumenti e processi di analisi complessi solo dopo aver esaurito l'output e le registrazioni disponibili.  
  
## <a name="BKMK_TshootVDCCloning"></a>Risoluzione dei problemi di virtualizzare la clonazione di controller di dominio  
In questa sezione vengono trattati gli argomenti seguenti:  
  
-   [Strumenti per la risoluzione dei problemi](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_Tools)  
  
-   [Opzioni di registrazione](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_LoggingOptions)  
  
-   [Metodologia generale per la risoluzione dei problemi di dominio la clonazione del Controller](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)  
  
-   [Server Core e il registro eventi](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_ServerCoreEvents)  
  
-   [Risoluzione di problemi specifici](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_SpecificProblems)  
  
La strategia di risoluzione dei problemi per la clonazione di controller di dominio personalizzati segue questo formato generale:  
  
![risoluzione dei problemi controller di dominio virtuale](media/Virtualized-Domain-Controller-Troubleshooting/ADDS_VDC_TroublehsootingFlowchart.png)  
  
### <a name="BKMK_Tools"></a>Strumenti per la risoluzione dei problemi  
  
#### <a name="BKMK_LoggingOptions"></a>Opzioni di registrazione  
I log predefiniti rappresentano lo strumento più importante per la risoluzione dei problemi di clonazione dei controller di dominio. Per impostazione predefinita, tutti questi log sono abilitati e configurati per il massimo livello di dettaglio.  
  
|||  
|-|-|  
|**Operazione**|**Log**|  
|**La clonazione**|-Visualizzatore eventi\log di Windows\Sistema<br />-Event Visualizzatore eventi\log di applicazioni e servizi\servizio directory<br />-   %systemroot%\debug\dcpromo.log|  
|**Innalzamento di livello**|-   %systemroot%\debug\dcpromo.log<br />-Event Visualizzatore eventi\log di applicazioni e servizi\servizio directory<br />-Visualizzatore eventi\log di Windows\Sistema<br />-Event Visualizzatore eventi\log di applicazioni e servizi servizi\servizio replica file<br />-Event Visualizzatore eventi\log di applicazioni e servizi\replica DFS|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Strumenti e comandi per la risoluzione dei problemi di configurazione dei controller di dominio  
Per risolvere i problemi non spiegati dai log, usare gli strumenti seguenti come punto di partenza:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Network Monitor 3.4  
  
### <a name="BKMK_GeneralMethodology"></a>Metodologia generale per la risoluzione dei problemi di dominio la clonazione del Controller  
  
1.  Se la VM viene avviata in modalità ripristino servizi directory (DSRM, Directory Service Repair Mode), significa che è necessario implementare procedure di risoluzione dei problemi. Per accedere in modalità DSRM, usare l'account **.\Administrator** e specificare l'apposita password.  
  
    1.  Esaminare il log Dcpromo.log.  
  
        1.  I passaggi iniziali di clonazione sono stati completati correttamente ma la promozione del controller di dominio non è riuscita?  
  
        2.  Gli errori indicano problemi con il controller di dominio locale o con l'ambiente di Servizi di dominio Active Directory, ad esempio errori restituiti dall'emulatore PDC?  
  
    2.  Esaminare i registri eventi di sistema e dei servizi directory, oltre ai file dccloneconfig.xml e CustomDCCloneAllowList.xml.  
  
        1.  Un'applicazione incompatibile deve essere inserita nell'elenco di elementi consentiti CustomDCCloneAllowList.xml?  
  
        2.  L'indirizzo IP o il nome computer è duplicato o invalido in dccloneconfig.xml?  
  
        3.  Il sito Active Directory non è valido in dccloneconfig.xml?  
  
        4.  L'indirizzo IP non è impostato in dccloningconfig.xml e non c'è nessun server DHCP disponibile?  
  
        5.  L'emulatore PDC è online e disponibile tramite il protocollo RPC?  
  
        6.  Il controller di dominio è un membro del gruppo Controller di dominio clonabili? L'autorizzazione **Consenti a un controller di dominio di creare un clone di se stesso** è impostata nella radice di dominio del gruppo?  
  
        7.  Il file Dccloneconfig.xml contiene errori di sintassi che impediscono l'analisi corretta?  
  
        8.  L'hypervisor è supportato?  
  
        9. La promozione del controller di dominio non è riuscita dopo il corretto avvio della clonazione?  
  
        10. È stato superato il numero massimo di nomi di controller di dominio generati automaticamente (9999)?  
  
        11. L'indirizzo MAC è duplicato?  
  
2.  Il nome host del clone è uguale al controller di dominio di origine?  
  
    1.  C'è un file Dccloneconfig.xml in uno dei percorsi consentiti?  
  
3.  La VM viene avviata in modalità normale e la clonazione è completata, ma il controller di dominio non funziona correttamente?  
  
    1.  Controllare prima di tutto se il nome host è stato cambiato nel clone. Se il nome host è diverso, la clonazione è stata almeno parzialmente completata.  
  
    2.  Il controller di dominio ha un indirizzo IP duplicato del controller di dominio di origine nel file dccloneconfig.xml, ma il controller di dominio di origine era offline durante la clonazione?  
  
    3.  Se il controller di dominio invia annunci, trattare il problema come qualsiasi altro problema di post-promozione che potrebbe verificarsi senza clonazione.  
  
    4.  Se il controller di dominio non invia annunci, esaminare i registri eventi del servizio directory, di sistema, dell'applicazione, di Replica file e di Replica DFS per rilevare eventuali errori di post-promozione.  
  
#### <a name="disabling-dsrm-boot"></a>Disabilitazione dell'avvio in modalità DSRM  
Dopo l'avvio in modalità DSRM a causa di un errore, diagnosticare la causa del problema e, se il file dcpromo.log non indica che la clonazione non può essere ritentata, correggere la causa dell'errore e reimpostare il flag della modalità DSRM. Un clone non riuscito non torna in modalità normale automaticamente al successivo riavvio, ma è necessario rimuovere il flag di avvio in modalità DSRM per provare di nuovo a eseguire la clonazione. Tutti questi passaggi richiedono l'esecuzione con privilegi elevati di amministratore.  
  
##### <a name="removing-dsrm-with-msconfigexe"></a>Rimozione della modalità DSRM con Msconfig.exe  
Per disattivare l'avvio in modalità DSRM con una GUI, usare lo strumento Configurazione di sistema:  
  
1.  Eseguire msconfig.exe.  
  
2.  Nella scheda **Avvio**, in **Opzioni di avvio**, deselezionare **Modalità provvisoria**. Questa opzione è già selezionata con l'opzione **Ripristina Active Directory** abilitata.  
  
3.  Fare clic su OK e riavviare quando richiesto.  
  
##### <a name="removing-dsrm-with-bcdeditexe"></a>Rimozione della modalità DSRM con Bcdedit.exe  
Per disattivare l'avvio in modalità DSRM dalla riga di comando, usare l'editor dell'archivio dati di configurazione di avvio:  
  
1.  Aprire un prompt dei comandi ed eseguire:  
  
    ```  
    Bcdedit.exe /deletevalue safeboot  
    ```  
  
2.  Riavviare il computer con:  
  
    ```  
    Shutdown.exe /t /0 /r  
    ```  
  
> [!NOTE]  
> Bcdedit.exe funziona anche in una console di Windows PowerShell. I comandi in questo caso sono i seguenti:  
>   
> Bcdedit.exe /deletevalue safeboot  
>   
> Restart-computer  
  
### <a name="BKMK_ServerCoreEvents"></a>Server Core e il registro eventi  
I registri eventi contengono molte informazioni utili sulle operazioni di clonazione dei controller di dominio virtualizzati. Per impostazione predefinita, l'installazione di un computer Windows Server 2012 è un'installazione Server Core, il che significa che non c'è nessuna interfaccia grafica e, pertanto, nessun modo per eseguire lo snap-in Visualizzatore eventi locale.  
  
Per esaminare i registri eventi in un server con un'installazione Server Core:  
  
-   Eseguire lo strumento Wevtutil.exe in locale.  
  
-   Eseguire il cmdlet Get-WinEvent di PowerShell in locale  
  
-   Se è stato abilitato le regole di Windows Firewall con sicurezza avanzata per i gruppi "Gestione remota registro eventi" (o porte equivalenti) consentire la comunicazione in ingresso, è possibile gestire il registro eventi in modalità remota tramite Eventvwr.exe, wevtutil.exe o Get-Winevent. Questa operazione può essere eseguita nell'installazione Server Core usando NETSH.exe, Criteri di gruppo il nuovo cmdlet Set-NetFirewallRule di Windows PowerShell 3.0.  
  
> [!WARNING]  
> Non tentare di riaggiungere la shell grafica al computer mentre è in modalità DSRM. Lo stack di manutenzione di Windows (CBS) non funziona correttamente in modalità provvisoria o DSRM. I tentativi di aggiungere funzionalità o ruoli in modalità DSRM non verranno completati e il computer rimarrà in uno stato instabile finché non verrà avviato normalmente. Poiché un clone di controller di dominio virtualizzato in modalità DSRM non può (e nella maggior parte dei casi non deve) essere avviato normalmente, è impossibile aggiungere correttamente la shell grafica. Questa operazione non è supportata e il server potrebbe diventare inutilizzabile.  
  
### <a name="BKMK_SpecificProblems"></a>Risoluzione di problemi specifici  
  
#### <a name="events"></a>Eventi  
Tutti gli eventi di clonazione dei controller di dominio virtualizzati vengono scritti nel registro eventi dei servizi directory della macchina virtuale del controller di dominio clone. Anche i registri eventi dell'applicazione, del servizio Replica file e del servizio Replica DFS possono contenere informazioni utili per la risoluzione dei problemi di clonazione. Gli errori che si verificano durante la chiamata RPC all'emulatore PDC possono essere disponibili nel registro eventi sull'emulatore PDC.  
  
Di seguito sono riportati gli eventi specifici della clonazione di Windows Server 2012 registrati nel registro eventi dei servizi directory, con note e suggerimenti per la risoluzione degli errori.  
  
##### <a name="directory-services-event-log"></a>Registro eventi dei servizi directory  
  
|||  
|-|-|  
|**ID evento**|**2160**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|Locale *<COMPUTERNAME>* ha individuato un controller di dominio virtuale i file di configurazione della clonazione.<br /><br />Percorso del file di configurazione: %1<br /><br />L'esistenza del file di configurazione della clonazione del controller di dominio virtuale indica che il controller di dominio virtuale locale è un clone di un altro controller di dominio virtuale. Il *<COMPUTERNAME>* inizieranno a autoclonazione.|  
|**Note e risoluzione**|Si tratta di un evento riuscito ed è un problema solo se imprevisto. Esaminare la directory di lavoro DSA, %systemroot%\ntds, e la radice di eventuali dischi locali o rimovibili cercando il file dcclconeconfig.xml.|  
  
|||  
|-|-|  
|**ID evento**|**2161**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|Locale *<COMPUTERNAME>* non ha individuato il controller di dominio virtuale i file di configurazione della clonazione. Il computer locale non è un controller di dominio clonato.|  
|**Note e risoluzione**|Si tratta di un evento riuscito ed è un problema solo se imprevisto. Esaminare la directory di lavoro DSA, %systemroot%\ntds, e la radice di eventuali dischi locali o rimovibili cercando il file dcclconeconfig.xml.|  
  
|||  
|-|-|  
|**ID evento**|**2162**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|Clonazione del controller di dominio virtuale non riuscita.<br /><br />Per ulteriori informazioni sugli errori corrispondenti al tentativo di clonazione del controller di dominio virtuale, controllare gli eventi registrati nei registri eventi di sistema e in %systemroot%\debug\dcpromo.log.<br /><br />Codice di errore: %1|  
|**Note e risoluzione**|Seguire le istruzioni del messaggi, questo errore è generico.|  
  
|||  
|-|-|  
|**ID evento**|**2163**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|È stato avviato il servizio DsRoleSvc per clonare il controller di dominio virtuale locale.|  
|**Note e risoluzione**|Si tratta di un evento riuscito ed è un problema solo se imprevisto. Esaminare la directory di lavoro DSA, %systemroot%\ntds, e la radice di eventuali dischi locali o rimovibili cercando il file dcclconeconfig.xml.|  
  
|||  
|-|-|  
|**ID evento**|**2164**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|*<COMPUTERNAME>* Impossibile avviare il servizio DsRoleSvc per clonare il controller di dominio virtuale locale.|  
|**Note e risoluzione**|Esaminare le impostazioni del servizio DsRoleSvc (DS Role Server Service) e assicurarsi che il tipo di avvio sia impostato su manuale. Verificare che nessun programma di terze parti impedisca l'avvio del servizio.|  
  
|||  
|-|-|  
|**ID evento**|**2165**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|*<COMPUTERNAME>* non è stato possibile avviare un thread durante la clonazione del controller di dominio virtuale locale.<br /><br />Codice errore:%1<br /><br />Messaggio di errore:%2<br /><br />Nome thread:%3|  
|**Note e risoluzione**|Contattare il Servizio Supporto Tecnico Clienti Microsoft.|  
  
|||  
|-|-|  
|**ID evento**|**2166**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|*<COMPUTERNAME>* è necessario il servizio RPCSS per avviare il riavvio in modalità DSRM. In attesa di inizializzazione di RPCSS in uno stato di esecuzione non riuscita.<br /><br />Codice errore:%1|  
|**Note e risoluzione**|Esaminare il registro eventi di sistema e le impostazioni del servizio RPCSS (RPC Server Service).|  
  
|||  
|-|-|  
|**ID evento**|**2167**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|*<COMPUTERNAME>* Impossibile inizializzare informazioni del controller di dominio virtuale. Per informazioni, vedere la voce precedente del registro eventi.<br /><br />Dati aggiuntivi<br /><br />Codice di errore:%1|  
|**Note e risoluzione**|Seguire le istruzioni del messaggi, questo errore è generico.|  
  
|||  
|-|-|  
|**ID evento**|**2168**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|Microsoft-Windows-ActiveDirectory_DomainService<br /><br />Il controller di dominio è in esecuzione in un hypervisor supportato. Rilevato ID di generazione macchina virtuale.<br /><br />Valore corrente dell'ID di generazione macchina virtuale: %1|  
|**Note e risoluzione**|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|**ID evento**|**2169**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|Nessun ID di generazione macchina virtuale rilevato. Il controller di dominio è ospitato in un computer fisico, una versione di livello inferiore di Hyper-V o un hypervisor che non supporta l'ID di generazione macchina virtuale.<br /><br />Dati aggiuntivi<br /><br />Codice di errore restituito alla verifica dell'ID di generazione macchina virtuale:%1|  
|**Note e risoluzione**|Si tratta di un evento riuscito se non si intende clonare. In caso contrario, esaminare il registro eventi di sistema e consultare la documentazione di supporto del prodotto hypervisor.|  
  
|||  
|-|-|  
|**ID evento**|**2170**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Avviso|  
|**Messaggio**|È stata rilevata una modifica all'ID di generazione.<br /><br />ID generazione memorizzato nella cache in DS (valore precedente):%1<br /><br />ID generazione attualmente nella macchina virtuale (nuovo valore):%2<br /><br />La modifica dell'ID di generazione si verifica dopo l'applicazione di uno snapshot della macchina virtuale, dopo un'operazione di importazione di una macchina virtuale o dopo un'operazione Live Migration. *<COMPUTERNAME>* verrà creato un nuovo ID di chiamata per ripristinare il controller di dominio. I controller di dominio virtualizzati non devono essere ripristinati mediante snapshot della macchina virtuale. Il metodo supportato per il ripristino o il rollback del contenuto di un database di Servizi di dominio Active Directory consiste nel ripristinare un backup dello stato del sistema creato con un'applicazione di backup abilitata per Servizi di dominio Active Directory.|  
|**Note e risoluzione**|Si tratta di un evento riuscito se si intende clonare. In caso contrario, esaminare il registro eventi di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2171**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|Nessuna modifica all'ID di generazione rilevata.<br /><br />ID generazione memorizzato nella cache in DS (valore precedente):%1<br /><br />ID generazione attualmente nella macchina virtuale (nuovo valore):%2|  
|**Note e risoluzione**|Si tratta di un evento riuscito se non si intende clonare e dovrebbe essere visualizzato a ogni riavvio di un controller di dominio virtualizzato. In caso contrario, esaminare il registro eventi di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2172**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|Lettura dell'attributo msDS-GenerationId dell'oggetto computer del controller di dominio.<br /><br />Valore dell'attributo msDS-GenerationId:%1|  
|**Note e risoluzione**|Si tratta di un evento riuscito se si intende clonare. In caso contrario, esaminare il registro eventi di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2173**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|Impossibile leggere l'attributo msDS-GenerationId dell'oggetto computer del controller di dominio. Il problema potrebbe essere causato da un errore di transazione di database o dall'assenza dell'ID di generazione dal database locale. Il DS-GenerationId non esiste durante il primo riavvio dopo l'esecuzione di dcpromo oppure il controller di dominio non è un controller di dominio virtuale.<br /><br />Dati aggiuntivi<br /><br />Codice di errore:%1|  
|**Note e risoluzione**|Si tratta di un evento riuscito se si intende clonare e se è il primo riavvio della VM dopo il completamento della clonazione. Può anche essere ignorato in controller di dominio non virtuali. In caso contrario, esaminare il registro eventi di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2174**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|l controller di dominio non è un clone di controller di dominio virtuale né uno snapshot di controller di dominio virtuale ripristinato.|  
|**Note e risoluzione**|Si tratta di un evento riuscito se non si intende clonare. In caso contrario, esaminare il registro eventi di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2175**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|File di configurazione del clone di controller di dominio virtuale esistente in una piattaforma non supportata.|  
|**Note e risoluzione**|Questo problema si verifica quando viene trovato un file dccloneconfig.xml, ma non è stato possibile trovare un ID di generazione VM, ad esempio quando il file dccloneconfig.xml si trova in un computer fisico o in un hypervisor che non supporta l'ID di generazione VM.|  
  
|||  
|-|-|  
|**ID evento**|**2176**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|File di configurazione del clone del controller di dominio virtuale rinominato.<br /><br />Dati aggiuntivi<br /><br />Nome file precedente:%1<br /><br />Nuovo nome file:%2|  
|**Note e risoluzione**|La ridenominazione è prevista quando si avvia il backup di una macchina virtuale di origine, perché l'ID di generazione VM non è cambiato. In questo modo si impedisce al controller di dominio di origine di tentare una clonazione.|  
  
|||  
|-|-|  
|**ID evento**|**2177**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|Ridenominazione del file di configurazione del clone del controller di dominio virtuale non riuscita.<br /><br />Dati aggiuntivi<br /><br />Nome file:%1<br /><br />Codice di errore:%2 %3|  
|**Note e risoluzione**|Il tentativo di ridenominazione è previsto quando si avvia il backup di una macchina virtuale di origine, perché l'ID di generazione VM non è cambiato. In questo modo si impedisce al controller di dominio di origine di tentare una clonazione. Rinominare manualmente il file e verificare se prodotti di terze parti installati possono impedire questa operazione.|  
  
|||  
|-|-|  
|**ID evento**|**2178**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|È stato rilevato un file di configurazione di un clone di controller di dominio virtuale, ma l'ID di generazione della macchina virtuale non è cambiato. Il controller di dominio locale è il controller di dominio di origine clone. Rinominare il file di configurazione del clone.|  
|**Note e risoluzione**|Questa situazione è prevista quando si avvia il backup di una macchina virtuale di origine, perché l'ID di generazione VM non è cambiato. In questo modo si impedisce al controller di dominio di origine di tentare una clonazione.|  
  
|||  
|-|-|  
|**ID evento**|**2179**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|L'attributo msDS-GenerationId dell'oggetto computer del controller di dominio è stato impostato sul parametro seguente:<br /><br />Attributo GenerationID:%1|  
|**Note e risoluzione**|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|**ID evento**|**2180**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Avviso|  
|**Messaggio**|Impossibile impostare l'attributo msDS-GenerationId dell'oggetto computer del controller di dominio.<br /><br />Dati aggiuntivi<br /><br />Codice di errore:%1|  
|**Note e risoluzione**|Esaminare il registro eventi di sistema e Dcpromo.log Cercare l'errore specifico in MS TechNet, nella Microsoft Knowledge Base e nei blog di Microsoft per determinarne il significato solito e quindi risolvere il problema in base a questi risultati.|  
  
|||  
|-|-|  
|**ID evento**|**2182**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|Evento interno: al servizio directory è stato richiesto di clonare un DSA remoto:|  
|**Note e risoluzione**|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|**ID evento**|**2183**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|Evento interno: *<COMPUTERNAME>* ha completato la richiesta di clonare l'oggetto Directory System Agent remoto.<br /><br />Nome del controller di dominio originale:%3<br /><br />Nome del controller di dominio clone richiesto:%4<br /><br />Sito del controller di dominio clone richiesto:%5<br /><br />Dati aggiuntivi<br /><br />Valore errore:%1 %2|  
|**Note e risoluzione**|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|**ID evento**|**2184**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|*<COMPUTERNAME>* non è stato possibile creare un account di controller di dominio per il controller di dominio clonato.<br /><br />Nome del controller di dominio originale: %1<br /><br />Numero consentito di controller di dominio clonati:%2<br /><br />Il limite sul numero di account di controller di dominio che è possibile generare clonando *<COMPUTERNAME>* è stata superata.|  
|**Note e risoluzione**|Un singolo nome di controller di dominio di origine può essere generato automaticamente solo 9999 volte se i controller di dominio non sono abbassati di livello, in base alla convenzione di denominazione. Usare l'elemento <computername> nel codice XML per generare un nuovo nome univoco o effettuare la clonazione da un controller di dominio con un nome diverso.|  
  
|||  
|-|-|  
|**ID evento**|**2191**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>* impostare il valore del Registro di sistema seguente per disabilitare gli aggiornamenti DNS.<br /><br />Chiave del Registro di sistema:%1<br /><br />Valore del Registro di sistema: %2<br /><br />Dati del valore del Registro di sistema: %3<br /><br />Durante il processo di clonazione, il computer locale può avere lo stesso nome computer del computer di origine clone per un breve periodo di tempo. La registrazione di record A e AAAA di DNS è disabilitata durante questo periodo, quindi i client non possono inviare richieste al computer locale sottoposto a clonazione. Con il processo di clonazione, i nuovi aggiornamenti del DNS dopo la clonazione vengono completati.|  
|**Note e risoluzione**|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|**ID evento**|**2192**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|*<COMPUTERNAME>* Impossibile impostare il valore del Registro di sistema seguente per disabilitare gli aggiornamenti DNS.<br /><br />Chiave del Registro di sistema:%1<br /><br />Valore del Registro di sistema: %2<br /><br />Dati del valore del Registro di sistema: %3<br /><br />Codice errore: %4<br /><br />Messaggio di errore: %5<br /><br />Durante il processo di clonazione, il computer locale può avere lo stesso nome computer del computer di origine clone per un breve periodo di tempo. La registrazione di record A e AAAA di DNS è disabilitata durante questo periodo, quindi i client non possono inviare richieste al computer locale sottoposto a clonazione.|  
|**Note e risoluzione**|Esaminare i registri eventi di sistema e dell'applicazione. Verificare la presenza di un'applicazione di terze parti che potrebbe bloccare gli aggiornamenti del Registro di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2193**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>* impostare il valore del Registro di sistema seguente per abilitare gli aggiornamenti DNS.<br /><br />Chiave del Registro di sistema:%1<br /><br />Valore del Registro di sistema: %2<br /><br />Dati del valore del Registro di sistema: %3<br /><br />Durante il processo di clonazione, il computer locale può avere lo stesso nome computer del computer di origine clone per un breve periodo di tempo. La registrazione di record A e AAAA di DNS è disabilitata durante questo periodo, quindi i client non possono inviare richieste al computer locale sottoposto a clonazione.|  
|**Note e risoluzione**|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|**ID evento**|**2194**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|*<COMPUTERNAME>* Impossibile impostare il valore del Registro di sistema seguente per abilitare gli aggiornamenti DNS.<br /><br />Chiave del Registro di sistema:%1<br /><br />Valore del Registro di sistema: %2<br /><br />Dati del valore del Registro di sistema: %3<br /><br />Codice errore: %4<br /><br />Messaggio di errore: %5<br /><br />Durante il processo di clonazione, il computer locale può avere lo stesso nome computer del computer di origine clone per un breve periodo di tempo. La registrazione di record A e AAAA di DNS è disabilitata durante questo periodo, quindi i client non possono inviare richieste al computer locale sottoposto a clonazione.|  
|**Note e risoluzione**|Esaminare i registri eventi di sistema e dell'applicazione. Verificare la presenza di un'applicazione di terze parti che potrebbe bloccare gli aggiornamenti del Registro di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2195**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|Impossibile impostare l'avvio in modalità DSRM.<br /><br />Codice errore:%1<br /><br />Messaggio di errore:%2<br /><br />Se la clonazione del controller di dominio virtuale non riesce o il file di configurazione del clone del controller di dominio virtuale viene visualizzato in un hypervisor non supportato, il computer locale viene riavviato in modalità DSRM per consentire la risoluzione dei problemi. Impostazione dell'avvio in modalità DSRM non riuscita.|  
|**Note e risoluzione**|Esaminare i registri eventi di sistema e dell'applicazione. Verificare la presenza di un'applicazione di terze parti che potrebbe bloccare gli aggiornamenti del Registro di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2196**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|Impossibile abilitare il privilegio di arresto del sistema.<br /><br />Codice errore:%1<br /><br />Messaggio di errore:%2<br /><br />Se la clonazione del controller di dominio virtuale non riesce o il file di configurazione del clone del controller di dominio virtuale viene visualizzato in un hypervisor non supportato, il computer locale viene riavviato in modalità DSRM per consentire la risoluzione dei problemi. Abilitazione del privilegio di arresto del sistema non riuscita.|  
|**Note e risoluzione**|Esaminare i registri eventi di sistema e dell'applicazione. Verificare la presenza di un'applicazione di terze parti che potrebbe bloccare l'utilizzo di privilegi.|  
  
|||  
|-|-|  
|**ID evento**|**2197**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|Impossibile avviare l'arresto del sistema.<br /><br />Codice errore:%1<br /><br />Messaggio di errore:%2<br /><br />Se la clonazione del controller di dominio virtuale non riesce o il file di configurazione del clone del controller di dominio virtuale viene visualizzato in un hypervisor non supportato, il computer locale viene riavviato in modalità DSRM per consentire la risoluzione dei problemi. Avvio dell'arresto del sistema non riuscito.|  
|**Note e risoluzione**|Esaminare i registri eventi di sistema e dell'applicazione. Verificare la presenza di un'applicazione di terze parti che potrebbe bloccare l'utilizzo di privilegi.|  
  
|||  
|-|-|  
|**ID evento**|**2198**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|*<COMPUTERNAME>* non è stato possibile creare o modificare il seguente oggetto controller di dominio clonato.<br /><br />Dati aggiuntivi.<br /><br />Oggetto:<br /><br />%1<br /><br />Valore errore: %2<br /><br />%3|  
|**Note e risoluzione**|Cercare l'errore specifico in MS TechNet, nella Microsoft Knowledge Base e nei blog di Microsoft per determinarne il significato solito e quindi risolvere il problema in base a questi risultati.|  
  
|||  
|-|-|  
|**ID evento**|**2199**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|*<COMPUTERNAME>* Impossibile creare il seguente oggetto controller di dominio clonato perché già esistente.<br /><br />Dati aggiuntivi.<br /><br />Controller di dominio di origine:<br /><br />%1<br /><br />Oggetto:<br /><br />%2|  
|**Note e risoluzione**|Verificare che il file dccloneconfig.xml non specifichi un controller di dominio esistente o che non siano state usate copie del file dccloneconfig.xml in più cloni senza modificare il nome. Se la collisione è comunque imprevista, determinare quale amministratore l'ha promossa. Contattarlo per verificare l'opportunità di abbassare di livello il controller di domino esistente, pulire i metadati del controller di dominio esistente o usare un nome diverso per il clone.|  
  
|||  
|-|-|  
|**ID evento**|**2203**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|Ultima clonazione del controller di dominio virtuale non riuscita. Questo è il primo riavvio dopo che si è verificato tale problema, pertanto deve trattarsi di un nuovo tentativo di eseguire la clonazione. Il file di configurazione del clone del controller di dominio virtuale tuttavia non esiste e non è stata rilevata una modifica dell'ID di generazione macchina virtuale. Eseguire l'avvio in modalità DSRM.<br /><br />Ultima clonazione del controller di dominio virtuale non riuscita:%1<br /><br />File di configurazione del clone di controller di dominio virtuale esistente:%2<br /><br />Modifica dell'ID di generazione della macchina virtuale rilevata:%3|  
|**Note e risoluzione**|Questa situazione è prevista se la clonazione non è riuscita in precedenza a causa di un file dccloneconfig.xml mancante o non valido|  
  
|||  
|-|-|  
|ID evento|2210|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Errore|  
|Messaggio|<COMPUTERNAME>: impossibile creare gli oggetti per il controller di dominio clone.<br /><br />Dati aggiuntivi.<br /><br />ID clone: %6<br /><br />Nome controller di dominio clone: %1<br /><br />Ciclo ripetizione: %2<br /><br />Valore eccezione: %3<br /><br />Valore di errore: %4<br /><br />DSID: %5|  
|Note e risoluzione|Esaminare i registri eventi di sistema e dei servizi directory e il log dcpromo.log per altri dettagli sul motivo per cui la clonazione non è riuscita.|  
  
|||  
|-|-|  
|ID evento|2211|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativo|  
|Messaggio|<COMPUTERNAME>: oggetti creati per il controller di dominio clone.<br /><br />Dati aggiuntivi.<br /><br />ID clone: %3<br /><br />Nome controller di dominio clone: %1<br /><br />Ciclo ripetizione: %2|  
|Note e risoluzione|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2212|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativo|  
|Messaggio|<COMPUTERNAME>: avviata creazione degli oggetti per il controller di dominio clone.<br /><br />Dati aggiuntivi.<br /><br />ID clone: %1<br /><br />Nome clone: %2<br /><br />Sito clone: %3<br /><br />Controller di dominio di sola lettura clone: %4|  
|Note e risoluzione|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2213|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativo|  
|Messaggio|<COMPUTERNAME>: nuovo oggetto KrbTgt creato per la clonazione del controller di dominio di sola lettura.<br /><br />Dati aggiuntivi.<br /><br />ID clone: %1<br /><br />GUID nuovo oggetto KrbTgt: %2|  
|Note e risoluzione|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2214|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativo|  
|Messaggio|<COMPUTERNAME>: verrà creato un oggetto computer per il controller di dominio clone.<br /><br />Dati aggiuntivi.<br /><br />ID clone: %1<br /><br />Controller di dominio originale: %2<br /><br />Controller di dominio clone: %3|  
|Note e risoluzione|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2215|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativo|  
|Messaggio|<COMPUTERNAME>: il controller di dominio clone verrà aggiunto nel sito seguente.<br /><br />Dati aggiuntivi.<br /><br />ID clone: %1<br /><br />Sito: %2|  
|Note e risoluzione|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2216|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativo|  
|Messaggio|<COMPUTERNAME>: verrà creato un contenitore di server per il controller di dominio clone.<br /><br />Dati aggiuntivi.<br /><br />ID clone: %1<br /><br />Contenitore server: %2|  
|Note e risoluzione|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2217|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativo|  
|Messaggio|<COMPUTERNAME>: verrà creato un oggetto server per il controller di dominio clone.<br /><br />Dati aggiuntivi.<br /><br />ID clone: %1<br /><br />Oggetto server: %2|  
|Note e risoluzione|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2218|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativo|  
|Messaggio|<COMPUTERNAME>: verrà creato un oggetto Impostazioni NTDS per il controller di dominio clone.<br /><br />Dati aggiuntivi.<br /><br />ID clone: %1<br /><br />Oggetto: %2|  
|Note e risoluzione|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2219|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativo|  
|Messaggio|<COMPUTERNAME>: verranno creati oggetti connessione per il controller di dominio di sola lettura clone.<br /><br />Dati aggiuntivi.<br /><br />ID clone: %1|  
|Note e risoluzione|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2220|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativo|  
|Messaggio|<COMPUTERNAME>: verranno creati oggetti SYSVOL per il controller di dominio di sola lettura clone.<br /><br />Dati aggiuntivi.<br /><br />ID clone: %1|  
|Note e risoluzione|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2221|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Errore|  
|Messaggio|<COMPUTERNAME>: impossibile generare una password casuale per il controller di dominio clonato.<br /><br />Dati aggiuntivi.<br /><br />ID clone: %1<br /><br />Nome controller di dominio clone: %2<br /><br />Errore: %3 %4|  
|Note e risoluzione|Esaminare il registro eventi di sistema per altri dettagli sul motivo per cui non è stato possibile creare la password dell'account del computer.|  
  
|||  
|-|-|  
|ID evento|2222|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Errore|  
|Messaggio|<COMPUTERNAME>: impossibile impostare una password per il controller di dominio clonato.<br /><br />Dati aggiuntivi.<br /><br />ID clone: %1<br /><br />Nome controller di dominio clone: %2<br /><br />Errore: %3 %4|  
|Note e risoluzione|Esaminare il registro eventi di sistema per altri dettagli sul motivo per cui non è stato possibile impostare la password dell'account del computer.|  
  
|||  
|-|-|  
|ID evento|2223|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativo|  
|Messaggio|<COMPUTERNAME>: password dell'account computer impostata per il controller di dominio clonato.<br /><br />Dati aggiuntivi.<br /><br />ID clone: %1<br /><br />Nome controller di dominio clone: %2<br /><br />Totale tentativi: %3|  
|Note e risoluzione|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2224|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale non riuscita. Il computer clonato include i seguenti %1 account del servizio gestito autonomo:<br /><br />%2<br /><br />Per completare la clonazione è necessario rimuovere tutti gli account del servizio gestito autonomo. A tale scopo è possibile utilizzare il cmdlet Remove-ADServiceAccount PowerShell.|  
|Note e risoluzione|Si tratta di un risultato previsto quando si usano account del servizio gestito autonomi e non di gruppo. *Non* seguire il consiglio dell'evento di rimuovere l'account, perché non è stato scritto correttamente. Usare Uninstall-AdServiceAccount - [ https://technet.microsoft.com/library/hh852310 ](https://technet.microsoft.com/library/hh852310).<br /><br />Gli account del servizio gestito autonomi, rilasciati per la prima volta in Windows Server 2008 R2, verranno sostituiti con account del servizio gestito di gruppo in Windows Server 2012. Questi account supportano la clonazione.|  
  
|||  
|-|-|  
|ID evento|2225|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Informativo|  
|Messaggio|I segreti memorizzati nella cache dell'entità di sicurezza seguente sono stati rimossi dal controller di dominio locale:<br /><br />%1<br /><br />Dopo aver clonato un controller di dominio di sola lettura, i segreti precedentemente memorizzati nella cache del controller di dominio di sola lettura di origine della clonazione verranno rimossi dal controller di dominio clonato.|  
|Note e risoluzione|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2226|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Errore|  
|Messaggio|Impossibile rimuovere i segreti memorizzati nella cache dell'entità di sicurezza indicata di seguito dal controller di dominio locale:<br /><br />%1<br /><br />Errore: %2 (%3)<br /><br />Dopo aver clonato un controller di dominio di sola lettura, i segreti precedentemente memorizzati nella cache del controller di dominio di sola lettura di origine della clonazione devono essere rimossi nel clone. Se non si esegue questa operazione, aumenta il rischio che l'autore di un attacco possa ottenere le credenziali dal clone rubato o compromesso. Se l'entità di sicurezza è un account con privilegi elevati e deve essere protetto da questa condizione, usare l'operazione rootDSE rODCPurgeAccount per cancellare manualmente i segreti nel controller di dominio locale.|  
|Note e risoluzione|Esaminare i registri eventi di sistema e dei servizi directory per altre informazioni.|  
  
|||  
|-|-|  
|ID evento|2227|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Errore|  
|Messaggio|Eccezione generata durante il tentativo di rimuovere i segreti memorizzati nella cache dal controller di dominio locale.<br /><br />Dati aggiuntivi.<br /><br />Valore eccezione: %1<br /><br />Valore errore: %2<br /><br />DSID: %3<br /><br />Dopo aver clonato un controller di dominio di sola lettura, i segreti precedentemente memorizzati nella cache del controller di dominio di sola lettura di origine della clonazione devono essere rimossi nel clone. Se non si esegue questa operazione, aumenta il rischio che l'autore di un attacco possa ottenere le credenziali dal clone rubato o compromesso. Se l'entità di sicurezza è un account con privilegi elevati e deve essere protetto da questa condizione, usare l'operazione rootDSE rODCPurgeAccount per cancellare manualmente i segreti nel controller di dominio locale.|  
|Note e risoluzione|Esaminare i registri eventi di sistema e dei servizi directory per altre informazioni.|  
  
|||  
|-|-|  
|ID evento|2228|  
|Source|Microsoft-Windows-ActiveDirectory_DomainService|  
|Severity|Errore|  
|Messaggio|L'ID di generazione della macchina virtuale nel database di Active Directory di questo controller di dominio è diverso dal valore corrente della macchina virtuale. Non è stato possibile trovare un file di configurazione del controller di dominio virtuale (DCCloneConfig.xml), pertanto la clonazione del controller di dominio non è stata tentata. Se è prevista un'operazione di clonazione del controller di dominio, accertarsi che un file DCCloneConfig.xml sia disponibile in uno qualsiasi dei percorsi supportati. Inoltre, l'indirizzo IP del controller di dominio è in conflitto con quello di un altro controller di dominio. Per evitare interruzioni del servizio, il controller di dominio è stato configurato per l'avvio in DSRM.<br /><br />Dati aggiuntivi.<br /><br />Indirizzo IP duplicato: %1|  
|Note e risoluzione|Il meccanismo di protezione arresta i controller di dominio duplicati quando è possibile (tranne, ad esempio, se si usa DHCP). Aggiungere un file DcCloneConfig.xml valido, rimuovere il flag DSRM e provare a rieseguire la clonazione.|  
  
|||  
|-|-|  
|ID evento|29218|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale non riuscita. Clonazione del controller di dominio virtuale non riuscita. Impossibile completare l'operazione di clonazione e il controller di dominio clonato è stato riavviato in modalità di ripristino di servizi directory (DSRM, Directory Services Restore Mode).<br /><br />Esaminare gli eventi precedentemente registrati e il file %systemroot%\debug\dcpromo.log per ulteriori informazioni sugli errori corrispondenti al tentativo di clonazione del controller di dominio virtuale e verificare se l'immagine del clone può essere riutilizzata.<br /><br />Se una o più voci di registro indicano che non è possibile ritentare il processo di clonazione, l'immagine deve essere eliminata in modo sicuro. In caso contrario, è possibile correggere gli errori, cancellare il flag di avvio DSRM e riavviare normalmente. Al riavvio, l'operazione di clonazione verrà tentata di nuovo.|  
|Note e risoluzione|Esaminare i registri eventi di sistema e dei servizi directory e il log dcpromo.log per altri dettagli sul motivo per cui la clonazione non è riuscita.|  
  
|||  
|-|-|  
|ID evento|29219|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Informativo|  
|Messaggio|Clonazione del controller di dominio virtuale completata.|  
|Note e risoluzione|Si tratta di un evento riuscito ed è un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|29248|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Errore|  
|Messaggio|Impossibile ottenere la notifica di Winlogon durante la clonazione del controller di dominio virtuale. Codice di errore restituito: %1 (%2).<br /><br />Per ulteriori informazioni sull'errore, cercare nel file %systemroot%\debug\dcpromo.log gli errori corrispondenti al tentativo di clonazione del controller di dominio virtuale.|  
|Note e risoluzione|Contattare il Servizio Supporto Tecnico Clienti Microsoft.|  
  
|||  
|-|-|  
|ID evento|29249|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Errore|  
|Messaggio|Impossibile analizzare il file di configurazione del controller di dominio virtuale durante la clonazione di tale sistema.<br /><br />Codice HRESULT restituito: %1.%n<br /><br />Il file di configurazione è:%2<br /><br />Correggere gli errori nel file di configurazione e ritentare l'operazione di clonazione.<br /><br />Per ulteriori informazioni sull'errore, vedere %systemroot%\debug\dcpromo.log.|  
|Note e risoluzione|Esaminare il file dclconeconfig.xml per rilevare eventuali errori di sintassi usando un editor XML e il file di schema DCCloneConfigSchema.xsd.|  
  
|||  
|-|-|  
|ID evento|29250|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale non riuscita. Nel controller di dominio virtuale clonato sono attualmente abilitati componenti software o servizi non presenti nell'elenco delle applicazioni consentite per la clonazione dei controller di dominio virtuali.<br /><br />Voci mancanti:<br /><br />%2<br /><br />%1 (se presente) utilizzato come elenco di inclusione definito.<br /><br />Impossibile completare l'operazione di clonazione se sono installate applicazioni non consentite per la clonazione.<br /><br />Eseguire il cmdlet Powershell di Active Directory Get-ADDCCloningExcludedApplicationList per verificare quali applicazioni sono installate nel computer clonato che non sono incluse nell'elenco delle applicazioni consentite e aggiungerle all'elenco delle applicazioni consentite se sono compatibili con la clonazione dei controller di dominio virtuali. Se tali applicazioni non sono compatibili con la clonazione dei controller di dominio virtuali, disinstallarle prima di ritentare l'operazione di clonazione.<br /><br />Il processo di clonazione dei controller di dominio virtuali cerca il file dell'elenco delle applicazioni consentite CustomDCCloneAllowList.xml sulla base dell'ordine di ricerca specificato di seguito. Viene utilizzato il primo file trovato, mentre tutti gli altri vengono ignorati:<br /><br />1. Nome del valore del Registro di sistema: HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters\AllowListFolder<br /><br />2. Stessa directory in cui risiede la cartella della directory di lavoro DSA<br /><br />3. %windir%\NTDS<br /><br />4. Supporti di lettura/scrittura rimovibili a partire dalla lettera di unità della radice dell'unità|  
|Note e risoluzione|Seguire le istruzioni del messaggio.|  
  
|||  
|-|-|  
|ID evento|29251|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Errore|  
|Messaggio|Impossibile reimpostare gli indirizzi IP del computer clonato durante la clonazione del controller di dominio virtuale.<br /><br />Codice di errore restituito: %1 (%2).<br /><br />L'errore potrebbe essere dovuto a un problema di configurazione delle sezioni relative alla configurazione della rete nel file di configurazione del controller di dominio virtuale.<br /><br />Vedere il file %systemroot%\debug\dcpromo.log per ulteriori informazioni sugli errori corrispondenti alla reimpostazione degli indirizzi IP durante i tentativi di clonazione del controller di dominio virtuale.<br /><br />Ulteriori informazioni sulla reimpostazione indirizzi IP nel computer clonato sono reperibile in https://go.microsoft.com/fwlink/?LinkId=208030|  
|Note e risoluzione|Verificare che le informazioni sugli indirizzi IP impostate nel file dccloneconfig.xml siano valide e non duplichino il computer di origine.|  
  
|||  
|-|-|  
|ID evento|29253|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale non riuscita. Il controller di dominio clone non è riuscito a individuare il master operazioni del controller di dominio primario (PDC) nel dominio home del computer clonato.<br /><br />Codice di errore restituito: %1 (%2).<br /><br />Verificare che il controller di dominio primario nel dominio home del computer clonato sia stato assegnato a un controller di dominio attivo, sia online e sia operativo. Verificare che il computer clonato disponga di connettività LDAP/RPC con il controller di dominio primario per le porte e i protocolli richiesti.|  
|Note e risoluzione|Verificare se le informazioni su DNS e indirizzo IP del controller di dominio clonato sono impostate. Dcdiag.exe /test: locatorcheck per verificare se l'emulatore PDC è online, usare Nltest.exe. exe /server:*<PDCE>* /dclist:*<domain>* valido RPC, ottenere un'acquisizione di rete dall'emulatore PDC durante la clonazione non riesce e analizzare il traffico.|  
  
|||  
|-|-|  
|ID evento|29254|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Errore|  
|Messaggio|Impossibile eseguire il binding al controller di dominio primario %1 durante la clonazione del controller di dominio virtuale.<br /><br />Il codice di errore restituito è %2 (%3).<br /><br />Verificare che il controller di dominio primario %1 sia online e operativo. Verificare che il computer clonato disponga di connettività LDAP/RPC con il controller di dominio primario per le porte e i protocolli richiesti.|  
|Note e risoluzione|Verificare se le informazioni su DNS e indirizzo IP del controller di dominio clonato sono impostate. Dcdiag.exe /test: locatorcheck per verificare se l'emulatore PDC è online, usare Nltest.exe. exe /server:*<PDCE>* /dclist:*<domain>* valido RPC, ottenere un'acquisizione di rete dall'emulatore PDC durante la clonazione non riesce e analizzare il traffico.|  
  
|||  
|-|-|  
|ID evento|29255|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale non riuscita.<br /><br />Durante il tentativo di creare nel controller di dominio primario %1 gli oggetti necessari per l'immagine da clonare è stato restituito l'errore %2 (%3).<br /><br />Verificare che il controller di dominio clonato disponga dei privilegi per clonarsi. Controllare gli eventi correlati nel registro eventi del servizio directory nel controller di dominio primario %1.|  
|Note e risoluzione|Cercare l'errore specifico in MS TechNet, nella Microsoft Knowledge Base e nei blog di Microsoft per determinarne il significato solito e quindi risolvere il problema in base a questi risultati.|  
  
|||  
|-|-|  
|ID evento|29256|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Errore|  
|Messaggio|Tentativo di impostazione del flag di avvio in modalità di ripristino di servizi directory non riuscito con codice di errore %1.<br /><br />Per ulteriori informazioni sugli errori, vedere %systemroot%\debug\dcpromo.log.|  
|Note e risoluzione|Esaminare il log dei servizi directory e dcpromo.log per i dettagli. Esaminare i registri eventi di sistema e dell'applicazione. Verificare la presenza di un'applicazione di terze parti che potrebbe bloccare l'utilizzo di privilegi.|  
  
|||  
|-|-|  
|ID evento|29257|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale completata. Tentativo di riavvio del computer non riuscito con codice di errore %1.<br /><br />Riavviare il computer per terminare l'operazione di clonazione.|  
|Note e risoluzione|Esaminare i registri eventi di sistema e dell'applicazione. Verificare la presenza di un'applicazione di terze parti che potrebbe bloccare l'utilizzo di privilegi.|  
  
|||  
|-|-|  
|ID evento|29264|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Errore|  
|Messaggio|Tentativo di eliminazione del flag di avvio in modalità di ripristino di servizi directory non riuscito con codice di errore %1.<br /><br />Per ulteriori informazioni sugli errori, vedere %systemroot%\debug\dcpromo.log.|  
|Note e risoluzione|Esaminare il log dei servizi directory e dcpromo.log per i dettagli. Esaminare i registri eventi di sistema e dell'applicazione. Verificare la presenza di un'applicazione di terze parti che potrebbe bloccare l'utilizzo di privilegi.|  
  
|||  
|-|-|  
|ID evento|29265|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Informativo|  
|Messaggio|Clonazione del controller di dominio virtuale completata. Il file di configurazione della clonazione del controller di dominio virtuale %1 è stato rinominato in %2.|  
|Note e risoluzione|N/D, si tratta di un evento riuscito.|  
  
|||  
|-|-|  
|ID evento|29266|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale completata. Il tentativo di ridenominazione del file di configurazione della clonazione del controller di dominio virtuale %1 non è riuscito con codice di errore %2(%3).|  
|Note e risoluzione|Rinominare manualmente il file dccloneconfig.xml.|  
  
|||  
|-|-|  
|ID evento|29267|  
|Source|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Severity|Errore|  
|Messaggio|Durante la clonazione del controller di dominio virtuale non è stato possibile controllare l'elenco di applicazioni consentite per la clonazione del controller di dominio virtuale.<br /><br />Codice di errore restituito: %1 (%2).<br /><br />Il problema potrebbe essere causato da un errore di sintassi nel file dell'elenco di applicazioni consentite per la clonazione (il file attualmente controllato è: %3). Per ulteriori informazioni sull'errore, vedere %systemroot%\debug\dcpromo.log.|  
|Note e risoluzione|Seguire le istruzioni dell'evento.|  
  
##### <a name="error-messages"></a>Messaggi di errore  
Non ci sono messaggi di errore interattivi diretti per la mancata riuscita della clonazione di un controller di domino virtuale. Tutte le informazioni sulla clonazione sono disponibili nei log di sistema e dei servizi directory, oltre che nei log di promozione del controller di dominio in dcpromo.log. Se tuttavia il server viene avviato in modalità ripristino servizi directory, esaminare immediatamente il motivo dell'errore di promozione o clonazione.  
  
Il log dcpromo.log è il primo file da controllare per informazioni sugli errori di clonazione. In base all'errore riportato, può essere necessario in seguito esaminare i log dei servizi directory e di sistema per un'ulteriore diagnosi.  
  
#### <a name="known-issues-and-support-scenarios"></a>Problemi noti e probabili e scenari di supporto  
Di seguito sono riportati gli errori comuni riscontrati durante il processo di sviluppo di Windows Server 2012. Tutti questi problemi sono "da progettazione" e prevedono una soluzione alternativa valida o una tecnica più appropriata per evitarli del tutto. Alcuni potranno essere risolti nelle versioni successive di Windows Server 2012.  
  
|||  
|-|-|  
|**Problema**|**Clonazione non riuscita, modalità ripristino servizi directory**|  
|**Sintomi**|Il clone viene avviato in modalità ripristino servizi directory.|  
|**Risoluzione e note**|Verificare tutti i passaggi eseguiti, riportati nelle sezioni Distribuzione di un controller di dominio virtualizzato e [Metodologia generale per la risoluzione dei problemi di clonazione dei controller di dominio](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology).<br /><br />Descrizione nell'articolo 2742844 della Knowledge Base.|  
  
|||  
|-|-|  
|**Problema**|**Lease IP aggiuntivi quando si usa DHCP da clonare**|  
|**Sintomi**|Dopo la corretta clonazione di un controller di dominio e l'uso di DHCP, al primo avvio il clone acquisisce un lease DHCP. Quindi, quando il server viene rinominato e riavviato come controller di dominio, acquisisce un secondo lease DHCP. Il primo indirizzo IP non viene rilasciato e si verifica un lease "fittizio".|  
|**Risoluzione e note**|Eliminare manualmente il lease dell'indirizzo inutilizzato in DHCP o consentirne la normale scadenza. Descrizione nell'articolo 2742836 della Knowledge Base.|  
  
|||  
|-|-|  
|**Problema**|**La clonazione non riesce in modalità ripristino servizi directory dopo il ritardo prolungato.**|  
|**Sintomi**|La clonazione sembra sospesa per 8-15 minuti durante la visualizzazione del messaggio "Percentuale di completamento della clonazione del controller di dominio: X%". In seguito, la clonazione non riesce e il sistema viene avviato in modalità DSRM.|  
|**Risoluzione e note**|Il computer clonato non riesce a ottenere un indirizzo IP dinamico da DHCP o SLAAC, usa un indirizzo IP duplicato oppure non riesce a trovare il PDC. I molteplici tentativi di ripetizione della clonazione portano al ritardo. Risolvere il problema di rete per consentire la clonazione.<br /><br />Descrizione nell'articolo 2742844 della Knowledge Base.|  
  
|||  
|-|-|  
|**Problema**|**La clonazione non ricrea tutti i nomi dell'entità servizio**|  
|**Sintomi**|Se un set di nomi dell'entità servizio (SPN) costituito da *tre parti* include un nome NetBIOS con una porta e un nome NetBIOS altrimenti identico ma senza porta, la voce senza porta non viene ricreata con il nuovo nome computer. Ad esempio: <br /><br />customspn/DC1:200/app1 INVALID USE OF SYMBOLS *: questa parte viene ricreata con il nuovo nome computer*<br /><br />customspn/DC1/app1 INVALID USE OF SYMBOLS *: questa parte non viene ricreata con il nuovo nome computer*<br /><br />I nomi completi vengono ricreati, così come i nomi SPN non costituiti da tre parti, indipendentemente dalle porte. Ad esempio, i nomi seguenti vengono ricreati correttamente nel clone:<br /><br />customspn/DC1:202 INVALID USE OF SYMBOLS*viene ricreato*<br /><br />customspn/DC1 INVALID USE OF SYMBOLS*viene ricreato*<br /><br />customspn/DC1.corp.contoso.com:202 INVALID USE OF SYMBOLS*è il nome ricreato*<br /><br />customspn/DC1.corp.contoso.com INVALID USE OF SYMBOLS*è il nome ricreato*|  
|**Risoluzione e note**|Si tratta di una limitazione del processo di ridenominazione dei controller di dominio di Windows, non specificamente legata alla clonazione. I nomi SPN in tre parti non vengono gestiti dalla logica di ridenominazione, in nessuno scenario. La maggior parte dei servizi di Windows inclusi non sono interessati da questo problema, in quanto i nomi SPN mancanti vengono ricreati secondo necessità. Altre applicazioni possono invece richiedere l'immissione manuale del nome SPN per risolvere il problema.<br /><br />Descrizione nell'articolo 2742874 della Knowledge Base.|  
  
|||  
|-|-|  
|**Problema**|**Errore di clonazione, viene avviato in modalità ripristino servizi directory, errori di rete generali**|  
|**Sintomi**|Il clone viene avviato in modalità ripristino servizi directory. Si verificano errori generici di rete.|  
|Risoluzione e note|Assicurarsi che al nuovo clone non sia stato assegnato un indirizzo MAC statico duplicato dal controller di dominio di origine. È possibile verificare se una VM usa indirizzi MAC statici eseguendo questo comando nell'host dell'hypervisor per le macchine virtuali clone e di origine:<br /><br />Get-VM -VMName *test-vm* &#124; Get-VMNetworkAdapter &#124; fl *<br /><br />Sostituire l'indirizzo MAC con un indirizzo statico univoco o passare a indirizzi MAC dinamici.<br /><br />Descrizione nell'articolo 2742844 della Knowledge Base.|  
  
|||  
|-|-|  
|**Problema**|**Errore di clonazione, viene avviato in modalità DSRM come duplicato del controller di dominio di origine**|  
|**Sintomi**|Un nuovo clone viene avviato senza clonazione. Il file dccloneconfig.xml non viene rinominato e il server viene avviato in modalità ripristino servizi directory. Il registro eventi dei servizi directory indica l'errore 2164.<br /><br />*<COMPUTERNAME>* Impossibile avviare il servizio DsRoleSvc per clonare il controller di dominio virtuale locale.|  
|**Risoluzione e note**|Esaminare le impostazioni del servizio DsRoleSvc (DS Role Server Service) e assicurarsi che il tipo di avvio sia impostato su manuale. Verificare che nessun programma di terze parti impedisca l'avvio del servizio.<br /><br />Per altre informazioni su come recuperare il controller di dominio secondario assicurandosi al contempo che gli aggiornamenti vengano replicati in uscita, vedere l'articolo 2742970 della Microsoft Knowledge Base.|  
  
|||  
|-|-|  
|**Problema**|**Errore di clonazione, viene avviato in modalità ripristino servizi directory, l'errore 8610**|  
|**Sintomi**|Il clone viene avviato in modalità ripristino servizi directory. Il log Dcpromo.log indica l'errore 8610, ossia ERROR_DS_ROLE_NOT_VERIFIED 8610 o 0x21A2.|  
|**Risoluzione e note**|Questo problema si verifica se il PDC può essere individuabile ma non ha eseguito una replica sufficiente per assumere il ruolo, ad esempio se la clonazione è stata avviata e un altro amministratore sposta il ruolo FSMO dell'emulatore PDC in un nuovo controller di dominio.<br /><br />Descrizione nell'articolo 2742916 della Knowledge Base.|  
  
|||  
|-|-|  
|**Problema**|**Errore di clonazione, viene avviato in modalità ripristino servizi directory, errori di rete generali**|  
|**Sintomi**|Il clone viene avviato in modalità ripristino servizi directory. Si verificano errori generici di rete.|  
|**Risoluzione e note**|Assicurarsi che al nuovo clone non sia stato assegnato un indirizzo MAC statico duplicato dal controller di dominio di origine. È possibile verificare se una VM usa indirizzi MAC statici eseguendo questo comando nell'host Hyper-V per le macchine virtuali clone e di origine:<br /><br />Get-VM -VMName *test-vm* &#124; Get-VMNetworkAdapter &#124; fl *<br /><br />Sostituire l'indirizzo MAC con un indirizzo statico univoco o passare a indirizzi MAC dinamici.<br /><br />Descrizione nell'articolo 2742844 della Knowledge Base.|  
  
|||  
|-|-|  
|**Problema**|**Errore di clonazione, viene avviato in modalità ripristino servizi directory**|  
|**Sintomi**|Il clone viene avviato in modalità ripristino servizi directory.|  
|**Risoluzione e note**|Assicurarsi che il file dccloneconfig.xml contenga la definizione dello schema (vedere la riga 2 di sampledccloneconfig.xml):<br /><br />**<d3c:DCCloneConfig xmlns:d3c="uri:microsoft.com:schemas:DCCloneConfig">**<br /><br />Descrizione nell'articolo 2742844 della Knowledge Base.|  
  
|||  
|-|-|  
|Problema|**Nessun server di accesso è la registrazione degli errori disponibile in modalità DSRM**|  
|**Sintomi**|Il clone viene avviato in modalità ripristino servizi directory. Dopo il tentativo di accesso si riceve il messaggio di errore seguente:<br /><br />**Esistono attualmente Nessun server di accesso è disponibile per soddisfare la richiesta di accesso**|  
|**Risoluzione e note**|Assicurarsi di effettuare l'accesso con l'account di amministratore DSRM e non con l'account di dominio. Usare la freccia sinistra e digitare un nome utente di tipo:<br /><br />**.\administrator**<br /><br />Descrizione nell'articolo 2742908 della Knowledge Base|  
  
|||  
|-|-|  
|**Problema**|**Si verifica un errore in modalità DSRM, errore di clonazione dell'origine**|  
|**Sintomi**|Durante la clonazione, si verifica l'errore 8437, indicante che la creazione di oggetti controller di dominio clone in PDC non è riuscita (0x20f5).|  
|**Risoluzione e note**|Il nome computer duplicato è stato impostato in DCCloneConfig.xml come controller di dominio di origine di un controller di dominio esistente. Anche il nome computer deve essere nel formato NetBIOS (massimo 15 caratteri, non un FQDN).<br /><br />Correggere il file dccloneconfig.xml impostando un nome valido e univoco.<br /><br />Descrizione nell'articolo 2742959 della Knowledge Base|  
  
|||  
|-|-|  
|**Problema**|**Errore di nuovo-addccloneconfigfile "indice non compreso nell'intervallo"**|  
|**Sintomi**|Quando si esegue il cmdlet new-addccloneconfigfile, viene visualizzato il messaggio di errore:<br /><br />Indice non compreso nell'intervallo consentito. Deve essere non negativo e minore della dimensione dell'insieme.|  
|**Risoluzione e note**|È necessario eseguire il cmdlet in una console di Windows PowerShell con privilegi elevati di amministratore. Questo errore è causato dalla mancata appartenenza al gruppo Administrators locale.<br /><br />Descrizione nell'articolo 2742927 della Knowledge Base|  
  
|||  
|-|-|  
|**Problema**|**La clonazione di controller di dominio duplicato ha esito negativo**|  
|**Sintomi**|Il clone viene avviato senza clonazione e viene duplicato il controller di dominio esistente.|  
|**Risoluzione e note**|Il computer è stato copiato e avviato, ma non contiene un file DcCloneConfig.xml in nessun percorso supportato e non ha un indirizzo IP duplicato con il controller di dominio di origine. Il controller di dominio deve essere rimosso correttamente per evitare la perdita di dati.<br /><br />Descrizione nell'articolo 2742970 della Knowledge Base|  
  
|||  
|-|-|  
|**Problema**|**New-ADDCCloneConfigFile non riesce con il server non è errore operativo quando controlla se il controller di dominio di origine è un membro del gruppo di controller di dominio clonabili se non è disponibile un catalogo globale.**|  
|**Sintomi**|Quando si esegue New-ADDCCloneConfigFile per creare un file dccloneconfig.xml, si riceve l'errore:<br /><br />Code - il server non operativo|  
|**Risoluzione e note**|Verificare la connettività a un catalogo globale dal server in cui si esegue New-ADDCCloneConfigFile e controllare che l'appartenenza del controller di dominio globale nel gruppo Controller di dominio clonabili sia stata replicata in tale catalogo globale.<br /><br />Eseguire il comando seguente per scaricare la cache del localizzatore di controller di dominio nel caso in cui un catalogo globale o un controller di dominio sia stato portato offline di recente:<br /><br />Code - nltest /dsgetdc: /GC /FORCE|  
  
### <a name="advanced-troubleshooting"></a>Risoluzione avanzata dei problemi  
Questo modulo contiene informazioni sulla risoluzione avanzata dei problemi, usando log *operativi* come esempi, oltre a una spiegazione dell'evento che si è verificato. Con una buona comprensione del funzionamento corretto di un controller di dominio virtualizzato, è possibile identificare con maggior chiarezza gli errori nell'ambiente. Questi log vengono presentati dalla relativa origine, con l'ordine crescente di eventi *previsti* (anche quando si tratta di avvisi ed errori) correlati a un controller di dominio clonato all'interno di ogni log.  
  
#### <a name="cloning-a-domain-controller"></a>Clonazione di un controller di dominio  
In questo esempio il controller di dominio clone usa DHCP per ottenere un indirizzo IP, replica SYSVOL tramite il servizio Replica file o Replica DFS (vedere il log appropriato, se necessario), è un catalogo globale e usa un file dccloneconfig.xml vuoto.  
  
##### <a name="directory-services-event-log"></a>Registro eventi dei servizi directory  
Il log dei servizi directory contiene la maggior parte delle informazioni operative sulla clonazione basata su evento. L'hypervisor cambia l'ID di generazione VM e il servizio NTDS ne prende nota, quindi invalida il pool di RID e cambia l'ID di richiesta. Il nuovo ID di generazione VM viene impostato e il server replica i dati di Active Directory in ingresso. Il servizio Replica DFS viene arrestato e il relativo database che ospita SYSVOL viene eliminato, forzando una sincronizzazione non autorevole in ingresso. Il limite massimo di USN viene modificato.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**2160**|ActiveDirectory_DomainService|L'istanza locale di Servizi di dominio Active Directory ha individuato un file di configurazione della clonazione del controller di dominio virtuale.<br /><br />Percorso del file di configurazione:<br /><br />*<path>* \DCCloneConfig.xml<br /><br />L'esistenza del file di configurazione della clonazione del controller di dominio virtuale indica che il controller di dominio virtuale locale è un clone di un altro controller di dominio virtuale. Servizi di dominio Active Directory verrà avviato per eseguire l'autoclonazione.|  
|**2191**|ActiveDirectory_DomainService|Servizi di dominio Active Directory ha impostato il seguente valore del Registro di sistema per disabilitare gli aggiornamenti DNS.<br /><br />Chiave del Registro di sistema:<br /><br />SYSTEM\CurrentControlSet\Services\Netlogon\Parameters<br /><br />Valore del Registro di sistema:<br /><br />UseDynamicDns<br /><br />Dati del valore del Registro di sistema:<br /><br />0<br /><br />Durante il processo di clonazione, il computer locale può avere lo stesso nome computer del computer di origine clone per un breve periodo di tempo. La registrazione di record A e AAAA di DNS è disabilitata durante questo periodo, quindi i client non possono inviare richieste al computer locale sottoposto a clonazione. Con il processo di clonazione, i nuovi aggiornamenti del DNS dopo la clonazione vengono completati.|  
|**2191**|ActiveDirectory_DomainService|Servizi di dominio Active Directory ha impostato il seguente valore del Registro di sistema per disabilitare gli aggiornamenti DNS.<br /><br />Chiave del Registro di sistema:<br /><br />SYSTEM\CurrentControlSet\Services\Dnscache\Parameters<br /><br />Valore del Registro di sistema:<br /><br />RegistrationEnabled<br /><br />Dati del valore del Registro di sistema:<br /><br />0<br /><br />Durante il processo di clonazione, il computer locale può avere lo stesso nome computer del computer di origine clone per un breve periodo di tempo. La registrazione di record A e AAAA di DNS è disabilitata durante questo periodo, quindi i client non possono inviare richieste al computer locale sottoposto a clonazione. Con il processo di clonazione, i nuovi aggiornamenti del DNS dopo la clonazione vengono completati.<br /><br />"Informazioni 2 o 7/2012 3:12:49 PM Microsoft-Windows-ActiveDirectory DomainService 2191 Internal Configuration" servizi di dominio Active Directory imposta il valore del Registro di sistema seguente per disabilitare gli aggiornamenti DNS.<br /><br />Chiave del Registro di sistema:<br /><br />SYSTEM\CurrentControlSet\Services\Tcpip\Parameters<br /><br />Valore del Registro di sistema:<br /><br />DisableDynamicUpdate<br /><br />Dati del valore del Registro di sistema:<br /><br />1<br /><br />Durante il processo di clonazione, il computer locale può avere lo stesso nome computer del computer di origine clone per un breve periodo di tempo. La registrazione di record A e AAAA di DNS è disabilitata durante questo periodo, quindi i client non possono inviare richieste al computer locale sottoposto a clonazione. Con il processo di clonazione, i nuovi aggiornamenti del DNS dopo la clonazione vengono completati.|  
|**2172**|ActiveDirectory_DomainService|Lettura dell'attributo msDS-GenerationId dell'oggetto computer del controller di dominio.<br /><br />Valore dell'attributo msDS-GenerationId:<br /><br />*<Number>*|  
|**2170**|ActiveDirectory_DomainService|È stata rilevata una modifica all'ID di generazione.<br /><br />ID generazione memorizzato nella cache in DS (valore precedente):<br /><br />*<Number>*<br /><br />ID generazione attualmente nella macchina virtuale (nuovo valore):<br /><br />*<Number>*<br /><br />La modifica dell'ID di generazione si verifica dopo l'applicazione di uno snapshot della macchina virtuale, dopo un'operazione di importazione di una macchina virtuale o dopo un'operazione Live Migration. In Servizi di dominio Active Directory verrà creato un nuovo ID di chiamata per ripristinare il controller di dominio. I controller di dominio virtualizzati non devono essere ripristinati mediante snapshot della macchina virtuale. Il metodo supportato per il ripristino o il rollback del contenuto di un database di Servizi di dominio Active Directory consiste nel ripristinare un backup dello stato del sistema creato con un'applicazione di backup abilitata per Servizi di dominio Active Directory.|  
|**1109**|ActiveDirectory_DomainService|L'attributo invocationID relativo a questo server di directory è stato modificato. Il numero di sequenza di aggiornamento più alto al momento del backup è stato creato nel modo seguente:<br /><br />Attributo InvocationID (vecchio valore):<br /><br />*<GUID>*<br /><br />Attributo InvocationID (nuovo valore):<br /><br />*<GUID>*<br /><br />Numero di sequenza di aggiornamento:<br /><br />*<Number>*<br /><br />L'attributo invocationID viene modificato quando un server di directory viene ripristinato dal supporto di backup,viene configurato per l'hosting di una partizione di directory applicativa scrivibile, viene ripreso dopo l'applicazione di una snapshot di macchina virtuale, dopo un'operazione di importazione di una macchina virtuale o dopo un'operazione Live Migration. I controller di dominio virtualizzati non devono essere ripristinati mediante snapshot della macchina virtuale. Il metodo supportato per il ripristino o il rollback del contenuto di un database di Servizi di dominio Active Directory consiste nel ripristinare un backup dello stato del sistema creato con un'applicazione di backup abilitata per Servizi di dominio Active Directory.|  
|**1000**|ActiveDirectory_DomainService|Avvio di Servizi di dominio Microsoft Active Directory completato.|  
|**1394**|ActiveDirectory_DomainService|Tutti i problemi che impediscono gli aggiornamenti al database di Servizi di dominio Active Directory sono stati risolti. Nuovi aggiornamenti al database di Servizi di dominio Active Directory in corso. Il servizio Accesso rete è stato riavviato.|  
|**2163**|ActiveDirectory_DomainService|È stato avviato il servizio DsRoleSvc per clonare il controller di dominio virtuale locale.|  
|**326**|NTDS ISAM|NTDS (536) NTDSA: Il motore di database ha collegato un database (1, C:\Windows\NTDS\ntds.dit). (Tempo=0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.016, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<br /><br />Cache salvata: 1|  
|**103**|NTDS ISAM|NTDS (536) NTDSA: Il motore di database ha arrestato l'istanza (0).<br /><br />Arresto anomalo: 0<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.032, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**102**|NTDS ISAM|NTDS (536) NTDSA: Il motore di database (6.02.8225.0000) sta avviando una nuova istanza (0).|  
|**105**|NTDS ISAM|NTDS (536) NTDSA: Il motore di database ha avviato una nuova istanza (0). (Tempo=0 secondi)<br /><br />Sequenza temporale interna: [1] 0.016, [2] 0.000, [3] 0.015, [4] 0.078, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.046, [10] 0.000, [11] 0.000.|  
|**1004**|ActiveDirectory_DomainService|Arresto di Servizi di dominio Active Directory completato.|  
|**102**|NTDS ISAM|NTDS (536) NTDSA: Il motore di database (6.02.8225.0000) sta avviando una nuova istanza (0).|  
|**326**|NTDS ISAM|NTDS (536) NTDSA: Il motore di database ha collegato un database (1, C:\Windows\NTDS\ntds.dit). (Tempo=0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.015, [3] 0.016, [4] 0.000, [5] 0.031, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<br /><br />Cache salvata: 1|  
|**105**|NTDS ISAM|NTDS (536) NTDSA: Il motore di database ha avviato una nuova istanza (0). (Tempo=1 secondo)<br /><br />Sequenza temporale interna: [1] 0.031, [2] 0.000, [3] 0.000, [4] 0.391, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000.|  
|**1109**|ActiveDirectory_DomainService|L'attributo invocationID relativo a questo server di directory è stato modificato. Il numero di sequenza di aggiornamento più alto al momento del backup è stato creato nel modo seguente:<br /><br />Attributo InvocationID (vecchio valore):<br /><br />*<GUID>*<br /><br />Attributo InvocationID (nuovo valore):<br /><br />*<GUID>*<br /><br />Numero di sequenza di aggiornamento:<br /><br />*<Number>*<br /><br />L'attributo invocationID viene modificato quando un server di directory viene ripristinato dal supporto di backup,viene configurato per l'hosting di una partizione di directory applicativa scrivibile, viene ripreso dopo l'applicazione di una snapshot di macchina virtuale, dopo un'operazione di importazione di una macchina virtuale o dopo un'operazione Live Migration. I controller di dominio virtualizzati non devono essere ripristinati mediante snapshot della macchina virtuale. Il metodo supportato per il ripristino o il rollback del contenuto di un database di Servizi di dominio Active Directory consiste nel ripristinare un backup dello stato del sistema creato con un'applicazione di backup abilitata per Servizi di dominio Active Directory.|  
|**1168**|ActiveDirectory_DomainService|Errore interno: Si è verificato un errore di Servizi di dominio Active Directory.<br /><br />Dati aggiuntivi<br /><br />Valore errore (decimale):<br /><br />2<br /><br />Valore errore (esadecimale):<br /><br />2<br /><br />ID interno:<br /><br />7011658|  
|**1110**|ActiveDirectory_DomainService|L'innalzamento di livello di questo controller di dominio a catalogo globale verrà eseguita dopo il seguente intervallo di tempo.<br /><br />Intervallo (minuti):<br /><br />5<br /><br />Tale ritardo è necessario per preparare le partizioni di directory richieste prima che sia annunciato il catalogo globale. Nel Registro di sistema è possibile specificare il numero di secondi che l'agente DSA deve attendere prima di promuovere il controller di dominio locale a catalogo globale. Per ulteriori informazioni sul valore del Registro Global Catalog Delay Advertisement, vedere Resource Kit Distributed Systems Guide.|  
|**103**|NTDS ISAM|NTDS (536) NTDSA: Il motore di database ha arrestato l'istanza (0).<br /><br />Arresto anomalo: 0<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.047, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.016, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**1004**|ActiveDirectory_DomainService|Arresto di Servizi di dominio Active Directory completato.|  
|**1539**|ActiveDirectory_DomainService|Impossibile disabilitare la cache in scrittura basata su software nel disco rigido seguente.<br /><br />Disco rigido:<br /><br />c:<br /><br />Gli errori di sistema possono provocare la perdita dei dati.|  
|**2179**|ActiveDirectory_DomainService|L'attributo msDS-GenerationId dell'oggetto computer del controller di dominio è stato impostato sul parametro seguente:<br /><br />Attributo GenerationID:<br /><br />*<Number>*|  
|**2173**|ActiveDirectory_DomainService|Impossibile leggere l'attributo msDS-GenerationId dell'oggetto computer del controller di dominio. Il problema potrebbe essere causato da un errore di transazione di database o dall'assenza dell'ID di generazione dal database locale. Il DS-GenerationId non esiste durante il primo riavvio dopo l'esecuzione di dcpromo oppure il controller di dominio non è un controller di dominio virtuale.<br /><br />Dati aggiuntivi<br /><br />Codice di errore:<br /><br />6|  
|**1000**|ActiveDirectory_DomainService|Avvio di Servizi di dominio Microsoft Active Directory completato, versione 6.2.8225.0|  
|**1394**|ActiveDirectory_DomainService|Tutti i problemi che impediscono gli aggiornamenti al database di Servizi di dominio Active Directory sono stati risolti. Nuovi aggiornamenti al database di Servizi di dominio Active Directory in corso. Il servizio Accesso rete è stato riavviato.|  
|**1128**|ActiveDirectory_DomainService|1128 Controllo di coerenza informazioni "È stata creata una connessione di replica dal seguente servizio directory di origine al servizio directory locale.<br /><br />Servizio directory di origine:<br /><br />CN = NTDS Settings,*<Domain Controller DN>*<br /><br />Servizio directory locale:<br /><br />CN = NTDS Settings, *<Domain Controller DN>*<br /><br />Dati aggiuntivi<br /><br />Codice motivo:<br /><br />0x2<br /><br />ID interno punto di creazione:<br /><br />f0a025d|  
|**1999**|ActiveDirectory_DomainService|Il servizio directory di origine ha ottimizzato il numero di sequenza di aggiornamento (USN) presentato dal servizio directory di destinazione. I due servizi directory hanno lo stesso partner di replica. Il servizio directory di destinazione viene aggiornato in base al partner di replica comune mentre il servizio directory di origine è stato installato utilizzando una copia di backup del partner stesso.<br /><br />ID servizio directory di destinazione:<br /><br />*<GUID> (<FQDN>)*<br /><br />ID servizio directory comune:<br /><br />*<GUID>*<br /><br />USN proprietà comune:<br /><br />*<Number>*<br /><br />Di conseguenza, il vettore degli aggiornamenti del servizio directory di destinazione è stato configurato in base alle seguenti impostazioni.<br /><br />Oggetto USN precedente:<br /><br />0<br /><br />Proprietà USN precedente:<br /><br />0<br /><br />GUID database:<br /><br />*<GUID>*<br /><br />USN oggetto:<br /><br />*<Number>*<br /><br />USN proprietà:<br /><br />*<Number>*|  
  
##### <a name="system-event-log"></a>Registro eventi di sistema  
Le indicazioni seguenti sulle operazioni di clonazione si trovano nel registro eventi di sistema. Quando l'hypervisor comunica al computer guest che è stato clonato o ripristinato da uno snapshot, il controller di dominio invalida immediatamente il pool di RID per evitare di duplicare le entità di sicurezza in seguito. Con il procedere della clonazione, vengono visualizzati vari messaggi e operazioni previsti, prevalentemente riguardo l'avvio e l'arresto di servizi e alcuni errori previsti corrispondenti. Al termine, nel registro eventi di sistema viene riportato l'esito positivo della clonazione complessiva.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**16654**|Directory-Services-SAM|Un pool di identificatori di account (RID) è stato invalidato. Questa situazione può verificarsi nei seguenti casi previsti:<br /><br />1. Un controller di dominio viene ripristinato da un backup.<br /><br />2. Un controller di dominio in esecuzione in una macchina virtuale viene ripristinato da uno snapshot.<br /><br />3. Un amministratore ha invalidato manualmente il pool.|  
|**7036**|Gestione controllo servizi|Il servizio Servizi di dominio Active Directory è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio Centro distribuzione chiavi Kerberos è in esecuzione.|  
|**3096**|Accesso rete|Impossibile trovare il controller di dominio primario per questo dominio.|  
|**7036**|Gestione controllo servizi|Il servizio Gestione account di sicurezza è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio Server è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio Accesso rete è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio Servizi Web Active Directory è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio Replica DFS è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio Replica file è in esecuzione.|  
|**14533**|Microsoft-Windows-DfsSvc|Creazione di tutti gli spazi dei nomi completata.|  
|**14531**|Microsoft-Windows-DfsSvc|Inizializzazione del server DFS completata.|  
|**7036**|Gestione controllo servizi|Il servizio Spazio dei nomi DFS è in esecuzione.|  
|**7023**|Gestione controllo servizi|Servizio Messaggistica tra siti terminato con l'errore:<br /><br />Il server specificato non può effettuare l'operazione richiesta.|  
|**7036**|Gestione controllo servizi|Il servizio Messaggistica tra siti è stato interrotto.|  
|**5806**|Accesso rete|Gli aggiornamenti dinamici DNS sono stati disabilitati manualmente nel controller di dominio.<br /><br />RIMEDIO<br /><br />Riconfigurare il controller di dominio in modo che utilizzi gli aggiornamenti dinamici DNS o inserire manualmente i record DNS contenuti nel the file '%SystemRoot%\System32\Config\Netlogon.dns' nel database DNS.|  
|**16651**|Directory-Services-SAM|Richiesta di un nuovo pool di identificatori di account non riuscita. L'operazione verrà ripetuta fino a quando la richiesta verrà accettata. Errore:<br /><br />L'operazione FSMO richiesta non è riuscita. Impossibile contattare il proprietario FSMO corrente.|  
|**7036**|Gestione controllo servizi|Il servizio Server DNS è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio Ruolo server DS è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio Accesso rete è stato interrotto.|  
|**7036**|Gestione controllo servizi|Il servizio Replica file è stato interrotto.|  
|**7036**|Gestione controllo servizi|Il servizio Centro distribuzione chiavi Kerberos è stato interrotto.|  
|**7036**|Gestione controllo servizi|Il servizio Server DNS è stato interrotto.|  
|**7036**|Gestione controllo servizi|Il servizio Servizi di dominio Active Directory è stato interrotto.|  
|**7036**|Gestione controllo servizi|Il servizio Accesso rete è in esecuzione.|  
|**7040**|Gestione controllo servizi|Il tipo di avvio del servizio Servizi di dominio Active Directory è cambiato da avvio automatico a disabilitato.|  
|**7036**|Gestione controllo servizi|Il servizio Accesso rete è stato interrotto.|  
|**7036**|Gestione controllo servizi|Il servizio Replica file è in esecuzione.|  
|**29219**|DirectoryServices-DSROLE-Server|Clonazione del controller di dominio virtuale completata.|  
|**29223**|DirectoryServices-DSROLE-Server|Ora il server è un controller di dominio.|  
|**29265**|DirectoryServices-DSROLE-Server|Clonazione del controller di dominio virtuale completata. Il file di configurazione della clonazione del controller di dominio virtuale C:\Windows\NTDS\DCCloneConfig.xml è stato rinominato in C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml.|  
|**1074**|User32|Il processo C:\Windows\system32\lsass.exe (DC2) a iniziato il riavvio del computer DC2 per conto dell'utente NT AUTHORITY\SYSTEM a causa del motivo seguente: Sistema operativo: riconfigurazione (pianificata)<br /><br />Codice motivo: 0x80020004<br /><br />Tipo di arresto: riavvio<br /><br />Commento: "|  
  
##### <a name="dcpromolog"></a>DCPROMO.LOG  
Il log Dcpromo.log contiene la parte effettiva di promozione della clonazione, non descritta nel registro eventi dei servizi directory. Poiché il log non fornisce il livello di spiegazione offerto dalle voci del registro eventi, questa sezione del modulo contiene un'ulteriore annotazione.  
  
Il processo di promozione implica che la clonazione viene avviata, viene eseguito lo scrubbing della configurazione corrente del controller di dominio, che viene rialzato di livello usando il database di Active Directory (in modo simile a una promozione di IFM). Il controller di dominio quindi replica i delta delle modifiche in ingresso di Active Directory e SYSVOL e la clonazione è completata.  
  
> [!NOTE]  
> Il log è stato modificato in questo modulo per migliorarne la leggibilità rimuovendo la colonna di date.  
  
> [!NOTE]  
> Per altri dettagli sul log dcpromo.log, vedere l'articolo di informazioni e risoluzione dei problemi dell'amministrazione semplificata di Servizi di dominio Active Directory in Windows Server 2012.  
>   
> [https://go.microsoft.com/fwlink/p/?LinkId=237244](https://go.microsoft.com/fwlink/p/?LinkId=237244)  
  
-   Avviare la promozione basata su clone.  
  
-   Impostare il flag della modalità ripristino servizi directory in modo che il server non venga avviato di nuovo normalmente come il clone originale e causi collisioni di denominazione o di servizi directory.  
  
-   Aggiornare il registro eventi dei servizi directory.  
  
```  
15:14:01 [INFO] vDC Cloneing: Setting Boot into DSRM flag succeeded.  
15:14:01 [WARNING] Cannot get user Token for Format Message: 1725l  
15:14:01 [INFO] vDC Cloning: Created vDCCloningUpdate event.  
15:14:01 [INFO] vDC Cloning: Created vDCCloningComplete event.  
```  
  
-   Arrestare il servizio Accesso rete in modo che il controller di dominio non invii annunci.  
  
```  
15:14:01 [INFO] Stopping service NETLOGON  
15:14:01 [INFO] ControlService(STOP) on NETLOGON returned 1(gle=0)  
15:14:01 [INFO] DsRolepWaitForService: waiting for NETLOGON to enter one of 7 states  
15:14:01 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:02 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=1  
15:14:02 [INFO] DsRolepWaitForService: exiting because NETLOGON entered STOPPED state  
15:14:02 [INFO] DsRolepWaitForService(for any end state) on NETLOGON service returned 0  
15:14:02 [INFO] ControlService(STOP) on NETLOGON returned 0(gle=1062)  
15:14:02 [INFO] Exiting service-stop loop after service NETLOGON entered STOPPED state  
15:14:02 [INFO] StopService on NETLOGON returned 0  
15:14:02 [INFO] Configuring service NETLOGON to 1 returned 0  
15:14:02 [INFO] Updating service status to 4  
15:14:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Esaminare il file dccloneconfig.xml per rilevare personalizzazioni specificate dall'amministratore.  
  
-   In questo caso di esempio si tratta di un file vuoto, quindi tutte le impostazioni vengono generate automaticamente gli indirizzi IP automatici vengono richiesti dalla rete.  
  
```  
15:14:02 [INFO] vDC Cloning: Clone config file C:\Windows\NTDS\DCCloneConfig.xml is considered to be a blank file (containing 0 bytes)  
15:14:02 [INFO] vDC Cloning: Parsing clone config file C:\Windows\NTDS\DCCloneConfig.xml returned HRESULT 0x0  
```  
  
-   Verificare che non siano installati servizi o programmi che non fanno parte del file DefaultDCCloneAllowList.xml o CustomDCCloneAllowList.xml  
  
```  
15:14:02 [INFO] vDC Cloning: Checking allowed list:  
15:14:03 [INFO] vDC Cloning: Completed checking allowed list:  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Abilitare DHCP sulle schede di rete, poiché le informazioni su IP non sono state specificate dall'amministratore.  
  
```  
15:14:03 [INFO] vDC Cloning: Enable DHCP:  
15:14:03 [INFO] WMI Instance: Win32_NetworkAdapterConfiguration.Index=12  
15:14:03 [INFO] Method: EnableDHCP  
15:14:03 [INFO] HRESULT code: 0x0 (0)  
15:14:03 [INFO] Return Value: 0x0 (0)  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Individuare l'emulatore PDC.  
  
-   Impostare il sito del clone (generato automaticamente in questo caso).  
  
-   Impostare il nome del clone (generato automaticamente in questo caso).  
  
```  
15:14:03 [INFO] vDC Cloning: Found PDC. Name: DC1.root.fabrikam.com  
15:14:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:04 [INFO] vDC Cloning: Winlogon UI Notification #1: Domain Controller cloning is at 5% completion...  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #2: Domain Controller cloning is at 10% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Site of the cloned DC: Default-First-Site-Name  
```  
  
-   Creare il nuovo oggetto computer clone.  
  
-   Rinominare il clone in base al nuovo nome.  
  
```  
15:14:05 [INFO] vDC Cloning: Clone DC objects are created on PDC.  
15:14:05 [INFO] Name of the cloned DC: DC2-CL0001  
15:14:05 [INFO] DsRolepSetRegStringValue on System\CurrentControlSet\Services\NTDS\Parameters\CloneMachineName to DC2-CL0001 returned 0  
15:14:05 [INFO] vDC Cloning: Save CloneMachineName in registry: 0x0 (0)  
```  
  
-   Specificare le impostazioni di promozione, in base al precedente file dccloneconfig.xml o a regole di generazione automatica.  
  
```  
15:14:05 [INFO] vDC Cloning: Promotion parameters setting:  
15:14:05 [INFO] DNS Domain Name: root.fabrikam.com  
15:14:05 [INFO] Replica Partner: \\DC1.root.fabrikam.com  
15:14:05 [INFO] Site Name: Default-First-Site-Name  
15:14:05 [INFO] DS Database Path: C:\Windows\NTDS  
15:14:05 [INFO] DS Log Path: C:\Windows\NTDS  
15:14:05 [INFO] SysVol Root Path: C:\Windows\SYSVOL  
15:14:05 [INFO] Account: root.fabrikam.com\DC2-CL0001$  
15:14:05 [INFO] Options: DSROLE_DC_CLONING (0x800400)  
```  
  
-   Avviare la promozione.  
  
```  
15:14:05 [INFO] Promote DC as a clone  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #3: Domain Controller cloning is at 15% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #4: Domain Controller cloning is at 16% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Validate supplied paths  
15:14:05 [INFO] Validating path C:\Windows\NTDS.  
15:14:05 [INFO] Path is a directory  
15:14:05 [INFO] Path is on a fixed disk drive.  
15:14:05 [INFO] Validating path C:\Windows\NTDS.  
15:14:05 [INFO] Path is a directory  
15:14:05 [INFO] Path is on a fixed disk drive.  
15:14:05 [INFO] Validating path C:\Windows\SYSVOL.  
15:14:05 [INFO] Path is on a fixed disk drive.  
15:14:05 [INFO] Path is on an NTFS volume  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #5: Domain Controller cloning is at 17% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Start the worker task  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #6: Domain Controller cloning is at 20% completion...  
15:14:05 [INFO] Request for promotion returning 0  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #7: Domain Controller cloning is at 21% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Arrestare e configurare tutti i servizi correlati a Servizi di dominio Active Directory (NTDS, NTFRS/DFSR, KDC, DNS).  
  
> [!NOTE]  
> I tempi lunghi di avvio del servizio DNS sono previsti in questo scenario, in quanto vengono usate zone integrate in AD che non erano più disponibili, neanche prima dell'arresto del servizio NTDS. Vedere gli eventi DNS descritti più avanti in questa sezione del modulo.  
  
```  
15:14:15 [INFO] Stopping service NTDS  
15:14:15 [INFO] Stopping service NtFrs  
15:14:15 [INFO] ControlService(STOP) on NtFrs returned 1(gle=0)  
15:14:15 [INFO] DsRolepWaitForService: waiting for NtFrs to enter one of 7 states  
15:14:15 [INFO] DsRolepWaitForService: QueryServiceStatus on NtFrs returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:16 [INFO] DsRolepWaitForService: QueryServiceStatus on NtFrs returned 1 (gle=0), SvcStatus.dwCS=1  
15:14:16 [INFO] DsRolepWaitForService: exiting because NtFrs entered STOPPED state  
15:14:16 [INFO] DsRolepWaitForService(for any end state) on NtFrs service returned 0  
15:14:16 [INFO] ControlService(STOP) on NtFrs returned 0(gle=1062)  
15:14:16 [INFO] Exiting service-stop loop after service NtFrs entered STOPPED state  
15:14:16 [INFO] StopService on NtFrs returned 0  
15:14:16 [INFO] Configuring service NtFrs to 1 returned 0  
15:14:16 [INFO] Stopping service Kdc  
15:14:16 [INFO] ControlService(STOP) on Kdc returned 1(gle=0)  
15:14:16 [INFO] DsRolepWaitForService: waiting for Kdc to enter one of 7 states  
15:14:16 [INFO] DsRolepWaitForService: QueryServiceStatus on Kdc returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:17 [INFO] DsRolepWaitForService: QueryServiceStatus on Kdc returned 1 (gle=0), SvcStatus.dwCS=1  
15:14:17 [INFO] DsRolepWaitForService: exiting because Kdc entered STOPPED state  
15:14:17 [INFO] DsRolepWaitForService(for any end state) on Kdc service returned 0  
15:14:17 [INFO] ControlService(STOP) on Kdc returned 0(gle=1062)  
15:14:17 [INFO] Exiting service-stop loop after service Kdc entered STOPPED state  
15:14:17 [INFO] StopService on Kdc returned 0  
15:14:17 [INFO] Configuring service Kdc to 1 returned 0  
15:14:17 [INFO] Stopping service DNS  
15:14:17 [INFO] ControlService(STOP) on DNS returned 1(gle=0)  
15:14:17 [INFO] DsRolepWaitForService: waiting for DNS to enter one of 7 states  
15:14:17 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:18 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:19 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:20 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:21 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:22 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:23 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:24 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:25 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:26 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:27 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:28 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:29 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:30 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:31 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:32 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:33 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:34 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:35 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:36 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:37 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:38 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:39 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:40 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:41 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:42 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:43 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:44 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:45 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:46 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:47 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:48 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:49 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:50 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:51 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:52 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:53 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:54 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:55 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:56 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:57 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:58 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:59 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:15:00 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=1  
15:15:00 [INFO] DsRolepWaitForService: exiting because DNS entered STOPPED state  
15:15:00 [INFO] DsRolepWaitForService(for any end state) on DNS service returned 0  
15:15:00 [INFO] ControlService(STOP) on DNS returned 0(gle=1062)  
15:15:00 [INFO] Exiting service-stop loop after service DNS entered STOPPED state  
15:15:00 [INFO] StopService on DNS returned 0  
15:15:00 [INFO] Configuring service DNS to 1 returned 0  
15:15:00 [INFO] ControlService(STOP) on NTDS returned 1(gle=1062)  
15:15:00 [INFO] DsRolepWaitForService: waiting for NTDS to enter one of 7 states  
15:15:00 [INFO] DsRolepWaitForService: QueryServiceStatus on NTDS returned 1 (gle=0), SvcStatus.dwCS=3  
15:15:01 [INFO] DsRolepWaitForService: QueryServiceStatus on NTDS returned 1 (gle=0), SvcStatus.dwCS=1  
15:15:01 [INFO] DsRolepWaitForService: exiting because NTDS entered STOPPED state  
15:15:01 [INFO] DsRolepWaitForService(for any end state) on NTDS service returned 0  
15:15:01 [INFO] ControlService(STOP) on NTDS returned 0(gle=1062)  
15:15:01 [INFO] Exiting service-stop loop after service NTDS entered STOPPED state  
15:15:01 [INFO] StopService on NTDS returned 0  
15:15:01 [INFO] Configuring service NTDS to 1 returned 0  
15:15:01 [INFO] Configuring service NTDS  
15:15:01 [INFO] Configuring service NTDS to 64 returned 0  
15:15:01 [INFO] vDC Cloning: Winlogon UI Notification #8: Domain Controller cloning is at 22% completion...  
15:15:01 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:01 [INFO] vDC Cloning: Winlogon UI Notification #9: Domain Controller cloning is at 25% completion...  
15:15:01 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Forzare la sincronizzazione dell'ora di NT5DS (NTP) con un altro controller di dominio, solitamente PDCE.  
  
```  
15:15:02 [INFO] Forcing time sync  
```  
  
-   Contattare il controller di domino che contiene il controller di dominio di origine del clone.  
  
-   Scaricare eventuali ticket Kerberos.  
  
```  
15:15:02 [INFO] Searching for a domain controller for the domain root.fabrikam.com that contains the account DC2$  
15:15:02 [INFO] Located domain controller DC1.root.fabrikam.com for domain root.fabrikam.com  
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #10: Domain Controller cloning is at 26% completion...  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:02 [INFO] Directing kerberos authentication to DC1.root.fabrikam.com returns 0  
15:15:02 [INFO] DsRolepFlushKerberosTicketCache() successfully flushed the Kerberos ticket cache  
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #11: Domain Controller cloning is at 27% completion...  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:02 [INFO] Using site Default-First-Site-Name for server \\DC1.root.fabrikam.com  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Arrestare il servizio Accesso rete e impostarne il tipo di avvio.  
  
```  
15:15:02 [INFO] Stopping service NETLOGON  
15:15:02 [INFO] Stopping service NETLOGON  
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #12: Domain Controller cloning is at 29% completion...  
15:15:02 [INFO] ControlService(STOP) on NETLOGON returned 1(gle=0)  
15:15:02 [INFO] DsRolepWaitForService: waiting for NETLOGON to enter one of 7 states  
15:15:02 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=3  
15:15:03 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=1  
15:15:03 [INFO] DsRolepWaitForService: exiting because NETLOGON entered STOPPED state  
15:15:03 [INFO] DsRolepWaitForService(for any end state) on NETLOGON service returned 0  
15:15:03 [INFO] ControlService(STOP) on NETLOGON returned 0(gle=1062)  
15:15:03 [INFO] Exiting service-stop loop after service NETLOGON entered STOPPED state  
15:15:03 [INFO] StopService on NETLOGON returned 0  
15:15:03 [INFO] Configuring service NETLOGON to 1 returned 0  
15:15:03 [INFO] Stopped NETLOGON  
15:15:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:03 [INFO] vDC Cloning: Winlogon UI Notification #13: Domain Controller cloning is at 30% completion...  
```  
  
-   Configurare i servizi DFSR/NTFRS per l'esecuzione automatica.  
  
-   Eliminare i file di database esistenti per forzare una sincronizzazione non autorevole di SYSVOL al successivo avvio del servizio.  
  
```  
15:15:03 [INFO] Configuring service DFSR  
15:15:03 [INFO] Configuring service DFSR to 256 returned 0  
15:15:03 [INFO] Configuring service NTFRS  
15:15:03 [INFO] Configuring service NTFRS to 256 returned 0  
15:15:03 [INFO] Removing DFSR Database files for SysVol  
15:15:03 [INFO] Removing FRS Database files in C:\Windows\ntfrs\jet  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edb.log  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbres00001.jrs  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbres00002.jrs  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbtmp.log  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\ntfrs.jdb  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\sys\edb.chk  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\temp\tmp.edb  
15:15:04 [INFO] Created system volume path  
15:15:04 [INFO] Configuring service DFSR  
15:15:04 [INFO] Configuring service DFSR to 128 returned 0  
15:15:04 [INFO] Configuring service NTFRS  
15:15:04 [INFO] Configuring service NTFRS to 128 returned 0  
15:15:04 [INFO] vDC Cloning: Winlogon UI Notification #14: Domain Controller cloning is at 40% completion...  
15:15:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Avviare il processo di promozione con il file di database NTDS esistente.  
  
-   Contattare il master RID.  
  
> [!NOTE]  
> Il servizio Servizi di domini Active Directory non è in realtà installato qui, si tratta di strumentazione legacy nel log.  
  
```  
15:15:04 [INFO] Installing the Directory Service  
15:15:04 [INFO] Calling NtdsInstall for root.fabrikam.com  
15:15:04 [INFO] Starting Active Directory Domain Services installation  
15:15:04 [INFO] Validating user supplied options  
15:15:04 [INFO] Determining a site in which to install  
15:15:04 [INFO] Examining an existing forest...  
15:15:04 [INFO] Starting a replication cycle between DC1.root.fabrikam.com and the RID operations master (2008r2-01.root.fabrikam.com), so that the new replica will be able to create users, groups, and computer objects...  
15:15:04 [INFO] Configuring the local computer to host Active Directory Domain Services  
15:15:04 [INFO] EVENTLOG (Warning): NTDS General / Service Control : 1539  
Active Directory Domain Services could not disable the software-based disk write cache on the following hard disk.  
Hard disk:  
c:  
Data might be lost during system failures.  
15:15:10 [INFO] EVENTLOG (Informational): NTDS General / Internal Processing : 2041  
Duplicate event log entries were suppressed.  
See the previous event log entry for details. An entry is considered a duplicate if  
the event code and all of its insertion parameters are identical. The time period for  
this run of duplicates is from the time of the previous event to the time of this event.  
Event Code:  
80000603  
Number of duplicate entries:   
2  
15:15:10 [INFO] EVENTLOG (Informational): NTDS General / Internal Configuration : 2121  
This Active Directory Domain Services server is disabling the Recycle Bin. Deleted objects may not be undeleted at this time.  
```  
  
-   Modificare l'ID di chiamata esistente nel database dei computer di origine.  
  
-   Creare un nuovo oggetto Impostazioni NTDS per il clone.  
  
-   Eseguire la replica nel delta dell'oggetto Active Directory dal controller di dominio del partner.  
  
> [!NOTE]  
> Anche se tutti gli oggetti sono elencati come replicati, in realtà si tratta solo dei metadati necessari per includere gli aggiornamenti. Tutti gli oggetti non modificati nel database NTDS clonato esistono già e non richiedono una nuova replica, come nel caso della promozione basata su IFM.  
  
```  
15:15:10 [INFO] EVENTLOG (Informational): NTDS Replication / Replication : 1109  
The invocationID attribute for this directory server has been changed. The highest update sequence number at the time the backup was created is as follows:  
InvocationID attribute (old value):  
24e7b22f-4706-402d-9b4f-f2690f730b40  
InvocationID attribute (new value):  
f74cefb2-89c2-442c-b1ba-3234b0ed62f8  
Update sequence number:  
20520  
The invocationID is changed when a directory server is restored from backup media, is configured to host a writeable application directory partition, has been resumed after a virtual machine snapshot has been applied, after a virtual machine import operation, or after a live migration operation. Virtualized domain controllers should not be restored using virtual machine snapshots. The supported method to restore or rollback the content of an Active Directory Domain Services database is to restore a system state backup made with an Active Directory Domain Services-aware backup application.  
15:15:10 [INFO] EVENTLOG (Error): NTDS General / Internal Processing : 1168  
Internal error: An Active Directory Domain Services error has occurred.  
Additional Data  
Error value (decimal):  
2  
Error value (hexadecimal):  
2  
Internal ID:  
7011658  
15:15:11 [INFO] Creating the NTDS Settings object for this Active Directory Domain Controller on the remote AD DC DC1.root.fabrikam.com...  
15:15:11 [INFO] Replicating the schema directory partition  
15:15:11 [INFO] Replicated the schema container.  
15:15:12 [INFO] Active Directory Domain Services updated the schema cache.  
15:15:12 [INFO] Replicating the configuration directory partition  
15:15:12 [INFO] Replicating data CN=Configuration,DC=root,DC=fabrikam,DC=com: Received 2612 out of approximately 2612 objects and 94 out of approximately 94 distinguished name (DN) values...  
15:15:12 [INFO] Replicated the configuration container.  
15:15:13 [INFO] Replicating critical domain information...  
15:15:13 [INFO] Replicating data DC=root,DC=fabrikam,DC=com: Received 109 out of approximately 109 objects and 35 out of approximately 35 distinguished name (DN) values...  
15:15:13 [INFO] Replicated the critical objects in the domain container.  
```  
  
-   Popolare le partizioni del catalogo globale con eventuali aggiornamenti mancanti.  
  
-   Completare la parte di Servizi di dominio Active Directory cruciale per la promozione.  
  
```  
15:15:13 [INFO] EVENTLOG (Informational): NTDS General / Global Catalog : 1110  
Promotion of this domain controller to a global catalog will be delayed for the following interval.  
Interval (minutes):  
5  
This delay is necessary so that the required directory partitions can be prepared before the global catalog is advertised. In the registry, you can specify the number of seconds that the directory system agent will wait before promoting the local domain controller to a global catalog. For more information about the Global Catalog Delay Advertisement registry value, see the Resource Kit Distributed Systems Guide.  
15:15:14 [INFO] EVENTLOG (Informational): NTDS General / Service Control : 1000  
Microsoft Active Directory Domain Services startup complete, version 6.2.8225.0   
15:15:15 [INFO] Creating new domain users, groups, and computer objects  
15:15:16 [INFO] Completing Active Directory Domain Services installation  
15:15:16 [INFO] NtdsInstall for root.fabrikam.com returned 0  
15:15:16 [INFO] DsRolepInstallDs returned 0  
15:15:16 [INFO] Installed Directory Service  
```  
  
-   Completare la replica in ingresso di SYSVOL.  
  
```  
15:15:16 [INFO] vDC Cloning: Winlogon UI Notification #15: Domain Controller cloning is at 60% completion...  
15:15:16 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] Completed system volume replication  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #16: Domain Controller cloning is at 70% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] SetProductType to 2 [LanmanNT] returned 0  
15:15:18 [INFO] Set the product type  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #17: Domain Controller cloning is at 71% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #18: Domain Controller cloning is at 72% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] Set the system volume path for NETLOGON  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #19: Domain Controller cloning is at 73% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] Replicating non critical information  
15:15:18 [INFO] User specified to not replicate non-critical data  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #20: Domain Controller cloning is at 80% completion...  
15:15:18 [INFO] Stopped the DS  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #21: Domain Controller cloning is at 90% completion...  
15:15:18 [INFO] Configuring service NTDS  
15:15:18 [INFO] Configuring service NTDS to 16 returned 0  
```  
  
-   Abilitare la registrazione del DNS client.  
  
```  
15:15:18 [INFO] vDC Cloning: Set DisableDynamicUpdate reg value to 0 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set UseDynamicDns reg value to 1 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set RegistrationEnabled reg value to 1 to enable dynamic update records registration.  
```  
  
-   Eseguire i moduli SYSPREP specificati dall'elemento DefaultDCCloneAllowList.xml <SysprepInformation>.  
  
```  
15:15:18 [INFO] vDC Cloning: Running sysprep providers.  
15:15:32 [INFO] vDC Cloning: Completed running sysprep providers.  
```  
  
-   La promozione della clonazione è a questo punto completata.  
  
-   Rimuovere il flag di avvio in modalità ripristino servizi directory in modo che la volta successiva il server venga avviato normalmente.  
  
-   Rinominare il file dccloneconfig.xml in modo che non venga letto di nuovo al successivo avvio.  
  
-   Riavviare il computer.  
  
```  
15:15:32 [INFO] The attempted domain controller operation has completed  
15:15:32 [INFO] Updating service status to 4  
15:15:32 [INFO] DsRolepSetOperationDone returned 0  
15:15:32 [INFO] vDC Cloning: Set vDCCloningComplete event.  
15:15:32 [INFO] vDC Cloneing: Clearing Boot into DSRM flag succeeded.  
15:15:32 [INFO] vDC Cloning: Winlogon UI Notification #22: Cloning Domain Controller succeeded. Now rebooting...  
15:15:33 [INFO] vDC Cloning: Renamed vDC clone configuration file.  
15:15:33 [INFO] vDC Cloning: The old name is: C:\Windows\NTDS\DCCloneConfig.xml  
15:15:33 [INFO] vDC Cloning: The new name is: C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml  
15:15:34 [INFO] vDC Cloning: Release Ipv4 on interface 'Wired Ethernet Connection 2', result=0.  
15:15:34 [INFO] vDC Cloning: Release Ipv6 on interface 'Wired Ethernet Connection 2', result=0.  
15:15:34 [INFO] Rebooting machine  
```  
  
##### <a name="active-directory-web-services-event-log"></a>Registro eventi di Servizi Web Active Directory  
Durante la clonazione, il database NTDS.DIT è spesso offline per periodi prolungati di tempo. Il servizio Servizi Web Active Directory registra almeno un evento per questa situazione. Al termine della clonazione, il servizio Servizi Web Active Directory viene avviato, nota che non è ancora disponibile un certificato di computer valido (la disponibilità varia a seconda che l'ambiente distribuisca o meno un'infrastruttura PKI Microsoft con registrazione automatica) e quindi avvia l'istanza del nuovo controller di dominio.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**1202**|Eventi di istanze di Servizi Web Active Directory|Il computer in uso ospita l'istanza di directory specificata, tuttavia Servizi Web non è in grado di servirla. Servizi Web Active Directory ritenterà l'operazione periodicamente.<br /><br />Istanza di directory: NTDS<br /><br />Porta LDAP dell'istanza di directory: 389<br /><br />Porta SSL dell'istanza di directory: 636|  
|**1000**|Eventi di istanze di Servizi Web Active Directory|Avvio di Servizi Web Active Directory in corso.|  
|**1008**|Eventi di istanze di Servizi Web Active Directory|Servizi Web Active Directory: i privilegi di sicurezza sono stati ridotti.|  
|**1100**|Eventi di istanze di Servizi Web Active Directory|I valori specificati nella sezione <appsettings> del file di configurazione di Servizi Web Active Directory sono stati caricati senza errori.|  
|**1400**|Eventi di istanze di Servizi Web Active Directory|Eventi di certificati di Servizi Web Active Directory Servizi Web Active Directory: impossibile trovare un certificato del server con il nome specificato. Per l'utilizzo di connessioni SSL/TLS è necessario un certificato. Per utilizzare connessioni SSL/TLS, verificare che nel computer sia installato un certificato di autenticazione server valido emesso da un'Autorità di certificazione (CA) attendibile.<br /><br />Nome certificato: *<Server FQDN>*|  
|**1100**|Eventi di istanze di Servizi Web Active Directory|I valori specificati nella sezione <appsettings> del file di configurazione di Servizi Web Active Directory sono stati caricati senza errori.|  
|**1200**|Eventi di istanze di Servizi Web Active Directory|Servizi Web Active Directory: l'istanza di directory specificata viene servita.<br /><br />Istanza di directory: NTDS<br /><br />Porta LDAP dell'istanza di directory: 389<br /><br />Porta SSL dell'istanza di directory: 636|  
  
##### <a name="dns-server-event-log"></a>Registro eventi del server DNS  
Durante la clonazione si verificano brevi interruzioni previste del servizio DNS, che è ancora in esecuzione mentre il database di Servizi di dominio Active Directory è offline. Ciò si verifica se si usa la zona DNS integrata in Active Directory, ma non la zona DNS primaria o secondaria standard. Questi errori vengono registrati più volte. Al termine della clonazione, il servizio DNS torna normalmente online.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**4013**|DNS-Server-Service|Il server DNS è in attesa della conferma del completamento della sincronizzazione iniziale della directory da parte di Servizi di dominio Active Directory. Impossibile avviare il servizio Server DNS prima del completamento della sincronizzazione iniziale: è possibile che dati DNS importanti non siano ancora replicati sul controller di dominio in uso. Se il registro eventi di Servizi di dominio Active Directory indica che si è verificato un problema nella risoluzione dei nomi DNS, è possibile aggiungere all'elenco dei server DNS nelle proprietà del protocollo IP per il computer in uso l'indirizzo IP di un altro server DNS per il dominio. L'evento sarà registrato ogni due minuti fino a quando sarà ricevuta la conferma di completamento della sincronizzazione iniziale da parte di Servizi di dominio Active Directory.|  
|**4015**|DNS-Server-Service|Il server DNS ha rilevato un errore critico in Active Directory. Controllare che Active Directory funzioni correttamente. Le informazioni di debug estese sull'errore (che potrebbero non essere presenti) sono """". L'errore è contenuto nei dati dell'evento.|  
|**4000**|DNS-Server-Service|Server DNS: impossibile aprire Active Directory.  Il server DNS è configurato in modo da ottenere e utilizzare le informazioni per la zona dalla directory e non è in grado di caricare la zona senza di essa.  Controllare che Active Directory funzioni correttamente e ricaricare la zona. I dati dell'evento sono rappresentati dal codice di errore.|  
|**4013**|DNS-Server-Service|Il server DNS è in attesa della conferma del completamento della sincronizzazione iniziale della directory da parte di Servizi di dominio Active Directory. Impossibile avviare il servizio Server DNS prima del completamento della sincronizzazione iniziale: è possibile che dati DNS importanti non siano ancora replicati sul controller di dominio in uso. Se il registro eventi di Servizi di dominio Active Directory indica che si è verificato un problema nella risoluzione dei nomi DNS, è possibile aggiungere all'elenco dei server DNS nelle proprietà del protocollo IP per il computer in uso l'indirizzo IP di un altro server DNS per il dominio. L'evento sarà registrato ogni due minuti fino a quando sarà ricevuta la conferma di completamento della sincronizzazione iniziale da parte di Servizi di dominio Active Directory.|  
|**2**|DNS-Server-Service|Il server DNS è avviato.|  
|**4**|DNS-Server-Service|Il caricamento in background delle zone da parte del server DNS è stato completato. Tutte le zone sono ora disponibili per gli aggiornamenti del DNS e i trasferimenti di zona, in base a quanto consentito dalla rispettiva configurazione di zona.|  
  
##### <a name="file-replication-service-event-log"></a>Registro eventi del servizio Replica file  
Il servizio Replica file viene sincronizzato in modo non autorevole da un partner durante la clonazione. La clonazione ottiene questo risultato eliminando i file di database NTFRS e lasciando inalterato il contenuto di SYSVOL per l'uso come dati sottoposti a pre-seeding. I due tentativi di sincronizzazione sono previsti.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**13562**|NtFrs|Di seguito è riportato il riepilogo dei messaggi di avviso e di errore riscontrati dal servizio Replica file durante il polling del controller di dominio DC2.root.fabrikam.com per recuperare le informazioni di configurazione del set di repliche del servizio Replica file.<br /><br />Impossibile eseguire il binding a un controller di dominio. L'operazione verrà ripetuta nel prossimo ciclo di polling.|  
|**13502**|NtFrs|Sta per essere arrestato il servizio Replica file.|  
|**13565**|NtFrs|Il servizio Replica file sta inizializzando il volume di sistema con dati provenienti da un altro controller di dominio. Il computer DC2 può diventare un controller di dominio solo al termine di questo processo. Il volume di sistema verrà quindi condiviso con il nome SYSVOL.<br /><br />Per controllare la presenza della condivisione SYSVOL, al prompt dei comandi digitare:<br /><br />net share<br /><br />Quando il servizio di Replica file ha completato il processo di inizializzazione, compare la condivisione SYSVOL.<br /><br />L'inizializzazione del volume di sistema può richiedere un certo tempo. Quanto tempo dipende dalla quantità di dati presente nel volume di sistema, dalla disponibilità di altri controller di dominio e dall'intervallo di replica tra i controller di dominio.|  
|**13501**|NtFrs|Sta per essere avviato il servizio Replica file.|  
|**13502**|NtFrs|Sta per essere arrestato il servizio Replica file.|  
|**13503**|NtFrs|Servizio Replica file interrotto.|  
|**13565**|NtFrs|Il servizio Replica file sta inizializzando il volume di sistema con dati provenienti da un altro controller di dominio. Il computer DC2 può diventare un controller di dominio solo al termine di questo processo. Il volume di sistema verrà quindi condiviso con il nome SYSVOL.<br /><br />Per controllare la presenza della condivisione SYSVOL, al prompt dei comandi digitare:<br /><br />net share<br /><br />Quando il servizio di Replica file ha completato il processo di inizializzazione, compare la condivisione SYSVOL.<br /><br />L'inizializzazione del volume di sistema può richiedere un certo tempo. Quanto tempo dipende dalla quantità di dati presente nel volume di sistema, dalla disponibilità di altri controller di dominio e dall'intervallo di replica tra i controller di dominio.|  
|**13501**|NtFrs|Sta per essere avviato il servizio Replica file.|  
|**13553**|NtFrs|Il servizio Replica file ha aggiunto questo computer al seguente insieme di replica:<br /><br />"DOMAIN SYSTEM VOLUME (SYSVOL SHARE)"<br /><br />Le informazioni relative a questo evento vengono mostrate di seguito:<br /><br />Nome DNS del computer  *<Domain Controller FQDN>*<br /><br />Nome del membro set di replica è *<Domain Controller>*<br /><br />Percorso radice dell'insieme di replica è *<path>*<br /><br />Percorso directory gestione temporanea della replica è *<path>*<br /><br />Percorso di directory di lavoro di replica è *<path>*|  
|**13520**|NtFrs|Il servizio Replica File ha spostato i file <path>al *<path>* \ntfrs_preexisting___see_eventlog.<br /><br />Il servizio Replica File potrebbe eliminare i file in *<path>* \NtFrs_PreExisting___See_EventLog in qualsiasi momento. I file possono essere salvati dall'eliminazione per copiarli *<path>* \ntfrs_preexisting___see_eventlog. Se i file vengono copiati in c:\windows\sysvol\domain possono verificarsi conflitti di nome nel caso in cui i file esistano già su altri partner di replica.<br /><br />In alcuni casi, il servizio Replica File copia un file da *<path>* \NtFrs_PreExisting___See_EventLog a *<path>* invece di replicare il file da un altro partner di replica.<br /><br />È possibile recuperare spazio eliminando i file in qualsiasi momento *<path>* \ntfrs_preexisting___see_eventlog. "|  
|**13508**|NtFrs|Servizio replica File problemi durante l'abilitazione della replica dal *\\ \\ <Domain Controller FQDN>* alla *<Domain Controller>* per *<path>* tramite il<br /><br />Nome DNS *\\ \\ <Domain Controller FQDN>*. Continuerà a tentare.<br /><br />Seguono alcune cause possibili di questo avviso.<br /><br />[1] il servizio Replica file non è possibile risolvere correttamente il nome DNS *\\ \\ <Domain Controller FQDN>* da questo computer.<br /><br />[2] servizio Replica file non è in esecuzione *\\ \\ <Domain Controller FQDN>*.<br /><br />[3] Le informazioni topologiche in Active Directory per questa replica non sono ancora state replicate su tutti i controller di dominio.<br /><br />Questo messaggio del registro eventi viene visualizzato una sola volta per connessione. Alla risoluzione del problema appare un altro messaggio del registro eventi che indica che è stata stabilita.|  
|**13509**|NtFrs|Il servizio Replica File ha attivato la replica dal *\\ \\ <Domain Controller FQDN>* alla *<Domain Controller>* per *<Path>* dopo ripetuti tentativi.|  
|**13516**|NtFrs|Il servizio Replica File è non impedisce più computer *<Domain Controller>* di diventare un controller di dominio. L'inizializzazione del volume di sistema è riuscita e al servizio Accesso rete è stato notificato che il volume di sistema può essere ora condiviso come SYSVOL.<br /><br />Digitare "net share" per controllare la condivisione SYSVOL."|  
  
##### <a name="dfs-replication-event-log"></a>Registro eventi di Replica DFS  
Il servizio Replica DFS viene sincronizzato in modo non autorevole da un partner durante la clonazione. La clonazione ottiene questo risultato eliminando i file di database DFSR e lasciando inalterato il contenuto di SYSVOL per l'uso come dati sottoposti a pre-seeding. I due tentativi di sincronizzazione sono previsti.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**1004**|Replica DFS|Il servizio Replica DFS è stato avviato.|  
|**1314**|Replica DFS|Servizio Replica DFS: i file registro di debug sono stati configurati<br /><br />Ulteriori informazioni:<br /><br />Percorso file registro di debug: C:\Windows\debug|  
|**6102**|Replica DFS|Il provider WMI è stato registrato in modo corretto.|  
|**1206**|Replica DFS|Servizio Replica DFS: contatto con il controller di dominio DC2.corp.contoso.com per accedere alle informazioni di configurazione.|  
|**1210**|Replica DFS|Servizio replica DFS: è stato impostato un listener RPC per le richieste di replica in ingresso.<br /><br />Ulteriori informazioni:<br /><br />Port: 0"|  
|**4614**|Replica DFS|Il servizio Replica DFS ha inizializzato SYSVOL nel percorso locale C:\Windows\SYSVOL\domain ed è in attesa di eseguire la replica iniziale. La cartella replicata manterrà lo stato di sincronizzazione iniziale fino al termine della replica con il partner Se era in corso la promozione del server a controller di dominio, il controller di dominio non visualizzerà annunci e non funzionerà come controller di dominio finché il problema non verrà risolto. Questo problema può verificarsi quando anche il partner specificato si trova nello stato di sincronizzazione iniziale oppure quando vengono rilevate violazioni di condivisione nel server o nel partner di sincronizzazione. Se questo evento si è verificato durante la migrazione di SYSVOL dal servizio Replica file (FRS) a Replica DFS, le modifiche non verranno replicate finché il problema non verrà risolto. La cartella SYSVOL di questo server potrebbe non essere sincronizzata con gli altri controller di dominio.<br /><br />Ulteriori informazioni:<br /><br />Nome cartella replicata: SYSVOL Share<br /><br />ID cartella replicata: *<GUID>*<br /><br />Nome gruppo di replica: Domain System Volume<br /><br />ID del gruppo di replica: *<GUID>*<br /><br />ID membro: *<GUID>*<br /><br />Sola lettura: 0|  
|**4604**|Replica DFS|Servizio Replica DFS: inizializzazione della cartella replicata SYSVOL nel percorso locale C:\Windows\SYSVOL\domain completata. Questo membro ha completato la sincronizzazione iniziale di SYSVOL con il partner dc1.corp.contoso.com.  Per verificare la presenza della condivisione SYSVOL, aprire una finestra del prompt dei comandi e digitare "net share".<br /><br />Ulteriori informazioni:<br /><br />Nome cartella replicata: SYSVOL Share<br /><br />ID cartella replicata: *<GUID>*<br /><br />Nome gruppo di replica: Domain System Volume<br /><br />ID del gruppo di replica: *<GUID>*<br /><br />ID membro: *<GUID>*<br /><br />Partner sincronizzazione: *<domain controller FQDN>*|  
  
## <a name="BKMK_TshootVDCSafeRestore"></a>Risoluzione dei problemi di ripristino sicuro di controller di dominio virtualizzati  
  
### <a name="tools-for-troubleshooting"></a>Strumenti per risoluzione dei problemi  
  
#### <a name="logging-options"></a>Opzioni di registrazione  
I log predefiniti rappresentano lo strumento più importante per la risoluzione dei problemi di ripristino sicuro degli snapshot di controller di dominio. Per impostazione predefinita, tutti questi log sono abilitati e configurati per il massimo livello di dettaglio.  
  
|||  
|-|-|  
|**Operazione**|**Log**|  
|**Creazione di snapshot**|-Event Visualizzatore eventi\log di applicazioni e servizi logs\Microsoft\Windows\Hyper-V-Worker|  
|**Ripristino dello snapshot**|-Event Visualizzatore eventi\log di applicazioni e servizi\servizio directory<br />-Visualizzatore eventi\log di Windows\Sistema<br />-Visualizzatore eventi\log di windows\applicazione<br />-Event Visualizzatore eventi\log di applicazioni e servizi servizi\servizio replica file<br />-Event Visualizzatore eventi\log di applicazioni e servizi\replica DFS<br />-Event Visualizzatore eventi\log di applicazioni e servizi\dfs<br />-Event Visualizzatore eventi\log di applicazioni e servizi logs\Microsoft\Windows\Hyper-V-Worker|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Strumenti e comandi per la risoluzione dei problemi di configurazione dei controller di dominio  
Per risolvere i problemi non spiegati dai log, usare gli strumenti seguenti come punto di partenza:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Network Monitor 3.4  
  
#### <a name="BKMK_TshhotSafeRestore"></a>Metodologia generale per la risoluzione dei problemi di ripristino sicuro di Controller di dominio  
  
1.  Il ripristino sicuro dello snapshot è previsto, ma si verificano problemi?  
  
    1.  Esaminare il registro eventi dei servizi directory.  
  
        1.  Si verificano errori di ripristino dello snapshot?  
  
        2.  Si verificano errori di replica di Active Directory?  
  
    2.  Esaminare il registro eventi di sistema.  
  
        1.  Si verificano errori di comunicazione?  
  
        2.  Si verificano errori di Active Directory?  
  
2.  Il ripristino sicuro dello snapshot è imprevisto?  
  
    1.  Esaminare i log di controllo per determinare chi o cosa ha causato un rollback.  
  
    2.  Contattare tutti gli amministratori dell'hypervisor per scoprire chi ha effettuato il rollback della macchina virtuale senza preavviso.  
  
3.  Il server implementa la protezione tramite rollback degli USN e non esegue un ripristino sicuro?  
  
    1.  Esaminare il registro eventi dei servizi directory per rilevare un hypervisor o servizi di integrazione non supportati.  
  
    2.  Esaminare il sistema operativo e verificare l'esecuzione di Windows Server 2012.  
  
### <a name="BKMK_TshootSpecificSafeRestore"></a>Risoluzione di problemi specifici  
  
#### <a name="events"></a>Eventi  
Tutti gli eventi di ripristino sicuro degli snapshot di controller di dominio vengono scritti nel registro eventi dei servizi directory della macchina virtuale del controller di dominio ripristinato. Anche i registri eventi dell'applicazione, del servizio Replica file e del servizio Replica DFS possono contenere informazioni utili per la risoluzione dei problemi di ripristini non riusciti.  
  
Di seguito sono illustrati gli eventi specifici del ripristino sicuro di Windows Server 2012 riportati nel registro eventi dei servizi directory.  
  
|||  
|-|-|  
|**ID evento**|**2170**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Avviso|  
|**Messaggio**|È stata rilevata una modifica all'ID di generazione.<br /><br />ID generazione memorizzato nella cache in DS (valore precedente):%1<br /><br />ID generazione attualmente nella macchina virtuale (nuovo valore):%2<br /><br />La modifica dell'ID di generazione si verifica dopo l'applicazione di uno snapshot della macchina virtuale, dopo un'operazione di importazione di una macchina virtuale o dopo un'operazione Live Migration. *<COMPUTERNAME>* verrà creato un nuovo ID di chiamata per ripristinare il controller di dominio. I controller di dominio virtualizzati non devono essere ripristinati mediante snapshot della macchina virtuale. Il metodo supportato per il ripristino o il rollback del contenuto di un database di Servizi di dominio Active Directory consiste nel ripristinare un backup dello stato del sistema creato con un'applicazione di backup abilitata per Servizi di dominio Active Directory.|  
|**Note e risoluzione**|Si tratta di un evento riuscito se lo snapshot era previsto. In caso contrario, esaminare il registro eventi di Hyper-V-Worker o contattare l'amministratore dell'hypervisor.|  
  
|||  
|-|-|  
|**ID evento**|**2174**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|l controller di dominio non è un clone di controller di dominio virtuale né uno snapshot di controller di dominio virtuale ripristinato.|  
|**Note e risoluzione**|Si tratta di un evento previsto quando si avviano controller di dominio fisici o controller di dominio virtualizzati non ripristinati da snapshot.|  
  
|||  
|-|-|  
|**ID evento**|**2181**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|La transazione è stata annullata a causa del ripristino dello stato precedente della macchina virtuale.  Questo errore si verifica dopo l'applicazione di uno snapshot della macchina virtuale, dopo un'operazione di importazione di una macchina virtuale o dopo un'operazione Live Migration.|  
|**Note e risoluzione**|Si tratta di un evento previsto quando si ripristina uno snapshot. Le transazioni tengono traccia della modifica dell'ID di generazione VM.|  
  
|||  
|-|-|  
|**ID evento**|**2185**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>* arrestare il servizio FRS o DFSR usato per replicare la cartella SYSVOL.<br /><br />Nome servizio:%1<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>* deve inizializzare un ripristino non autorevole per la replica SYSVOL locale. L'operazione viene eseguita arrestando il servizio FRS o DFSR usato per replicare la cartella SYSVOL e avviandolo con i valori e le chiavi del Registro di sistema appropriati per attivare il ripristino. L'evento 2187 verrà registrato al riavvio del servizio FRS o DFSR.|  
|**Note e risoluzione**|Si tratta di un evento previsto quando si ripristina uno snapshot. Tutti i dati SYSVOL presenti nel controller di dominio vengono sostituiti con una copia del controller di dominio del partner.|  
|**ID evento**|2186|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|*<COMPUTERNAME>* non è stato possibile arrestare il servizio FRS o DFSR usato per replicare la cartella SYSVOL.<br /><br />Nome servizio:%1<br /><br />Codice errore: %2<br /><br />Messaggio di errore: % 3<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>* deve inizializzare un ripristino non autorevole per la replica SYSVOL locale. L'operazione viene eseguita arrestando il servizio di replica FRS o DFSR usato per replicare la cartella SYSVOL e avviandolo con le chiavi e i valori del Registro di sistema appropriati per attivare il ripristino. *<COMPUTERNAME>* Impossibile arrestare il servizio corrente in esecuzione e non può completare il ripristino non autorevole. Esegui il ripristino non autorevole manualmente.|  
|**Note e risoluzione**|Esaminare i registri eventi di sistema, del servizio Replica file e del servizio Replica DFS per altre informazioni.|  
  
|||  
|-|-|  
|**ID evento**|**2187**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>* avviare il servizio FRS o DFSR usato per replicare la cartella SYSVOL.<br /><br />Nome servizio:%1<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>* ha dovuto inizializzare un ripristino non autorevole per la replica SYSVOL locale. L'operazione è stata eseguita arrestando il servizio FRS o DFSR usato per replicare la cartella SYSVOL e avviandolo con i valori e le chiavi del Registro di sistema appropriati per attivare il ripristino.|  
|**Note e risoluzione**|Si tratta di un evento previsto quando si ripristina uno snapshot. Tutti i dati SYSVOL presenti nel controller di dominio vengono sostituiti con una copia del controller di dominio del partner.|  
  
|||  
|-|-|  
|**ID evento**|**2188**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|*<COMPUTERNAME>* Impossibile avviare il servizio FRS o DFSR usato per replicare la cartella SYSVOL.<br /><br />Nome servizio:%1<br /><br />Codice errore: %2<br /><br />Messaggio di errore: % 3<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>* deve inizializzare un ripristino non autorevole per la replica SYSVOL locale. L'operazione viene eseguita arrestando il servizio FRS o DFSR usato per replicare SYSVOL e avviandolo con i valori e le chiavi del Registro di sistema appropriati per attivare il ripristino. *<COMPUTERNAME>* Impossibile avviare il servizio FRS o DFSR usato per replicare la cartella SYSVOL e non può completare il ripristino non autorevole. Esegui il ripristino non autorevole manualmente e riavvia il servizio.|  
|**Note e risoluzione**|Esaminare i registri eventi di sistema, del servizio Replica file e del servizio Replica DFS per altre informazioni.|  
  
|||  
|-|-|  
|**ID evento**|**2189**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>* impostare i valori del Registro di sistema seguente per inizializzare la replica SYSVOL durante un ripristino non autorevole:<br /><br />Chiave del Registro di sistema:%1<br /><br />Valore del Registro di sistema: %2<br /><br />Dati del valore del Registro di sistema: %3<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>* deve inizializzare un ripristino non autorevole per la replica SYSVOL locale. L'operazione viene eseguita arrestando il servizio FRS o DFSR usato per replicare la cartella SYSVOL e avviandolo con i valori e le chiavi del Registro di sistema appropriati per attivare il ripristino.|  
|**Note e risoluzione**|Si tratta di un evento previsto quando si ripristina uno snapshot. Tutti i dati SYSVOL presenti nel controller di dominio vengono sostituiti con una copia del controller di dominio del partner.|  
  
|||  
|-|-|  
|**ID evento**|**2190**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|*<COMPUTERNAME>* non è stato possibile impostare i valori del Registro di sistema seguenti per inizializzare la replica SYSVOL durante un ripristino non autorevole:<br /><br />Chiave del Registro di sistema:%1<br /><br />Valore del Registro di sistema: %2<br /><br />Dati del valore del Registro di sistema: %3<br /><br />Codice errore: %4<br /><br />Messaggio di errore: %5<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>* deve inizializzare un ripristino non autorevole per la replica SYSVOL locale. L'operazione viene eseguita arrestando il servizio FRS o DFSR usato per replicare la cartella SYSVOL e avviandolo con i valori e le chiavi del Registro di sistema appropriati per attivare il ripristino. *<COMPUTERNAME>* Impossibile impostare i seguenti valori del Registro di sistema e non è possibile completare il ripristino non autorevole. Esegui il ripristino non autorevole manualmente.|  
|**Note e risoluzione**|Esaminare i registri eventi di sistema e dell'applicazione. Verificare la presenza di applicazioni di terze parti che potrebbero bloccare gli aggiornamenti del Registro di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2200**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>* inizializzata la replica per aggiornare il controller di dominio. L'evento 2201 verrà registrato al termine della replica.|  
|**Note e risoluzione**|Si tratta di un evento previsto quando si ripristina uno snapshot. Segna l'inizio di una replica di Active Directory in ingresso.|  
  
|||  
|-|-|  
|**ID evento**|**2201**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>* è stata completata la replica per aggiornare il controller di dominio.|  
|**Note e risoluzione**|Si tratta di un evento previsto quando si ripristina uno snapshot. Segna la fine di una replica di Active Directory in ingresso.|  
  
|||  
|-|-|  
|**ID evento**|**2202**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>* replica non riusciti per aggiornare il controller di dominio. Il controller di dominio verrà aggiornato dopo la prossima replica periodica.|  
|**Note e risoluzione**|Esaminare i registri eventi dei servizi directory e di sistema. Usare repadmin.exe per provare a forzare la replica e controllare se si verificano errori.|  
  
|||  
|-|-|  
|**ID evento**|**2204**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>* è stata rilevata una modifica dell'ID di generazione macchina virtuale. Questa modifica indica che è stato ripristinato uno stato precedente del controller di dominio virtuale. *<COMPUTERNAME>* verranno eseguite le operazioni seguenti per proteggere tale controller di dominio da possibili divergenze di dati e per proteggere la creazione di entità di sicurezza con SID duplicati:<br /><br />Creazione di un nuovo ID di chiamata<br /><br />Invalidamento del pool di RID corrente<br /><br />Convalida della proprietà dei ruoli FSMO alla successiva replica in ingresso. Durante questa fase, se il controller di dominio ha ricoperto un ruolo FSMO, questo non risulterà disponibile.<br /><br />Avvio dell'operazione di ripristino del servizio di replica SYSVOL.<br /><br />Avvio della replica per impostare lo stato più recente del controller di dominio ripristinato.<br /><br />Richiesta di un nuovo pool di RID.|  
|**Note e risoluzione**|Si tratta di un evento previsto quando si ripristina uno snapshot. Questo spiega tutte le varie operazioni di reimpostazione che si verificheranno come parte del processo di ripristino sicuro.|  
  
|||  
|-|-|  
|**ID evento**|**2205**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>* invalidato il pool RID corrente dopo il controller di dominio virtuale è stata ripristinata a uno stato precedente.|  
|**Note e risoluzione**|Si tratta di un evento previsto quando si ripristina uno snapshot. Il pool di RID locale deve essere rimosso, in quanto il controller di dominio è più aggiornato e possono essersi già verificati problemi.|  
  
|||  
|-|-|  
|**ID evento**|**2206**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|ERRORE|  
|**Messaggio**|*<COMPUTERNAME>* : Impossibile invalidare il pool RID corrente dopo il ripristino dello stato precedente controller di dominio virtuale.<br /><br />Dati aggiuntivi.<br /><br />Codice di errore: %1<br /><br />Valore errore: %2|  
|**Note e risoluzione**|Esaminare i registri eventi dei servizi directory e di sistema. Verificare che il master RID sia online e che possa essere raggiunto dal server con Dcdiag.exe /test:ridmanager|  
  
|||  
|-|-|  
|**ID evento**|**2207**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|ERRORE|  
|**Messaggio**|*<COMPUTERNAME>* Impossibile ripristinare dopo il ripristino dello stato precedente controller di dominio virtuale. Riavviare il sistema in DSRM. Per ulteriori informazioni, controllare gli eventi precedenti.|  
|**Note e risoluzione**|Esaminare i registri eventi dei servizi directory e di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2208**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>* eliminare i database DFSR per inizializzare la replica SYSVOL durante un ripristino non autorevole.|  
|**Note e risoluzione**|Si tratta di un evento previsto quando si ripristina uno snapshot. Garantisce che il servizio DFSR non sincronizzi in modo non autorevole SYSVOL da un controller di dominio partner. Si noti che anche qualsiasi altra cartella replicata con DFS nello stesso volume di SYSVOL verrà sincronizzata in modo non autorevole. Non è consigliabile che i controller di dominio ospitino set di repliche DFS personalizzate nello stesso volume di SYSVOL.|  
  
|||  
|-|-|  
|**ID evento**|**2209**|  
|**Origine**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Severity**|Errore|  
|**Messaggio**|*<COMPUTERNAME>* non è stato possibile eliminare i database DFSR.<br /><br />Dati aggiuntivi.<br /><br />Codice di errore: %1<br /><br />Valore errore: %2<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>* deve inizializzare un ripristino non autorevole per la replica SYSVOL locale. Per DFSR l'operazione viene eseguita arrestando il servizio DFSR, eliminando i database DFSR e riavviando il servizio. Al riavvio DFSR ricostruirà i database e avvierà la sincronizzazione iniziale.|  
|**Note e risoluzione**|Esaminare il registro eventi di Replica DFS.|  
  
#### <a name="error-messages"></a>Messaggi di errore  
Non ci sono messaggi di errore interattivi diretti per la mancata riuscita del ripristino sicuro dello snapshot di un controller di dominio virtualizzato. Tutte le informazioni di clonazione vengono registrate nei registri eventi dei servizi directory. Ovviamente, eventuali errori critici di replica o di invio annunci del server si manifestano come sintomi altrove.  
  
#### <a name="known-issues-and-support-scenarios"></a>Problemi noti e probabili e scenari di supporto  
Il [metodologia generale per la risoluzione dei problemi di dominio ripristino sicuro dei Controller](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootSpecificSafeRestore) sono in genere adeguati per risolvere la maggior parte dei problemi.  
  
|||  
|-|-|  
|**Problema**|**Non è possibile creare nuove entità di sicurezza sul controller di dominio ripristinato di recente sicuro**|  
|**Sintomi**|Dopo il ripristino di uno snapshot, i tentativi di creare una nuova entità di sicurezza (utente, computer, gruppo) in questo dominio non riescono e viene visualizzato il messaggio:<br /><br />Errore 0x2010<br /><br />Il servizio directory non è in grado di allocare l'identificatore relativo.|  
|**Risoluzione e note**|Questo problema è causato dalle informazioni non aggiornate del computer ripristinato sul ruolo FSMO del master RID. Se il ruolo è stato spostato in questo o in un altro controller di dominio dopo l'acquisizione dello snapshot e in seguito è stato ripristinato, il controller di dominio ripristinato non avrà informazioni sul master RID finché non verrà completata la replica iniziale.<br /><br />Per risolvere il problema, consentire il completamento della replica di Active Directory in ingresso nel controller di dominio ripristinato. Se il problema persiste, verificare che tutti i controller di dominio abbiano le stesse informazioni corrette sul controller di dominio che ospita il master RID.|  
  
|||  
|-|-|  
|**Problema**|**Controller di dominio ripristinati non condividono SYSVOL e pubblicizzare**|  
|**Sintomi**|Dopo il ripristino di uno snapshot, uno o più controller di dominio non inviano annunci, non condividono SYSVOL e non hanno contenuti SYSVOL aggiornati.|  
|**Risoluzione e note**|I partner upstream del controller di dominio non hanno una replica di SYSVOL funzionante che viene replicata correttamente con Replica DFS o Replica file. Questo problema non è correlato al ripristino sicuro, ma è probabile che indichi un problema di ripristino sicuro, perché il cliente non era al corrente dell'altro problema di replica che interessa i controller di dominio non ripristinati.|  
  
### <a name="advanced-troubleshooting"></a>Risoluzione avanzata dei problemi  
Questo modulo contiene informazioni sulla risoluzione avanzata dei problemi, usando log *operativi* come esempi, oltre a una spiegazione dell'evento che si è verificato. Con una buona comprensione del funzionamento corretto di un controller di dominio virtualizzato, è possibile identificare con maggior chiarezza gli errori nell'ambiente. Questi log vengono presentati dalla relativa origine, con l'ordine crescente di eventi *previsti* correlati a un controller di dominio clonato all'interno di ogni log.  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-dfsr"></a>Ripristino di un controller di dominio che replica SYSVOL con Replica DFS  
  
##### <a name="directory-services-event-log"></a>Registro eventi dei servizi directory  
Il log dei servizi directory contiene la maggior parte delle informazioni operative sul ripristino sicuro. L'hypervisor cambia l'ID di generazione VM e il servizio NTDS ne prende nota, quindi invalida il pool di RID e cambia l'ID di richiesta. Il nuovo ID di generazione VM viene impostato e i server replicano i dati di Active Directory in ingresso. Il servizio Replica DFS viene arrestato e il relativo database che ospita SYSVOL viene eliminato, forzando una sincronizzazione non autorevole in ingresso. Il limite massimo di USN viene modificato.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**2170**|ActiveDirectory_DomainService|È stata rilevata una modifica all'ID di generazione.<br /><br />ID generazione memorizzato nella cache in DS (valore precedente):<br /><br />*<number>*<br /><br />ID generazione attualmente nella macchina virtuale (nuovo valore):<br /><br />*<number>*<br /><br />La modifica dell'ID di generazione si verifica dopo l'applicazione di uno snapshot della macchina virtuale, dopo un'operazione di importazione di una macchina virtuale o dopo un'operazione Live Migration. In Servizi di dominio Active Directory verrà creato un nuovo ID di chiamata per ripristinare il controller di dominio. I controller di dominio virtualizzati non devono essere ripristinati mediante snapshot della macchina virtuale. Il metodo supportato per il ripristino o il rollback del contenuto di un database di Servizi di dominio Active Directory consiste nel ripristinare un backup dello stato del sistema creato con un'applicazione di backup abilitata per Servizi di dominio Active Directory.|  
|**2181**|ActiveDirectory_DomainService|La transazione è stata annullata a causa del ripristino dello stato precedente della macchina virtuale.  Questo errore si verifica dopo l'applicazione di uno snapshot della macchina virtuale, dopo un'operazione di importazione di una macchina virtuale o dopo un'operazione Live Migration.|  
|**2204**|ActiveDirectory_DomainService|Servizi di dominio Active Directory: è stata rilevata una modifica dell'ID di generazione della macchina virtuale. Questa modifica indica che è stato ripristinato uno stato precedente del controller di dominio virtuale. Verranno eseguite le operazioni seguenti per proteggere tale controller di dominio da possibili divergenze di dati e per proteggere la creazione di entità di sicurezza con SID duplicati:<br /><br />Creazione di un nuovo ID di chiamata<br /><br />Invalidamento del pool di RID corrente<br /><br />Convalida della proprietà dei ruoli FSMO alla successiva replica in ingresso. Durante questa fase, se il controller di dominio ha ricoperto un ruolo FSMO, questo non risulterà disponibile.<br /><br />Avvio dell'operazione di ripristino del servizio di replica SYSVOL.<br /><br />Avvio della replica per impostare lo stato più recente del controller di dominio ripristinato.<br /><br />Richiesta di un nuovo pool di RID.|  
|**2181**|ActiveDirectory_DomainService|La transazione è stata annullata a causa del ripristino dello stato precedente della macchina virtuale.  Questo errore si verifica dopo l'applicazione di uno snapshot della macchina virtuale, dopo un'operazione di importazione di una macchina virtuale o dopo un'operazione Live Migration.|  
|**1109**|ActiveDirectory_DomainService|L'attributo invocationID relativo a questo server di directory è stato modificato. Il numero di sequenza di aggiornamento più alto al momento del backup è stato creato nel modo seguente:<br /><br />Attributo InvocationID (vecchio valore):<br /><br />*<GUID>*<br /><br />Attributo InvocationID (nuovo valore):<br /><br />*<GUID>*<br /><br />Numero di sequenza di aggiornamento:<br /><br />*<number>*<br /><br />L'attributo invocationID viene modificato quando un server di directory viene ripristinato dal supporto di backup,viene configurato per l'hosting di una partizione di directory applicativa scrivibile, viene ripreso dopo l'applicazione di una snapshot di macchina virtuale, dopo un'operazione di importazione di una macchina virtuale o dopo un'operazione Live Migration. I controller di dominio virtualizzati non devono essere ripristinati mediante snapshot della macchina virtuale. Il metodo supportato per il ripristino o il rollback del contenuto di un database di Servizi di dominio Active Directory consiste nel ripristinare un backup dello stato del sistema creato con un'applicazione di backup abilitata per Servizi di dominio Active Directory.|  
|**2179**|ActiveDirectory_DomainService|L'attributo msDS-GenerationId dell'oggetto computer del controller di dominio è stato impostato sul parametro seguente:<br /><br />Attributo GenerationID:<br /><br />*<number>*|  
|**2200**|ActiveDirectory_DomainService|Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. In Servizi di dominio Active Directory verrà inizializzata la replica per aggiornare il controller di dominio. L'evento 2201 verrà registrato al termine della replica.|  
|**2201**|ActiveDirectory_DomainService|Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. In Servizi di dominio Active Directory la replica per aggiornare il controller di dominio è stata completata.|  
|**2185**|ActiveDirectory_DomainService|Servizi di dominio Active Directory ha arrestato il servizio FRS o DFSR usato per replicare la cartella SYSVOL.<br /><br />Nome servizio:<br /><br />Replica DFS<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. Servizi di dominio Active Directory deve inizializzare un ripristino non autorevole per la replica SYSVOL locale. L'operazione viene eseguita arrestando il servizio FRS o DFSR usato per replicare la cartella SYSVOL e avviandolo con i valori e le chiavi del Registro di sistema appropriati per attivare il ripristino. L'evento 2187 verrà registrato al riavvio del servizio FRS o DFSR.|  
|**2208**|ActiveDirectory_DomainService|Servizi di dominio Active Directory ha eliminato i database DFSR per inizializzare la replica SYSVOL durante un ripristino non autorevole.<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. Servizi di dominio Active Directory deve inizializzare un ripristino non autorevole per la replica SYSVOL locale. Per DFSR l'operazione viene eseguita arrestando il servizio DFSR, eliminando i database DFSR e riavviando il servizio. Al riavvio DFSR ricostruirà i database e avviare la sincronizzazione iniziale. "|  
|**2187**|ActiveDirectory_DomainService|Servizi di dominio Active Directory ha avviato il servizio FRS o DFSR usato per replicare la cartella SYSVOL.<br /><br />Nome servizio:<br /><br />Replica DFS<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. Servizi di dominio Active Directory ha dovuto inizializzare un ripristino non autorevole per la replica SYSVOL locale. L'operazione è stata eseguita arrestando il servizio FRS o DFSR usato per replicare la cartella SYSVOL e avviandolo con i valori e le chiavi del Registro di sistema appropriati per attivare il ripristino. "|  
|**1587**|ActiveDirectory_DomainService|Il servizio directory è stato ripristinato oppure configurato per l'hosting di una partizione di directory applicativa. Di conseguenza, la relativa identità di replica è cambiata. Un partner ha chiesto cambiamenti di replica utilizzando l'identità precedente dell'utente. Il numero della sequenza iniziale è stato corretto.<br /><br />Il servizio directory di destinazione corrispondente al seguente GUID oggetto ha chiesto cambiamenti a partire dall'USN che precede quello in cui il servizio directory locale è stato ripristinato dal supporto di backup<br /><br />GUID oggetto:<br /><br />*<GUID> (<FQDN of partner domain controller>)*<br /><br />USN al momento del ripristino<br /><br />*<number>*<br /><br />Di conseguenza, il vettore degli aggiornamenti del servizio directory di destinazione è stato configurato in base alle seguenti impostazioni.<br /><br />GUID database precedente:<br /><br />*<GUID>*<br /><br />Oggetto USN precedente:<br /><br />*<number>*<br /><br />Proprietà USN precedente:<br /><br />*<number>*<br /><br />Nuovo GUID database:<br /><br />*<GUID>*<br /><br />Nuovo USN oggetto:<br /><br />*<number>*<br /><br />Nuovo USN proprietà:<br /><br />*<number>*|  
  
##### <a name="system-event-log"></a>Registro eventi di sistema  
Il registro eventi di sistema annota l'ora del computer quando una macchina virtuale offline viene portata nuovamente online e l'ora della sincronizzazione con l'host. Il pool di RID viene invalidato e i servizi Replica DFS o Replica file vengono riavviati.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**1**|Kernel-General|L'ora di sistema cambiata da *?<now>* dal *< ora/data snapshot >*.<br /><br />Motivo modifica: Un'applicazione o un componente del sistema ha modificato l'ora.|  
|**16654**|Directory-Services-SAM|Un pool di identificatori di account (RID) è stato invalidato. Questa situazione può verificarsi nei seguenti casi previsti:<br /><br />1. Un controller di dominio viene ripristinato da un backup.<br /><br />2. Un controller di dominio in esecuzione in una macchina virtuale viene ripristinato da uno snapshot.<br /><br />3. Un amministratore ha invalidato manualmente il pool.<br /><br />Per ulteriori informazioni, vedere https://go.microsoft.com/fwlink/?LinkId=226247.|  
|**7036**|Gestione controllo servizi|Il servizio Replica DFS è stato interrotto.|  
|**7036**|Gestione controllo servizi|Il servizio Replica DFS è in esecuzione.|  
  
##### <a name="application-event-log"></a>Registro eventi dell'applicazione  
Il registro eventi dell'applicazione annota l'arresto e l'avvio del database DFSR.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**103**|ESENT|DFSR (1360) \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db: Il motore di database ha arrestato l'istanza (0).<br /><br />Arresto anomalo: 0<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.141, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.016, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**102**|ESENT|DFSR (532) \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db: Il motore di database (6.02.8189.0000) sta avviando una nuova istanza (0).|  
|**105**|ESENT|DFSR (532) \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db: Il motore di database ha avviato una nuova istanza (0). (Tempo=0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000.|  
|||DFSR (532) \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db: Il motore di database ha creato un nuovo database (1, \\ \\.\C:\System Volume Information\DFSR\database *_<GUID>* \dfsr.db). (Tempo=0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.016, [4] 0.062, [5] 0.000, [6] 0.016, [7] 0.000, [8] 0.000, [9] 0.015, [10] 0.000, [11] 0.000.|  
  
##### <a name="dfs-replication-event-log"></a>Registro eventi di Replica DFS  
Il servizio Replica DFS viene arrestato e il database che contiene SYSVOL viene eliminato, forzando una sincronizzazione non autorevole in ingresso.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**1006**|Replica DFS|È in corso l'interruzione del servizio Replica DFS.|  
|**1008**|Replica DFS|Il servizio Replica DFS è stato arrestato.|  
|**1002**|Replica DFS|È in corso l'avvio del servizio Replica DFS.|  
|**1004**|Replica DFS|Il servizio Replica DFS è stato avviato.|  
|**1314**|Replica DFS|Servizio Replica DFS: i file registro di debug sono stati configurati<br /><br />Ulteriori informazioni:<br /><br />Percorso file registro di debug: C:\Windows\debug|  
|**6102**|Replica DFS|Il provider WMI è stato registrato in modo corretto.|  
|**1206**|Replica DFS|Il controller di dominio è stato contattato servizio Replica DFS *<domain controller FQDN>* per accedere alle informazioni di configurazione.|  
|**1210**|Replica DFS|Servizio replica DFS: è stato impostato un listener RPC per le richieste di replica in ingresso.<br /><br />Ulteriori informazioni:<br /><br />Port: 0|  
|**4614**|Replica DFS|Il servizio Replica DFS ha inizializzato SYSVOL nel percorso locale C:\Windows\SYSVOL\domain ed è in attesa di eseguire la replica iniziale. La cartella replicata manterrà lo stato di sincronizzazione iniziale fino al termine della replica con il partner Se era in corso la promozione del server a controller di dominio, il controller di dominio non visualizzerà annunci e non funzionerà come controller di dominio finché il problema non verrà risolto. Questo problema può verificarsi quando anche il partner specificato si trova nello stato di sincronizzazione iniziale oppure quando vengono rilevate violazioni di condivisione nel server o nel partner di sincronizzazione. Se questo evento si è verificato durante la migrazione di SYSVOL dal servizio Replica file (FRS) a Replica DFS, le modifiche non verranno replicate finché il problema non verrà risolto. La cartella SYSVOL di questo server potrebbe non essere sincronizzata con gli altri controller di dominio.<br /><br />Ulteriori informazioni:<br /><br />Nome cartella replicata: SYSVOL Share<br /><br />ID cartella replicata: *<GUID>*<br /><br />Nome gruppo di replica: Domain System Volume<br /><br />ID del gruppo di replica: *<GUID>*<br /><br />ID membro: *<GUID>*<br /><br />Sola lettura: 0|  
|**4604**|Replica DFS|Servizio Replica DFS: inizializzazione della cartella replicata SYSVOL nel percorso locale C:\Windows\SYSVOL\domain completata. Questo membro ha completato la sincronizzazione iniziale di SYSVOL con il partner dc1.corp.contoso.com.  Per verificare la presenza della condivisione SYSVOL, aprire una finestra del prompt dei comandi e digitare "net share".<br /><br />Ulteriori informazioni:<br /><br />Nome cartella replicata: SYSVOL Share<br /><br />ID cartella replicata: *<GUID>*<br /><br />Nome gruppo di replica: Domain System Volume<br /><br />ID del gruppo di replica: *<GUID>*<br /><br />ID membro: *<GUID>*<br /><br />Partner sincronizzazione: *<partner domain controller FQDN>*|  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-frs"></a>Ripristino di un controller di dominio che replica SYSVOL con Replica file  
Il registro eventi di Replica file viene usato al posto del registro eventi di Replica DFS in questo caso. Anche il registro eventi dell'applicazione scrive diversi eventi correlati al servizio Replica file. Altrimenti, i messaggi dei registri eventi dei servizi directory e di sistema si trovano in genere nello stesso ordine descritto in precedenza.  
  
##### <a name="file-replication-service-event-log"></a>Registro eventi del servizio Replica file  
Il servizio Replica file viene arrestato e riavviato con un valore D2 BURFLAGS per la sincronizzazione non autorevole di SYSVOL.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**13502**|NTFRS|Sta per essere arrestato il servizio Replica file.|  
|**13503**|NTFRS|Servizio Replica file interrotto.|  
|**13501**|NTFRS|Sta per essere avviato il servizio Replica file.|  
|**13512**|NTFRS|Il servizio Replica file ha rilevato una cache in scrittura del disco attivata sull'unità che contiene la directory c:\windows\ntfrs\jet sul computer DC4. Nel caso di interruzione dell'alimentazione all'unità è possibile che il servizio Replica file non venga ripristinato e che aggiornamenti critici vadano perduti.|  
|**13565**|NTFRS|Il servizio Replica file sta inizializzando il volume di sistema con dati provenienti da un altro controller di dominio. Il computer DC4 può diventare un controller di dominio solo al termine di questo processo. Il volume di sistema verrà quindi condiviso con il nome SYSVOL.<br /><br />Per controllare la presenza della condivisione SYSVOL, al prompt dei comandi digitare:<br /><br />net share<br /><br />Quando il servizio di Replica file ha completato il processo di inizializzazione, compare la condivisione SYSVOL.<br /><br />L'inizializzazione del volume di sistema può richiedere un certo tempo. Quanto tempo dipende dalla quantità di dati presente nel volume di sistema, dalla disponibilità di altri controller di dominio e dall'intervallo di replica tra i controller di dominio.|  
|**13520**|NTFRS|Il servizio Replica File ha spostato i file *<path>* al *<path>* \ntfrs_preexisting___see_eventlog.<br /><br />Il servizio Replica File potrebbe eliminare i file in *<path>* \NtFrs_PreExisting___See_EventLog in qualsiasi momento. I file possono essere salvati dall'eliminazione per copiarli *<path>* \ntfrs_preexisting___see_eventlog. Copia dei file in *<path>* può causare conflitti di nome nel caso i file esistano già su altri partner di replica.<br /><br />In alcuni casi, il servizio Replica File copia un file da *<path>* \NtFrs_PreExisting___See_EventLog a *<path>* invece di replicare il file da un altro partner di replica.<br /><br />È possibile recuperare spazio eliminando i file in qualsiasi momento *<path>* \ntfrs_preexisting___see_eventlog.|  
|**13553**|NTFRS|Il servizio Replica file ha aggiunto questo computer al seguente insieme di replica:<br /><br />"DOMAIN SYSTEM VOLUME (SYSVOL SHARE)"<br /><br />Le informazioni relative a questo evento vengono mostrate di seguito:<br /><br />Nome DNS del computer è "*<domain controller FQDN>*"<br /><br />Nome del membro set di replica è "*<domain controller name>*"<br /><br />Percorso radice dell'insieme di replica è "*<path>*"<br /><br />Percorso della directory di gestione temporanea della replica è "*<path>* "<br /><br />Percorso di directory di lavoro di replica è "*<path>*"|  
|**13554**|NTFRS|Il servizio Replica file ha aggiunto le seguenti connessioni all'insieme di replica:<br /><br />"DOMAIN SYSTEM VOLUME (SYSVOL SHARE)"<br /><br />In ingresso da "*<partner domain controller FQDN>*"<br /><br />In uscita da "*<partner domain controller FQDN>*"<br /><br />Maggiori informazioni potranno essere visualizzate nei successivi messaggi del registro eventi.|  
|**13516**|NTFRS|Il servizio Replica file non impedisce più al computer DC4 di diventare un controller di dominio. L'inizializzazione del volume di sistema è riuscita e al servizio Accesso rete è stato notificato che il volume di sistema può essere ora condiviso come SYSVOL.<br /><br />Digitare "net share" per controllare la condivisione SYSVOL.|  
  
##### <a name="application-event-log"></a>Registro eventi dell'applicazione  
Il database del servizio Replica file viene arrestato e avviato e viene ripulito a causa dell'operazione D2 BURFLAGS.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**327**|ESENT|ntfrs (1424) Il motore di database ha scollegato un database (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tempo=0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.015, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.516, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.063, [12] 0.000.<br /><br />Cache ripristinata: 0|  
|**103**|ESENT|ntfrs (1424) Il motore di database ha arrestato l'istanza (0).<br /><br />Arresto anomalo: 0<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.016, [12] 0.000, [13] 0.000, [14] 0.047, [15] 0.000.|  
|**102**|ESENT|ntfrs (3000) Il motore di database (6.02.8189.0000) sta avviando una nuova istanza (0).|  
|**105**|ESENT|ntfrs (3000) Il motore di database ha avviato una nuova istanza (0). (Tempo=0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.062, [10] 0.000, [11] 0.141.|  
|**103**|ESENT|ntfrs (3000) Il motore di database ha arrestato l'istanza (0).<br /><br />Arresto anomalo: 0<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.015, [14] 0.000, [15] 0.000.|  
|**102**|ESENT|ntfrs (3000) Il motore di database (6.02.8189.0000) sta avviando una nuova istanza (0).|  
|**105**|ESENT|ntfrs (3000) Il motore di database ha avviato una nuova istanza (0). (Tempo=0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.078, [10] 0.000, [11] 0.109.|  
|**325**|ESENT|ntfrs (3000) Il motore di database ha creato un nuovo database (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tempo=0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.016, [4] 0.016, [5] 0.000, [6] 0.015, [7] 0.000, [8] 0.000, [9] 0.078, [10] 0.016, [11] 0.000.|  
|**103**|ESENT|ntfrs (3000) Il motore di database ha arrestato l'istanza (0).<br /><br />Arresto anomalo: 0<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.078, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.125, [10] 0.016, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**102**|ESENT|ntfrs (3000) Il motore di database (6.02.8189.0000) sta avviando una nuova istanza (0).|  
|**105**|ESENT|ntfrs (3000) Il motore di database ha avviato una nuova istanza (0). (Tempo=0 secondi)<br /><br />Sequenza temporale interna: [1] 0.016, [2] 0.000, [3] 0.000, [4] 0.094, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.032, [10] 0.000, [11] 0.000.|  
|**326**|ESENT|ntfrs (3000) Il motore di database ha collegato un database (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tempo=0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.015, [3] 0.000, [4] 0.000, [5] 0.016, [6] 0.015, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<br /><br />Cache salvata: 1|  
  


