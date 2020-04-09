---
title: Sicurezza e controllo
description: Panoramica sulla sicurezza in Windows Server 2016
ms.prod: windows-server
ms.technology: techgroup-security
ms.topic: article
ms.date: 07/27/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: medium
ms.openlocfilehash: ec389fab792ecc714684ff5f8976845b5cea05ba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859334"
---
# <a name="security-and-assurance-in-windows-server"></a>Sicurezza e controllo in Windows Server 

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

>[!TIP]
> Per informazioni sulle versioni precedenti di Windows Server, Vedi le nostre altre [librerie di Windows Server](/previous-versions/windows/) in docs.microsoft.com. È anche possibile cercare informazioni specifiche [in questo sito](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions).

<img src="../media/landing-icons/security.png" style='float:left; padding:.5em;' alt="Icon representing a lock"> La sicurezza e il controllo si basano su nuovi livelli di protezione integrati nel sistema operativo come ulteriore misura di protezione contro le violazioni della sicurezza. Consentono di bloccare gli attacchi dannosi e di migliorare la protezione di macchine virtuali, applicazioni e dati.


### <a name="windows-server-security-blog-post"></a>[Post di Blog sulla sicurezza di Windows Server](https://blogs.technet.microsoft.com/windowsserver/2016/04/25/ten-reasons-youll-love-windows-server-2016-8-security/)
Questo post di blog del team per la sicurezza di Windows Server evidenzia molti dei miglioramenti di Windows Server che aumentano la sicurezza per gli ambienti di hosting e cloud ibridi.

### <a name="datacenter-and-private-cloud-security-blog"></a>[Blog sulla sicurezza di datacenter e cloud privato](https://blogs.technet.microsoft.com/datacentersecurity/)
Questo è il blog centrale per i contenuti tecnici del team per la sicurezza dei centri dati e dei cloud privati di Microsoft.                                    

### <a name="addressing-emerging-threats-and-landscape-shifts"></a>[Affrontare le minacce emergenti e i turni orizzontali](https://www.youtube.com/watch?v=B5JMYxYWx1k&feature=youtu.be)
In questo video di 6 minuti, Anders Vinberg fornisce una panoramica della strategia di sicurezza e controllo di Microsoft e illustra le tendenze del settore e i turni orizzontali in relazione alla sicurezza. Analizza quindi le iniziative principali di Microsoft relative alla protezione dei carichi di lavoro dall'infrastruttura sottostante e alla protezione da attacchi diretti da account con privilegi. Infine, nel caso di violazione, spiega come le nuove funzionalità di rilevamento e analisi forense consentano di identificare la minaccia.

### <a name="protecting-your-datacenter-and-cloud-from-emerging-threats-blog-post"></a>[Post di Blog sulla protezione del Data Center e del cloud da minacce emergenti](https://blogs.technet.com/b/windowsserver/archive/2015/11/18/protecting-your-datacenter-and-cloud-november-update.aspx)
Questo post di blog illustra come è possibile usare le tecnologie di Microsoft per proteggere i centri dati e i cloud da minacce emergenti.                   

### <a name="security-and-assurance-overview-session-at-ignite"></a>[Sessione Panoramica di Security and Assurance in Ignite](https://channel9.msdn.com/events/ignite/2015/brk2482)
In questa sessione di Ignite vengono trattate le minacce persistenti, le violazioni interne, la criminalità informatica organizzata e la protezione della piattaforma Microsoft Cloud (in locale e servizi connessi con Azure). Include gli scenari per la protezione dei carichi di lavoro, i titolari di aziende di grandi dimensioni e i provider di servizi.                                                                   

## <a name="secure-virtualization-with-shielded-vms"></a>Virtualizzazione sicura con macchine virtuali schermate

### <a name="shielded-vm-in-channel-9"></a>[Macchina virtuale schermata su Channel 9](https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
Una procedura dettagliata della tecnologia e dei vantaggi offerti dalle VM schermate.                           

### <a name="shielded-vm-demo"></a>[Demo delle macchine virtuali schermate](https://www.youtube.com/watch?v=xip5Qtk-7d8)
Questo video di 4 minuti descrive il valore delle VM schermate e le differenze tra una VM schermata e una VM non schermata.                                   

