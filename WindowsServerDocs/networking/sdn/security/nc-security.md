---
title: Sicurezza controller di rete
description: È possibile utilizzare questo argomento per informazioni su come configurare la sicurezza per tutte le comunicazioni tra il controller di rete e altri software e dispositivi.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 54a8b9490fdf83d04c6b69fa88f4e8beca4f703a
ms.sourcegitcommit: 51e0b575ef43cd16b2dab2db31c1d416e66eebe8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76259066"
---
# <a name="secure-the-network-controller"></a>Proteggere Controller di rete

In questo argomento si apprenderà come configurare la sicurezza per tutte le comunicazioni tra il [controller di rete](../technologies/network-controller/network-controller.md) e altri software e dispositivi. 

I percorsi di comunicazione che è possibile proteggere includono la comunicazione in direzione nord sul piano di gestione, la comunicazione del cluster tra macchine virtuali del controller di rete \(VM\) in un cluster e la comunicazione a sud sul piano dati.

1. **Comunicazione verso nord**. Il controller di rete comunica sul piano di gestione con SDN\-software di gestione in grado di supportare come Windows PowerShell e System Center Virtual Machine Manager \(SCVMM\). Questi strumenti di gestione offrono la possibilità di definire i criteri di rete e di creare uno stato obiettivo per la rete, in cui è possibile confrontare la configurazione di rete effettiva per rendere la configurazione effettiva in parità con lo stato obiettivo.

2. **Comunicazione del cluster del controller di rete**. Quando si configurano tre o più macchine virtuali come nodi del cluster di controller di rete, questi nodi comunicano tra loro. Questa comunicazione potrebbe essere correlata alla sincronizzazione e alla replica dei dati tra i nodi o a una comunicazione specifica tra i servizi del controller di rete.

3.  **Comunicazione a sud**. Il controller di rete comunica sul piano dati con l'infrastruttura SDN e altri dispositivi come bilanciamento del carico software, gateway e computer host. È possibile usare il controller di rete per configurare e gestire questi dispositivi a sud in modo che mantengano lo stato degli obiettivi configurato per la rete.


## <a name="northbound-communication"></a>Comunicazione verso nord

Il controller di rete supporta l'autenticazione, l'autorizzazione e la crittografia per la comunicazione verso nord. Nelle sezioni seguenti vengono fornite informazioni su come configurare queste impostazioni di sicurezza.

### <a name="authentication"></a>Autenticazione

Quando si configura l'autenticazione per la comunicazione del controller di rete in direzione nord, si consente ai nodi del cluster del controller di rete e ai client di gestione di verificare l'identità del dispositivo con cui stanno comunicando.

Il controller di rete supporta le tre modalità di autenticazione seguenti tra i client di gestione e i nodi del controller di rete.

>[!Note]
>Se si distribuisce il controller di rete con System Center Virtual Machine Manager, è supportata solo la modalità **Kerberos** .

1. **Kerberos**. Utilizzare l'autenticazione Kerberos quando si uniscono sia il client di gestione che tutti i nodi del cluster controller di rete a un dominio Active Directory. Il dominio Active Directory deve disporre di account di dominio utilizzati per l'autenticazione.

2. **X509**. Usare X509 per l'autenticazione basata su certificato\-per i client di gestione non aggiunti a un dominio di Active Directory. È necessario registrare i certificati in tutti i nodi del cluster di controller di rete e i client di gestione. Inoltre, tutti i nodi e i client di gestione devono considerare attendibili i certificati di tutti gli altri.

3. **None**. Utilizzare None a scopo di test in un ambiente di testing e, pertanto, non è consigliabile utilizzarlo in un ambiente di produzione. Quando si sceglie questa modalità, non viene eseguita alcuna autenticazione tra i nodi e i client di gestione.

