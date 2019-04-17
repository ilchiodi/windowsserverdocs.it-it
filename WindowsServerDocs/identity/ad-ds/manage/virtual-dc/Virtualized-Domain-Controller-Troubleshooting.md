---
ms.assetid: 249ba1be-b0d3-4a77-99af-3699074a2b6e
title: Dominio virtualizzati Controller risoluzione dei problemi
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 63f96e7a3035bc25f7a7a349acb6bf26f62a2a3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-troubleshooting"></a>Dominio virtualizzati Controller risoluzione dei problemi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento fornisce una metodologia dettagliata sulla risoluzione dei problemi di funzionalità del controller di dominio virtualizzati.  
  
-   [Risoluzione dei problemi virtualizzato clonazione del controller di dominio](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCCloning)  
  
-   [Risoluzione dei problemi di ripristino sicuro di controller di dominio virtualizzati](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCSafeRestore)  
  
## <a name="BKMK_Intro"></a>Introduzione  
Il modo più importante per migliorare le tue conoscenze sulla risoluzione dei problemi è di un lab di test ed esaminare rigoroso normale scenari operativi. Se si verificano errori, sono più ovvi e facili da comprendere, dato che hai quindi una base solida del funzionamento della promozione del controller di dominio. In questo modo è possibile creare le tue competenze analisi in analisi e della rete. Questo vale per tutte le tecnologie di sistemi distribuiti, distribuzione del controller di dominio virtualizzati non appena.  
  
Gli elementi cruciali per la risoluzione avanzata dei problemi di configurazione del controller di dominio sono:  
  
1.  Analisi lineare abbinata a una particolare attenzione ai dettagli.  
  
2.  Informazioni sulle analisi di acquisizione di rete  
  
3.  Interpretazione dei log predefiniti  
  
Il primo e secondo esulano dall'abito di questo argomento, mentre il terzo può essere illustrato in maggior dettaglio. Risoluzione dei problemi di controller di dominio virtualizzati richiede un metodo logico e lineare. La chiave consiste nell'affrontare il problema usando i dati forniti e ricorrere all'analisi e strumenti complessi solo dopo aver esaurito l'output e la registrazione.  
  
## <a name="BKMK_TshootVDCCloning"></a>Risoluzione dei problemi virtualizzato clonazione del controller di dominio  
In questa sezione viene illustrato come:  
  
-   [Strumenti per la risoluzione dei problemi](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_Tools)  
  
-   [Opzioni di registrazione](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_LoggingOptions)  
  
-   [Metodologia generale per la risoluzione dei problemi di dominio la clonazione del Controller](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)  
  
-   [Server Core e nel registro eventi](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_ServerCoreEvents)  
  
-   [Risoluzione di problemi specifici](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_SpecificProblems)  
  
La strategia di risoluzione dei problemi di clonazione del controller di dominio virtualizzati segue questo formato generale:  
  
![risoluzione dei problemi di controller di dominio virtuale](media/Virtualized-Domain-Controller-Troubleshooting/ADDS_VDC_TroublehsootingFlowchart.png)  
  
### <a name="BKMK_Tools"></a>Strumenti per la risoluzione dei problemi  
  
#### <a name="BKMK_LoggingOptions"></a>Opzioni di registrazione  
I log predefiniti rappresentano lo strumento più importante per la risoluzione dei problemi di clonazione del controller di dominio. Tutti questi log sono abilitati e configurati per massimo livello di dettaglio, per impostazione predefinita.  
  
|||  
|-|-|  
|**Operazione**|**Registro**|  
|**La clonazione**|-Visualizzatore eventi\log di Windows\Sistema<br />-Event eventi\log di applicazioni e Servizi\servizio directory<br />-%systemroot%\Debug\Dcpromo.log.|  
|**L'innalzamento di livello**|-%systemroot%\Debug\Dcpromo.log.<br />-Event eventi\log di applicazioni e Servizi\servizio directory<br />-Visualizzatore eventi\log di Windows\Sistema<br />-Event eventi\log di applicazioni e servizi\servizio replica file<br />-Event eventi\log di applicazioni e servizi\replica DFS|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Strumenti e comandi per la risoluzione dei problemi di configurazione del Controller di dominio  
Per risolvere i problemi non spiegati dai log, usare gli strumenti seguenti come punto di partenza:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Network Monitor 3.4  
  
### <a name="BKMK_GeneralMethodology"></a>Metodologia generale per la risoluzione dei problemi di dominio la clonazione del Controller  
  
1.  La VM viene avviata in modalità ripristino directory (DSRM)? Questo valore indica la risoluzione dei problemi necessarie. Per accedere in modalità ripristino servizi directory, utilizzare **. \Administrator** account e specificare la password modalità ripristino servizi directory.  
  
    1.  Esaminare il log Dcpromo.log.  
  
        1.  Procedura di clonazione iniziale ha esito positivo ma promozione del controller di dominio non riesce?  
  
        2.  Gli errori indicano problemi con il controller di dominio locale o con l'ambiente di dominio Active Directory, ad esempio errori restituiti dall'emulatore del PDC?  
  
    2.  Esaminare i registri eventi di sistema e dei servizi Directory e il file dccloneconfig.xml e CustomDCCloneAllowList.xml  
  
        1.  Un'applicazione incompatibile deve essere in CustomDCCloneAllowList.xml elenco Consenti?  
  
        2.  È il nome di computer o indirizzo IP è duplicato o invalido in dccloneconfig.xml?  
  
        3.  Sito di Active Directory non è valido in dccloneconfig.xml?  
  
        4.  L'indirizzo IP non è impostato in dccloningconfig.xml e non è disponibile alcun server DHCP?  
  
        5.  È l'emulatore PDC online e disponibile tramite il protocollo RPC?  
  
        6.  È il controller di dominio membro del gruppo di controller di dominio clonabili? L'autorizzazione **consentire un controller di dominio di creare un clone di se stesso** imposta nell'elemento radice per il gruppo di dominio?  
  
        7.  Il file Dccloneconfig.xml contiene errori di sintassi che impediscono l'analisi corretta?  
  
        8.  L'hypervisor è supportato?  
  
        9. Non ha esito negativo di innalzamento di livello controller di dominio dopo il corretto avvio della clonazione?  
  
        10. È stato il numero massimo di nomi controller di dominio generati automaticamente (9999) superato?  
  
        11. L'indirizzo MAC è duplicato?  
  
2.  Nome host del clone è lo stesso come controller di dominio di origine?  
  
    1.  Esiste un file Dccloneconfig.xml in uno dei percorsi consentiti?  
  
3.  La VM viene avviata in modalità normale e la clonazione è completata, ma il controller di dominio non funziona correttamente?  
  
    1.  Prima di tutto, controlla se il nome host è cambiato nel clone. Se il nome host è diverso, la clonazione almeno parzialmente completata.  
  
    2.  Il controller di dominio dispone di un indirizzo IP duplicato del controller di dominio di origine da file dccloneconfig.xml, ma il controller di dominio di origine era offline durante la clonazione?  
  
    3.  Se il controller di dominio invia annunci, trattare il problema come qualsiasi problema di post-promozione normale che potrebbe verificarsi senza clonazione.  
  
    4.  Se il controller di dominio non invia annunci, esaminare i registri eventi del servizio Directory, sistema, applicazione, la replica dei File e replica DFS per gli errori di post-promozioni.  
  
#### <a name="disabling-dsrm-boot"></a>La disabilitazione di avvio in modalità DSRM  
Dopo l'avvio in modalità DSRM a causa di un errore, diagnosticare la causa dell'errore e se il log dcpromo.log non indica che la clonazione non può essere ritentata, correggere la causa dell'errore e reimpostare il flag della modalità ripristino servizi directory. Un clone non riuscito non torna in modalità normale autonomamente al successivo riavvio; è necessario rimuovere il flag di avvio in modalità ripristino servizi directory per riprovare la clonazione. Tutti questi passaggi richiedono l'esecuzione come amministratore con privilegi elevati.  
  
##### <a name="removing-dsrm-with-msconfigexe"></a>Rimozione della modalità DSRM con Msconfig.exe  
Per attivare avvio in modalità DSRM con una GUI, utilizzare lo strumento di configurazione di sistema:  
  
1.  Eseguire msconfig.exe  
  
