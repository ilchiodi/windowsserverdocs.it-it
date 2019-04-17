---
title: Servizio DNS interno (iDNS) per SDN
description: Questo argomento illustra come fornire i servizi DNS per i carichi di lavoro tenant ospitata tramite DNS interno (iDNS), che è integrato con Software Defined Networking in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ad848a5b-0811-4c67-afe5-6147489c0384
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4bdb495b585cf60315dee60f9365e282877fdc1d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="internal-dns-service-idns-for-sdn"></a>\(IDNS\) del servizio DNS interno per SDN

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Se si lavora per un Provider di servizi Cloud \(CSP\) o organizzazione che prevede di distribuire Software Defined Networking \(SDN\) in Windows Server 2016, è possibile fornire i servizi DNS per i carichi di lavoro tenant ospitata tramite \(iDNS\) DNS interno, che è integrato con SDN.

Macchine virtuali ospitate \(VMs\) e le applicazioni richiedono DNS per comunicare all'interno di reti e risorse esterne su Internet. Con i nomi IDN, è possibile fornire tenant con servizi di risoluzione dei nomi DNS per la loro spazio dei nomi locali, isolato e per le risorse Internet.

Poiché il servizio nomi IDN non è accessibile da reti virtuali tenant, diverso da tramite il proxy di nomi IDN, il server non è vulnerabile ad attività dannose nelle reti tenant.

**Funzionalità principali**

Di seguito sono le funzionalità chiave per i nomi IDN.

- Fornisce che servizi per tenant di risoluzione dei nomi DNS condivisa carichi di lavoro
- Servizio DNS autorevole per la risoluzione dei nomi e la registrazione DNS all'interno dello spazio dei nomi tenant
- Servizio DNS Recursive per la risoluzione dei nomi Internet da macchine virtuali tenant.
- Se si desidera, è possibile configurare simultanee che ospita i nomi di infrastruttura e tenant
- Una soluzione economica DNS - tenant non è necessario distribuire la propria infrastruttura DNS
- Disponibilità elevata con l'integrazione di Active Directory che è obbligatorio.

Oltre a queste funzionalità, se si teme mantenendo l'integrato in Active Directory i server DNS aprire a Internet, è possibile distribuire server di nomi IDN dietro un altro resolver ricorsivo nella rete perimetrale.

Poiché i nomi IDN è un server centralizzato per tutte le query DNS, un CSP o Enterprise può anche implementare tenant DNS firewall, applicare filtri, rilevare attività dannose e controllare le transazioni in una posizione centrale

## <a name="idns-infrastructure"></a>nomi IDN infrastruttura
L'infrastruttura di nomi IDN include nomi IDN server e nomi IDN proxy.

### <a name="idns-servers"></a>nomi IDN server
nomi IDN include un insieme di server DNS che ospitano dati specifica del tenant, ad esempio record di risorse macchina virtuale.

server di nomi IDN sono server autorevoli per le zone DNS interno e anche agire come un resolver per i nomi pubblici tenant quando si tenta di macchine virtuali di connettersi a risorse esterne.

Tutti i nomi host per le macchine virtuali in reti virtuali vengono archiviati come record di risorse DNS nella zona stesso. Ad esempio, se si distribuisce nomi IDN per una zona denominata Contoso. Local, nell'area di Contoso. Local vengono archiviati i record di risorse DNS per le macchine virtuali in tale rete.

Tenant VM nomi di dominio completi \(FQDNs\) composto il nome del computer e la stringa del suffisso DNS per la rete virtuale, in formato GUID. Ad esempio, se si dispone di un tenant di macchina virtuale denominata TENANT1 che si trova in una rete virtuale contoso, locale, nome di dominio completo della macchina virtuale è TENANT1. *vn guid*. contoso. Local, in cui *vn guid* è la stringa del suffisso DNS per la rete virtuale.

>[!NOTE]
>Se sei un amministratore dell'infrastruttura, è possibile utilizzare l'infrastruttura CSP o Enterprise DNS come nomi IDN server anziché la distribuzione di nuovi server DNS in modo specifico per ricavarne nomi IDN server. Se si distribuiscono nuovi server per i nomi IDN o si utilizza l'infrastruttura esistente, i nomi IDN si basa su Active Directory per garantire un'elevata disponibilità. I server di nomi IDN devono pertanto essere integrati con Active Directory.

### <a name="idns-proxy"></a>nomi IDN Proxy
proxy nomi IDN è un servizio di Windows che viene eseguito in ogni host e tenant che inoltra il traffico per i nomi IDN Server DNS della rete virtuale.

La figura seguente illustra i percorsi del traffico DNS dal tenant reti virtuali tramite il proxy di nomi IDN per i nomi IDN Server e Internet.

