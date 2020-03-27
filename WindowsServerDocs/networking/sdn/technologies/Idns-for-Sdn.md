---
title: Servizio DNS interno (iDNS) per SDN
description: Questo argomento illustra come fornire i servizi DNS ai carichi di lavoro tenant ospitati usando DNS interno (IDN), integrato con Software Defined Networking in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ad848a5b-0811-4c67-afe5-6147489c0384
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0b08e5af20b3d089391717479817e0d26329fde2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317250"
---
# <a name="internal-dns-service-idns-for-sdn"></a>Servizio DNS interno (iDNS) per SDN

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Se si lavora per un provider di servizi cloud \(CSP\) o Enterprise che prevede la distribuzione di Software Defined Networking \(SDN\) in Windows Server 2016, è possibile fornire servizi DNS ai carichi di lavoro tenant ospitati usando DNS interno \(IDN\), che è integrato con SDN.

Le macchine virtuali ospitate \(VM\) e le applicazioni richiedono che il DNS comunichi all'interno delle proprie reti e con risorse esterne su Internet. Con IDN, è possibile fornire ai tenant i servizi di risoluzione dei nomi DNS per lo spazio dei nomi locale isolato e per le risorse Internet.

Poiché il servizio IDN non è accessibile da reti virtuali tenant, ad eccezione del proxy IDN, il server non è vulnerabile a attività dannose nelle reti tenant.

**Funzionalità principali**

Di seguito sono riportate le funzionalità chiave per IDN.

- Fornisce servizi di risoluzione dei nomi DNS condivisi per i carichi di lavoro tenant
- Servizio DNS autorevole per la risoluzione dei nomi e la registrazione DNS nello spazio dei nomi del tenant
- Servizio DNS ricorsivo per la risoluzione dei nomi Internet dalle macchine virtuali tenant.
- Se lo si desidera, è possibile configurare l'hosting simultaneo di nomi di infrastruttura e tenant
- Una soluzione DNS conveniente: i tenant non devono distribuire la propria infrastruttura DNS
- Disponibilità elevata con l'integrazione di Active Directory, richiesta.

Oltre a queste funzionalità, se si è interessati a mantenere i server DNS integrati AD aperti a Internet, è possibile distribuire i server IDN dietro un altro resolver ricorsivo nella rete perimetrale.

Poiché IDN è un server centralizzato per tutte le query DNS, un CSP o un'azienda può implementare anche firewall DNS tenant, applicare filtri, rilevare attività dannose e controllare le transazioni in una posizione centrale

## <a name="idns-infrastructure"></a>Infrastruttura IDN
L'infrastruttura di IDN include i server IDN e il proxy IDN.

### <a name="idns-servers"></a>Server IDN
IDN include un set di server DNS che ospitano dati specifici del tenant, ad esempio i record di risorse DNS della macchina virtuale.

i server IDN sono i server autorevoli per le zone DNS interne e fungono anche da resolver per i nomi pubblici quando le macchine virtuali del tenant tentano di connettersi a risorse esterne.

Tutti i nomi host per le macchine virtuali nelle reti virtuali vengono archiviati come record di risorse DNS nella stessa zona. Se ad esempio si distribuisce IDN per una zona denominata Contoso. local, i record di risorse DNS per le macchine virtuali in tale rete vengono archiviati nella zona contoso. local.

I nomi di dominio completi della macchina virtuale tenant \(FQDN\) sono costituiti dal nome del computer e dalla stringa del suffisso DNS per la rete virtuale, in formato GUID. Ad esempio, se si dispone di una VM tenant denominata TENANT1 che si trova nella rete virtuale Contoso, local, il nome di dominio completo della macchina virtuale è TENANT1. *VN-GUID*. contoso. local, dove *VN-GUID* è la stringa del suffisso DNS per la rete virtuale.

>[!NOTE]
>Se si è un amministratore dell'infrastruttura, è possibile usare il CSP o l'infrastruttura DNS aziendale come server IDN anziché distribuire i nuovi server DNS in modo specifico per l'uso come server IDN. Se si distribuiscono nuovi server per IDN o si usa l'infrastruttura esistente, IDN si basa su Active Directory per garantire la disponibilità elevata. I server IDN devono pertanto essere integrati con Active Directory.

### <a name="idns-proxy"></a>Proxy IDN
il proxy IDN è un servizio Windows che viene eseguito in ogni host e che invia il traffico DNS della rete virtuale del tenant al server IDN.

La figura seguente illustra i percorsi di traffico DNS dalle reti virtuali tenant tramite il proxy IDN al server IDN e a Internet.

