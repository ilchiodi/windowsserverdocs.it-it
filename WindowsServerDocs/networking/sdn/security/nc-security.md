---
title: Sicurezza controller di rete
description: È possibile utilizzare questo argomento per informazioni su come configurare la sicurezza per tutte le comunicazioni tra Controller di rete e altri software e ai dispositivi.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: fccd6ee1ba734d72629264bd5b3bb7d93ddcdb70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884762"
---
# <a name="secure-the-network-controller"></a>Proteggere Controller di rete

In questo argomento descrive come configurare la sicurezza per tutte le comunicazioni tra [Controller di rete](../technologies/network-controller/network-controller.md) e altro software e i dispositivi. 

I percorsi di comunicazione che è possibile proteggere la comunicazione di Northbound sul piano di gestione, comunicazioni del cluster tra le macchine virtuali Controller di rete di includere \(macchine virtuali\) in un cluster e la comunicazione di Southbound sui dati piano.

1. **La comunicazione di northbound**. Controller di rete comunica sul piano di gestione con SDN\-software di gestione in grado di supportare, ad esempio Windows PowerShell e System Center Virtual Machine Manager \(SCVMM\). Questi strumenti di gestione offrono la possibilità di definire criteri di rete e per creare un obiettivo di stato per la rete, in cui è possibile confrontare la configurazione di rete effettivi per rendere la configurazione effettiva parità con lo stato dell'obiettivo.

2. **Le comunicazioni del Cluster di Controller di rete**. Quando si configurano tre o più macchine virtuali come nodi del cluster Controller di rete, questi nodi comunicano tra loro. Questa comunicazione potrebbe essere correlata alla replica dei dati e la sincronizzazione tra nodi o comunicazione specifici tra i servizi del Controller di rete.

3.  **La comunicazione di southbound**. Controller di rete comunica sul piano dati con l'infrastruttura SDN e altri dispositivi, ad esempio servizi di bilanciamento del carico software, i gateway e macchine host. È possibile usare Controller di rete per configurare e gestire questi dispositivi southbound in modo che mantengano lo stato dell'obiettivo che è stato configurato per la rete.


## <a name="northbound-communication"></a>Comunicazione di northbound

Controller di rete supporta l'autenticazione, autorizzazione e crittografia per la comunicazione di Northbound. Le sezioni seguenti forniscono informazioni su come configurare queste impostazioni di sicurezza.

### <a name="authentication"></a>Autenticazione

Quando si configura l'autenticazione per la comunicazione di Northbound di Controller di rete, è consentire i nodi del cluster Controller di rete e i client di gestione per verificare l'identità del dispositivo con cui stanno comunicando.

Controller di rete supporta le seguenti tre modalità di autenticazione tra i client di gestione e i nodi di Controller di rete.

>[!Note]
>Se si distribuiscono Controller di rete con System Center Virtual Machine Manager, solo **Kerberos** modalità è supportata.

1. **Kerberos**. Usare l'autenticazione Kerberos quando si aggiunge il client di gestione e tutti i nodi del cluster di Controller di rete a un dominio di Active Directory. Il dominio di Active Directory deve avere gli account di dominio usati per l'autenticazione.

2. **X509**. Usare X509 certificato\-basato su autenticazione per i client di gestione non aggiunti a un dominio di Active Directory. È necessario registrare i certificati per tutti i nodi del cluster Controller di rete e i client di gestione. Inoltre, tutti i nodi e i client di gestione devono considerare attendibile i certificati reciproci.

3. **Nessuno**. Usare None per il test a scopo in un ambiente di test e, pertanto, non è consigliato per l'uso in un ambiente di produzione. Quando si sceglie questa modalità, non vi è alcuna autenticazione eseguita tra i nodi e i client di gestione.

