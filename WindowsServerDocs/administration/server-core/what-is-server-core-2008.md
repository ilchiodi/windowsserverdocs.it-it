---
title: What ' s Server Core 2008?
description: Scopri l'opzione di installazione Server Core in Windows Server 2008
ms.prod: windows-server-threshold
ms.author: helohr
ms.date: 11/01/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: 8d1aaf8b61142155ea7b2a5391367cc677596ebe
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435606"
---
# <a name="what-is-server-core-2008"></a>What ' s Server Core 2008?
>Si applica a: Windows Server 2008

>[!NOTE]
>Queste informazioni si applicano a Windows Server 2008. Per informazioni su Server Core in Windows Server, vedere [qual è l'installazione Server Core in Windows Server](https://docs.microsoft.com/windows-server/administration/server-core/what-is-server-core). 

L'opzione Server Core è una nuova opzione di installazione minima che è disponibile quando si distribuisce l'edizione Standard, Enterprise o Datacenter di Windows Server 2008. Server Core offre un'installazione minima di Windows Server 2008 che supporta l'installazione solo determinati ruoli del server, come descritto più avanti in questo capitolo. Confrontare questa operazione con l'opzione di installazione completa di Windows Server 2008 che supporta l'installazione di tutti i ruoli server disponibili e altri Microsoft o le applicazioni server di terze parti, ad esempio Microsoft Exchange Server o SAP. 

Prima di continuare, deve essere spiegato la frase "opzione di installazione". In genere, quando si acquista una copia di Windows Server 2008, si acquista una licenza per l'uso di determinate edizioni o unità SKU (SKU). Tabella 1 1 Elenca le diverse versioni di Windows Server 2008 che sono disponibili. La tabella indica anche le opzioni di installazione (Full, Server Core o entrambi) sono disponibili per ogni edizione.

**Tabella 1-1** edizioni di Windows Server 2008 e il relativo supporto per le opzioni di installazione

| Edizione       | Completi          | Server Core  |
| ------------- | :-------------: | :------------: |
| Windows Server 2008 Standard (x86 e x64)       | x | x        |
| Windows Server 2008 Enterprise (x86 e x64)       | x | x        |
| Windows Server 2008 Datacenter (x86 e x64)        | x | x       |
| Windows Web Server 2008 (x86 e x64)       | x | x  |
| Windows Server 2008 For Itanium di       | x |     |
| Windows HPC Server 2008 (solo x64)       | x |   |
| Windows Server 2008 Standard senza Hyper-V (x86 e x64) | x | x |
| Windows Server 2008 Enterprise senza Hyper-V (x86 e x64)  | x | x |
| Windows Server 2008 Standard senza Hyper-V (x86 e x64) | x | x |

Per comprendere che cos'è un'opzione di installazione"", si supponga che è stata acquistata una licenza di volume che consente di installare una copia di Windows Server 2008 Enterprise Edition. Quando si inserisce il supporto con contratto multilicenza in un sistema e avviare il processo di installazione, uno degli schermi che si vedrà, come illustrato nella figura 1-1, offre una varietà di edizioni e opzioni di installazione.

![Selezionare un'opzione di installazione Server Core per l'installazione](../media/what-is-server-core-2008/FIg1-1.png)

**Figura 1-1** selezionando un'opzione di installazione Server Core per l'installazione

Nella figura 1-1, i contratti multilicenza (o codice product key, per la media delle vendite al dettaglio) offre due opzioni di installazione è possibile scegliere tra: la seconda opzione (una completa installazione di Windows Server 2008 Enterprise) e l'opzione quinto (un Server Core installazione di Windows Server 2008 Enterprise Edition), con quest'ultimo selezionati in questo esempio. 

## <a name="full-vs-server-core"></a>Transazioni completamente durevoli e. Server Core 
Sin dai tempi della piattaforma Microsoft Windows, Windows Server sono stati essenzialmente "tutto" i server inclusi tutti i tipi di caratteristiche, alcune delle quali potrebbe essere effettivamente mai usate per l'ambiente di rete. Ad esempio, durante l'installazione di Windows Server 2003 in un sistema, i file binari per il Routing e accesso remoto (RRAS) sono stati installati nel server anche se non si ha alcuna necessità di questo servizio (anche se era ancora necessario configurare e abilitare RRAS prima di garantirne il corretto funzionamento). Windows Server 2008 consente di migliorare le versioni precedenti installando i file binari necessari per un ruolo del server solo se si sceglie di installare tale particolare ruolo nel server. Tuttavia, l'opzione di installazione completa di Windows Server 2008 installa ancora molti servizi e altri componenti che spesso non sono necessari per un determinato scenario di utilizzo. 

Questo è il motivo Microsoft ha creato una seconda opzione di installazione, Server Core, ovvero per Windows Server 2008: per eliminare tutti i servizi e altre funzionalità che non sono essenziali per il supporto di alcuni comunemente utilizzati i ruoli del server. Ad esempio, un server di sistema DNS (Domain Name) non necessariamente installato perché l'utente non desideri esplorano il Web da un server DNS per motivi di sicurezza di Windows Internet Explorer. E un server DNS non è neanche necessario un'interfaccia utente grafica (GUI), poiché è possibile gestire praticamente tutti gli aspetti del DNS dalla riga di comando usando il comando Dnscmd.exe potente, o in modalità remota tramite Microsoft Management Console (MMC) di DNS lo snap-in.

Per evitare questo problema, Microsoft ha deciso di rimuovere tutti i dati da Windows Server 2008 che è stato non è assolutamente necessario per l'esecuzione di servizi di rete di base, ad esempio Active Directory Domain Services (AD DS), DNS, Dynamic Host Configuration Protocol (DHCP), File e stampa, e un oggetto alcuni altri ruoli del server. Il risultato è la nuova opzione di installazione Server Core, che può essere usato per creare un server che supporta solo un numero limitato di ruoli e funzionalità. 

## <a name="the-server-core-gui"></a>L'interfaccia utente grafica di Server Core
Al termine dell'installazione Server Core in un sistema e di accesso per la prima volta, viene visualizzato per un po' di stupore. La figura 1-2 mostra l'interfaccia utente di base del Server dopo il primo accesso.

![Interfaccia utente di Server Core](../media/what-is-server-core-2008/Fig1-2.png)

**La figura 1-2** dell'interfaccia utente di Server Core

Non c'è nessun desktop! Non vi è alcun shell Esplora risorse di Windows, con il menu Start, sulla barra delle applicazioni e le altre funzionalità che sono abituati a vedere. È un prompt dei comandi, ciò significa che è necessario per la maggior parte delle operazioni di configurazione di un'installazione Server Core digitando i comandi uno alla volta, che è lenta, o tramite script e file batch, che consentono di velocizzare e semplificare le configurati attività grazie all'automazione dei loro. È anche possibile eseguire alcune attività di configurazione iniziale utilizzando file di risposta quando si esegue un'installazione automatica di base del Server. 

Per gli amministratori esperti di usando strumenti da riga di comando come Netsh.exe Dfscmd.exe e Dnscmd.exe, configurazione e gestione di un'installazione Server Core può essere semplice e persino divertente. Per coloro che non sono esperti, tuttavia, non tutto è perduto. È comunque possibile usare gli strumenti di MMC di Windows Server 2008 standard per la gestione di un'installazione Server Core. È sufficiente per usarli in un altro sistema che esegue un'installazione completa di Windows Server 2008 o Windows Vista con Service Pack 1. 

Si fornite altre informazioni sulla configurazione e gestione di un'installazione Server Core in capitoli da 3 a 6 di questo libro, mentre negli ultimi capitoli occuparsi di come gestire ruoli del server specifici e altri componenti. Per altre informazioni su come usarli e i vari strumenti da riga di comando di Windows, esistono due ottime risorse per consultare:
* La sezione di riferimento ai comandi della libreria TechNet di Windows Server 2008) 
* *Pocket Consultant dell'amministratore della riga di comando Windows* da William (Microsoft Press, 2008) 