2.  Nel **avvio** nella scheda **opzioni di avvio**, deselezionare **avvio in modalità provvisoria** (è già selezionata con l'opzione **Ripristina Active Directory** abilitato)  
  
3.  Fare clic su OK e riavviare quando richiesto.  
  
##### <a name="removing-dsrm-with-bcdeditexe"></a>Rimozione della modalità DSRM con Bcdedit.exe  
Per disattivare l'avvio in modalità DSRM dalla riga di comando, utilizzare l'Editor dell'archivio dati configurazione di avvio:  
  
1.  Aprire un prompt dei comandi ed eseguire:  
  
    ```  
    Bcdedit.exe /deletevalue safeboot  
    ```  
  
2.  Riavviare il computer con:  
  
    ```  
    Shutdown.exe /t /0 /r  
    ```  
  
> [!NOTE]  
> Bcdedit.exe funziona anche in una console di Windows PowerShell. I comandi sono:  
>   
> Modalità provvisoria /deletevalue bcdedit.exe  
>   
> Riavvio del computer  
  
### <a name="BKMK_ServerCoreEvents"></a>Server Core e nel registro eventi  
I registri eventi contengono molte informazioni utili sulle operazioni di clonazione di controller di dominio virtualizzati. Per impostazione predefinita, l'installazione su un computer Windows Server 2012 è un'installazione Server Core, che non esiste alcuna interfaccia grafica e, pertanto, nessun modo per eseguire lo snap-in Visualizzatore eventi locale.  
  
Per esaminare i registri eventi in un server che esegue un'installazione Server Core:  
  
-   Eseguire lo strumento Wevtutil.exe in locale  
  
-   Eseguire il cmdlet Get-WinEvent di PowerShell in locale  
  
-   Se è stata abilitata le regole avanzate di Windows Firewall per i gruppi "Gestione remota registro eventi" (o porte equivalenti) consentire la comunicazione in ingresso, è possibile gestire il registro eventi in remoto usando Eventvwr.exe, wevtutil.exe o Get-Winevent. Questa operazione può essere eseguita in installazioni Server Core usando NETSH.exe, criteri di gruppo o il nuovo cmdlet Set-NetFirewallRule in Windows PowerShell 3.0.  
  
> [!WARNING]  
> Non tentare di riaggiungere la shell grafica al computer mentre è in modalità ripristino servizi directory. Lo stack (CBS) di manutenzione di Windows non può funzionare correttamente in modalità provvisoria o DSRM. I tentativi di aggiungere funzionalità o ruoli in modalità DSRM non verranno completato e il computer rimarrà in uno stato instabile finché verrà avviato normalmente. Poiché un clone di controller di dominio virtualizzati in modalità DSRM non può essere avviato normalmente e non deve essere avviato normalmente in molti casi, è Impossibile aggiungere correttamente la shell grafica. Questa operazione non è supportata e può lasciarti con un server inutilizzabile.  
  
### <a name="BKMK_SpecificProblems"></a>Risoluzione di problemi specifici  
  
#### <a name="events"></a>Eventi  
Tutti eventi di clonazione di controller di dominio virtualizzati scrivere nel registro eventi di servizi di Directory del controller di dominio clone macchina virtuale. I registri eventi dell'applicazione, servizio Replica File e replica DFS possono contenere anche informazioni sulla risoluzione dei problemi utili per la clonazione non riuscita. Errori durante la chiamata RPC all'emulatore PDC potrebbero essere disponibili nel registro eventi sull'emulatore PDC.  
  
Di seguito sono gli eventi di Windows Server 2012 la clonazione specifico nel registro eventi di servizi di Directory, con note e le soluzioni consigliate per gli errori.  
  
##### <a name="directory-services-event-log"></a>Registro eventi dei servizi directory  
  
|||  
|-|-|  
|**ID evento**|**2160**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|Locale * <COMPUTERNAME> * ha individuato un controller di dominio virtuale i file di configurazione della clonazione.<br /><br />File di configurazione della clonazione di controller di dominio virtuale viene trovato in: %1<br /><br />L'esistenza del file configurazione clonazione controller di dominio virtuale indica che il controller di dominio virtuale locale è un clone di un altro controller di dominio virtuale. Il * <COMPUTERNAME> * verrà avviato per eseguire l'autoclonazione.|  
|**Note e risoluzione**|Si tratta di un evento riuscito e un problema solo se imprevisto. Esaminare la Directory di lavoro DSA, systemroot%\ntds e radice di eventuali dischi locali o rimovibili cercando il file dcclconeconfig.xml.|  
  
|||  
|-|-|  
|**ID evento**|**2161**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|Locale * <COMPUTERNAME> * non ha individuato il controller di dominio virtuale i file di configurazione della clonazione. Il computer locale non è un controller di dominio clonato.|  
|**Note e risoluzione**|Si tratta di un evento riuscito e un problema solo se imprevisto. Esaminare la Directory di lavoro DSA, systemroot%\ntds e radice di eventuali dischi locali o rimovibili cercando il file dcclconeconfig.xml.|  
  
|||  
|-|-|  
|**ID evento**|**2162**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|Clonazione del controller di dominio virtuale non è riuscito.<br /><br />Controllare gli eventi registrati nei registri eventi di sistema e %systemroot%\debug\dcpromo.log per ulteriori informazioni sugli errori corrispondenti al tentativo di clonazione del controller di dominio virtuale.<br /><br />Codice di errore: %1|  
|**Note e risoluzione**|Seguire le istruzioni del messaggi, questo errore è generico.|  
  
|||  
|-|-|  
|**ID evento**|**2163**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|È stato avviato il servizio DsRoleSvc per clonare il controller di dominio virtuale locale.|  
|**Note e risoluzione**|Si tratta di un evento riuscito e un problema solo se imprevisto. Esaminare la Directory di lavoro DSA, systemroot%\ntds e radice di eventuali dischi locali o rimovibili cercando il file dcclconeconfig.xml.|  
  
|||  
|-|-|  
|**ID evento**|**2164**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|*<COMPUTERNAME>*Impossibile avviare il servizio DsRoleSvc per clonare il controller di dominio virtuale locale.|  
|**Note e risoluzione**|Esaminare le impostazioni del servizio per il servizio ruolo Server DS (DsRoleSvc) e verificare che il tipo di avvio sia impostato su manuale. Verificare che nessun programma di terze parti impedisca l'avvio del servizio.|  
  
|||  
|-|-|  
|**ID evento**|**2165**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|*<COMPUTERNAME>*Impossibile avviare un thread durante la clonazione del controller di dominio virtuale locale.<br /><br />Codice di errore: % 1<br /><br />Messaggio di errore: % 2<br /><br />Nome thread: % 3|  
|**Note e risoluzione**|Contattare il supporto tecnico Microsoft|  
  
|||  
|-|-|  
|**ID evento**|**2166**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|*<COMPUTERNAME>*è necessario il servizio RPCSS per avviare il riavvio del sistema in modalità DSRM. In attesa di inizializzazione in uno stato di esecuzione non riuscito di RPCSS.<br /><br />Codice di errore: % 1|  
|**Note e risoluzione**|Esaminare il registro eventi di sistema e le impostazioni del servizio per il servizio Server RPC (Rpcss)|  
  
|||  
|-|-|  
|**ID evento**|**2167**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|*<COMPUTERNAME>*Impossibile inizializzare informazioni del controller di dominio virtuale. Vedere voce del registro eventi precedenti per informazioni dettagliate.<br /><br />Dati aggiuntivi<br /><br />Codice di errore: % 1|  
|**Note e risoluzione**|Seguire le istruzioni del messaggi, questo errore è generico.|  
  
|||  
|-|-|  
|**ID evento**|**2168**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|Microsoft-Windows-ActiveDirectory DomainService<br /><br />Il controller di dominio è in esecuzione in un hypervisor supportato. ID di generazione VM viene rilevato.<br /><br />Valore corrente dell'ID di generazione VM: %1|  
|**Note e risoluzione**|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|**ID evento**|**2169**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|Non esiste alcun ID di generazione VM rilevato. Il controller di dominio è ospitato in un computer fisico, una versione di livello inferiore di Hyper-V o un hypervisor che non supporta l'ID di generazione VM<br /><br />Dati aggiuntivi<br /><br />Codice di errore restituito alla verifica di generazione della macchina virtuale ID: % 1|  
|**Note e risoluzione**|Si tratta di un evento riuscito se non si intende clonare. In caso contrario, esaminare il registro eventi di sistema ed esaminare la documentazione di supporto prodotto hypervisor.|  
  
|||  
|-|-|  
|**ID evento**|**2170**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Avviso|  
|**Messaggio**|È stata rilevata una modifica dell'ID di generazione.<br /><br />ID generazione memorizzato nella cache in DS (valore precedente): %1<br /><br />ID generazione attualmente nella macchina virtuale (nuovo valore): %2<br /><br />La modifica dell'ID di generazione si verifica dopo l'applicazione di uno snapshot della macchina virtuale, dopo un'operazione di importazione della macchina virtuale o dopo un'operazione live migration. *<COMPUTERNAME>*verrà creato un nuovo ID di chiamata per ripristinare il controller di dominio. Controller di dominio virtualizzati non devono essere ripristinati mediante snapshot della macchina virtuale. Il metodo supportato per il ripristino o il rollback del contenuto di un database di servizi di dominio Active Directory consiste nel ripristinare un backup dello stato del sistema creato con un'applicazione di backup compatibile con servizi di dominio Active Directory.|  
|**Note e risoluzione**|Si tratta di un evento riuscito se si intende clonare. In caso contrario, esaminare il registro eventi di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2171**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|Non è stata rilevata nessuna modifica all'ID di generazione.<br /><br />ID generazione memorizzato nella cache in DS (valore precedente): %1<br /><br />ID generazione attualmente nella macchina virtuale (nuovo valore): %2|  
|**Note e risoluzione**|Si tratta di un evento riuscito se non si intende clonare e dovrebbe essere visualizzato a ogni riavvio di un controller di dominio virtualizzati. In caso contrario, esaminare il registro eventi di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2172**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|Leggere l'attributo msDS-GenerationId dell'oggetto computer del Controller di dominio.<br /><br />valore attributo msDS-GenerationId: % 1|  
|**Note e risoluzione**|Si tratta di un evento riuscito se si intende clonare. In caso contrario, esaminare il registro eventi di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2173**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|Non è riuscito a leggere l'attributo msDS-GenerationId dell'oggetto computer del Controller di dominio. Questo può essere causato da errori delle transazioni di database o l'id di generazione non esiste nel database locale. L'attributo msDS-GenerationId non esiste durante il primo riavvio dopo l'esecuzione di dcpromo oppure il controller di dominio non è un controller di dominio virtuale.<br /><br />Dati aggiuntivi<br /><br />Codice di errore: % 1|  
|**Note e risoluzione**|Si tratta di un evento riuscito se si intende clonare ed è il primo riavvio della VM dopo la clonazione è stata completata. Inoltre, può essere ignorato in controller di dominio non virtuali. In caso contrario, esaminare il registro eventi di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2174**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|Il controller di dominio è un clone di controller di dominio virtuale né uno snapshot di controller di dominio virtuale ripristinato.|  
|**Note e risoluzione**|Si tratta di un evento riuscito se non si intende clonare. In caso contrario, esaminare il registro eventi di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2175**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|File di configurazione clone di controller di dominio virtuale esistente in una piattaforma non supportata.|  
|**Note e risoluzione**|Ciò si verifica quando viene trovato un file dccloneconfig.xml, ma un ID di generazione VM non è possibile trovare, ad esempio quando un file dccloneconfig.xml si trova in un computer fisico o in un hypervisor che non supporta la generazione di VM-ID.|  
  
|||  
|-|-|  
|**ID evento**|**2176**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|Rinominare i file di configurazione clone di controller di dominio virtuale.<br /><br />Dati aggiuntivi<br /><br />Vecchio file nome: % 1<br /><br />Nuovo nome file: % 2|  
|**Note e risoluzione**|Ridenominazione è prevista quando si avvia una macchina virtuale di origine eseguire il backup, perché l'ID di generazione VM non è cambiato. Ciò impedisce che il controller di dominio di origine tentare una clonazione.|  
  
|||  
|-|-|  
|**ID evento**|**2177**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|Ridenominazione di file di configurazione clone di controller di dominio virtuale non è riuscito.<br /><br />Dati aggiuntivi<br /><br />Nome del file: % 1<br /><br />Codice di errore: % 2 %3|  
|**Note e risoluzione**|Previsto quando si avvia una macchina virtuale di origine il backup, perché l'ID di generazione VM non è cambiato il tentativo di ridenominazione. Ciò impedisce che il controller di dominio di origine tentare una clonazione. Rinominare il file e analizzare i prodotti di terze parti installati possono impedire la ridenominazione di file manualmente.|  
  
|||  
|-|-|  
|**ID evento**|**2178**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|Rilevato file di configurazione clone di controller di dominio virtuale, ma l'ID di generazione VM non è stato modificato. Il controller di dominio locale è il controller di dominio di origine clone. Rinominare il file di configurazione clone.|  
|**Note e risoluzione**|Previsto quando si avvia una macchina virtuale di origine il backup, perché l'ID di generazione VM non è cambiato. Ciò impedisce che il controller di dominio di origine tentare una clonazione.|  
  
|||  
|-|-|  
|**ID evento**|**2179**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|L'attributo msDS-GenerationId dell'oggetto computer del Controller di dominio è stata impostata per il parametro seguente:<br /><br />Attributo GenerationID: % 1|  
|**Note e risoluzione**|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|**ID evento**|**2180**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Avviso|  
|**Messaggio**|Non è riuscito a impostare l'attributo msDS-GenerationId dell'oggetto computer del Controller di dominio.<br /><br />Dati aggiuntivi<br /><br />Codice di errore: % 1|  
|**Note e risoluzione**|Esaminare il registro eventi di sistema e Dcpromo.log. Cercare l'errore specifico in MS TechNet, Knowledgebase MS e nei blog di Microsoft per determinarne il significato solito e quindi risolvere il problema in base a questi risultati.|  
  
|||  
|-|-|  
|**ID evento**|**2182**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|Evento interno: il servizio Directory è stato richiesto di clonare un DSA remoto:|  
|**Note e risoluzione**|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|**ID evento**|**2183**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|Evento interno: * <COMPUTERNAME> * ha completato la richiesta di clonare l'oggetto Directory System Agent remoto.<br /><br />Controller di dominio nome originale: % 3<br /><br />Nome del controller di dominio: % 4 clone richiesto<br /><br />Richiedere clone di controller di dominio del sito: % 5<br /><br />Dati aggiuntivi<br /><br />Valore errore: % 1 %2|  
|**Note e risoluzione**|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|**ID evento**|**2184**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|*<COMPUTERNAME>*non è riuscito a creare un account di controller di dominio per il controller di dominio clonato.<br /><br />Controller di dominio nome originale: % 1<br /><br />Numero consentito di clonato DC: % 2<br /><br />Il limite al numero di account controller di dominio che è possibile generare clonando * <COMPUTERNAME> *è stato superato.|  
|**Note e risoluzione**|Un nome di controller di dominio di origine singolo generare automaticamente solo 9999 volte se i controller di dominio non sono abbassati di livello, in base alla convenzione di denominazione. Utilizzare il <computername> elemento nel codice XML per generare un nuovo nome univoco o effettuare la clonazione da un controller di dominio denominato in modo diverso.|  
  
|||  
|-|-|  
|**ID evento**|**2191**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>*impostare il valore del Registro di sistema seguente per disabilitare gli aggiornamenti DNS.<br /><br />Chiave del Registro di sistema: % 1<br /><br />Il valore del Registro di sistema: %2<br /><br />Dati del valore del Registro di sistema: %3<br /><br />Durante il processo di clonazione, il computer locale può avere lo stesso nome di computer del computer di origine clone per un breve periodo di tempo. DNS A e la registrazione di record AAAA vengono disabilitate durante questo periodo, in modo che i client non possono inviare richieste al computer locale sottoposto a clonazione. Il processo di clonazione verrà attivati gli aggiornamenti DNS nuovamente dopo la clonazione vengono completati.|  
|**Note e risoluzione**|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|**ID evento**|**2192**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|*<COMPUTERNAME>*Impossibile impostare il valore del Registro di sistema seguente per disabilitare gli aggiornamenti DNS.<br /><br />Chiave del Registro di sistema: % 1<br /><br />Il valore del Registro di sistema: %2<br /><br />Dati del valore del Registro di sistema: %3<br /><br />Codice di errore: % 4<br /><br />Messaggio di errore: % 5<br /><br />Durante il processo di clonazione, il computer locale può avere lo stesso nome di computer del computer di origine clone per un breve periodo di tempo. DNS A e la registrazione di record AAAA vengono disabilitate durante questo periodo, in modo che i client non possono inviare richieste al computer locale sottoposto a clonazione.|  
|**Note e risoluzione**|Esaminare i registri eventi dell'applicazione e sistema. Analizzare l'applicazione di terze parti che potrebbe bloccare gli aggiornamenti del Registro di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2193**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>*impostare il valore del Registro di sistema seguente per abilitare gli aggiornamenti DNS.<br /><br />Chiave del Registro di sistema: % 1<br /><br />Il valore del Registro di sistema: %2<br /><br />Dati del valore del Registro di sistema: %3<br /><br />Durante il processo di clonazione, il computer locale può avere lo stesso nome di computer del computer di origine clone per un breve periodo di tempo. DNS A e la registrazione di record AAAA vengono disabilitate durante questo periodo, in modo che i client non possono inviare richieste al computer locale sottoposto a clonazione.|  
|**Note e risoluzione**|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|**ID evento**|**2194**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|*<COMPUTERNAME>*Impossibile impostare il valore del Registro di sistema seguente per abilitare gli aggiornamenti DNS.<br /><br />Chiave del Registro di sistema: % 1<br /><br />Il valore del Registro di sistema: %2<br /><br />Dati del valore del Registro di sistema: %3<br /><br />Codice di errore: % 4<br /><br />Messaggio di errore: % 5<br /><br />Durante il processo di clonazione, il computer locale può avere lo stesso nome di computer del computer di origine clone per un breve periodo di tempo. DNS A e la registrazione di record AAAA vengono disabilitate durante questo periodo, in modo che i client non possono inviare richieste al computer locale sottoposto a clonazione.|  
|**Note e risoluzione**|Esaminare i registri eventi dell'applicazione e sistema. Analizzare l'applicazione di terze parti che potrebbe bloccare gli aggiornamenti del Registro di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2195**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|Impossibile impostare l'avvio in modalità DSRM.<br /><br />Codice di errore: % 1<br /><br />Messaggio di errore: % 2<br /><br />Quando la clonazione di controller di dominio virtuale non riuscita o file di configurazione clone di controller di dominio virtuale viene visualizzato in un hypervisor non supportato, il computer locale viene riavviato in modalità DSRM per la risoluzione dei problemi. Avvio in modalità DSRM impostazione non è riuscito.|  
|**Note e risoluzione**|Esaminare i registri eventi dell'applicazione e sistema. Analizzare l'applicazione di terze parti che potrebbe bloccare gli aggiornamenti del Registro di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2196**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|Impossibile abilitare il privilegio di arresto.<br /><br />Codice di errore: % 1<br /><br />Messaggio di errore: % 2<br /><br />Quando la clonazione di controller di dominio virtuale non riuscita o file di configurazione clone di controller di dominio virtuale viene visualizzato in un hypervisor non supportato, il computer locale viene riavviato in modalità DSRM per la risoluzione dei problemi. Impossibile abilitare il privilegio di arresto.|  
|**Note e risoluzione**|Esaminare i registri eventi dell'applicazione e sistema. Analizzare l'applicazione di terze parti che potrebbe bloccare l'utilizzo di privilegi.|  
  
|||  
|-|-|  
|**ID evento**|**2197**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|Impossibile avviare l'arresto del sistema.<br /><br />Codice di errore: % 1<br /><br />Messaggio di errore: % 2<br /><br />Quando la clonazione di controller di dominio virtuale non riuscita o file di configurazione clone di controller di dominio virtuale viene visualizzato in un hypervisor non supportato, il computer locale viene riavviato in modalità DSRM per la risoluzione dei problemi. Arresto del sistema Inizializzazione non riuscita.|  
|**Note e risoluzione**|Esaminare i registri eventi dell'applicazione e sistema. Analizzare l'applicazione di terze parti che potrebbe bloccare l'utilizzo di privilegi.|  
  
|||  
|-|-|  
|**ID evento**|**2198**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|*<COMPUTERNAME>*non è riuscito a creare o modificare il seguente oggetto controller di dominio clonato.<br /><br />Dati aggiuntivi:<br /><br />Oggetto:<br /><br />%1<br /><br />Valore di errore: %2<br /><br />%3|  
|**Note e risoluzione**|Cercare l'errore specifico in MS TechNet, Knowledgebase MS e nei blog di Microsoft per determinarne il significato solito e quindi risolvere il problema in base a questi risultati.|  
  
|||  
|-|-|  
|**ID evento**|**2199**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|*<COMPUTERNAME>*Impossibile creare il seguente oggetto controller di dominio clonato perché l'oggetto esiste già.<br /><br />Dati aggiuntivi:<br /><br />Controller di dominio di origine:<br /><br />%1<br /><br />Oggetto:<br /><br />%2|  
|**Note e risoluzione**|Convalidare il file dccloneconfig.xml non è stato specificato un controller di dominio esistente o che le copie del file dccloneconfig.xml sono state utilizzate in più cloni senza modificare il nome. Se la collisione è comunque imprevista, determinare quale amministratore alzato di livello. contattarlo per discutere se deve essere abbassato di livello il controller di dominio esistente, pulire i metadati di controller di dominio esistente o se il clone deve utilizzare un nome diverso.|  
  
|||  
|-|-|  
|**ID evento**|**2203**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|Ultimo controller di dominio virtuale clonazione non riuscita. Questo è il primo riavvio dopo quindi pertanto deve essere un nuovo tentativo di eseguire la clonazione. Tuttavia, nessuna delle due file di configurazione clone di controller di dominio virtuale esiste né modifica dell'ID di generazione macchina virtuale viene rilevato. Eseguire l'avvio in modalità ripristino servizi directory.<br /><br />Ultimo controller di dominio virtuale clonazione non riuscita: %1<br /><br />File di configurazione clone di controller di dominio virtuale esistente: %2<br /><br />Modifica dell'ID di generazione macchina virtuale viene rilevata: %3|  
|**Note e risoluzione**|Previsto se la clonazione non riuscita in precedenza, a causa di file dccloneconfig.xml mancante o non valido|  
  
|||  
|-|-|  
|ID evento|2210|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Errore|  
|Messaggio|<COMPUTERNAME> non è riuscito a creare oggetti per i controller di dominio clone.<br /><br />Dati aggiuntivi:<br /><br />Id clone: %6<br /><br />Nome controller di dominio clone: %1<br /><br />Ciclo ripetizione: %2<br /><br />Valore eccezione: %3<br /><br />Valore di errore: %4<br /><br />DSID: %5|  
|Note e risoluzione|Esaminare i registri eventi di sistema e dei servizi Directory e il log dcpromo.log per altri dettagli sul motivo per cui la clonazione non riuscita.|  
  
|||  
|-|-|  
|ID evento|2211|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Informativo|  
|Messaggio|<COMPUTERNAME> ha creato gli oggetti per i controller di dominio clone.<br /><br />Dati aggiuntivi:<br /><br />Id clone: %3<br /><br />Nome controller di dominio clone: %1<br /><br />Ciclo ripetizione: %2|  
|Note e risoluzione|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2212|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Informativo|  
|Messaggio|<COMPUTERNAME> avviata Creazione degli oggetti per il controller di dominio clone.<br /><br />Dati aggiuntivi:<br /><br />Id clone: %1<br /><br />Nome clone: %2<br /><br />Sito clone: %3<br /><br />Clonazione di controller di dominio: %4|  
|Note e risoluzione|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2213|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Informativo|  
|Messaggio|<COMPUTERNAME> un nuovo oggetto KrbTgt creato per la clonazione di controller di dominio di sola lettura.<br /><br />Dati aggiuntivi:<br /><br />Id clone: %1<br /><br />Guid nuovo oggetto KrbTgt: %2|  
|Note e risoluzione|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2214|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Informativo|  
|Messaggio|<COMPUTERNAME> verrà creato un oggetto computer per il controller di dominio clone.<br /><br />Dati aggiuntivi:<br /><br />Id clone: %1<br /><br />Controller di dominio originale: %2<br /><br />Controller di dominio clone: %3|  
|Note e risoluzione|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2215|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Informativo|  
|Messaggio|<COMPUTERNAME> aggiungerà il controller di dominio clone nel sito seguente.<br /><br />Dati aggiuntivi:<br /><br />Id clone: %1<br /><br />Sito: %2|  
|Note e risoluzione|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2216|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Informativo|  
|Messaggio|<COMPUTERNAME> verrà creato un contenitore di server per il controller di dominio clone.<br /><br />Dati aggiuntivi:<br /><br />Id clone: %1<br /><br />Contenitore server: %2|  
|Note e risoluzione|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2217|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Informativo|  
|Messaggio|<COMPUTERNAME> verrà creato un oggetto server per il controller di dominio clone.<br /><br />Dati aggiuntivi:<br /><br />Id clone: %1<br /><br />Oggetto server: %2|  
|Note e risoluzione|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2218|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Informativo|  
|Messaggio|<COMPUTERNAME> verrà creato un oggetto impostazioni NTDS per il controller di dominio clone.<br /><br />Dati aggiuntivi:<br /><br />Id clone: %1<br /><br />Oggetto: %2|  
|Note e risoluzione|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2219|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Informativo|  
|Messaggio|<COMPUTERNAME> : verranno creati oggetti connessione per il controller di dominio di sola lettura clone.<br /><br />Dati aggiuntivi:<br /><br />Id clone: %1|  
|Note e risoluzione|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2220|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Informativo|  
|Messaggio|<COMPUTERNAME> : verranno creati oggetti SYSVOL per il controller di dominio di sola lettura clone.<br /><br />Dati aggiuntivi:<br /><br />Id clone: %1|  
|Note e risoluzione|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2221|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Errore|  
|Messaggio|<COMPUTERNAME> non è riuscito a generare una password casuale per il controller di dominio clonato.<br /><br />Dati aggiuntivi:<br /><br />Id clone: %1<br /><br />Nome controller di dominio clone: %2<br /><br />Errore: %3 %4|  
|Note e risoluzione|Esaminare il registro eventi di sistema per altri dettagli sul motivo per cui non è possibile creare la password dell'account computer.|  
  
|||  
|-|-|  
|ID evento|2222|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Errore|  
|Messaggio|<COMPUTERNAME> non è riuscito a impostare una password per il controller di dominio clonato.<br /><br />Dati aggiuntivi:<br /><br />Id clone: %1<br /><br />Nome controller di dominio clone: %2<br /><br />Errore: %3 %4|  
|Note e risoluzione|Esaminare il registro eventi di sistema per altri dettagli sul motivo per cui non è possibile impostare la password dell'account computer.|  
  
|||  
|-|-|  
|ID evento|2223|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Informativo|  
|Messaggio|<COMPUTERNAME> impostare correttamente password dell'account computer per il controller di dominio clonato.<br /><br />Dati aggiuntivi:<br /><br />Id clone: %1<br /><br />Nome controller di dominio clone: %2<br /><br />Totale tentativi: %3|  
|Note e risoluzione|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2224|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale non è riuscito. Account del servizio gestito presenti nel computer clonato il %1 seguenti:<br /><br />%2<br /><br />Per la clonazione riesca, è necessario rimuovere tutti gli account del servizio gestito. Questa operazione può essere eseguita utilizzando il cmdlet Remove-Adserviceaccount PowerShell.|  
|Note e risoluzione|Previsto quando si usano MSAs autonomo (non MSA del gruppo). Eseguire *non* seguire il Consiglio dell'evento per rimuovere l'account, è stato scritto correttamente. Use Uninstall-AdServiceAccount - [https://technet.microsoft.com/library/hh852310](https://technet.microsoft.com/library/hh852310).<br /><br />Servizio gestito autonomi e - primo rilasciate in Windows Server 2008 R2 - sono stati sostituiti con in Windows Server 2012 MSAs gruppo (gMSA). Questi account supportano la clonazione.|  
  
|||  
|-|-|  
|ID evento|2225|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Informativo|  
|Messaggio|I segreti memorizzati nella cache dell'identità di protezione seguenti sono stati rimossi dal controller di dominio locale:<br /><br />%1<br /><br />Dopo la clonazione del controller di dominio di sola lettura, i segreti precedentemente memorizzati cache nel controller di dominio di sola lettura origine clonazione verranno rimossi dal controller di dominio clonato.|  
|Note e risoluzione|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|2226|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Errore|  
|Messaggio|Non è riuscito a rimuovere i segreti memorizzati nella cache dell'identità di protezione seguenti dal controller di dominio locale:<br /><br />%1<br /><br />Errore: %2 (%3)<br /><br />Dopo la clonazione del controller di dominio di sola lettura, i segreti precedentemente memorizzati cache sulla necessità di controller di dominio di sola lettura origine clonazione di essere rimossi nel clone per ridurre il rischio che un utente malintenzionato può ottenere le credenziali dal clone rubato o compromesso. Se l'entità di sicurezza è un account con privilegiato elevati e deve essere protetto da questa, utilizzare l'operazione rootDSE rODCPurgeAccount per cancellare manualmente i segreti nel controller di dominio locale.|  
|Note e risoluzione|Esaminare i registri eventi di sistema e dei servizi Directory per ulteriori informazioni.|  
  
|||  
|-|-|  
|ID evento|2227|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Errore|  
|Messaggio|Eccezione durante il tentativo di rimuovere i segreti memorizzati nella cache dal controller di dominio locale.<br /><br />Dati aggiuntivi:<br /><br />Valore eccezione: %1<br /><br />Valore di errore: %2<br /><br />DSID: %3<br /><br />Dopo la clonazione del controller di dominio di sola lettura, i segreti precedentemente memorizzati cache sulla necessità di controller di dominio di sola lettura origine clonazione di essere rimossi nel clone per ridurre il rischio che un utente malintenzionato può ottenere le credenziali dal clone rubato o compromesso. Se qualsiasi entità di sicurezza è un account con privilegiato elevati e deve essere protetto da questa, utilizzare l'operazione rootDSE rODCPurgeAccount per cancellare manualmente i segreti nel controller di dominio locale.|  
|Note e risoluzione|Esaminare i registri eventi di sistema e dei servizi Directory per ulteriori informazioni.|  
  
|||  
|-|-|  
|ID evento|2228|  
|Origine|Microsoft-Windows-ActiveDirectory DomainService|  
|Livello di gravità|Errore|  
|Messaggio|L'ID di generazione macchina virtuale nel database di Active Directory di questo controller di dominio è diverso dal valore corrente della macchina virtuale. Tuttavia, un file di configurazione clone di controller dominio virtuale (DCCloneConfig.xml) non è possibile individuare in modo clonazione del controller di dominio non è stata tentata. Se è prevista l'operazione di clonazione di un controller di dominio, assicurarsi che viene fornito un file DCCloneConfig.xml in uno qualsiasi dei percorsi supportati. Inoltre, l'indirizzo IP del controller di dominio in conflitto con l'indirizzo IP del controller di dominio un altro. Per garantire senza interruzioni del servizio, il controller di dominio è stato configurato per l'avvio in modalità ripristino servizi directory.<br /><br />Dati aggiuntivi:<br /><br />L'indirizzo IP duplicato: %1|  
|Note e risoluzione|Questo meccanismo di protezione Arresta i controller di dominio duplicati quando possibile (tranne non quando si utilizza DHCP, ad esempio). Aggiungere un file DcCloneConfig.xml valido, rimuovere il flag DSRM e provare a rieseguire la clonazione|  
  
|||  
|-|-|  
|ID evento|29218|  
|Origine|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Livello di gravità|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale non è riuscito. Impossibile completare l'operazione di clonazione e il controller di dominio clonato è stato riavviato nel ripristino servizi Directory modalità ().<br /><br />Facci controllo registrato in precedenza gli eventi e %systemroot%\debug\dcpromo.log per ulteriori informazioni sugli errori corrispondenti al controller di dominio virtuale, la clonazione tentativo e o meno l'immagine del clone può essere riutilizzato.<br /><br />Se una o più voci di registro indicano che non è possibile ritentare il processo di clonazione, l'immagine deve essere eliminata in modo sicuro. In caso contrario potrebbe correggere gli errori, cancellare il flag di avvio DSRM e riavviare normalmente; al riavvio del sistema verrà ripetuta l'operazione di clonazione.|  
|Note e risoluzione|Esaminare i registri eventi di sistema e dei servizi Directory e il log dcpromo.log per altri dettagli sul motivo per cui la clonazione non riuscita.|  
  
|||  
|-|-|  
|ID evento|29219|  
|Origine|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Livello di gravità|Informativo|  
|Messaggio|Clonazione del controller di dominio virtuale completata.|  
|Note e risoluzione|Si tratta di un evento riuscito e un problema solo se imprevisto.|  
  
|||  
|-|-|  
|ID evento|29248|  
|Origine|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Livello di gravità|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale non è riuscito a ottenere la notifica di Winlogon. Il codice di errore restituito è %1 (%2).<br /><br />Per ulteriori informazioni su questo errore, vedere %systemroot%\Debug\Dcpromo.log. per errori corrispondenti al tentativo di clonazione di controller di dominio virtuale.|  
|Note e risoluzione|Contattare il supporto tecnico Microsoft|  
  
|||  
|-|-|  
|ID evento|29249|  
|Origine|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Livello di gravità|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale non è riuscito analizzare i file di configurazione di controller di dominio virtuale.<br /><br />Codice HRESULT restituito: %1.<br /><br />File di configurazione: %2<br /><br />Correggere gli errori nel file di configurazione e ritentare l'operazione di clonazione.<br /><br />Per ulteriori informazioni su questo errore, vedere systemroot%\debug\dcpromo.log.|  
|Note e risoluzione|Esaminare il file dclconeconfig.xml per gli errori di sintassi usando un editor XML e il tipo di schema dccloneconfigschema.xsd.|  
  
|||  
|-|-|  
|ID evento|29250|  
|Origine|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Livello di gravità|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale non è riuscito. Sono disponibili software o i servizi attualmente abilitati nel controller di dominio virtuale clonato non presentano nell'elenco delle applicazioni consentite per la clonazione di controller di dominio virtuale.<br /><br />Di seguito sono voci mancanti:<br /><br />%2<br /><br />l'utilizzo di %1 (se presente) 1 come elenco di inclusione definito.<br /><br />Impossibile completare l'operazione di clonazione se sono installate applicazioni non consentite.<br /><br />Eseguire Active Directory PowerShell Cmdlet Get-ADDCCloningExcludedApplicationList per verificare quali applicazioni sono installate nel computer clonato, ma non inclusi nell'elenco Consenti e aggiungerli all'elenco Consenti se sono compatibili con la clonazione di controller di dominio virtuale. Se una di queste applicazioni non sono compatibile con la clonazione di controller di dominio virtuali, disinstallarle prima di ritentare l'operazione di clonazione.<br /><br />Processo di clonazione di controller di dominio virtuale Cerca il file elenco applicazioni consentite, CustomDCCloneAllowList.xml, in base all'ordine di ricerca seguenti; viene utilizzato il primo file trovato e tutti gli altri vengono ignorati:<br /><br />1. il nome del valore del Registro di sistema: HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters\AllowListFolder<br /><br />2. nella stessa directory in cui risiede la cartella Directory di lavoro DSA<br /><br />3. %windir%\NTDS<br /><br />4. supporto di rimovibile di lettura/scrittura in ordine di lettera di unità alla radice dell'unità|  
|Note e risoluzione|Seguire le istruzioni del messaggio|  
  
|||  
|-|-|  
|ID evento|29251|  
|Origine|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Livello di gravità|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale non è riuscito a reimpostare gli indirizzi IP del computer clonato.<br /><br />Il codice di errore restituito è %1 (%2).<br /><br />Questo errore potrebbe essere causato da errori di configurazione nelle sezioni di configurazione di rete nel file di configurazione del controller di dominio virtuale.<br /><br />Vedere %systemroot%\debug\dcpromo.log per ulteriori informazioni sugli errori corrispondenti agli indirizzi IP reimpostazione durante i tentativi di clonazione del controller di dominio virtuale.<br /><br />Details on resetting machine IP addresses on the cloned machine can be found at https://go.microsoft.com/fwlink/?LinkId=208030|  
|Note e risoluzione|Verificare le informazioni IP impostate in dccloneconfig.xml sia valide e non duplichino il computer di origine originale.|  
  
|||  
|-|-|  
|ID evento|29253|  
|Origine|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Livello di gravità|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale non è riuscito. Il controller di dominio clone è stato in grado di individuare il master operazioni di dominio primario (PDC) controller nel dominio home del computer clonato del computer clonato.<br /><br />Il codice di errore restituito è %1 (%2).<br /><br />Verificare che il controller di dominio primario nel dominio home del computer clonato viene assegnato a un controller di dominio, sia online e operativo. Verificare che il computer clonato disponga della connettività LDAP/RPC al controller di dominio primario tramite le porte necessarie e i protocolli.|  
|Note e risoluzione|Convalidare l'indirizzo IP del controller di dominio clonato e le informazioni DNS sono impostate. Usare Dcdiag.exe /test: locatorcheck per verificare se l'emulatore PDC è online e usare Nltest.exe /server:* <PDCE> * /dclist:* <domain> * per verificare il RPC, ottenere un'acquisizione di rete dall'emulatore PDC durante l'errore di clonazione e analizzare il traffico.|  
  
|||  
|-|-|  
|ID evento|29254|  
|Origine|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Livello di gravità|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale non è riuscito a eseguire il binding al controller di dominio primario %1.<br /><br />Il codice di errore restituito è %2 (%3).<br /><br />Verificare che il controller di dominio primario %1 sia online e operativo. Verificare che il computer clonato disponga della connettività LDAP/RPC al controller di dominio primario tramite le porte necessarie e i protocolli.|  
|Note e risoluzione|Convalidare l'indirizzo IP del controller di dominio clonato e le informazioni DNS sono impostate. Usare Dcdiag.exe /test: locatorcheck per verificare se l'emulatore PDC è online e usare Nltest.exe /server:* <PDCE> * /dclist:* <domain> * per verificare il RPC, ottenere un'acquisizione di rete dall'emulatore PDC durante l'errore di clonazione e analizzare il traffico.|  
  
|||  
|-|-|  
|ID evento|29255|  
|Origine|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Livello di gravità|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale non è riuscito.<br /><br />Un tentativo di creare oggetti nel controller di dominio primario %1 necessari per l'immagine da clonare è stato restituito l'errore %2 (%3).<br /><br />Verificare che il controller di dominio clonato disponga dei privilegi per eseguire l'autoclonazione. Controllare gli eventi correlati nel registro eventi del servizio di Directory nel controller di dominio primario %1.|  
|Note e risoluzione|Cercare l'errore specifico in MS TechNet, Knowledgebase MS e nei blog di Microsoft per determinarne il significato solito e quindi risolvere il problema in base a questi risultati.|  
  
|||  
|-|-|  
|ID evento|29256|  
|Origine|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Livello di gravità|Errore|  
|Messaggio|Un tentativo di impostare l'avvio in flag della modalità ripristino servizi Directory non riuscito con codice di errore %1.<br /><br />Vedere %systemroot%\debug\dcpromo.log per ulteriori informazioni sugli errori.|  
|Note e risoluzione|Esaminare il Registro di servizi di Directory e dcpromo.log per i dettagli. Esaminare i registri eventi dell'applicazione e sistema. Analizzare l'applicazione di terze parti che potrebbe bloccare l'utilizzo di privilegi.|  
  
|||  
|-|-|  
|ID evento|29257|  
|Origine|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Livello di gravità|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale completata. Un tentativo di riavvio del computer non riuscito con codice di errore %1.<br /><br />Riavviare il computer per completare l'operazione di clonazione.|  
|Note e risoluzione|Esaminare i registri eventi dell'applicazione e sistema. Analizzare l'applicazione di terze parti che potrebbe bloccare l'utilizzo di privilegi.|  
  
|||  
|-|-|  
|ID evento|29264|  
|Origine|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Livello di gravità|Errore|  
|Messaggio|Tentativo di cancellare l'avvio in flag della modalità ripristino servizi Directory non riuscito con codice di errore %1.<br /><br />Vedere %systemroot%\debug\dcpromo.log per ulteriori informazioni sugli errori.|  
|Note e risoluzione|Esaminare il Registro di servizi di Directory e dcpromo.log per i dettagli. Esaminare i registri eventi dell'applicazione e sistema. Analizzare l'applicazione di terze parti che potrebbe bloccare l'utilizzo di privilegi.|  
  
|||  
|-|-|  
|ID evento|29265|  
|Origine|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Livello di gravità|Informativo|  
|Messaggio|Clonazione del controller di dominio virtuale completata. Il dominio virtuale controller clonazione file di configurazione %1 è stato rinominato in %2.|  
|Note e risoluzione|N/d, si tratta di un evento riuscito.|  
  
|||  
|-|-|  
|ID evento|29266|  
|Origine|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Livello di gravità|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale completata. Il tentativo di ridenominazione del dominio virtuale file clonazione del controller configurazione %1 non riuscito con codice di errore %2 (%3).|  
|Note e risoluzione|Rinominare manualmente il file dccloneconfig.xml.|  
  
|||  
|-|-|  
|ID evento|29267|  
|Origine|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Livello di gravità|Errore|  
|Messaggio|Clonazione del controller di dominio virtuale non è riuscito a controllare che la clonazione del controller di dominio virtuale elenco di applicazioni consentite.<br /><br />Il codice di errore restituito è %1 (%2).<br /><br />Questo errore potrebbe essere causato da una sintassi di errore nel clone elenco file consentite (il file attualmente controllato è: %3). Per ulteriori informazioni su questo errore, vedere systemroot%\debug\dcpromo.log.|  
|Note e risoluzione|Seguire le istruzioni dell'evento|  
  
##### <a name="error-messages"></a>Messaggi di errore  
Non sono presenti errori interattivi diretti per la clonazione del controller di dominio virtualizzati non riuscito; tutte le informazioni di clonazione nei log di sistema e servizi Directory e registra la promozione del controller di dominio in Dcpromo.log viene visualizzato. Tuttavia, se il server viene avviato in modalità ripristino servizi directory, provare a utilizzare immediatamente, come l'innalzamento di livello o clonazione.  
  
Il log dcpromo.log è il punto di controllo per la clonazione di errore. In base all'errore riportato, potrebbe essere necessario in seguito esaminare i registri di sistema per un'ulteriore diagnosi e servizi di Directory.  
  
#### <a name="known-issues-and-support-scenarios"></a>Problemi noti e scenari di supporto  
Di seguito sono indicati i problemi comuni riscontrati durante il processo di sviluppo di Windows Server 2012. Tutti questi problemi sono "da progettazione" e si dispone di una soluzione alternativa valida o tecnica più appropriata per evitarli in primo luogo. Alcuni potranno essere risolti nelle versioni successive di Windows Server 2012.  
  
|||  
|-|-|  
|**Problema**|**Clonazione non riuscita, modalità ripristino servizi directory**|  
|**Sintomi**|Il clone viene avviato in modalità ripristino servizi Directory|  
|**Risoluzione e note**|Convalidare tutti i passaggi eseguiti, riportati nelle sezioni distribuzione di Controller di dominio virtualizzato e [metodologia generale per la risoluzione dei problemi di clonazione del Controller di dominio](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)<br /><br />Descrizione nell'articolo 2742844 della Knowledge.|  
  
|||  
|-|-|  
|**Problema**|**Lease IP aggiuntivi quando si usa DHCP per clonare**|  
|**Sintomi**|Dopo la clonazione correttamente un controller di dominio e utilizza DHCP, il primo avvio del clone accetta un lease DHCP. Quindi, quando il server viene rinominato e riavviato come controller di dominio, viene eseguito un secondo lease DHCP. Il primo indirizzo IP non viene rilasciato e finire con un lease "fittizio"|  
|**Risoluzione e note**|Eliminare il lease dell'indirizzo inutilizzato in DHCP o consentirne la normale scadenza manualmente. Descrizione nell'articolo 2742836 della Knowledge.|  
  
|||  
|-|-|  
|**Problema**|**La clonazione non riesce in modalità DSRM dopo un ritardo prolungato.**|  
|**Sintomi**|La clonazione sembra sospesa "clonazione del controller di dominio è x % completamento" per tra 8 e 15 minuti. Successivamente, la clonazione non riesce e viene avviato in modalità ripristino servizi directory.|  
|**Risoluzione e note**|Il computer clonato non può ottenere un indirizzo IP dinamico da DHCP o SLAAC, Usa un indirizzo IP duplicato o non riesci a trovare il PDC. Più tentativi di ripetizione della clonazione portano al ritardo. Risolvere il problema di rete per la clonazione.<br /><br />Descrizione nell'articolo 2742844 della Knowledge.|  
  
|||  
|-|-|  
|**Problema**|**La clonazione non ricrea tutti i nomi dell'entità servizio**|  
|**Sintomi**|Se un set di *tre parti* nomi dell'entità servizio (SPN) include sia un nome NetBIOS con una porta e un nome NetBIOS altrimenti identico senza una porta, la voce senza porta non viene ricreata con il nuovo nome computer. Per esempio:<br /><br />customspn / DC1: 200 / app1 INVALID USE OF SYMBOLS *questa parte viene ricreata con il nuovo nome computer*<br /><br />customspn/DC1/app1 INVALID USE OF SYMBOLS *questo non viene ricreato con il nuovo nome computer*<br /><br />I nomi completi vengono ricreati e SPN senza tre parti vengono ricreati, indipendentemente dalle porte. Ad esempio, questi vengono ricreati correttamente nel clone:<br /><br />customspn / DC1: 202 INVALID USE OF SYMBOLS *questa parte viene ricreata*<br /><br />customspn/DC1 INVALID USE OF SYMBOLS *questa parte viene ricreata*<br /><br />customspn/DC1.corp.contoso.com:202 INVALID USE OF SYMBOLS *questa parte viene ricreata nome*<br /><br />customspn/DC1.corp.contoso.com INVALID USE OF SYMBOLS *questa parte viene ricreata*|  
|**Risoluzione e note**|Si tratta di una limitazione del processo di ridenominazione di controller dominio di Windows, non solo nella clonazione. SPN a tre parti non vengono gestiti dalla logica di ridenominazione in qualsiasi scenario. Servizi di Windows più inclusi sono interessati da questo problema, come i nomi SPN mancanti vengono ricreati in base alle esigenze. Altre applicazioni possono richiedere l'immissione manuale del nome SPN per risolvere il problema.<br /><br />Descrizione nell'articolo 2742874 della Knowledge.|  
  
|||  
|-|-|  
|**Problema**|**Errore di clonazione, viene avviato in modalità ripristino servizi directory, errori di rete generali**|  
|**Sintomi**|Il clone viene avviato in modalità ripristino servizi Directory. Ci sono errori generici di rete.|  
|Risoluzione e note|Assicurarsi che il nuovo clone non dispone di un indirizzo MAC statico duplicato dal controller di dominio di origine; assegnato è possibile vedere se una VM Usa indirizzi MAC statici eseguendo questo comando nell'host dell'hypervisor per le macchine virtuali clone e di origine:<br /><br />Get-VM - Name *test-vm* & #124; Get-VMNetworkAdapter & #124; fl *<br /><br />Modificare l'indirizzo MAC per un indirizzo statico univoco o passare all'utilizzo di indirizzi MAC dinamici.<br /><br />Descritto nell'articolo 2742844 della Knowledge|  
  
|||  
|-|-|  
|**Problema**|**Errore di clonazione, viene avviato in modalità DSRM come duplicato del controller di dominio di origine**|  
|**Sintomi**|Un nuovo clone viene avviato senza clonazione. Il file dccloneconfig.xml non viene rinominato e il server viene avviato in modalità ripristino servizi directory. Il registro eventi dei servizi Directory Mostra l'errore 2164.<br /><br />*<COMPUTERNAME>*Impossibile avviare il servizio DsRoleSvc per clonare il controller di dominio virtuale locale.|  
|**Risoluzione e note**|Esaminare le impostazioni del servizio per il servizio ruolo Server DS (DsRoleSvc) e verificare che il tipo di avvio sia impostato su manuale. Verificare che nessun programma di terze parti impedisca l'avvio del servizio.<br /><br />Per ulteriori informazioni su come recuperare questo controller di dominio secondario assicurandosi che gli aggiornamenti vengano replicati in uscita al contempo, vedere l'articolo 2742970 della Microsoft KB.|  
  
|||  
|-|-|  
|**Problema**|**Errore di clonazione, viene avviato in modalità ripristino servizi directory, l'errore 8610**|  
|**Sintomi**|Il clone viene avviato in modalità ripristino servizi Directory. Il log Dcpromo.log viene visualizzato l'errore 8610 (ossia ERROR_DS_ROLE_NOT_VERIFIED 8610 o 0x21A2)|  
|**Risoluzione e note**|Si verifica se il PDC può essere individuabile ma non ha eseguito una replica sufficiente per assumere il ruolo. Ad esempio, se la clonazione viene avviata e un altro amministratore sposta il ruolo FSMO PDCE in un nuovo controller di dominio.<br /><br />Descrizione nell'articolo 2742916 della Knowledge.|  
  
|||  
|-|-|  
|**Problema**|**Errore di clonazione, viene avviato in modalità ripristino servizi directory, errori di rete generali**|  
|**Sintomi**|Il clone viene avviato in modalità ripristino servizi Directory. Ci sono errori generici di rete.|  
|**Risoluzione e note**|Assicurarsi che il nuovo clone non dispone di un indirizzo MAC statico duplicato dal controller di dominio di origine; assegnato è possibile vedere se una VM Usa indirizzi MAC statici eseguendo questo comando nell'host Hyper-V per le macchine virtuali clone e di origine:<br /><br />Get-VM - Name *test-vm* & #124; Get-VMNetworkAdapter & #124; fl *<br /><br />Modificare l'indirizzo MAC per un indirizzo statico univoco o passare all'utilizzo di indirizzi MAC dinamici.<br /><br />Descrizione nell'articolo 2742844 della Knowledge.|  
  
|||  
|-|-|  
|**Problema**|**Errore di clonazione, viene avviato in modalità ripristino servizi directory**|  
|**Sintomi**|Il clone viene avviato in modalità ripristino servizi Directory|  
|**Risoluzione e note**|Assicurarsi che il file dccloneconfig.xml contenga la definizione dello schema (vedere sampledccloneconfig.xml, riga 2):<br /><br />**< d3c:DCCloneConfig xmlns:d3c="uri:microsoft.com:schemas:DCCloneConfig" >**<br /><br />Descritto nell'articolo 2742844 della Knowledge|  
  
|||  
|-|-|  
|Problema|**Nessun server di accesso è la registrazione degli errori disponibili in modalità ripristino servizi directory**|  
|**Sintomi**|Il clone viene avviato in modalità ripristino servizi Directory. Il tentativo di accesso e ricevere l'errore:<br /><br />**Esistono attualmente Nessun server di accesso è disponibile per soddisfare la richiesta di accesso**|  
|**Risoluzione e note**|Assicurarsi di effettuare l'accesso con l'account di amministratore modalità ripristino servizi directory e non l'account di dominio. Utilizzare la freccia sinistra e digitare un nome utente:<br /><br />**. \administrator.**<br /><br />Descritto nell'articolo 2742908 della Knowledge|  
  
|||  
|-|-|  
|**Problema**|**Non è in grado di origine clone in modalità DSRM, errore**|  
|**Sintomi**|Durante la clonazione, si verifica un errore 8437 "creare controller di dominio clone gli oggetti in PDC non riuscita" (0x20f5)|  
|**Risoluzione e note**|Nome computer duplicato è stato impostato in DCCloneConfig.xml come origine di controller di dominio o un controller di dominio esistente. Il nome del computer deve inoltre essere nel formato di nome computer NetBIOS (15 caratteri o meno, non un FQDN).<br /><br />Correggere il file dccloneconfig.xml impostando un nome valido e univoco.<br /><br />Descritto nell'articolo 2742959 della Knowledge|  
  
|||  
|-|-|  
|**Problema**|**Errore di nuovo-addccloneconfigfile "indice non compreso nell'intervallo"**|  
|**Sintomi**|Quando si esegue il cmdlet new-addccloneconfigfile, viene visualizzato l'errore:<br /><br />Indice non compreso nell'intervallo. Deve essere non negativo e minore della dimensione dell'insieme.|  
|**Risoluzione e note**|È necessario eseguire il cmdlet in una console di Windows PowerShell con privilegi elevati di amministratore. Questo errore è causato dalla mancanza di appartenenza al gruppo di amministratore locale nel computer.<br /><br />Descritto nell'articolo 2742927 della Knowledge|  
  
|||  
|-|-|  
|**Problema**|**Clonazione non riuscita, controller di dominio duplicato**|  
|**Sintomi**|Il clone viene avviato senza clonazione, duplicati controller di dominio esistente|  
|**Risoluzione e note**|Il computer è stato copiato e avviato, ma non contiene un file DcCloneConfig.xml in uno qualsiasi dei percorsi supportati e non ha un indirizzo IP duplicato con il controller di dominio di origine. Il controller di dominio deve essere rimosso correttamente per evitare la perdita di dati.<br /><br />Descritto nell'articolo 2742970 della Knowledge|  
  
|||  
|-|-|  
|**Problema**|**New-ADDCCloneConfigFile non riesce con il server non è errore operativo quando controlla se il controller di dominio di origine è un membro del gruppo di controller di dominio clonabili se non è disponibile un catalogo globale.**|  
|**Sintomi**|Quando si esegue New-ADDCCloneConfigFile per creare un file dccloneconfig.xml, viene visualizzato l'errore:<br /><br />Codice - il server non sia operativo|  
|**Risoluzione e note**|Verificare la connettività a un catalogo globale dal server in cui si esegue New-ADDCCloneConfigFile e verificare che l'appartenenza del controller di dominio di origine nel gruppo di controller di dominio clonabili sia replicata in tale catalogo globale.<br /><br />Come mezzo per scaricare la cache del localizzatore di controller di dominio per i casi in cui un catalogo globale o controller di dominio sia stato portato non in linea recente, eseguire il comando seguente:<br /><br />Codice - nltest /dsgetdc: /FORCE /GC|  
  
### <a name="advanced-troubleshooting"></a>Risoluzione avanzata dei problemi  
Questo modulo ha lo scopo di illustrare la risoluzione avanzata dei problemi con *funziona* registri come esempi, con una spiegazione dell'accaduto. Se si conosce l'aspetto di un'operazione di controller di dominio virtualizzati, maggior chiarezza nell'ambiente gli errori. Questi log vengono presentati dalla relativa origine, con l'ordine crescente di *previsto* gli eventi (anche quando sono avvisi ed errori) correlati a un controller di dominio clonato all'interno di ogni log.  
  
#### <a name="cloning-a-domain-controller"></a>La clonazione di un Controller di dominio  
In questo esempio, il controller di dominio clone Usa DHCP per ottenere un indirizzo IP, replica SYSVOL tramite il servizio FRS o DFSR (vedere il log appropriato in base alle esigenze), è un catalogo globale e utilizza un file dccloneconfig.xml vuoto.  
  
##### <a name="directory-services-event-log"></a>Registro eventi dei servizi directory  
Il log dei servizi Directory contiene la maggior parte delle informazioni operative clonazione basata su evento. L'hypervisor cambia l'ID di generazione VM e il servizio NTDS ne prende nota, quindi invalida il pool di RID e cambia l'ID di chiamata. Il nuovo ID di generazione VM viene impostato e il server di replica di Active Directory in ingresso dati. Il servizio Replica DFS viene arrestato e il relativo database che ospita SYSVOL viene eliminato, forzando una sincronizzazione non autorevole in ingresso. Il limite massimo USN viene modificato.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**2160**|ActiveDirectory DomainService|Servizi di dominio di Active Directory locale ha rilevato un file configurazione clonazione controller di dominio virtuale.<br /><br />Il file configurazione clonazione controller di dominio virtuale si trova in:<br /><br />*<path>*\DCCloneConfig.Xml<br /><br />L'esistenza del file configurazione clonazione controller di dominio virtuale indica che il controller di dominio virtuale locale è un clone di un altro controller di dominio virtuale. Servizi di dominio Active Directory verrà avviato per eseguire l'autoclonazione.|  
|**2191**|ActiveDirectory DomainService|Servizi di dominio Active Directory impostare il valore del Registro di sistema seguente per disabilitare gli aggiornamenti DNS.<br /><br />Chiave del Registro di sistema:<br /><br />SYSTEM\CurrentControlSet\Services\Netlogon\Parameters<br /><br />Valore del Registro di sistema:<br /><br />UseDynamicDns<br /><br />Dati del valore del Registro di sistema:<br /><br />0<br /><br />Durante il processo di clonazione, il computer locale può avere lo stesso nome di computer del computer di origine clone per un breve periodo di tempo. DNS A e la registrazione di record AAAA vengono disabilitate durante questo periodo, in modo che i client non possono inviare richieste al computer locale sottoposto a clonazione. Il processo di clonazione verrà attivati gli aggiornamenti DNS nuovamente dopo la clonazione vengono completati.|  
|**2191**|ActiveDirectory DomainService|Servizi di dominio Active Directory impostare il valore del Registro di sistema seguente per disabilitare gli aggiornamenti DNS.<br /><br />Chiave del Registro di sistema:<br /><br />SYSTEM\CurrentControlSet\Services\Dnscache\Parameters<br /><br />Valore del Registro di sistema:<br /><br />RegistrationEnabled<br /><br />Dati del valore del Registro di sistema:<br /><br />0<br /><br />Durante il processo di clonazione, il computer locale può avere lo stesso nome di computer del computer di origine clone per un breve periodo di tempo. DNS A e la registrazione di record AAAA vengono disabilitate durante questo periodo, in modo che i client non possono inviare richieste al computer locale sottoposto a clonazione. Il processo di clonazione verrà attivati gli aggiornamenti DNS nuovamente dopo la clonazione vengono completati.<br /><br />"Informazioni 2/7/2012 3:12:49 PM Microsoft-Windows-ActiveDirectory DomainService 2191 Internal Configuration" servizi di dominio Active Directory imposta il valore del Registro di sistema seguente per disabilitare gli aggiornamenti DNS.<br /><br />Chiave del Registro di sistema:<br /><br />SYSTEM\CurrentControlSet\Services\Tcpip\Parameters<br /><br />Valore del Registro di sistema:<br /><br />DisableDynamicUpdate<br /><br />Dati del valore del Registro di sistema:<br /><br />1<br /><br />Durante il processo di clonazione, il computer locale può avere lo stesso nome di computer del computer di origine clone per un breve periodo di tempo. DNS A e la registrazione di record AAAA vengono disabilitate durante questo periodo, in modo che i client non possono inviare richieste al computer locale sottoposto a clonazione. Il processo di clonazione verrà attivati gli aggiornamenti DNS nuovamente dopo la clonazione vengono completati.|  
|**2172**|ActiveDirectory DomainService|Leggere l'attributo msDS-GenerationId dell'oggetto computer del Controller di dominio.<br /><br />valore dell'attributo msDS-GenerationId:<br /><br />*<Number>*|  
|**2170**|ActiveDirectory DomainService|È stata rilevata una modifica dell'ID di generazione.<br /><br />ID generazione memorizzato nella cache in DS (valore precedente):<br /><br />*<Number>*<br /><br />ID generazione attualmente nella macchina virtuale (nuovo valore):<br /><br />*<Number>*<br /><br />La modifica dell'ID di generazione si verifica dopo l'applicazione di uno snapshot della macchina virtuale, dopo un'operazione di importazione della macchina virtuale o dopo un'operazione live migration. Servizi di dominio Active Directory verrà creato un nuovo ID di chiamata per ripristinare il controller di dominio. Controller di dominio virtualizzati non devono essere ripristinati mediante snapshot della macchina virtuale. Il metodo supportato per il ripristino o il rollback del contenuto di un database di servizi di dominio Active Directory consiste nel ripristinare un backup dello stato del sistema creato con un'applicazione di backup compatibile con servizi di dominio Active Directory.|  
|**1109**|ActiveDirectory DomainService|L'attributo invocationID per il server di directory è stato modificato. Il numero di sequenza di aggiornamento più alto al momento che della creazione del backup è come segue:<br /><br />Attributo InvocationID (vecchio valore):<br /><br />*<GUID>*<br /><br />Attributo InvocationID (nuovo valore):<br /><br />*<GUID>*<br /><br />Numero di sequenza di aggiornamento:<br /><br />*<Number>*<br /><br />L'attributo invocationID viene modificato quando un server di directory viene ripristinato dal supporto di backup, è configurato per ospitare una partizione di directory applicativa scrivibile, viene ripreso dopo aver applicato uno snapshot macchina virtuale, dopo un'operazione di importazione della macchina virtuale o dopo un'operazione live migration. Controller di dominio virtualizzati non devono essere ripristinati mediante snapshot della macchina virtuale. Il metodo supportato per il ripristino o il rollback del contenuto di un database di servizi di dominio Active Directory è per ripristinare un backup dello stato del sistema creato con un'applicazione di backup compatibile con Active Directory Domain Services.|  
|**1000**|ActiveDirectory DomainService|Avvio di servizi di dominio Microsoft Active Directory completato.|  
|**1394**|ActiveDirectory DomainService|Tutti i problemi che impediscono gli aggiornamenti al database di servizi di dominio Active Directory sono stati risolti. Nuovi aggiornamenti al database di servizi di dominio Active Directory in corso. Il servizio Accesso rete è stato riavviato|  
|**2163**|ActiveDirectory DomainService|È stato avviato il servizio DsRoleSvc per clonare il controller di dominio virtuale locale.|  
|**326**|NTDS ISAM|NTDSA NTDS (536): Il motore di database collegato un database (1, C:\Windows\NTDS\ntds.dit). (Tempo = 0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.016, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<br /><br />Cache salvata: 1|  
|**103**|NTDS ISAM|NTDSA NTDS (536): Il motore di database ha arrestato l'istanza (0).<br /><br />Arresto anomalo: 0<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.032, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**102**|NTDS ISAM|NTDSA NTDS (536): Il motore di database (6.02.8225.0000) sta avviando una nuova istanza (0).|  
|**105**|NTDS ISAM|NTDSA NTDS (536): Il motore di database avviato una nuova istanza (0). (Tempo = 0 secondi)<br /><br />Sequenza temporale interna: [1] 0.016, [2] 0.000, [3] 0.015, [4] 0.078, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.046, [10] 0.000, [11] 0.000.|  
|**1004**|ActiveDirectory DomainService|Servizi di dominio Active Directory è stato arrestato.|  
|**102**|NTDS ISAM|NTDSA NTDS (536): Il motore di database (6.02.8225.0000) sta avviando una nuova istanza (0).|  
|**326**|NTDS ISAM|NTDSA NTDS (536): Il motore di database collegato un database (1, C:\Windows\NTDS\ntds.dit). (Tempo = 0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.015, [3] 0.016, [4] 0.000, [5] 0.031, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<br /><br />Cache salvata: 1|  
|**105**|NTDS ISAM|NTDSA NTDS (536): Il motore di database avviato una nuova istanza (0). (Tempo = 1 secondo)<br /><br />Sequenza temporale interna: [1] 0.031, [2] 0.000, [3] 0.000, [4] 0.391, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000.|  
|**1109**|ActiveDirectory DomainService|L'attributo invocationID per il server di directory è stato modificato. Il numero di sequenza di aggiornamento più alto al momento che della creazione del backup è come segue:<br /><br />Attributo InvocationID (vecchio valore):<br /><br />*<GUID>*<br /><br />Attributo InvocationID (nuovo valore):<br /><br />*<GUID>*<br /><br />Numero di sequenza di aggiornamento:<br /><br />*<Number>*<br /><br />L'attributo invocationID viene modificato quando un server di directory viene ripristinato dal supporto di backup, è configurato per ospitare una partizione di directory applicativa scrivibile, viene ripreso dopo aver applicato uno snapshot macchina virtuale, dopo un'operazione di importazione della macchina virtuale o dopo un'operazione live migration. Controller di dominio virtualizzati non devono essere ripristinati mediante snapshot della macchina virtuale. Il metodo supportato per il ripristino o il rollback del contenuto di un database di servizi di dominio Active Directory è per ripristinare un backup dello stato del sistema creato con un'applicazione di backup compatibile con Active Directory Domain Services.|  
|**1168**|ActiveDirectory DomainService|Errore interno: si è verificato un errore di Active Directory Domain Services.<br /><br />Dati aggiuntivi<br /><br />Valore di errore (decimale):<br /><br />2<br /><br />Valore di errore (esadecimale):<br /><br />2<br /><br />ID interno:<br /><br />7011658|  
|**1110**|ActiveDirectory DomainService|Verrà ritardata innalzamento di livello controller di dominio a un catalogo globale per l'intervallo seguente.<br /><br />Intervallo (minuti):<br /><br />5<br /><br />Questo ritardo è necessario in modo che le partizioni di directory necessaria preparare prima che sia annunciato il catalogo globale. Nel Registro di sistema, è possibile specificare il numero di secondi che l'oggetto directory system agent attenderà prima di promuovere il controller di dominio locale a catalogo globale. Per ulteriori informazioni sul valore del Registro di sistema Global Catalog Delay Advertisement, vedere Resource Kit Distributed Systems Guide|  
|**103**|NTDS ISAM|NTDSA NTDS (536): Il motore di database ha arrestato l'istanza (0).<br /><br />Arresto anomalo: 0<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.047, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.016, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**1004**|ActiveDirectory DomainService|Servizi di dominio Active Directory è stato arrestato.|  
|**1539**|ActiveDirectory DomainService|Impossibile disabilitare la cache in scrittura basata sul software disco nel disco rigido seguente servizi di dominio Active Directory.<br /><br />Disco rigido:<br /><br />c:<br /><br />Dati potrebbero andare persi durante gli errori di sistema|  
|**2179**|ActiveDirectory DomainService|L'attributo msDS-GenerationId dell'oggetto computer del Controller di dominio è stata impostata per il parametro seguente:<br /><br />Attributo GenerationID:<br /><br />*<Number>*|  
|**2173**|ActiveDirectory DomainService|Non è riuscito a leggere l'attributo msDS-GenerationId dell'oggetto computer del Controller di dominio. Questo può essere causato da errori delle transazioni di database o l'id di generazione non esiste nel database locale. L'attributo msDS-GenerationId non esiste durante il primo riavvio dopo l'esecuzione di dcpromo oppure il controller di dominio non è un controller di dominio virtuale.<br /><br />Dati aggiuntivi<br /><br />Codice di errore:<br /><br />6|  
|**1000**|ActiveDirectory DomainService|Avvio di servizi di dominio Microsoft Active Directory completato, versione 6.2.8225.0|  
|**1394**|ActiveDirectory DomainService|Tutti i problemi che impediscono gli aggiornamenti al database di servizi di dominio Active Directory sono stati risolti. Nuovi aggiornamenti al database di servizi di dominio Active Directory in corso. Il servizio Accesso rete è stato riavviato.|  
|**1128**|ActiveDirectory DomainService|1128 controllo di coerenza informazioni "una connessione di replica è stata creata dal servizio directory di origine seguenti per il servizio directory locale.<br /><br />Servizio directory di origine:<br /><br />CN = NTDS Settings,*<Domain Controller DN>*<br /><br />Servizio directory locale:<br /><br />CN = NTDS Settings,*<Domain Controller DN>*<br /><br />Dati aggiuntivi<br /><br />Codice motivo:<br /><br />0x2<br /><br />ID interno punto di creazione:<br /><br />f0a025d|  
|**1999**|ActiveDirectory DomainService|Il servizio directory di origine ha ottimizzato il numero di sequenza di aggiornamento (USN) presentato dal servizio directory di destinazione. I servizi di directory di origine e di destinazione dispongano di un partner di replica comune. Il servizio directory di destinazione viene aggiornato con i partner di replica comune e il servizio directory di origine è stato installato utilizzando un backup del partner.<br /><br />ID servizio directory di destinazione:<br /><br />*<GUID> (<FQDN>)*<br /><br />ID servizio directory comune:<br /><br />*<GUID>*<br /><br />USN proprietà comune:<br /><br />*<Number>*<br /><br />Di conseguenza, il vettore di aggiornamento del servizio directory di destinazione è stato configurato con le impostazioni seguenti.<br /><br />Oggetto USN precedente:<br /><br />0<br /><br />Proprietà USN precedente:<br /><br />0<br /><br />GUID database:<br /><br />*<GUID>*<br /><br />USN oggetto:<br /><br />*<Number>*<br /><br />USN proprietà:<br /><br />*<Number>*|  
  
##### <a name="system-event-log"></a>Registro eventi di sistema  
Le indicazioni seguenti sulle operazioni di clonazione vengono registrati nel registro eventi di sistema. Quando l'hypervisor comunica al computer guest che è stato clonato o ripristinato da uno snapshot, il controller di dominio invalida immediatamente il pool di RID per evitare di duplicare le entità di sicurezza in un secondo momento. Come procedere della clonazione, visualizzati vari messaggi e operazioni previsti, prevalentemente riguardo l'avvio e arresto di servizi e alcuni errori previsti corrispondenti da questo. Al termine le note sulla registro eventi di sistema globale clonazione esito positivo.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**16654**|Directory-Services-SAM|Un pool di identificatori di account (RID) è stato invalidato. Ciò può verificarsi nei seguenti casi previsti:<br /><br />1. un controller di dominio viene ripristinato da backup.<br /><br />2. un controller di dominio in una macchina virtuale viene ripristinato da uno snapshot.<br /><br />3. un amministratore ha invalidato manualmente il pool|  
|**7036**|Gestione controllo servizi|Il servizio servizi di dominio Active Directory è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio Centro distribuzione chiavi Kerberos è in esecuzione.|  
|**3096**|Servizio Accesso rete|Impossibile trova il Controller di dominio primario per questo dominio.|  
|**7036**|Gestione controllo servizi|Il servizio di gestione account di sicurezza è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio Server è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio Accesso rete è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio servizi Web Active Directory è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio Replica DFS è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio Replica File è in esecuzione.|  
|**14533**|Microsoft-Windows-DfsSvc|Creazione di tutti gli spazi dei nomi completata.|  
|**14531**|Microsoft-Windows-DfsSvc|Server DFS completata l'inizializzazione.|  
|**7036**|Gestione controllo servizi|Il servizio DFS Namespace è in esecuzione.|  
|**7023**|Gestione controllo servizi|Il servizio Messaggistica tra siti terminato con l'errore seguente:<br /><br />Il server specificato non può eseguire l'operazione richiesta.|  
|**7036**|Gestione controllo servizi|Il servizio Messaggistica tra siti è stato interrotto.|  
|**5806**|Servizio Accesso rete|Gli aggiornamenti dinamici sono stati disabilitati manualmente nel controller di dominio.<br /><br />AZIONE UTENTE<br /><br />Riconfigurare il controller di dominio per utilizzare gli aggiornamenti dinamici o aggiungere manualmente i record DNS dal file '% SystemRoot%\System32\Config\Netlogon.dns' nel database DNS".|  
|**16651**|Directory-Services-SAM|La richiesta di un nuovo pool di identificatori di account non riuscita. L'operazione verrà ripetuta fino a quando la richiesta ha esito positivo. L'errore è<br /><br />L'operazione FSMO richiesta non è riuscito. Non è possibile contattare il proprietario FSMO corrente.|  
|**7036**|Gestione controllo servizi|Il servizio Server DNS è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio ruolo Server DS è in esecuzione.|  
|**7036**|Gestione controllo servizi|Il servizio Accesso rete è stato interrotto.|  
|**7036**|Gestione controllo servizi|Il servizio Replica File è stato interrotto.|  
|**7036**|Gestione controllo servizi|Il servizio Centro distribuzione chiavi Kerberos è stato interrotto.|  
|**7036**|Gestione controllo servizi|Il servizio Server DNS è stato interrotto.|  
|**7036**|Gestione controllo servizi|Il servizio servizi di dominio Active Directory è stato interrotto.|  
|**7036**|Gestione controllo servizi|Il servizio Accesso rete è in esecuzione.|  
|**7040**|Gestione controllo servizi|Il tipo di avvio del servizio servizi di dominio Active Directory è stato modificato da avvio automatico a disabilitato.|  
|**7036**|Gestione controllo servizi|Il servizio Accesso rete è stato interrotto.|  
|**7036**|Gestione controllo servizi|Il servizio Replica File è in esecuzione.|  
|**29219**|DirectoryServices-DSROLE-Server|Clonazione del controller di dominio virtuale completata.|  
|**29223**|DirectoryServices-DSROLE-Server|Ora il server è un Controller di dominio.|  
|**29265**|DirectoryServices-DSROLE-Server|Clonazione del controller di dominio virtuale completata. Il controller di dominio virtuale C:\Windows\NTDS\DCCloneConfig.xml file di configurazione della clonazione è stato rinominato in c:\windows\ntds\dccloneconfig.20120207-151533.Xml..|  
|**1074**|User32|Il processo C:\Windows\system32\lsass.exe (DC2) ha iniziato il riavvio del computer DC2 per conto di utente NT AUTHORITY\SYSTEM per il seguente motivo: sistema operativo: riconfigurazione (pianificata)<br /><br />Codice motivo: 0x80020004<br /><br />Tipo di arresto: riavvio<br /><br />Commento: "|  
  
##### <a name="dcpromolog"></a>DCPROMO. REGISTRO  
Il log Dcpromo.log contiene la parte effettiva di promozione della clonazione che non è descritto il registro eventi dei servizi Directory. Poiché il log non fornisce il livello di spiegazione offerto dalle voci del registro eventi, in questa sezione del modulo contiene un'ulteriore annotazione.  
  
Il processo di promozione implica che la clonazione viene avviata, il controller di dominio viene rialzato di configurazione corrente e livello usando il database di Active Directory esistente (analogamente una promozione di IFM), quindi il controller di dominio replica i delta delle modifiche in ingresso di Active Directory e SYSVOL e la clonazione è stata completata.  
  
> [!NOTE]  
> Il log è stato modificato in questo modulo per migliorarne la leggibilità rimuovendo la colonna di date.  
  
> [!NOTE]  
> Per ulteriori dettagli sul log dcpromo.log, vedere le informazioni e risoluzione dei problemi semplificata l'amministrazione Active Directory in Windows Server 2012.  
>   
> [https://go.microsoft.com/fwlink/p/?LinkId=237244](https://go.microsoft.com/fwlink/p/?LinkId=237244)  
  
-   Avviare la promozione basata su clone  
  
-   Impostare il flag della modalità ripristino servizi Directory in modo che il server non avviato di nuovo normalmente come il clone originale e causa di denominazione o collisioni di servizio Directory  
  
-   Aggiornare il registro eventi di servizi di Directory  
  
```  
15:14:01 [INFO] vDC Cloneing: Setting Boot into DSRM flag succeeded.  
15:14:01 [WARNING] Cannot get user Token for Format Message: 1725l  
15:14:01 [INFO] vDC Cloning: Created vDCCloningUpdate event.  
15:14:01 [INFO] vDC Cloning: Created vDCCloningComplete event.  
```  
  
-   Arrestare il servizio Accesso rete in modo che il controller di dominio non invii annunci  
  
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
  
-   Esaminare il file dccloneconfig.xml per le personalizzazioni specificato dall'amministratore.  
  
-   In questo caso di esempio è un file vuoto, in modo che tutte le impostazioni vengono generate automaticamente e indirizzi IP automatici vengono richiesti dalla rete.  
  
```  
15:14:02 [INFO] vDC Cloning: Clone config file C:\Windows\NTDS\DCCloneConfig.xml is considered to be a blank file (containing 0 bytes)  
15:14:02 [INFO] vDC Cloning: Parsing clone config file C:\Windows\NTDS\DCCloneConfig.xml returned HRESULT 0x0  
```  
  
-   Verificare che non siano non installati servizi o programmi che non fanno parte del file DefaultDCCloneAllowList.xml o CustomDCCloneAllowList.xml  
  
```  
15:14:02 [INFO] vDC Cloning: Checking allowed list:  
15:14:03 [INFO] vDC Cloning: Completed checking allowed list:  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Abilitare DHCP sulle schede di rete, poiché le informazioni di IP non è state specificate dall'amministratore  
  
```  
15:14:03 [INFO] vDC Cloning: Enable DHCP:  
15:14:03 [INFO] WMI Instance: Win32_NetworkAdapterConfiguration.Index=12  
15:14:03 [INFO] Method: EnableDHCP  
15:14:03 [INFO] HRESULT code: 0x0 (0)  
15:14:03 [INFO] Return Value: 0x0 (0)  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Individuare l'emulatore PDC  
  
-   Impostare il sito del clone (generato automaticamente in questo caso)  
  
-   Impostare il nome del clone (generato automaticamente in questo caso)  
  
```  
15:14:03 [INFO] vDC Cloning: Found PDC. Name: DC1.root.fabrikam.com  
15:14:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:04 [INFO] vDC Cloning: Winlogon UI Notification #1: Domain Controller cloning is at 5% completion...  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #2: Domain Controller cloning is at 10% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Site of the cloned DC: Default-First-Site-Name  
```  
  
-   Creare il nuovo oggetto computer clone  
  
-   Rinominare il clone in base al nuovo nome  
  
```  
15:14:05 [INFO] vDC Cloning: Clone DC objects are created on PDC.  
15:14:05 [INFO] Name of the cloned DC: DC2-CL0001  
15:14:05 [INFO] DsRolepSetRegStringValue on System\CurrentControlSet\Services\NTDS\Parameters\CloneMachineName to DC2-CL0001 returned 0  
15:14:05 [INFO] vDC Cloning: Save CloneMachineName in registry: 0x0 (0)  
```  
  
-   Fornire le impostazioni di promozione, in base alle regole di generazione automatica o precedente file dccloneconfig.xml  
  
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
  
-   Avviare la promozione  
  
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
  
-   Arrestare e configurare tutti i servizi di directory correlati a Active Directory (NTDS, NTFRS/DFSR, KDC, DNS)  
  
> [!NOTE]  
> In questo scenario è previsto il servizio DNS impiega troppo tempo per la chiusura, quanto vengono usate zone integrate in Active Directory che non erano più disponibili, neanche prima del servizio NTDS - vedere gli eventi DNS descritti più avanti in questa sezione del modulo.  
  
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
  
-   Forzare la sincronizzazione dell'ora di NT5DS (NTP) con un altro controller di dominio (in genere l'emulatore PDC)  
  
```  
15:15:02 [INFO] Forcing time sync  
```  
  
-   Contattare un controller di dominio che contiene l'account di controller di dominio di origine del clone  
  
-   Scaricare eventuali ticket Kerberos  
  
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
  
-   Arrestare il servizio NetLogon e impostare il tipo di avvio  
  
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
  
-   Configurare i servizi DFSR/NTFRS per l'esecuzione automatica  
  
-   Eliminare i file di database esistenti per forzare la sincronizzazione non autorevole di SYSVOL al successivo avvio del servizio  
  
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
  
-   Avviare il processo di innalzamento di livello utilizzando il file di database NTDS esistente  
  
-   Contattare il Master RID  
  
> [!NOTE]  
> Il servizio di dominio Active Directory non è in realtà installato qui, si tratta di strumentazione legacy nel log  
  
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
  
-   Modificare l'ID di chiamata esistente che erano presenti nel database del computer di origine  
  
-   Creare un nuovo oggetto impostazioni NTDS per il clone.  
  
-   Eseguire la replica nel delta dell'oggetto Active Directory dal controller di dominio partner  
  
> [!NOTE]  
> Anche se tutti gli oggetti sono elencati come replicati, si tratta solo dei metadati necessari per includere gli aggiornamenti. Tutti gli oggetti non modificati nel database NTDS clonato esistono già e non richiedono la replica, come la promozione basata su IFM.  
  
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
  
-   Completare la parte critica di dominio Active Directory della promozione  
  
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
  
-   Completare la replica in ingresso di SYSVOL  
  
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
  
-   Abilitare la registrazione del client DNS  
  
```  
15:15:18 [INFO] vDC Cloning: Set DisableDynamicUpdate reg value to 0 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set UseDynamicDns reg value to 1 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set RegistrationEnabled reg value to 1 to enable dynamic update records registration.  
```  
  
-   Eseguire i moduli SYSPREP specificati dal DefaultDCCloneAllowList.xml <SysprepInformation> elemento.  
  
```  
15:15:18 [INFO] vDC Cloning: Running sysprep providers.  
15:15:32 [INFO] vDC Cloning: Completed running sysprep providers.  
```  
  
-   La promozione della clonazione è completata  
  
-   Rimuovere il flag di avvio in modalità ripristino servizi directory in modo che il server viene avviato normalmente successivo  
  
-   Rinominare il file dccloneconfig.xml in modo che non venga letto di nuovo al successivo avvio.  
  
-   Riavviare il computer  
  
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
  
##### <a name="active-directory-web-services-event-log"></a>Registro eventi dei servizi Web Active Directory  
Durante la clonazione, NTDS. Database DIT è spesso offline per periodi prolungati. Il servizio servizi Web Active Directory registra almeno un evento a questo scopo. Al termine della clonazione, il servizio servizi Web Active Directory viene avviato, nota che ci non è ancora un certificato computer valido ancora (possono o potrebbe non essere, a seconda dell'ambiente di distribuzione di una PKI Microsoft con registrazione automatica o non) e quindi avvia l'istanza per il nuovo controller di dominio.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**1202**|Eventi di istanza di servizi Web Active Directory|Questo computer in uso ospita l'istanza di directory specificata, ma potrebbe non service servizi Web Active Directory. Servizi Web Active Directory ritenterà l'operazione periodicamente.<br /><br />Istanza di directory: NTDS<br /><br />Porta LDAP dell'istanza di directory: 389<br /><br />Porta SSL dell'istanza di directory: 636|  
|**1000**|Eventi di istanza di servizi Web Active Directory|Avvio di servizi Web Active Directory|  
|**1008**|Eventi di istanza di servizi Web Active Directory|Servizi Web Active Directory sono stati ridotti i privilegi di sicurezza|  
|**1100**|Eventi di istanza di servizi Web Active Directory|I valori specificati nel <appsettings> sezione del file di configurazione per servizi Web Active Directory sono stati caricati senza errori.|  
|**1400**|Eventi di istanza di servizi Web Active Directory|Eventi di certificati "servizi Web Active Directory Impossibile trovare un certificato del server con il nome del certificato specificato. Un certificato è necessaria per usare le connessioni SSL/TLS. Per usare le connessioni SSL/TLS, verificare che un certificato di autenticazione server valido da un'autorità di certificazione attendibile (CA) sia installato nel computer.<br /><br />Nome del certificato:*<Server FQDN>*|  
|**1100**|Eventi di istanza di servizi Web Active Directory|I valori specificati nel <appsettings> sezione del file di configurazione per servizi Web Active Directory sono stati caricati senza errori.|  
|**1200**|Eventi di istanza di servizi Web Active Directory|Servizi Web Active Directory viene servita l'istanza di directory specificata.<br /><br />Istanza di directory: NTDS<br /><br />Porta LDAP dell'istanza di directory: 389<br /><br />Porta SSL dell'istanza di directory: 636|  
  
##### <a name="dns-server-event-log"></a>Registro eventi di Server DNS  
Il servizio DNS noteranno brevi interruzioni previste durante la clonazione, come il servizio DNS è ancora in esecuzione mentre il database di Active Directory è offline. Ciò si verifica se si usa DNS integrata di Active Directory, ma non Standard primario o secondario DNS. Questi errori vengono registrati più volte. Al termine della clonazione, DNS ritorna in linea normalmente.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**4013**|Servizio di Server DNS|Il server DNS è in attesa per Active Directory Domain Services (AD DS) segnalare che è stata completata la sincronizzazione iniziale della directory. Impossibile avviare il servizio server DNS fino a quando la sincronizzazione iniziale è stata completata poiché i dati DNS importanti non potrebbero ancora essere replicati sul controller di dominio. Se gli eventi nel registro eventi di dominio Active Directory indicano che si è verificato un problema con la risoluzione dei nomi DNS, è possibile aggiungere l'indirizzo IP di un altro server DNS per il dominio all'elenco di server DNS nelle proprietà del protocollo IP del computer. Questo evento verrà registrato ogni due minuti fino a quando non di dominio Active Directory ha segnalato che la sincronizzazione iniziale è completata.|  
|**4015**|Servizio di Server DNS|Il server DNS ha rilevato un errore critico da Active Directory. Controllare che Active Directory funzioni correttamente. Le informazioni di debug di errore estesi (che possono essere vuote) sono "" ". I dati dell'evento contengono l'errore.|  
|**4000**|Servizio di Server DNS|Il server DNS è riuscito ad aprire Active Directory.  Questo server DNS è configurato per ottenere e usare informazioni dalla directory per la zona e non è in grado di caricare la zona senza di essa.  Controllare che Active Directory funzioni correttamente e ricaricare la zona. I dati dell'evento sono il codice di errore.|  
|**4013**|Servizio di Server DNS|Il server DNS è in attesa per Active Directory Domain Services (AD DS) segnalare che è stata completata la sincronizzazione iniziale della directory. Impossibile avviare il servizio server DNS fino a quando la sincronizzazione iniziale è stata completata poiché i dati DNS importanti non potrebbero ancora essere replicati sul controller di dominio. Se gli eventi nel registro eventi di dominio Active Directory indicano che si è verificato un problema con la risoluzione dei nomi DNS, è possibile aggiungere l'indirizzo IP di un altro server DNS per il dominio all'elenco di server DNS nelle proprietà del protocollo IP del computer. Questo evento verrà registrato ogni due minuti fino a quando non di dominio Active Directory ha segnalato che la sincronizzazione iniziale è completata.|  
|**2**|Servizio di Server DNS|Il server DNS è stato avviato.|  
|**4**|Servizio di Server DNS|Il server DNS ha terminato il caricamento in background delle zone. Tutte le zone sono ora disponibili per gli aggiornamenti DNS e i trasferimenti di zona, come consentito dalla rispettiva configurazione di zona.|  
  
##### <a name="file-replication-service-event-log"></a>Registro eventi del servizio Replica file  
Il servizio Replica File consente la sincronizzazione non autorevole da un partner durante la clonazione. La clonazione ottiene questo risultato eliminando i file di database NTFRS e lasciando inalterato per l'uso come dati sottoposti a pre-seeding il contenuto di SYSVOL. Sono previsti due tentativi di sincronizzazione.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**13562**|NtFrs|Segue un riepilogo degli avvisi e gli errori riscontrati dal servizio Replica File durante il polling di DC2.root.fabrikam.com per replica FRS recuperare Controller di dominio di impostare le informazioni di configurazione.<br /><br />Impossibile eseguire il binding a un Controller di dominio. Operazione verrà ripetuta nel successivo ciclo di polling|  
|**13502**|NtFrs|Arresto del servizio Replica File.|  
|**13565**|NtFrs|Servizio Replica file sta inizializzando il volume di sistema con i dati da un altro controller di dominio. Il computer DC2 può diventare un controller di dominio fino al completamento del processo. Il volume di sistema verrà quindi condiviso come SYSVOL.<br /><br />Per controllare la condivisione SYSVOL, al prompt dei comandi, digitare:<br /><br />condivisione di rete<br /><br />Quando replica File ha completato il processo di inizializzazione, verrà visualizzata la condivisione SYSVOL.<br /><br />L'inizializzazione del volume di sistema può richiedere del tempo. Il tempo è dipende la quantità di dati nel volume di sistema, la disponibilità di altri controller di dominio e l'intervallo di replica tra controller di dominio.|  
|**13501**|NtFrs|Avvio del servizio Replica File|  
|**13502**|NtFrs|Arresto del servizio Replica File.|  
|**13503**|NtFrs|Il servizio Replica File è stato arrestato.|  
|**13565**|NtFrs|Servizio Replica file sta inizializzando il volume di sistema con i dati da un altro controller di dominio. Il computer DC2 può diventare un controller di dominio fino al completamento del processo. Il volume di sistema verrà quindi condiviso come SYSVOL.<br /><br />Per controllare la condivisione SYSVOL, al prompt dei comandi, digitare:<br /><br />condivisione di rete<br /><br />Quando replica File ha completato il processo di inizializzazione, verrà visualizzata la condivisione SYSVOL.<br /><br />L'inizializzazione del volume di sistema può richiedere del tempo. Il tempo è dipende la quantità di dati nel volume di sistema, la disponibilità di altri controller di dominio e l'intervallo di replica tra controller di dominio.|  
|**13501**|NtFrs|Il servizio Replica File sta avviando.|  
|**13553**|NtFrs|Il servizio Replica File ha aggiunto questo computer al set di repliche seguenti:<br /><br />"VOLUME SISTEMA DOMINIO (CONDIVISIONE SYSVOL)"<br /><br />Informazioni relative a questo evento sono illustrate di seguito:<br /><br />Nome DNS del computer è*<Domain Controller FQDN>*<br /><br />Nome del membro dell'insieme di replica è*<Domain Controller>*<br /><br />Percorso radice dell'insieme di replica è*<path>*<br /><br />Percorso gestione temporanea di replica*<path>*<br /><br />Percorso della directory di lavoro replica è*<path>*|  
|**13520**|NtFrs|Il servizio Replica File ha spostato i file preesistenti <path>a * <path> *\ntfrs_preexisting___see_eventlog..<br /><br />Il servizio Replica File potrebbe eliminare i file in * <path> *\NtFrs_PreExisting___See_EventLog in qualsiasi momento. File possono essere salvati dall'eliminazione copiandoli fuori * <path> *\ntfrs_preexisting___see_eventlog.. Copiare i file vengono copiati in c:\windows\sysvol\domain possono verificarsi conflitti di nome di se i file esistano già su altri partner di replica.<br /><br />In alcuni casi, il servizio Replica File copia un file da * <path> *\NtFrs_PreExisting___See_EventLog a * <path> * anziché da altri partner di replica per il file.<br /><br />In qualsiasi momento è possibile recuperare spazio eliminando i file in * <path> *\ntfrs_preexisting___see_eventlog.. "|  
|**13508**|NtFrs|il servizio Replica File sta avendo problemi di abilitare la replica da * \\\\ <Domain Controller FQDN> * a * <Domain Controller> * per * <path> * utilizzando il<br /><br />DNS name *\\\\<Domain Controller FQDN>*. Continuerà a tentare.<br /><br />Di seguito sono alcuni dei motivi che è possibile visualizzare l'avviso.<br /><br />[1] il servizio Replica file non può risolvere correttamente il nome DNS * \\\\ <Domain Controller FQDN> * da questo computer.<br /><br />[2] il servizio Replica file non è in esecuzione * \\\\ <Domain Controller FQDN> *.<br /><br />[3] le informazioni sulla topologia di servizi di dominio Active Directory per la replica non è ancora stato replicato in tutti i controller di dominio.<br /><br />Una volta per ogni connessione, dopo aver risolto il problema verrà visualizzato un altro messaggio del registro eventi che indicano che la connessione è stata stabilita, verrà visualizzato questo messaggio del registro eventi.|  
|**13509**|NtFrs|Il servizio Replica File ha attivato la replica da * \\\\ <Domain Controller FQDN> * a * <Domain Controller> * per * <Path> * dopo ripetuti tentativi.|  
|**13516**|NtFrs|Il servizio Replica File non impedisce il computer * <Domain Controller> * di diventare un controller di dominio. Il volume di sistema è stato inizializzato correttamente e il servizio Accesso rete è stato notificato che il volume di sistema è ora pronto per essere condiviso come SYSVOL.<br /><br />Digitare "net share" per controllare la condivisione SYSVOL. "|  
  
##### <a name="dfs-replication-event-log"></a>Registro eventi di replica DFS  
Il servizio Replica DFS consente la sincronizzazione non autorevole da un partner durante la clonazione. La clonazione ottiene questo risultato eliminando i file di database DFSR e lasciando inalterato per l'uso come dati sottoposti a pre-seeding il contenuto di SYSVOL. Sono previsti due tentativi di sincronizzazione.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**1004**|REPLICA DFS|Il servizio Replica DFS è stato avviato.|  
|**1314**|REPLICA DFS|Il servizio Replica DFS è stato configurato i file di registro di debug.<br /><br />Informazioni aggiuntive:<br /><br />Percorso File registro di debug: C:\Windows\debug|  
|**6102**|REPLICA DFS|Il servizio Replica DFS è stato registrato il provider WMI|  
|**1206**|REPLICA DFS|Il servizio Replica DFS riuscito a contattare controller di dominio DC2.corp.contoso.com per accedere alle informazioni di configurazione.|  
|**1210**|REPLICA DFS|Il servizio Replica DFS è stato configurato da un listener RPC per le richieste di replica in ingresso.<br /><br />Informazioni aggiuntive:<br /><br />Porta: da 0"|  
|**4614**|REPLICA DFS|Il servizio Replica DFS inizializzato SYSVOL nel percorso locale C:\Windows\SYSVOL\domain ed è in attesa di eseguire la replica iniziale. La cartella replicata rimarrà in stato di sincronizzazione iniziale fino a quando non replicato con il partner. Se il server non è in corso l'innalzamento di livello a controller di dominio, il controller di dominio non annunciare e non funzionerà come controller di dominio finché questo problema viene risolto. Ciò può verificarsi se il partner specificato è anche in stato di sincronizzazione iniziale oppure se vengono rilevate violazioni di condivisione server o il partner di sincronizzazione. Se questo evento si è verificato durante la migrazione di SYSVOL dal servizio Replica File (FRS) a replica DFS, le modifiche non verranno replicate finché non viene risolto il problema. Ciò può causare la cartella SYSVOL su questo server per perdere la sincronizzazione con altri controller di dominio.<br /><br />Informazioni aggiuntive:<br /><br />Nome cartella replicata: Condivisione SYSVOL<br /><br />ID cartella replicata:*<GUID>*<br /><br />Nome gruppo di replica: Volume sistema dominio<br /><br />ID gruppo di replica:*<GUID>*<br /><br />ID membro:*<GUID>*<br /><br />Sola lettura: 0|  
|**4604**|REPLICA DFS|Il servizio Replica DFS inizializzato correttamente la cartella replicata SYSVOL nel percorso locale C:\Windows\SYSVOL\domain. This member has completed initial synchronization of SYSVOL with partner dc1.corp.contoso.com.  To check for the presence of the SYSVOL share, open a command prompt window and then type ""net share"".<br /><br />Informazioni aggiuntive:<br /><br />Nome cartella replicata: Condivisione SYSVOL<br /><br />ID cartella replicata:*<GUID>*<br /><br />Nome gruppo di replica: Volume sistema dominio<br /><br />ID gruppo di replica:*<GUID>*<br /><br />ID membro:*<GUID>*<br /><br />Partner sincronizzazione:*<domain controller FQDN>*|  
  
## <a name="BKMK_TshootVDCSafeRestore"></a>Risoluzione dei problemi di ripristino sicuro di controller di dominio virtualizzati  
  
### <a name="tools-for-troubleshooting"></a>Strumenti per la risoluzione dei problemi  
  
#### <a name="logging-options"></a>Opzioni di registrazione  
I log predefiniti rappresentano lo strumento più importante per la risoluzione dei problemi di ripristino sicuro dello snapshot di controller di dominio. Tutti questi log sono abilitati e configurati per massimo livello di dettaglio, per impostazione predefinita.  
  
|||  
|-|-|  
|**Operazione**|**Registro**|  
|**Creazione di snapshot**|-Event eventi\log di applicazioni e servizi logs\Microsoft\Windows\Hyper-V-Worker|  
|**Ripristino di snapshot**|-Event eventi\log di applicazioni e Servizi\servizio directory<br />-Visualizzatore eventi\log di Windows\Sistema<br />-Visualizzatore eventi\log di windows\applicazione<br />-Event eventi\log di applicazioni e servizi\servizio replica file<br />-Event eventi\log di applicazioni e servizi\replica DFS<br />-Event eventi\log di applicazioni e servizi\dfs<br />-Event eventi\log di applicazioni e servizi logs\Microsoft\Windows\Hyper-V-Worker|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Strumenti e comandi per la risoluzione dei problemi di configurazione del Controller di dominio  
Per risolvere i problemi non spiegati dai log, usare gli strumenti seguenti come punto di partenza:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Network Monitor 3.4  
  
#### <a name="BKMK_TshhotSafeRestore"></a>Metodologia generale per la risoluzione dei problemi di ripristino sicuro di Controller di dominio  
  
1.  Il ripristino sicuro dello snapshot previsto, ma si verificano problemi?  
  
    1.  Esaminare il registro eventi di servizi di Directory  
  
        1.  Sono presenti errori di ripristino di snapshot?  
  
        2.  Sono presenti errori di replica di Active Directory?  
  
    2.  Esaminare il registro eventi di sistema  
  
        1.  Sono presenti errori di comunicazione?  
  
        2.  Sono presenti errori di Active Directory?  
  
2.  È il ripristino sicuro dello snapshot è imprevisto?  
  
    1.  Esaminare il log di controllo per determinare chi o cosa ha causato un rollback  
  
    2.  Contattare tutti gli amministratori dell'hypervisor e scoprire che ha eseguito il rollback della macchina virtuale senza notifica  
  
3.  È la protezione da rollback degli USN implementazione server e il ripristino in modo non sicuro?  
  
    1.  Esaminare il registro eventi di servizi di Directory per un hypervisor o integrazione di servizi non supportati  
  
    2.  Esaminare il sistema operativo e verificare che esegue Windows Server 2012?  
  
### <a name="BKMK_TshootSpecificSafeRestore"></a>Risoluzione di problemi specifici  
  
#### <a name="events"></a>Eventi  
Tutti virtualizzati ripristino sicuro dello snapshot controller di dominio scrivere gli eventi nel registro eventi di servizi di Directory del controller di dominio ripristinato macchina virtuale. I registri eventi dell'applicazione, sistema, servizio Replica File e replica DFS può inoltre contenere informazioni utili per ripristini non riusciti la risoluzione dei problemi.  
  
Di seguito sono gli eventi specifici del ripristino sicuri di Windows Server 2012 nel registro eventi di servizi di Directory.  
  
|||  
|-|-|  
|**ID evento**|**2170**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Avviso|  
|**Messaggio**|È stata rilevata una modifica dell'ID di generazione.<br /><br />ID generazione memorizzato nella cache in DS (valore precedente): %1<br /><br />ID generazione attualmente nella macchina virtuale (nuovo valore): %2<br /><br />La modifica dell'ID di generazione si verifica dopo l'applicazione di uno snapshot della macchina virtuale, dopo un'operazione di importazione della macchina virtuale o dopo un'operazione live migration. *<COMPUTERNAME>*verrà creato un nuovo ID di chiamata per ripristinare il controller di dominio. Controller di dominio virtualizzati non devono essere ripristinati mediante snapshot della macchina virtuale. Il metodo supportato per il ripristino o il rollback del contenuto di un database di servizi di dominio Active Directory consiste nel ripristinare un backup dello stato del sistema creato con un'applicazione di backup compatibile con servizi di dominio Active Directory.|  
|**Note e risoluzione**|Si tratta di un evento riuscito se lo snapshot era previsto. In caso contrario, esaminare il registro eventi di Hyper-V-Worker o contattare l'amministratore dell'hypervisor.|  
  
|||  
|-|-|  
|**ID evento**|**2174**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|Il controller di dominio è un clone di controller di dominio virtuale né uno snapshot di controller di dominio virtuale ripristinato.|  
|**Note e risoluzione**|Evento previsto quando si avvia controller di dominio fisici o controller di dominio virtualizzati non ripristinati da snapshot.|  
  
|||  
|-|-|  
|**ID evento**|**2181**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|La transazione è stata interrotta a causa della macchina virtuale viene ripristinata a uno stato precedente.  Ciò si verifica dopo l'applicazione di uno snapshot della macchina virtuale, dopo un'operazione di importazione della macchina virtuale o dopo un'operazione di migrazione in tempo reale.|  
|**Note e risoluzione**|Previsto quando si ripristina uno snapshot. Le transazioni tengono traccia la modifica di ID di generazione VM|  
  
|||  
|-|-|  
|**ID evento**|**2185**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>*arrestare il servizio FRS o DFSR usato per replicare la cartella SYSVOL.<br /><br />Nome servizio: % 1<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>*deve inizializzare un ripristino non autorevole la replica SYSVOL locale. Questa operazione viene eseguita arrestando il servizio FRS o DFSR usato per replicare la cartella SYSVOL e avviandolo con i valori per attivare il ripristino e le chiavi del Registro di sistema. L'evento 2187 verrà registrato al riavvio del servizio FRS o DFSR.|  
|**Note e risoluzione**|Previsto quando si ripristina uno snapshot. Tutti i dati SYSVOL nel controller di dominio viene sostituito con copia di un partner del controller di dominio.|  
|**ID evento**|2186|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|*<COMPUTERNAME>*Impossibile interrompere il servizio FRS o DFSR usato per replicare la cartella SYSVOL.<br /><br />Nome servizio: % 1<br /><br />Codice di errore: % 2<br /><br />Messaggio di errore: % 3<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>*deve inizializzare un ripristino non autorevole la replica SYSVOL locale. In tal caso, l'arresto del servizio di replica FRS o DFSR usato per replicare la cartella SYSVOL e avviandolo con i valori per attivare il ripristino e le chiavi del Registro di sistema. *<COMPUTERNAME>*Impossibile interrompere il servizio corrente in esecuzione e non può completare il ripristino non autorevole. Eseguire un ripristino non autorevole manualmente.|  
|**Note e risoluzione**|Esaminare i registri eventi di sistema, FRS e replica DFS per ulteriori informazioni.|  
  
|||  
|-|-|  
|**ID evento**|**2187**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>*avviare il servizio FRS o DFSR usato per replicare la cartella SYSVOL.<br /><br />Nome servizio: % 1<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>*per inizializzare un ripristino non autorevole per la replica SYSVOL locale. Questa operazione è stata eseguita arrestando il servizio FRS o DFSR usato per replicare la cartella SYSVOL e avviandolo con i valori per attivare il ripristino e le chiavi del Registro di sistema.|  
|**Note e risoluzione**|Previsto quando si ripristina uno snapshot. Tutti i dati SYSVOL nel controller di dominio viene sostituito con copia di un partner del controller di dominio.|  
  
|||  
|-|-|  
|**ID evento**|**2188**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|*<COMPUTERNAME>*Impossibile avviare il servizio FRS o DFSR usato per replicare la cartella SYSVOL.<br /><br />Nome servizio: % 1<br /><br />Codice di errore: % 2<br /><br />Messaggio di errore: % 3<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>*deve inizializzare un ripristino non autorevole per la replica SYSVOL locale. Questa operazione viene eseguita arrestando il servizio FRS o DFSR usato per replicare SYSVOL e avviandolo con i valori per attivare il ripristino e le chiavi del Registro di sistema. *<COMPUTERNAME>*Impossibile avviare il servizio FRS o DFSR usato per replicare la cartella SYSVOL e non può completare il ripristino non autorevole. Per eseguire un ripristino non autorevole manualmente e riavviare il servizio.|  
|**Note e risoluzione**|Esaminare i registri eventi di sistema, FRS e replica DFS per ulteriori informazioni.|  
  
|||  
|-|-|  
|**ID evento**|**2189**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>*impostare i seguenti valori del Registro di sistema per inizializzare la replica SYSVOL durante un ripristino non autorevole:<br /><br />Chiave del Registro di sistema: % 1<br /><br />Il valore del Registro di sistema: %2<br /><br />Dati del valore del Registro di sistema: %3<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>*deve inizializzare un ripristino non autorevole per la replica SYSVOL locale. Questa operazione viene eseguita arrestando il servizio FRS o DFSR usato per replicare la cartella SYSVOL e avviandolo con i valori per attivare il ripristino e le chiavi del Registro di sistema.|  
|**Note e risoluzione**|Previsto quando si ripristina uno snapshot. Tutti i dati SYSVOL nel controller di dominio viene sostituito con copia di un partner del controller di dominio.|  
  
|||  
|-|-|  
|**ID evento**|**2190**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|*<COMPUTERNAME>*non è riuscito a impostare i seguenti valori del Registro di sistema per inizializzare la replica SYSVOL durante un ripristino non autorevole:<br /><br />Chiave del Registro di sistema: % 1<br /><br />Il valore del Registro di sistema: %2<br /><br />Dati del valore del Registro di sistema: %3<br /><br />Codice di errore: % 4<br /><br />Messaggio di errore: % 5<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il ruolo di controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>*deve inizializzare un ripristino non autorevole per la replica SYSVOL locale. Questa operazione viene eseguita arrestando il servizio FRS o DFSR usato per replicare la cartella SYSVOL e avviandolo con i valori per attivare il ripristino e le chiavi del Registro di sistema. *<COMPUTERNAME>*non è riuscito a impostare i seguenti valori del Registro di sistema e non può completare il ripristino non autorevole. Eseguire un ripristino non autorevole manualmente.|  
|**Note e risoluzione**|Esaminare i registri eventi dell'applicazione e sistema. Esaminare le applicazioni di terze parti che potrebbero bloccare gli aggiornamenti del Registro di sistema.|  
  
|||  
|-|-|  
|**ID evento**|**2200**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>*Consente di inizializzare la replica per aggiornare il controller di dominio. L'evento 2201 verrà registrato al termine della replica.|  
|**Note e risoluzione**|Previsto quando si ripristina uno snapshot. Segna l'inizio della replica di Active Directory in ingresso.|  
  
|||  
|-|-|  
|**ID evento**|**2201**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>*è stata completata la replica per aggiornare il controller di dominio.|  
|**Note e risoluzione**|Previsto quando si ripristina uno snapshot. Segna la fine di replica di Active Directory in ingresso.|  
  
|||  
|-|-|  
|**ID evento**|**2202**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>*replica non riuscito per aggiornare il controller di dominio. Il controller di dominio verrà aggiornato dopo la prossima replica periodica.|  
|**Note e risoluzione**|Esaminare i registri eventi di sistema e servizi di Directory. Usare repadmin.exe per provare a forzare la replica e prendere nota di eventuali errori.|  
  
|||  
|-|-|  
|**ID evento**|**2204**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>*è stata rilevata una modifica dell'ID di generazione macchina virtuale. Questa modifica indica che il controller di dominio virtuale è stato ripristinato a uno stato precedente. *<COMPUTERNAME>*verranno eseguite le operazioni seguenti per proteggere tale controller di dominio da possibili divergenze di dati e per proteggere la creazione di entità di sicurezza con SID duplicati:<br /><br />Creare un nuovo ID di chiamata<br /><br />Invalidare il pool RID corrente<br /><br />Verrà convalidata proprietà dei ruoli FSMO alla successiva replica in ingresso. Durante questo periodo se il controller di dominio rimane un ruolo FSMO, tale ruolo non sarà disponibile.<br /><br />Avviare l'operazione di ripristino del servizio replica di SYSVOL.<br /><br />Avviare la replica per aggiornare il controller di dominio ripristinato allo stato più recente.<br /><br />Richiedere un nuovo pool di RID.|  
|**Note e risoluzione**|Previsto quando si ripristina uno snapshot. Questo spiega che tutte le varie operazioni di reimpostazione che si verificheranno come parte del processo di ripristino sicuro.|  
  
|||  
|-|-|  
|**ID evento**|**2205**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>*invalidato il pool RID corrente dopo il ripristino dello stato precedente controller di dominio virtuale.|  
|**Note e risoluzione**|Previsto quando si ripristina uno snapshot. Il pool di RID locale deve essere rimosso il controller di dominio è più aggiornato e possono sono già stati rilasciati.|  
  
|||  
|-|-|  
|**ID evento**|**2206**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|ERRORE|  
|**Messaggio**|*<COMPUTERNAME>*Impossibile invalidare il pool RID corrente dopo il ripristino di controller di dominio virtuale uno stato precedente.<br /><br />Dati aggiuntivi:<br /><br />Codice di errore: %1<br /><br />Valore di errore: %2|  
|**Note e risoluzione**|Esaminare i registri eventi di sistema e servizi di Directory. Verificare che il Master RID sia in linea può essere raggiunto dal server con Dcdiag.exe /test: RidManager|  
  
|||  
|-|-|  
|**ID evento**|**2207**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|ERRORE|  
|**Messaggio**|*<COMPUTERNAME>*Impossibile ripristinare dopo il ripristino dello stato precedente controller di dominio virtuale. È stato richiesto un riavvio in modalità DSRM. Controllare gli eventi precedenti per ulteriori informazioni.|  
|**Note e risoluzione**|Esaminare i registri eventi di sistema e servizi di Directory.|  
  
|||  
|-|-|  
|**ID evento**|**2208**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Informativo|  
|**Messaggio**|*<COMPUTERNAME>*eliminare i database DFSR per inizializzare la replica SYSVOL durante un ripristino non autorevole.|  
|**Note e risoluzione**|Previsto quando si ripristina uno snapshot. In questo modo che il servizio DFSR non sincronizzi in SYSVOL da un controller di dominio partner. Si noti che qualsiasi altra cartella replicata con DFS nello stesso volume di SYSVOL saranno sincronizzati anche non autorevole (dominio controller non sono consigliati per l'host personalizzati che set di repliche DFS nello stesso volume di SYSVOL).|  
  
|||  
|-|-|  
|**ID evento**|**2209**|  
|**Origine**|Microsoft-Windows-ActiveDirectory DomainService|  
|**Livello di gravità**|Errore|  
|**Messaggio**|*<COMPUTERNAME>*non è riuscito a eliminare i database DFSR.<br /><br />Dati aggiuntivi:<br /><br />Codice di errore: %1<br /><br />Valore di errore: %2<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. *<COMPUTERNAME>*deve inizializzare un ripristino non autorevole per la replica SYSVOL locale. Per replica DFS, questa operazione viene eseguita arrestando il servizio DFSR, eliminando i database DFSR e riavviando il servizio. Al riavvio DFSR ricostruirà i database e avvierà la sincronizzazione iniziale.|  
|**Note e risoluzione**|Esaminare il registro eventi di replica DFS.|  
  
#### <a name="error-messages"></a>Messaggi di errore  
Non sono presenti errori interattivi diretti per ripristino sicuro dello snapshot di controller di dominio virtualizzati; non è riuscito tutte le informazioni di clonazione registra nel registro eventi di servizi di Directory. Ovviamente, eventuali errori critici di replica o server pubblicità si manifestano come sintomi altrove.  
  
#### <a name="known-issues-and-support-scenarios"></a>Problemi noti e scenari di supporto  
Il [metodologia generale per la risoluzione dei problemi di dominio ripristino sicuro dei Controller](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootSpecificSafeRestore) sono in genere sufficienti a risolvere la maggior parte dei problemi.  
  
|||  
|-|-|  
|**Problema**|**Impossibile creare nuove entità di sicurezza nel controller di dominio ripristinato di recente sicuro**|  
|**Sintomi**|Dopo il ripristino di uno snapshot, tenta di creare una nuova entità di sicurezza (utente, computer, gruppo) in tale dominio non riescono con:<br /><br />Errore 0x2010<br /><br />Il servizio directory è stato in grado di allocare l'identificatore relativo.|  
|**Risoluzione e note**|Questo problema è causato da informazioni non aggiornate del computer ripristinato del ruolo FSMO Master RID. Se il ruolo spostato in controller di dominio di questo o un altro dopo l'uno snapshot è stato eseguito e quindi ripristinato, il controller di dominio ripristinato non sarà possibile conoscere il master RID fino al completamento della replica iniziale.<br /><br />Per risolvere il problema, consentire il completamento della replica di Active Directory in ingresso per il controller di dominio ripristinato. Se ancora non funziona, verificare che tutti i controller di dominio abbiano le stesse informazioni corrette dei quali controller di dominio ospita il Master RID.|  
  
|||  
|-|-|  
|**Problema**|**Controller di dominio ripristinati non condividono SYSVOL e annunciare**|  
|**Sintomi**|Dopo il ripristino di uno snapshot, uno o più controller di dominio non inviano annunci, non condividono sysvol e non è aggiornato contenuto SYSVOL|  
|**Risoluzione e note**|I partner upstream del controller di dominio non sono una replica di SYSVOL funzionante che viene replicata correttamente con replica DFS o replica file. Questo problema è correlato al ripristino sicuro, ma è probabile che indichi un problema di ripristino sicuro, perché il cliente non era a conoscenza di altro problema di replica che interessa i controller di dominio non ripristinati|  
  
### <a name="advanced-troubleshooting"></a>Risoluzione avanzata dei problemi  
Questo modulo ha lo scopo di illustrare la risoluzione avanzata dei problemi con *funziona* registri come esempi, con una spiegazione dell'accaduto. Se si conosce l'aspetto di un'operazione di controller di dominio virtualizzati, maggior chiarezza nell'ambiente gli errori. Questi log vengono presentati dalla relativa origine, con l'ordine crescente di *previsto* degli eventi relativi a un controller di dominio clonato all'interno di ogni log.  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-dfsr"></a>Ripristino di un Controller di dominio che replica SYSVOL con replica DFS  
  
##### <a name="directory-services-event-log"></a>Registro eventi dei servizi directory  
Il log dei servizi Directory contiene la maggior parte delle informazioni operative sul ripristino sicuro. L'hypervisor cambia l'ID di generazione VM e il servizio NTDS ne prende nota, quindi invalida il pool di RID e cambia l'ID di chiamata. Il nuovo ID di generazione VM è set e i server replicano i dati di Active Directory in ingresso. Il servizio Replica DFS viene arrestato e il relativo database che ospita SYSVOL viene eliminato, forzando una sincronizzazione non autorevole in ingresso. Il limite massimo USN viene modificato.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**2170**|ActiveDirectory DomainService|È stata rilevata una modifica dell'ID di generazione.<br /><br />ID generazione memorizzato nella cache in DS (valore precedente):<br /><br />*<number>*<br /><br />ID generazione attualmente nella macchina virtuale (nuovo valore):<br /><br />*<number>*<br /><br />La modifica dell'ID di generazione si verifica dopo l'applicazione di uno snapshot della macchina virtuale, dopo un'operazione di importazione della macchina virtuale o dopo un'operazione live migration. Servizi di dominio Active Directory verrà creato un nuovo ID di chiamata per ripristinare il controller di dominio. Controller di dominio virtualizzati non devono essere ripristinati mediante snapshot della macchina virtuale. Il metodo supportato per il ripristino o il rollback del contenuto di un database di servizi di dominio Active Directory consiste nel ripristinare un backup dello stato del sistema creato con un'applicazione di backup compatibile con servizi di dominio Active Directory."|  
|**2181**|ActiveDirectory DomainService|La transazione è stata interrotta a causa della macchina virtuale viene ripristinata a uno stato precedente.  Ciò si verifica dopo l'applicazione di uno snapshot della macchina virtuale, dopo un'operazione di importazione della macchina virtuale o dopo un'operazione di migrazione in tempo reale.|  
|**2204**|ActiveDirectory DomainService|Servizi di dominio Active Directory è stata rilevata una modifica dell'ID di generazione macchina virtuale. Questa modifica indica che il controller di dominio virtuale è stato ripristinato a uno stato precedente. Servizi di dominio Active Directory verranno eseguite le operazioni seguenti per proteggere tale controller di dominio da possibili divergenze di dati e per proteggere la creazione di entità di sicurezza con SID duplicati:<br /><br />Creare un nuovo ID di chiamata<br /><br />Invalidare il pool RID corrente<br /><br />Verrà convalidata proprietà dei ruoli FSMO alla successiva replica in ingresso. Durante questo periodo se il controller di dominio rimane un ruolo FSMO, tale ruolo non sarà disponibile.<br /><br />Avviare l'operazione di ripristino del servizio replica di SYSVOL.<br /><br />Avviare la replica per aggiornare il controller di dominio ripristinato allo stato più recente.<br /><br />Richiedere un nuovo pool di RID".|  
|**2181**|ActiveDirectory DomainService|La transazione è stata interrotta a causa della macchina virtuale viene ripristinata a uno stato precedente.  Ciò si verifica dopo l'applicazione di uno snapshot della macchina virtuale, dopo un'operazione di importazione della macchina virtuale o dopo un'operazione di migrazione in tempo reale.|  
|**1109**|ActiveDirectory DomainService|L'attributo invocationID per il server di directory è stato modificato. Il numero di sequenza di aggiornamento più alto al momento che della creazione del backup è come segue:<br /><br />Attributo InvocationID (vecchio valore):<br /><br />*<GUID>*<br /><br />Attributo InvocationID (nuovo valore):<br /><br />*<GUID>*<br /><br />Numero di sequenza di aggiornamento:<br /><br />*<number>*<br /><br />L'attributo invocationID viene modificato quando un server di directory viene ripristinato dal supporto di backup, è configurato per ospitare una partizione di directory applicativa scrivibile, viene ripreso dopo aver applicato uno snapshot macchina virtuale, dopo un'operazione di importazione della macchina virtuale o dopo un'operazione live migration. Controller di dominio virtualizzati non devono essere ripristinati mediante snapshot della macchina virtuale. Il metodo supportato per il ripristino o il rollback del contenuto di un database di servizi di dominio Active Directory consiste nel ripristinare un backup dello stato del sistema creato con un'applicazione di backup compatibile con Active Directory Domain Services."|  
|**2179**|ActiveDirectory DomainService|L'attributo msDS-GenerationId dell'oggetto computer del Controller di dominio è stata impostata per il parametro seguente:<br /><br />Attributo GenerationID:<br /><br />*<number>*|  
|**2200**|ActiveDirectory DomainService|Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. Servizi di dominio Active Directory verrà inizializzata la replica per aggiornare il controller di dominio corrente. L'evento 2201 verrà registrato al termine della replica.|  
|**2201**|ActiveDirectory DomainService|Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. Servizi di dominio Active Directory è stata completata la replica per aggiornare il controller di dominio.|  
|**2185**|ActiveDirectory DomainService|Servizi di dominio Active Directory è stato arrestato il servizio FRS o DFSR usato per replicare la cartella SYSVOL.<br /><br />Nome servizio:<br /><br />REPLICA DFS<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. Servizi di dominio Active Directory deve inizializzare un ripristino non autorevole la replica SYSVOL locale. Questa operazione viene eseguita arrestando il servizio FRS o DFSR usato per replicare la cartella SYSVOL e avviandolo con i valori per attivare il ripristino e le chiavi del Registro di sistema. L'evento 2187 verrà registrato al riavvio del servizio FRS o DFSR."|  
|**2208**|ActiveDirectory DomainService|Servizi di dominio Active Directory eliminato i database DFSR per inizializzare la replica SYSVOL durante un ripristino non autorevole.<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. Servizi di dominio Active Directory deve inizializzare un ripristino non autorevole per la replica SYSVOL locale. Per replica DFS, questa operazione viene eseguita arrestando il servizio DFSR, eliminando i database DFSR e riavviando il servizio. Upon restarting DFSR will rebuild the databases and start the initial sync. "|  
|**2187**|ActiveDirectory DomainService|Servizi di dominio Active Directory è stato avviato il servizio FRS o DFSR usato per replicare la cartella SYSVOL.<br /><br />Nome servizio:<br /><br />REPLICA DFS<br /><br />Active Directory ha rilevato che la macchina virtuale che ospita il controller di dominio è stata ripristinata a uno stato precedente. Servizi di dominio Active Directory è necessario inizializzare un ripristino non autorevole la replica SYSVOL locale. Questa operazione è stata eseguita arrestando il servizio FRS o DFSR usato per replicare la cartella SYSVOL e avviandolo con i valori per attivare il ripristino e le chiavi del Registro di sistema. "|  
|**1587**|ActiveDirectory DomainService|Il servizio directory è stato ripristinato oppure configurato per ospitare una partizione di directory. Di conseguenza, l'identità di replica è stato modificato. Un partner ha chiesto cambiamenti di replica utilizzando i nostri identità precedente dell'utente. Il numero di sequenza iniziale è stato modificato.<br /><br />Il servizio directory di destinazione corrispondente per il seguente GUID oggetto che ha chiesto cambiamenti a partire dall'USN che precede quello in cui il servizio directory locale è stato ripristinato dal supporto di backup.<br /><br />GUID oggetto:<br /><br />*<GUID> (<FQDN of partner domain controller>)*<br /><br />USN al momento del ripristino:<br /><br />*<number>*<br /><br />Di conseguenza, il vettore di aggiornamento del servizio directory di destinazione è stato configurato con le impostazioni seguenti.<br /><br />GUID database precedente:<br /><br />*<GUID>*<br /><br />Oggetto USN precedente:<br /><br />*<number>*<br /><br />Proprietà USN precedente:<br /><br />*<number>*<br /><br />Nuovo GUID database:<br /><br />*<GUID>*<br /><br />Nuovo USN oggetto:<br /><br />*<number>*<br /><br />Nuovo USN proprietà:<br /><br />*<number>*|  
  
##### <a name="system-event-log"></a>Registro eventi di sistema  
Registro eventi di sistema Annota l'ora del computer che si verifica quando riportare online una macchina virtuale non in linea e la sincronizzazione con l'ora dell'host. Il pool di RID viene invalidato e i servizi DFS o replica file vengono riavviati.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**1**|Kernel-General|L'ora di sistema cambiata da *?<now>* da *< ora/data snapshot >*.<br /><br />Motivo modifica: Un'applicazione o un componente del sistema ha modificato l'ora.|  
|**16654**|Directory-Services-SAM|Un pool di identificatori di account (RID) è stato invalidato. Ciò può verificarsi nei seguenti casi previsti:<br /><br />1. un controller di dominio viene ripristinato da backup.<br /><br />2. un controller di dominio in una macchina virtuale viene ripristinato da uno snapshot.<br /><br />3. un amministratore ha invalidato manualmente il pool.<br /><br />See https://go.microsoft.com/fwlink/?LinkId=226247 for more information.|  
|**7036**|Gestione controllo servizi|Il servizio Replica DFS è stato interrotto.|  
|**7036**|Gestione controllo servizi|Il servizio Replica DFS è in esecuzione.|  
  
##### <a name="application-event-log"></a>Registro eventi dell'applicazione  
Registro eventi dell'applicazione Annota l'arresto e avvio del database DFSR.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**103**|ESE|DFSR (1360) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: il motore di database ha arrestato l'istanza (0).<br /><br />Arresto anomalo: 0<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.141, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.016, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**102**|ESE|DFSR (532) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: il motore di database (6.02.8189.0000) sta avviando una nuova istanza (0).|  
|**105**|ESE|DFSR (532) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: il motore di database avviato una nuova istanza (0). (Tempo = 0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.000.|  
|||DFSR (532) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: il motore di database ha creato un nuovo database (1, \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db). (Tempo = 0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.016, [4] 0.062, [5] 0.000, [6] 0.016, [7] 0.000, [8] 0.000, [9] 0.015, [10] 0.000, [11] 0.000.|  
  
##### <a name="dfs-replication-event-log"></a>Registro eventi di replica DFS  
Il servizio Replica DFS viene arrestato e il database che contiene SYSVOL viene eliminato, forzando una sincronizzazione non autorevole in ingresso.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**1006**|REPLICA DFS|Il servizio Replica DFS.|  
|**1008**|REPLICA DFS|Il servizio Replica DFS è stato arrestato.|  
|**1002**|REPLICA DFS|Viene avviato il servizio Replica DFS.|  
|**1004**|REPLICA DFS|Il servizio Replica DFS è stato avviato.|  
|**1314**|REPLICA DFS|Il servizio Replica DFS è stato configurato i file di registro di debug.<br /><br />Informazioni aggiuntive:<br /><br />Percorso File registro di debug: C:\Windows\debug|  
|**6102**|REPLICA DFS|Il servizio Replica DFS è stato registrato nel provider WMI.|  
|**1206**|REPLICA DFS|Il servizio Replica DFS ha contattato dominio controller * <domain controller FQDN> * alle informazioni sulla configurazione di accesso.|  
|**1210**|REPLICA DFS|Il servizio Replica DFS è stato configurato da un listener RPC per le richieste di replica in ingresso.<br /><br />Informazioni aggiuntive:<br /><br />Porta: 0|  
|**4614**|REPLICA DFS|Il servizio Replica DFS inizializzato SYSVOL nel percorso locale C:\Windows\SYSVOL\domain ed è in attesa di eseguire la replica iniziale. La cartella replicata rimarrà in stato di sincronizzazione iniziale fino a quando non replicato con il partner. Se il server non è in corso l'innalzamento di livello a controller di dominio, il controller di dominio non annunciare e non funzionerà come controller di dominio finché questo problema viene risolto. Ciò può verificarsi se il partner specificato è anche in stato di sincronizzazione iniziale oppure se vengono rilevate violazioni di condivisione server o il partner di sincronizzazione. Se questo evento si è verificato durante la migrazione di SYSVOL dal servizio Replica File (FRS) a replica DFS, le modifiche non verranno replicate finché non viene risolto il problema. Ciò può causare la cartella SYSVOL su questo server per perdere la sincronizzazione con altri controller di dominio.<br /><br />Informazioni aggiuntive:<br /><br />Nome cartella replicata: Condivisione SYSVOL<br /><br />ID cartella replicata:*<GUID>*<br /><br />Nome gruppo di replica: Volume sistema dominio<br /><br />ID gruppo di replica:*<GUID>*<br /><br />ID membro:*<GUID>*<br /><br />Sola lettura: 0|  
|**4604**|REPLICA DFS|Il servizio Replica DFS inizializzato correttamente la cartella replicata SYSVOL nel percorso locale C:\Windows\SYSVOL\domain. This member has completed initial synchronization of SYSVOL with partner dc1.corp.contoso.com.  To check for the presence of the SYSVOL share, open a command prompt window and then type "net share".<br /><br />Informazioni aggiuntive:<br /><br />Nome cartella replicata: Condivisione SYSVOL<br /><br />ID cartella replicata:*<GUID>*<br /><br />Nome gruppo di replica: Volume sistema dominio<br /><br />ID gruppo di replica:*<GUID>*<br /><br />ID membro:*<GUID>*<br /><br />Partner sincronizzazione:*<partner domain controller FQDN>*|  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-frs"></a>Ripristino di un Controller di dominio che replica SYSVOL con replica file  
Il registro eventi di replica File viene utilizzato in questo caso anziché nel registro eventi di replica DFS. Inoltre, il registro eventi dell'applicazione scrive diversi eventi correlati al servizio Replica file. In caso contrario, i servizi di Directory e registro eventi di sistema messaggi sono in genere lo stesso e nello stesso ordine in precedenza descritto.  
  
##### <a name="file-replication-service-event-log"></a>Registro eventi del servizio Replica file  
Il servizio Replica file viene arrestato e riavviato con un valore D2 BURFLAGS per la sincronizzazione non autorevole di SYSVOL.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**13502**|NTFRS|Arresto del servizio Replica File.|  
|**13503**|NTFRS|Il servizio Replica File è stato arrestato.|  
|**13501**|NTFRS|Avvio del servizio Replica File|  
|**13512**|NTFRS|Il servizio Replica File ha rilevato una cache di scrittura del disco attivata sull'unità che contiene la directory c:\windows\ntfrs\jet sul computer DC4. Il servizio Replica File potrebbe non ripristinare quando potenza per l'unità viene interrotto e aggiornamenti critici vadano perduti.|  
|**13565**|NTFRS|Servizio Replica file sta inizializzando il volume di sistema con i dati da un altro controller di dominio. Computer DC4 può diventare un controller di dominio fino al completamento del processo. Il volume di sistema verrà quindi condiviso come SYSVOL.<br /><br />Per controllare la condivisione SYSVOL, al prompt dei comandi, digitare:<br /><br />condivisione di rete<br /><br />Quando replica File ha completato il processo di inizializzazione, verrà visualizzata la condivisione SYSVOL.<br /><br />L'inizializzazione del volume di sistema può richiedere del tempo. Il tempo è dipende dalla quantità di dati nel volume di sistema, la disponibilità di altri controller di dominio e l'intervallo di replica tra controller di dominio".|  
|**13520**|NTFRS|Il servizio Replica File ha spostato i file preesistenti * <path> * a * <path> *\ntfrs_preexisting___see_eventlog..<br /><br />Il servizio Replica File potrebbe eliminare i file in * <path> *\NtFrs_PreExisting___See_EventLog in qualsiasi momento. File possono essere salvati dall'eliminazione copiandoli fuori * <path> *\ntfrs_preexisting___see_eventlog.. Copia dei file in * <path> * possono verificarsi conflitti di nome di se i file esistano già su altri partner di replica.<br /><br />In alcuni casi, il servizio Replica File copia un file da * <path> *\NtFrs_PreExisting___See_EventLog a * <path> * anziché da altri partner di replica per il file.<br /><br />In qualsiasi momento è possibile recuperare spazio eliminando i file in * <path> *\ntfrs_preexisting___see_eventlog..|  
|**13553**|NTFRS|Il servizio Replica File ha aggiunto questo computer al set di repliche seguenti:<br /><br />"VOLUME SISTEMA DOMINIO (CONDIVISIONE SYSVOL)"<br /><br />Informazioni relative a questo evento sono illustrate di seguito:<br /><br />Nome DNS del computer è "*<domain controller FQDN>*"<br /><br />Nome del membro dell'insieme di replica è "*<domain controller name>*"<br /><br />Percorso radice dell'insieme di replica è "*<path>*"<br /><br />Percorso della directory di gestione temporanea della replica è "* <path> * "<br /><br />Percorso della directory di lavoro replica è "*<path>*"|  
|**13554**|NTFRS|Il servizio Replica File ha aggiunto le connessioni al set di repliche illustrate di seguito:<br /><br />"VOLUME SISTEMA DOMINIO (CONDIVISIONE SYSVOL)"<br /><br />In ingresso da "*<partner domain controller FQDN>*"<br /><br />In uscita da "*<partner domain controller FQDN>*"<br /><br />Altre informazioni potranno essere visualizzate in messaggi del registro eventi successivi.|  
|**13516**|NTFRS|Il servizio Replica File è non impedisce più al computer DC4 di diventare un controller di dominio. Il volume di sistema è stato inizializzato correttamente e il servizio Accesso rete è stato notificato che il volume di sistema è ora pronto per essere condiviso come SYSVOL.<br /><br />Digitare "net share" per controllare la condivisione SYSVOL.|  
  
##### <a name="application-event-log"></a>Registro eventi dell'applicazione  
Il database FRS arresta viene avviato e viene ripulito a causa dell'operazione D2 BURFLAGS.  
  
||||  
|-|-|-|  
|**ID evento**|**Origine**|**Messaggio**|  
|**327**|ESE|NTFRS (1424) il motore di database scollegato da un database (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tempo = 0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.015, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.516, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.063, [12] 0.000.<br /><br />Cache ripristinata: 0|  
|**103**|ESE|NTFRS (1424) il motore di database ha arrestato l'istanza (0).<br /><br />Arresto anomalo: 0<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.031, [10] 0.000, [11] 0.016, [12] 0.000, [13] 0.000, [14] 0.047, [15] 0.000.|  
|**102**|ESE|NTFRS (3000) il motore di database (6.02.8189.0000) sta avviando una nuova istanza (0).|  
|**105**|ESE|NTFRS (3000) il motore di database avviato una nuova istanza (0). (Tempo = 0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.062, [10] 0.000, [11] 0.141.|  
|**103**|ESE|NTFRS (3000) il motore di database ha arrestato l'istanza (0).<br /><br />Arresto anomalo: 0<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000, [13] 0.015, [14] 0.000, [15] 0.000.|  
|**102**|ESE|NTFRS (3000) il motore di database (6.02.8189.0000) sta avviando una nuova istanza (0).|  
|**105**|ESE|NTFRS (3000) il motore di database avviato una nuova istanza (0). (Tempo = 0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.078, [10] 0.000, [11] 0.109.|  
|**325**|ESE|NTFRS (3000) il motore di database creato un nuovo database (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tempo = 0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.016, [4] 0.016, [5] 0.000, [6] 0.015, [7] 0.000, [8] 0.000, [9] 0.078, [10] 0.016, [11] 0.000.|  
|**103**|ESE|NTFRS (3000) il motore di database ha arrestato l'istanza (0).<br /><br />Arresto anomalo: 0<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.000, [3] 0.000, [4] 0.000, [5] 0.078, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.125, [10] 0.016, [11] 0.000, [12] 0.000, [13] 0.000, [14] 0.000, [15] 0.000.|  
|**102**|ESE|NTFRS (3000) il motore di database (6.02.8189.0000) sta avviando una nuova istanza (0).|  
|**105**|ESE|NTFRS (3000) il motore di database avviato una nuova istanza (0). (Tempo = 0 secondi)<br /><br />Sequenza temporale interna: [1] 0.016, [2] 0.000, [3] 0.000, [4] 0.094, [5] 0.000, [6] 0.000, [7] 0.000, [8] 0.000, [9] 0.032, [10] 0.000, [11] 0.000.|  
|**326**|ESE|NTFRS (3000) il motore di database collegato un database (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tempo = 0 secondi)<br /><br />Sequenza temporale interna: [1] 0.000, [2] 0.015, [3] 0.000, [4] 0.000, [5] 0.016, [6] 0.015, [7] 0.000, [8] 0.000, [9] 0.000, [10] 0.000, [11] 0.000, [12] 0.000.<br /><br />Cache salvata: 1|  
  


