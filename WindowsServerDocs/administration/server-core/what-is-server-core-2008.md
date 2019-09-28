---
title: Che cos'è Server Core 2008?
description: Informazioni sull'opzione di installazione dei componenti di base del server in Windows Server 2008
ms.prod: windows-server
ms.author: helohr
ms.date: 11/01/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: 0a109d0bfc4fc09b5e8097059d68b728d17752a6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383376"
---
# <a name="what-is-server-core-2008"></a>Che cos'è Server Core 2008?
>Si applica a: Windows Server 2008

>[!NOTE]
>Queste informazioni si applicano a Windows Server 2008. Per informazioni su Server Core in Windows Server, vedere [che cos'è l'installazione dei componenti di base del server in Windows Server](https://docs.microsoft.com/windows-server/administration/server-core/what-is-server-core). 

L'opzione componenti di base del server è una nuova opzione di installazione minima disponibile quando si distribuisce l'edizione standard, Enterprise o Datacenter di Windows Server 2008. Server Core fornisce un'installazione minima di Windows Server 2008 che supporta l'installazione di solo determinati ruoli del server, come descritto più avanti in questo capitolo. Si confronti con l'opzione di installazione completa per Windows Server 2008, che supporta l'installazione di tutti i ruoli server disponibili e anche altre applicazioni server Microsoft o di terze parti, ad esempio Microsoft Exchange Server o SAP. 

Prima di continuare, è necessario spiegare la frase "opzione di installazione". In genere, quando si acquista una copia di Windows Server 2008, si acquista una licenza per l'uso di determinate edizioni o di unità riservate (SKU). La tabella 1-1 elenca le varie edizioni di Windows Server 2008 disponibili. La tabella indica anche le opzioni di installazione (completo, Server Core o entrambi) disponibili per ogni edizione.

**Tabella 1-1** Edizioni di Windows Server 2008 e supporto per le opzioni di installazione

| Edizione       | Completi          | Server Core  |
| ------------- | :-------------: | :------------: |
| Windows Server 2008 standard (x86 e x64)       | x | x        |
| Windows Server 2008 Enterprise (x86 e x64)       | x | x        |
| Windows Server 2008 Datacenter (x86 e x64)        | x | x       |
| Windows Web Server 2008 (x86 e x64)       | x | x  |
| Windows Server 2008 per sistemi basati su Itanium       | x |     |
| Windows HPC Server 2008 (solo x64)       | x |   |
| Windows Server 2008 standard senza Hyper-V (x86 e x64) | x | x |
| Windows Server 2008 Enterprise senza Hyper-V (x86 e x64)  | x | x |
| Windows Server 2008 standard senza Hyper-V (x86 e x64) | x | x |

Per capire che cos'è un'opzione di installazione, si supponga di aver acquistato un contratto multilicenza che consente di installare una copia di Windows Server 2008 Enterprise Edition. Quando si inserisce il supporto con contratto multilicenza in un sistema e si avvia il processo di installazione, una delle schermate visualizzate, come illustrato nella figura 1-1, presenta una scelta di edizioni e opzioni di installazione.

![Selezione di un'opzione di installazione Server Core da installare](../media/what-is-server-core-2008/FIg1-1.png)

**Figura 1-1** Selezione di un'opzione di installazione Server Core da installare

Nella figura 1-1, il contratto multilicenza (o codice Product Key, per il supporto di vendita al dettaglio) offre due opzioni di installazione che è possibile scegliere tra: la seconda opzione (un'installazione completa di Windows Server 2008 Enterprise) e la quinta opzione (un'installazione Server Core di Windows Server 2008 Enterprise), con l'ultimo selezionato in questo esempio. 

## <a name="full-vs-server-core"></a>Confronto tra Full e Server Core 
Dai primi giorni della piattaforma Microsoft Windows, i server Windows erano essenzialmente server "tutto" che includevano tutti i tipi di funzionalità, alcuni dei quali potrebbero non essere mai utilizzati nell'ambiente di rete. Ad esempio, quando è stato installato Windows Server 2003 in un sistema, i file binari per il servizio Routing e accesso remoto (RRAS) erano installati nel server anche se non era necessario questo servizio (anche se era ancora necessario configurare e abilitare RRAS prima che funzioni). Windows Server 2008 migliora le versioni precedenti installando i file binari necessari per un ruolo del server solo se si sceglie di installare quel particolare ruolo nel server. Tuttavia, l'opzione di installazione completa di Windows Server 2008 installa ancora molti servizi e altri componenti spesso non necessari per uno scenario di utilizzo specifico. 

Questo è il motivo per cui Microsoft ha creato una seconda opzione di installazione, Server Core, per Windows Server 2008: per eliminare tutti i servizi e altre funzionalità che non sono essenziali per il supporto di determinati ruoli server usati comunemente. Un server di Domain Name System (DNS), ad esempio, non richiede l'installazione di Windows Internet Explorer, perché non si desidera esplorare il Web da un server DNS per motivi di sicurezza. Inoltre, un server DNS non necessita di un'interfaccia utente grafica (GUI), perché è possibile gestire virtualmente tutti gli aspetti del DNS dalla riga di comando utilizzando il potente comando Dnscmd. exe o in modalità remota tramite lo snap-in DNS di Microsoft Management Console (MMC).

Per evitare questo problema, Microsoft ha deciso di rimuovere tutti gli elementi da Windows Server 2008 non assolutamente essenziali per l'esecuzione di servizi di rete core come Active Directory Domain Services (AD DS), DNS, Dynamic Host Configuration Protocol (DHCP), file e stampa e un pochi altri ruoli del server. Il risultato è la nuova opzione di installazione dei componenti di base del server, che può essere usata per creare un server che supporta solo un numero limitato di ruoli e funzionalità. 

## <a name="the-server-core-gui"></a>GUI dei componenti di base del server
Al termine dell'installazione di Server Core in un sistema e di accesso per la prima volta, ci si trova in una sorpresa. La figura 1-2 Mostra l'interfaccia utente di Server Core dopo il primo accesso.

![Interfaccia utente di Server Core](../media/what-is-server-core-2008/Fig1-2.png)

**Figura 1-2** Interfaccia utente di Server Core

Nessun desktop Ovvero non è disponibile alcuna shell di Esplora risorse, con il menu Start, la barra delle applicazioni e le altre funzionalità che possono essere usate per vedere. Si tratta di un prompt dei comandi, il che significa che è necessario eseguire la maggior parte delle operazioni di configurazione di un'installazione Server Core digitando i comandi uno alla volta, rallentando o usando script e file batch, che consentono di velocizzare e semplificare la configurati sulle attività mediante l'automazione. Quando si esegue un'installazione automatica di Server Core, è anche possibile eseguire alcune attività di configurazione iniziale usando i file di risposta. 

Per gli amministratori esperti nell'utilizzo di strumenti da riga di comando come netsh. exe, dfscmd. exe e Dnscmd. exe, la configurazione e la gestione di un'installazione dei componenti di base del server può essere facile, anche divertente. Per coloro che non sono esperti, tuttavia, tutti non vengono persi. Per gestire un'installazione dei componenti di base del server, è comunque possibile usare gli strumenti di MMC standard di Windows Server 2008. È sufficiente utilizzarli in un sistema diverso che esegue un'installazione completa di Windows Server 2008 o Windows Vista con Service Pack 1. 

Verranno fornite ulteriori informazioni sulla configurazione e sulla gestione di un'installazione dei componenti di base del server nei capitoli da 3 a 6 di questo libro, mentre i capitoli successivi gestiscono come gestire ruoli server specifici e altri componenti. Per altre informazioni sui vari strumenti della riga di comando di Windows e su come usarli, sono disponibili due risorse valide per consultare:
* La sezione di riferimento del comando della raccolta di documentazione tecnica di Windows Server 2008 () 
* *Windows Command-line Administrator ' s Pocket Consultant* di William R. Stanek (Microsoft Press, 2008) 

La tabella 1-2 elenca le applicazioni GUI principali, insieme ai relativi file eseguibili, che sono disponibili in un'installazione dei componenti di base del server.

**Tabella 1-2** Applicazioni GUI disponibili in un'installazione Server Core

| Applicazione GUI | Eseguibile con percorso |
| -------------   | -------------       | 
| Prompt dei comandi | %WINDIR%\System32\Cmd.exe |
| Strumento di diagnostica supporto tecnico Microsoft | %WINDIR%\System32\MSdt.exe |
| Blocco note di Windows | %WINDIR%\System32\Notepad.exe |
| Editor del registro di sistema | %WINDIR%\System32\Regedt32.exe |
| Informazioni di sistema | %WINDIR%\System32\MSinfo32.exe |
| Gestione attività | %WINDIR%\System32\Taskmgr.exe |
| Windows Installer | %WINDIR%\System32\MSiexec.exe |

Questo è un elenco piuttosto breve. Ecco un elenco di elementi dell'interfaccia utente che non sono inclusi in Server Core:
* Windows Explorer Desktop Shell (Explorer. exe) e tutte le funzionalità di supporto quali i temi 
* Tutte le console MMC 
* Tutte le utilità del pannello di controllo, ad eccezione delle opzioni internazionali e della lingua (Intl. cpl) e di data e ora (timet. cpl) 
* Tutti i motori di rendering Hypertext Markup Language (HTML), inclusi Internet Explorer e la Guida HTML 
* Windows Mail 
* Windows Media Player 
* La maggior parte degli accessori, ad esempio Paint, Calculator e WordPad

Il .NET Framework non è inoltre presente in Server Core, il che significa che non è disponibile alcun supporto per l'esecuzione di codice gestito in un'installazione dei componenti di base del server. Solo il codice nativo, il codice scritto con le API (Application Programming Interface) di Windows, può essere eseguito in Server Core. In breve, le applicazioni GUI che dipendono dalla .NET Framework o dalla shell di Explorer. exe non verranno eseguite in Server Core. 

>[!NOTE]
>Poiché Windows PowerShell richiede la .NET Framework, non è possibile installare Windows PowerShell in Server Core. È tuttavia possibile gestire un'installazione Server Core in remoto usando Windows PowerShell purché si usino solo i comandi WMI di PowerShell.

## <a name="supported-server-roles"></a>Ruoli server supportati 
Un'installazione dei componenti di base del server include solo un numero limitato di ruoli del server rispetto a un'installazione completa di Windows Server 2008. La tabella 1-3 Confronta i ruoli disponibili per le installazioni complete e dei componenti di base del server di Windows Server 2008 Enterprise Edition. 

**Tabella 1-3** Confronto dei ruoli del server per le installazioni complete e dei componenti di base del server di Windows Server 2008 Enterprise Edition

| Ruolo server  | Disponibile nell'installazione completa  | Disponibile in Server Core  |
| ------------- | :-------------: | :------------: |
| Servizi certificati Active Directory  | x |  |
| Servizi di dominio Active Directory  | x  | x |
| Active Directory Federation Services (ADFS)  | x  |  |
| Active Directory Lightweight Directory Services  | x  | x |
| Active Directory Rights Management Services (AD RMS)  | x  |  |
| Server applicazioni  | x  |  |
| Server DHCP  | x  | x |
| Server DNS  | x  | x |
| Server fax  | x  |  |
| Servizi file  | x  | x |
| Hyper-V  | x | x |
| Servizi di accesso e criteri di rete  | x  |  |
| Servizi di stampa  | x  | x |
| Servizi flussi multimediali  | x  | x |
| Servizi terminal  | x  |  |
| Servizi UDDI  | x  |  |
| Server Web (IIS) | x | x |
| Servizi di distribuzione Windows  | x |  |

Mentre i ruoli disponibili per Server Core sono generalmente gli stessi indipendentemente dall'architettura (x86 o x64) e dalla versione del prodotto, esistono alcune eccezioni:
* Il ruolo Hyper-V (virtualizzazione) è disponibile solo se è stato acquistato Windows Server 2008 con supporto del prodotto Hyper-V (Hyper-V è disponibile solo per le versioni x64). Se questo ruolo non è necessario, è invece possibile acquistare Windows Server 2008 senza supporto del prodotto Hyper-V. 
* Il ruolo Servizi file in Standard Edition è limitato a una radice file system distribuito autonoma (DFS) e non supporta la replica tra file (DFS-R). 
* Prima di poter installare il ruolo Servizi flussi multimediali in Server Core, è necessario scaricare e installare il pacchetto autonomo Microsoft Update appropriato (file con estensione msu) per l'architettura del server (x86 o x64) dall'area download Microsoft.
* Il ruolo server Web (IIS) non supporta ASP.NET. Ciò è dovuto al fatto che l'.NET Framework non è supportata in Server Core, che limita le operazioni che è possibile eseguire con un server Web di base server. 

## <a name="supported-optional-features"></a>Funzionalità facoltative supportate
Un'installazione dei componenti di base del server supporta anche solo un subset limitato di funzionalità disponibili in un'installazione completa di Windows Server 2008. La tabella 1-4 Confronta le funzionalità disponibili per le installazioni complete e dei componenti di base del server di Windows Server 2008 Enterprise Edition.

**Tabella 1-4** Confronto tra le funzionalità per le installazioni complete e Server Core di Windows Server 2008 Enterprise Edition

| Funzionalità  | Disponibile nell'installazione completa  | Disponibile in Server Core  |
| ------------- | :-------------: | :------------: |
| Funzionalità di .NET Framework 3,0  | x  |  |
| Crittografia unità BitLocker  | x  | x |
| Estensioni del server BITS  | x  |  |
| Connection Manager Administration Kit  | x |  |
| Esperienza desktop  | x |  |
| Clustering di failover  | x  | x |
| Gestione Criteri di gruppo  | x  |  |
| Client di stampa Internet  | x  |  |
| Internet Storage Name Server  | x  |  |
| Monitoraggio porta LPR  | x  |  |
| Accodamento messaggi  | x  |  |
| I/o a percorsi multipli  | x  | x |
| Bilanciamento carico di rete  | x  | x |
| Protocollo PNRP (Peer Name Resolution Protocol)  | x  |  |
| Servizio audio/video Windows di qualità  | x |  |
| Assistenza remota  | x  |  |
| Compressione differenziale remota | x  |  |
| Strumenti di amministrazione server remoto  | x  |  |
| Gestione archivi rimovibili | x  | x |
| Proxy RPC su HTTP | x  |  |
| Servizi TCP/IP semplificati  | x  |  |
| Server SMTP  | x  |  |
| Servizi SMNP  | x  | x  |
| Gestione archivi SAN  | x  |  |
| Subsystem for UNIX-based Applications | x | x  |
| Client Telnet  | x | x  |
| Server Telnet  | x   |  |
| Client TFTP  | x   |  |
| Database interno di Windows  | x  |  |
| Windows PowerShell  | x  |  |
| Servizio attivazione prodotto Windows  | x   |  |
| Funzionalità di Windows Server Backup  | x  | x  |
| Gestione risorse di sistema Windows  | x  |  |
| Server WINS  | x | x |
| Servizio LAN Wireless | x  |  |

Anche in questo caso, è necessario conoscere alcuni aspetti relativi alle funzionalità disponibili in Server Core:
* Alcune funzionalità possono richiedere hardware speciale per funzionare correttamente (o per tutti) in Server Core. Queste funzionalità includono Crittografia unità BitLocker, clustering di failover, i/o a percorsi multipli, bilanciamento carico di rete e archiviazione rimovibile. 
* Il clustering di failover non è disponibile in Standard Edition.

## <a name="server-core-architecture"></a>Architettura Server Core
Esaminando più a fondo il server core, esaminiamo brevemente l'architettura di un'installazione dei componenti di base del server di Windows Server 2008 confrontandolo con quella di un'installazione completa. In primo luogo, tenere presente che Server Core non è una versione diversa di Windows Server 2008 ma semplicemente un'opzione di installazione che è possibile selezionare quando si installa Windows Server 2008 in un sistema. Ciò implica quanto segue:
* Il kernel in un'installazione dei componenti di base del server è lo stesso che si trova in un'installazione completa della stessa architettura hardware (x86 o x64) ed edizione. 
* Se un file binario è presente in un'installazione dei componenti di base del server, un'installazione completa della stessa architettura hardware (x86 o x64) e dell'edizione ha la stessa versione di quel particolare file binario (con due eccezioni illustrate più avanti). 
* Se una determinata impostazione, ad esempio un'eccezione del firewall specifica o il tipo di avvio di un determinato servizio, dispone di una determinata configurazione predefinita in un'installazione dei componenti di base del server, tale impostazione viene configurata esattamente allo stesso modo in un'installazione completa dello stesso architettura hardware (x86 o x64) ed edizione.

La figura 1-3 illustra una visualizzazione semplificata dell'architettura di un'installazione completa e di un'installazione Server Core di Windows Server 2008. La linea tratteggiata indica l'architettura di Server Core, mentre l'intero diagramma rappresenta l'architettura di un'installazione completa. 

Il diagramma illustra l'architettura modulare di Windows Server 2008, con Server Core costruito su un subset delle principali funzionalità del sistema operativo. Per la stessa architettura ed edizione hardware, tutti i file presenti in un'installazione pulita di Server Core sono presenti anche in un'installazione completa, fatta eccezione per due file speciali (scregedit. wsf e oclist. exe), che sono presenti solo in Server Core. Questi file speciali sono stati inclusi in Server Core per semplificare la configurazione iniziale di un'installazione dei componenti di base del server e l'aggiunta o la rimozione di ruoli e componenti facoltativi. 

![Architetture di Server Core e installazioni complete](../media/what-is-server-core-2008/Fig1-3.png)

**Figura 1-3** Architetture di Server Core e installazioni complete

## <a name="driver-support"></a>Supporto driver
Il diagramma dell'architettura di Server Core illustrato nella figura 1-3 è ovviamente semplificato; una cosa che non mostra è la differenza nel supporto dei driver di dispositivo tra i componenti di base del server e le installazioni complete. Un'installazione completa di Windows Server 2008 contiene migliaia di driver incassati per diversi tipi di dispositivi, che consentono di installare i prodotti in un'ampia gamma di configurazioni hardware diverse. I sistemi operativi client come Windows Vista includono un numero ancora maggiore di driver per il supporto di dispositivi quali fotocamere digitali e scanner che in genere non vengono utilizzati con i server. 

Se un nuovo dispositivo è connesso o installato in un'installazione completa di Windows Server 2008, il sottosistema Plug and Play (PnP) verifica innanzitutto se è presente un driver interno per il dispositivo. Se viene trovato un driver compatibile in box, il sottosistema PnP installa automaticamente il driver e il dispositivo funziona. In un'installazione completa di Windows Server 2008, è possibile che venga visualizzata una notifica popup del fumetto, che indica che il driver è stato installato e che il dispositivo è pronto per l'uso. 

In un'installazione dei componenti di base del server, il processo di installazione del driver è lo stesso (il sottosistema PnP è presente in Server Core) con due qualifiche. In primo luogo, Server Core include solo un numero minimo di driver incorporati e solo per i tipi di dispositivi seguenti:
* Driver video VGA (video graphics array) standard 
* Driver per dispositivi di archiviazione 
* Driver per le schede di rete

Si noti che per ognuna delle tre categorie di dispositivi qui elencate, Server Core include gli stessi driver incorporati che si trovano in un'installazione completa corrispondente (per la stessa architettura hardware). 

Inoltre, quando il sottosistema PnP installa automaticamente un driver per un nuovo dispositivo, lo esegue in modo invisibile all'utente, non viene visualizzata alcuna notifica popup del fumetto. Perché no? Poiché non è presente alcuna interfaccia utente grafica in Server Core, non è disponibile alcuna barra delle applicazioni, quindi non esiste alcuna area di notifica sulla barra delle applicazioni. 

Cosa fare quando si aggiunge il ruolo Servizi di stampa a un'installazione dei componenti di base del server e si vuole installare una stampante? Il driver della stampante viene aggiunto manualmente al server: Server Core non dispone di driver di stampa in box.

## <a name="service-footprint"></a>Footprint servizio
Poiché Server Core è un'installazione minima, presenta un footprint di servizio di sistema inferiore rispetto a un'installazione completa corrispondente della stessa architettura hardware ed edizione. Per impostazione predefinita, ad esempio, vengono installati circa 75 servizi di sistema in un'installazione completa di Windows Server 2008, di cui circa 50 sono configurati per l'avvio automatico. Al contrario, Server Core dispone solo di circa 70 Servizi installati per impostazione predefinita e meno di 40 di questi vengono avviati automaticamente. 

La tabella 1-5 elenca i servizi installati per impostazione predefinita in un'installazione dei componenti di base del server, con la modalità di avvio per e l'account utilizzato da ogni servizio.

**Tabella 1-5** Servizi di sistema installati per impostazione predefinita in Server Core

| Nome del servizio  | `Display name`  | Modalità di avvio  | Account  |
| ------------- | ------------- | ------------ | ------------ |
| AeLookupSvc  | Esperienza dell'applicazione  | Automatico | LocalSystem |
| AppMgmt  | Gestione applicazioni  | Manuale | LocalSystem |
| BFE | Base Filtering Engine  | Automatico | LocalService |
| BITS | Servizio trasferimento intelligente in background  | Automatico | LocalSystem |
| Browser | Browser di computer  | Manuale | LocalSystem |
| CertPropSvc | Propagazione certificati  | Manuale | LocalSystem |
| COMSysApp  | Applicazione di sistema COM+  | Manuale | LocalSystem |
| CryptSvc  | Servizi di crittografia  | Automatico | Servizio di rete |
| DcomLaunch  | Utilità di avvio processi server DCOM  | Automatico | LocalSystem |
| Dhcp  | Client DHCP  | Automatico | LocalService |
| Dnscache | Client DNS  | Automatico | Servizio di rete |
| DPS  | Servizio Criteri di diagnostica  | Automatico | LocalService |
| EventLog | Registro eventi di Windows  | Automatico | LocalService |
| EventSystem  | COM+ Event System  | Automatico | LocalService |
| FCRegSvc  | Servizio registrazione piattaforma Microsoft Fibre Channel  | Manuale | LocalService |
| gpsvc  | Client di Criteri di gruppo  | Automatico | LocalSystem |
| hidserv | Accesso dispositivo interfaccia umana  | Manuale | LocalSystem |
| hkmsvc  | Gestione di chiavi e certificati di integrità  | Manuale | LocalSystem |
| IKEEXT  | Moduli di impostazione chiavi IPSec IKE e Auth-IP  | Automatico | LocalSystem |
| iphlpsvc  | Helper IP  | Automatico | LocalSystem |
| KeyIso | Isolamento chiavi CNG  | Manuale | LocalSystem |
| KtmRm  | KtmRm per Distributed Transaction Coordinator  | Automatico | Servizio di rete |
| LanmanServer  | Server  | Automatico | LocalSystem |
| LanmanWorkstation  | Workstation  | Automatico | LocalService |
| lltdsvc  | Mapper individuazione topologia livelli di collegamento  | Manuale | LocalService |
| lmhosts  | Helper NetBIOS di TCP/IP  | Automatico | LocalService |
| MpsSvc  | Windows Firewall  | Automatico | LocalService |
| MSDTC  | Distributed Transaction Coordinator  | Automatico | Servizio di rete |
| MSiSCSI  | Servizio iniziatore iSCSI Microsoft  | Manuale | LocalSystem |
| msiserver  | Windows Installer  | Manuale | LocalSystem |
| napagent  | Agente protezione accesso alla rete  | Manuale | Servizio di rete |
| Accesso rete  | Accesso rete  | Manuale | LocalSystem |
| netprofm  | Servizio Elenco reti  | Automatico | LocalService |
| NlaSvc  | Riconoscimento presenza in rete  | Automatico | Servizio di rete |
| nsi  | Servizio Interfaccia archivio di rete  | Automatico | LocalService |
| pla  | Avvisi e registri di prestazioni  | Manuale | LocalService |
| PlugPlay  | Plug and Play  | Automatico | LocalSystem |
| PolicyAgent  | Agente criteri IPsec  | Automatico | Servizio di rete |
| ProfSvc  | Servizio profili utente  | Automatico | LocalSystem |
| ProtectedStorage  | Archiviazione protetta  | Manuale | LocalSystem |
| RemoteRegistry  | Registro di sistema remoto  | Automatico | LocalService |
| RpcSs  | RPC (Remote Procedure Call)  | Automatico | Servizio di rete |
| RSoPProv | Provider Gruppo di criteri risultante  | Manuale | LocalSystem |
| sacsvr  | Helper console di amministrazione speciale  | Manuale | LocalSystem |
| SamSs  | Sistema di gestione degli account di sicurezza (SAM)  | Automatico | LocalSystem |
| SCardSvr | Smart card  | Manuale | LocalService |
| Pianificazione | Utilità di pianificazione  | Automatico | LocalSystem |
| SCPolicySvc | Criterio rimozione smart card  | Manuale | LocalSystem |
| seclogon | Accesso secondario  | Automatico | LocalSystem |
| SENS | Servizio di notifica eventi di sistema  | Automatico | LocalSystem |
| SessionEnv | Configurazione Servizi terminal  | Manuale | LocalSystem |
| slsvc  | Gestione licenze software | Automatico | Servizio di rete |
| SNMPTRAP  | Trap SNMP  | Manuale | LocalService |
| swprv  | Provider di copie shadow software Microsoft | Manuale | LocalSystem |
| TBS | Servizi di base TPM  | Manuale | LocalService |
| TermService  | Servizi terminal | Automatico | Servizio di rete |
| TrustedInstaller | Programma di installazione dei moduli di Windows  | Automatico | LocalSystem |
| UmRdpService | Redirector porta UserMode di Servizi terminal  | Manuale | LocalSystem |
| vds | Disco virtuale  | Manuale | LocalSystem |
| VSS | Copia shadow del volume  | Manuale | LocalSystem |
| W32Time | Ora di Windows  | Automatico | LocalService |
| WcsPlugInService  | Sistema colori Windows  | Manuale | LocalService |
| WdiServiceHost  | Host servizio di diagnostica  | Manuale | LocalService |
| WdiSystemHost  | Host sistema di diagnostica  | Manuale | LocalSystem |
| Wecsvc | Raccolta eventi Windows  | Manuale | Servizio di rete |
| WinHttpAuto-ProxySvc  | Servizio rilevamento automatico proxy WinHTTP  | Automatico | LocalService |
| Winmgmt | Strumentazione gestione Windows (WMI) | Automatico | LocalSystem |
| WinRM  | Gestione remota Windows (WS-Management) | Automatico | Servizio di rete |
| wmiApSrv  | Scheda delle prestazioni WMI  | Manuale | LocalSystem |
| wuauserv | Windows Update | Automatico | LocalSystem |