### <a name="shielded-virtual-machines-in-windows-server-video-walkthrough"></a>[Video della procedura dettagliata per le macchine virtuali schermate di Windows Server](http://microsoft-cloud.cloudguides.com/Guides/Shielded Virtual Machines in Windows Server.htm)
Questo video dettagliato illustra come il servizio Sorveglianza host autorizza le macchine virtuali schermate in modo che i dati riservati siano protetti dall'accesso non autorizzato da parte degli amministratori di host Hyper-V.

### <a name="harden-the-fabric-protecting-tenant-secrets-in-hyper-v-ignite-video"></a>[Rafforzare l'infrastruttura: protezione dei segreti dei tenant in Hyper-V (Ignite video)](https://channel9.msdn.com/events/ignite/2015/brk3457)

Questa presentazione di Ignite illustra i miglioramenti apportati in Hyper-V, Virtual Machine Manager e un nuovo ruolo del server di Sorveglianza host per abilitare le macchine virtuali schermate.                

### <a name="guarded-fabric-deployment-guide"></a>[Guida alla distribuzione di un'infrastruttura sorvegliata](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview)
Questa guida fornisce informazioni sull'installazione e la convalida per Windows Server e System Center Virtual Machine Manager per gli host dell'infrastruttura sorvegliata e le macchine virtuali schermate.

### <a name="shielded-vm-and-guarded-fabric-in-branch-offices"></a>[Macchina virtuale schermata e infrastruttura sorvegliata nelle succursali](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office)
Questa guida fornisce le procedure consigliate per l'esecuzione nelle macchine virtuali schermate nelle succursali e altri scenari remoti in cui gli host Hyper-V possono avere periodi di tempo con connessione limitata a HGS.

### <a name="shielded-vm-and-guarded-fabric-troubleshooting-guide"></a>[Guida alla risoluzione dei problemi delle macchine virtuali schermate e delle infrastrutture sorvegliate](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-overview)
Questa guida fornisce informazioni su come risolvere i problemi che possono verificarsi nell'ambiente della VM schermata.

### <a name="shielded-vm-article"></a>[Articolo sulle macchine virtuali schermate](http://windowsitpro.com/hyper-v/super-secure-hyper-v-environments-shielded-vms-2016)
Questo white paper offre una panoramica su come le VM schermate possano migliorare la protezione globale da eventuali manomissioni.                                         

## <a name="privileged-access-management"></a>la gestione degli accessi con privilegi)
### <a name="securing-privileged-access"></a>[Protezione dell'accesso con privilegi](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access)
Una guida di orientamento per proteggere l'accesso con privilegi. Questa guida di orientamento è strutturata in base all'esperienza combinata del team di sicurezza server, del team IT Microsoft, del team Azure e di Microsoft Consulting Services.                           

### <a name="just-in-time-administration-with-microsoft-identity-manager"></a>[Articolo relativo all'amministrazione Just in Time con Microsoft Identity Manager](https://technet.microsoft.com/library/mt150258.aspx)
In questo articolo vengono illustrate le funzionalità di Microsoft Identity Manager, incluso il supporto per la gestione accessi con privilegi Just In Time (JIT).                                                                    

### <a name="protecting-windows-and-microsoft-azure-active-directory-with-privileged-access-management"></a>[Protezione di Windows e Microsoft Azure Active Directory con Privileged Access Management](https://channel9.msdn.com/events/ignite/2015/brk3873)
Questa presentazione di Ignite descrive la strategia e gli investimenti di Windows Server, PowerShell, Active Directory, Identity Manager e Azure Active Directory di Microsoft per affrontare i rischi di accesso amministratore tramite un'autenticazione più efficace e per gestire l'accesso usando JIT (Just in Time) e JEA (Just Enough Administration).

### <a name="just-enough-administration-article"></a>[Articolo su JEA (Just Enough Administration)](https://aka.ms/JEA)
Questo documento illustra le finalità e i dettagli tecnici di JEA (Just Enough Administration), un toolkit di PowerShell che consente alle organizzazioni di ridurre i rischi limitando l'accesso degli operatori per eseguire attività specifiche.

