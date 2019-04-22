---
title: Informazioni generali sui criteri QoS
description: Questo argomento fa parte del DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ad8fa904f43bb51871e5063eaddedd0762d54d95
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814302"
---
# <a name="dns-policies-overview"></a>Informazioni generali sui criteri QoS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sui criteri DNS, che rappresenta una novità in Windows Server 2016. È possibile utilizzare criteri DNS per posizione geografica basato su gestione traffico, risposte DNS intelligente basate sull'ora del giorno, per gestire un singolo server DNS configurato per la suddivisione\-distribuzione cervello, applicazione di filtri alle query DNS e altro ancora. Gli elementi seguenti forniscono altri dettagli su queste funzionalità.

-   **Bilanciamento del carico dell'applicazione.** Quando si distribuiscono più istanze di un'applicazione in posizioni diverse, è possibile utilizzare criteri DNS per bilanciare il carico del traffico tra le istanze di applicazione diversi, allocare dinamicamente il carico del traffico per l'applicazione.

-   **Geografica\-Location Based di gestione traffico.** È possibile utilizzare criteri DNS per consentire ai server DNS primario e secondario rispondere alle query client DNS in base alla posizione geografica del client e la risorsa a cui il client sta provando a connettersi, fornendo il client con l'indirizzo IP di quello più vicino risorsa. 

-   **Dividere cervello DNS.** Divisione\-brain DNS, i record DNS vengono suddivise in diversi ambiti di zona nello stesso server DNS e i client DNS ricevano una risposta basata sul fatto che i client trovano i client interni o esterni. È possibile configurare la suddivisione\-brain DNS per le zone integrate con Active Directory o per le zone sui server DNS autonomi.

-   **Filtering.** È possibile configurare criteri DNS per creare filtri per query sono basati su criteri che viene fornito. Filtri di query in Criteri di DNS consentono di configurare il server DNS per rispondere in modo personalizzato in base alla query DNS e client DNS che invia la query DNS. 
-   **Analisi scientifiche.** È possibile utilizzare criteri DNS per reindirizzare i client DNS non autorizzati a non\-indirizzo IP esistente anziché indirizzare li al computer che sta provando a raggiungere.

-   **Ora del giorno in base reindirizzamento.** È possibile utilizzare criteri DNS per distribuire il traffico dell'applicazione tra diverse istanze distribuite geograficamente di un'applicazione usando i criteri DNS che si basano sull'ora del giorno.

## <a name="new-concepts"></a>Nuovi concetti  
Per creare i criteri per supportare gli scenari elencati in precedenza, è necessario essere in grado di identificare i gruppi di record in una zona, gruppi di client in una rete, tra gli altri elementi. Questi elementi sono rappresentati dai nuovi oggetti DNS seguenti:  

- **Subnet del client:** un oggetto della subnet client rappresenta una subnet IPv4 o IPv6 da cui le query vengono inviate a un server DNS. È possibile creare le subnet per definire in seguito i criteri da applicare in base alle quali subnet provengono le richieste. Ad esempio, in un scenario di split brain DNS, la richiesta per la risoluzione per un nome, ad esempio *www.microsoft.com* è possibile rispondere con un indirizzo IP interno per i client dalla subnet interne e un indirizzo IP diverso per i client esterni subnet.

- **Ambito di ricorsione:** gli ambiti di ricorsione sono istanze univoche di un gruppo di impostazioni che controllano la ricorsione in un server DNS. Un ambito di ricorsione contiene un elenco di server d'inoltro e specifica se è abilitata la ricorsione. Un server DNS può avere molti ambiti di ricorsione. Criteri di ricorsione dei server DNS consentono di scegliere un ambito di ricorsione per un set di query. Se il server DNS non autorevole per determinate query, i criteri di ricorsione server DNS consentono di controllare come risolvere le query. È possibile specificare quali server d'inoltro da utilizzare e se usare la ricorsione.

- **Gli ambiti di zona:** una zona DNS può avere più ambiti di zona, con ogni ambito di zona che contiene il proprio set di record DNS. Lo stesso record possono essere presenti in più ambiti, con indirizzi IP diversi. Inoltre, i trasferimenti di zona vengono eseguiti a livello di ambito di zona. Ciò significa che i record da un ambito di zona in zona primaria verranno trasferiti allo stesso ambito di zona in un'area secondaria.

## <a name="types-of-policy"></a>Tipi di criteri