Tabella 1 e 2 elenca le applicazioni GUI principale, insieme ai relativi eseguibili, sono disponibili in un'installazione Server Core.

**Tabella 1 e 2** applicazioni GUI disponibili in un'installazione Server Core

| Applicazione GUI | File eseguibile con il percorso |
| -------------   | -------------       | 
| Prompt dei comandi | %WINDIR%\System32\Cmd.exe |
| Strumento di diagnostica supporto tecnico Microsoft | %WINDIR%\System32\MSdt.exe |
| Blocco note di Windows | %WINDIR%\System32\Notepad.exe |
| Editor del Registro di sistema | %WINDIR%\System32\Regedt32.exe |
| Informazioni di sistema | %WINDIR%\System32\MSinfo32.exe |
| Gestione attività | %WINDIR%\System32\Taskmgr.exe |
| Windows Installer | %WINDIR%\System32\MSiexec.exe |

Che è un elenco ridotto vera e propria. Ecco ora un elenco di elementi dell'interfaccia utente che non sono inclusi in Server Core:
* La shell desktop di Windows Explorer (Explorer.exe) e qualsiasi funzionalità di supporto, ad esempio i temi 
* Tutte le console MMC 
* Tutte le utilità del Pannello di controllo, fatta eccezione per opzioni internazionali e della lingua (intl. cpl) e la data e ora (timedate. cpl) 
* Tutti i motori di rendering Hypertext Markup Language (HTML), tra cui Internet Explorer e la Guida HTML 
* Posta elettronica di Windows 
* Windows Media Player 
* La maggior parte degli accessori come Paint, calcolatrice e Wordpad

