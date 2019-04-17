---
ms.assetid: e4c31187-f15f-410b-bb79-8d63e2f2b421
title: Aggiornare controller di dominio a Windows Server 2012 R2 e Windows Server 2012
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e317c5a939d417bac844c4080223d7b5e0eec149
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012"></a>Aggiornare controller di dominio a Windows Server 2012 R2 e Windows Server 2012

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono fornite informazioni su servizi di dominio Active Directory in Windows Server 2012 R2 e Windows Server 2012 e viene illustrato il processo di aggiornamento dei controller di dominio da Windows Server 2008 o Windows Server 2008 R2.  
  
-   [Operazioni di aggiornamento di controller di dominio](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_UpgradeWorkflow)  
  
-   [What's new in Windows Server 2012?](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_WhatsNewEight)  
  
-   [What's new in Active Directory in Windows Server 2012 R2?](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_NewWS2012R2)  
  
-   [What's new in Active Directory in Windows Server 2012?](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_WhatsNewAD)  
  
-   [Modifiche installazione del ruolo server di dominio Active Directory](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_InstallationChanges)  
  
-   [Funzionalità deprecate e modifiche del comportamento relative a servizi di dominio Active Directory in Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures)  
  
-   [Requisiti del sistema operativo](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_SysReqs)  
  
-   [Percorsi di aggiornamento sul posto supportati](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_UpgradePaths)  
  
-   [Requisiti e le funzionalità a livello funzionale](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_FunctionalLevels)  
  
-   [Interoperabilità di Active Directory con altri ruoli server e sistemi operativi Windows](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_ServerRoles)  
  
-   [Ruoli di master operazioni](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_OpsMasters)  
  
-   [La virtualizzazione dei controller di dominio che eseguono Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_Virtual)  
  
-   [Amministrazione dei server Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_Admin)  
  
-   [Compatibilità delle applicazioni](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat)  
  
-   [Problemi noti](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_KnownIssues)  
  
## <a name="BKMK_UpgradeWorkflow"></a>Operazioni di aggiornamento di controller di dominio  
Il metodo consigliato per aggiornare un dominio è alzare di livello controller di dominio che eseguono versioni più recenti di Windows Server e abbassare di livello controller di dominio precedenti in base alle esigenze. Questo metodo è preferibile per l'aggiornamento del sistema operativo di un controller di dominio esistente. Questo elenco enumera i passaggi generali da seguire prima di promuovere un controller di dominio che esegue una versione più recente di Windows Server:  
  
1.  Verify the target server meets [system requirements](https://technet.microsoft.com/library/dn303418.aspx).  
  
2.  Verificare [compatibilità delle applicazioni](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat).  
  
3.  Verificare le impostazioni di sicurezza. For more information, see [Deprecated features and behavior changes related to AD DS in Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures) and [Secure default settings in Windows Server 2008 and Windows Server 2008 R2](https://technet.microsoft.com/library/upgrade-domain-controllers-to-windows-server-2008-r2(WS.10).aspx#BKMK_SecureDefault).  
  
4.  Verificare la connettività al server di destinazione dal computer in cui si prevede di eseguire l'installazione.  
  
5.  Controllare la disponibilità dei ruoli master operazione necessari:  
  
    -   Per installare il primo controller di dominio che esegue Windows Server 2012 in un dominio esistente e della foresta, il computer in cui si esegue l'installazione disponga della connettività al master schema per poter eseguire adprep /forestprep e al master infrastrutture per poter eseguire adprep /domainprep.  
  
    -   Per installare il primo controller di dominio in un dominio in cui è già esteso lo schema della foresta, è necessario solo connettività al master infrastrutture.  
  
    -   Per installare o rimuovere un dominio in una foresta esistente, è necessaria la connettività al master denominazione domini.  
  
    -   Qualsiasi installazione di controller di dominio richiede inoltre la connettività al master RID.  
  
    -   Se si installa il primo controller di dominio di sola lettura in una foresta esistente, è necessaria la connettività al master infrastrutture per ogni partizione di directory applicativa, noto anche come un contesto dei nomi non di dominio o detta.  
  
6.  Assicurarsi di fornire le credenziali necessarie per eseguire l'installazione di Active Directory.  
  
    |Azione di installazione|Credenziali necessarie|  
    |-----------------------|---------------------------|  
    |Installare una nuova foresta|Amministratore locale sul server di destinazione|  
    |Installare un nuovo dominio in una foresta esistente|Enterprise Admins|  
    |Installare un controller di dominio aggiuntivo in un dominio esistente|Domain Admins|  
    |Eseguire adprep /forestprep|Schema Admins, Enterprise Admins e Domain Admins|  
    |Eseguire adprep /domainprep.|Domain Admins|  
    |Eseguire adprep /domainprep /gpprep|Domain Admins|  
    |Eseguire adprep /rodcprep|Enterprise Admins|  
  
    È possibile delegare le autorizzazioni per installare Active Directory. For more information, see [Installation Management Tasks](https://technet.microsoft.com/library/cc773327(WS.10).aspx).  
  
Sono disponibili istruzioni dettagliate passaggi per promuovere nuovi e controller di dominio di replica Windows Server 2012 usando i cmdlet di Windows PowerShell e Server Manager nei collegamenti seguenti:  
  
-   [Installare servizi di dominio Active Directory (livello 100)](https://technet.microsoft.com/library/hh472162.aspx)  
  
-   [Installare una nuova foresta di Active Directory di Windows Server 2012 (livello 200)](https://technet.microsoft.com/library/jj574166.aspx)  
  
-   [Installare un Controller di dominio di Replica Windows Server 2012 in un dominio esistente (livello 200)](https://technet.microsoft.com/library/jj574134.aspx)  
  
-   [Installare un nuovo figlio di Active Directory di Windows Server 2012 o dominio albero (livello 200)](https://technet.microsoft.com/library/jj574105.aspx)  
  
-   [Installare un Controller di dominio di sola lettura di Server 2012 Active Directory di Windows (RODC) (livello 200)](https://technet.microsoft.com/library/jj574152.aspx)  
  
-   [Guida dettagliata per l'impostazione di Windows Server 2012 Controller di dominio (en-US)](https://social.technet.microsoft.com/wiki/contents/articles/12370.step-by-step-guide-for-setting-up-windows-server-2012-domain-controller-en-us.aspx)  
  
## <a name="BKMK_WhatsNewEight"></a>What's new in Windows Server 2012?  
Nuove funzionalità elencati in base al ruolo di server e area tecnologica sono elencati nella tabella seguente. For more whitepapers, video demonstrations, and presentations about other features in Windows Server 2012, see [Server and Cloud Platform](https://www.microsoft.com/server-cloud/default.aspx).  
  
||||  
|-|-|-|  
|[Servizi certificati Active Directory (AD CS)](https://technet.microsoft.com/library/hh831373.aspx)|[Active Directory Rights Management Services (AD RMS)](https://technet.microsoft.com/library/hh831554.aspx)|[Crittografia unità BitLocker](https://technet.microsoft.com/library/hh831412.aspx)|  
|[BranchCache](https://technet.microsoft.com/library/jj127252.aspx)|[Dynamic Host Configuration protocol (DHCP)](https://technet.microsoft.com/library/jj200226.aspx)|[Domain Name System (DNS)](https://technet.microsoft.com/library/jj200224.aspx)|  
|[Clustering di failover](https://technet.microsoft.com/library/hh831414.aspx)|[Gestione risorse file Server](https://technet.microsoft.com/library/hh831746.aspx)|[Criteri di gruppo](https://technet.microsoft.com/library/jj574108.aspx)|  
|[Hyper-V](https://technet.microsoft.com/library/hh831410.aspx)|[Gestione indirizzi IP (IPAM)](https://technet.microsoft.com/library/jj200214.aspx)|[Autenticazione Kerberos](https://technet.microsoft.com/library/hh831747.aspx)|  
|[Account dei servizi gestiti](https://technet.microsoft.com/library/hh831451.aspx)|[Funzionalità di rete](https://technet.microsoft.com/library/jj200215.aspx)|[Servizi Desktop remoto](https://technet.microsoft.com/library/hh831527.aspx)|  
|[Controllo della sicurezza](https://technet.microsoft.com/library/hh849638.aspx)|[Server Manager](https://blogs.technet.com/b/servermanager/archive/2012/06/27/server-manager-power-of-many-simplicity-of-one.aspx)|[Smart card](https://technet.microsoft.com/library/hh849637.aspx)|  
|[TLS/SSL (SSP Schannel)](https://technet.microsoft.com/library/hh831771.aspx)|[Servizi di distribuzione Windows](https://technet.microsoft.com/library/hh974416.aspx)|[Windows PowerShell 3.0](https://technet.microsoft.com/library/hh857339)|  
  
### <a name="automatic-maintenance-and-changes-to-restart-behavior-after-updates-are-applied-by-windows-update"></a>Manutenzione automatica e le modifiche apportate al riavvio dopo l'applicazione degli aggiornamenti tramite Windows Update  
Prima del rilascio di Windows 8, Windows Update gestiva la pianificazione interna per controllare gli aggiornamenti e per scaricarli e installarli. È necessario che l'agente Windows Update è sempre in esecuzione in background, consumando memoria e altre risorse di sistema.  
  
Windows 8 and Windows Server 2012 introduce a new feature called [Automatic Maintenance](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx). Manutenzione automatica Consolida più funzionalità diverse, ciascuna usata per gestire la pianificazione e la logica di esecuzione. This consolidation allows for all these components to use far less system resources, work consistently, respect the new [Connected Standby](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) state for new device types, and consume less battery on portable devices.  
  
Poiché Windows Update è una parte di manutenzione automatica in Windows 8 e Windows Server 2012, la pianificazione interna per l'impostazione di giorno e ora per installare gli aggiornamenti non è più efficace. To help ensure consistent and predictable restart behavior for all devices and computers in your enterprise, including those that run Windows 8 and Windows Server 2012, see Microsoft KB article [2885694](https://support.microsoft.com/kb/2885694) (or see October 2013 cumulative rollup [2883201](https://support.microsoft.com/kb/2883201)), then configure policy settings described in the WSUS blog post [Enabling a more predictable Windows Update experience for Windows 8 and Windows Server 2012 (KB 2885694)](http://blogs.technet.com/b/wsus/archive/2013/10/08/enabling-a-more-predictable-windows-update-experience-for-windows-8-and-windows-server-2012-kb-2885694.aspx).  
  

## <a name="BKMK_NewWS2012R2"></a>What's new in Active Directory in Windows Server 2012 R2?  
Nella tabella seguente vengono riepilogate le funzionalità nuove per Active Directory in Windows Server 2012 R2, con un link a informazioni più dettagliate in cui è disponibile. For a more detailed explanation of some features, including their requirements, see [What's New in Active Directory in Windows Server 2012 R2](https://technet.microsoft.com/library/dn268294.aspx).  
  
|Funzionalità|Descrizione|  
|-----------|---------------|  
|[Aggiunta alla rete aziendale](https://technet.microsoft.com/library/dn280945.aspx)|Consente agli information worker di aggiungere i dispositivi personali alla società per accedere ai servizi e alle risorse aziendali.|  
|[Proxy applicazione Web](https://technet.microsoft.com/library/dn280942.aspx)|Fornisce l'accesso all'applicazione web utilizzando un nuovo servizio ruolo Accesso remoto.|  
|[Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx)|Distribuzione semplificata e miglioramenti che consentono agli utenti di accedere alle risorse dai dispositivi personali e ai reparti IT la gestione del controllo di accesso AD FS.|  
|[Unicità SPN e UPN](https://technet.microsoft.com/library/dn535779.aspx)|I controller di dominio che esegue Windows Server 2012 R2 bloccano la creazione di nomi dell'entità servizio (SPN) e i nomi dell'entità utente (UPN).|  
|[Accesso automatico riavvio Sign-On (Winlogon)](https://technet.microsoft.com/library/dn535772.aspx)|Consente di bloccare applicazioni schermata di essere riavviate e disponibili nei dispositivi Windows 8.1.|  
|[Attestazione chiave TPM](https://technet.microsoft.com/library/dn581921.aspx)|Consente alle CA a livello di crittografia attestare in un certificato rilasciato che la chiave privata del certificato richiedente è effettivamente protetto da un modulo TPM (Trusted Platform).|  
|[Gestione e protezione delle credenziali](https://technet.microsoft.com/library/dn408190.aspx)|Nuove credenziali protezione e il dominio controlli di autenticazione per ridurre il furto di credenziali.|  
|[Deprecazione del servizio Replica File (FRS)](https://technet.microsoft.com/library/dn535775.aspx)|Il livello di funzionalità del dominio Windows Server 2003 è deprecato anche perché a livello di funzionalità FRS viene usato per replicare SYSVOL. Che significa che quando si crea un nuovo dominio in un server che esegue Windows Server 2012 R2, il livello di funzionalità del dominio deve essere Windows Server 2008 o versione successiva. È comunque possibile aggiungere un controller di dominio che esegue Windows Server 2012 R2 a un dominio esistente che dispone di un livello funzionalità dominio di Windows Server 2003. è possibile creare un nuovo dominio solo a tale livello.|  
|[Nuovi livelli di funzionalità di domini e foreste](../active-directory-functional-levels.md)|Esistono nuovi livelli di funzionalità per Windows Server 2012 R2. Nuove funzionalità sono disponibili in Windows Server 2012 R2 DFL.|  
|[Modifiche a query optimizer LDAP](https://technet.microsoft.com/library/dn535775.aspx)|Miglioramenti delle prestazioni nell'efficienza della ricerca LDAP e durata ricerca LDAP delle query complesse.|  
|[Miglioramenti dell'evento 1644](https://technet.microsoft.com/library/dn535775.aspx)|Statistiche dei risultati di ricerca LDAP sono state aggiunte all'ID evento 1644 per agevolare la risoluzione dei problemi.|  
|[Miglioramento della velocità effettiva di replica di Active Directory](https://technet.microsoft.com/library/dn535775.aspx)|Regola velocità effettiva massima replica di Active Directory da 40 Mbps a circa 600 Mbps|  
  
## <a name="BKMK_WhatsNewAD"></a>What's new in Active Directory in Windows Server 2012?  
Nella tabella seguente sono riepilogate le nuove funzionalità di dominio Active Directory in Windows Server 2012, con un link a informazioni più dettagliate in cui è disponibile. For a more detailed explanation of some features, including their requirements, see [What's New in Active Directory Domain Services (AD DS)](https://technet.microsoft.com/library/hh831477.aspx).  
  
|Funzionalità|Descrizione|  
|-----------|---------------|  
|Active Directory-Based Activation (AD BA) see [Volume Activation Overview](https://technet.microsoft.com/library/hh831612.aspx)|Semplifica l'attività di configurazione della distribuzione e gestione dei contratti multilicenza del software.|  
|[Active Directory Federation Services (ADFS)](https://technet.microsoft.com/library/hh831502.aspx)|Aggiunge installazione dei ruoli attraverso Server Manager, semplificato-configurazione del trust, gestione automatica dei trust, supporto del protocollo SAML e altro.|  
|Eventi di cancellazione pagina persi di Active Directory|Evento NTDS ISAM 530 con errore jet-1119 viene registrato per rilevare eventi di cancellazione pagina persi database di Active Directory.|  
|[Cestino di Active Directory interfaccia utente del Cestino](https://technet.microsoft.com/library/hh831702.aspx#ad_recycle_bin_mgmt)|Centro di amministrazione di Active Directory (centro) aggiunge una caratteristica di gestione del Cestino originariamente introdotta in Windows Server 2008 R2.|  
|[Cmdlet Active Directory replica topologia e Windows PowerShell](https://technet.microsoft.com/library/hh831757.aspx)|Supporta la creazione e gestione dei siti di Active Directory, i collegamenti di sito, oggetti connessione e più con Windows PowerShell.|  
|[Controllo dinamico degli accessi](https://technet.microsoft.com/library/hh831717.aspx)|Nuova piattaforma di autorizzazione basata sulle attestazioni che migliora il modello di controllo di accesso legacy.|  
|[Interfaccia utente dei criteri granulari per le Password](https://technet.microsoft.com/library/hh831702.aspx#fine_grained_pswd_policy_mgmt)|Centro aggiunge il supporto di interfaccia utente grafica per la creazione, modifica e l'assegnazione di oggetti Impostazioni password aggiunti in origine in Windows Server 2008.|  
|[Group Managed Service Accounts (gMSA)](https://technet.microsoft.com/library/hh831782.aspx)|Un nuova protezione tipo di entità noto come un gestito. Servizi in esecuzione su più host possono essere eseguiti con lo stesso account gestito.|  
|[Aggiunta a dominio Offline di DirectAccess](https://technet.microsoft.com/library/jj574150.aspx)|Estende aggiunta a dominio offline mediante l'inclusione dei prerequisiti di DirectAccess.|  
|[Distribuzione rapida attraverso la clonazione (DC) controller di dominio virtuale](https://technet.microsoft.com/library/hh831734.aspx#virtualized_dc_cloning)|È possibile distribuire rapidamente controller di dominio virtualizzati attraverso la clonazione dei controller di dominio virtuali esistenti utilizzando i cmdlet di Windows PowerShell.|  
|[Modifiche al pool di RID](https://technet.microsoft.com/library/jj574229.aspx)|Aggiunge nuovi eventi di monitoraggio e quote come protezione contro un consumo eccessivo del pool di RID globale. Facoltativamente, raddoppia le dimensioni del pool di RID globale se il pool originale si esaurisce.|  
|Servizio tempo di protezione|Aumenta la sicurezza W32tm rimozione dei segreti dalle trasmissioni, rimuovendo le funzioni hash MD5 e che sia il server per l'autenticazione con i client ora Windows 8|  
|[Protezione da rollback degli USN per i controller di dominio virtualizzati](https://technet.microsoft.com/library/hh831734.aspx#safe_virt_dc)|Il ripristino accidentale backup snapshot di controller di dominio virtualizzati non provoca rollback degli USN.|  
|[Visualizzatore della cronologia di PowerShell di Windows](https://technet.microsoft.com/library/hh831702.aspx#windows_powershell_history_viewer)|Consentire agli amministratori di visualizzare i comandi di Windows PowerShell eseguiti durante l'utilizzo di centro.|  
  
### <a name="BKMK_"></a>Manutenzione automatica e le modifiche apportate al riavvio dopo l'applicazione degli aggiornamenti tramite Windows Update  
Prima del rilascio di Windows 8, Windows Update gestiva la pianificazione interna per controllare gli aggiornamenti e per scaricarli e installarli. È necessario che l'agente Windows Update è sempre in esecuzione in background, consumando memoria e altre risorse di sistema.  
  
Windows 8 and Windows Server 2012 introduce a new feature called [Automatic Maintenance](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx). Manutenzione automatica Consolida più funzionalità diverse, ciascuna usata per gestire la pianificazione e la logica di esecuzione. This consolidation allows for all these components to use far less system resources, work consistently, respect the new [Connected Standby](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) state for new device types, and consume less battery on portable devices.  
  
Poiché Windows Update è una parte di manutenzione automatica in Windows 8 e Windows Server 2012, la pianificazione interna per l'impostazione di giorno e ora per installare gli aggiornamenti non è più efficace. Per garantire il comportamento di riavvio coerente e prevedibile per tutti i dispositivi e computer nell'organizzazione, inclusi quelli che eseguono Windows 8 e Windows Server 2012, è possibile configurare le impostazioni di criteri di gruppo seguenti:  
  
-   **Configurazione computer | Criteri | Modelli amministrativi | Componenti di Windows | Windows Update | Configurare gli aggiornamenti automatici**  
  
-   **Configurazione computer | Criteri | Modelli amministrativi | Componenti di Windows | Windows Update | Nessun riavvio automatico per installazioni pianificate**  
  
-   **Configurazione computer | Criteri | Modelli amministrativi | Componenti di Windows | Pianificazione di manutenzione | Ritardo casuale manutenzione**  
  
Nella tabella seguente sono elencati alcuni esempi di come configurare queste impostazioni per consentire il riavvio del sistema desiderato.  
  
|||  
|-|-|  
|**Scenario**|**Configurazioni consigliate**|  
|**Gestito da WSUS**<br /><br />-Installare gli aggiornamenti una volta alla settimana<br />-Riavviare il venerdì alle 23|Impostare i computer per l'installazione automatica, impedire il riavvio automatico fino all'ora desiderata<br /><br />**Criteri**: Configura Aggiornamenti automatici (attivato)<br /><br />Configura aggiornamento automatico: 4 - download automatico e pianificazione dell'installazione<br /><br />**Criteri**: Escludi riavvio automatico con gli utenti connessi (disabilitata)<br /><br />**Scadenze WSUS**: impostare su venerdì alle 23|  
|**Gestito da WSUS**<br /><br />-Sfalsare le installazioni su ore/giorni diversi|Set di gruppi di destinazione per gruppi diversi di computer che devono essere aggiornati insieme<br /><br />Usare i passaggi dello scenario precedente<br /><br />Impostare scadenze diverse per gruppi di destinazione diversi|  
|**Non gestito da WSUS - Nessun supporto per le scadenze**<br /><br />-Sfalsare le installazioni in momenti diversi|**Criteri**: Configura Aggiornamenti automatici (attivato)<br /><br />Configura aggiornamento automatico: 4 - download automatico e pianificazione dell'installazione<br /><br />**Registry key:** Enable the registry key discussed in Microsoft KB article [2835627](https://support.microsoft.com/kb/2835627)<br /><br />**Criterio:** ritardo casuale manutenzione automatica (abilitato)<br /><br />Impostare **ritardo casuale manutenzione regolare** a PT6H per ritardo casuale di 6 ore per fornire il comportamento seguente:<br /><br />-Gli aggiornamenti verranno installati all'ora di manutenzione configurata più un ritardo casuale<br /><br />-Il riavvio per ogni computer verrà eseguito esattamente 3 giorni<br /><br />In alternativa, impostare un tempo di manutenzione diversa per ogni gruppo di computer|  
  
Per ulteriori informazioni sui motivi per cui il team tecnico Windows ha implementato queste modifiche, vedere [riducendo al minimo riavvii dopo l'aggiornamento automatico in Windows Update](http://blogs.msdn.com/b/b8/archive/2011/11/14/minimizing-restarts-after-automatic-updating-in-windows-update.aspx).  
  
## <a name="BKMK_InstallationChanges"></a>Modifiche installazione del ruolo server di dominio Active Directory  
In Windows Server 2003 a Windows Server 2008 R2, è stato eseguito il x86 o X64 versione dello strumento da riga di comando di Adprep.exe prima di eseguire l'installazione guidata Active Directory, Dcpromo.exe e Dcpromo.exe aveva varianti facoltative per l'installazione da supporto o per l'installazione automatica.  
  
A partire da Windows Server 2012, le installazioni da riga di comando vengono eseguite mediante il modulo ADDSDeployment in Windows PowerShell. Promozioni con interfaccia grafica vengono eseguite in Server Manager mediante una configurazione guidata di Active Directory completamente nuova. Per semplificare il processo di installazione, ADPREP è stato integrato nell'installazione di servizi di dominio Active Directory e viene eseguito automaticamente in base alle esigenze. La configurazione guidata Directory basate su Windows PowerShell di Active Directory fa automaticamente riferimento i ruoli di master schema e infrastrutture nei domini in cui vengono aggiunti i controller di dominio, quindi esegue in remoto i comandi ADPREP necessari sui controller di dominio pertinenti.  
  
I controlli dei prerequisiti nell'installazione guidata di Active Directory identificano i potenziali errori prima dell'inizio dell'installazione. È possibile correggere le condizioni di errore per eliminare il rischio di eseguire un aggiornamento parziale. Inoltre, la procedura guidata Esporta uno script di Windows PowerShell contenente tutte le opzioni specificate durante l'installazione grafica.  
  
Le modifiche di installazione di servizi di dominio Active Directory nel loro insieme, semplificano il processo di installazione del ruolo di controller di dominio e riducono le probabilità di errori amministrativi, specialmente in caso di distribuzione di più controller di dominio in aree e domini globali.  
More detailed information on GUI and Windows PowerShell-based installations, including command line syntax and step-by-step wizard instructions, see [Install Active Directory Domain Services](https://technet.microsoft.com/library/hh472162.aspx). Per gli amministratori che desiderano controllare l'introduzione delle modifiche dello schema in una foresta di Active Directory indipendente dall'installazione di controller di dominio di Windows Server 2012 in una foresta esistente, è ancora possibile eseguirlo comandi di Adprep.exe in un prompt dei comandi con privilegi elevati.  
  
## <a name="BKMK_DeprecatedFeatures"></a>Funzionalità deprecate e modifiche del comportamento relative a servizi di dominio Active Directory in Windows Server 2012  
Esistono alcune modifiche relative ai servizi di dominio Active Directory:  
  
-   **Elementi deprecati di Adprep32.exe**  
  
    Esiste solo una versione di Adprep.exe e può essere eseguita in base alle necessità nei server a 64 bit che eseguono Windows Server 2008 o versione successiva. Possono essere eseguito in remoto e deve essere eseguito in modalità remota se le operazioni di ruolo di master è ospitato in un sistema operativo a 32 bit o Windows Server 2003 che di destinazione.  
  
-   **Dcpromo.exe è deprecato**  
  
    Dcpromo è deprecato Sebbene in Windows Server 2012 solo, è ancora possibile eseguirlo con un file di risposte o parametri della riga di comando per dare tempo alle organizzazioni di passare dall'automazione esistente alle nuove opzioni di installazione di Windows PowerShell.  
  
-   **LMHash è disabilitato sugli account utente**  
  
    Le impostazioni predefinite sicure in sicurezza modelli in Windows Server 2008, Windows Server 2008 R2 e Windows Server 2012 abilitano il criterio NoLMHash disabilitato nei modelli di sicurezza di Windows 2000 e controller di dominio di Windows Server 2003. Disable the NoLMHash policy for LMHash-dependent clients as required, using the steps in KB article [946405](https://support.microsoft.com/kb/946405).  
  
A partire da Windows Server 2008, i controller di dominio dispongono anche le seguenti impostazioni predefinite sicure, rispetto ai controller di dominio che eseguono Windows Server 2003 o Windows 2000.  
  
|||||  
|-|-|-|-|  
|Tipo di crittografia o criteri|Impostazione predefinita in Windows Server 2008|Impostazione predefinita in Windows Server 2012 e Windows Server 2008 R2|Commento|  
|AllowNT4Crypto|Disabilitato|Disabilitato|I client Server Message Block (SMB) di terze parti potrebbero non essere compatibili con le impostazioni predefinite sicure nei controller di dominio. In tutti i casi, queste impostazioni possono essere ridotte per consentire l'interoperabilità, ma solo a scapito della sicurezza. For more information, see [article 942564](https://go.microsoft.com/fwlink/?LinkId=164558) in the Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=164558).|  
|DES|Abilitato|Disabilitato|[Article 977321](https://go.microsoft.com/fwlink/?LinkId=177717) in the Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=177717)|  
|CBT/protezione estesa per l'autenticazione integrata|N/D|Abilitato|See [Microsoft Security Advisory (937811)](https://go.microsoft.com/fwlink/?LinkId=164559) (https://go.microsoft.com/fwlink/?LinkId=164559) and [article 976918](https://go.microsoft.com/fwlink/?LinkId=178251) in the Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=178251).<br /><br />Review and install the hotfix in [article 977073](https://go.microsoft.com/fwlink/?LinkId=186394) (https://go.microsoft.com/fwlink/?LinkId=186394) in the Microsoft Knowledge Base as required.|  
|LMv2|Abilitato|Disabilitato|[Article 976918](https://go.microsoft.com/fwlink/?LinkId=178251) in the Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=178251)|  
  
## <a name="BKMK_SysReqs"></a>Requisiti del sistema operativo  
Nella tabella seguente sono elencati i requisiti minimi di sistema per Windows Server 2012. For more information about system requirements and pre-installation information, see [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). Non sono previsti requisiti di sistema aggiuntive per installare una nuova foresta di Active Directory, ma è consigliabile aggiungere memoria sufficiente per memorizzare nella cache il contenuto del database di Active Directory per migliorare le prestazioni dei controller di dominio, le richieste client LDAP e applicazioni abilitate per Active Directory. Se si esegue l'aggiornamento di un controller di dominio esistente o aggiunge un nuovo controller di dominio a una foresta esistente, esaminare la sezione seguente per verificare che il server soddisfi i requisiti di spazio su disco.  
  
|||  
|-|-|  
|Processore|1,4 bit da Ghz 64 processori|  
|RAM|512 MB|  
|Requisiti di spazio su disco disponibile|32 GB|  
|Risoluzione dello schermo|800 x 600 o superiore|  
|Varie|Unità DVD, tastiera, accesso a Internet|  
  
### <a name="BKMK_DiskSpaceDCWin8"></a>Requisiti di spazio su disco per l'aggiornamento dei controller di dominio  
Questa sezione vengono descritti i requisiti di spazio su disco solo per l'aggiornamento dei controller di dominio da Windows Server 2008 o Windows Server 2008 R2. For more information about disk space requirements for upgrading domain controllers to earlier versions of Windows Server, see [Disk space requirements for upgrading to Windows Server 2008](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008) or [Disk space requirements for upgrading to Windows Server 2008 R2](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008R2).  
  
Dimensionare il disco che ospita i file di registro e il database di Active Directory per supportare le estensioni dello schema personalizzate e basate su applicazione, applicazioni e gli indici avviati dall'amministratore, più spazio per gli oggetti e attributi che verranno aggiunte alla directory ciclo di vita di distribuzione del controller di dominio (in genere da 5 a 8 anni). Un corretto dimensionamento al momento della distribuzione è in genere un buon investimento, rispetto ai costi molto più elevati necessari per espandere l'archiviazione su disco dopo la distribuzione. For more information, see [Capacity Planning for Active Directory Domain Services](https://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx).  
  
Nei controller di dominio che si prevede di eseguire l'aggiornamento, assicurarsi che l'unità che ospita Active Directory database (NTDS. DIT) dispone di spazio su disco equivalente almeno al 20% del NTDS. File DIT prima di iniziare l'aggiornamento del sistema operativo. Se è presente spazio su disco insufficiente nel volume, l'aggiornamento può avere esito negativo e il rapporto sulla compatibilità restituisce un errore che indica lo spazio su disco insufficiente:  
  
In questo caso, puoi provare una deframmentazione non in linea del database di Active Directory per liberare spazio aggiuntivo e quindi riprovare l'aggiornamento. For more information, see [Compact the Directory Database File (Offline Defragmentation)](https://technet.microsoft.com/library/cc794920(v=WS.10).aspx).  
  
### <a name="available-skus"></a>SKU disponibili  
Esistono 4 edizioni di Windows Server: Foundation, Essentials, Standard e Datacenter.   
Le due edizioni che supportano il ruolo di dominio Active Directory sono Standard e Datacenter.  
  
Nelle versioni precedenti, edizioni di Windows Server differiscono supporto per i ruoli del server, contatori del processore e grandi quantità di memoria. Le edizioni Standard e Datacenter di Windows Server supportano tutte le funzionalità e hardware sottostante, ma hanno diritti di virtualizzazione diversi - per l'edizione Standard sono consentite due istanze virtuali e istanze virtuali illimitate consentite per l'edizione Datacenter.  
  
### <a name="windows-client-and-windows-server-operating-systems-that-are-supported-to-join-windows-server-domains"></a>Client Windows e sistemi operativi Windows Server supportati per l'aggiunta ai domini di Windows Server  
I seguenti client di Windows e sistemi operativi Windows Server sono supportate per i computer membri del dominio con controller di dominio che eseguono Windows Server 2012 o versione successiva:  
  
-   Sistemi operativi client: Windows 8.1, Windows 8, Windows 7, Windows Vista 
  
    Computer che eseguono Windows 8.1 o Windows 8 sono inoltre in grado di aggiungere domini con controller di dominio che eseguono una versione precedente di Windows Server, incluso Windows Server 2003 o versione successiva. In questo caso, tuttavia, alcune funzionalità di Windows 8 potrebbero richiedere una configurazione aggiuntiva o potrebbero non essere disponibili. For more information about those features and other recommendations for managing Windows 8 clients in downlevel domains, see [Running Windows 8 member computers in Windows Server 2003 domains](https://social.technet.microsoft.com/wiki/contents/articles/17361.running-windows-8-member-computers-in-windows-server-2003-domains.aspx).  
  
-   Sistemi operativi server: Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2, Windows Server 2003  
  
## <a name="BKMK_UpgradePaths"></a>Percorsi di aggiornamento sul posto supportati  
I controller di dominio che eseguono versioni a 64 bit di Windows Server 2008 o Windows Server 2008 R2 possono essere aggiornati a Windows Server 2012. È possibile aggiornare i controller di dominio che eseguono Windows Server 2003 o versioni a 32 bit di Windows Server 2008. Per sostituirli, installare controller di dominio che eseguono una versione successiva di Windows Server nel dominio e quindi rimuovere i controller di dominio di Windows Server 2003.  
  
|Se si eseguono queste edizioni|È possibile eseguire l'aggiornamento a queste edizioni|  
|-------------------------------------|-------------------------------------|  
|Windows Server 2008 Standard con SP2<br /><br />OR<br /><br />Windows Server 2008 Enterprise con SP2|Windows Server 2012 Standard<br /><br />OR<br /><br />Windows Server 2012 Datacenter|  
|Windows Server 2008 Datacenter con SP2|Windows Server 2012 Datacenter|  
|Windows Web Server 2008|Windows Server 2012 Standard|  
|Windows Server 2008 R2 Standard con SP1<br /><br />OR<br /><br />Windows Server 2008 R2 Enterprise Edition con SP1|Windows Server 2012 Standard<br /><br />OR<br /><br />Windows Server 2012 Datacenter|  
|Windows Server 2008 R2 Datacenter con SP1|Windows Server 2012 Datacenter|  
|Windows Web Server 2008 R2|Windows Server 2012 Standard|  
  
For more information about supported upgrade paths, see [Evaluation Versions and Upgrade Options for Windows Server 2012](https://go.microsoft.com/fwlink/?LinkId=260917). Si noti che è possibile convertire un controller di dominio che esegue una versione di valutazione di Windows Server 2012 direttamente a una versione definitiva. Al contrario, installare un controller di dominio aggiuntivo in un server che esegue una versione definitiva e rimuovere servizi di dominio Active Directory dal controller di dominio che esegue nella versione di valutazione.  
  
A causa di un problema noto, è possibile aggiornare un controller di dominio che esegue un'installazione Server Core di Windows Server 2008 R2 a un'installazione Server Core di Windows Server 2012. L'aggiornamento provoca una schermata nera nelle fasi finali del processo di aggiornamento. Il riavvio di questi controller di dominio espone un'opzione del file Boot. ini per eseguire il rollback alla versione precedente del sistema operativo. Un ulteriore riavvio attiva il rollback automatico alla versione precedente del sistema operativo. Fino a quando non è disponibile una soluzione, si consiglia di installare un nuovo controller di dominio che esegue un'installazione Server Core di Windows Server 2012 invece di un controller di dominio esistente che esegue un'installazione Server Core di Windows Server 2008 R2 di aggiornamento sul posto. For more information, see KB article [2734222](https://support.microsoft.com/kb/2734222).  
  
## <a name="BKMK_FunctionalLevels"></a>Requisiti e le funzionalità a livello funzionale  
 Windows Server 2012 richiede un livello di funzionalità foresta Windows Server 2003. Vale a dire, prima di poter aggiungere un controller di dominio che esegue Windows Server 2012 a una foresta di Active Directory esistente, il livello di funzionalità foresta deve essere Windows Server 2003 o versione successiva. Questo significa che i controller di dominio che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003 possono operare nella stessa foresta, ma i controller di dominio che eseguono Windows 2000 Server non sono supportati e blocca l'installazione di un controller di dominio che esegue Windows Server 2012. Se la foresta contiene controller di dominio che esegue Windows Server 2003 o versione successiva ma le funzionalità della foresta di livello è ancora Windows 2000, l'installazione viene ugualmente bloccata.  
  
I controller di dominio Windows 2000 devono essere rimossa prima di aggiungere i controller di dominio Windows Server 2012 all'insieme di strutture. In questo caso, prendere in considerazione il flusso di lavoro seguente:  
  
1.  Installare controller di dominio che eseguono Windows Server 2003 o versione successiva. Questi controller di dominio possono essere distribuiti in una versione di valutazione di Windows Server. This step also requires [running adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx) for that operating system release as a prerequisite.  
  
2.  Rimuovere i controller di dominio Windows 2000. In particolare, normalmente abbassare di livello o forzare la rimozione di controller di dominio Windows Server 2000 del dominio utilizzato Active Directory Users e computer per rimuovere gli account di controller di dominio per tutti i controller di dominio rimossi.  
  
3.  Aumentare il livello di funzionalità della foresta a Windows Server 2003 o versione successiva.  
  
4.  Installare controller di dominio che eseguono Windows Server 2012.  
  
5.  Rimuovere i controller di dominio che eseguono versioni precedenti di Windows Server.  
  
Il nuovo livello funzionalità dominio di Windows Server 2012 consente una nuova caratteristica: il **supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos** criterio dei modelli amministrativi KDC ha due impostazioni (**Fornisci sempre attestazioni** e **Rifiuta richieste di autenticazione non blindate**) che richiedono il livello di funzionalità dominio Windows Server 2012.  
  
Il livello di funzionalità foresta Windows Server 2012 non fornisce nuove funzionalità, ma garantisce che ogni nuovo dominio creato nella foresta venga automaticamente utilizzato a livello funzionale di dominio di Windows Server 2012. Il livello di funzionalità del dominio Windows Server 2012 non fornisce altre nuove funzionalità oltre al supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos. Ma garantisce che qualsiasi controller di dominio nel dominio esegue Windows Server 2012. Per ulteriori informazioni sulle altre funzionalità che sono disponibili a diversi livelli funzionali, vedere [livelli di funzionalità di servizi di dominio Active Directory informazioni (AD DS)](../active-directory-functional-levels.md).  
  
Dopo aver impostato il livello funzionale della foresta su un determinato valore, è possibile eseguire il rollback né abbassare il livello di funzionalità foresta, con le eccezioni seguenti: dopo aver aumentato il livello di funzionalità della foresta a Windows Server 2012, è possibile diminuirlo a Windows Server 2008 R2. Se Cestino per Active Directory non è stato abilitato, è inoltre possibile diminuire rollback del livello di funzionalità da Windows Server 2012 per Windows Server 2008 R2 o Windows Server 2008 o Windows Server 2008 R2 a Windows Server 2008 della foresta. Se il livello funzionale della foresta è impostato su Windows Server 2008 R2, non può essere eseguito il rollback, ad esempio, per Windows Server 2003.  
  
Dopo aver impostato il livello di funzionalità del dominio su un determinato valore, è possibile eseguire il rollback né abbassare il livello funzionale di dominio, con le eccezioni seguenti: l'opzione di ripristino dello stato il livello di funzionalità del dominio quando si aumenta il livello di funzionalità del dominio a Windows Server 2008 R2 o Windows Server 2012 e se il livello funzionale della foresta è Windows Server 2008 o inferiore, è necessario eseguire il backup per Windows Server 2008 o Windows Server 2008 R2. È possibile abbassare il livello di funzionalità del dominio solo da Windows Server 2012 per Windows Server 2008 R2 o Windows Server 2008 o Windows Server 2008 R2 a Windows Server 2008. Se il livello di funzionalità del dominio è impostato su Windows Server 2008 R2, non può essere eseguito il rollback, ad esempio, per Windows Server 2003.  
  
Per ulteriori informazioni sulle funzionalità disponibili a livelli di funzionalità inferiori, vedere [livelli di funzionalità di servizi di dominio Active Directory informazioni (AD DS)](../active-directory-functional-levels.md).  
  
Oltre a livelli di funzionalità, un controller di dominio che esegue Windows Server 2012 fornisce funzionalità aggiuntive che non sono disponibili in un controller di dominio che esegue una versione precedente di Windows Server. Ad esempio, un controller di dominio che esegue Windows Server 2012 utilizzabile per la clonazione del controller di dominio virtuale, operazione Impossibile con un controller di dominio che esegue una versione precedente di Windows Server. Clonazione del controller di dominio virtuale e le relative protezioni in Windows Server 2012 non è installato ma tutti i livelli di funzionalità.  
  
> [!NOTE]  
> Microsoft Exchange Server 2013 richiede un livello di funzionalità della foresta di Windows server 2003 o versione successiva.  
  
## <a name="BKMK_ServerRoles"></a>Interoperabilità di Active Directory con altri ruoli server e sistemi operativi Windows  
Servizi di dominio Active Directory non è supportato nei sistemi operativi Windows seguenti:  
  
-   Windows MultiPoint Server  
  
-   Windows Server 2012 Essentials  
  
Servizi di dominio Active Directory non è possibile installare in un server che esegue anche i seguenti ruoli del server o dei servizi ruolo:  
  
-   Hyper-V Server  
  
-   Gestore connessione Desktop remoto  
  
## <a name="BKMK_OpsMasters"></a>Ruoli di master operazioni  
Alcune nuove funzionalità in Windows Server 2012 influenzano i ruoli di master operazioni:  
  
-   L'emulatore PDC deve essere in esecuzione Windows Server 2012 per supportare la clonazione di controller di dominio virtuale. Vi sono altri prerequisiti per la clonazione del controller di dominio. For more information, see [Active Directory Domain Services (AD DS) Virtualization](https://technet.microsoft.com/library/hh831734.aspx).  
  
-   Quando l'emulatore PDC esegue Windows Server 2012, vengono create nuove entità di sicurezza.  
  
-   Il Master RID nuovo rilascio di RID e funzionalità di monitoraggio. I miglioramenti includono una migliore registrazione degli eventi, limiti più appropriati, e la possibilità di - in caso di emergenza - aumentare il RID globale allocazione del pool di un bit. Per ulteriori informazioni, vedere [gestione delle emissioni di RID](../../ad-ds/manage/Managing-RID-Issuance.md).  
  
> [!NOTE]  
> Sebbene non costituiscano ruoli di master operazioni, un'ulteriore modifica all'installazione di Active Directory è che il ruolo server DNS e il catalogo globale vengono installati per impostazione predefinita su tutti i controller di dominio che eseguono Windows Server 2012.  
  
## <a name="BKMK_Virtual"></a>Virtualizzazione dei controller di dominio  
Miglioramenti di inizio di dominio Active Directory in Windows Server 2012 abilitano una virtualizzazione più sicura dei controller di dominio e la possibilità di clonare i controller di dominio. La clonazione del controller di dominio a sua volta consente la distribuzione rapida di controller di dominio aggiuntivo in un nuovo dominio e altri vantaggi. Per ulteriori informazioni, vedere [Introduzione a servizi di dominio Active Directory e 40 #; Servizi di dominio Active Directory & #41; Virtualizzazione & #40; Livello 100 & #41; ](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).  
  
## <a name="BKMK_Admin"></a>Amministrazione dei server Windows Server 2012  
Use the [Remote Server Administration Tools for Windows 8](https://www.microsoft.com/download/details.aspx?id=28972) to manage domain controllers and other servers that run  Windows Server 2012 . È possibile eseguire Windows Server 2012 Server strumenti di amministrazione remota in un computer che esegue Windows 8.  
  
## <a name="BKMK_AppCompat"></a>Compatibilità delle applicazioni  
Nella tabella seguente descrive le applicazioni Microsoft integrate in Active Directory comune. La tabella indica le versioni di Windows Server che le applicazioni possono essere installate e che l'introduzione dei controller di dominio di Windows Server 2012 influisce sulla compatibilità delle applicazioni.  
  
|Prodotto|Note|  
|-----------|---------|  
|[Microsoft Configuration Manager 2007](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|Configuration Manager 2007 con SP2 (include Configuration Manager 2007 R2 e Configuration Manager 2007 R3):<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter **Nota:** anche se queste versioni siano pienamente supportate come client, non è prevista per aggiungere il supporto per la distribuzione di tali versioni come sistemi operativi utilizzando la funzionalità di distribuzione del sistema operativo di Configuration Manager 2007. Inoltre, non i server del sito o sistemi del sito saranno supportati in qualsiasi SKU di Windows Server 2012.|  
|[Microsoft SharePoint 2007](https://support.microsoft.com/kb/2728964)|Microsoft Office SharePoint Server 2007 non è supportata per l'installazione in Windows Server 2012.|  
|[Microsoft SharePoint 2010](https://support.microsoft.com/kb/2724471)|Per installare e utilizzare è richiesto SharePoint 2010 Service Pack 2 <br />SharePoint 2010 sui server Windows Server 2012<br /><br />Per installare e il funzionamento di SharePoint Foundation 2010 sui server di Windows Server 2012 è richiesto SharePoint 2010 Foundation Service Pack 2<br /><br />Il processo di installazione di SharePoint Server 2010 (senza service pack) non riesce in Windows Server 2012<br /><br />L'installazione dei prerequisiti SharePoint Server 2010 (PrerequisiteInstaller.exe) non riesce con errore "il programma presenta problemi di compatibilità". Fare clic su "Esegui il programma senza visualizzare la Guida" viene visualizzato l'errore "verifica se SharePoint può essere installato & #124; Impossibile installare SharePoint Server 2010 (senza service pack) in Windows Server 2012."|  
|[Microsoft SharePoint 2013](https://technet.microsoft.com/library/cc262485(v=office.15).aspx)|Requisiti minimi per un server di database in una farm<br /><br />L'edizione a 64 bit di Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Enterprise o Datacenter o l'edizione a 64 bit di Windows Server 2012 Standard o Datacenter<br /><br />Requisiti minimi per un singolo server con database incorporato:<br /><br />L'edizione a 64 bit di Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Enterprise o Datacenter o l'edizione a 64 bit di Windows Server 2012 Standard o Datacenter<br /><br />Requisiti minimi per server web front-end e server di applicazioni in una farm:<br /><br />Edizione a 64 bit di Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Enterprise o Datacenter o l'edizione a 64 bit di Windows Server 2012 Standard o Datacenter.|  
|[Microsoft System Center Configuration Manager 2012](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|System Center 2012 Configuration Manager Service Pack 1:<br /><br />Microsoft aggiungerà alla matrice del supporto client con la versione di Service Pack 1 sistemi operativi seguenti:<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter<br /><br />Tutti i ruoli del server del sito, inclusi i server del sito, il provider SMS e punti di gestione - è possibile distribuire ai server con le seguenti edizioni del sistema operativo:<br /><br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|  
|[Microsoft Lync Server 2013](https://technet.microsoft.com/library/gg412883.aspx)|Lync Server 2013 richiede con Windows Server 2008 R2 o Windows Server 2012. Non è possibile eseguire in un'installazione Server Core. It can be run on [virtual servers](https://technet.microsoft.com/library/gg399035.aspx).|  
|[Lync Server 2010](https://support.microsoft.com/kb/2777359)|Lync Server 2010 can be installed on a new (not upgraded) installation Windows Server 2012 if [October 2012 cumulative updates for Lync Server](https://support.microsoft.com/?kbid=2493736) are installed. Aggiornamento del sistema operativo a Windows Server 2012 per un'installazione esistente di Lync Server 2010 non è supportato. Microsoft Lync Server 2010 Group Chat Server non è supportato in Windows Server 2012.|  
|[System Center 2012 Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|System Center 2012 Endpoint Protection Service Pack 1 aggiorna la matrice del supporto client per includere i seguenti sistemi operativi<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|  
|[System Center 2012 Forefront Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|Forefront Endpoint Protection 2010 con aggiornamento cumulativo 1 aggiorna la matrice del supporto client per includere i sistemi operativi seguenti:<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|  
|Forefront Threat Management Gateway (TMG)|TMG è supportata per l'esecuzione solo in Windows Server 2008 e Windows Server 2008 R2. For more information, see [System requirements for Forefront TMG](https://technet.microsoft.com/library/dd896981.aspx).|  
|Windows Server Update Services|Questa versione di WSUS supporta già i computer basati su Windows 8 o computer basati su Windows Server 2012 come client.|  
|Windows Server Update Services 3.0|Update KB article [2734608](https://support.microsoft.com/kb/2734608) lets servers that are running Windows Server Update Services (WSUS) 3.0 SP2 provide updates to computers that are running Windows 8 or Windows Server 2012: **Note:** Customers with standalone WSUS 3.0 SP2 environments or System Center Configuration Manager 2007 Service Pack 2 environments with WSUS 3.0 SP2 require [2734608](https://support.microsoft.com/kb/2734608) to properly manage Windows 8-based computers or Windows Server 2012-based computers as clients.|  
|[Exchange 2013](https://technet.microsoft.com/library/bb691354.aspx)|Windows Server 2012 Standard e Datacenter sono supportate per i ruoli seguenti: master schema, server di catalogo globale, controller di dominio, ruolo del server accesso client e cassetta postale<br /><br />Livello di funzionalità della foresta: Windows Server 2003 o superiore<br /><br />Origine: Requisiti di sistema Exchange 2013|  
|Exchange 2010|[Origine: Exchange 2010 Service Pack 3](https://blogs.technet.com/b/exchange/archive/2012/09/25/announcing-exchange-2010-service-pack-3.aspx)<br /><br />Può essere installato Exchange 2010 con Service Pack 3 sui server membri di Windows Server 2012.<br /><br />[Exchange 2010 System Requirements](https://technet.microsoft.com/library/aa996719(EXCHG.141).aspx) lists the latest supported schema master, global catalog and domain controller as Windows Server 2008 R2.<br /><br />Livello di funzionalità della foresta: Windows Server 2003 o superiore|  
|SQL Server 2012|Source: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />SQL Server 2012 RTM è supportato in Windows Server 2012.|  
|SQL Server 2008 R2|Source: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Per installare Windows Server 2012 è necessario SQL Server 2008 R2 con Service Pack 1 o versione successiva.|  
|SQL Server 2008|Source: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Per installare Windows Server 2012 è necessario SQL Server 2008 con Service Pack 3 o versione successiva.|  
|SQL Server 2005|Source: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Non è supportato per installare Windows Server 2012.|  
  
## <a name="BKMK_KnownIssues"></a>Problemi noti  
Nella tabella seguente sono elencati i problemi noti relativi all'installazione di Active Directory.  
  
||||  
|-|-|-|  
|Titolo e il numero di articolo KB|Area tecnologica interessata|Descrizione del problema /|  
|[2830145](https://support.microsoft.com/kb/2830145): SID S-1-18-1 and SID S-1-18-2 can't be mapped on Windows 7 or Windows Server 2008 R2-based computers in a domain environment|AD DS Gestione/compatibilità delle applicazioni|Applicazioni che eseguono il mapping SID S-1-18-1 e SID S-1-18-2, sono una novità in Windows Server 2012, potrebbero avere esito negativo perché i SID non possono essere risolti in computer basati su Windows Server 2008 R2 o Windows 7. Per risolvere questo problema, installare l'hotfix nei computer basati su Windows Server 2008 R2 e Windows 7 nel dominio.|  
|[2737129](https://support.microsoft.com/kb/2737129): Group Policy preparation is not performed when you automatically prepare an existing domain for Windows Server 2012|Installazione di Active Directory|Adprep /domainprep /gpprep non viene eseguita automaticamente come parte dell'installazione il primo controller di dominio che esegue Windows Server 2012 in un dominio. Se non è mai stato eseguito in precedenza nel dominio, deve essere eseguito manualmente.|  
|[2737416](https://support.microsoft.com/kb/2737416): Windows PowerShell-based domain controller deployment repeats warnings|Installazione di Active Directory|Gli avvisi di essere visualizzati durante la convalida dei prerequisiti e nuovamente visualizzati durante l'installazione.|  
|[2737424](https://support.microsoft.com/kb/2737424): "Format of the specified domain name is invalid" error when you try to remove Active Directory Domain Services from a domain controller|Installazione di Active Directory|Questo errore viene visualizzato se si sta rimuovendo l'ultimo controller di dominio in un dominio in cui gli account di controller di dominio creati in precedenza sono ancora disponibili. Questa azione influisce su Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008.|  
|[2737463](https://support.microsoft.com/kb/2737463): Domain controller does not start, c00002e2 error occurs, or "Choose an option" is displayed|Installazione di Active Directory|Un controller di dominio non viene avviato perché un amministratore ha utilizzato Dism.exe, Pkgmgr.exe o Ocsetup.exe per rimuovere il ruolo DirectoryServices-DomainController.|  
|[2737516](https://support.microsoft.com/kb/2737516): IFM verification limitations in Windows Server 2012 Server Manager|Installazione di Active Directory|Verifica IFM può presentare limitazioni, come illustrato nell'articolo della Knowledge Base.|  
|[2737535](https://support.microsoft.com/kb/2737535): Install-AddsDomainController cmdlet returns parameter set error for RODC|Installazione di Active Directory|È possibile ricevere un errore quando si tenta di associare un server a un account di controller di dominio se si specificano argomenti già popolati nell'account del controller di dominio creato in precedenza.|  
|[2737560](https://support.microsoft.com/kb/2737560): "Unable to perform Exchange schema conflict check" error, and prerequisites check fails|Installazione di Active Directory|Controllo dei prerequisiti non riesce quando si configura il primo Windows Server 2012 controller di dominio in un dominio esistente perché i controller di dominio mancano le SeServiceLogonRight per servizio di rete o perché i protocolli WMI o DCOM sono bloccati.|  
|[2737797](https://support.microsoft.com/kb/2737797): AddsDeployment module with the -Whatif argument shows incorrect DNS results|Installazione di Active Directory|Il parametro - WhatIf Mostra DNS server non verrà installato, ma sarà.|  
|[2737807](https://support.microsoft.com/kb/2737807): The Next button is not available on the Domain Controller Options page|Installazione di Active Directory|Il pulsante Avanti è disabilitato nella pagina Opzioni Controller di dominio, perché l'indirizzo IP del controller di dominio di destinazione non è mappato a una subnet o sito esistente, oppure perché la password modalità ripristino servizi directory non è stata digitata e confermata correttamente.|  
|[2737935](https://support.microsoft.com/kb/2737935): Active Directory installation stalls at the "Creating the NTDS settings object" stage|Installazione di Active Directory|L'installazione si interrompe perché la password dell'amministratore locale corrisponde alla password di amministratore di dominio o perché completamento della replica critica di evitare problemi di rete.|  
|[2738060](https://support.microsoft.com/kb/2738060): "Access is denied" error message when you create a child domain remotely by using Install-AddsDomain|Installazione di Active Directory|L'errore viene visualizzato quando si esegue Install-ADDSDomain con il cmdlet Invoke-Command se la DNSDelegationCredential è una password errata.|  
|[2738697](https://support.microsoft.com/kb/2738697): "The server is not operational" domain controller configuration error when you configure a server by using Server Manager|Installazione di Active Directory|Questo errore viene visualizzato quando si tenta di installare servizi di dominio Active Directory in un computer del gruppo di lavoro perché l'autenticazione NTLM è disabilitata.|  
|[2738746](https://support.microsoft.com/kb/2738746): You receive access denied errors after you log on to a local administrator domain account|Installazione di Active Directory|Quando si accede utilizzando un account amministratore locale anziché l'account Administrator predefinito e quindi crea un nuovo dominio, l'account non viene aggiunto al gruppo Domain Admins.|  
|[2743345](https://support.microsoft.com/kb/2743345): "The system cannot find the file specified" Adprep /gpprep error, or tool crashes|Installazione di Active Directory|Questo errore viene visualizzato quando si esegue adprep /gpprep perché il master infrastrutture implementa uno spazio dei nomi indipendente|  
|[2743367](https://support.microsoft.com/kb/2743367): Adprep "not a valid Win32 application" error on Windows Server 2003, 64-bit version|Installazione di Active Directory|Questo errore viene visualizzato perché non è possibile eseguire Windows Server 2012 Adprep su Windows Server 2003.|  
|[2753560](https://support.microsoft.com/kb/2753560): ADMT 3.2 and PES 3.1 installation errors on Windows Server 2012|ADMT|Impossibile installare ADMT 3.2 su Windows Server 2012 per impostazione predefinita.|  
|[2750857](https://support.microsoft.com/kb/2750857): DFS Replication diagnostic reports do not display correctly in Internet Explorer 10|Replica DFS|Rapporto di diagnostica di replica DFS non vengono visualizzati correttamente a causa delle modifiche in Internet Explorer 10.|  
|[2741537](https://support.microsoft.com/kb/2741537): Remote Group Policy updates are visible to users|Criteri di gruppo|Ciò è dovuto attività pianificate eseguite nel contesto di ogni utente che ha effettuato l'accesso. La progettazione di utilità di pianificazione Windows richiede un prompt interattivo in questo scenario.|  
|[2741591](https://support.microsoft.com/kb/2741591): ADM files are not present in SYSVOL in the GPMC Infrastructure Status option|Criteri di gruppo|Replica di criteri di gruppo può segnalare "replica in corso" perché lo stato dell'infrastruttura di gestione criteri di gruppo non vengono seguite le regole di filtro personalizzate.|  
|[2737880](https://support.microsoft.com/kb/2737880): "The service cannot be started" error during AD DS configuration|La clonazione del controller di dominio virtuale|Questo errore viene visualizzato durante l'installazione o rimozione di servizi di dominio Active Directory o la clonazione, perché il servizio ruolo Server DS è disattivato.|  
|[2742836](https://support.microsoft.com/kb/2742836): Two DHCP leases are created for each domain controller when you use the VDC cloning feature|La clonazione del controller di dominio virtuale|Ciò accade perché il controller di dominio clonato ha ricevuto un lease prima della clonazione e nuovamente quando la clonazione è stata completata.|  
|[2742844](https://support.microsoft.com/kb/2742844): Domain controller cloning fails and the server restarts in DSRM in Windows Server 2012|La clonazione del controller di dominio virtuale|Il controller di dominio clonato viene avviato in modalità ripristino servizi directory perché la clonazione non è riuscita per uno qualsiasi dei vari motivi illustrati nell'articolo della Knowledge Base.|  
|[2742874](https://support.microsoft.com/kb/2742874): Domain controller cloning does not re-create all service principal names|La clonazione del controller di dominio virtuale|Alcuni SPN a tre parti non vengono ricreati nel controller di dominio clonato a causa di una limitazione del processo di ridenominazione del dominio.|  
|[2742908](https://support.microsoft.com/kb/2742908): "No logon servers are available" error after cloning domain controller|La clonazione del controller di dominio virtuale|Questo errore viene visualizzato quando si tenta di accedere dopo la clonazione di un controller di dominio virtualizzato perché la clonazione non è riuscito e il controller di dominio avviato in modalità ripristino servizi directory. Accedere come. \administrator a risolvere il problema della clonazione.|  
|[2742916](https://support.microsoft.com/kb/2742916): Domain controller cloning fails with error 8610 in dcpromo.log|La clonazione del controller di dominio virtuale|Errore di clonazione perché l'emulatore PDC non ha eseguito la replica in ingresso della partizione di dominio, probabilmente perché il ruolo è stato trasferito.|  
|[2742927](https://support.microsoft.com/kb/2742927): "Index was out of range" New-AdDcCloneConfig error|La clonazione del controller di dominio virtuale|Dopo aver eseguito il cmdlet New-ADDCCloneConfigFile durante la clonazione di controller di dominio virtuali, il cmdlet non è stato eseguito da un prompt dei comandi con privilegi elevati oppure perché il token di accesso non contiene il gruppo Administrators, viene visualizzato l'errore.|  
|[2742959](https://support.microsoft.com/kb/2742959): Domain controller cloning fails with error 8437: "invalid parameter was specified for this replication operation"|La clonazione del controller di dominio virtuale|La clonazione non riuscita perché è stato specificato un nome di clone non valido o un nome NetBIOS duplicato.|  
|[2742970](https://support.microsoft.com/kb/2742970): DC Cloning fails with no DSRM, duplicate source and clone computer|La clonazione del controller di dominio virtuale|Il controller di dominio virtuale clonato viene avviato in modalità servizi Directory riparazione (DSRM), utilizzando un nome duplicato come controller di dominio di origine, perché non è stato creato il file DCCloneConfig.xml nel percorso corretto o perché il controller di dominio di origine è stato riavviato prima della clonazione.|  
|[2743278](https://support.microsoft.com/kb/2743278): Domain controller cloning error 0x80041005|La clonazione del controller di dominio virtuale|Il controller di dominio clonato viene avviato in modalità ripristino servizi directory perché è stato specificato un solo server WINS. Se viene specificato un server WINS, è necessario specificare server preferito e alternativo WINS.|  
|[2745013](https://support.microsoft.com/kb/2745013): "Server is not operational" error message if you run New-AdDcCloneConfigFile in Windows Server 2012|La clonazione del controller di dominio virtuale|Questo errore viene visualizzato dopo aver eseguito il cmdlet New-ADDCCloneConfigFile perché il server non riesce a contattare un server di catalogo globale.|  
|[2747974](https://support.microsoft.com/kb/2747974): Domain controller cloning event 2224 provides incorrect guidance|La clonazione del controller di dominio virtuale|ID evento 2224 segnala erroneamente che l'account del servizio gestiti deve essere rimossa prima della clonazione. Servizio gestito autonomi e deve essere rimosso, ma questi account non bloccano la clonazione.|  
|[2748266](https://support.microsoft.com/kb/2748266): You cannot unlock a BitLocker-encrypted drive after you upgrade to Windows 8|BitLocker|Viene visualizzato un errore "Impossibile trovare l'applicazione" quando si tenta di sbloccare un'unità in un computer che è stato aggiornato da Windows 7.|  
  
## <a name="see-also"></a>Vedere anche  
[Risorse di valutazione di Windows Server 2012](https://technet.microsoft.com/evalcenter/hh708766.aspx)  
[Guida alla valutazione di Windows Server 2012](https://download.microsoft.com/download/5/B/2/5B254183-FA53-4317-B577-7561058CEF42/WS%202012%20Evaluation%20Guide.pdf)  
[Installare e distribuire Windows Server 2012](https://technet.microsoft.com/library/hh831620.aspx)  
  


