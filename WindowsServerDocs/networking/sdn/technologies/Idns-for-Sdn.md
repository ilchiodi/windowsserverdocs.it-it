---
title: Servizio DNS interno (iDNS) per SDN
description: Questo argomento viene illustrato come è possibile fornire i servizi DNS per i carichi di lavoro tenant ospitato usando DNS interno (iDNS), che è integrato con Software Defined Networking di Windows Server 2016.
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
ms.openlocfilehash: 4d4ae5ee5f5600d86349ca26b7acbdb284b45bac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824082"
---
# <a name="internal-dns-service-idns-for-sdn"></a>Servizio DNS interno (iDNS) per SDN

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Se si lavora per un Provider di servizi Cloud \(CSP\) o dell'organizzazione che prevede di distribuire Software Defined Networking \(SDN\) in Windows Server 2016, è possibile fornire servizi DNS per i carichi di lavoro tenant ospitato tramite DNS interno \(IDN\), che è integrato con SDN.

Le macchine virtuali ospitate \(macchine virtuali\) e le applicazioni richiedono il DNS per comunicare all'interno delle proprie reti e con risorse esterne su Internet. Con i nomi IDN, è possibile fornire tenant con servizi di risoluzione dei nomi DNS relativi lo spazio dei nomi locale, isolato e per le risorse Internet.

Poiché il servizio nomi IDN non è accessibile da reti virtuali tenant, diverso da attraverso il proxy di nomi IDN, il server non è vulnerabile ad attività dannose su reti tenant.

**Funzionalità principali**

Di seguito sono le funzionalità principali per nomi IDN.

- Fornisce che servizi di risoluzione per il tenant del nome DNS condivisi i carichi di lavoro
- Servizio DNS autorevole per la risoluzione dei nomi e la registrazione DNS all'interno di spazio dei nomi di tenant
- Servizio Recursive DNS per la risoluzione dei nomi Internet dalla VM tenant.
- Se si desidera, è possibile configurare l'hosting simultaneo dei nomi dell'infrastruttura e tenant
- Una conveniente soluzione DNS - tenant non è necessario distribuire la propria infrastruttura DNS
- Disponibilità elevata con l'integrazione di Active Directory, che è obbligatorio.

Oltre a queste funzionalità, se si è interessati a mantenere l'integrato in Active Directory i server DNS aperte a Internet, è possibile distribuire server di nomi IDN dietro a un altro sistema di risoluzione ricorsiva nella rete perimetrale.

Poiché i nomi IDN è un server centralizzato per tutte le query DNS, un CSP o Enterprise può anche implementare tenant DNS firewall, applicare filtri, rilevare attività dannose e controllare le transazioni in una posizione centrale

## <a name="idns-infrastructure"></a>nomi IDN infrastruttura
L'infrastruttura di nomi IDN include i nomi IDN server e nomi IDN proxy.

### <a name="idns-servers"></a>nomi IDN server
i nomi IDN include un set di server DNS che ospitano i dati specifici del tenant, ad esempio i record DNS della macchina virtuale.

server di nomi IDN sono server autorevoli per le zone DNS interni e fungere anche da un sistema di risoluzione dei nomi pubblici del tenant quando il tentativo di macchine virtuali per la connessione alle risorse esterne.

Tutti i nomi host per le macchine virtuali in reti virtuali vengono archiviati come record di risorse DNS nella stessa zona. Ad esempio, se si distribuisce iDNS per una zona denominata Contoso. Local, i record risorsa DNS per le macchine virtuali in tale rete vengono archiviati nell'area di Contoso. Local.

Macchina virtuale di nomi di dominio completi del tenant \(FQDN\) costituito il nome del computer e la stringa di suffisso DNS per la rete virtuale, in formato GUID. Ad esempio, se si dispone di un tenant di macchina virtuale denominata TENANT1 che si trova in contoso, rete virtuale locale, nome di dominio completo della macchina virtuale è TENANT1. *vn-guid*. contoso. Local, dove *vn-guid* è la stringa di suffisso DNS per la rete virtuale.

>[!NOTE]
>Se sei un amministratore dell'infrastruttura, è possibile utilizzare l'infrastruttura CSP o Enterprise DNS come nomi IDN server invece di distribuire nuovi server DNS in modo specifico per utilizzare come nomi IDN server. Se si distribuiscono nuovi server per nomi IDN o si usa l'infrastruttura esistente, i nomi IDN si basa su Active Directory per garantire un'elevata disponibilità. I server di nomi IDN devono pertanto essere integrati con Active Directory.

### <a name="idns-proxy"></a>iDNS Proxy
i nomi IDN proxy è un servizio Windows che viene eseguito in ogni host e tenant che inoltra il traffico DNS della rete virtuale per i nomi IDN Server.

La figura seguente illustra i percorsi del traffico DNS da tenant le reti virtuali tramite il proxy di nomi IDN per i nomi IDN, Server e Internet.