### <a name="just-enough-administration-demo-video"></a>[Video dimostrativo su JEA (Just Enough Administration)](https://www.youtube.com/watch?v=xnBrbkY9P20)
Procedura dettagliata dimostrativa su Just Enough Administration.                                                                                                                  
## <a name="credential-protection"></a>Protezione delle credenziali

### <a name="protect-derived-domain-credentials-with-credential-guard"></a>[Proteggere le credenziali di dominio derivate con Credential Guard](https://docs.microsoft.com/windows/security/identity-protection/credential-guard/credential-guard)
Credential Guard usa la protezione basata su virtualizzazione per isolare i segreti in modo che solo il software di sistema con privilegi possa accedervi. L'accesso non autorizzato a questi segreti può produrre attacchi di furto delle credenziali come attacchi di tipo Pass-the-Hash o Pass-The-Ticket. Credential Guard impedisce questi attacchi proteggendo gli hash delle password NTLM e i ticket di concessione ticket Kerberos.

### <a name="protect-remote-desktop-credentials-with-remote-credential-guard"></a>[Proteggere le credenziali di Desktop remoto con Remote Credential Guard](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard)
Credential Guard remoto consente di proteggere le credenziali tramite una connessione Desktop remoto reindirizzando le richieste Kerberos al dispositivo che richiede la connessione. Fornisce inoltre il punto di accesso singolo per le sessioni Desktop remoto.                                                                                                        |
### <a name="credential-guard-demo-video"></a>[Video dimostrativo su Credential Guard](https://www.youtube.com/watch?v=eUpKOGSl7yk)
Questo video dimostrativo di 5 minuti descrive Credential Guard e Remote Credential Guard.         

## <a name="hardening-the-os-and-applications"></a>Protezione avanzata del sistema operativo e delle applicazioni
### <a name="windows-defender-application-control-wdac-deployment-guide"></a>[Guida alla distribuzione di Controllo di applicazioni di Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control)
Controllo di applicazioni di Windows Defender è un criterio di integrità del codice configurabile che consente alle aziende di controllare le applicazioni in esecuzione nell'ambiente in uso e non prevede requisiti hardware o software specifici oltre all'esecuzione di Windows 10.

### <a name="device-guard-demo-video"></a>[Video dimostrativo su Device Guard](https://www.youtube.com/watch?v=F-pTkesjkhI)
Device Guard è una combinazione di Controllo di applicazioni di Windows Defender e dell'integrità del codice protetta da Hypervisor. Questo video di 7 minuti descrive Device Guard e come viene usato in Windows Server.

### <a name="transport-layer-security-registry-settings"></a>[Impostazioni del registro di Transport Layer Security](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings)
Informazioni sulle impostazioni di registro supportate per l'implementazione in Windows del protocollo Transport Layer Security (TLS) e del protocollo SSL (Secure Sockets Layer).

### <a name="control-flow-guard"></a>[Protezione del flusso di controllo](https://docs.microsoft.com/windows/desktop/SecBP/control-flow-guard)
La protezione del flusso di controllo fornisce protezione integrata contro alcune classi di attacchi al danneggiamento della memoria.

### <a name="windows-defender"></a>[Windows Defender](https://technet.microsoft.com/windows-server-docs/security/windows-defender/windows-defender-overview-windows-server)
Windows Defender fornisce funzionalità di rilevamento attivo per bloccare i malware conosciuti. Windows Defender è attivato per impostazione predefinita ed è ottimizzato per supportare i vari ruoli server in Windows Server.

## <a name="detecting-and-responding-to-threats"></a>Rilevamento e risposta alle minacce
### <a name="security-threat-analysis-using-microsoft-operations-management-suite"></a>[Analisi delle minacce di sicurezza tramite Microsoft Operations Management Suite](https://channel9.msdn.com/events/ignite/2015/brk3464)
Questa presentazione di Ignite illustra come è possibile usare Operational Insights per eseguire l'analisi delle minacce alla sicurezza.

