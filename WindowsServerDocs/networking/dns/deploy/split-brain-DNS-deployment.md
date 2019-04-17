---
title: Usare i criteri DNS per la distribuzione DNS "split Brain"
description: In questo argomento fa parte di DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b25ac752ea347f4d184628eb26bc7e297443306
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>Usare i criteri DNS per la distribuzione DNS "split Brain"

>Si applica a: Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come configurare criteri DNS in Windows Server&reg; 2016 per le distribuzioni DNS "split Brain", in cui sono presenti due versioni di una singola zona - uno per gli utenti interni nella rete intranet dell'organizzazione e uno per gli utenti esterni, che sono in genere gli utenti su Internet.

>[!NOTE]
>Per informazioni su come usare i criteri DNS per la distribuzione DNS "split Brain" con Active Directory zone DNS integrate, vedere [utilizzare criteri DNS per DNS Split-Brain in Active Directory](dns-sb-with-ad.md).

In precedenza, questo scenario è necessario che gli amministratori DNS mantenere due diversi server DNS, ogni fornire servizi a ogni gruppo di utenti, interni ed esterni. Se solo alcuni record all'interno della zona sono stati se non Split o entrambe le istanze della zona (interna ed esterna) sono state delegate allo stesso dominio padre, questo è diventato un problema di gestione. 

Un altro scenario di configurazione per la distribuzione "split Brain" è selettiva ricorsione controllo per la risoluzione dei nomi DNS. In alcuni casi, i server DNS aziendale dovrebbero eseguire risoluzione ricorsiva tramite Internet per gli utenti interni, mentre anche deve fungere da server dei nomi autorevole per gli utenti esterni e bloccare la ricorsione per loro. 

In questo argomento contiene le sezioni seguenti.