.NET Framework non è inoltre presente in Server Core, che significa che non c'è Nessun supporto per codice gestito in esecuzione in un'installazione Server Core. Solo codice nativo, ovvero il codice scritto utilizzando application programming interface (API) Windows, ovvero possono eseguire in Server Core. In breve, tutte le applicazioni GUI che dipendono da entrambi .NET Framework o nel Explorer.exe shell non viene eseguito in Server Core. 

>[!NOTE]
>Poiché Windows PowerShell richiede .NET Framework, è possibile installare Windows PowerShell in Server Core. Tuttavia, è possibile, gestire un'installazione Server Core in remoto mediante Windows PowerShell, purché si utilizzano solo i comandi di PowerShell WMI.

## <a name="supported-server-roles"></a>Ruoli server supportati 
Un'installazione Server Core include solo un numero limitato di ruoli del server rispetto a un'installazione completa di Windows Server 2008. Tabella da 1 a 3 confronta i ruoli disponibili per le installazioni di sia completo e Server Core di Windows Server 2008 Enterprise Edition. 

**Tabella da 1 a 3** confronto dei ruoli del server per le installazioni completa e dei Server Core di Windows Server 2008 Enterprise Edition

| Ruolo server  | Disponibile in installazione completa  | Disponibile in Server Core  |
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

Anche se i ruoli disponibili per Server Core sono in genere lo stesso indipendentemente dall'edizione del prodotto e architettura (x86 o x64), esistono alcune eccezioni:
* Il ruolo Hyper-V (virtualizzazione) è disponibile solo se è stato acquistato Windows Server°2008 con supporto del prodotto Hyper-V (Hyper-V è disponibile solo per x64 versioni). Se non è necessario questo ruolo, è possibile acquistare Windows Server 2008 senza supporto del prodotto Hyper-V. 
* Il ruolo Servizi File in Standard Edition è limitato a una radice del File System distribuito (DFS) autonomo e non supporta la replica Cross-File (DFS-R). 
* Prima di installare il ruolo Servizi flussi multimediali in Server Core, è necessario scaricare e installare l'appropriato pacchetto autonomo Microsoft Update (file. msu) per l'architettura del server (x86 o x64) dal Microsoft Download Center.
* Il ruolo Server Web (IIS) non supportati da ASP.NET. Questo avviene perché .NET Framework non è supportato in Server Core, limitando operazioni eseguibili con un server Server Core Web. 

## <a name="supported-optional-features"></a>Funzionalità opzionali supportate
Un'installazione Server Core supporta anche solo un sottoinsieme limitato delle funzionalità disponibili in un'installazione completa di Windows Server 2008. Tabella 1-4 confronta le funzionalità disponibili per le installazioni di sia completo e Server Core di Windows Server 2008 Enterprise Edition.

**Tabella 1-4** confronto delle funzionalità per le installazioni completa e dei Server Core di Windows Server 2008 Enterprise Edition