È possibile configurare la modalità di autenticazione per la comunicazione verso il Nord usando il comando di Windows PowerShell **[Install-NetworkController](https://docs.microsoft.com/powershell/module/networkcontroller/install-networkcontroller)** con il parametro _clientauthentication_ . 


### <a name="authorization"></a>Authorization

Quando si configura l'autorizzazione per la comunicazione verso il controller di rete in direzione nord, i nodi del cluster controller di rete e i client di gestione consentono di verificare che il dispositivo con cui stanno comunicando sia attendibile e disponga dell'autorizzazione per partecipare al comunicazione.

Usare i metodi di autorizzazione seguenti per ciascuna modalità di autenticazione supportata dal controller di rete.

1.  **Kerberos**. Quando si usa il metodo di autenticazione Kerberos, si definiscono gli utenti e i computer autorizzati a comunicare con il controller di rete creando un gruppo di sicurezza in Active Directory e quindi aggiungendo gli utenti e i computer autorizzati al gruppo. È possibile configurare il controller di rete per l'uso del gruppo di sicurezza per l'autorizzazione usando il parametro _ClientSecurityGroup_ del comando **[Install-NetworkController](https://docs.microsoft.com/powershell/module/networkcontroller/install-networkcontroller)** di Windows PowerShell. Dopo l'installazione del controller di rete, è possibile modificare il gruppo di sicurezza usando il comando **[set-NetworkController](https://docs.microsoft.com/powershell/module/networkcontroller/Set-NetworkController)** con il parametro _-ClientSecurityGroup_. Se si usa SCVMM, è necessario fornire il gruppo di sicurezza come parametro durante la distribuzione.

2.  **X509**. Quando si usa il metodo di autenticazione X509, il controller di rete accetta solo le richieste provenienti dai client di gestione le cui identificazioni personali del certificato sono note al controller di rete. È possibile configurare queste identificazioni personali usando il parametro _ClientCertificateThumbprint_ del comando **[Install-NetworkController](https://docs.microsoft.com/powershell/module/networkcontroller/install-networkcontroller)** di Windows PowerShell. È possibile aggiungere altre identificazioni personali del client in qualsiasi momento usando il comando **[set-NetworkController](https://docs.microsoft.com/powershell/module/networkcontroller/Set-NetworkController)** .

3.  **None**. Quando si sceglie questa modalità, non viene eseguita alcuna autenticazione tra i nodi e i client di gestione. Utilizzare None a scopo di test in un ambiente di testing e, pertanto, non è consigliabile utilizzarlo in un ambiente di produzione. 


### <a name="encryption"></a>Encryption

La comunicazione in direzione Nord USA Secure Sockets Layer \(\) SSL per creare un canale crittografato tra i client di gestione e i nodi del controller di rete. La crittografia SSL per la comunicazione verso il Nord include i requisiti seguenti:

- Tutti i nodi del controller di rete devono avere un certificato identico che includa l'autenticazione server e gli scopi di autenticazione client nell'utilizzo chiavi avanzato \(le estensioni EKU\). 

- L'URI utilizzato dai client di gestione per comunicare con il controller di rete deve essere il nome del soggetto del certificato. Il nome del soggetto del certificato deve contenere il nome di dominio completo (FQDN) o l'indirizzo IP dell'endpoint REST del controller di rete.

- Se i nodi del controller di rete si trovano in subnet diverse, il nome del soggetto dei rispettivi certificati deve corrispondere al valore usato per il parametro _restname_ nel comando **Install-NetworkController** di Windows PowerShell. 

- Tutti i client di gestione devono considerare attendibile il certificato SSL.


### <a name="ssl-certificate-enrollment-and-configuration"></a>Registrazione e configurazione dei certificati SSL

È necessario registrare manualmente il certificato SSL nei nodi del controller di rete.

Dopo la registrazione del certificato, è possibile configurare il controller di rete per l'uso del certificato con il parametro **-serverCertificate** del comando **Install-NetworkController** di Windows PowerShell. Se è già stato installato il controller di rete, è possibile aggiornare la configurazione in qualsiasi momento usando il comando **set-NetworkController** .

>[!NOTE]
>Se si utilizza SCVMM, è necessario aggiungere il certificato come risorsa di libreria. Per altre informazioni, vedere [configurare un controller di rete Sdn nell'infrastruttura VMM](https://docs.microsoft.com/system-center/vmm/sdn-controller).

## <a name="network-controller-cluster-communication"></a>Comunicazione del cluster del controller di rete

Il controller di rete supporta l'autenticazione, l'autorizzazione e la crittografia per la comunicazione tra i nodi del controller di rete. La comunicazione viene [stWindows Communication Foundation](https://docs.microsoft.com/dotnet/framework/wcf/whats-wcf) \(\) WCF e TCP.

Questa modalità può essere configurata con il parametro **ClusterAuthentication** del comando **Install-NetworkControllerCluster** di Windows PowerShell. 

Per ulteriori informazioni, vedere [Install-NetworkControllerCluster](https://docs.microsoft.com/powershell/module/networkcontroller/install-networkcontrollercluster).

### <a name="authentication"></a>Autenticazione

Quando si configura l'autenticazione per la comunicazione del cluster del controller di rete, è possibile consentire ai nodi del cluster controller di rete di verificare l'identità degli altri nodi con cui stanno comunicando.

Il controller di rete supporta le seguenti tre modalità di autenticazione tra i nodi del controller di rete.

>[!NOTE]
>Se si distribuisce il controller di rete tramite SCVMM, è supportata solo la modalità **Kerberos** .

1. **Kerberos**. È possibile usare l'autenticazione Kerberos quando tutti i nodi del cluster del controller di rete vengono aggiunti a un dominio Active Directory, con account di dominio usati per l'autenticazione.

2. **X509**. X509 è l'autenticazione basata su\-certificato. È possibile usare l'autenticazione X509 quando i nodi del cluster del controller di rete non sono aggiunti a un dominio Active Directory. Per usare X509, è necessario registrare i certificati in tutti i nodi del cluster del controller di rete e tutti i nodi devono considerare attendibili i certificati. Inoltre, il nome del soggetto del certificato registrato in ogni nodo deve essere uguale al nome DNS del nodo.

3. **None**. Quando si sceglie questa modalità, non viene eseguita alcuna autenticazione tra i nodi del controller di rete. Questa modalità viene fornita solo a scopo di test e non è consigliata per l'uso in un ambiente di produzione.

### <a name="authorization"></a>Authorization

Quando si configura l'autorizzazione per la comunicazione del cluster del controller di rete, è possibile consentire ai nodi del cluster controller di rete di verificare che i nodi con cui stanno comunicando siano attendibili e abbiano l'autorizzazione a partecipare alla comunicazione.

Per ogni modalità di autenticazione supportata dal controller di rete, vengono utilizzati i metodi di autorizzazione seguenti.

1. **Kerberos**. I nodi del controller di rete accettano richieste di comunicazione solo da altri account computer del controller di rete. È possibile configurare questi account quando si distribuisce il controller di rete usando il parametro **Name** del comando [New-NetworkControllerNodeObject](https://docs.microsoft.com/powershell/module/networkcontroller/new-networkcontrollernodeobject) di Windows PowerShell.

2. **X509**. I nodi del controller di rete accettano richieste di comunicazione solo da altri account computer del controller di rete. È possibile configurare questi account quando si distribuisce il controller di rete usando il parametro **Name** del comando [New-NetworkControllerNodeObject](https://docs.microsoft.com/powershell/module/networkcontroller/new-networkcontrollernodeobject) di Windows PowerShell.

3. **None**. Quando si sceglie questa modalità, non viene eseguita alcuna autorizzazione tra i nodi del controller di rete. Questa modalità viene fornita solo a scopo di test e non è consigliata per l'uso in un ambiente di produzione.

### <a name="encryption"></a>Encryption

La comunicazione tra i nodi del controller di rete viene crittografata mediante la crittografia a livello di trasporto WCF. Questa forma di crittografia viene utilizzata quando i metodi di autenticazione e autorizzazione sono certificati Kerberos o X509. Per ulteriori informazioni, vedere gli argomenti seguenti.

- [Procedura: Proteggere un servizio con credenziali di Windows](https://docs.microsoft.com/dotnet/framework/wcf/how-to-secure-a-service-with-windows-credentials)
- [Procedura: proteggere un servizio con certificati X. 509](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-secure-a-service-with-an-x-509-certificate).

## <a name="southbound-communication"></a>Comunicazione a sud

Il controller di rete interagisce con diversi tipi di dispositivi per la comunicazione a sud. Queste interazioni utilizzano protocolli diversi. Per questo motivo, esistono diversi requisiti per l'autenticazione, l'autorizzazione e la crittografia, a seconda del tipo di dispositivo e protocollo usato dal controller di rete per comunicare con il dispositivo.

La tabella seguente fornisce informazioni sull'interazione del controller di rete con diversi dispositivi in Sud.

| Dispositivo/servizio a sud | Protocollo              | Autenticazione utilizzata    |
|---------------------------|-----------------------|------------------------|
| Bilanciamento del carico software    | WCF (MUX), TCP (host) | Certificati           |
| Firewall                  | OVSDB                 | Certificati           |
| Gateway                   | WinRM                 | Kerberos, certificati |
| Reti virtuali        | OVSDB, WCF            | Certificati           |
| Routing definito dall'utente      | OVSDB                 | Certificati           |

Per ognuno di questi protocolli, il meccanismo di comunicazione è descritto nella sezione seguente.

### <a name="authentication"></a>Autenticazione

Per la comunicazione a sud vengono usati i protocolli e i metodi di autenticazione seguenti.

1. **WCF/TCP/OVSDB**. Per questi protocolli, l'autenticazione viene eseguita usando i certificati X509. Sia il controller di rete che il bilanciamento del carico software peer \(SLB\) multiplexer \(MUX\)computer/host presentano i rispettivi certificati per l'autenticazione reciproca. Ogni certificato deve essere considerato attendibile dal peer remoto.

    Per l'autenticazione a sud, è possibile usare lo stesso certificato SSL configurato per la crittografia della comunicazione con i client in direzione nord. È anche necessario configurare un certificato nei dispositivi SLB MUX e host. Il nome del soggetto del certificato deve corrispondere al nome DNS del dispositivo.

2. **WinRM**. Per questo protocollo, l'autenticazione viene eseguita usando \(Kerberos per i computer aggiunti al dominio\) e usando i certificati \(per i computer non appartenenti al dominio\).

### <a name="authorization"></a>Authorization

Per la comunicazione a sud vengono usati i protocolli e i metodi di autorizzazione seguenti.

1. **WCF/TCP**. Per questi protocolli, l'autorizzazione si basa sul nome del soggetto dell'entità peer. Il controller di rete archivia il nome DNS del dispositivo peer e lo usa per l'autorizzazione. Questo nome DNS deve corrispondere al nome del soggetto del dispositivo nel certificato. Analogamente, il certificato del controller di rete deve corrispondere al nome DNS del controller di rete archiviato nel dispositivo peer.

2. **WinRM**. Se si utilizza Kerberos, l'account client WinRM deve essere presente in un gruppo predefinito in Active Directory o nel gruppo Administrators locale nel server. Se vengono utilizzati i certificati, il client presenta un certificato al server che il server autorizza utilizzando il nome soggetto/emittente e il server utilizza un account utente mappato per eseguire l'autenticazione.

3.  **OVSDB**. Non è stata fornita alcuna autorizzazione per questo protocollo.

### <a name="encryption"></a>Encryption

Per la comunicazione a sud, per i protocolli vengono usati i metodi di crittografia seguenti.

1. **WCF/TCP/OVSDB**. Per questi protocolli, la crittografia viene eseguita utilizzando il certificato registrato sul client o sul server.

2. **WinRM**. Per impostazione predefinita, il traffico WinRM è crittografato utilizzando Security Support Provider Kerberos \(\)SSP. Nel server WinRM è possibile configurare la crittografia aggiuntiva, sotto forma di SSL.