I criteri di DNS sono suddivisi per livello e tipo. È possibile usare i criteri di risoluzione di Query per definire la modalità di elaborazione delle query e i criteri di trasferimento di zona per definire la modalità in cui si verificano trasferimenti di zona. È possibile applicare ogni tipo di criteri a livello di server o il livello di zona.
  
### <a name="query-resolution-policies"></a>Criteri di risoluzione di query

È possibile utilizzare criteri di risoluzione di Query DNS per specificare la risoluzione in ingresso come le query vengono gestite da un server DNS. Ogni criterio di risoluzione di Query DNS contiene gli elementi seguenti:  
  
|Campo|Descrizione|Valori possibili|  
|---------|---------------|-------------------|  
|**Name**|Nome criterio|-Fino a 256 caratteri<br />-Può contenere qualsiasi carattere valido per un nome di file|  
|**Stato**|Stato dei criteri|-Abilitare (valore predefinito)<br />-Disabilitato|  
|**Livello**|Livello di criteri|-   Server<br />-Zona|  
|**Ordine di elaborazione**|Una volta che viene classificata dal livello e si applica a una query, il server rileva il primo criterio per il quale la query corrisponde ai criteri e li applica per eseguire una query|: Valore numerico<br />: Valore univoco per ogni criterio che contiene lo stesso livello e si applica al valore|  
|**Azione**|Azione da eseguire dal server DNS|-Consentire (impostazione predefinita per il livello di zona)<br />-Deny (impostazione predefinita nel livello di server)<br />-Ignorare|  
|**Criteri**|Condizione dei criteri (e/o) e un elenco di criteri che devono essere soddisfatti per l'applicazione dei criteri|-Operatore condition (e/o)<br />-Elenco dei criteri (vedere la tabella di criterio seguente)|  
|**Ambito**|Elenco di ambiti di zona e valori ponderati per ogni ambito. Valori ponderati vengono usati per la distribuzione di bilanciamento del carico. Ad esempio, se questo elenco include datacenter1 con un peso pari a 3 e datacenter2 con un peso pari a 5 server risponderà con un record da datacentre1 tre volte all'esterno di otto richieste|-Elenco di ambiti di zona (individuato tramite nome) e pesi|  

> [!NOTE]
> I criteri a livello di server possono avere solo i valori **Deny** oppure **Ignore** come azione.

Il campo ai criteri DNS dei criteri è composto da due elementi:

|Nome|Descrizione|Valori di esempio|
|--------|---------------|-----------------|
|**Subnet del client**|Nome di una subnet del client predefinito. Utilizzato per verificare la subnet da cui la query è stata inviata.|-   **EQ, Spagna, Francia** -risolve su true se la subnet viene identificata come Spagna o Francia<br />-   **NE, Canada, Messico** -risolve su true se la subnet del client è qualsiasi subnet diverso da in Canada e Messico|  
|**Protocollo di trasporto**|Il trasporto di protocollo utilizzato nella query. Sono possibili voci **UDP** e **TCP**|-   **EQ,TCP**<br />-   **EQ,UDP**|  
|**Internet Protocol**|Protocollo di rete utilizzato nella query. Sono possibili voci **IPv4** e **IPv6**|-   **EQ,IPv4**<br />-   **EQ,IPv6**|  
|**Indirizzo IP dell'interfaccia del server**|Indirizzo IP per l'interfaccia di rete di server DNS in ingresso|-   **EQ,10.0.0.1**<br />-   **EQ,192.168.1.1**|  
|**FQDN**|Nome di dominio completo del record nella query, con la possibilità di usare un carattere jolly|-   **EQ, www.contoso.com** -risoluzione: messaggi accodati rue solo se la query sta tentando di risolvere le *www.contoso.com* FQDN<br />-   **EQ,\*. contoso.com\*. woodgrove.com** -risolve su true se la query è per tutti i record che terminano con *contoso.com***o***woodgrove.com*|  
|**Tipo di query**|Tipo di record sottoposto a query (A, SVR, TXT)|-   **EQ, TXT, SRV** -viene risolto in true se la query sta richiedendo un TXT **o** record SRV<br />-   **EQ, MX** -risolve su true se la query sta richiedendo un record MX|  
|**Ora del giorno**|Ora del giorno che viene ricevuta la query|-   **EQ, 10.00-12:00: 22:00-23.00** -viene risolto in true se la query non viene ricevuta tra mezzogiorno e alle 10 **o** tra PM di 10 e 11 PM|  
  