| Funzionalità  | Disponibile in installazione completa  | Disponibile in Server Core  |
| ------------- | :-------------: | :------------: |
| Funzionalità di .NET framework 3.0  | x  |  |
| Crittografia unità BitLocker  | x  | x |
| Estensioni Server BITS  | x  |  |
| Connection Manager Administration Kit  | x |  |
| Esperienza desktop  | x |  |
| Clustering di failover  | x  | x |
| Gestione Criteri di gruppo  | x  |  |
| Client di stampa Internet  | x  |  |
| Internet Storage Name Server  | x  |  |
| Monitoraggio porta LPR  | x  |  |
| Accodamento messaggi  | x  |  |
| / O a percorsi multipli  | x  | x |
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
| SMNP Services  | x  | x  |
| Gestione archivi SAN  | x  |  |
| Subsystem for UNIX-based Applications | x | x  |
| Client Telnet  | x | x  |
| Server Telnet  | x   |  |
| Client TFTP  | x   |  |
| Database interno di Windows  | x  |  |
| Windows PowerShell  | x  |  |
| Windows Product Activation Service  | x   |  |
| Funzionalità di Windows Server Backup  | x  | x  |
| Gestione risorse di sistema Windows  | x  |  |
| Server WINS  | x | x |
| Servizio LAN Wireless | x  |  |

Anche in questo caso, ci sono alcuni aspetti che occorre sapere sulle relative funzionalità disponibili in Server Core:
* Alcune funzionalità potrebbero richiedere hardware speciale per funzionare correttamente (o tutti) in Server Core. Queste funzionalità includono crittografia unità BitLocker, Clustering di Failover, Multipath i/o, Bilanciamento carico di rete e archivi rimovibili. 
* Clustering di failover non è disponibile nell'edizione Standard.