![nomi IDN infrastruttura](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>Come distribuire nomi IDN
Quando si distribuisce SDN in Windows Server 2016 tramite script, i nomi IDN automaticamente è incluso nella distribuzione.

Per ulteriori informazioni, vedere gli argomenti seguenti.

- [Distribuire un'infrastruttura Software Defined Network usando gli script](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>Informazioni sui nomi IDN passaggi di distribuzione
È possibile utilizzare questa sezione per comprendere a fondo come nomi IDN è installato e configurato quando si distribuisce SDN con gli script.

Ecco un riepilogo dei passaggi necessari per distribuire i nomi IDN.

>[!NOTE]
>Se è stato distribuito SDN tramite script, non è necessario eseguire una qualsiasi di questi passaggi. Vengono forniti i passaggi per informazioni e solo a scopo di risoluzione dei problemi.

### <a name="step-1-deploy-dns"></a>Passaggio 1: Distribuzione di DNS
È possibile distribuire un server DNS utilizzando l'esempio seguente comando di Windows PowerShell.
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>Passaggio 2: Configurare le informazioni di nomi IDN nel Controller di rete
Questo segmento di script è una chiamata REST che viene eseguita dall'amministratore al Controller di rete, che viene informato sulla configurazione di zona nomi IDN -, ad esempio l'indirizzo IP del iDNSServer e la zona che viene usata per ospitare i nomi IDN. 

```
    Url: https://<url>/networking/v1/iDnsServer/configuration
Method: PUT
{
      "properties": {
        "connections": [
          {
            "managementAddresses": [
              "10.0.0.9"
            ],
            "credential": {
              "resourceRef": "/credentials/iDnsServer-Credentials"
            },
            "credentialType": "usernamePassword"
          }
        ],
        "zone": "contoso.local"
      }
    }
```

>[!NOTE]
>Si tratta di un estratto nella sezione **configurazione ConfigureIDns** in SDNExpress.ps1. Per ulteriori informazioni, vedere [distribuire un'infrastruttura Software Defined Network usando gli script](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-3-configure-the-idns-proxy-service"></a>Passaggio 3: Configurare il servizio Proxy nomi IDN
Il viene eseguito il servizio Proxy nomi IDN in ognuno degli host Hyper-V, fornendo il bridging tra le reti virtuali dei tenant e la rete fisica in cui si trovano i server di nomi IDN. Le seguenti chiavi del Registro di sistema devono essere create in ogni host Hyper-V.


**Porta DNS:** risolto porta 53

- Chiave del Registro di sistema = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- ValueName = "Porta"
- ValueData = 53
- ValueType = "Dword"
       

**Porta del Proxy DNS:** risolto porta 53

- Chiave del Registro di sistema = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- ValueName = "ProxyPort"
- ValueData = 53
- ValueType = "Dword"
        
**IP DNS:** questo è l'indirizzo IP fisso configurato nell'interfaccia di rete, nel caso in cui il tenant sceglie di utilizzare il servizio nomi IDN.

- Chiave del Registro di sistema = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- ValueName = "IP"
- ValueData = "169.254.169.254"
- ValueType = "String"

        
**Indirizzo MAC:** indirizzi Media Access Control del server DNS

- Chiave del Registro di sistema = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- ValueName = "MAC"
- ValueData = "aa-bb-cc-aa-bb-cc"
- ValueType = "String"

**Indirizzo del Server di nomi IDN:** un elenco delimitato da virgole di nomi IDN server.

- Chiave del Registro di sistema: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- ValueName = "Server d'inoltro"
- ValueData = "10.0.0.9"
- ValueType = "String"



>[!NOTE]
>Si tratta di un estratto nella sezione **configurazione ConfigureIDnsProxy** in SDNExpress.ps1. Per ulteriori informazioni, vedere [distribuire un'infrastruttura Software Defined Network usando gli script](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>Passaggio 4: Riavviare il servizio di agente Host Controller di rete
È possibile utilizzare il comando di Windows PowerShell seguente per riavviare il servizio di agente Host Controller di rete.
    
    Restart-Service nchostagent -Force
    
Per ulteriori informazioni, vedere [Restart-Service](https://technet.microsoft.com/library/hh849823.aspx).

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>Abilitare le regole del firewall per il servizio proxy DNS
È possibile utilizzare il seguente comando di Windows PowerShell per creare una regola firewall che consenta le eccezioni per il proxy comunicare con la macchina virtuale e il server di nomi IDN.
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

Per ulteriori informazioni, vedere [Enable-NetFirewallRule](https://technet.microsoft.com/library/jj554869.aspx).
    
### <a name="validate-the-idns-service"></a>Convalidare il servizio nomi IDN
Per convalidare i nomi IDN servizio, è necessario distribuire un carico di lavoro di esempio tenant.

Per ulteriori informazioni, vedere [creare una macchina virtuale e connettersi a una rete virtuale del Tenant o VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).

Se vuoi che il macchina virtuale per utilizzare il servizio nomi IDN tenant, è necessario lasciare vuota la configurazione di Server DNS interfacce di rete VM e consentire le interfacce per l'utilizzo di DHCP. 

Dopo l'avvio della macchina virtuale con un'interfaccia di rete, riceve automaticamente una configurazione che consente di utilizzare nomi IDN della macchina virtuale e la macchina virtuale avvia immediatamente l'esecuzione di risoluzione dei nomi utilizzando il servizio nomi IDN.

Se si configura il macchina virtuale per utilizzare il servizio nomi IDN lasciando vuoto Server DNS e Server DNS alternativo informazioni di interfaccia di rete tenant, Controller di rete fornisce la macchina virtuale con un indirizzo IP ed esegue la registrazione di un nome DNS per conto della macchina virtuale con i nomi IDN Server. 

Controller di rete, inoltre, informa il proxy di nomi IDN sulla macchina virtuale e i dettagli necessari per eseguire la risoluzione dei nomi per la macchina virtuale. 

Quando la macchina virtuale esegue una query DNS, il proxy funge da un server d'inoltro della query dalla rete virtuale per il servizio nomi IDN. 

Il proxy DNS garantisce inoltre che le query di macchina virtuale tenant siano isolate. Se il server di nomi IDN è autorevole per la query, il server di nomi IDN risponde con una risposta autorevole. Se il server di nomi IDN non è autorevole per la query, esegue una ricorsione DNS per risolvere i nomi Internet.

>[!NOTE]
>Queste informazioni sono incluse nella sezione **configurazione AttachToVirtualNetwork** in SDNExpressTenant.ps1. Per ulteriori informazioni, vedere [distribuire un'infrastruttura Software Defined Network usando gli script](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

