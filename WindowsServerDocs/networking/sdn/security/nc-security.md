---
title: Protezione del Controller di rete
description: È possibile utilizzare questo argomento per informazioni su come configurare la protezione di tutte le comunicazioni tra i Controller di rete e altri software e dispositivi.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ab04d3f528beb037988e6390fe85ad9af219266b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller-security"></a>Protezione del Controller di rete

È possibile utilizzare questo argomento per informazioni su come configurare la protezione di tutte le comunicazioni tra i Controller di rete e altri software e dispositivi. 

>[!NOTE]
>Per una panoramica del Controller di rete, vedere [Controller di rete](../technologies/network-controller/network-controller.md).

I percorsi di comunicazione che è possibile proteggere includono Northbound comunicazioni sul piano di gestione, comunicazioni del cluster tra \(VMs\) macchine virtuali Controller di rete in un cluster e comunicazione Southbound sul piano dati.

1. **Comunicazione northbound**. Controller di rete comunica sul piano di gestione con software di gestione in grado di supportare SDN\ come Windows PowerShell e \(SCVMM\) System Center Virtual Machine Manager. Questi strumenti di gestione ti offrono la possibilità di definire criteri di rete e per creare un obiettivo di stato per la rete, rispetto alla quale è possibile confrontare la configurazione di rete effettivo per aggiornare la configurazione effettiva in parità con lo stato di obiettivo.

2. **Comunicazioni del Cluster Controller di rete**. Quando si configurano tre o più macchine virtuali come nodi del cluster Controller di rete, questi nodi comunicano tra loro. Questa comunicazione potrebbe essere correlata alla sincronizzazione e la replica dei dati tra nodi o comunicazione specifiche tra i servizi del Controller di rete.

3.  **Comunicazione southbound**. Controller di rete comunica sul piano dati con l'infrastruttura SDN e altri dispositivi come servizi di bilanciamento del carico software, gateway e macchine host. È possibile utilizzare Controller di rete per configurare e gestire questi dispositivi southbound in modo che mantengono lo stato di obiettivo che è stato configurato per la rete.

## <a name="northbound-communication"></a>Comunicazione northbound

Controller di rete supporta l'autenticazione, autorizzazione e la crittografia per la comunicazione Northbound. Le sezioni seguenti forniscono informazioni su come configurare queste impostazioni di sicurezza.

**Autenticazione**

Quando si configura l'autenticazione per la comunicazione di rete Controller Northbound, si consente i nodi del cluster Controller di rete per i client di gestione per verificare l'identità del dispositivo con cui stanno comunicando.

Controller di rete supporta le seguenti tre modalità di autenticazione tra i client di gestione e i nodi di Controller di rete.

>[!Note]
>Se si distribuiscono Controller di rete con System Center Virtual Machine Manager, solo **Kerberos** modalità è supportata.

1. **Kerberos**. È possibile utilizzare l'autenticazione Kerberos quando entrambi client di gestione, ad esempio i computer che eseguono SCVMM e tutti i nodi del cluster di Controller di rete vengono aggiunti a un dominio di Active Directory, con gli account di dominio utilizzati per l'autenticazione.

2. **X509**. X509 è l'autenticazione basata su certificate\. È possibile utilizzare X509 autenticazione quando i client di gestione non appartengono a un dominio Active Directory. Per usare X509, è necessario registrare i certificati per tutti i nodi del cluster Controller di rete e i client di gestione e tutti i nodi e i client di gestione deve considerare attendibile tra loro ' certificati.

3. **Nessuno**. Quando si sceglie questa modalità, non esiste alcuna autenticazione eseguita tra i client di gestione e Controller di rete. Questa modalità viene fornita solo a scopo di test e non è consigliata per l'uso in un ambiente di produzione. 

È possibile configurare la modalità di autenticazione per la comunicazione Northbound utilizzando il comando Windows PowerShell **installazione NetworkController** con il **ClientAuthentication** parametro. 

Per ulteriori informazioni, vedere gli argomenti seguenti. 

- [Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)
- [Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)

**Autorizzazione**

Quando si configura l'autorizzazione per la comunicazione di rete Controller Northbound, si consente i nodi del cluster Controller di rete per i client di gestione per verificare che il dispositivo con cui stanno comunicando sia attendibile e che dispone dell'autorizzazione per partecipare alla comunicazione.

Per ognuna delle modalità di autenticazione supportata da Controller di rete, vengono utilizzati i seguenti metodi di autorizzazione.

1.  **Kerberos**. Quando si utilizza il metodo di autenticazione Kerberos, si definiscono gli utenti e computer che sono autorizzati a comunicare con il Controller di rete creando un gruppo di sicurezza in Active Directory e quindi aggiungere i computer e utenti autorizzati al gruppo. È possibile configurare il Controller di rete per utilizzare il gruppo di sicurezza per l'autorizzazione tramite il **ClientSecurityGroup** parametro del **installazione NetworkController** comando di Windows PowerShell. Dopo l'installazione di Controller di rete, è possibile modificare il gruppo di sicurezza utilizzando il [NetworkController Set](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller) comando con il parametro **- ClientSecurityGroup**. Se si utilizza SCVMM, è necessario fornire il gruppo di sicurezza come parametro durante la distribuzione.

