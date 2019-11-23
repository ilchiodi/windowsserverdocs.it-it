---
title: Usare i criteri DNS per la distribuzione DNS "split brain"
description: Questo argomento fa parte della Guida allo scenario dei criteri DNS per Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5449c9e96a5a9ecd08ca35e703a76927f4e27158
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356023"
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>Usare i criteri DNS per la distribuzione DNS split\-Brain

>Si applica a: Windows Server 2016

È possibile usare questo argomento per informazioni su come configurare i criteri DNS in Windows Server&reg; 2016 per le distribuzioni DNS "Split Brain", in cui sono disponibili due versioni di una singola zona, una per gli utenti interni della Intranet dell'organizzazione e una per gli utenti esterni, che in genere sono utenti di Internet.

>[!NOTE]
>Per informazioni su come usare i criteri DNS per la distribuzione DNS split\-Brain con Active Directory Zone DNS integrato, vedere [usare i criteri DNS per DNS split-brain in Active Directory](dns-sb-with-ad.md).

In precedenza, questo scenario richiedeva che gli amministratori DNS mantenessero due server DNS diversi, ciascuno dei quali fornisce servizi a ogni set di utenti, interno ed esterno. Se solo pochi record all'interno della zona venivano divisi\-brained o entrambe le istanze della zona (interna ed esterna) venivano delegate allo stesso dominio padre, questo è diventato un enigma di gestione. 

Un altro scenario di configurazione per la distribuzione Split-Brain è il controllo della ricorsione selettiva per la risoluzione dei nomi DNS. In alcuni casi, è previsto che i server DNS dell'organizzazione eseguano la risoluzione ricorsiva su Internet per gli utenti interni, mentre devono fungere anche da server dei nomi autorevoli per gli utenti esterni e bloccarne la ricorsione. 

In questo argomento sono incluse le sezioni seguenti.