![nomi IDN infrastruttura](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>Come distribuire IDN
Quando si distribuisce SDN in Windows Server 2016 tramite script, i nomi IDN è automaticamente incluso nella distribuzione.

Per ulteriori informazioni, vedere gli argomenti seguenti.

- [Distribuire un'infrastruttura Software Defined Networking tramite script](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>Informazioni sui nomi IDN passaggi di distribuzione
È possibile utilizzare questa sezione per comprendere come i nomi IDN è installato e configurato quando si distribuisce SDN tramite script.

Viene riportato un riepilogo dei passaggi necessari per distribuire i nomi IDN.

>[!NOTE]
>Se è stata distribuita SDN tramite script, non devi eseguire uno di questi passaggi. Vengono forniti i passaggi per informazioni e risoluzione dei problemi relativi a solo scopo.

### <a name="step-1-deploy-dns"></a>Passaggio 1: DNS di distribuzione
È possibile distribuire un server DNS tramite il seguente comando Windows PowerShell.
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>Passaggio 2: Configurare le informazioni di nomi IDN nel Controller di rete
Il segmento di script è una chiamata REST che viene eseguita dall'amministratore al Controller di rete, che viene informato sulla configurazione della zona iDNS - ad esempio l'indirizzo IP del iDNSServer e il fuso orario che viene usato per ospitare i nomi IDN. 

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
>Questo è un estratto dalla sezione **ConfigureIDns configurazione** in SDNExpress.ps1. Per altre informazioni, vedere [distribuire un'infrastruttura Software Defined Networking tramite script](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-3-configure-the-idns-proxy-service"></a>Passaggio 3: Configurare il servizio Proxy di nomi IDN
I nomi IDN servizio Proxy viene eseguito in ognuno degli host Hyper-V, fornendo il ponte tra le reti virtuali dei tenant e la rete fisica in cui si trovano i server di nomi IDN. Le seguenti chiavi del Registro di sistema devono essere create in ogni host Hyper-V.


**Porta DNS:** Porta fissa 53

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "Port"
- ValueData = 53
- ValueType = "Dword"
       

**Porta del Proxy DNS:** Porta fissa 53

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "ProxyPort"
- ValueData = 53
- ValueType = "Dword"
        
**INDIRIZZO IP DNS:** Indirizzo IP fisso configurato nell'interfaccia di rete, nel caso in cui il tenant viene scelto di utilizzare il servizio nomi IDN

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "IP"
- ValueData = "169.254.169.254"
- ValueType = "String"

        
**Indirizzo MAC:** Indirizzo Media Access Control del server DNS

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- ValueName = "MAC"
- ValueData = “aa-bb-cc-aa-bb-cc”
- ValueType = "String"

**Indirizzo del Server di nomi IDN:** Un elenco delimitato da virgole di nomi IDN server.

- Chiave del Registro di sistema: HKLM\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- ValueName = "Server di inoltro"
- ValueData = “10.0.0.9”
- ValueType = "String"



>[!NOTE]
>Questo è un estratto dalla sezione **ConfigureIDnsProxy configurazione** in SDNExpress.ps1. Per altre informazioni, vedere [distribuire un'infrastruttura Software Defined Networking tramite script](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>Passaggio 4: Riavviare il servizio agente Host di rete Controller
È possibile utilizzare il comando Windows PowerShell seguente per riavviare il servizio di agente Host di Controller di rete.
    
    Restart-Service nchostagent -Force
    
Per altre informazioni, vedere [Restart-Service](https://technet.microsoft.com/library/hh849823.aspx).

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>Abilitare le regole del firewall per il servizio proxy DNS
È possibile usare il comando Windows PowerShell seguente per creare una regola del firewall che supporta le eccezioni per il proxy comunicare con la macchina virtuale e il server di nomi IDN.
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

Per altre informazioni, vedere [Enable-NetFirewallRule](https://technet.microsoft.com/library/jj554869.aspx).
    
### <a name="validate-the-idns-service"></a>Convalidare il servizio nomi IDN
Per convalidare i nomi IDN, servizio, è necessario distribuire un carico di lavoro tenant di esempio.

Per altre informazioni, vedere [creare una macchina virtuale e connettersi a una rete virtuale del Tenant o VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).

Se si desidera che il tenant di macchina virtuale da usare il servizio nomi IDN, è necessario lasciare vuota la configurazione di Server DNS interfacce di rete della macchina virtuale e consentire le interfacce da usare il protocollo DHCP. 

Dopo aver avviata la macchina virtuale con un'interfaccia di rete, riceve automaticamente una configurazione che consente la macchina virtuale usi nomi IDN, e la macchina virtuale inizia immediatamente eseguendo la risoluzione dei nomi usando il servizio nomi IDN.

Se si configura il macchina virtuale usare il servizio nomi IDN lasciando vuoto Server DNS e Server DNS alternativo informazioni di interfaccia di rete tenant, Controller di rete fornisce la macchina virtuale con un indirizzo IP ed esegue la registrazione di un nome DNS per conto della VM con il Server di nomi IDN . 

Controller di rete indica inoltre il proxy di nomi IDN, sulla macchina virtuale e i dettagli necessari per eseguire la risoluzione dei nomi per la macchina virtuale. 

Quando la macchina virtuale viene avviata una query DNS, il proxy agisce come un server d'inoltro di query dalla rete virtuale per il servizio nomi IDN. 

Il proxy DNS assicura anche che le query di macchine Virtuali tenant siano isolate. Se il server di nomi IDN è autorevole per la query, il server di nomi IDN invia una risposta autorevole. Se il server di nomi IDN non autorevole per la query, esegue una ricorsione di DNS per risolvere i nomi di Internet.

>[!NOTE]
>Queste informazioni sono incluse nella sezione **AttachToVirtualNetwork configurazione** in SDNExpressTenant.ps1. Per altre informazioni, vedere [distribuire un'infrastruttura Software Defined Networking tramite script](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