2.  **X509**. Quando si utilizza il X509 metodo di autenticazione, Controller di rete accetta solo le richieste dei client di gestione cui identificazioni personali certificato sono noti al Controller di rete. È possibile configurare questi identificazioni personali tramite il **ClientCertificateThumbprint** parametro del **installazione NetworkController** comando di Windows PowerShell. È possibile aggiungere identificazioni personali da altri client in qualsiasi momento utilizzando il **NetworkController Set** comando.

3.  **Nessuno**. Quando si sceglie questa modalità, non esiste alcuna autorizzazione eseguita per i tentativi di comunicazione tra i client di gestione e i nodi di Controller di rete. Questa modalità viene fornita solo a scopo di test e non è consigliata per l'uso in un ambiente di produzione. 

Per ulteriori informazioni, vedere gli argomenti seguenti. 

- [Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)
- [Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)

**Crittografia**

Comunicazione northbound Usa \(SSL\) SSL (Secure Sockets Layer) per creare un canale crittografato tra i client di gestione e i nodi di Controller di rete. La crittografia SSL per la comunicazione Northbound include i seguenti requisiti.

- Tutti i nodi di Controller di rete devono avere un certificato che include gli scopi di autenticazione Server e Client nelle estensioni di utilizzo chiavi avanzato \(EKU\) identico. 

- L'URI utilizzato dai client di gestione per comunicare con il Controller di rete deve essere il nome soggetto del certificato. Il nome soggetto del certificato deve contenere il nome di dominio completo (FQDN) o l'indirizzo IP dell'Endpoint REST Controller di rete.

- Se i nodi di Controller di rete si trovano in subnet diverse, il nome del soggetto dei relativi certificati deve essere esattamente a quello che usi per la **RestName** parametro il **installazione NetworkController** comando di Windows PowerShell. 

- Il certificato SSL deve essere considerato attendibile da tutti i client di gestione.

### <a name="ssl-certificate-enrollment-and-configuration"></a>Configurazione e registrazione di certificati SSL

È necessario registrare manualmente il certificato SSL nei nodi di Controller di rete.

Dopo aver registrato il certificato, è possibile configurare il Controller di rete per utilizzare il certificato con la **- server** parametro del **installazione NetworkController** comando di Windows PowerShell. Se è già stato installato il Controller di rete, è possibile aggiornare la configurazione in qualsiasi momento utilizzando il **NetworkController Set** comando.