## <a name="server-core-architecture"></a>Architettura di Server Core
Alcuni approfondimenti nei Server Core, prendiamo l'architettura di un'installazione Server Core di Windows Server 2008 e confrontarlo con quello di un'installazione completa. In primo luogo, ricordare che Server Core non è una versione diversa di Windows Server 2008, ma semplicemente un'opzione di installazione che è possibile selezionare durante l'installazione di Windows Server 2008 in un sistema. Questo implica quanto segue:
* Il kernel in un'installazione Server Core è identico a quello trovato in un'installazione completa della stessa architettura hardware (x86 o x64) e dell'edizione. 
* Se un file binario è presente in un'installazione Server Core, un'installazione completa della stessa architettura hardware (x86 o x64) ed edizione ha la stessa versione di quel determinato file binario (con due eccezioni illustrate più avanti). 
* Se una particolare impostazione (ad esempio, un'eccezione del firewall specifiche o il tipo di avvio di un determinato servizio) ha una determinata configurazione predefinita in un'installazione Server Core, questa impostazione è configurata esattamente allo stesso modo in un'installazione completa dello stesso Architettura hardware (x86 o x64) e l'edizione.

La figura 1-3 illustra una vista semplificata dell'architettura di un'installazione completa sia un'installazione Server Core di Windows Server 2008. La linea punteggiata indica l'architettura di base del Server, mentre l'intero diagramma rappresenta l'architettura di un'installazione completa. 

Il diagramma illustra l'architettura modulare di Windows Server 2008, che viene costruito su un subset delle funzionalità principali del sistema operativo Server Core. Per la stessa architettura hardware e l'edizione, ogni file presente in un'installazione pulita di Server Core è inoltre presente in un'installazione completa, ad eccezione dei due file speciale (Scregedit.wsf e Oclist.exe), che sono presenti solo in Server Core. Questi file speciali sono stati inclusi in Server Core per semplificare la configurazione iniziale di un'installazione Server Core e l'aggiunta o la rimozione di ruoli e componenti facoltativi. 

![Le architetture di installazioni Server Core e Full](../media/what-is-server-core-2008/Fig1-3.png)

**La figura 1-3** le architetture di installazioni Server Core e Full

## <a name="driver-support"></a>Supporto driver
Diagramma dell'architettura di base del Server illustrato in figura 1-3 è ovviamente semplificato. non è presente una cosa è la differenza nel supporto di driver di dispositivo tra installazioni complete e di base. Un'installazione completa di Windows Server 2008 contiene migliaia di driver per i diversi tipi di dispositivi, che consentono di installare i prodotti a un'ampia gamma di configurazioni hardware diverse. (Sistemi operativi client come Windows Vista include anche altri driver per supportare dispositivi, quali fotocamere digitali e gli scanner che normalmente non vengono usati con i server). 

Se un nuovo dispositivo è connesso a (o installato) un'installazione completa di Windows Server 2008, il sottosistema di Plug and Play (PnP) controlla innanzitutto se è presentano un driver in arrivo per il dispositivo. Se viene trovato un driver compatibile in arrivo, il sottosistema Plug and Play automaticamente consente di installare il driver e il dispositivo e opera. In un'installazione completa di Windows Server 2008, potrebbe essere visualizzato un fumetto di notifica popup, per indicare che il driver è stato installato e il dispositivo è pronto per l'uso. 

In un'installazione Server Core, il processo di installazione di driver è lo stesso (il sottosistema PnP è presente in Server Core) con due qualificazioni. In primo luogo, Server Core include un numero minimo di driver e solo per i tipi di dispositivi seguenti:
* Un Video grafica matrice (driver VGA standard) video 
* Driver dei dispositivi di archiviazione 
* Driver per schede di rete

Si noti che per ognuna delle categorie di tre dispositivi illustrate di seguito, Server Core include gli stessi driver in arrivo che si trovano in un'installazione completa corrispondente (per la stessa architettura hardware). 

Inoltre, quando il sottosistema PnP viene automaticamente installato un driver per un nuovo dispositivo, esegue l'operazione invisibile all'utente, viene visualizzata alcuna notifica popup fumetto. Perché no? Poiché non vi è alcuna interfaccia utente grafica in Server Core non è Nessuna barra delle applicazioni, pertanto non presenta alcuna area di notifica della barra delle applicazioni. 

Quindi, cosa fare quando si aggiunge il ruolo Servizi di stampa da un'installazione Server Core e si vuole installare una stampante? Si aggiunta manualmente il driver della stampante nel server, Server Core non dispone di alcun driver della stampante in arrivo.

## <a name="service-footprint"></a>Footprint di servizio
Poiché Server Core è un'installazione minima, dispone di un footprint di servizio di sistema più piccolo rispetto a un'installazione completa corrispondente della stessa architettura hardware e l'edizione. Ad esempio, circa 75 servizi di sistema vengono installati per impostazione predefinita in un'installazione completa di Windows Server 2008, che circa 50 sono configurati per l'avvio automatico. Al contrario, Server Core è installato per impostazione predefinita e meno di 40 questi start automaticamente solo circa 70 services. 

Tabella da 1 a 5 sono elencati i servizi che vengono installati per impostazione predefinita in un'installazione Server Core, con la modalità di avvio per e account utilizzati da ciascun servizio.

**Tabella da 1 a 5** servizi di sistema installati per impostazione predefinita in Server Core

| Nome del servizio  | `Display name`  | Modalità di avvio  | Account  |
| ------------- | ------------- | ------------ | ------------ |
| AeLookupSvc  | Esperienza dell'applicazione  | Automatico | LocalSystem |
| AppMgmt  | Gestione applicazioni  | Manual | LocalSystem |
| BFE | Motore di filtraggio di base  | Automatico | LocalService |
| BITS | Servizio trasferimento intelligente in background  | Automatico | LocalSystem |
| Browser | Browser di computer  | Manual | LocalSystem |
| CertPropSvc | Propagazione certificati  | Manual | LocalSystem |
| COMSysApp  | Applicazione di sistema COM+  | Manual | LocalSystem |
| CryptSvc  | Servizi di crittografia  | Automatico | Network-Service |
| DcomLaunch  | Utilità di avvio processo Server DCOM  | Automatico | LocalSystem |
| DHCP  | Client DHCP  | Automatico | LocalService |
| DnsCache | Client DNS  | Automatico | Network-Service |
| DPS  | Servizio criteri di diagnostica  | Automatico | LocalService |
| Registro eventi | Registro eventi di Windows  | Automatico | LocalService |
| EventSystem  | Sistema di eventi COM+  | Automatico | LocalService |
| FCRegSvc  | Servizio di registrazione della piattaforma Microsoft Fibre Channel  | Manual | LocalService |
| gpsvc  | Client di Criteri di gruppo  | Automatico | LocalSystem |
| hidserv | Accesso Human Interface Device  | Manual | LocalSystem |
| hkmsvc  | Chiave di integrità e la gestione dei certificati  | Manual | LocalSystem |
| IKEEXT  | Moduli di impostazione chiavi IPSec IKE e Auth-IP  | Automatico | LocalSystem |
| iphlpsvc  | Helper IP  | Automatico | LocalSystem |
| KeyIso | Isolamento chiavi CNG  | Manual | LocalSystem |
| KtmRm  | KtmRm per Distributed Transaction Coordinator  | Automatico | Network-Service |
| LanmanServer  | Server  | Automatico | LocalSystem |
| LanmanWorkstation  | Workstatione  | Automatico | LocalService |
| lltdsvc  | Mapper individuazione topologia livelli di collegamento  | Manual | LocalService |
| lmhosts  | NetBIOS Helper di TCP/IP  | Automatico | LocalService |
| MpsSvc  | Windows Firewall  | Automatico | LocalService |
| MSDTC  | Distributed Transaction Coordinator  | Automatico | Network-Service |
| MSiSCSI  | Servizio iniziatore iSCSI Microsoft  | Manual | LocalSystem |
| msiserver  | Windows Installer  | Manual | LocalSystem |
| napagent  | Agente di protezione accesso alla rete  | Manual | Network-Service |
| Accesso rete  | Accesso rete  | Manual | LocalSystem |
| netprofm  | Servizio elenco reti  | Automatico | LocalService |
| NlaSvc  | Riconoscimento presenza in rete  | Automatico | Network-Service |
| nsi  | Servizio di interfaccia di rete Store  | Automatico | LocalService |
| pla  | Gli avvisi e registri di prestazioni  | Manual | LocalService |
| PlugPlay  | Servizio Plug and Play  | Automatico | LocalSystem |
| PolicyAgent  | Agente criteri IPsec  | Automatico | Network-Service |
| ProfSvc  | Servizio profili utente  | Automatico | LocalSystem |
| ProtectedStorage  | Archiviazione protetta  | Manual | LocalSystem |
| RemoteRegistry  | Registro di sistema remoto  | Automatico | LocalService |
| RpcSs  | RPC (Remote Procedure Call)  | Automatico | Network-Service |
| RSoPProv | Set di Provider di criteri risultante  | Manual | LocalSystem |
| sacsvr  | Helper di Console di amministrazione speciale  | Manual | LocalSystem |
| SamSs  | Gestione degli account di sicurezza  | Automatico | LocalSystem |
| SCardSvr | Smart card  | Manual | LocalService |
| Pianificazione | Utilità di pianificazione  | Automatico | LocalSystem |
| SCPolicySvc | Criterio di rimozione della Smart Card  | Manual | LocalSystem |
| seclogon | Accesso secondario  | Automatico | LocalSystem |
| SENS | Servizio di notifica di eventi di sistema  | Automatico | LocalSystem |
| SessionEnv | Configurazione Servizi terminal  | Manual | LocalSystem |
| slsvc  | Gestione licenze software | Automatico | Network-Service |
| SNMPTRAP  | Trap SNMP  | Manual | LocalService |
| swprv  | Provider di copie Shadow Software Microsoft | Manual | LocalSystem |
| TBS | Servizi di Base di TPM  | Manual | LocalService |
| TermService  | Servizi terminal | Automatico | Network-Service |
| TrustedInstaller | Programma di installazione di Windows i moduli  | Automatico | LocalSystem |
| UmRdpService | Redirector porta UserMode di Servizi terminal  | Manual | LocalSystem |
| VDS | Disco virtuale  | Manual | LocalSystem |
| VSS | Copia Shadow del volume  | Manual | LocalSystem |
| W32Time | Ora di Windows  | Automatico | LocalService |
| WcsPlugInService  | Sistema colori di Windows  | Manual | LocalService |
| WdiServiceHost  | Host del servizio diagnostica  | Manual | LocalService |
| WdiSystemHost  | Host di sistema di diagnostica  | Manual | LocalSystem |
| Wecsvc | Raccolta eventi Windows  | Manual | Network-Service |
| WinHttpAuto-ProxySvc  | Servizio di rilevamento automatico Proxy Web di WinHTTP  | Automatico | LocalService |
| Winmgmt | Strumentazione gestione Windows (WMI) | Automatico | LocalSystem |
| WinRM  | Gestione remota Windows (WS-Management) | Automatico | Network-Service |
| wmiApSrv  | Scheda WMI Performance  | Manual | LocalSystem |
| wuauserv | Windows Update | Automatico | LocalSystem |