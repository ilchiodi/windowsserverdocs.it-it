---
ms.assetid: e4c31187-f15f-410b-bb79-8d63e2f2b421
title: Aggiornare controller di dominio a Windows Server 2012 R2 e Windows Server 2012
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5291c30baf3aff4a9b3540f0bbaa5193aba176aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369595"
---
# <a name="upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012"></a>Aggiornare controller di dominio a Windows Server 2012 R2 e Windows Server 2012

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono fornite informazioni complementari sulle Active Directory Domain Services in Windows Server 2012 R2 e Windows Server 2012 e viene illustrato il processo per l'aggiornamento dei controller di dominio da Windows Server 2008 o Windows Server 2008 R2.  
  
## <a name="BKMK_UpgradeWorkflow"></a>Passaggi di aggiornamento del controller di dominio  
Il metodo consigliato per aggiornare un dominio è alzare di livello i controller di dominio che eseguono versioni più recenti di Windows Server e abbassare di livello i controller di dominio precedenti in base alle necessità. Questo metodo è preferibile all'aggiornamento del sistema operativo di un controller di dominio esistente. Questo elenco include i passaggi generali da seguire prima di alzare di livello un controller di dominio che esegue una versione più recente di Windows Server:  
  
1. Verificare che il server di destinazione soddisfi i [requisiti di sistema](https://technet.microsoft.com/library/dn303418.aspx).  
2. Verificare [compatibilità delle applicazioni](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat).  
3. Verificare le impostazioni di sicurezza Per altre informazioni, vedere [Caratteristiche deprecate e modifiche del comportamento relative a Servizi di dominio Active Directory in Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures) e [Secure default settings in Windows Server 2008 e Windows Server 2008 R2](https://technet.microsoft.com/library/upgrade-domain-controllers-to-windows-server-2008-r2(WS.10).aspx#BKMK_SecureDefault).  
4. Controllare la connettività al server di destinazione dal computer in cui si prevede di eseguire l'installazione.  
5. Controllare la disponibilità dei ruoli master operazione necessari:  

   - Per installare il primo controller di dominio che esegue Windows Server 2012 in un dominio e una foresta esistenti, il computer in cui si esegue l'installazione necessita della connettività al master schema per eseguire adprep/forestprep e il master infrastrutture per eseguire adprep/ DomainPrep.  
   - Per installare il primo controller di dominio in un dominio in cui lo schema della foresta è già esteso è necessaria solo la connettività al master infrastrutture.  
   - Per installare o rimuovere un dominio in una foresta esistente è necessaria la connettività al master per la denominazione dei domini.  
   - Ogni installazione di controller di dominio richiede inoltre la connettività al master RID.  
   - Se si sta installando il primo controller di dominio di sola lettura in una foresta esistente è necessaria la connettività al master infrastrutture per ogni partizione di directory applicativa, altrimenti detta contesto dei nomi non di dominio.  

6. Assicurarsi di fornire le credenziali necessarie per eseguire l'installazione di Servizi di dominio Active Directory.  

   |Operazione di installazione|Requisiti relativi alle credenziali|  
   |-----------------------|---------------------------|  
   |Installare una nuova foresta|Amministratore locale nel server di destinazione|  
   |Installare un nuovo dominio in una foresta esistente|Enterprise Admins|  
   |Installare un controller di dominio aggiuntivo in un dominio esistente|Domain Admins|  
   |Eseguire adprep /forestprep|Schema Admins, Enterprise Admins e Domain Admins|  
   |Eseguire adprep /domainprep|Domain Admins|  
   |Eseguire adprep /domainprep /gpprep|Domain Admins|  
   |Eseguire adprep /rodcprep|Enterprise Admins|  

   È possibile delegare le autorizzazioni per installare Servizi di dominio Active Directory. Per altre informazioni, vedere [Attività di gestione dell'installazione](https://technet.microsoft.com/library/cc773327(WS.10).aspx).  

Istruzioni dettagliate per eseguire l'innalzamento di livello dei controller di dominio di Windows Server 2012 nuovi e di replica utilizzando i cmdlet di Windows PowerShell e Server Manager sono disponibili consultando i seguenti collegamenti:  

- [Installare Active Directory Domain Services (livello 100)](https://technet.microsoft.com/library/hh472162.aspx)  
- [Installare una nuova foresta di Active Directory Windows Server 2012 (livello 200)](https://technet.microsoft.com/library/jj574166.aspx)  
- [Installare un controller di dominio Windows Server 2012 di replica in un dominio esistente (livello 200)](https://technet.microsoft.com/library/jj574134.aspx)  
- [Installare un nuovo Windows Server 2012 Active Directory dominio figlio o albero (livello 200)](https://technet.microsoft.com/library/jj574105.aspx)  
- [Installare Windows Server 2012 Active Directory controller di dominio di sola lettura (RODC) (livello 200)](https://technet.microsoft.com/library/jj574152.aspx)  
- [Guida dettagliata per la configurazione del controller di dominio Windows Server 2012 (en-US)](https://social.technet.microsoft.com/wiki/contents/articles/12370.step-by-step-guide-for-setting-up-windows-server-2012-domain-controller-en-us.aspx)  

## <a name="windows-update-considerations"></a>Considerazioni Windows Update

Prima del rilascio di Windows 8, Windows Update gestiva la pianificazione interna per verificare la disponibilità di aggiornamenti e per scaricarli e installarli. L'agente di Windows Update doveva essere sempre in esecuzione in background, consumando memoria e altre risorse di sistema.  
  
In Windows 8 e Windows Server 2012 è stata introdotta la nuova funzionalità [Manutenzione automatica](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx). Manutenzione automatica consolida più funzionalità diverse, ciascuna usata per gestire la pianificazione e la logica di esecuzione. Questo consolidamento consente a tutti questi componenti di usare molte meno risorse di sistema, di operare in modo coerente, di rispettare il nuovo stato [Standby connesso](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) per i nuovi tipi di dispositivo e di consumare meno batteria sui dispositivi portatili.  
  
Poiché Windows Update fa parte di Manutenzione automatica in Windows 8 e Windows Server 2012, la pianificazione interna per l'impostazione di giorno e ora per installare gli aggiornamenti non è più operativa. Per garantire un riavvio del sistema coerente e prevedibile per tutti i dispositivi e i computer di una grande impresa, inclusi quelli che eseguono Windows 8 e Windows Server 2012, vedere l'articolo della Microsoft KB [2885694](https://support.microsoft.com/kb/2885694) (o vedere il rollup cumulativo di ottobre 2013 [2883201](https://support.microsoft.com/kb/2883201)), quindi configurare le impostazioni dei criteri descritte nel post del blog su WSUS relativo a come [consentire un'esperienza Windows Update più prevedibile per Windows 8 e Windows Server 2012 (KB 2885694)](http://blogs.technet.com/b/wsus/archive/2013/10/08/enabling-a-more-predictable-windows-update-experience-for-windows-8-and-windows-server-2012-kb-2885694.aspx).  

## <a name="BKMK_NewWS2012R2"></a>Quali sono le novità di servizi di dominio Active Directory in Windows Server 2012 R2?

Nella tabella seguente sono riepilogate le nuove caratteristiche di Servizi di dominio Active Directory in Windows Server 2012 R2, insieme a un collegamento a informazioni più dettagliate, ove disponibili. Per informazioni più dettagliate su alcune caratteristiche e relativi requisiti, vedere [Novità di Active Directory in Windows Server 2012 R2](https://technet.microsoft.com/library/dn268294.aspx).  

|Funzionalità|Descrizione|  
|-----------|---------------|  
|[Workplace Join](https://technet.microsoft.com/library/dn280945.aspx)|Consente agli Information Worker di aggiungere i dispositivi personali alla società per accedere alle risorse e ai servizi aziendali.|  
|[Proxy applicazione Web](https://technet.microsoft.com/library/dn280942.aspx)|Consente l'accesso all'applicazione Web con un nuovo servizio ruolo Accesso remoto.|  
|[Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx)|In ADFS sono disponibili una distribuzione semplificata e miglioramenti che consentono agli utenti di accedere alle risorse dai dispositivi personali e ai reparti IT di gestire il controllo di accesso.|  
|[Unicità di SPN e UPN](https://technet.microsoft.com/library/dn535779.aspx)|I controller di dominio che eseguono Windows Server 2012 R2 bloccano la creazione di nomi dell'entità servizio (SPN) e di nomi dell'entità utente (UPN).|  
|[Accesso automatico al riavvio Winlogon](https://technet.microsoft.com/library/dn535772.aspx)|Consente alle applicazioni schermata di blocco di essere riavviate e disponibili sui dispositivi Windows 8.1.|  
|[Attestazione chiave TPM](https://technet.microsoft.com/library/dn581921.aspx)|Consente alle CA di attestare a livello di crittografia in un certificato rilasciato che la chiave privata del richiedente del certificato è effettivamente protetta da un Trusted Platform Module (TPM).|  
|[Gestione e protezione delle credenziali](https://technet.microsoft.com/library/dn408190.aspx)|Nuovi controlli di protezione delle credenziali e autenticazione dei domini per ridurre il furto di credenziali.|  
|[Deprecazione del servizio Replica file (FRS)](https://technet.microsoft.com/library/dn535775.aspx)|Anche il livello di funzionalità del dominio Windows Server 2003 è deprecato perché a livello di funzionalità FRS viene usato per replicare SYSVOL. Pertanto, quando si crea un nuovo dominio su un server che esegue Windows Server 2012 R2, il livello di funzionalità del dominio deve essere Windows Server 2008 o versione successiva. È comunque possibile aggiungere un controller di dominio che esegue Windows Server 2012 R2 a un dominio esistente con un livello di funzionalità del dominio di Windows Server 2003. non è possibile creare un nuovo dominio a tale livello.|  
|[Nuovi livelli di funzionalità di domini e foreste](../active-directory-functional-levels.md)|Esistono nuovi livelli di funzionalità per Windows Server 2012 R2. Nuove caratteristiche sono disponibili nel livello di funzionalità del dominio di Windows Server 2012 R2.|  
|[Modifiche Query Optimizer LDAP](https://technet.microsoft.com/library/dn535775.aspx)|Miglioramenti delle prestazioni nell'efficienza della ricerca LDAP e nei tempi di ricerca LDAP delle query complesse.|  
|[Miglioramenti dell'evento 1644](https://technet.microsoft.com/library/dn535775.aspx)|Le statistiche dei risultati di ricerca LDAP sono state aggiunte all'ID evento 1644 come supporto per la risoluzione dei problemi.|  
|[Miglioramento della velocità effettiva della replica Active Directory](https://technet.microsoft.com/library/dn535775.aspx)|Regola la velocità effettiva massima della replica AD facendola passare da 40 Mbps a circa 600 Mbps|  

## <a name="BKMK_WhatsNewAD"></a>Quali sono le novità di servizi di dominio Active Directory in Windows Server 2012?

Nella tabella seguente sono riepilogate le nuove caratteristiche di Servizi di dominio Active Directory in Windows Server 2012, insieme a un collegamento a informazioni più dettagliate, ove disponibili. Per una spiegazione più dettagliata di alcune funzionalità, inclusi i relativi requisiti, vedere Novità di [Active Directory Domain Services (ad DS)](https://technet.microsoft.com/library/hh831477.aspx).  
  
|Funzionalità|Descrizione|  
|-----------|---------------|  
|Attivazione basata su Active Directory, vedere [Panoramica dell'attivazione di contratti multilicenza](https://technet.microsoft.com/library/hh831612.aspx)|Semplifica l'attività di configurazione della distribuzione e della gestione dei contratti multilicenza del software.|  
|[Active Directory Federation Services (AD FS)](https://technet.microsoft.com/library/hh831502.aspx)|Aggiunge installazione dei ruoli attraverso Server Manager, configurazione semplificata dei trust, gestione automatica dei trust, supporto del protocollo SAML e altro ancora.|  
|Eventi di cancellazione di pagina persi in Active Directory|Evento NTDS ISAM 530 con errore Jet -1119 caricato per rilevare eventi di cancellazione pagina persi nei database di Active Directory.|  
|[Interfaccia utente del Cestino Active Directory](https://technet.microsoft.com/library/hh831702.aspx#ad_recycle_bin_mgmt)|Il Centro di amministrazione di Active Directory aggiunge una caratteristica di gestione del Cestino con interfaccia grafica introdotta in origine in Windows Server 2008 R2.|  
|[Active Directory i cmdlet di Windows PowerShell per la replica e la topologia](https://technet.microsoft.com/library/hh831757.aspx)|Supporta la creazione e la gestione di siti, collegamenti di sito, oggetti connessione e altro di Active Directory tramite Windows PowerShell.|  
|[Controllo dinamico degli accessi](https://technet.microsoft.com/library/hh831717.aspx)|Nuova piattaforma di autorizzazione basata su attestazioni che migliora il modello legacy di controllo degli accessi.|  
|[Interfaccia utente per i criteri granulari per le password](https://technet.microsoft.com/library/hh831702.aspx#fine_grained_pswd_policy_mgmt)|Il Centro di amministrazione di Active Directory aggiunge il supporto per l'interfaccia grafica per la creazione, la modifica e l'assegnazione degli oggetti Impostazioni password aggiunti in origine in Windows Server 2008.|  
|[Account del servizio gestito del gruppo (gMSA)](https://technet.microsoft.com/library/hh831782.aspx)|Una nuova entità di sicurezza nota come account del servizio gestito di gruppo. I servizi in esecuzione su più host possono essere eseguiti nello stesso account del servizio gestito di gruppo.|  
|[Aggiunta a un dominio offline di DirectAccess](https://technet.microsoft.com/library/jj574150.aspx)|Estende l'aggiunta al dominio offline mediante l'inclusione dei prerequisiti di DirectAccess.|  
|[Distribuzione rapida tramite clonazione del controller di dominio virtuale (DC)](https://technet.microsoft.com/library/hh831734.aspx#virtualized_dc_cloning)|È possibile distribuire rapidamente i controller di dominio virtualizzati attraverso la clonazione dei controller di dominio virtuali esistenti mediante i cmdlet di Windows PowerShell.|  
|[Modifiche al pool di RID](https://technet.microsoft.com/library/jj574229.aspx)|Aggiunge nuovi eventi di monitoraggio e quote come protezione contro un consumo eccessivo del pool RID globale. Facoltativamente, raddoppia le dimensioni del pool RID globale se il pool originale si esaurisce.|  
|Servizio tempo di protezione|Aumenta la sicurezza in W32tm attraverso la rimozione dei segreti dalle trasmissioni, la rimozione delle funzioni hash MD5 e la necessità per il server di eseguire l'autenticazione con i client orario di Windows 8|  
|[Protezione da rollback degli USN per i controller di dominio virtualizzati](https://technet.microsoft.com/library/hh831734.aspx#safe_virt_dc)|Il ripristino accidentale dei backup delle istantanee dei controller di dominio virtualizzati non provoca più il rollback degli USN|  
|[Visualizzatore della cronologia di Windows PowerShell](https://technet.microsoft.com/library/hh831702.aspx#windows_powershell_history_viewer)|Consente agli amministratori di visualizzare i comandi di Windows PowerShell eseguiti durante l'utilizzo di Centro di amministrazione di Active Directory.|  
  
### <a name="BKMK_"></a>Manutenzione automatica e modifiche al comportamento di riavvio dopo l'applicazione degli aggiornamenti da Windows Update

Prima del rilascio di Windows 8, Windows Update gestiva la pianificazione interna per verificare la disponibilità di aggiornamenti e per scaricarli e installarli. L'agente di Windows Update doveva essere sempre in esecuzione in background, consumando memoria e altre risorse di sistema.  
  
In Windows 8 e Windows Server 2012 è stata introdotta la nuova funzionalità [Manutenzione automatica](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx). Manutenzione automatica consolida più funzionalità diverse, ciascuna usata per gestire la pianificazione e la logica di esecuzione. Questo consolidamento consente a tutti questi componenti di usare molte meno risorse di sistema, di operare in modo coerente, di rispettare il nuovo stato [Standby connesso](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) per i nuovi tipi di dispositivo e di consumare meno batteria sui dispositivi portatili.  
  
Poiché Windows Update fa parte di Manutenzione automatica in Windows 8 e Windows Server 2012, la pianificazione interna per l'impostazione di giorno e ora per installare gli aggiornamenti non è più operativa. Per garantire un riavvio del sistema coerente e prevedibile per tutti i dispositivi e i computer di una grande impresa, inclusi quelli che eseguono Windows 8 e Windows Server 2012, è possibile configurare le impostazioni di Criteri di gruppo seguenti:  

- **Configurazione computer | Criteri | Modelli amministrativi | Componenti di Windows | Windows Update | Configurare Aggiornamenti automatici**  
- **Configurazione computer | Criteri | Modelli amministrativi | Componenti di Windows | Windows Update | Nessun riavvio automatico con utenti connessi**  
- **Configurazione computer | Criteri | Modelli amministrativi | Componenti di Windows | Utilità di pianificazione di manutenzione | Ritardo casuale manutenzione**  

Nella tabella seguente sono elencati alcuni esempi di configurazione di queste impostazioni per consentire il riavvio del sistema desiderato.  

|||  
|-|-|  
|**Scenario**|**Configurazioni consigliate**|  
|**Gestito da WSUS**<br /><br />-Installa gli aggiornamenti una volta alla settimana<br />-Riavvio venerdì alle 23.00|Impostare i computer sull'installazione automatica, impedire il riavvio automatico fino all'ora desiderata<br /><br />**Criterio**: Configura Aggiornamenti automatici (attivato)<br /><br />Configura aggiornamento automatico: 4 - download automatico e pianificazione dell'installazione<br /><br />**Criterio**: Nessun riavvio automatico con utenti connessi (disabilitato)<br /><br />**Scadenze WSUS**: impostare su venerdì alle 23|  
|**Gestito da WSUS**<br /><br />-Scagliona le installazioni tra ore/giorni diversi|Impostare i gruppi di destinazione per gruppi diversi di computer da aggiornare insieme<br /><br />Usare i passaggi dello scenario precedente<br /><br />Impostare scadenze diverse per gruppi di destinazione diversi|  
|**Non gestito da WSUS-nessun supporto per le scadenze**<br /><br />-Scaglionare le installazioni in momenti diversi|**Criterio**: Configura Aggiornamenti automatici (attivato)<br /><br />Configura aggiornamento automatico: 4 - download automatico e pianificazione dell'installazione<br /><br />**Chiave del registro di sistema:** Abilitare la chiave del registro di sistema descritta nell'articolo [2835627](https://support.microsoft.com/kb/2835627) della Microsoft Knowledge Knowledge<br /><br />**Criterio:** Ritardo casuale manutenzione automatica (attivato)<br /><br />Impostare **Ritardo casuale manutenzione normale** su PT6H per un ritardo casuale di 6 ore per implementare il comportamento seguente:<br /><br />-Gli aggiornamenti vengono installati al momento della manutenzione configurata più un ritardo casuale<br /><br />-Il riavvio per ogni computer verrà eseguita esattamente tre giorni dopo<br /><br />In alternativa, impostare un'ora di manutenzione diversa per ogni gruppo di computer|  

Per altre informazioni sui motivi per cui il team tecnico Windows ha implementato queste modifiche, vedere [Riduzione dei riavvii dopo l'aggiornamento automatico in Windows Update](http://blogs.msdn.com/b/b8/archive/2011/11/14/minimizing-restarts-after-automatic-updating-in-windows-update.aspx).  

## <a name="BKMK_InstallationChanges"></a>Modifiche all'installazione del ruolo server Servizi di dominio Active Directory

A partire da Windows Server 2003, fino a Windows Server 2008 R2, prima di eseguire l'Installazione guidata di Active Directory, Dcpromo.exe, veniva eseguita la versione x86 o X64 dello strumento da riga di comando Adprep.exe. Inoltre esistevano varianti facoltative di Dcpromo.exe per l'installazione da supporto o in caso di installazione automatica.  
  
A partire da Windows Server 2012, le installazioni da riga di comando vengono eseguite mediante il modulo ADDSDeployment in Windows PowerShell. Le promozioni con interfaccia grafica vengono eseguite in Server Manager mediante una Configurazione guidata Servizi di dominio Active Directory completamente nuova. Per semplificare il processo di installazione, nell'installazione dei Servizi di dominio Active Directory è stato integrato ADPREP, che viene eseguito automaticamente in caso di necessità. La configurazione guidata servizi di dominio Active Directory basata su Windows PowerShell è destinata automaticamente ai ruoli master schema e infrastruttura nei domini in cui vengono aggiunti i controller di dominio, quindi esegue in remoto i comandi ADPREP necessari sui controller di dominio pertinenti.  
  
I controlli dei prerequisiti nell'Installazione guidata Servizi di dominio Active Directory identifica i potenziali errori prima dell'inizio dell'installazione. È possibile correggere le condizioni di errore al fine di eliminare il rischio di eseguire un aggiornamento parziale. L'installazione guidata, inoltre, esporta uno script di Windows PowerShell contenente tutte le opzioni specificate durante l'installazione grafica  
  
Nel loro insieme, le modifiche all'installazione dei Servizi di dominio Active Directory apportate semplificano il processo di installazione del ruolo controller di dominio e riducono le probabilità di errori amministrativi, specialmente in caso di distribuzione di più controller di dominio in aree e domini globali.  
Per informazioni più dettagliate sulle installazioni con interfaccia grafica o Windows PowerShell, inclusa la sintassi della riga di comando e istruzioni dettagliate sulle procedure guidate, vedere [Installare Servizi di dominio Active Directory](https://technet.microsoft.com/library/hh472162.aspx). Per gli amministratori che desiderano controllare l'introduzione di modifiche allo schema in una foresta di Active Directory indipendente dall'installazione di controller di dominio di Windows Server 2012 in una foresta esistente, è comunque possibile eseguire i comandi Adprep.exe a un prompt dei comandi con privilegi elevati.  

## <a name="BKMK_DeprecatedFeatures"></a>Funzionalità deprecate e modifiche del comportamento relative a servizi di dominio Active Directory in Windows Server 2012

Sono presenti alcune modifiche relative ai Servizi di dominio Active Directory:  

- **Deprecazione di Adprep32. exe**
   - Esiste una sola versione di Adprep.exe, che può essere eseguita, se necessario, sui server a 64 bit che eseguono Windows Server 2008 o versione successiva. È eseguibile in remoto e deve essere eseguita in remoto se il ruolo di master delle operazioni di riferimento è ospitato su un sistema operativo a 32 bit o su Windows Server 2003.  
- **Deprecazione di Dcpromo. exe**
   - Dcpromo è deprecato anche se solo in Windows Server 2012 può essere eseguito con un file di risposte o con i parametri della riga di comando per consentire alle organizzazioni di passare dall'automazione esistente alle nuove opzioni di installazione di Windows PowerShell.  
- **LMHash è disabilitato negli account utente**
  - Le impostazioni predefinite sicure di Modelli di sicurezza in Windows Server 2008, Windows Server 2008 R2 e Windows Server 2012 abilitano il criterio NoLMHash, disabilitato nei modelli di sicurezza dei controller di dominio di Windows 2000 e Windows Server 2003. Disabilitare, se necessario, il criterio NoLMHash per i client dipendenti da LMHash, secondo la procedura descritta nell'articolo [946405](https://support.microsoft.com/kb/946405)della Knowledge Base.  

A partire da Windows Server 2008, i controller di dominio dispongono anche delle seguenti impostazioni predefinite sicure, rispetto ai controller di dominio che eseguono Windows Server 2003 o Windows 2000.

|||||  
|-|-|-|-|  
|Tipo di crittografia o criteri|Impostazione predefinita in Windows Server 2008|Impostazione predefinita in Windows Server 2012 e Windows Server 2008 R2|Commento|  
|AllowNT4Crypto|Disabilitata|Disabilitata|È possibile che i client SMB (Server Message Block) di terze parti non siano compatibili con le impostazioni predefinite sicure dei controller di dominio. In tutti i casi, è possibile ridurre queste impostazioni per consentire l'interoperabilità, ma solo a scapito della sicurezza. Per ulteriori informazioni, vedere l' [articolo 942564](https://go.microsoft.com/fwlink/?LinkId=164558) della Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=164558).|  
|DES|Enabled|Disabilitata|[Articolo 977321](https://go.microsoft.com/fwlink/?LinkId=177717) della Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=177717)|  
|CBT/Protezione estesa per autenticazione integrata|N/D|Enabled|Vedere [Microsoft Security Advisory (937811)](https://go.microsoft.com/fwlink/?LinkId=164559) (https://go.microsoft.com/fwlink/?LinkId=164559) e l' [articolo 976918](https://go.microsoft.com/fwlink/?LinkId=178251) nella microsoft Knowledge base (https://go.microsoft.com/fwlink/?LinkId=178251).<br /><br />Esaminare e installare l'hotfix nell' [articolo 977073](https://go.microsoft.com/fwlink/?LinkId=186394) (https://go.microsoft.com/fwlink/?LinkId=186394) nella Microsoft Knowledge base come richiesto.|  
|LMv2|Enabled|Disabilitata|[Articolo 976918](https://go.microsoft.com/fwlink/?LinkId=178251) della Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=178251)|  

## <a name="BKMK_SysReqs"></a>Requisiti del sistema operativo

Nella tabella seguente sono elencati i requisiti minimi di sistema per Windows Server 2012. Per altre informazioni sui requisiti di sistema e le informazioni di preinstallazione, vedere [Installazione di Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). Non vi sono ulteriori requisiti per l'installazione di una nuova foresta di Active Directory, tuttavia è consigliabile aggiungere una quantità di memoria sufficiente per memorizzare nella cache i contenuti del database di Active Directory allo scopo di migliorare le prestazioni dei controller di dominio, le richieste dei client LDAP e le applicazioni abilitate all'uso di Active Directory. Se si aggiorna un controller di dominio esistente o se si aggiunge a una foresta esistente un nuovo controller di dominio, leggere la sezione seguente per verificare se il server soddisfa i requisiti di spazio su disco.  

|||  
|-|-|  
|Processore|Processore a 64 bit da 1,4 GHz|  
|RAM|512 MB|  
|Requisiti di spazio libero su disco|32 GB|  
|Risoluzione dello schermo|800 x 600 o superiore|  
|Miscellaneous|Unità DVD, tastiera, accesso a Internet|  

### <a name="BKMK_DiskSpaceDCWin8"></a>Requisiti di spazio su disco per l'aggiornamento dei controller di dominio

In questa sezione vengono illustrati i requisiti di spazio su disco solo per l'aggiornamento dei controller di dominio da Windows Server 2008 o Windows Server 2008 R2. Per altre informazioni sui requisiti dello spazio su disco per l'aggiornamento dei controller di dominio a versioni precedenti di Windows Server, vedere [Requisiti dello spazio su disco per l'aggiornamento a Windows Server 2008](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008) o [Requisiti dello spazio su disco per l'aggiornamento a Windows Server 2008 R2](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008R2).  
  
Dimensionare il disco che ospita il database e i file di registro di Active Directory in modo che contenga le estensioni dello schema personalizzate e quelle richieste dalle applicazioni, gli indici delle applicazioni e quelli inizializzati dagli amministratori, oltre allo spazio per gli oggetti e gli attributi che verranno aggiunti alla directory durante il ciclo di vita della distribuzione del controller di dominio, tipicamente da 5 a 8 anni. Un corretto dimensionamento al momento della distribuzione costituisce in genere un buon investimento, rispetto ai costi molto più elevati che richiede l'espansione dello spazio di archiviazione su disco dopo la distribuzione. Per altre informazioni, vedere [Pianificazione della capacità per Servizi di dominio Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx).  
  
Per i controller di dominio di cui si è già pianificato l'aggiornamento, verificare che l'unità in cui si trova il database di Active Directory (NTDS.DIT) disponga di una quantità di spazio libero su disco equivalente almeno al 20% del file NTDS.DIT prima di iniziare l'aggiornamento del sistema operativo. Se sul volume non è disponibile spazio libero su disco sufficiente, è possibile che l'aggiornamento abbia esito negativo e che il rapporto sulla compatibilità segnali un errore di spazio su disco insufficiente:  
  
In questo caso è possibile eseguire una deframmentazione offline del database di Active Directory per liberare spazio aggiuntivo e quindi riprovare l'aggiornamento. Per altre informazioni, vedere [Compattare il file del database delle directory (deframmentazione offline)](https://technet.microsoft.com/library/cc794920(v=WS.10).aspx).  
  
### <a name="available-skus"></a>SKU disponibili

Esistono 4 edizioni di Windows Server: Foundation, Essentials, Standard e Datacenter.
Le due edizioni dotate di supporto per il ruolo Servizi di dominio Active Directory sono le edizioni Standard e Datacenter.  
  
Nelle versioni precedenti, le edizioni di Windows Server forniscono un supporto per ruoli server, contatori del processore e grandi quantità di memoria diverso. Le edizioni standard e Datacenter di Windows Server supportano tutte le funzionalità e l'hardware sottostante, ma variano nei propri diritti di virtualizzazione: sono consentite due istanze virtuali per l'edizione standard e sono consentite istanze virtuali illimitate per il Data Center. edizione.  
  
### <a name="windows-client-and-windows-server-operating-systems-that-are-supported-to-join-windows-server-domains"></a>Sistemi operativi Windows client e Windows Server supportati per l'aggiunta ai domini di Windows Server

I sistemi operativi Windows client e Windows Server indicati di seguito sono supportati dai computer membri del dominio con controller di dominio che eseguono Windows Server 2012 o versione successiva:  
  
- Sistemi operativi client: Windows 8.1, Windows 8, Windows 7, Windows Vista
   - I computer che eseguono Windows 8.1 o Windows 8 possono anche aggiungere domini con controller di dominio che eseguono una versione precedente di Windows Server, incluso Windows Server 2003 o versione successiva. In questo caso tuttavia, alcune funzionalità di Windows 8 potrebbero richiedere una configurazione aggiuntiva o non essere disponibili. Per altre informazioni su queste funzionalità e altri suggerimenti per la gestione di client Windows 8 in domini di livello inferiore, vedere [Esecuzione di computer membro Windows 8 in domini di Windows Server 2003](https://social.technet.microsoft.com/wiki/contents/articles/17361.running-windows-8-member-computers-in-windows-server-2003-domains.aspx).  
- Sistemi operativi server: Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2, Windows Server 2003  

## <a name="BKMK_UpgradePaths"></a>Percorsi di aggiornamento sul posto supportati

I controller di dominio che eseguono versioni a 64 bit di Windows Server 2008 o Windows Server 2008 R2 possono essere aggiornati a Windows Server 2012. Non è possibile aggiornare i controller di dominio che eseguono Windows Server 2003 o Windows Server 2008 a 32 bit. Per sostituirli, installare controller di dominio che eseguono una versione successiva di Windows Server nel dominio, quindi rimuovere i controller di dominio che eseguono Windows Server 2003.  

|Edizioni eseguite:|Edizioni a cui è possibile eseguire l'aggiornamento:|  
|-------------------------------------|-------------------------------------|  
|Windows Server 2008 Standard con SP2<br /><br />O<br /><br />Windows Server 2008 Enterprise con SP2|Windows Server 2012 Standard<br /><br />O<br /><br />Windows Server 2012 Datacenter|  
|Windows Server 2008 Datacenter con SP2|Windows Server 2012 Datacenter|  
|Windows Web Server 2008|Windows Server 2012 Standard|  
|Windows Server 2008 R2 Standard con SP1<br /><br />O<br /><br />Windows Server 2008 R2 Enterprise con SP1|Windows Server 2012 Standard<br /><br />O<br /><br />Windows Server 2012 Datacenter|  
|Windows Server 2008 R2 Datacenter con SP1|Windows Server 2012 Datacenter|  
|Windows Web Server 2008 R2|Windows Server 2012 Standard|  
  
Per altre informazioni sui percorsi di aggiornamento supportati, vedere [Versioni di valutazione e opzioni di aggiornamento per Windows Server 2012](https://go.microsoft.com/fwlink/?LinkId=260917). Si noti che non è possibile convertire direttamente alla versione definitiva un controller di dominio che esegue una versione di valutazione di Windows Server 2012. Provvedere invece a installare un controller di dominio aggiuntivo in un server che esegue una versione definitiva e rimuovere Servizi di dominio Active Directory dal controller di dominio in esecuzione nella versione di valutazione.  
  
A causa di un problema noto, non è possibile aggiornare un controller di dominio che esegue un'installazione dei componenti di base del server di Windows Server 2008 R2 a un'installazione dei componenti di base del server di Windows Server 2012. Questo problema provoca la visualizzazione di una schermata nera verso la fine del processo di aggiornamento. Il riavvio di questi controller di dominio espone un'opzione del file boot.ini file al rollback alla versione precedente del sistema operativo. Un ulteriore riavvio attiva il rollback automatico alla versione precedente del sistema operativo. Fino a quando non sarà disponibile una soluzione, è consigliabile installare un nuovo controller di dominio che esegue un'installazione dei componenti di base del server di Windows Server 2012 anziché l'aggiornamento sul posto di un controller di dominio esistente che esegue un'installazione dei componenti di base del server di Windows Server 2008 R2. Per altre informazioni, vedere l' [articolo 2734222](https://support.microsoft.com/kb/2734222)della Microsoft Knowledge Base.  

## <a name="BKMK_FunctionalLevels"></a>Caratteristiche e requisiti del livello funzionale

Windows Server 2012 richiede un livello di funzionalità della foresta di Windows Server 2003. Ovvero, prima di poter aggiungere un controller di dominio che esegue Windows Server 2012 a una foresta Active Directory esistente, il livello di funzionalità della foresta deve essere Windows Server 2003 o versione successiva. Di conseguenza, i controller di dominio che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003 possono operare nella stessa foresta, mentre i controller di dominio che eseguono Windows 2000 Server non sono supportati e bloccano l'installazione di un controller di dominio che esegue Windows Server 2012. Se la foresta contiene controller di dominio che eseguono Windows Server 2003 o versione successiva, ma il livello funzionale della foresta è ancora Windows 2000, l'installazione viene ugualmente bloccata.  
  
Prima di aggiungere alla foresta i controller di dominio di Windows Server 2012 è necessario rimuovere i controller di dominio di Windows 2000. In questo caso, osservare il flusso di lavoro seguente:  

1. Installare controller di dominio che eseguono Windows Server 2003 o versione successiva. È possibile distribuire questi controller di dominio su una versione di valutazione di Windows Server. Questo passaggio richiede anche, come prerequisito, l' [esecuzione di adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx) per quella versione del sistema operativo.  
2. Rimuovere i controller di dominio di Windows 2000. Nello specifico, provocare un abbassamento del livello dei controller di dominio di Windows Server 2000 oppure eliminarli dal dominio e utilizzare Utenti e computer di Active Directory per rimuovere gli account del controller di dominio relativi a tutti i controller di dominio rimossi.  
3. Aumentare il livello di funzionalità della foresta a Windows Server 2003 o superiore.  
4. Installare controller di dominio che eseguono Windows Server 2012.  
5. Rimuovere i controller di dominio che eseguono versioni precedenti di Windows Server.  

Il nuovo livello di funzionalità del dominio Windows Server 2012 Abilita una nuova funzionalità: il criterio dei modelli amministrativi KDC **Supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos** ha due impostazioni (**Fornisci sempre attestazioni** e **non riesce richieste di autenticazione non blindate**) che richiedono il livello di funzionalità del dominio di Windows Server 2012.  
  
Il livello di funzionalità foresta Windows Server 2012 non fornisce tutte le nuove funzionalità, ma garantisce che ogni nuovo dominio creato nella foresta venga automaticamente utilizzato a livello funzionale di dominio di Windows Server 2012. Il livello di funzionalità del dominio Windows Server 2012 non fornisce altre nuove funzionalità oltre al supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos. Ma assicura che qualsiasi controller di dominio nel dominio esegua Windows Server 2012. Per altre informazioni sulle altre caratteristiche disponibili a livelli di funzionalità diversi, vedere [Informazioni sui livelli di funzionalità di Servizi di dominio Active Directory](../active-directory-functional-levels.md).  
  
Dopo aver impostato il livello di funzionalità della foresta su un determinato valore, non è possibile eseguire il rollback né abbassare il livello di funzionalità della foresta, con le eccezioni seguenti: dopo aver aumentato il livello di funzionalità della foresta a Windows Server 2012, è possibile abbassarlo a Windows Server 2008 R2. Se Active Directory Cestino non è stato abilitato, è anche possibile abbassare il livello di funzionalità della foresta da Windows Server 2012 a Windows Server 2008 R2 o Windows Server 2008 o da Windows Server 2008 R2 a Windows Server 2008. Se il livello di funzionalità della foresta è impostato su Windows Server 2008 R2, non è possibile eseguire il rollback, ad esempio, in Windows Server 2003.  
  
Dopo aver impostato il livello di funzionalità del dominio su un determinato valore, non è possibile eseguire il rollback né abbassare il livello di funzionalità del dominio, con le eccezioni seguenti: quando si aumenta il livello di funzionalità del dominio a Windows Server 2008 R2 o Windows Server 2012 e se la foresta functio il livello NAL è Windows Server 2008 o inferiore, è possibile eseguire il rollback del livello di funzionalità del dominio a Windows Server 2008 o Windows Server 2008 R2. È possibile abbassare il livello di funzionalità del dominio solo da Windows Server 2012 a Windows Server 2008 R2 o Windows Server 2008 o da Windows Server 2008 R2 a Windows Server 2008. Se il livello di funzionalità del dominio è impostato su Windows Server 2008 R2, non è possibile eseguire il rollback, ad esempio, in Windows Server 2003.  
  
Per altre informazioni sulle caratteristiche disponibili a livelli di funzionalità inferiori, vedere [Informazioni sui livelli di funzionalità di Servizi di dominio Active Directory](../active-directory-functional-levels.md).  
  
Oltre a livelli di funzionalità, un controller di dominio che esegue Windows Server 2012 fornisce funzionalità aggiuntive che non sono disponibili in un controller di dominio che esegue una versione precedente di Windows Server. Ad esempio, un controller di dominio che esegue Windows Server 2012 utilizzabile per la clonazione del controller di dominio virtuale, mentre non è un controller di dominio che esegue una versione precedente di Windows Server. Tuttavia, la clonazione del controller di dominio virtuale e le misure di sicurezza del controller di dominio virtuale in Windows Server 2012 non hanno requisiti a livello funzionale.  

> [!NOTE]  
> Microsoft Exchange Server 2013 richiede il livello di funzionalità della foresta Windows server 2003 o superiore.  

## <a name="BKMK_ServerRoles"></a>Interoperabilità di servizi di dominio Active Directory con altri ruoli server e sistemi operativi Windows

Servizi di dominio Active Directory non è supportato nei sistemi operativi Windows seguenti:  
  
- Windows MultiPoint Server  
- Windows Server 2012 Essentials  
  
Non è possibile installare Servizi di dominio Active Directory su un server che esegue anche i ruoli server o servizi ruolo seguenti:  
  
- Server Hyper-V  
- Gestore connessione Desktop remoto  
  
## <a name="BKMK_OpsMasters"></a>Ruoli di master operazioni

Alcune nuove funzionalità di Windows Server 2012 influiscono sui ruoli di master operazioni:  

- Per supportare la clonazione di controller di dominio virtuali, l'emulatore PDC deve eseguire Windows Server 2012. Vi sono altri prerequisiti per la clonazione dei controller di dominio. Per altre informazioni, vedere [Virtualizzazione dei Servizi di dominio Active Directory](https://technet.microsoft.com/library/hh831734.aspx).  
- Quando l'emulatore PDC esegue Windows Server 2012, vengono create nuove entità di sicurezza.  
- Il nuovo master RID ha una nuova funzionalità per l'emissione e il monitoraggio RID. I miglioramenti includono una migliore registrazione degli eventi, limiti più appropriati e la capacità di aumentare, in caso di emergenza, l'allocazione globale dei pool di RID di un bit. Per altre informazioni, vedere [Managing RID Issuance](../../ad-ds/manage/Managing-RID-Issuance.md).  

> [!NOTE]  
> Sebbene non siano ruoli di master operazioni, un'altra modifica nell'installazione di servizi di dominio Active Directory è che il ruolo server DNS e il catalogo globale vengono installati per impostazione predefinita in tutti i controller di dominio che eseguono Windows Server 2012.  

## <a name="BKMK_Virtual"></a>Virtualizzazione di controller di dominio

I miglioramenti introdotti in servizi di dominio Active Directory a partire da Windows Server 2012 consentono una virtualizzazione più sicura dei controller di dominio e la possibilità di clonare controller A sua volta, la clonazione dei controller di dominio consente una distribuzione rapida di ulteriori controller di dominio in un nuovo dominio, oltre ad altri vantaggi. Per ulteriori informazioni, vedere [Introduzione a Active Directory Domain Services &#40;livello di&#41; virtualizzazione &#40;di&#41;servizi di dominio Active Directory 100](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).  

## <a name="BKMK_Admin"></a>Amministrazione dei server Windows Server 2012

Usare il [strumenti di amministrazione remota del server per Windows 8](https://www.microsoft.com/download/details.aspx?id=28972) per gestire i controller di dominio e altri server che eseguono windows Server 2012. È possibile eseguire Windows Server 2012 Strumenti di amministrazione remota del server in un computer che esegue Windows 8.  

## <a name="BKMK_AppCompat"></a>Compatibilità delle applicazioni

Nella tabella seguente sono riportate le applicazioni Microsoft integrate in Active Directory più diffuse. La tabella indica le versioni di Windows Server sui cui è possibile installare le applicazioni e se l'introduzione dei controller di dominio di Windows Server 2012 può influire o meno sulla compatibilità delle applicazioni.  

|Prodotto|Note|  
|-----------|---------|  
|[Microsoft Configuration Manager 2007](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|Configuration Manager 2007 con SP2 (include Configuration Manager 2007 R2 e Configuration Manager 2007 R3):<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 standard<br />-Nota di Windows Server 2012 Datacenter **:** Sebbene queste versioni siano pienamente supportate come client, non è prevista l'aggiunta di alcun supporto per la distribuzione di tali versioni come sistemi operativi mediante la caratteristica di distribuzione del sistema operativo di Configuration Manager 2007. Non è inoltre previsto il supporto per i server o i sistemi del sito su qualsiasi SKU di Windows Server 2012.|  
|[Microsoft SharePoint 2007](https://support.microsoft.com/kb/2728964)|L'installazione di Microsoft Office SharePoint Server 2007 su Windows Server 2012 non è supportata.|  
|[Microsoft SharePoint 2010](https://support.microsoft.com/kb/2724471)|Per l'installazione e il funzionamento di SharePoint 2010 sui server di Windows Server 2012 <br />è richiesto SharePoint 2010 Service Pack 2<br /><br />Per l'installazione e il funzionamento di SharePoint 2010 sui server di Windows Server 2012 è richiesto SharePoint 2010 Foundation Service Pack 2<br /><br />Il processo di installazione di SharePoint Server 2010 (senza Service Pack) non riesce in Windows Server 2012<br /><br />Il programma di installazione dei prerequisiti di SharePoint Server 2010 (PrerequisiteInstaller. exe) ha esito negativo con errore "il programma presenta problemi di compatibilità". Se si fa &#124; clic su "Esegui il programma senza visualizzare la guida" viene visualizzato l'errore "verifica dell'installazione di sharepoint server 2010 (senza Service Pack) in Windows Server 2012".|  
|[Microsoft SharePoint 2013](https://technet.microsoft.com/library/cc262485(v=office.15).aspx)|Requisiti minimi per un server di database in una farm<br /><br />Edizione a 64 bit di Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Enterprise o Datacenter o edizione a 64 bit di Windows Server 2012 Standard o Datacenter<br /><br />Requisiti minimi per un singolo server con database incorporato:<br /><br />Edizione a 64 bit di Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Enterprise o Datacenter o edizione a 64 bit di Windows Server 2012 Standard o Datacenter<br /><br />Requisiti minimi per server Web front-end e server di applicazioni in una farm:<br /><br />Edizione a 64 bit di Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Enterprise o Datacenter o edizione a 64 bit di Windows Server 2012 Standard o Datacenter.|  
|[Microsoft System Center Configuration Manager 2012](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|System Center 2012 Configuration Manager Service Pack 1:<br /><br />Con l'uscita del Service Pack 1, Microsoft aggiungerà alla matrice del supporto client i sistemi operativi seguenti:<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 standard<br />-Windows Server 2012 Datacenter<br /><br />È possibile distribuire ai server tutti i ruoli server di siti, inclusi server di siti, provider SMS e punti di gestione, con le seguenti edizioni di sistemi operativi:<br /><br />-Windows Server 2012 standard<br />-Windows Server 2012 Datacenter|  
|[Microsoft Lync Server 2013](https://technet.microsoft.com/library/gg412883.aspx)|Con Lync Server 2013 è richiesto Windows Server 2008 R2 o Windows Server 2012. Non è possibile eseguire Lync Server 2013 su un'installazione Server Core, mentre è possibile eseguirlo su [server virtuali](https://technet.microsoft.com/library/gg399035.aspx).|  
|[Lync Server 2010](https://support.microsoft.com/kb/2777359)|È possibile installare Lync Server 2010 in un'installazione pulita (non un aggiornamento) di Windows Server 2012 se sono installati gli [aggiornamenti cumulativi per Lync Server dell'ottobre 2012](https://support.microsoft.com/?kbid=2493736) . L'aggiornamento del sistema operativo a Windows Server 2012 per un'installazione di Lync Server 2010 esistente non è supportato. In Windows Server 2012, inoltre, non è supportato Group Chat Server per Microsoft Lync Server 2010.|  
|[System Center 2012 Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|System Center 2012 Endpoint Protection Service Pack 1 aggiorna la matrice del supporto client per includere i sistemi operativi seguenti<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 standard<br />-Windows Server 2012 Datacenter|  
|[System Center 2012 Forefront Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|Forefront Endpoint Protection 2010 con Aggiornamento cumulativo 1 aggiorna la matrice del supporto client per includere i sistemi operativi seguenti:<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 standard<br />-Windows Server 2012 Datacenter|  
|Forefront Threat Management Gateway|L'esecuzione di Forefront Threat Management Gateway è supportata solo su Windows Server 2008 e Windows Server 2008 R2. Per altre informazioni, vedere [Requisiti di sistema per Forefront TMG](https://technet.microsoft.com/library/dd896981.aspx).|  
|Windows Server Update Services|Questa versione di WSUS fornisce già il supporto per l'impiego, come client, di computer basati su Windows 8 o Windows Server 2012.|  
|Windows Server Update Services 3.0|L'aggiornamento contenuto nell'[articolo 2734608](https://support.microsoft.com/kb/2734608) della Knowledge Base Microsoft consente ai server che eseguono Windows Server Update Services (WSUS) 3.0 SP2 di fornire aggiornamenti ai computer che eseguono Windows 8 o Windows Server 2012: **Si noti** Presso i clienti che si avvalgono di ambienti WSUS 3.0 SP2 autonomi o ambienti System Center Configuration Manager 2007 Service Pack 2 con WSUS 3.0 SP2 è necessario l'aggiornamento [2734608](https://support.microsoft.com/kb/2734608) per una corretta gestione dei computer client basati su Windows 8 o Windows Server 2012.|  
|[Exchange 2013](https://technet.microsoft.com/library/bb691354.aspx)|È fornito il supporto per Windows Server 2012 Standard e Datacenter per i ruoli seguenti: master schema, server di catalogo globale, controller di dominio, mailbox e ruolo del server Accesso client<br /><br />Livello di funzionalità della foresta: Windows Server 2003 o versione successiva<br /><br />Origine: Requisiti di sistema di Exchange 2013|  
|Exchange 2010|[Origine: Exchange 2010 Service Pack 3 @ no__t-0<br /><br />È possibile installare Exchange 2010 con Service Pack 3 sui server membri di Windows Server 2012.<br /><br />In[Requisiti di sistema di Exchange 2010](https://technet.microsoft.com/library/aa996719(EXCHG.141).aspx) sono elencati l'ultimo master schema, catalogo globale e controller di dominio supportati come Windows Server 2008 R2.<br /><br />Livello di funzionalità della foresta: Windows Server 2003 o versione successiva|  
|SQL Server 2012|Origine: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />SQL Server 2012 RTM è supportato in Windows Server 2012.|  
|SQL Server 2008 R2|Origine: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Per l'installazione su Windows Server 2012 è necessario SQL Server 2008 R2 con Service Pack 1 o successivo.|  
|SQL Server 2008|Origine: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Per l'installazione su Windows Server 2012 è necessario SQL Server 2008 con Service Pack 3 o successivo.|  
|SQL Server 2005|Origine: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Non supportato per l'installazione su Windows Server 2012.|  

## <a name="BKMK_KnownIssues"></a>Problemi noti

Nella tabella seguente sono indicati i problemi noti relativi all'installazione di Servizi di dominio Active Directory.  

||||  
|-|-|-|  
|Numero e titolo dell'articolo della Knowledge Base|Area tecnologica interessata|Problema/descrizione|  
|[2830145](https://support.microsoft.com/kb/2830145): Impossibile eseguire il mapping del SID S-1-18-1 e del SID S-1-18-2 su computer basati su Windows 7 o Windows Server 2008 R2 in un ambiente di dominio|Gestione di Servizi di dominio Active Directory/compatibilità delle applicazioni|Le applicazioni che eseguono il mapping del SID S-1-18-1 e del SID S-1-18-2, nuove in Windows Server 2012, potrebbero avere esito negativo perché i SID non possono essere risolti su computer basati su Windows 7 o Windows Server 2008 R2. Per risolvere il problema, installare l'hotfix nei computer del dominio basati su Windows 7 e su Windows Server 2008 R2.|  
|[2737129](https://support.microsoft.com/kb/2737129): Quando si prepara automaticamente un dominio esistente per Windows Server 2012 la preparazione di Criteri di gruppo non viene eseguita|Installazione di Servizi di dominio Active Directory|Adprep /domainprep /gpprep non viene eseguito automaticamente nell'ambito dell'installazione del primo controller di dominio che esegue Windows Server 2012 in un dominio. Se non è mai stato eseguito prima nel dominio, è necessario eseguirlo manualmente.|  
|[2737416](https://support.microsoft.com/kb/2737416): Avvisi ripetuti durante la distribuzione del controller di dominio basato su Windows PowerShell|Installazione di Servizi di dominio Active Directory|Durante la convalida dei prerequisiti è possibile che vengano visualizzati avvisi, che vengono nuovamente visualizzati durante l'installazione.|  
|[2737424](https://support.microsoft.com/kb/2737424): Quando si tenta di rimuovere Servizi di dominio Active Directory da un controller di dominio viene visualizzato l'errore "Il formato del nome di dominio specificato non è valido"|Installazione di Servizi di dominio Active Directory|Questo errore viene visualizzato quando si rimuove l'ultimo controllore di dominio in un dominio che contiene ancora controller di dominio di sola lettura creati preventivamente. Il problema si verifica in Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008.|  
|[2737463](https://support.microsoft.com/kb/2737463): Il controller di dominio non si avvia, viene generato l'errore c00002e2 oppure viene visualizzato il messaggio "Selezionare un'opzione"|Installazione di Servizi di dominio Active Directory|Un controller di dominio non viene avviato perché un amministratore ha utilizzato Dism.exe, Pkgmgr.exe o Ocsetup.exe per rimuovere il ruolo DirectoryServices-DomainController.|  
|[2737516](https://support.microsoft.com/kb/2737516): Limitazioni della verifica IFM in Server Manager di Windows Server 2012|Installazione di Servizi di dominio Active Directory|La verifica IFM può presentare limitazioni, secondo quanto illustrato nell'articolo della Knowledge Base.|  
|[2737535](https://support.microsoft.com/kb/2737535): Il cmdlet Install-AddsDomainController restituisce un errore di impostazione parametro per il controller di dominio di sola lettura|Installazione di Servizi di dominio Active Directory|È possibile che si riceva un errore quando si tenta di associare un server a un account di controller di dominio di sola lettura se si specificano argomenti già popolati nell'account di controller di dominio di sola lettura creati preventivamente.|  
|[2737560](https://support.microsoft.com/kb/2737560): Viene visualizzato l'errore "Impossibile eseguire la verifica dei conflitti dello schema di Exchange" e il controllo dei prerequisiti non riesce|Installazione di Servizi di dominio Active Directory|Il controllo dei prerequisiti non riesce quando si configura il primo controller di dominio di Windows Server 2012 in un dominio esistente perché i controller di dominio non dispongono del diritto di accesso SeServiceLogonRight per Servizio di rete oppure perché i protocolli WMI o DCOM sono bloccati.|  
|[2737797](https://support.microsoft.com/kb/2737797): Il modulo AddsDeployment con l'argomento -Whatif visualizza risultati errati per il DNS|Installazione di Servizi di dominio Active Directory|Il parametro-WhatIf mostra che il server DNS non verrà installato, ma sarà.|  
|[2737807](https://support.microsoft.com/kb/2737807): Il pulsante Avanti non è disponibile nella pagina Opzioni Controller di dominio|Installazione di Servizi di dominio Active Directory|Nella pagina Opzioni controller di dominio il pulsante Avanti è disattivato perché l'indirizzo IP del controller di dominio di destinazione non è mappato ad alcuna subnet o sito esistente, oppure perché la password della modalità DSRM non è stata digitata e confermata correttamente.|  
|[2737935](https://support.microsoft.com/kb/2737935): L'installazione di Active Directory si blocca nella fase di "Creazione dell'oggetto Impostazioni NTDS"|Installazione di Servizi di dominio Active Directory|L'installazione si interrompe perché la password di amministratore locale coincide con la password di amministratore del dominio, oppure a causa di problemi di rete che impediscono il completamento della replica della parte critica.|  
|[2738060](https://support.microsoft.com/kb/2738060): Quando si crea un dominio figlio in remoto con Install-AddsDomain viene visualizzato il messaggio di errore "Accesso negato"|Installazione di Servizi di dominio Active Directory|L'errore viene visualizzato quando si esegue Install-ADDSDomain con il cmdlet Invoke-Command se la password di DNSDelegationCredential è errata.|  
|[2738697](https://support.microsoft.com/kb/2738697): Quando si configura un server con Server Manager viene visualizzato l'errore di configurazione del controller di dominio "Server non operativo"|Installazione di Servizi di dominio Active Directory|L'errore viene visualizzato quando si tenta di installare su un computer del gruppo di lavoro perché l'autenticazione NTLM è disattivata.|  
|[2738746](https://support.microsoft.com/kb/2738746): Dopo l'accesso a un account di dominio amministratore locale vengono visualizzati errori di accesso negato|Installazione di Servizi di dominio Active Directory|Quando si effettua l'accesso utilizzando un account di amministratore locale invece dell'account di amministratore predefinito e quindi si crea un nuovo dominio, l'account non viene aggiunto al gruppo Domain Admins.|  
|[2743345](https://support.microsoft.com/kb/2743345): Con adprep /gpprep viene visualizzato l'errore "Impossibile trovare il file specificato" oppure lo strumento si arresta in modo anomalo|Installazione di Servizi di dominio Active Directory|L'errore viene visualizzato quando si esegue adprep /gpprep perché il master infrastrutture implementa uno spazio dei nomi non connesso|  
|[2743367](https://support.microsoft.com/kb/2743367): In Windows Server 2003 versione a 64 bit viene visualizzato l'errore adprep "L'elemento specificato non è un'applicazione Win32 valida"|Installazione di Servizi di dominio Active Directory|Questo errore viene visualizzato perché non è possibile eseguire Windows Server 2012 Adprep su Windows Server 2003.|  
|[2753560](https://support.microsoft.com/kb/2753560): Errori di installazione di ADMT versione 3.2 e PES 3.1 in Windows Server 2012|ADMT|Per impostazione predefinita, non è possibile installare ADMT 3.2 su Windows Server 2012.|  
|[2750857](https://support.microsoft.com/kb/2750857): I rapporti di diagnostica di replica DFS non vengono visualizzati correttamente in Internet Explorer 10|Replica DFS|Il rapporto di diagnostica di Replica DFS non viene visualizzato correttamente a causa delle modifiche apportate a Internet Explorer 10.|  
|[2741537](https://support.microsoft.com/kb/2741537): Gli aggiornamenti di Criteri di gruppo remoto sono visibili agli utenti|Criteri di gruppo|La causa di questo problema sono le attività pianificate eseguite nel contesto dei singoli utenti che hanno effettuato l'accesso. In questo scenario, l'Utilità di pianificazione di Windows è progettata in modo da richiedere un prompt interattivo.|  
|[2741591](https://support.microsoft.com/kb/2741591): I file ADM non sono presenti in SYSVOL nell'opzione relativa allo stato dell'infrastruttura della console Gestione Criteri di gruppo|Criteri di gruppo|La replica di GP può segnalare la "replica in corso" perché lo stato dell'infrastruttura GPMC non segue le regole di filtro personalizzate.|  
|[2737880](https://support.microsoft.com/kb/2737880): Durante la configurazione di Active Directory viene visualizzato l'errore "Impossibile avviare il servizio"|Clonazione di controller di dominio virtuali|L'errore viene visualizzato durante l'installazione o la rimozione, o la clonazione, di Servizi di dominio Active Directory perché il servizio Ruolo server DS è disattivato.|  
|[2742836](https://support.microsoft.com/kb/2742836): Quando si utilizza la funzionalità di clonazione VDC vengono creati due lease DHCP per ogni controller di dominio|Clonazione di controller di dominio virtuali|Questo problema si verifica perché il controller di dominio clonato ha ricevuto un lease prima della clonazione e poi di nuovo a clonazione completata.|  
|[2742844](https://support.microsoft.com/kb/2742844): In Windows Server 2012 la clonazione del controller di dominio non riesce e il server viene riavviato in modalità ripristino servizi directory|Clonazione di controller di dominio virtuali|Il controller di dominio clonato viene avviato in modalità ripristino servizi directory perché la clonazione non è riuscita a causa di vari motivi illustrati nell'articolo della Knowledge Base.|  
|[2742874](https://support.microsoft.com/kb/2742874): La clonazione del controller di dominio non ricrea tutti i nomi dell'entità servizio|Clonazione di controller di dominio virtuali|Alcuni SPN a tre parti non vengono ricreati nel controller di dominio clonato a causa di una limitazione del processo di ridenominazione dei domini.|  
|[2742908](https://support.microsoft.com/kb/2742908): Dopo la clonazione del controller di dominio viene visualizzato l'errore "Nessun server di accesso disponibile"|Clonazione di controller di dominio virtuali|L'errore viene visualizzato quando si tenta di effettuare l'accesso dopo aver clonato un controller di dominio virtualizzato perché la clonazione non è riuscita e il controller di dominio è stato avviato in modalità ripristino servizi directory. Per risolvere il problema della clonazione non riuscita, effettuare l'accesso come .\administrator.|  
|[2742916](https://support.microsoft.com/kb/2742916): La clonazione del controller di dominio non riesce e in dcpromo.log viene visualizzato l'errore 8610|Clonazione di controller di dominio virtuali|La clonazione non riesce perché l'emulatore PDC non ha eseguito la replica in ingresso della partizione di dominio, probabilmente perché il ruolo è stato trasferito.|  
|[2742927](https://support.microsoft.com/kb/2742927): Errore di New-AdDcCloneConfig "Indice non compreso nell'intervallo"|Clonazione di controller di dominio virtuali|L'errore viene visualizzato dopo l'esecuzione del cmdlet New-ADDCCloneConfigFile durante la clonazione di controller di dominio virtuali, perché il cmdlet non è stato eseguito da un prompt dei comandi con privilegi elevati oppure perché il token di accesso non contiene il gruppo Administrators.|  
|[2742959](https://support.microsoft.com/kb/2742959): La clonazione del controller di dominio non riesce e viene visualizzato l'errore 8437: "Parametro non valido specificato per la replica"|Clonazione di controller di dominio virtuali|La clonazione non è riuscita perché è stato specificato un nome di clone non valido o un nome NetBIOS doppio.|  
|[2742970](https://support.microsoft.com/kb/2742970): La clonazione del controller di dominio non riesce senza modalità ripristino servizi directory, computer di origine e computer clone|Clonazione di controller di dominio virtuali|Il controller di dominio virtuale clonato si avvia in modalità ripristino servizi directory utilizzando un nome duplicato come controller di dominio di origine poiché il file DCCloneConfig.xml non è stato creato nella posizione corretta oppure il controller di dominio di origine è stato riavviato prima della clonazione.|  
|[2743278](https://support.microsoft.com/kb/2743278): Errore 0x80041005 di clonazione del controller di dominio|Clonazione di controller di dominio virtuali|Il controller di dominio clonato si avvia in modalità di ripristino dei servizi directory perché è stato specificato un solo server WINS. Se è stato specificato un server WINS, è necessario specificare sia il server WINS preferito che quello alternativo.|  
|[2745013](https://support.microsoft.com/kb/2745013): Se si esegue New-AdDcCloneConfigFile in Windows Server 2012 viene visualizzato il messaggio di errore "Server non operativo"|Clonazione di controller di dominio virtuali|L'errore viene visualizzato dopo l'esecuzione del cmdlet New-ADDCCloneConfigFile perché il server non riesce a contattare un server di catalogo globale.|  
|[2747974](https://support.microsoft.com/kb/2747974): L'evento 2224 di clonazione del controller di dominio fornisce istruzioni errate|Clonazione di controller di dominio virtuali|L'ID evento 2224 segnala erroneamente che è necessario rimuovere gli account del servizio gestito prima della clonazione. È necessario rimuovere gli account del servizio gestito autonomi, ma gli account del account del servizio gestito di gruppo non bloccano la clonazione.|  
|[2748266](https://support.microsoft.com/kb/2748266): Impossibile sbloccare un'unità crittografata da BitLocker dopo l'aggiornamento a Windows 8|BitLocker|Quando si tenta di sbloccare un'unità su un computer aggiornato da Windows 7, viene visualizzato l'errore "applicazione non trovata".|  

## <a name="see-also"></a>Vedere anche

[Risorse di valutazione di Windows Server 2012](https://technet.microsoft.com/evalcenter/hh708766.aspx)  
[Guida alla valutazione di Windows Server 2012](https://download.microsoft.com/download/5/B/2/5B254183-FA53-4317-B577-7561058CEF42/WS%202012%20Evaluation%20Guide.pdf)  
[Installare e distribuire Windows Server 2012](https://technet.microsoft.com/library/hh831620.aspx)  
