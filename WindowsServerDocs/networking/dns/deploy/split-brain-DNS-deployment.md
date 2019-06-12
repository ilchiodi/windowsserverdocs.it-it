---
title: Usare i criteri DNS per la distribuzione DNS "split brain"
description: Questo argomento fa parte del DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c74bb2ee2f1647716c8c38e392434a5b7f01805f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446388"
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>Usare i criteri DNS per divisione\-Brain distribuzione DNS

>Si applica a: Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come configurare criteri DNS in Windows Server&reg; 2016 per le distribuzioni DNS "split Brain", in cui sono presenti due versioni di una singola zona - uno per gli utenti interni nella rete intranet dell'organizzazione e uno per gli utenti esterni, chi sono in genere gli utenti su Internet.

>[!NOTE]
>Per informazioni su come usare i criteri DNS per la suddivisione\-zone DNS integrate cervello distribuzione DNS con Active Directory, vedere [usare i criteri DNS per DNS Split-Brain in Active Directory](dns-sb-with-ad.md).

In precedenza, questo scenario richiede che gli amministratori DNS mantenere due server DNS diversi, ogni che fornisce servizi per ogni set di utenti interni ed esterni. Se solo alcuni record all'interno della zona sono state suddivise\-se non o entrambe le istanze della zona (interna ed esterna) sono state delegate allo stesso dominio padre, questo è diventato un problema di gestione. 

Un altro scenario di configurazione per la distribuzione di "split Brain" è selettivo ricorsione controllo per la risoluzione dei nomi DNS. In alcuni casi, i server DNS aziendale sono previsti per eseguire la risoluzione ricorsiva tramite Internet per gli utenti interni, anche se sono anche deve fungere da server dei nomi autorevole per gli utenti esterni e bloccare la ricorsione per loro. 

In questo argomento sono incluse le sezioni seguenti.