- [Esempio di distribuzione di DNS "split Brain"](#bkmk_sbexample)
- [Esempio di controllo di ricorsione selettiva DNS](#bkmk_recursion)

##<a name="bkmk_sbexample"></a>Esempio di distribuzione di DNS "split Brain"
Ecco un esempio di come è possibile utilizzare criteri DNS per eseguire lo scenario descritto in precedenza di DNS "split Brain".

Questa sezione contiene gli argomenti seguenti.

- [Funzionamento della distribuzione di DNS "split Brain"](#bkmk_sbhow)
- [Come configurare la distribuzione DNS "split Brain"](#bkmk_sbconfigure)


Questo esempio Usa una società fittizia, Contoso, che gestisce un sito Web di carriera in www.career.contoso.com.

Il sito dispone di due versioni, uno per gli utenti interni in cui sono disponibili le registrazioni processo interno. Questo sito interno è disponibile all'indirizzo IP locale 10.0.0.39. 

La seconda versione è la versione pubblica dello stesso sito, disponibile all'indirizzo IP pubblico 65.55.39.10.

In assenza di criteri DNS, l'amministratore è necessario per ospitare queste due zone in server DNS di Windows Server distinti e gestirli separatamente. 

Utilizzo dei criteri DNS queste aree possono essere ospitate ora nello stesso server DNS.  

Nella figura seguente viene illustrato questo scenario.

![Distribuzione di DNS "split Brain"](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)  


##<a name="bkmk_sbhow"></a>Funzionamento della distribuzione di DNS "split Brain"

Quando il server DNS è configurato con i criteri necessari DNS, ogni richiesta di risoluzione dei nomi viene valutata rispetto ai criteri sul server DNS.

L'interfaccia server viene utilizzato in questo esempio come i criteri per distinguere i client interni ed esterni.

Se l'interfaccia del server su cui viene ricevuta la query corrisponde a uno qualsiasi dei criteri, l'ambito di zona associato viene utilizzato per rispondere alla query. 

Quindi, nel nostro esempio, le query DNS per www.career.contoso.com ricevuti IP privato (10.0.0.56) ricevano una risposta DNS che contiene un indirizzo IP interno. e le query DNS che vengono ricevute nell'interfaccia di rete pubblica ricevano una risposta DNS che contiene l'indirizzo IP pubblico nell'ambito di zona predefinito (si tratta dello stesso come la risoluzione di query normale).  

##<a name="bkmk_sbconfigure"></a>Come configurare la distribuzione DNS "split Brain"
Per configurare DNS Split-Brain distribuzione tramite criteri di DNS, è necessario utilizzare la procedura seguente.

- [Creare gli ambiti di zona](#bkmk_zscopes)  
- [Aggiungere record per gli ambiti di zona](#bkmk_records)  
- [Creare i criteri DNS](#bkmk_policies)

Le sezioni seguenti forniscono istruzioni dettagliate di configurazione.

>[!IMPORTANT]
>Le sezioni seguenti includono esempi di comandi Windows PowerShell che contengono i valori di esempio per numero di parametri. Assicurarsi di sostituire i valori di esempio in questi comandi con i valori appropriati per la distribuzione prima di eseguire questi comandi. 

###<a name="bkmk_zscopes"></a>Creare gli ambiti di zona

Un ambito di zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona che contiene un proprio set di record DNS. Lo stesso record possono essere presenti in più ambiti, con indirizzi IP diversi o gli stessi indirizzi IP. 

>[!NOTE]
>Per impostazione predefinita, un ambito di una zona esistente nelle zone DNS. Questo ambito di zona con lo stesso nome della zona e operazioni DNS legacy funzionano in questo ambito. Questo ambito di zona predefinito ospiterà la versione di www.career.contoso.com esterna.

È possibile utilizzare il comando seguente per l'ambito di zona contoso.com per creare un ambito di zona interna di partizione. L'ambito di zona interno verrà utilizzato per mantenere la versione interna di www.career.contoso.com.

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

Per ulteriori informazioni, vedere [Aggiungi DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

###<a name="bkmk_records"></a>Aggiungere record per gli ambiti di zona

Il passaggio successivo consiste per aggiungere i record che rappresenta l'host del server Web in ambiti due zone - interni e l'impostazione predefinita (per i client esterni). 

Nell'ambito di zona interna, il record **www.career.contoso.com** viene aggiunto con l'indirizzo IP 10.0.0.39, ovvero un indirizzo IP privato; e nell'ambito di zona predefinito lo stesso record, **www.career.contoso.com**, viene aggiunto con l'indirizzo IP 65.55.39.10.

Non **– ZoneScope** nei comandi di esempio seguente viene fornito parametro quando il record viene aggiunta all'ambito di zona predefinito. È simile all'aggiunta di record a una zona vaniglia.

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

###<a name="bkmk_policies"></a>Creare i criteri DNS

Dopo avere identificato le interfacce del server per la rete esterna e la rete interna e aver creato gli ambiti di zona, è necessario creare criteri DNS che si connettono gli ambiti di zona interni ed esterni.

>[!NOTE]
>Questo esempio Usa l'interfaccia del server come criteri per distinguere i client interni ed esterni. Un altro metodo per distinguere i client esterni e interni è utilizzando subnet del client come un criterio. Se è possibile identificare la subnet a cui appartengono i client interni, è possibile configurare criteri DNS per distinguere in base alle subnet del client. Per informazioni su come configurare la gestione del traffico tramite criteri di subnet di client, vedere [usare i criteri DNS per la gestione del traffico basato su posizione geografica con i server primari](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers).

Quando il server DNS riceve una query sull'interfaccia privata, la risposta alla query DNS viene restituita dall'ambito di zona interna.

>[!NOTE]
>Criteri non sono necessari per il mapping di ambito predefinito zona. 

Nell'esempio il comando seguente, 10.0.0.56 è l'indirizzo IP nell'interfaccia di rete privata, come illustrato nella figura precedente.

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).  


## <a name="bkmk_recursion"></a>Esempio di controllo di ricorsione selettiva DNS

Ecco un esempio di come è possibile utilizzare criteri DNS per eseguire lo scenario descritto in precedenza di controllo di ricorsione selettiva DNS.

Questa sezione contiene gli argomenti seguenti.

- [Come funziona controllo ricorsione selettiva di DNS](#bkmk_recursionhow)
- [Come configurare il controllo di ricorsione selettiva DNS](#bkmk_recursionconfigure)

Questo esempio Usa la stessa società fittizia come nell'esempio precedente, Contoso, che gestisce un sito Web di carriera in www.career.contoso.com.

Nell'esempio DNS "split Brain" distribuzione stesso server DNS risponde ai client esterni e interni e fornisce risposte diverse. 

Alcune distribuzioni di DNS potrebbero richiedere il server DNS stesso per eseguire la risoluzione dei nomi ricorsiva per i client interni oltre funge da server dei nomi autorevole per i client esterni. In questo caso viene chiamato controllo ricorsione selettiva DNS.

Nelle versioni precedenti di Windows Server, consentendo la ricorsione significava che era stata attivata l'intero server DNS per tutte le zone. Poiché il server DNS è in ascolto anche alle query esterna, ricorsione è abilitata per i client interni ed esterni, rendendo il server DNS un resolver aperto. 

Un server DNS configurato come un resolver aprire potrebbe essere esposto a esaurimento delle risorse e può essere sfruttato dai client dannoso per creare riflesso attacchi. 

Per questo motivo, gli amministratori DNS Contoso non desidera il server DNS per contoso.com eseguire la risoluzione dei nomi ricorsiva per i client esterni. È necessario solo per il controllo di ricorsione per i client interni, mentre la ricorsione controllo può essere bloccato per i client esterni. 

Nella figura seguente viene illustrato questo scenario.

![Controllo ricorsione selettiva](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg) 


### <a name="bkmk_recursionhow"></a>Come funziona controllo ricorsione selettiva di DNS

Se si riceve una query per cui il server DNS di Contoso è autorevole, ad esempio per www.microsoft.com, quindi la richiesta di risoluzione nome viene valutata rispetto ai criteri sul server DNS. 

Poiché queste query non rientrano in qualsiasi zona, la zona livello criteri \ (come definito nel example\ "split Brain") non vengono valutate. 

Il server DNS valuta i criteri di ricorsione e le query che vengono ricevute sulla corrispondenza interfaccia privata la **SplitBrainRecursionPolicy**. Questo criterio punta a un ambito di ricorsione in cui è abilitata la ricorsione.

Il server DNS esegue la ricorsione per ottenere la risposta per www.microsoft.com da Internet e memorizza nella cache in locale la risposta. 

Se la query viene ricevuta l'interfaccia esterna non corrispondenza di criteri DNS e l'impostazione predefinita la ricorsione - che in questo caso è **disabilitato** -viene applicato.

Ciò impedisce che il server che agisce come un resolver aperto per i client esterni, mentre viene utilizzato come un resolver di memorizzazione nella cache per i client interni. 

### <a name="bkmk_recursionconfigure"></a>Come configurare il controllo di ricorsione selettiva DNS

Per configurare il DNS ricorsione selettiva controllo tramite criteri di DNS, è necessario utilizzare la procedura seguente.

- [Creare ambiti di ricorsione DNS](#bkmk_recscopes)
- [Creare criteri di ricorsione DNS](#bkmk_recpolicy)

#### <a name="bkmk_recscopes"></a>Creare ambiti di ricorsione DNS

Gli ambiti di ricorsione sono istanze univoche di un gruppo di impostazioni che controllano la ricorsione in un server DNS. Un ambito di ricorsione contiene un elenco di server d'inoltro e specifica se è abilitata la ricorsione. Un server DNS può avere molti gli ambiti di ricorsione. 

L'impostazione di ricorsione legacy e un elenco di server d'inoltro vengono definiti per l'ambito di ricorsione predefinito. È possibile aggiungere o rimuovere l'ambito di ricorsione predefinito, identificato dal punto nome \("." \).

In questo esempio, l'impostazione di ricorsione predefinita è disabilitato, mentre viene creato un nuovo ambito di ricorsione per i client interni in cui è abilitata la ricorsione.

    
    Set-DnsServerRecursionScope -Name . -EnableRecursion $False
    Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True 
    

Per ulteriori informazioni, vedere [aggiungere DnsServerRecursionScope](https://technet.microsoft.com/library/mt126268.aspx)

#### <a name="bkmk_recpolicy"></a>Creare criteri di ricorsione DNS

Creare criteri di ricorsione per scegliere un ambito di ricorsione per un set di query che corrispondono ai criteri specificati server DNS. 

Se il server DNS non è autorevole per alcune query, criteri di ricorsione dei server DNS consentono di controllare la modalità di risolvere le query. 

In questo esempio, l'ambito di ricorsione interna con ricorsione abilitata è associato all'interfaccia di rete privata

È possibile utilizzare il comando seguente per configurare i criteri DNS ricorsione.

    
    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.39"
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).

Ora il server DNS è configurato con i criteri necessari DNS per un server dei nomi "split Brain" o un server DNS con il controllo ricorsione selettiva abilitato per i client interni.

È possibile creare migliaia di criteri DNS in base del traffico di gestione, e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, su query in ingresso. 

Per ulteriori informazioni, vedere [Guida agli scenari dei criteri DNS](DNS-Policy-Scenario-Guide.md).
