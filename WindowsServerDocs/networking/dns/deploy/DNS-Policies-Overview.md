---
title: Informazioni generali sui criteri QoS
description: Questo argomento fa parte della Guida allo scenario dei criteri DNS per Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a6fe98dea50dd194c2bb2303a663968f93818332
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317959"
---
# <a name="dns-policies-overview"></a>Informazioni generali sui criteri QoS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per informazioni sui criteri DNS, che è una novità di Windows Server 2016. È possibile usare i criteri DNS per la gestione del traffico basata sulla posizione geografica, le risposte DNS intelligenti basate sull'ora del giorno, per gestire un singolo server DNS configurato per la distribuzione di Split\-Brain, l'applicazione di filtri alle query DNS e altro ancora. Gli elementi seguenti forniscono maggiori dettagli su queste funzionalità.

-   **Bilanciamento del carico dell'applicazione.** Quando sono state distribuite più istanze di un'applicazione in posizioni diverse, è possibile usare i criteri DNS per bilanciare il carico del traffico tra le diverse istanze dell'applicazione, allocando dinamicamente il carico del traffico per l'applicazione.

-   **Gestione del traffico basata sulla posizione geo\-geografica.** È possibile usare i criteri DNS per consentire ai server DNS primari e secondari di rispondere alle query del client DNS in base alla posizione geografica del client e alla risorsa a cui il client sta tentando di connettersi, fornendo al client l'indirizzo IP del più vicino risorse. 

-   **Split Brain DNS.** Con il DNS split\-Brain, i record DNS vengono suddivisi in ambiti di zona diversi nello stesso server DNS e i client DNS ricevono una risposta a seconda che i client siano client interni o esterni. È possibile configurare il DNS split\-Brain per Active Directory zone integrate o per le zone in server DNS autonomi.

-   **Filtro.** È possibile configurare i criteri DNS per creare filtri di query in base ai criteri forniti. I filtri query nei criteri DNS consentono di configurare il server DNS per rispondere in modo personalizzato in base alla query DNS e al client DNS che invia la query DNS. 
-   **Analisi.** È possibile usare i criteri DNS per reindirizzare i client DNS dannosi a un indirizzo IP non\-esistente anziché indirizzarli al computer che tentano di raggiungere.

-   **Reindirizzamento basato sull'ora del giorno.** È possibile usare i criteri DNS per distribuire il traffico delle applicazioni tra diverse istanze distribuite geograficamente di un'applicazione usando criteri DNS basati sull'ora del giorno.

## <a name="new-concepts"></a>Nuovi concetti  
Per creare criteri per supportare gli scenari elencati in precedenza, è necessario essere in grado di identificare gruppi di record in una zona, gruppi di client in una rete, tra gli altri elementi. Questi elementi sono rappresentati dai nuovi oggetti DNS seguenti:  

- **Subnet client:** un oggetto subnet client rappresenta una subnet IPv4 o IPv6 da cui vengono inviate le query a un server DNS. È possibile creare subnet per definire in seguito i criteri da applicare in base alla subnet da cui provengono le richieste. In uno scenario DNS split brain, ad esempio, la richiesta di risoluzione per un nome, ad esempio <em>www.Microsoft.com</em> , può ricevere una risposta con un indirizzo IP interno ai client da subnet interne e un indirizzo IP diverso ai client in subnet esterne.

- **Ambito ricorsione:** gli ambiti di ricorsione sono istanze univoche di un gruppo di impostazioni che controllano la ricorsione in un server DNS Un ambito di ricorsione contiene un elenco di server d'attesa e specifica se è abilitata la ricorsione. Un server DNS può avere molti ambiti di ricorsione. I criteri di ricorsione del server DNS consentono di scegliere un ambito di ricorsione per un set di query. Se il server DNS non è autorevole per determinate query, i criteri di ricorsione del server DNS consentono di controllare la modalità di risoluzione delle query. È possibile specificare quali server d'inoltri usare e se usare la ricorsione.

- **Ambiti di zona:** una zona DNS può avere più ambiti di zona, con ogni ambito di zona contenente il proprio set di record DNS. Lo stesso record può essere presente in più ambiti con indirizzi IP diversi. Inoltre, i trasferimenti di zona vengono eseguiti a livello di ambito della zona. Ciò significa che i record da un ambito di zona in una zona primaria verranno trasferiti nello stesso ambito di zona in una zona secondaria.

## <a name="types-of-policy"></a>Tipi di criteri