È possibile configurare la modalità di autenticazione per la comunicazione di Northbound con il comando di Windows PowerShell **[installazione NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** con il _ClientAuthentication_ parametro. 


### <a name="authorization"></a>Authorization

Quando si configura l'autorizzazione per la comunicazione di Northbound di Controller di rete, è consentire di nodi del cluster Controller di rete e i client di gestione per verificare che il dispositivo con il quale comunicano è attendibile e dispone dell'autorizzazione per partecipare il comunicazione.

Usare i metodi di autorizzazione seguenti per ognuna delle modalità di autenticazione supportate dal Controller di rete.

1.  **Kerberos**. Quando si usa il metodo di autenticazione Kerberos, si definiscono gli utenti e computer autorizzati a comunicare con il Controller di rete creando un gruppo di sicurezza in Active Directory e quindi aggiungere gli utenti autorizzati e i computer al gruppo. È possibile configurare il Controller di rete per usare il gruppo di sicurezza per l'autorizzazione tramite il _ClientSecurityGroup_ parametro delle **[installazione NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** Comando di Windows PowerShell. Dopo aver installato il Controller di rete, è possibile modificare il gruppo di sicurezza usando il **[NetworkController Set](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** comando con il parametro _- ClientSecurityGroup_ . Se si usa SCVMM, è necessario fornire il gruppo di sicurezza come parametro durante la distribuzione.

2.  **X509**. Quando si utilizza X509 come metodo di autenticazione Controller di rete accetta solo le richieste dai client di gestione cui identificazioni personali del certificato sono noti al Controller di rete. È possibile configurare le identificazioni personali tramite il _ClientCertificateThumbprint_ parametro delle **[installazione NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** comando Windows PowerShell. È possibile aggiungere altri le identificazioni personali del client in qualsiasi momento usando il **[NetworkController Set](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** comando.

3.  **Nessuno**. Quando si sceglie questa modalità, non vi è alcuna autenticazione eseguita tra i nodi e i client di gestione. Usare None per il test a scopo in un ambiente di test e, pertanto, non è consigliato per l'uso in un ambiente di produzione. 


### <a name="encryption"></a>Crittografia

La comunicazione di northbound Usa Secure Sockets Layer \(SSL\) per creare un canale crittografato tra i client di gestione e i nodi di Controller di rete. La crittografia SSL per la comunicazione di Northbound include i requisiti seguenti:

- Tutti i nodi di Controller di rete devono avere un certificato identico che include gli scopi di autenticazione Server e Client autenticazione utilizzo chiavi avanzato \(EKU\) estensioni. 

- L'URI utilizzato dai client di gestione per comunicare con il Controller di rete deve essere il nome soggetto del certificato. Il nome soggetto del certificato deve contenere il nome di dominio completo (FQDN) o l'indirizzo IP dell'Endpoint REST Controller di rete.

- Se i nodi di Controller di rete in subnet diverse, il nome del soggetto dei relativi certificati deve essere lo stesso come il valore usato per il _RestName_ parametro nel **installazione NetworkController** Windows Comando di PowerShell. 

- Tutti i client di gestione deve considerare attendibile il certificato SSL.


### <a name="ssl-certificate-enrollment-and-configuration"></a>Configurazione e registrazione del certificato SSL

È necessario registrare manualmente il certificato SSL nei nodi di Controller di rete.

Dopo aver registrato il certificato, è possibile configurare il Controller di rete per usare il certificato con il **- ServerCertificate** parametro delle **installazione NetworkController** comando Windows PowerShell . Se è già stato installato il Controller di rete, è possibile aggiornare la configurazione in qualsiasi momento usando il **NetworkController Set** comando.

>[!NOTE]
>Se si usa SCVMM, è necessario aggiungere il certificato come una risorsa di libreria. Per altre informazioni, vedere [configurare un controller di rete SDN nell'infrastruttura di VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

## <a name="network-controller-cluster-communication"></a>Comunicazioni del Cluster di Controller di rete

Controller di rete supporta l'autenticazione, autorizzazione e crittografia per la comunicazione tra i nodi del Controller di rete. La comunicazione è posizionato [Windows Communication Foundation](https://msdn.microsoft.com/library/ms731082.aspx) \(WCF\) e TCP.

È possibile configurare la modalità con la **ClusterAuthentication** parametro delle **installazione NetworkControllerCluster** comando Windows PowerShell. 

Per altre informazioni, vedere [installazione NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster).

### <a name="authentication"></a>Autenticazione

Quando si configura l'autenticazione per le comunicazioni del Cluster di Controller di rete, è consentire ai nodi di cluster di Controller di rete per verificare l'identità degli altri nodi con cui stanno comunicando.

Controller di rete supporta le seguenti tre modalità di autenticazione tra i nodi del Controller di rete.

>[!NOTE]
>Se si distribuisce il Controller di rete con SCVMM, solo **Kerberos** modalità è supportata.

1. **Kerberos**. È possibile usare l'autenticazione Kerberos quando tutti i nodi del cluster di Controller di rete vengono aggiunti a un dominio di Active Directory, con gli account di dominio usati per l'autenticazione.

2. **X509**. X509 è certificato\-l'autenticazione basata su. È possibile usare l'autenticazione quando i nodi del cluster Controller di rete non vengono aggiunti a un dominio di Active Directory X509. Per usare X509, è necessario registrare i certificati per tutti i nodi del cluster di Controller di rete e tutti i nodi devono considerare attendibili i certificati. Inoltre, il nome del soggetto del certificato registrato in ogni nodo deve essere uguale al nome DNS del nodo.

3. **Nessuno**. Quando si sceglie questa modalità, non vi è alcuna autenticazione eseguita tra i nodi del Controller di rete. Questa modalità viene fornita solo per scopi di test e non è consigliata per l'uso in un ambiente di produzione.

### <a name="authorization"></a>Authorization

Quando si configura l'autorizzazione per le comunicazioni del Cluster di Controller di rete, è consentire ai nodi di cluster di Controller di rete per verificare che i nodi con cui comunicano siano attendibili e dispone dell'autorizzazione per partecipare alla comunicazione.

Per ognuna delle modalità di autenticazione supportate dal Controller di rete, vengono usati i seguenti metodi di autorizzazione.

1. **Kerberos**. Nodi di Controller di rete accettano le richieste di comunicazione solo da altri account di computer Controller di rete. È possibile configurare questi account quando si distribuisce Controller di rete usando il **Name** parametro del [New NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) comando Windows PowerShell.

2. **X509**. Nodi di Controller di rete accettano le richieste di comunicazione solo da altri account di computer Controller di rete. È possibile configurare questi account quando si distribuisce Controller di rete usando il **Name** parametro del [New NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) comando Windows PowerShell.

3. **Nessuno**. Quando si sceglie questa modalità, non è disponibile alcuna autorizzazione eseguita tra i nodi del Controller di rete. Questa modalità viene fornita solo per scopi di test e non è consigliata per l'uso in un ambiente di produzione.

### <a name="encryption"></a>Crittografia

Comunicazione tra i nodi di Controller di rete viene crittografata con crittografia a livello di trasporto WCF. Questo modulo di crittografia viene utilizzato quando i metodi di autenticazione e autorizzazione sono Kerberos o X509 i certificati. Per ulteriori informazioni, vedere gli argomenti seguenti.

- [Procedura: Proteggere un servizio con le credenziali di Windows](https://msdn.microsoft.com/library/ms734673.aspx)
- [Procedura: Proteggere un servizio con certificati X.509](https://msdn.microsoft.com/library/ms788968.aspx).

## <a name="southbound-communication"></a>Comunicazione di southbound

Controller di rete interagisce con diversi tipi di dispositivi per la comunicazione di Southbound. Tali interazioni utilizzano protocolli diversi. Per questo motivo, esistono diversi requisiti per l'autenticazione, autorizzazione e la crittografia in base al tipo di dispositivo e il protocollo usato dal Controller di rete per comunicare con il dispositivo.

Nella tabella seguente fornisce informazioni sull'interazione di Controller di rete con diversi dispositivi southbound.

| Servizio/dispositivo southbound | Protocollo              | Autenticazione usata    |
|---------------------------|-----------------------|------------------------|
| Bilanciamento del carico software    | WCF (MUX), TCP (Host) | Certificati           |
| Firewall                  | OVSDB                 | Certificati           |
| Gateway                   | WinRM                 | Kerberos, i certificati |
| Rete virtuale        | OVSDB, WCF            | Certificati           |
| Routing definito dall'utente      | OVSDB                 | Certificati           |

Per ognuno di questi protocolli, il meccanismo di comunicazione è descritto nella sezione seguente.

### <a name="authentication"></a>Autenticazione

Per la comunicazione di Southbound, vengono usati i seguenti protocolli e metodi di autenticazione.

1. **WCF/TCP/OVSDB**. Per questi protocolli, l'autenticazione viene eseguita utilizzando X509 i certificati. Controller di rete sia il peer Software Load Balancing \(SLB\) Multiplexer \(MUX \) /macchine host presentano i certificati tra loro per l'autenticazione reciproca. Ogni certificato deve essere considerato attendibile dal peer remoto.

    Per l'autenticazione southbound, è possibile usare lo stesso certificato SSL configurato per crittografare le comunicazioni con i client Northbound. È anche necessario configurare un certificato sul MUX SLB e dispositivi host. Il nome soggetto del certificato deve essere uguale al nome DNS del dispositivo.

2. **WinRM**. Per questo protocollo, l'autenticazione viene eseguita tramite Kerberos \(per dominio di computer aggiunti\) e l'utilizzo di certificati \(per non di dominio i computer appartenenti al\).

### <a name="authorization"></a>Authorization

Per la comunicazione di Southbound, vengono usati i seguenti protocolli e metodi di autorizzazione.

1. **WCF/TCP**. Per questi protocolli, l'autorizzazione è basata sul nome del soggetto dell'entità peer. Controller di rete archivia il nome DNS del dispositivo peer e lo usa per l'autorizzazione. Questo nome DNS deve corrispondere al nome soggetto del dispositivo nel certificato. Analogamente, certificato del Controller di rete deve corrispondere al nome DNS di Controller di rete archiviato nel dispositivo peer.

2. **WinRM**. Se viene utilizzato Kerberos, l'account del client WinRM deve essere presente in un gruppo predefinito in Active Directory o del gruppo Administrators locale nel server. Se si utilizzano certificati, il client presenta un certificato al server che autorizza il server usando il nome di soggetto/autorità emittente e il server utilizza un account utente con mapping per eseguire l'autenticazione.

3.  **OVSDB**. Non è disponibile alcuna autorizzazione disponibili per questo protocollo.

### <a name="encryption"></a>Crittografia

Per la comunicazione di Southbound, vengono utilizzati i seguenti metodi di crittografia per i protocolli.

1. **WCF/TCP/OVSDB**. Per questi protocolli, la crittografia viene eseguita usando il certificato che viene registrato nel client o server.

2. **WinRM**. Il traffico di gestione remota Windows viene crittografato per impostazione predefinita utilizzando provider supporto sicurezza Kerberos \(SSP\). È possibile configurare la crittografia aggiuntiva, sotto forma di SSL, nel server di gestione remota Windows.
