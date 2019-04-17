---
ms.assetid: 5ab76733-804d-4f30-bee6-cb672ad5075a
title: Risoluzione dei problemi di distribuzione del Controller di dominio
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 254209b0da6a7bc0cd5f3880e14a7e5b16822cdd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshooting-domain-controller-deployment"></a>Risoluzione dei problemi di distribuzione del Controller di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento illustra una metodologia dettagliata per la risoluzione dei problemi di distribuzione e configurazione dei controller di dominio.  
  
-   [Introduzione alla risoluzione dei problemi](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md#BKMK_Intro)  
  
-   [Opzioni di risoluzione dei problemi](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md#BKMK_Options)  
  
## <a name="BKMK_Intro"></a>Introduzione alla risoluzione dei problemi  
![Risoluzione dei problemi](media/Troubleshooting-Domain-Controller-Deployment/adds_deploy_troubleshooting.png)  
  
## <a name="BKMK_Options"></a>Opzioni di risoluzione dei problemi  
  
### <a name="logging-options"></a>Opzioni di registrazione  
I log predefiniti rappresentano lo strumento più importante per la risoluzione dei problemi relativi alla promozione del controller di dominio e l'abbassamento di livello. Tutti questi log sono abilitati e configurati per massimo livello di dettaglio per impostazione predefinita.  
  
|Fase|Registro|  
|---------|-------|  
|Operazioni di Server Manager o ADDSDeployment di Windows PowerShell|-%systemroot%\debug\dcpromoui.log<br /><br />-%systemroot%\debug\dcpromoui*.log|  
|Installazione/innalzamento di livello del controller di dominio|-%systemroot%\Debug\Dcpromo.log.<br /><br />-%systemroot%\debug\dcpromo*.log<br /><br />-Visualizzatore eventi\log di Windows\Sistema<br /><br />-Visualizzatore eventi\log di windows\applicazione<br /><br />-Event eventi\log di applicazioni e Servizi\servizio directory<br /><br />-Event eventi\log di applicazioni e servizi\servizio replica file<br /><br />-Event eventi\log di applicazioni e servizi\replica DFS|  
|Aggiornamento di foresta o dominio|-%systemroot%\debug\adprep\\<datetime>\adprep.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\csv.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\dspecup.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\ldif.log*|  
|Motore di distribuzione di Server Manager ADDSDeployment di Windows PowerShell|-Event eventi\log di applicazioni e servizi Visualizzatore servizi\microsoft\windows\directoryservices-deployment\operational|  
|Manutenzione di Windows|-%systemroot%\Logs\CBS\\*<br /><br />-%systemroot%\servicing\sessions\sessions.xml<br /><br />-%systemroot%\winsxs\poqexec.log<br /><br />-%systemroot%\winsxs\pending.xml|  
  
### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Strumenti e comandi per la risoluzione dei problemi di configurazione del Controller di dominio  
Per risolvere i problemi non spiegati dai log, usare gli strumenti seguenti come punto di partenza:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   [AutoRuns.exe](https://technet.microsoft.com/sysinternals/bb963902.aspx), Gestione attività e MSInfo32.exe  
  
-   [Network Monitor 3.4](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=4865) (o uno strumento di analisi e acquisizione di rete terze parti)  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>Metodologia generale per la risoluzione dei problemi di configurazione del Controller di dominio  
  
1.  È stato un problema semplice sintassi causa l'errore?  
  
    1.  Non sono stati errato o fornito un argomento di ADDSDeployment di Windows PowerShell? Ad esempio, se si usa ADDSDeployment di Windows PowerShell, provare a aggiungere l'argomento obbligatorio **- domainname** con un nome valido?  
  
    2.  Esaminare l'output della console di Windows PowerShell con attenzione per vedere esattamente perché non riesce a analizzare la riga di comando fornito.  
  
2.  L'errore è un errore relativo ai prerequisiti?  
  
    1.  Molti errori visualizzati come risultati di innalzamento di livello irreversibili ora vengono evitati grazie il controllo dei prerequisiti.  
  
    2.  Esaminare con attenzione, il testo degli errori dei prerequisiti forniscono le indicazioni necessarie per risolvere la maggior parte dei problemi, così come sono scenari controllati.  
  
3.  È l'errore di innalzamento di livello e pertanto irreversibile?  
  
    1.  Esaminare attentamente i risultati: molti errori esistono spiegazioni semplici, ad esempio password errate, risoluzione dei nomi di rete o controller di dominio offline critici.  
  
    2.  Esaminare il Dcpromoui.log e dcpromo.log per gli errori visualizzati nell'output, quindi procedere a ritroso per individuare le indicazioni di causa dell'errore.  
  
        1.  Eseguire sempre il confronto di un log di esempio funzionante  
  
        2.  Esaminare i registri di ADPrep per gli errori solo se i risultati indicano un problema di estensione dello schema o preparare la foresta o dominio.  
  
        3.  Esaminare il registro eventi DirectoryServices-Deployment errori solo se il Dcpromoui.log mancano dettagli o se termina in modo arbitrario a causa di un'eccezione non gestita nel processo di configurazione.  
  
    3.  Esaminare i registri eventi di servizi di Directory, sistema e dell'applicazione per altre indicazioni di un problema di configurazione. Spesso, la promozione del controller di dominio è solo un sintomo di altri errori di configurazione di rete che interessano tutti i sistemi distribuiti.  
  
    4.  Usare dcdiag.exe e repadmin.exe per convalidare l'integrità della foresta complessiva e indicare lievi errori di configurazione che potrebbero impedire ulteriori controller di dominio.  
  
    5.  Usare AutoRuns.exe, Gestione attività o MSinfo32.exe per esaminare il computer per il software di terze parti che potrebbe interferire.  
  
        1.  Rimuovere il software di terze parti (non è sufficiente disattivare il software; ciò non impedisce il caricamento dei driver).  
  
    6.  Installare NetMon 3.4 nel computer in cui si verifica un errore e promuovere il controller di dominio partner di replica e analizzare il processo di innalzamento di livello con acquisizioni di rete su due lati.  
  
        1.  Confrontare l'ambiente funzionante per comprendere come apparirà un corretto innalzamento di livello e in cui non riesce.  
  
        2.  A questo punto, gli errori sono probabilmente con gli oggetti della foresta, modifiche di sicurezza non predefinite o la rete e il nuovo controller di dominio è una vittima di errori di configurazione DNS, firewall, software di prevenzione intrusioni host o altri fattori esterni.  
  
### <a name="troubleshooting-specific-problems"></a>Risoluzione di problemi specifici  
  
#### <a name="events-and-error-messages"></a>Gli eventi e messaggi di errore  
Promozione del controller di dominio e l'abbassamento restituisce un codice alla fine dell'operazione e, a differenza di maggior parte dei programmi, non restituisce zero per il successo. Per visualizzare il codice alla fine di una configurazione di controller di dominio, sono disponibili diverse opzioni:  
  
1.  Quando si utilizza Server Manager, esaminare i risultati dell'innalzamento nei 10 secondi prima del riavvio automatico.  
  
2.  Quando si usa ADDSDeployment di Windows PowerShell, esaminare i risultati dell'innalzamento nei 10 secondi prima del riavvio automatico. In alternativa, scegliere di non riavviare automaticamente al completamento. È consigliabile aggiungere il **Format-List** pipeline per rendere più leggibile l'output. Per esempio:  
  
    ```  
    Install-addsdomaincontroller <options> -norebootoncompletion:$true | format-list  
  
    ```  
  
    Errori di verifica e di convalida dei prerequisiti non continuano dopo il riavvio, pertanto sono visibili in tutti i casi. Per esempio:  
  
  ![Risoluzione dei problemi](media/Troubleshooting-Domain-Controller-Deployment/ADDS_PSPrereqError.png)  
  
3.  In qualsiasi scenario, esaminare il file dcpromo.log e dcpromoui.log.  
  
    > [!NOTE]  
    > Alcuni degli errori elencati di seguito non sono più possibili a causa di modifiche alla configurazione controller del sistema operativo e di dominio nei sistemi operativi successivi. I nuovi codici di ADDSDeployment di Windows PowerShell anche impedisce alcuni errori, ma dcpromo.exe /unattend non; Questo è un altro motivo per trasferire tutta l'automazione corrente da DCPromo l'obsoleto di ADDSDeployment di Windows PowerShell.  
  
L'innalzamento e abbassamento di livello il successo seguente messaggio codici restituiti.  
  
|Codice di errore|Spiegazione|Nota|  
|--------------|---------------|--------|  
|1|Uscita, operazione riuscita|È sempre necessario riavviare, questo indica semplicemente che il riavvio automatico flag è stato rimosso|  
|2|Uscita, operazione riuscita, occorre riavviare||  
|3|Uscita, operazione riuscita con un errore non critico|Generalmente si verifica quando viene restituito il messaggio di avviso di delega DNS. Se non si configura la delega DNS, usare:<br /><br />-creatednsdelegation: $false|  
|4|Uscita, operazione riuscita con un errore non critico, occorre riavviare|Generalmente si verifica quando viene restituito il messaggio di avviso di delega DNS. Se non si configura la delega DNS, usare:<br /><br />-creatednsdelegation: $false|  
  
L'innalzamento e abbassamento di livello del seguente errore messaggio codici restituiti. È inoltre disponibile potrebbe essere un messaggio di errore estesi. leggere l'intero errore sempre con attenzione, non solo la parte numerica.  
  
|Codice di errore|Spiegazione|Soluzione suggerita|  
|--------------|---------------|------------------------|  
|11|Promozione del controller di dominio è già in esecuzione|Non eseguire più di un'istanza di promozione del controller di dominio nello stesso momento per lo stesso computer di destinazione|  
|12|È necessario essere amministratore|Accesso come membro del gruppo Administrators predefinito e assicurarsi disporre di privilegi elevati con controllo dell'account utente|  
|13|Autorità di certificazione è installata|È possibile abbassare di livello questo controller di dominio, perché è anche un'autorità di certificazione. Non rimuovere la CA prima di averne verificato attentamente l'utilizzo: se rilascia certificati, la rimozione del ruolo causerà un'interruzione del servizio. L'esecuzione di CA nei controller di dominio è sconsigliato.|  
|14|In esecuzione in modalità provvisoria|Avviare il server in modalità normale|  
|15|Modifica del ruolo è in corso o occorre riavviare|È necessario riavviare il server (a causa delle modifiche di configurazione precedenti) prima dell'innalzamento di livello|  
|16|Esecuzione nella piattaforma errata|*È improbabile che si verifichi questo errore*|  
|17|Non esistono alcuna unità NTFS 5|Questo errore non è possibile in Windows Server 2012, che richiede almeno % systemdrive % sia formattata con NTFS|  
|18|Spazio insufficiente in windir|Liberare spazio nel volume % systemdrive % mediante cleanmgr.exe|  
|19|Nome modifica in sospeso, è necessario un riavvio|Riavviare il server|  
|20|Nome del computer è sintassi non valida|Rinominare il computer con un nome valido|  
|21|Questo controller di dominio riveste ruoli FSMO, è un catalogo globale e/o è un server DNS|Aggiungere **- demoteoperationmasterrole** quando si utilizza **- forceremoval**.|  
|22|TCP/IP deve essere installato o non funziona|Verificare che il computer disponga TCP/IP configurato, associato e che funzioni correttamente|  
|23|Client DNS deve essere configurato prima di tutto|Impostare un server DNS primario quando si aggiunge un nuovo controller di dominio a un dominio|  
|24|Le credenziali fornite sono gli elementi obbligatori mancanti o non validi|Verificare il nome utente e la password sia corretta|  
|25|Controller di dominio per il dominio specificato non è possibile trovare|Convalidare le impostazioni client DNS, le regole del firewall|  
|26|Impossibile leggere l'elenco dei domini dalla foresta|Convalidare le impostazioni client DNS, le funzionalità LDAP e le regole del firewall|  
|27|Nome di dominio mancante|Specifica un dominio durante l'innalzamento o abbassamento di livello|  
|28|Nome di dominio errato|Scegliere un nome di dominio DNS valido diverso, l'innalzamento di livello|  
|29|Dominio padre non esiste|Verificare il dominio padre specificato quando si crea un nuovo dominio figlio o dominio albero|  
|30|Dominio non esiste nella foresta|Verificare il nome di dominio fornito|  
|31|Dominio figlio esiste già|Specificare un nome di dominio diverso|  
|32|Nome di dominio NetBIOS errato|Specificare un nome di dominio NetBIOS valido|  
|33|Percorso dei file IFM non è valido|Verificare il percorso della cartella installa da supporto|  
|34|Il database IFM non è valido|Utilizzare l'installa da supporto corretta per questo sistema operativo e un ruolo (stessa versione del sistema operativo, stesso tipo di controller di dominio - lettura e scrittura)|  
|35|SYSKEY mancante|L'installazione da supporto è crittografata ed è necessario fornire SYSKEY valido per usarlo|  
|37|Percorso del Database NTDS o relativi registri non è valido|Impostare il percorso del Database e registri su un volume NTFS fisso, non un'unità mappata o percorso UNC|  
|38|Volume non dispone di spazio sufficiente per database NTDS o dei registri|Liberare spazio usando cleanmgr.exe, aggiungere ulteriore spazio su disco, liberare spazio manualmente spostando altrove i dati non necessari|  
|39|Percorso per SYSVOL non è valido|Impostare il percorso della cartella SYSVOL su un volume NTFS fisso, non un'unità mappata o percorso UNC|  
|40|Nome del sito non valido|Fornire un nome di sito esistente|  
|41|È necessario specificare una password per la modalità provvisoria|Fornire una password per l'account della modalità ripristino servizi directory, non può essere lasciarla vuota indipendentemente da come sono configurato i criteri password|  
|42|Password in modalità provvisoria non soddisfa i criteri (solo innalzamento di livello)|Fornire una password per l'account della modalità ripristino servizi directory che soddisfi le regole configurate del criterio password|  
|43|Password amministratore non soddisfa i criteri (solo abbassamento di livello)|Fornire una password per l'account amministratore locale che soddisfi i criteri di password configurate regole|  
|44|Il nome specificato per la foresta è valido|Specificare un nome dominio DNS radice della foresta valido|  
|45|Un insieme di strutture con il nome specificato esiste già|Scegliere un nome di dominio DNS radice foresta diversa|  
|46|Il nome specificato per la struttura ad albero non è valido|Specificare un nome di dominio DNS valido|  
|47|Una struttura ad albero con il nome specificato esiste già|Scegliere un nome di dominio DNS albero diverso|  
|48|Il nome dell'albero non rientra nella struttura della foresta|Scegliere un nome di dominio DNS albero diverso|  
|49|Il dominio specificato non esiste|Verificare il nome di dominio digitato|  
|50|Durante l'abbassamento di livello, ultimo controller di dominio è stato rilevato anche se non è o è stato specificato l'ultimo controller di dominio, ma non|Non specificare **ultimo Controller di dominio nel dominio** (**- lastdomaincontrollerindomain**) a meno che non è vero. Utilizzare **- ignorelastdcindomainmismatch** eseguire l'override se questo sia veramente l'ultimo controller di dominio e sia i metadati di controller di dominio fantasma|  
|51|Esistono partizioni di directory nel controller di dominio|Specificare per **Rimuovi partizioni applicative** (**- removeapplicationpartitions**)|  
|52|Obbligatorio argomento della riga di comando è manca (ovvero, un file di risposte deve essere specificato nella riga di comando)|*Visualizzato solo con dcpromo /Unattend, che è deprecato. Vedere la documentazione precedente*|  
|53|L'innalzamento/abbassamento di livello non è riuscito, macchina deve essere riavviata per eseguire la pulizia|Esaminare i registri e errore estesi|  
|54|L'innalzamento/abbassamento di livello non riuscita|Esaminare i registri e errore estesi|  
|55|L'innalzamento/abbassamento di livello è stata annullata dall'utente|Esaminare i registri e errore estesi|  
|56|L'innalzamento/abbassamento di livello è stata annullata dall'utente, riavviare il computer per eseguire la pulizia|Esaminare i registri e errore estesi|  
|58|Durante l'innalzamento di livello del controller di dominio deve essere specificato un nome di sito|È necessario specificare un sito per un RODC, pertanto non verranno automaticamente rilevati come una scrittura|  
|59|Durante l'abbassamento di livello, il controller di dominio è l'ultimo server DNS per una delle relative zone|Specificare che questo è il **ultimo Server DNS nel dominio** o utilizzare **- ignorelastdnsserverfordomain**|  
|60|Un controller di dominio che esegue Windows Server 2008 o versione successiva per poter installare controller di dominio deve essere presente nel dominio|Alzare di livello almeno un Windows Server 2008 o versione successiva controller di dominio scrivibile modello|  
|61|Non è possibile installare servizi di dominio Active Directory con DNS in un dominio esistente che non ospita già DNS|*Non è possibile che si verifichi questo errore*|  
|62|File di risposte non dispone di una sezione [DCInstall]|*Visualizzato solo con dcpromo /Unattend, che è deprecato. Vedere la documentazione precedente.*|  
|63|Livello di funzionalità della foresta è inferiore a windows server 2003|Aumentare il livello di funzionalità foresta ad almeno Windows Server 2003 nativo. Windows 2000 e Windows NT 4.0 non sono più sistemi operativi supportati|  
|64|L'innalzamento di livello non riuscito rilevamento del binario del componente|Installare il ruolo di dominio Active Directory|  
|65|L'innalzamento di livello non riuscito installazione binario del componente|Installare il ruolo di dominio Active Directory|  
|66|L'innalzamento di livello non riuscito rilevamento del sistema operativo|Esaminare i registri; e errore estesi il server non riesce a restituire la versione del sistema operativo. È probabile che il computer dovrà essere reinstallato, come l'integrità complessiva risulta ambigua|  
|68|Partner di replica non è valido|Usare repadmin.exe o **Get-ADReplication\ *** Windows PowerShell per convalidare l'integrità del controller di dominio partner|  
|69|Porta necessaria è già in uso da un'altra applicazione|Utilizzare **netstat.exe - anob** per individuare i processi erroneamente assegnati alle porte di dominio Active Directory riservate|  
|70|Il controller di dominio radice foresta deve essere un catalogo globale|*Visualizzato solo con dcpromo /Unattend, che è deprecato. Vedere la documentazione precedente*|  
|71|Server DNS è già installato|Non specificare di installare il sistema DNS (**- installDNS**) se il servizio DNS è già installato|  
|72|Computer è in esecuzione Servizi Desktop remoto in modalità non amministrativa|È possibile alzare di livello questo controller di dominio, perché è anche un server di servizi desktop remoto configurato per più di due utenti admin. Non rimuovere Servizi Desktop remoto prima di averne verificato attentamente l'utilizzo: se viene utilizzato da applicazioni o gli utenti finali, la rimozione causerà un'interruzione del servizio|  
|73|Il livello di funzionalità foresta specificata non è valido.|Specificare un livello di funzionalità foresta valido|  
|74|Il livello di funzionalità del dominio specificato non è valido.|Specificare un livello funzionale di dominio valido|  
|75|Impossibile determinare i criteri di replica password predefiniti.|Verificare che i criteri di replica password RODC esista e sono accessibili|  
|76|Gruppi di sicurezza replicati/non replicati specificati non sono validi|Verificare che siano stati digitati account di dominio e utente valido quando si specificano i criteri di replica password|  
|77|L'argomento specificato non è valido|Esaminare i registri e errore estesi|  
|78|Impossibile esaminare la foresta di Active Directory|Esaminare i registri e errore estesi|  
|79|Impossibile innalzare di livello controller di dominio perché non è stato eseguito rodcprep|Usare Windows Server 2012 per preparare la foresta o eseguire **adprep.exe /rodcprep**|  
|80|DomainPrep non è stato eseguito|Usare Windows Server 2012 per preparare il dominio o eseguire **adprep.exe /domainprep**|  
|81|ForestPrep non è stato eseguito|Usare Windows Server 2012 per preparare la foresta o eseguire **adprep.exe /forestprep**|  
|82|Mancata corrispondenza degli schemi di foresta|Usare Windows Server 2012 per preparare la foresta o eseguire **adprep.exe /forestprep**|  
|83|SKU non supportata|*È improbabile che si verifichi questo errore*|  
|84|Impossibile rilevare un account di controller di dominio|Convalida per i controller di dominio è stato impostato l'attributo di controllo account utente corretto.|  
|85|Impossibile selezionare un account di controller di dominio per la fase 2|Restituito se si specifica "Usa Account esistente" ma viene trovato alcun account o si verifica un errore durante la ricerca dell'account. Verificare di che avere specificato l'account di controller di dominio preconfigurato corretto|  
|86|È necessario eseguire l'innalzamento di livello fase 2|Restituito se si innalza di livello un controller di dominio, ma esiste un account esistente "Consenti reinstallazione" non è stato specificato|  
|87|Esiste un account di controller di dominio di tipo in conflitto|Rinominare il computer prima che l'innalzamento di livello, se non si tenta di connettersi a un controller di dominio non occupato. È necessario connettere l'account controller di dominio non occupato usando **- useexistingaccount** e l'argomento di sola lettura o scrivibile corretto, a seconda del tipo di account|  
|88|L'amministratore del server specificato non è valido|È stato specificato un account non valido per la delega dell'amministrazione del controller di dominio. Verificare che l'account specificato sia un utente valido o un gruppo|  
|89|Master RID per il dominio specificato non è in linea.|Utilizzare **netdom.exe query fsmo** per rilevare il master RID. Portarlo online e renderlo accessibile al controller di dominio che si alzano di livello|  
|90|Master denominazione domini è offline.|Utilizzare **netdom.exe query fsmo** per rilevare il master denominazione domini. Portarlo online e renderlo accessibile al controller di dominio che si alzano di livello|  
|91|Impossibile rilevare se il processo è wow64|*Non è possibile che si verifichi questo errore perché il sistema operativo è a 64 bit*|  
|92|Il processo WOW64 non è supportato.|*Non è possibile che si verifichi questo errore perché il sistema operativo è a 64 bit*|  
|93|Servizio controller di dominio non è in esecuzione per l'abbassamento di livello non forzato|Avviare il servizio di dominio Active Directory|  
|94|Password di amministratore locale non soddisfa i requisiti: perché è vuota o non necessari|Fornire una password non vuota e verificare che i criteri password locali richiedano una password|  
|95|Impossibile abbassare di livello ultimo Windows Server 2008 o successive controller di dominio nel dominio in cui si trovano lettura live|È innanzitutto necessario abbassare di livello tutti lettura prima di poter abbassare di livello tutti Windows Server 2008 o versione successiva controller di dominio scrivibili|  
|96|Non è possibile disinstallare i file binari DS|*Visualizzato solo con dcpromo /Unattend, che è deprecato. Vedere la documentazione precedente*|  
|97|Insieme di strutture funzionale livello versione successiva a quella del sistema operativo dominio figlio|Fornire una funzionalità del dominio figlio di maggiore o uguale rispetto al livello funzionale di foresta|  
|98|Componente binario installare o disinstallare è in corso.|*Visualizzato solo con dcpromo /Unattend, che è deprecato. Vedere la documentazione precedente*|  
|99|Livello di funzionalità della foresta è troppo basso (errore è Windows Server 2012 solo)|Aumentare il livello di funzionalità foresta ad almeno Windows Server 2003 nativo. Windows 2000 e Windows NT 4.0 non sono più sistemi operativi supportati|  
|100|Livello di funzionalità del dominio è troppo basso (errore è Windows Server 2012 solo)|Aumentare il livello di funzionalità del dominio per almeno Windows Server 2003 nativo. Windows 2000 e Windows NT 4.0 non sono più sistemi operativi supportati|  
  
#### <a name="knownlikely-issues-and-support-scenarios"></a>Problemi noti e probabili e scenari di supporto  
Di seguito sono indicati i problemi comuni riscontrati durante il processo di sviluppo di Windows Server 2012. Tutti questi problemi sono "da progettazione" e si dispone di una soluzione alternativa valida o tecnica più appropriata per evitarli in primo luogo. Molti di questi comportamenti sono identici in Windows Server 2008 R2 e sistemi operativi precedenti, ma la riscrittura di distribuzione di dominio Active Directory presenta maggiore sensibilità nei problemi.  
  
|Problema|L'abbassamento di livello un controller di dominio, il DNS viene eseguito senza zone|  
|---------|-----------------------------------------------------------------|  
|Sintomi|Server ancora risponde alle richieste DNS ma è disponibile alcuna informazione di zona|  
|Risoluzione e note|Quando si rimuove il ruolo di dominio Active Directory, anche rimuovere il ruolo Server DNS o impostare il servizio Server DNS a disabilitato. Ricordare di indirizzare il client DNS in un altro server diverso da se stesso. Se si usa Windows PowerShell, eseguire il comando seguente dopo abbassare di livello il server:<br /><br />Codice - dns-windowsfeature disinstallare<br /><br />o<br /><br />Codice - dns - starttype set-service disabilitato<br />Interrompi servizio dns|  
  
|Problema|L'innalzamento di livello a Windows Server 2012 in un dominio con etichetta singola esistente non viene configurato updatetopleveldomain = 1 o allowsinglelabeldnsdomain = 1|  
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|  
|Sintomi|La registrazione dinamica dei record DNS non viene eseguito|  
|Risoluzione e note|Impostare questi valori usando i criteri di gruppo Netlogon e DNS. Microsoft ha iniziato a bloccare la creazione di dominio con etichetta singola in Windows Server 2008; è possibile utilizzare ADMT o lo strumento di ridenominazione del dominio per impostare una struttura di dominio DNS approvata.|  
  
|Problema|Abbassamento di livello dell'ultimo controller di dominio in un dominio non riesce se sono presenti account di controller di dominio creato in precedenza, non occupato|  
|---------|------------------------------------------------------------------------------------------------------------|  
|Sintomi|Abbassamento di livello non riesce con messaggio:<br /><br />**Dcpromo.General.54**<br /><br />Servizi di dominio Active Directory non è riuscito a trovare un altro Controller dominio Active Directory per trasferire i dati rimanenti nella partizione di directory CN = Schema, CN = Configuration, DC = corp, DC = contoso, DC = com.<br /><br />"Il formato del nome di dominio specificato è valido".|  
|Risoluzione e note|Rimuovere eventuali creati in modo preliminare account controller di dominio prima di abbassare di livello un dominio, con **Dsa.msc** o **pulizia dei metadati Ntdsutil.exe**.|  
  
|Problema|Preparazione foresta e dominio automatica non esegue GPPREP|  
|---------|---------------------------------------------------------------|  
|Sintomi|Funzionalità di pianificazione di domini per criteri di gruppo, la modalità di pianificazione di gruppo di criteri (RSOP), richiede aggiornati i file system e autorizzazioni di Active Directory per criteri di gruppo esistenti. Senza Gpprep non è possibile utilizzare gruppo di criteri risultante pianificazione tra domini.|  
|Risoluzione e note|Eseguire **adprep.exe /gpprep** manualmente per tutti i domini non preparati in precedenza per Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2. Gli amministratori devono eseguire GPPrep una sola volta nella cronologia di un dominio, non a ogni aggiornamento. Non viene eseguito automaticamente da adprep perché se hai già impostata autorizzazioni personalizzate adeguate, tutto il contenuto SYSVOL nuovamente replicato in tutti i controller di dominio.|  
  
|Problema|Dal supporto di installazione non riesce a verificare quando punta a un percorso UNC|  
|---------|------------------------------------------------------------------|  
|Sintomi|Errore restituito:<br /><br />Codice - Impossibile convalidare il percorso di supporto. Eccezione chiamata "GetDatabaseInfo" con "2" argomenti. La cartella non è valida.|  
|Risoluzione e note|È necessario archiviare i file IFM in un disco locale, non un percorso UNC remoto. Questo blocco intenzionale impedisce l'innalzamento di livello del server parziale a causa di un'interruzione di rete.|  
  
|Problema|Avviso di delega DNS visualizzato due volte durante la promozione del controller di dominio|  
|---------|-------------------------------------------------------------------------|  
|Sintomi|Avviso restituito *due volte* l'innalzamento di livello usando ADDSDeployment di Windows PowerShell:<br /><br />Codice - "Impossibile creare una delega per questo server DNS perché la zona padre autorevole non è possibile trovare o non viene eseguito Windows DNS server. Se sta effettuando l'integrazione con un'infrastruttura DNS esistente, è necessario creare manualmente una delega a questo server DNS nella zona padre per garantire una risoluzione nomi affidabile dall'esterno al dominio. In caso contrario, è richiesta alcuna azione."|  
|Risoluzione e note|Ignorare. ADDSDeployment di Windows PowerShell Visualizza l'avviso una volta durante il controllo dei prerequisiti e quindi nuovamente durante la configurazione del controller di dominio. Se non si desidera configurare la delega DNS, utilizzare l'argomento:<br /><br />Codice - - creatednsdelegation: $false<br /><br />Eseguire *non* ignorare i controlli dei prerequisiti per eliminare il messaggio|  
  
|Problema|Specificare le credenziali non appartenenti al dominio o UPN durante la configurazione vengono restituiti errori ingannevoli|  
|---------|--------------------------------------------------------------------------------------------|  
|Sintomi|Server Manager restituisce l'errore:<br /><br />Codice - eccezione chiamata "DNSOption" con "6" argomenti<br /><br />ADDSDeployment di Windows PowerShell restituisce l'errore:<br /><br />Codice - verifica delle autorizzazioni utente non è riuscito. È necessario specificare il nome del dominio a cui appartiene l'account utente.|  
|Risoluzione e note|Verificare di avere specificato credenziali di dominio valido in forma di **dominio\utente**.|  
  
|Problema|Rimuovere il ruolo DirectoryServices-DomainController con Dism.exe lead server risulta non avviabile|  
|---------|---------------------------------------------------------------------------------------------------|  
|Sintomi|Se si usa Dism.exe per rimuovere il ruolo di dominio Active Directory prima di abbassare di livello un controller di dominio normalmente, il server non viene avviato normalmente e viene visualizzato l'errore:<br /><br />Codice - stato: 0x000000000<br />Info: Verificato un errore imprevisto.|  
|Risoluzione e note|Avvio in modalità ripristino servizi Directory utilizzando *MAIUSC + F8*. Aggiungere nuovamente il ruolo di dominio Active Directory e quindi abbassamento di livello forzato il controller di dominio. In alternativa, è possibile ripristinare lo stato del sistema dal backup. Non usare Dism.exe per la rimozione del ruolo di dominio Active Directory; l'utilità non è in grado di controller di dominio.|  
  
|Problema|Installare una nuova foresta non riesce quando ForestMode è impostato su Win2012|  
|---------|--------------------------------------------------------------------|  
|Sintomi|Innalzamento di livello con ADDSDeployment di Windows PowerShell restituisce l'errore:<br /><br />Codice - Test.VerifyDcPromoCore.DCPromo.General.74<br /><br />Verifica dei prerequisiti per la promozione del Controller di dominio non è riuscito. Il livello di funzionalità del dominio specificato non è valido|  
|Risoluzione e note|Non specificare una modalità di funzionalità foresta di Win2012 senza *anche* specificando una modalità di funzionalità del dominio di Win2012. Ecco un esempio che funziona senza errori:<br /><br />Codice - - forestmode Win2012 - domainmode Win2012]|  
  
|||  
|-|-|  
|Problema|Facendo clic su verifica nell'installazione dall'area di selezione Media sembra che non esegue alcuna operazione|  
|Sintomi|Quando si specifica un percorso a una cartella IFM, scegliendo il **verifica** pulsante mai restituisce un messaggio e sembra che eseguire alcuna operazione.|  
|Risoluzione e note|Il **verifica** pulsante restituisce solo gli errori se sono presenti problemi. In caso contrario, rendendo il **Avanti** pulsante selezionabile se è stato specificato un percorso IFM. È necessario fare clic **verifica** per procedere se è stato selezionato IFM.|  
  
|||  
|-|-|  
|Problema|L'abbassamento di livello con Server Manager non fornisce feedback fino al completamento.|  
|Sintomi|Quando si utilizza Server Manager per rimuovere il ruolo di dominio Active Directory e abbassare di livello un controller di dominio, non esiste alcun feedback continuo assegnato fino a quando non è completata o l'abbassamento di livello ha esito negativo.|  
|Risoluzione e note|Si tratta di una limitazione di Server Manager. Per il feedback, utilizzare il cmdlet ADDSDeployment di Windows PowerShell:<br /><br />Codice - addsdomaincontroller disinstallare|  
  
|||  
|-|-|  
|Problema|Installazione da supporto verificare non rileva che il supporto di controller di dominio fornito per i controller di dominio scrivibile o viceversa.|  
|Sintomi|L'innalzamento di livello un nuovo controller di dominio tramite IFM e fornire supporto non corretto per l'installazione da supporto, ad esempio il supporto di controller di dominio per un controller di dominio scrivibile o un supporto di scrittura per un RODC - il pulsante verifica non restituisce un errore. In un secondo momento, l'innalzamento di livello non riesce con errore:<br /><br />Codice - si è verificato un errore durante il tentativo di configurare questo computer come controller di dominio. <br />Impossibile avviare la promozione da supporti di installazione di un controller di dominio di sola lettura del database di origine specificato non è consentito. Solo i database da altri lettura possono essere utilizzati per la promozione di IFM di un controller di dominio.|  
|Risoluzione e note|Verifica convalida solo l'integrità complessiva di IFM. Non fornire il tipo IFM errato per un server. Riavviare il server prima di tentare nuovamente con il supporto corretto innalzamento di livello.|  
  
|||  
|-|-|  
|Problema|L'innalzamento di livello un controller di dominio in un account computer creato in precedenza non riesce|  
|Sintomi|Quando si usa ADDSDeployment di Windows PowerShell per promuovere un nuovo controller di dominio con un account computer preconfigurato, errore:<br /><br />Codice - set di parametri non può essere risolto utilizzando denominato parametri specificato.    <br />Argomento non valido: ParameterBindingException<br />    + FullyQualifiedErrorId: AmbiguousParameterSet,Microsoft.DirectoryServices.Deployment.PowerShell.Commands.Install|  
|Risoluzione e note|Non fornire parametri già definiti in un account di controller di dominio creato in precedenza. Queste includono:<br /><br />Codice - - readonlyreplica<br />-installdns<br />-donotconfigureglobalcatalog<br />-sitename<br />-installdns|  
  
|||  
|-|-|  
|Problema|Seleziona/deseleziona "Riavvia ogni server di destinazione automaticamente se necessario" non ha alcun effetto|  
|Sintomi|Quando si seleziona (o non si seleziona) l'opzione di Server Manager **riavvia automaticamente i server di destinazione se necessario** whendemoting un controller di dominio tramite rimozione del ruolo, il server viene sempre riavviato, indipendentemente dall'opzione selezionata.|  
|Risoluzione e note|Questa condizione è voluta. Il processo di abbassamento di livello un riavvio del server indipendentemente da questa impostazione.|  
  
|||  
|-|-|  
|Problema|Dcpromo.log Mostra "[errore] l'impostazione della sicurezza nel file server non è riuscita con 2"|  
|Sintomi|Abbassamento di livello un controller di dominio viene completato senza errori, ma se si esamina il registro dcpromo viene visualizzato l'errore:<br /><br />Codice - [errore] l'impostazione di sicurezza nel file server non riuscita con 2|  
|Risoluzione e note|Ignorarlo, l'errore è previsto e cosmetico.|  
  
|||  
|-|-|  
|Problema|Controllo dei prerequisiti di adprep non riesce con errore "Impossibile eseguire la verifica dei conflitti dello schema di Exchange"|  
|Sintomi|Quando si tenta di alzare di livello un controller di dominio Windows Server 2012 in un esistente di Windows Server 2003, Windows Server 2008 o foresta Windows Server 2008 R2, controllo dei prerequisiti non riesce con errore:<br /><br />Codice - durante la verifica dei prerequisiti per la preparazione di Active Directory non è riuscito. Impossibile eseguire la verifica dei conflitti dello schema di Exchange per dominio * <domain name> * (eccezione: il server RPC non è disponibile)<br /><br />Il file adprep.log viene visualizzato l'errore:<br /><br />Codice - Adprep Impossibile recuperare i dati dal server di*<domain controller>*<br /><br />tramite Strumentazione gestione Windows (WMI).|  
|Risoluzione e note|Il nuovo controller di dominio non può accedere a WMI tramite protocolli DCOM/RPC con il controller di dominio esistenti. La data, sono state tre cause per questo:<br /><br />-Un firewall regola blocca l'accesso ai controller di dominio esistente<br /><br />-L'account del servizio di rete è assente dal "Accesso come servizio" privilegio (SeServiceLogonRight) nei controller di dominio esistente<br /><br />-NTLM viene disabilitato nei controller di dominio utilizzando criteri di sicurezza descritti [Introduzione alla restrizione dell'autenticazione NTLM](https://technet.microsoft.com/library/dd560653(WS.10).aspx)|  
  
|||  
|-|-|  
|Problema|Creazione di una nuova foresta di Active Directory sempre Mostra avviso DNS|  
|Sintomi|Quando la creazione di una nuova foresta di Active Directory e la creazione della zona DNS nel nuovo controller di dominio per se stesso, puoi sempre visualizzato il messaggio di avviso:<br /><br />Codice - errore è stato rilevato nella configurazione DNS. <br />Nessuno dei server DNS utilizzati dal computer ha risposto entro l'intervallo di timeout.<br />(codice di errore 0x000005B4 "ERROR_TIMEOUT")|  
|Risoluzione e note|Ignorare. Questo avviso è intenzionale per il primo controller di dominio nel dominio radice di una nuova foresta, nel caso in cui si intenda puntare a un server DNS esistente e la zona.|  
  
|||  
|-|-|  
|Problema|Argomento - whatif di Windows PowerShell restituisce informazioni di server DNS non corrette|  
|Sintomi|Se si utilizza il **- whatif** argomento durante la configurazione di un controller di dominio con implicito o esplicito **- installdns: $true**, l'output risultante Mostra:<br /><br />Codice - "Server DNS: No"|  
|Risoluzione e note|Ignorare. DNS viene installato e configurato correttamente.|  
  
|||  
|-|-|  
|Problema|Dopo l'innalzamento di livello, accesso ha esito negativo con "memoria insufficiente è disponibile per eseguire il comando"|  
|Sintomi|Dopo aver alzare di livello un nuovo controller di dominio e quindi disconnettersi e tentano di accedere in modo interattivo, viene visualizzato l'errore:<br /><br />Codice - memoria insufficiente è disponibile per eseguire il comando|  
|Risoluzione e note|Il controller di dominio non è stato riavviato dopo l'innalzamento di livello, a causa di un errore o perché è stato specificato l'argomento di ADDSDeployment di Windows PowerShell **- norebootoncompletion**. Riavviare il controller di dominio.|  
  
|||  
|-|-|  
|Problema|Il pulsante Avanti non è disponibile nella pagina Opzioni Controller di dominio|  
|Sintomi|Anche se è stata impostata una password, il **Avanti** pulsante il **opzioni Controller di dominio** pagina in Server Manager non è disponibile. Nessun sito elencato nel **nome sito** menu.|  
|Risoluzione e note|Si dispongono di più siti di Active Directory e almeno uno mancano le subnet; Questo futuro controller di dominio appartiene a una di queste subnet. È necessario selezionare manualmente la subnet dal menu a discesa Nome sito. È consigliabile consultare anche tutti i siti di Active Directory utilizzando DSSITE. MSC o usare il comando di Windows PowerShell seguente per trovare tutti i siti mancano le subnet:<br /><br />Codice - get-adreplicationsite-filtro *-subnet proprietà & #124; WHERE-object {! $_.subnets - eq "\ *"} & #124; nome di Format-table|  
  
|||  
|-|-|  
|Problema|Operazione di innalzamento o abbassamento di livello ha esito negativo con il messaggio "non può essere"avviare il servizio|  
|Sintomi|Se si tenta di innalzamento di livello, abbassamento di livello o clonazione di un controller di dominio viene visualizzato l'errore:<br /><br />Codice - Impossibile avviare il servizio, sia perché è disabilitato o sono disponibili periferiche abilitate associate"(0x80070422)<br /><br />L'errore potrebbe essere interattivo, un evento, o scritto in un registro, ad esempio dcpromoui.log o dcpromo.log|  
|Risoluzione e note|Il servizio ruolo Server DS (DsRoleSvc) è disabilitato. Per impostazione predefinita, questo servizio viene installato durante l'installazione del ruolo di dominio Active Directory e a un tipo di avvio manuale. Non disabilitare questo servizio. Reimpostarlo su manuale e consentire le operazioni del ruolo DS di avviarlo e arrestarlo su richiesta. Questo comportamento è da progettazione.|  
  
|||  
|-|-|  
|Problema|Server Manager ancora avvisa che è necessario alzare di livello controller di dominio|  
|Sintomi|Se si alzare di livello un controller di dominio con l'obsoleti dcpromo.exe / installazione automatica o aggiornare un controller di dominio Windows Server 2008 R2 esistente sul posto a Windows Server 2012, Server Manager visualizza ancora l'attività di configurazione post-distribuzione **promuovere il server a un controller di dominio**.|  
|Risoluzione e note|Fare clic sul collegamento di avviso post-distribuzione e non verrà più visualizzato il messaggio in modo definitivo. Questo comportamento è cosmetico e previsto.|  
  
|||  
|-|-|  
|Problema|Script di distribuzione di Server Manager manca l'installazione del ruolo|  
|Sintomi|Se si alzare di livello un controller di dominio utilizzando Server Manager e salvare lo script di distribuzione di Windows PowerShell, non include il cmdlet di installazione di ruoli e gli argomenti (install-windowsfeature-nome ad-domain-services - includemanagementtools). Senza il ruolo, il controller di dominio non può essere configurato.|  
|Risoluzione e note|Aggiungere manualmente il cmdlet e gli argomenti a qualsiasi script. Questo comportamento è previsto e da progettazione.|  
  
|||  
|-|-|  
|Problema|Script di distribuzione di Server Manager non è assegnato il nome PS1|  
|Sintomi|Se si alzare di livello un controller di dominio utilizzando Server Manager e salvare lo script di distribuzione di Windows PowerShell, il file è denominato con un nome temporaneo casuale e non come file PS1.|  
|Risoluzione e note|Rinominare manualmente il file. Questo comportamento è previsto e da progettazione.|  
  
|Problema|Dcpromo /Unattend consente livelli di funzionalità non supportati|  
|-|-|  
|Sintomi|Se alzare di livello un controller di dominio usando dcpromo /Unattend con il file di risposte di esempio seguente:<br /><br />Codice-<br /><br />[DCInstall]<br />Il nuovo dominio = insieme di strutture<br /><br />ReplicaOrNewDomain = dominio<br /><br />NewDomainDNSName=corp.contoso.com<br /><br />SafeModeAdminPassword =Safepassword@6<br /><br />DomainNetbiosName = corp<br /><br />DNSOnNetwork = Sì<br /><br />AutoConfigDNS = Sì<br /><br />RebootOnSuccess = NoAndNoPromptEither<br /><br />RebootOnCompletion = No<br /><br />*DomainLevel = 0*<br /><br />*ForestLevel = 0*<br /><br />L'innalzamento di livello ha esito negativo con errori nel file dcpromoui.log seguenti:<br /><br />Codice - dcpromoui 0089 EA4.5B8 13:31:50.783 DomainLevel CArgumentsSpec::ValidateArgument invio<br /><br />Dcpromoui EA4.5B8 008A 13:31:50.783 valore per DomainLevel è 0<br /><br />Dcpromoui EA4.5B8 008B 13:31:50.783 codice di uscita è 77<br /><br />Dcpromoui EA4.5B8 008C 13:31:50.783 l'argomento specificato non è valido.<br /><br />Dcpromoui EA4.5B8 chiusura 13:31:50.783 D 008 registro<br /><br />Dcpromoui 0032 EA4.5B8 13:31:50.830 codice di uscita è 77<br /><br />Livello 0 è Windows 2000, che non è supportato in Windows Server 2012.|  
|Risoluzione e note|Non utilizzare il dcpromo obsoleto / installazione automatica e comprendere che consente di specificare impostazioni non valide che successivamente avranno esito negativo. Questo comportamento è previsto e da progettazione.|  

|Problema|L'innalzamento di livello "blocchi" durante la creazione dell'oggetto impostazioni NTDS, non viene mai completata|  
|-|-|  
|Sintomi|Se si innalza di livello una replica controller di dominio o controller di dominio, l'innalzamento di livello raggiunge "creazione dell'oggetto impostazioni NTDS" e non continua o viene completato. I registri arrestare anche l'aggiornamento.|  
|Risoluzione e note|Si tratta di un problema noto causato dall'immissione delle credenziali dell'account amministratore locale predefinito con una password corrispondente all'account amministratore di dominio predefinito. Ciò causa un errore verso il basso nel motore di installazione dei componenti di base che non di errore, ma attende in modo indefinito (quasi a ciclo continuo). È previsto, sebbene indesiderato - comportamento.<br /><br />Per correggere il server:<br /><br />1. riavviare.<br /><br />1. in Active Directory, eliminare l'account del computer del server membro (non ancora sarà un account di controller di dominio)<br /><br />1. in tale server, forzatamente separarlo dal dominio<br /><br />1. in tale server, rimuovere il ruolo di dominio Active Directory.<br /><br />1. riavvio<br /><br />1. aggiungere nuovamente il dominio Active Directory ruolo e ripetere l'innalzamento di livello, che per garantire sempre il ***dominio\admin*** formattato le credenziali per l'innalzamento di livello del controller di dominio e non solo l'account amministratore locale predefinito|  