I criteri DNS sono divisi per livello e tipo. È possibile utilizzare i criteri di risoluzione delle query per definire la modalità di elaborazione delle query e i criteri di trasferimento di zona per definire il modo in cui si verificano i trasferimenti È possibile applicare ogni tipo di criteri a livello di server o di zona.

### <a name="query-resolution-policies"></a>Criteri di risoluzione delle query

È possibile usare i criteri di risoluzione delle query DNS per specificare in che modo le query di risoluzione in ingresso vengono gestite da un server DNS. Tutti i criteri di risoluzione delle query DNS contengono gli elementi seguenti:  

|Campo|Descrizione|Valori possibili|  
|---------|---------------|-------------------|  
|**Nome**|Nome criteri|-Fino a 256 caratteri<br />-Può contenere qualsiasi carattere valido per un nome file|  
|**Stato**|Stato dei criteri|-Enable (impostazione predefinita)<br />-Disabilitato|  
|**Livello**|Livello di criteri|-Server<br />-Zona|  
|**Ordine di elaborazione**|Quando una query viene classificata in base al livello e si applica a, il server trova il primo criterio per il quale la query corrisponde ai criteri e lo applica alla query|-Valore numerico<br />-Valore univoco per ogni criterio che contiene lo stesso livello e si applica al valore|  
|**Azione**|Azione che deve essere eseguita dal server DNS|-Allow (valore predefinito per il livello di zona)<br />-Deny (impostazione predefinita a livello di server)<br />-Ignora|  
|**Criteri**|Condizione dei criteri (e/o) e elenco di criteri da soddisfare per l'applicazione dei criteri|-Condition (operatore) (e/o)<br />-Elenco di criteri (vedere la tabella dei criteri di seguito)|  
|**Ambito**|Elenco degli ambiti di zona e dei valori ponderati per ambito. I valori ponderati vengono usati per la distribuzione del bilanciamento del carico. Se, ad esempio, l'elenco include Datacenter1 con un peso di 3 e Datacenter2 con un peso pari a 5, il server risponderà con un record di datacentre1 tre volte di otto richieste|-Elenco degli ambiti di zona (per nome) e pesi|  

> [!NOTE]
> I criteri a livello di server possono avere solo i valori **Deny** o **Ignore** come azione.

Il campo criteri dei criteri DNS è costituito da due elementi:


|              Name               |                                         Descrizione                                          |                                                                                                                               Valori di esempio                                                                                                                               |
|---------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        **Subnet client**        | Nome di una subnet del client predefinito. Utilizzato per verificare la subnet da cui la query è stata inviata. |                             -   **EQ, Spagna, Francia** -restituisce true se la subnet è identificata come Spagna o Francia<br />-   **ne, Canada, Messico** : viene risolto in true se la subnet del client è una subnet diversa da Canada e Messico                             |
|     **Protocollo di trasporto**      |        Il trasporto di protocollo utilizzato nella query. Le voci possibili sono **UDP** e **TCP**        |                                                                                                                    -   **EQ, TCP**<br />-   **EQ, UDP**                                                                                                                     |
|      **Protocollo Internet**      |        Protocollo di rete utilizzato nella query. Le voci possibili sono **IPv4** e **IPv6**        |                                                                                                                   -   **EQ, IPv4**<br />-   **EQ, IPv6**                                                                                                                    |
| **Indirizzo IP dell'interfaccia server** |                   Indirizzo IP per l'interfaccia di rete del server DNS in ingresso                   |                                                                                                              -   **EQ, 10.0.0.1**<br />-   **EQ, 192.168.1.1**                                                                                                              |
|            **FQDN**             |            FQDN del record nella query, con la possibilità di usare un carattere jolly            | -   **EQ, www. contoso. com** -restituisce true solo se la query sta tentando di risolvere il nome di dominio completo <em>www.contoso.com</em><br />-   **EQ,\*. contoso.com,\*. Woodgrove.com** -restituisce true se la query è per qualsiasi record che termina con *contoso.com***o***Woodgrove.com* |
|         **Tipo di query**          |                          Tipo di record sottoposto A query (A, SRV, TXT)                          |                                                  -   **EQ, txt, SRV** -restituisce true se la query richiede un record TXT **o** SRV<br />-   **EQ, MX** -restituisce true se la query richiede un record MX                                                   |
|         **Ora del giorno**         |                              Ora del giorno in cui viene ricevuta la query                               |                                                                    -   **EQ, 10:00-12:00, 22:00-23:00** -restituisce true se la query viene ricevuta tra le 10:00 e mezzogiorno **oppure** tra le 22.00 e le 23.00                                                                    |