>[!NOTE]
>Se si utilizza SCVMM, è necessario aggiungere il certificato come una risorsa di libreria. Per ulteriori informazioni, vedere [configurare un controller di rete SDN nell'infrastruttura VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

## <a name="network-controller-cluster-communication"></a>Comunicazioni del Cluster Controller di rete

Controller di rete supporta l'autenticazione, autorizzazione e la crittografia per la comunicazione tra i nodi di Controller di rete. La comunicazione è su [Windows Communication Foundation](https://msdn.microsoft.com/library/ms731082.aspx) \(WCF\) e TCP.

È possibile configurare questa modalità con la **ClusterAuthentication** parametro del **installazione NetworkControllerCluster** comando di Windows PowerShell. 

Per ulteriori informazioni, vedere [installazione NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster).

**Autenticazione**

Quando si configura l'autenticazione per le comunicazioni del Cluster di Controller di rete, è consentire i nodi del cluster Controller di rete verificare l'identità degli altri nodi con cui stanno comunicando.

Controller di rete supporta le seguenti tre modalità di autenticazione tra i nodi di Controller di rete.

>[!NOTE]
>Se si distribuisce Controller di rete utilizzando solo SCVMM, **Kerberos** modalità è supportata.

1. **Kerberos**. È possibile utilizzare l'autenticazione Kerberos quando tutti i nodi del cluster di Controller di rete vengono aggiunti a un dominio di Active Directory, con gli account di dominio utilizzati per l'autenticazione.

2. **X509**. X509 è l'autenticazione basata su certificate\. È possibile utilizzare l'autenticazione quando i nodi del cluster Controller di rete non appartengono a un dominio Active Directory X509. Per usare X509, è necessario registrare i certificati per tutti i nodi del cluster di Controller di rete e tutti i nodi devono considerare attendibili i certificati. Inoltre, il nome del soggetto del certificato registrato in ogni nodo deve essere lo stesso nome DNS del nodo.

3. **Nessuno**. Quando si sceglie questa modalità, non esiste alcuna autenticazione eseguita tra i nodi di Controller di rete. Questa modalità viene fornita solo a scopo di test e non è consigliata per l'uso in un ambiente di produzione.

**Autorizzazione**

Quando si configura l'autorizzazione per le comunicazioni del Cluster di Controller di rete, è consentire i nodi del cluster Controller di rete verificare che i nodi con cui stanno comunicando siano attendibili e disporre dell'autorizzazione per partecipare alla comunicazione.

Per ognuna delle modalità di autenticazione supportata da Controller di rete, vengono utilizzati i seguenti metodi di autorizzazione.

1. **Kerberos**. Nodi di Controller di rete accettano le richieste di comunicazione solo dagli altri account computer del Controller di rete. È possibile configurare questi account quando si distribuisce Controller di rete utilizzando il **nome** parametro del [New NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) comando di Windows PowerShell.

2. **X509**. Nodi di Controller di rete accettano le richieste di comunicazione solo dagli altri account computer del Controller di rete. È possibile configurare questi account quando si distribuisce Controller di rete utilizzando il **nome** parametro del [New NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) comando di Windows PowerShell.

3. **Nessuno**. Quando si sceglie questa modalità, non esiste alcuna autorizzazione eseguita tra i nodi di Controller di rete. Questa modalità viene fornita solo a scopo di test e non è consigliata per l'uso in un ambiente di produzione.

**Crittografia**

Comunicazione tra i nodi di Controller di rete viene crittografata con la crittografia a livello di trasporto WCF. Questa forma di crittografia viene usata quando i metodi di autenticazione e autorizzazione sono Kerberos o X509 i certificati. Per ulteriori informazioni, vedere gli argomenti seguenti.

- [Procedura: proteggere un servizio con le credenziali di Windows](https://msdn.microsoft.com/library/ms734673.aspx)
- [Procedura: proteggere un servizio con certificati x. 509](https://msdn.microsoft.com/library/ms788968.aspx).

## <a name="southbound-communication"></a>Comunicazione southbound

Controller di rete interagisce con diversi tipi di dispositivi per la comunicazione Southbound. Queste interazioni utilizzano protocolli diversi. Per questo motivo, esistono requisiti diversi per l'autenticazione, autorizzazione e la crittografia a seconda del tipo di dispositivo e protocollo utilizzato dal Controller di rete per comunicare con il dispositivo.

Nella tabella seguente vengono fornite informazioni sull'interazione di Controller di rete con i dispositivi southbound diversi.

| Dispositivo o del servizio southbound | Protocollo              | Utilizzata l'autenticazione    |
|---------------------------|-----------------------|------------------------|
| Bilanciamento del carico software    | WCF (MUX), TCP (Host) | Certificati           |
| Firewall                  | OVSDB                 | Certificati           |
| Gateway                   | Gestione remota Windows                 | Kerberos, i certificati |
| Rete virtuale        | OVSDB, WCF            | Certificati           |
| Routing definita dall'utente      | OVSDB                 | Certificati           |

Per ognuno di questi protocolli, il meccanismo di comunicazione è descritta nella sezione seguente.

**Autenticazione**

Per la comunicazione Southbound, vengono utilizzati i seguenti protocolli e metodi di autenticazione.

1. **TCP/WCF/OVSDB**. Per questi protocolli, l'autenticazione viene eseguita utilizzando X509 i certificati. I Controller di rete e il bilanciamento del carico Software \(SLB\) peer Multiplexer \ (MUX\) al computer host presentare i certificati tra di loro per l'autenticazione reciproca. Ogni certificato deve essere considerato attendibile dal peer remoto.

    Per l'autenticazione southbound, è possibile utilizzare lo stesso certificato SSL configurato per crittografare la comunicazione con i client Northbound. È inoltre necessario configurare un certificato nel MUX SLB e dispositivi host. Il nome soggetto del certificato deve essere uguale al nome DNS del dispositivo.

2. **WinRM**. Per questo protocollo, viene eseguita l'autenticazione tramite Kerberos \ (per Machines \ appartenenti a un dominio) e utilizzando i certificati \ (per non appartenenti al dominio aggiunto Machines \).

**Autorizzazione**

Per la comunicazione Southbound, vengono utilizzati i seguenti protocolli e i metodi di autorizzazione.

1. **WCF/TCP**. Per questi protocolli, l'autorizzazione è basata sul nome del soggetto dell'entità di peer. Controller di rete archivia il nome DNS del dispositivo peer e la Usa per l'autorizzazione. Questo nome DNS deve corrispondere al nome soggetto del dispositivo nel certificato. Allo stesso modo, il certificato di Controller di rete deve corrispondere al nome di rete Controller DNS archiviato nel dispositivo peer.

2. **WinRM**. Se viene utilizzata Kerberos, l'account del client WinRM deve essere presente in un gruppo predefinito in Active Directory o del gruppo Administrators locale nel server. Se vengono utilizzati i certificati, il client presenta un certificato per il server in questione autorizza il server utilizzando il nome soggetto/autorità emittente, che il server utilizza un account utente mappati per eseguire l'autenticazione.

3.  **OVSDB**. Non esiste alcuna autorizzazione disponibili per questo protocollo.

**Crittografia**

Per la comunicazione Southbound, vengono utilizzati i seguenti metodi di crittografia per i protocolli.

1. **TCP/WCF/OVSDB**. Per questi protocolli, la crittografia viene eseguita usando il certificato che viene registrato nel client o server.

2. **WinRM**. WinRM traffico viene crittografato per impostazione predefinita utilizzando provider supporto sicurezza Kerberos \(SSP\). È possibile configurare la crittografia aggiuntiva, sotto forma di SSL, nel server di gestione remota Windows.