![Infrastruttura IDN](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>Come distribuire IDN
Quando si distribuisce SDN in Windows Server 2016 usando gli script, IDN viene incluso automaticamente nella distribuzione.

Per ulteriori informazioni, vedere gli argomenti seguenti.

- [Distribuire un'infrastruttura software defined Network usando gli script](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>Informazioni sui passaggi per la distribuzione di IDN
È possibile usare questa sezione per comprendere in che modo IDN viene installato e configurato quando si distribuisce SDN usando gli script.

Di seguito è riportato un riepilogo dei passaggi necessari per distribuire IDN.

>[!NOTE]
>Se SDN è stato distribuito usando gli script, non è necessario eseguire una di queste operazioni. La procedura viene fornita solo a scopo informativo e di risoluzione dei problemi.

### <a name="step-1-deploy-dns"></a>Passaggio 1: distribuire DNS
È possibile distribuire un server DNS usando il comando di Windows PowerShell di esempio seguente.
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>Passaggio 2: configurare le informazioni IDN nel controller di rete
Questo segmento di script è una chiamata REST effettuata dall'amministratore al controller di rete, per informarla sulla configurazione della zona IDN, ad esempio l'indirizzo IP del iDNSServer e la zona utilizzata per ospitare i nomi IDN. 

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
>Si tratta di un estratto dalla sezione **Configuration ConfigureIDns** in SDNExpress. ps1. Per altre informazioni, vedere [distribuire un'infrastruttura software defined Network usando gli script](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-3-configure-the-idns-proxy-service"></a>Passaggio 3: configurare il servizio proxy IDN
Il servizio proxy IDN viene eseguito in ogni host Hyper-V, fornendo il Bridge tra le reti virtuali dei tenant e la rete fisica in cui si trovano i server IDN. È necessario creare le seguenti chiavi del registro di sistema in ogni host Hyper-V.


**Porta DNS:** Porta fissa 53

- Chiave del registro di sistema = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- VALUENAME = "porta"
- ValueData = 53
- ValueType = "DWORD"
       

**Porta proxy DNS:** Porta fissa 53

- Chiave del registro di sistema = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- VALUENAME = "ProxyPort"
- ValueData = 53
- ValueType = "DWORD"
        
**IP DNS:** Indirizzo IP fisso configurato nell'interfaccia di rete, nel caso in cui il tenant scelga di usare il servizio IDN

- Chiave del registro di sistema = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- VALUENAME = "IP"
- ValueData = "169.254.169.254"
- ValueType = "stringa"

        
**Indirizzo Mac:** Indirizzo del controllo di accesso multimediale del server DNS

- Chiave del registro di sistema = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- VALUENAME = "MAC"
- ValueData = "AA-BB-CC-AA-BB-CC"
- ValueType = "stringa"

**Indirizzo del server IDN:** Elenco delimitato da virgole di server IDN.

- Chiave del registro di sistema: HKLM\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- VALUENAME = "server d'inoltri"
- ValueData = "10.0.0.9"
- ValueType = "stringa"



>[!NOTE]
>Si tratta di un estratto dalla sezione **Configuration ConfigureIDnsProxy** in SDNExpress. ps1. Per altre informazioni, vedere [distribuire un'infrastruttura software defined Network usando gli script](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>Passaggio 4: riavviare il servizio agente host del controller di rete
Per riavviare il servizio agente host del controller di rete, è possibile utilizzare il comando di Windows PowerShell seguente.
    
    Restart-Service nchostagent -Force
    
Per altre informazioni, vedere [Restart-Service](https://technet.microsoft.com/library/hh849823.aspx).

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>Abilitare le regole del firewall per il servizio proxy DNS
È possibile usare il comando di Windows PowerShell seguente per creare una regola del firewall che consenta alle eccezioni per la comunicazione del proxy con la macchina virtuale e il server IDN.
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

Per ulteriori informazioni, vedere [Enable-NetFirewallRule](https://technet.microsoft.com/library/jj554869.aspx).
    
### <a name="validate-the-idns-service"></a>Convalidare il servizio IDN
Per convalidare il servizio IDN, è necessario distribuire un carico di lavoro tenant di esempio.

Per altre informazioni, vedere [creare una macchina virtuale e connettersi a una rete virtuale tenant o a una VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).

Se si vuole che la macchina virtuale tenant usi il servizio IDN, è necessario lasciare vuota la configurazione del server DNS delle interfacce di rete VM e consentire alle interfacce di usare DHCP. 

Dopo l'avvio della macchina virtuale con un'interfaccia di rete di questo tipo, riceve automaticamente una configurazione che consente alla macchina virtuale di usare IDN e la macchina virtuale inizia immediatamente a eseguire la risoluzione dei nomi usando il servizio IDN.

Se si configura la macchina virtuale tenant per l'uso del servizio IDN lasciando vuoti il server DNS dell'interfaccia di rete e le informazioni del server DNS alternativo, il controller di rete fornisce la macchina virtuale con un indirizzo IP ed esegue una registrazione del nome DNS per conto della macchina virtuale con il server IDN . 

Il controller di rete informa anche il proxy IDN sulla macchina virtuale e i dettagli necessari per eseguire la risoluzione dei nomi per la macchina virtuale. 

Quando la macchina virtuale avvia una query DNS, il proxy funge da server d'inoltro della query dalla rete virtuale al servizio IDN. 

Il proxy DNS garantisce inoltre che le query della VM tenant siano isolate. Se il server IDN è autorevole per la query, il server IDN risponde con una risposta autorevole. Se il server IDN non è autorevole per la query, esegue una ricorsione DNS per risolvere i nomi Internet.

>[!NOTE]
>Queste informazioni sono incluse nella sezione **Configuration AttachToVirtualNetwork** in SDNExpressTenant. ps1. Per altre informazioni, vedere [distribuire un'infrastruttura software defined Network usando gli script](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