Utilizzando la tabella precedente come punto di partenza, è possibile usare la tabella seguente per definire un criterio usato per trovare una corrispondenza tra le query per qualsiasi tipo di record, ma i record SRV nel dominio contoso.com provenienti da un client nella subnet 10.0.0.0/24 tramite TCP compreso tra 8 e 10 PM tramite l'interfaccia 10.0.0.3:  

|Name|Valore|  
|--------|---------|  
|Subnet client|EQ, 10.0.0.0/24|  
|Protocollo di trasporto|EQ, TCP|  
|Indirizzo IP dell'interfaccia server|EQ, 10.0.0.3|  
|FQDN|EQ, *. contoso. com|  
|Tipo di query|NE, SRV|  
|Ora del giorno|EQ, 20:00-22:00|  

È possibile creare più criteri di risoluzione delle query dello stesso livello, purché dispongano di un valore diverso per l'ordine di elaborazione. Quando sono disponibili più criteri, il server DNS elabora le query in ingresso nel modo seguente:  

![Elaborazione dei criteri DNS](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  

### <a name="recursion-policies"></a>Criteri di ricorsione  
I criteri di ricorsione sono un **tipo** speciale di criteri a livello di server. I criteri di ricorsione controllano come il server DNS esegue la ricorsione per una query. I criteri di ricorsione si applicano solo quando l'elaborazione della query raggiunge il percorso di È possibile scegliere il valore DENY o IGNOre per la ricorsione per un set di query. In alternativa, è possibile scegliere un set di server d'installazione per un set di query.  

È possibile usare i criteri di ricorsione per implementare una configurazione DNS "Split Brain". In questa configurazione, il server DNS esegue la ricorsione per un set di client per una query, mentre il server DNS non esegue la ricorsione per altri client per la query.  

I criteri di ricorsione contengono gli stessi elementi che contengono i normali criteri di risoluzione delle query DNS, insieme agli elementi della tabella seguente:  

|Name|Descrizione|  
|--------|---------------|  
|**Applica alla ricorsione**|Specifica che questi criteri devono essere utilizzati solo per la ricorsione.|  
|**Ambito di ricorsione**|Nome dell'ambito di ricorsione.|  

> [!NOTE]  
> I criteri di ricorsione possono essere creati solo a livello di server.  

### <a name="zone-transfer-policies"></a>Criteri di trasferimento di zona  
I criteri di trasferimento di zona controllano se un trasferimento di zona è consentito o meno dal server DNS. È possibile creare criteri per il trasferimento di zona a livello di server o di zona. I criteri a livello di server si applicano a ogni query di trasferimento di zona che si verifica nel server DNS. I criteri a livello di zona si applicano solo alle query su una zona ospitata nel server DNS. L'uso più comune per i criteri a livello di zona consiste nell'implementazione di elenchi bloccati o sicuri.  

> [!NOTE]  
> I criteri di trasferimento di zona possono usare solo le azioni DENY o IGNOre.  

È possibile usare i criteri di trasferimento di zona a livello di server riportati di seguito per negare un trasferimento di zona per il dominio contoso.com da una determinata subnet:  

```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfContosoToFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  

È possibile creare più criteri di trasferimento di zona dello stesso livello, purché dispongano di un valore diverso per l'ordine di elaborazione. Quando sono disponibili più criteri, il server DNS elabora le query in ingresso nel modo seguente:  

![Processo DNS per più criteri di trasferimento di zona](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  

## <a name="managing-dns-policies"></a>Gestione dei criteri DNS  
È possibile creare e gestire i criteri DNS usando PowerShell. Gli esempi seguenti passano attraverso diversi scenari di esempio che è possibile configurare tramite i criteri DNS:  

### <a name="traffic-management"></a>Gestione del traffico  
È possibile indirizzare il traffico in base a un nome di dominio completo a server diversi a seconda della posizione del client DNS. Nell'esempio seguente viene illustrato come creare criteri di gestione del traffico per indirizzare i clienti da una determinata subnet a un Data Center nordamericano e da un'altra subnet a un data center europeo.  

```  
Add-DnsServerClientSubnet -Name "NorthAmericaSubnet" -IPv4Subnet "172.21.33.0/24"  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "172.17.44.0/24"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "NorthAmericaZoneScope"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.17.97.97" -ZoneScope "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.21.21.21" -ZoneScope "NorthAmericaZoneScope"  
Add-DnsServerQueryResolutionPolicy -Name "NorthAmericaPolicy" -Action ALLOW -ClientSubnet "eq,NorthAmericaSubnet" -ZoneScope "NorthAmericaZoneScope,1" -ZoneName "Contoso.com"  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName contoso.com  
```  

Le prime due righe dello script creano oggetti subnet client per America del Nord e Europe. Le due righe successive creano un ambito di zona all'interno del dominio contoso.com, una per ogni area. Le due righe successive creano un record in ogni zona che associa ww.contoso.com a un indirizzo IP diverso, uno per l'Europa, un altro per America del Nord. Infine, le ultime righe dello script creano due criteri di risoluzione delle query DNS, uno da applicare alla subnet America del Nord, un altro alla subnet Europe.  

### <a name="block-queries-for-a-domain"></a>Blocca le query per un dominio  
È possibile usare i criteri di risoluzione delle query DNS per bloccare le query a un dominio. Nell'esempio seguente vengono bloccate tutte le query in treyresearch.net:  

```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  

### <a name="block-queries-from-a-subnet"></a>Bloccare le query da una subnet  
È anche possibile bloccare le query provenienti da una subnet specifica. Lo script seguente crea una subnet per 172.0.33.0/24 e quindi crea un criterio per ignorare tutte le query provenienti da tale subnet:  

```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  

### <a name="allow-recursion-for-internal-clients"></a>Consenti ricorsione per client interni  
È possibile controllare la ricorsione usando un criterio di risoluzione delle query DNS. L'esempio seguente può essere usato per abilitare la ricorsione per i client interni, disabilitando la ricorsione per i client esterni in uno scenario Split Brain.  

```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  

La prima riga nello script modifica l'ambito di ricorsione predefinito, denominato semplicemente "." (punto) per disabilitare la ricorsione. La seconda riga crea un ambito di ricorsione denominato *InternalClients* con ricorsione abilitata. La terza riga crea quindi un criterio per applicare il nuovo ambito di ricorsione create a tutte le query in arrivo tramite un'interfaccia server con 10.0.0.34 come indirizzo IP.  

### <a name="create-a-server-level-zone-transfer-policy"></a>Creare un criterio di trasferimento di zona a livello di server  
È possibile controllare il trasferimento di zona in un formato più granulare usando i criteri di trasferimento di zona DNS. Lo script di esempio seguente può essere usato per consentire i trasferimenti di zona per qualsiasi server in una determinata subnet:  

```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  

La prima riga nello script crea un oggetto subnet denominato *AllowedSubnet* con il blocco IP 172.21.33.0/24. La seconda riga crea un criterio di trasferimento di zona per consentire i trasferimenti di zona a qualsiasi server DNS nella subnet creata in precedenza.  

### <a name="create-a-zone-level-zone-transfer-policy"></a>Creare un criterio di trasferimento di zona a livello di zona  
È anche possibile creare criteri di trasferimento di zona a livello di zona. Nell'esempio seguente viene ignorata qualsiasi richiesta di trasferimento di zona per contoso.com in arrivo da un'interfaccia server con un indirizzo IP 10.0.0.33:  

```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "eq,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  

## <a name="dns-policy-scenarios"></a>Scenari di criteri DNS

Per informazioni su come usare i criteri DNS per scenari specifici, vedere gli argomenti seguenti in questa guida.

- [Usare i criteri DNS per la gestione del traffico basata sulla posizione geografica con i server primari](primary-geo-location.md)  
- [Usare i criteri DNS per la gestione del traffico basata sulla posizione geografica con distribuzioni primarie secondarie](primary-secondary-geo-location.md)  
- [Usare i criteri DNS per le risposte DNS intelligenti in base all'ora del giorno](dns-tod-intelligent.md)
- [Risposte DNS basate sull'ora del giorno con un server app cloud di Azure](dns-tod-azure-cloud-app-server.md)
- [Usare i criteri DNS per la distribuzione DNS split brain](split-brain-DNS-deployment.md)
- [Usare i criteri DNS per DNS split-brain in Active Directory](dns-sb-with-ad.md)
- [Usare i criteri DNS per l'applicazione di filtri alle query DNS](apply-filters-on-dns-queries.md)
- [Usare i criteri DNS per il bilanciamento del carico dell'applicazione](app-lb.md)
- [Usare i criteri DNS per il bilanciamento del carico dell'applicazione con la consapevolezza della posizione geografica](app-lb-geo.md)

## <a name="using-dns-policy-on-read-only-domain-controllers"></a>Uso dei criteri DNS nei controller di dominio di sola lettura

I criteri DNS sono compatibili con i controller di dominio di sola lettura. Si noti che è necessario riavviare il servizio server DNS per il caricamento dei nuovi criteri DNS nei controller di dominio di sola lettura. Questa operazione non è necessaria per i controller di dominio scrivibili.
