---
title: Utilizzare criteri DNS per DNS "split brain" in Active Directory
description: È possibile usare questo argomento per sfruttare le funzionalità di gestione del traffico dei criteri DNS per le distribuzioni Split Brain con Active Directory zone DNS integrate in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1a05bdcbf6205b8be7044c92e3dcf71a6e62bed6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356030"
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>Utilizzare criteri DNS per DNS "split brain" in Active Directory

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per sfruttare le funzionalità di gestione del traffico dei criteri DNS per le distribuzioni\-Brain con Active Directory le zone DNS integrate in Windows Server 2016.

In Windows Server 2016, il supporto per i criteri DNS viene esteso per Active Directory zone DNS integrate. Active Directory integrazione offre funzionalità di disponibilità elevata per più\-master per il server DNS. 

In precedenza, questo scenario richiedeva che gli amministratori DNS mantenessero due server DNS diversi, ciascuno dei quali fornisce servizi a ogni set di utenti, interno ed esterno. Se solo pochi record all'interno della zona venivano divisi\-brained o entrambe le istanze della zona (interna ed esterna) venivano delegate allo stesso dominio padre, questo è diventato un enigma di gestione.

> [!NOTE]
> - Le distribuzioni DNS sono divise\-Brain quando sono presenti due versioni di una singola zona, una versione per gli utenti interni nell'Intranet dell'organizzazione e una versione per gli utenti esterni, che in genere sono utenti su Internet.
> - L'argomento [usare i criteri DNS per la distribuzione di DNS split-brain](split-brain-DNS-deployment.md) spiega come usare i criteri DNS e gli ambiti di zona per distribuire un sistema dns Split\-Brain in un singolo server DNS Windows Server 2016.



##  <a name="example-split-brain-dns-in-active-directory"></a>Esempio di suddivisione del DNS\-Brain in Active Directory

Questo esempio usa una società fittizia, contoso, che gestisce un sito Web di carriera presso www.career.contoso.com.

Il sito dispone di due versioni, una per gli utenti interni in cui sono disponibili invii di processi interni. Questo sito interno è disponibile nell'indirizzo IP locale 10.0.0.39. 

La seconda versione è la versione pubblica dello stesso sito, disponibile nell'indirizzo IP pubblico 65.55.39.10.

In assenza di criteri DNS, l'amministratore deve ospitare queste due zone in server DNS Windows Server distinti e gestirli separatamente. 

Uso dei criteri DNS queste zone possono ora essere ospitate nello stesso server DNS.

Se il server DNS per contoso.com è Active Directory integrato ed è in ascolto su due interfacce di rete, l'amministratore DNS contoso può seguire la procedura descritta in questo argomento per ottenere una distribuzione\-Brain divisa.

L'amministratore DNS configura le interfacce del server DNS con i seguenti indirizzi IP.

- La scheda di rete con connessione Internet viene configurata con un indirizzo IP pubblico di 208.84.0.53 per le query esterne.
- La scheda di rete connessa alla Intranet è configurata con un indirizzo IP privato di 10.0.0.56 per le query interne.

Nella figura seguente viene illustrato questo scenario.