- [Esempio di distribuzione di DNS split brain](#bkmk_sbexample)
- [Esempio di controllo di ricorsione selettiva DNS](#bkmk_recursion)

## <a name="bkmk_sbexample"></a>Esempio di distribuzione di DNS split brain
Di seguito è riportato un esempio di come è possibile usare i criteri DNS per ottenere lo scenario descritto in precedenza di DNS "Split Brain".

Questa sezione descrive gli argomenti seguenti:

- [Funzionamento della distribuzione Split Brain di DNS](#bkmk_sbhow)
- [Come configurare la distribuzione della divisione Brain DNS](#bkmk_sbconfigure)


Questo esempio usa una società fittizia, contoso, che gestisce un sito Web di carriera presso www.career.contoso.com.

Il sito dispone di due versioni, una per gli utenti interni in cui sono disponibili invii di processi interni. Questo sito interno è disponibile nell'indirizzo IP locale 10.0.0.39. 

La seconda versione è la versione pubblica dello stesso sito, disponibile nell'indirizzo IP pubblico 65.55.39.10.

In assenza di criteri DNS, l'amministratore deve ospitare queste due zone in server DNS Windows Server distinti e gestirli separatamente. 

Uso dei criteri DNS queste zone possono ora essere ospitate nello stesso server DNS.  

Nella figura seguente viene illustrato questo scenario.

![Distribuzione DNS split brain](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)  


## <a name="bkmk_sbhow"></a>Funzionamento della distribuzione Split Brain di DNS

Quando il server DNS è configurato con i criteri DNS necessari, ogni richiesta di risoluzione dei nomi viene valutata in base ai criteri nel server DNS.

L'interfaccia server viene usata in questo esempio come criterio per distinguere tra i client interni ed esterni.

Se l'interfaccia server su cui viene ricevuta la query corrisponde a uno qualsiasi dei criteri, l'ambito della zona associato viene utilizzato per rispondere alla query. 

Quindi, in questo esempio, le query DNS per www.career.contoso.com ricevute sull'IP privato (10.0.0.56) ricevono una risposta DNS che contiene un indirizzo IP interno; e le query DNS ricevute sull'interfaccia di rete pubblica ricevono una risposta DNS che contiene l'indirizzo IP pubblico nell'ambito di zona predefinito (corrisponde alla normale risoluzione delle query).  

## <a name="bkmk_sbconfigure"></a>Come configurare la distribuzione della divisione Brain DNS
Per configurare la distribuzione DNS split-brain usando i criteri DNS, è necessario seguire questa procedura.

- [Creare gli ambiti di zona](#bkmk_zscopes)  
- [Aggiungere record agli ambiti di zona](#bkmk_records)  
- [Creazione dei criteri DNS](#bkmk_policies)

Le sezioni seguenti forniscono le istruzioni di configurazione dettagliate.

>[!IMPORTANT]
>Nelle sezioni seguenti includono esempi di comandi Windows PowerShell che contengono valori di esempio per numero di parametri. Assicurarsi di sostituire i valori di esempio in questi comandi con i valori appropriati per la distribuzione prima di eseguire questi comandi. 

### <a name="bkmk_zscopes"></a>Creare gli ambiti di zona

Un ambito di una zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona contenente il proprio set di record DNS. Lo stesso record può essere presente in più ambiti, con indirizzi IP diversi o con gli stessi indirizzi IP. 

> [!NOTE]
> Per impostazione predefinita, un ambito di una zona esistente nelle zone DNS. Questo ambito di zona ha lo stesso nome della zona e le operazioni DNS legacy funzionano in questo ambito. Questo ambito di zona predefinito ospiterà la versione esterna di www.career.contoso.com.

È possibile usare il comando di esempio seguente per partizionare l'ambito della zona contoso.com per creare un ambito di zona interno. L'ambito della zona interna verrà usato per gestire la versione interna di www.career.contoso.com.

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

Per ulteriori informazioni, vedere [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records"></a>Aggiungere record agli ambiti di zona

Il passaggio successivo consiste nell'aggiungere i record che rappresentano l'host del server Web nei due ambiti di zona, interni e predefiniti (per i client esterni). 

Nell'ambito della zona interna, il record <strong>www.career.contoso.com</strong> viene aggiunto con l'indirizzo IP 10.0.0.39, che è un indirizzo IP privato. nell'ambito della zona predefinita viene aggiunto lo stesso record, <strong>www.career.contoso.com</strong>, con l'indirizzo IP 65.55.39.10.

Il parametro **-ZoneScope** non viene fornito nei comandi di esempio seguenti quando il record viene aggiunto all'ambito di zona predefinito. Questa operazione è simile all'aggiunta di record a una zona vaniglia.

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="bkmk_policies"></a>Creazione dei criteri DNS

Una volta identificate le interfacce server per la rete esterna e la rete interna e sono stati creati gli ambiti di zona, è necessario creare criteri DNS che connettono gli ambiti di zona interni ed esterni.

>[!NOTE]
>In questo esempio viene utilizzata l'interfaccia server come criterio per distinguere tra i client interni ed esterni. Un altro metodo per distinguere i client esterni e interni consiste nell'usare le subnet client come criteri. Se è possibile identificare le subnet a cui appartengono i client interni, è possibile configurare i criteri DNS per distinguerli in base alla subnet del client. Per informazioni su come configurare la gestione del traffico usando i criteri di subnet client, vedere [usare i criteri DNS per la gestione del traffico basata sulla posizione geografica con i server primari](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers).

Quando il server DNS riceve una query sull'interfaccia privata, la risposta alla query DNS viene restituita dall'ambito della zona interna.

>[!NOTE]
>Criteri non sono necessari per il mapping tra l'ambito di orario predefinito. 

Nel comando di esempio seguente, 10.0.0.56 è l'indirizzo IP nell'interfaccia di rete privata, come illustrato nella figura precedente.

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  


## <a name="bkmk_recursion"></a>Esempio di controllo di ricorsione selettiva DNS

Di seguito è riportato un esempio di come è possibile usare i criteri DNS per ottenere lo scenario descritto in precedenza del controllo della ricorsione selettiva DNS.

Questa sezione descrive gli argomenti seguenti:

- [Funzionamento del controllo di ricorsione selettiva DNS](#bkmk_recursionhow)
- [Come configurare il controllo di ricorsione selettiva DNS](#bkmk_recursionconfigure)

Questo esempio usa la stessa società fittizia dell'esempio precedente, contoso, che gestisce un sito Web di carriera presso www.career.contoso.com.

Nell'esempio di distribuzione di DNS split brain, lo stesso server DNS risponde ai client interni ed esterni e fornisce risposte diverse. 

Alcune distribuzioni DNS potrebbero richiedere lo stesso server DNS per eseguire la risoluzione dei nomi ricorsiva per i client interni, oltre a fungere da server dei nomi autorevole per i client esterni. Questa circostanza viene chiamata controllo di ricorsione selettiva DNS.

Nelle versioni precedenti di Windows Server, l'abilitazione della ricorsione significava che era abilitata nell'intero server DNS per tutte le zone. Poiché il server DNS è in ascolto anche di query esterne, la ricorsione è abilitata per i client interni ed esterni, rendendo il server DNS un sistema di risoluzione aperto. 

Un server DNS configurato come sistema di risoluzione aperto potrebbe essere vulnerabile all'esaurimento delle risorse e può essere usato da client malintenzionati per creare attacchi di Reflection. 

Per questo motivo, gli amministratori DNS di Contoso non desiderano che il server DNS per contoso.com esegua la risoluzione dei nomi ricorsiva per i client esterni. È necessario solo il controllo della ricorsione per i client interni, mentre il controllo di ricorsione può essere bloccato per i client esterni. 

Nella figura seguente viene illustrato questo scenario.

![Controllo ricorsione selettiva](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg) 


### <a name="bkmk_recursionhow"></a>Funzionamento del controllo di ricorsione selettiva DNS

Se viene ricevuta una query per la quale il server DNS contoso è non autorevole, ad esempio per www.microsoft.com, la richiesta di risoluzione dei nomi viene valutata in base ai criteri nel server DNS. 

Poiché queste query non rientrano in nessuna zona, i criteri a livello di zona \(come definito nell'esempio Split Brain\) non vengono valutati. 

Il server DNS valuta i criteri di ricorsione e le query ricevute sull'interfaccia privata corrispondono a **SplitBrainRecursionPolicy**. Questo criterio punta a un ambito di ricorsione in cui è attivata la ricorsione.

Il server DNS esegue quindi la ricorsione per ottenere la risposta per www.microsoft.com da Internet e memorizza nella cache la risposta localmente. 

Se la query viene ricevuta sull'interfaccia esterna, nessun criterio DNS corrisponde e viene applicata l'impostazione di ricorsione predefinita, che in questo caso è **disabilitata** .

In questo modo si impedisce che il server funga da sistema di risoluzione aperto per i client esterni, mentre funge da resolver di memorizzazione nella cache per i client interni. 

### <a name="bkmk_recursionconfigure"></a>Come configurare il controllo di ricorsione selettiva DNS

Per configurare il controllo della ricorsione selettiva DNS usando i criteri DNS, è necessario seguire questa procedura.

- [Creare ambiti di ricorsione DNS](#bkmk_recscopes)
- [Creare criteri di ricorsione DNS](#bkmk_recpolicy)

#### <a name="bkmk_recscopes"></a>Creare ambiti di ricorsione DNS

Gli ambiti di ricorsione sono istanze univoche di un gruppo di impostazioni che controllano la ricorsione in un server DNS. Un ambito di ricorsione contiene un elenco di server d'attesa e specifica se è abilitata la ricorsione. Un server DNS può avere molti ambiti di ricorsione. 

L'impostazione di ricorsione legacy e l'elenco dei server d'inoltri sono detti ambito di ricorsione predefinito. Non è possibile aggiungere o rimuovere l'ambito di ricorsione predefinito, identificato dal nome punto \("."\).

In questo esempio l'impostazione di ricorsione predefinita è disabilitata, mentre viene creato un nuovo ambito di ricorsione per i client interni in cui è abilitata la ricorsione

    
    Set-DnsServerRecursionScope -Name . -EnableRecursion $False
    Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True 
    

Per ulteriori informazioni, vedere [Add-DnsServerRecursionScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverrecursionscope?view=win10-ps)

#### <a name="bkmk_recpolicy"></a>Creare criteri di ricorsione DNS

È possibile creare criteri di ricorsione del server DNS per scegliere un ambito di ricorsione per un set di query che soddisfano criteri specifici. 

Se il server DNS non è autorevole per alcune query, i criteri di ricorsione del server DNS consentono di controllare la modalità di risoluzione delle query. 

In questo esempio, l'ambito di ricorsione interno con ricorsione abilitato è associato all'interfaccia di rete privata.

È possibile usare il comando di esempio seguente per configurare i criteri di ricorsione DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP "EQ,10.0.0.39"
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Il server DNS è ora configurato con i criteri DNS necessari per un server dei nomi Split-Brain o un server DNS con il controllo di ricorsione selettiva abilitato per i client interni.

È possibile creare migliaia di criteri DNS in base del traffico di gestione e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, per le query in ingresso. 

Per ulteriori informazioni, vedere la [Guida allo scenario dei criteri DNS](DNS-Policy-Scenario-Guide.md).