### <a name="microsoft-operations-management-suite-oms"></a>[Microsoft Operations Management Suite (OMS)](https://www.microsoft.com/server-cloud/operations-management-suite/overview.aspx)
La soluzione di sicurezza e controllo Microsoft Operations Management Suite (OMS) elabora i registri di protezione e gli eventi del firewall in ambienti locali e cloud per analizzare e rilevare comportamenti dannosi.

### <a name="oms-and-windows-server"></a>[OMS e Windows Server](https://www.youtube.com/watch?v=_SaDw1dRy2k)
Questo video di 3 minuti illustra in che modo OMS consente di rilevare potenziali comportamenti sospetti bloccati da Windows Server.  

### <a name="microsoft-advanced-threat-analytics"></a>[Analisi avanzata delle minacce Microsoft](https://blogs.technet.com/b/ad/archive/2015/07/22/microsoft-advanced-threat-analytics-coming-next-month.aspx)
Questo post di blog descrive Microsoft Advanced Threat Analytics, un prodotto locale che usa il traffico di rete Active Directory e i dati SIeM per rilevare i rischi potenziali e avvisare in merito.

### <a name="microsoft-advanced-threat-analytics"></a>[Analisi avanzata delle minacce Microsoft](https://www.youtube.com/watch?v=0nA9FeTRZFw&list=PL8nfc9haGeb5IZGM8HvmRozetHRpBDKSw)
In questo video di 3 minuti vengono presentate le informazioni generali sull'aggiunta delle funzionalità di analisi delle minacce in Windows Server da parte di Microsoft.                                                                                 |

## <a name="network-security"></a>Sicurezza di rete

### <a name="datacenter-firewall-overview"></a>[Panoramica del firewall del data center](https://technet.microsoft.com/library/dn920240.aspx)
Questo articolo illustra il firewall di data center, un firewall a livello di rete, con 5 tupla (protocollo, numeri di porta di origine e destinazione, indirizzi IP di origine e destinazione), con stato e multi-tenant.

### <a name="whats-new-in-dns-in-windows-server"></a>[Novità di DNS in Windows Server](https://technet.microsoft.com/windows-server-docs/networking/dns/what-s-new-in-dns-server)
Questa argomento riporta brevi descrizioni delle nuove funzionalità in DNS insieme a collegamenti per altre informazioni.                                                                           

## <a name="mapping-security-features-to-compliance-regulations"></a>Mapping di funzionalità di sicurezza in conformità alle normative

La conformità è un aspetto importante delle funzionalità di sicurezza. Microsoft fornisce agli esperti indicazioni su come ottenere la conformità e su come viene percepita tale conformità dai consulenti, ma fornisce anche mapping iniziale da usare durante la valutazione di Windows Server.

-   [White paper relativo al mapping sulla conformità delle macchine virtuali schermate in Hyper-V](https://download.microsoft.com/download/6/D/0/6D06E149-B4C1-4EED-ACD5-DF6066E93CC0/Coalfire_Branded_Hyper_V_Shielded_VMs_Whitepaper_EN_US.pdf)

-   [White paper relativo al mapping sulla conformità JEA e JIT](https://download.microsoft.com/download/2/7/A/27A2B5BB-6B52-4482-87C1-DA9D6B6D8C8D/Coalfire_Branded_Privileged_Identity_Manager_Whitepaper_EN_US.pdf)

-   [White paper sul mapping di conformità di Device Guard](https://download.microsoft.com/download/6/9/D/69D9E610-D23C-4F7E-A8CC-D65B87CEB4F8/Coalfire_Branded_Device_Guard_Whitepaper_EN_US.pdf)

-   [White paper relativo al mapping sulla conformità di Credential Guard](https://download.microsoft.com/download/8/1/2/812009C9-E4B8-4D4B-AADD-FDC373D0A076/Coalfire_Branded_Credential_Guard_Whitepaper_EN_US.pdf)

-   [White paper relativo al mapping sulla conformità di Windows Defender](https://download.microsoft.com/download/C/7/7/C778B7BB-0783-42D7-93A9-B86DFB5A7BAD/Coalfire_Branded_Windows_Defender_Whitepaper_EN_US.pdf)