![Distribuzione DNS integrata di AD Split Brain](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>Come funzionano i criteri DNS per il DNS split\-Brain in Active Directory

Quando il server DNS è configurato con i criteri DNS necessari, ogni richiesta di risoluzione dei nomi viene valutata in base ai criteri nel server DNS.

L'interfaccia server viene usata in questo esempio come criterio per distinguere tra i client interni ed esterni.

Se l'interfaccia server su cui viene ricevuta la query corrisponde a uno qualsiasi dei criteri, l'ambito della zona associato viene utilizzato per rispondere alla query. 

Quindi, in questo esempio, le query DNS per www.career.contoso.com ricevute sull'IP privato (10.0.0.56) ricevono una risposta DNS che contiene un indirizzo IP interno; e le query DNS ricevute sull'interfaccia di rete pubblica ricevono una risposta DNS che contiene l'indirizzo IP pubblico nell'ambito di zona predefinito (corrisponde alla normale risoluzione delle query).  

Il supporto per il DNS dinamico \(gli aggiornamenti del\) DDNS e lo scavenging è supportato solo nell'ambito della zona predefinita. Poiché i client interni sono serviti dall'ambito di zona predefinito, gli amministratori DNS di Contoso possono continuare a usare i meccanismi esistenti (DNS dinamico o statico) per aggiornare i record in contoso.com. Per gli ambiti di zona predefiniti non\-\(ad esempio l'ambito esterno in questo esempio\), il supporto per DDNS o lo scavenging non è disponibile.

### <a name="high-availability-of-policies"></a>Disponibilità elevata dei criteri

I criteri DNS non sono Active Directory integrati. Per questo motivo, i criteri DNS non vengono replicati negli altri server DNS che ospitano la stessa area Active Directory integrata. 

I criteri DNS vengono archiviati nel server DNS locale. È possibile esportare facilmente i criteri DNS da un server a un altro usando i comandi di Windows PowerShell di esempio seguenti.

    $policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
    
    $policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02

Per ulteriori informazioni, vedere gli argomenti di riferimento di Windows PowerShell seguenti.

- [Get-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/get-dnsserverqueryresolutionpolicy?view=win10-ps)
- [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)


## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>Come configurare i criteri DNS per il DNS split\-Brain in Active Directory

Per configurare la distribuzione DNS split-brain usando i criteri DNS, è necessario usare le sezioni seguenti, che forniscono istruzioni dettagliate per la configurazione.

### <a name="add-the-active-directory-integrated-zone"></a>Aggiungere la zona integrata Active Directory

È possibile usare il comando di esempio seguente per aggiungere la Active Directory zona contoso.com integrata al server DNS.

    Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru

Per ulteriori informazioni, vedere [Add-DnsServerPrimaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverprimaryzone?view=win10-ps).

### <a name="create-the-scopes-of-the-zone"></a>Creare ambiti della zona

È possibile usare questa sezione per partizionare la zona contoso.com per creare un ambito di zona esterno.

Un ambito di una zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona contenente il proprio set di record DNS. Lo stesso record può essere presente in più ambiti, con indirizzi IP diversi o con gli stessi indirizzi IP. 

Poiché si aggiunge questo nuovo ambito di zona in una Active Directory zona integrata, l'ambito della zona e i record in esso contenuti verranno replicati tramite Active Directory ad altri server di replica nel dominio.

Per impostazione predefinita, un ambito di zona esiste in ogni zona DNS. Questo ambito di zona ha lo stesso nome della zona e le operazioni DNS legacy funzionano in questo ambito. Questo ambito di zona predefinito ospiterà la versione interna di www.career.contoso.com.

È possibile usare il comando di esempio seguente per creare l'ambito della zona nel server DNS.

    Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"

Per ulteriori informazioni, vedere [Aggiungi DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).

### <a name="add-records-to-the-zone-scopes"></a>Aggiungere record per gli ambiti di zona

Il passaggio successivo consiste nell'aggiungere i record che rappresentano l'host del server Web nei due ambiti di zona: \(esterno e predefinito per i client interni\). 

Nell'ambito dell'area interna predefinita, il record www.career.contoso.com viene aggiunto con l'indirizzo IP 10.0.0.39, che è un indirizzo IP privato; nell'ambito della zona esterna viene aggiunto lo stesso record \(www.career.contoso.com\) con l'indirizzo IP pubblico 65.55.39.10. 

I record \(sia nell'ambito di zona interna predefinito che nell'ambito della zona esterna\) verranno replicati automaticamente nel dominio con i rispettivi ambiti di zona.

È possibile usare il comando di esempio seguente per aggiungere record agli ambiti di zona nel server DNS.

    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
    
    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”

> [!NOTE]
> Il parametro **– ZoneScope** non è incluso quando il record viene aggiunto all'ambito di zona predefinito. Questa azione equivale all'aggiunta di record in una zona normale.

Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="create-the-dns-policies"></a>Creare i criteri DNS

Una volta identificate le interfacce server per la rete esterna e la rete interna e sono stati creati gli ambiti di zona, è necessario creare criteri DNS che connettono gli ambiti di zona interni ed esterni.

> [!NOTE]
> In questo esempio viene utilizzata l'interfaccia server \(parametro-ServerInterface nel comando di esempio seguente\) come criterio per distinguere tra i client interni ed esterni. Un altro metodo per distinguere i client esterni e interni consiste nell'usare le subnet client come criteri. Se è possibile identificare le subnet a cui appartengono i client interni, è possibile configurare i criteri DNS per distinguerli in base alla subnet del client. Per informazioni su come configurare la gestione del traffico usando i criteri di subnet client, vedere [usare i criteri DNS per la gestione del traffico basata sulla posizione geografica con i server primari](primary-geo-location.md).

Dopo aver configurato i criteri, quando viene ricevuta una query DNS sull'interfaccia pubblica, la risposta viene restituita dall'ambito esterno della zona. 

> [!NOTE]
> Non sono necessari criteri per il mapping dell'ambito di zona interna predefinito. 

    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com

> [!NOTE]
> 208.84.0.53 è l'indirizzo IP dell'interfaccia di rete pubblica.

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Il server DNS è ora configurato con i criteri DNS necessari per un server dei nomi Split-Brain con una Active Directory zona DNS integrata.

È possibile creare migliaia di criteri DNS in base del traffico di gestione e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, per le query in ingresso. 