Utilizzando la tabella sopra come punto di partenza, nella tabella seguente può essere usata per definire un criterio che consente di far corrispondere le query per qualsiasi tipo di record, ma i record SRV nel dominio contoso.com provenienti da un client nella subnet 10.0.0.0/24 tramite TCP compreso tra 8 e 10 PM attraverso i interfaccia 10.0.0.3:  
  
|Nome|Value|  
|--------|---------|  
|Subnet del client|EQ,10.0.0.0/24|  
|Protocollo di trasporto|EQ, TCP|  
|Indirizzo IP dell'interfaccia del server|EQ,10.0.0.3|  
|FQDN|EQ,*.contoso.com|  
|Tipo di query|NE, SRV|  
|Ora del giorno|EQ,20:00-22:00|  
  
È possibile creare query più criteri di risoluzione dello stesso livello, come purché abbiano un valore diverso per l'ordine di elaborazione. Quando sono disponibili più criteri, il server DNS elabora le query in ingresso nel modo seguente:  
  
![Elaborazione dei criteri DNS](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  
  
### <a name="recursion-policies"></a>Criteri di ricorsione  
I criteri di ricorsione sono uno speciale **tipo** dei criteri a livello di server. I criteri di ricorsione controllano la modalità con cui il server DNS esegue la ricorsione per una query. I criteri di ricorsione si applicano solo quando l'elaborazione delle query raggiunge il percorso di ricorsione. È possibile scegliere un valore di nega o IGNORE provést kontrolu rekurze per un set di query. In alternativa, è possibile scegliere un set di server d'inoltro per un set di query.  
  
È possibile usare i criteri di ricorsione per implementare una configurazione DNS Split-brain. In questa configurazione, il server DNS esegue la ricorsione per un set di client per una query, mentre il server DNS non esegue la ricorsione per altri client per tale query.  
  
I criteri di ricorsione contiene gli stessi elementi contiene un criterio di risoluzione di query DNS regolare, insieme a elementi nella tabella seguente:  
  
|Nome|Descrizione|  
|--------|---------------|  
|**Applicare la ricorsione**|Specifica che questo criterio deve essere utilizzato solo per la ricorsione.|  
|**Ambito di ricorsione**|Nome dell'ambito di ricorsione.|  
  
> [!NOTE]  
> I criteri di ricorsione possono essere creati solo a livello di server.  
  
### <a name="zone-transfer-policies"></a>Criteri di trasferimento zona  
I criteri di trasferimento di zona controllano se un trasferimento di zona è consentito o meno, il server DNS. È possibile creare criteri per il trasferimento di zona a livello di server oppure a livello di zona. I criteri a livello di server si applicano a ogni query di trasferimento di zona che si verifica nel server DNS. I criteri a livello di zona si applicano solo alle query in una zona ospitata nel server DNS. L'uso più comune per i criteri a livello di zona consiste nell'implementare elenchi provvisoria o bloccati.  
  
> [!NOTE]  
> I criteri di trasferimento di zona possono usare solo DENY o IGNORE come azioni.  
  
È possibile usare i seguenti di criteri di trasferimento zona a livello server per negare un trasferimento di zona per il dominio contoso.com da una determinata subnet:  
  
```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfContosoToFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  
  
È possibile creare il trasferimento di zona più criteri allo stesso livello, come purché abbiano un valore diverso per l'ordine di elaborazione. Quando sono disponibili più criteri, il server DNS elabora le query in ingresso nel modo seguente:  
  
![Processo di DNS per più criteri di trasferimento zona](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  
  
## <a name="managing-dns-policies"></a>Gestione dei criteri DNS  
È possibile creare e gestire i criteri di DNS con PowerShell. Gli esempi seguenti passare attraverso scenari di esempio diversi che è possibile configurare tramite i criteri DNS:  
  
### <a name="traffic-management"></a>Gestione traffico  
È possibile indirizzare il traffico in base a un nome di dominio completo in server diversi a seconda della posizione del client DNS. L'esempio seguente viene illustrato come creare i criteri di gestione per indirizzare i clienti da una determinata subnet in un datacenter nordamericano e da un'altra subnet per un Data Center dell'Europa il traffico.  
  
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
  
Le prime due righe dello script di creano client oggetti di subnet per America del Nord ed Europa. Le due righe in seguito creano un ambito di zona nel dominio contoso.com, uno per ogni area. Le due righe in seguito creano un record in ogni zona che associa ww.contoso.com per indirizzo IP diverso, uno per l'Europa, un altro per l'America del Nord. Infine, le ultime righe dello script di creano due i criteri di risoluzione, Query DNS uno da applicare alla subnet America del Nord, un altro alla subnet Europa.  
  
### <a name="block-queries-for-a-domain"></a>Query di blocco per un dominio  
È possibile usare un criterio di risoluzione di Query DNS alle query di blocco a un dominio. L'esempio seguente consente di bloccare tutte le query treyresearch.net:  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  
  
### <a name="block-queries-from-a-subnet"></a>Query di blocco da una subnet  
È inoltre possibile bloccare le query provenienti da una subnet specifica. Lo script seguente crea una subnet per 172.0.33.0/24 e quindi crea un criterio per ignorare tutte le query provenienti da tale subnet:  
  
```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  
  
### <a name="allow-recursion-for-internal-clients"></a>Consenti la ricorsione per i client interni  
È possibile controllare la ricorsione tramite un criterio di risoluzione di Query DNS. L'esempio seguente è utilizzabile per abilitare la ricorsione per i client interni, durante la sua disabilitazione per i client esterni in un scenario di split brain.  
  
```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  
  
La prima riga nello script viene modificato l'ambito di ricorsione predefinito, denominato semplicemente come "." (punto) per disabilitare la ricorsione. La seconda riga crea un ambito di ricorsione denominato *InternalClients* con abilitata la ricorsione. E la terza riga crea un criterio per applicare l'appena creato con ambito di ricorsione per tutte le query che passano attraverso un'interfaccia di server che ha 10.0.0.34 come indirizzo IP.  
  
### <a name="create-a-server-level-zone-transfer-policy"></a>Creare un criterio di trasferimento di zona a livello di server  
È possibile controllare il trasferimento di zona in un formato più granulare usando i criteri di trasferimento di zona DNS. Lo script di esempio riportato di seguito è utilizzabile per consentire i trasferimenti di zona per qualsiasi server in una determinata subnet:  
  
```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  
  
La prima riga nello script crea un oggetto di subnet denominato *AllowedSubnet* con l'indirizzo IP bloccato 172.21.33.0/24. La seconda riga crea un criterio di trasferimento di zona per consentire i trasferimenti di zona a qualsiasi server DNS nella subnet creata in precedenza.  
  
### <a name="create-a-zone-level-zone-transfer-policy"></a>Creare un criterio di trasferimento di zona a livello di zona  
È anche possibile creare criteri di trasferimento di zona a livello di zona. L'esempio seguente ignora qualsiasi richiesta per un trasferimento di zona per contoso.com provenienti da un'interfaccia di server che abbia un indirizzo IP di 10.0.0.33:  
  
```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "ne,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  
  
## <a name="dns-policy-scenarios"></a>Scenari relativi ai criteri DNS

Per informazioni su come usare i criteri DNS per scenari specifici, vedere gli argomenti seguenti in questa Guida.

- [Usare i criteri DNS per posizione geografica basato su gestione traffico con i server primari](primary-geo-location.md)  
- [Usare i criteri DNS per posizione geografica basato su gestione traffico con distribuzioni primario secondario](primary-secondary-geo-location.md)  
- [Usare i criteri DNS per risposte DNS intelligente basate sull'ora del giorno](dns-tod-intelligent.md)
- [Server di App Cloud risposte DNS basate sull'ora del giorno con un'istanza di Azure](dns-tod-azure-cloud-app-server.md)
- [Usare i criteri DNS per la distribuzione DNS "split Brain"](split-brain-DNS-deployment.md)
- [Usare i criteri DNS per DNS "split Brain" in Active Directory](dns-sb-with-ad.md)
- [Usare i criteri DNS per l'applicazione di filtri alle query DNS](apply-filters-on-dns-queries.md)
- [Usare i criteri DNS per bilanciamento del carico dell'applicazione](app-lb.md)
- [Usare i criteri DNS per applicazione bilanciamento del carico con la consapevolezza della posizione geografica](app-lb-geo.md)

## <a name="using-dns-policy-on-read-only-domain-controllers"></a>Utilizzando criteri DNS nei controller di dominio di sola lettura

Criteri DNS è compatibili con i controller di dominio di sola lettura. Si noti che il riavvio del servizio Server DNS è obbligatorio per i nuovi criteri di DNS devono essere caricati nei controller di dominio di sola lettura. Non è necessario nei controller di dominio scrivibile.