- [Esempio di distribuzione di DNS "split Brain"](#bkmk_sbexample)
- [Esempio di controllo di ricorsione selettiva DNS](#bkmk_recursion)

## <a name="bkmk_sbexample"></a>Esempio di distribuzione di DNS "split Brain"
Ecco un esempio di come è possibile utilizzare criteri DNS per eseguire lo scenario descritto in precedenza di DNS "split Brain".

Questa sezione descrive gli argomenti seguenti:

- [Funzionamento della distribuzione di DNS "split Brain"](#bkmk_sbhow)
- [Come configurare la distribuzione DNS "split Brain"](#bkmk_sbconfigure)


Questo esempio Usa una società fittizia, Contoso, che mantiene un sito Web di crescita professionale in www.career.contoso.com.

Il sito dispone di due versioni, uno per gli utenti interni in cui sono disponibili annunci di lavoro interno. Questo sito interno è disponibile all'indirizzo IP locale 10.0.0.39. 

La seconda versione è la versione pubblica del sito stesso, disponibile all'indirizzo IP pubblico 65.55.39.10.

In assenza di criteri di DNS, l'amministratore è necessario per ospitare questi due zone su server DNS di Windows Server separati e gestirli separatamente. 

Usando i criteri DNS queste zone possono ora essere ospitate nello stesso server DNS.  

Nella figura seguente viene illustrato questo scenario.

![Distribuzione di DNS "split Brain"](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)  


## <a name="bkmk_sbhow"></a>Funzionamento della distribuzione di DNS "split Brain"

Quando il server DNS è configurato con i criteri necessari DNS, viene valutato ogni richiesta di risoluzione nome con i criteri nel server DNS.

Il server dell'interfaccia viene utilizzato in questo esempio come criterio di distinguere tra i client interni ed esterni.

Se l'interfaccia del server su cui viene ricevuta la query corrisponde a uno qualsiasi dei criteri, l'ambito delle zone associati viene utilizzato per rispondere alla query. 

Pertanto, nel nostro esempio, le query DNS per www.career.contoso.com ricevuti sull'indirizzo IP privato (10.0.0.56) ricevano una risposta DNS contenente un indirizzo IP interno. e le query DNS che vengono ricevute nell'interfaccia di rete pubblica ricevano una risposta DNS che contiene l'indirizzo IP pubblico nell'ambito di zona predefinito (questo è quello utilizzato per la risoluzione di query normali).  

## <a name="bkmk_sbconfigure"></a>Come configurare la distribuzione DNS "split Brain"
Per configurare la distribuzione DNS Split-Brain tramite criteri di DNS, è necessario utilizzare la procedura seguente.

- [Creare gli ambiti di zona](#bkmk_zscopes)  
- [Aggiungere i record per gli ambiti di zona](#bkmk_records)  
- [Creare i criteri DNS](#bkmk_policies)

Le sezioni seguenti forniscono le istruzioni di configurazione dettagliate.

>[!IMPORTANT]
>Nelle sezioni seguenti includono esempi di comandi Windows PowerShell che contengono valori di esempio per numero di parametri. Assicurarsi di sostituire i valori di esempio in questi comandi con i valori appropriati per la distribuzione prima di eseguire questi comandi. 

### <a name="bkmk_zscopes"></a>Creare gli ambiti di zona

Un ambito di una zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona che contiene un proprio set di record DNS. Lo stesso record possono essere presenti in più ambiti, con diversi indirizzi IP o gli stessi indirizzi IP. 

> [!NOTE]
> Per impostazione predefinita, un ambito di una zona esistente nelle zone DNS. In questo ambito di zona ha lo stesso nome della zona e operazioni DNS legacy funzionano in questo ambito. In questo ambito di zona predefinito ospiterà la versione di www.career.contoso.com esterna.

È possibile usare il comando seguente per partizionare l'ambito di zona contoso.com per creare un ambito di zona interno. L'ambito di zona interno verrà utilizzato per mantenere la versione interna del www.career.contoso.com.

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

Per altre informazioni, vedere [Aggiungi DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records"></a>Aggiungere i record per gli ambiti di zona

Il passaggio successivo consiste nell'aggiungere i record che rappresenta l'host del server Web in ambiti due zone - interni e predefinito (per i client esterni). 

Nell'ambito di zona interno, il record <strong>www.career.contoso.com</strong> viene aggiunto con l'indirizzo IP 10.0.0.39, ovvero un indirizzo IP privato; e nell'ambito di zona predefinito lo stesso record <strong>www.career.contoso.com</strong>, è aggiunta con l'indirizzo IP 65.55.39.10.

No **– ZoneScope** nei comandi di esempio seguente viene fornito parametro quando il record viene aggiunto all'ambito di zona predefinito. È simile all'aggiunta di record a una zona "vanilla".

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="bkmk_policies"></a>Creare i criteri DNS

Dopo aver identificato le interfacce di server per la rete esterna e la rete interna e siano stati creati gli ambiti di zona, è necessario creare criteri DNS che si connettono gli ambiti di zona interni ed esterni.

>[!NOTE]
>Questo esempio Usa l'interfaccia del server come criterio per distinguere tra i client interni ed esterni. Un altro metodo per distinguere tra i client interni ed esterni consiste nell'usare una subnet del client come criterio. Se è possibile identificare la subnet a cui appartengono i client interni, è possibile configurare criteri DNS per distinguere in base alle subnet del client. Per informazioni su come configurare Gestione traffico usando criteri di subnet client, vedere [usare i criteri DNS per la gestione del traffico basato su posizione geografica con i server primari](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers).

Quando il server DNS riceve una query sull'interfaccia privata, la risposta alla query DNS viene restituita dall'ambito di zona interno.

>[!NOTE]
>Criteri non sono necessari per il mapping tra l'ambito di orario predefinito. 

Nell'esempio di comando seguente, 10.0.0.56 è l'indirizzo IP nell'interfaccia di rete privata, come illustrato nella figura precedente.

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  


## <a name="bkmk_recursion"></a>Esempio di controllo di ricorsione selettiva DNS

Ecco un esempio di come è possibile utilizzare criteri DNS per eseguire lo scenario descritto in precedenza del controllo di ricorsione selettiva DNS.

Questa sezione descrive gli argomenti seguenti:

- [Il funzionamento del controllo DNS ricorsione selettiva](#bkmk_recursionhow)
- [Come configurare controllo ricorsione selettiva DNS](#bkmk_recursionconfigure)

Questo esempio viene usato dalla società fittizia stesso dell'esempio precedente, Contoso, che mantiene un sito Web di crescita professionale in www.career.contoso.com.

Nell'esempio DNS "split Brain" distribuzione, lo stesso server DNS risponde ai client esterni e interni e offrire risposte diverse. 

Alcune distribuzioni di DNS potrebbero richiedere lo stesso server DNS per eseguire la risoluzione dei nomi ricorsiva per i client interni oltre a funge da server dei nomi autorevole per i client esterni. Questa circostanza è chiamata controllo ricorsione selettiva DNS.

Nelle versioni precedenti di Windows Server, l'abilitazione di ricorsione significava che è stato abilitato nell'intero server per tutte le zone DNS. Poiché il server DNS è inoltre in ascolto per le query esterne, la ricorsione è abilitata per i client interni ed esterni, rendendo un resolver di aprire il server DNS. 

Un server DNS è configurato come un resolver di aprire potrebbe essere vulnerabile a esaurimento delle risorse e può essere sfruttato da un client non autorizzati per creare attacchi di tipo reflection. 

Per questo motivo, gli amministratori DNS Contoso non si desidera che il server DNS per contoso.com eseguire la risoluzione dei nomi ricorsiva per i client esterni. È necessario solo per il controllo di ricorsione per i client interni, anche se il controllo di ricorsione può essere bloccato per i client esterni. 

Nella figura seguente viene illustrato questo scenario.

![Controllo di ricorsione selettiva](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg) 


### <a name="bkmk_recursionhow"></a>Il funzionamento del controllo DNS ricorsione selettiva

Se viene ricevuta una query per il quale il server DNS di Contoso è non autorevoli, ad esempio per www.microsoft.com, quindi la richiesta di risoluzione del nome viene valutata con i criteri nel server DNS. 

Poiché queste query non rientrano in qualsiasi zona, la zona il livello di criteri \(come definito nell'esempio di "split Brain"\) non vengono valutate. 

Il server DNS valuta i criteri di ricorsione e le query che vengono ricevute sulla corrispondenza interfaccia privata di **SplitBrainRecursionPolicy**. Questo criterio che punta a un ambito di ricorsione in cui è abilitata la ricorsione.

Quindi, il server DNS esegue la ricorsione per ottenere la risposta per www.microsoft.com da Internet e memorizza nella cache la risposta in locale. 

Se la query è stata ricevuta in interfaccia esterna non corrispondenza di criteri DNS e l'impostazione di ricorsione predefinita - che in questo caso è **disabilitato** -viene applicato.

Ciò impedisce che il server che agisce come un sistema di risoluzione aperta per i client esterni, mentre viene utilizzato come un resolver di memorizzazione nella cache per i client interni. 

### <a name="bkmk_recursionconfigure"></a>Come configurare controllo ricorsione selettiva DNS

Per configurare DNS ricorsione selettiva controllo tramite criteri di DNS, è necessario utilizzare la procedura seguente.

- [Creare ambiti di ricorsione DNS](#bkmk_recscopes)
- [Creare criteri di ricorsione DNS](#bkmk_recpolicy)

#### <a name="bkmk_recscopes"></a>Creare ambiti di ricorsione DNS

Gli ambiti di ricorsione sono istanze univoche di un gruppo di impostazioni che controllano la ricorsione in un server DNS. Un ambito di ricorsione contiene un elenco di server d'inoltro e specifica se è abilitata la ricorsione. Un server DNS può avere molti ambiti di ricorsione. 

L'impostazione di ricorsione legacy e un elenco di server d'inoltro sono detti l'ambito di ricorsione predefinito. Non è possibile aggiungere o rimuovere l'ambito di ricorsione predefinito, identificato da un punto di nome \("." \).

In questo esempio, l'impostazione di ricorsione predefinita è disabilitato, mentre viene creato un nuovo ambito di ricorsione per i client interni in cui è abilitata la ricorsione.

    
    Set-DnsServerRecursionScope -Name . -EnableRecursion $False
    Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True 
    

Per altre informazioni, vedere [Add-DnsServerRecursionScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverrecursionscope?view=win10-ps)

#### <a name="bkmk_recpolicy"></a>Creare criteri di ricorsione DNS

Creare i criteri di ricorsione per scegliere un ambito di ricorsione per un set di query che corrispondono a criteri specifici server DNS. 

Se il server DNS non autorevole per alcune query, i criteri di ricorsione server DNS consentono di controllare il modo risolvere le query. 

In questo esempio, l'ambito di ricorsione interna con abilitata la ricorsione è associato all'interfaccia di rete privata.

È possibile usare il comando seguente per configurare i criteri di ricorsione DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP "EQ,10.0.0.39"
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Ora il server DNS è configurato con i criteri necessari DNS per un server di nome "split Brain" o un server DNS con il controllo di ricorsione selettiva abilitato per i client interni.

È possibile creare migliaia di criteri DNS in base del traffico di gestione e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, per le query in ingresso. 

Per altre informazioni, vedere [Guida agli scenari di criteri DNS](DNS-Policy-Scenario-Guide.md).
