---
title: Utilizzare criteri DNS per DNS "split brain" in Active Directory
description: È possibile utilizzare questo argomento per il traffico di sfruttare le funzionalità di gestione dei criteri DNS per le distribuzioni di "split Brain" con Active Directory integrato le zone DNS in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 66931d2196b741e469cb726929f7b58985b8d0cd
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812146"
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>Utilizzare criteri DNS per DNS "split brain" in Active Directory

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per sfruttare le funzionalità di gestione traffico di criteri DNS per divisione\-distribuzioni cervello con Active Directory integrato le zone DNS in Windows Server 2016.

In Windows Server 2016, supporto per i criteri DNS è stato esteso per Active Directory zone DNS integrate. Integrazione di Active Directory fornisce più\-master le funzionalità di disponibilità elevata per il server DNS. 

In precedenza, questo scenario richiede che gli amministratori DNS mantenere due server DNS diversi, ogni che fornisce servizi per ogni set di utenti interni ed esterni. Se solo alcuni record all'interno della zona sono state suddivise\-se non o entrambe le istanze della zona (interna ed esterna) sono state delegate allo stesso dominio padre, questo è diventato un problema di gestione.

> [!NOTE]
> - Le distribuzioni DNS vengono suddivise\-brain quando sono presenti due versioni di una singola zona, una versione per gli utenti interni su intranet dell'organizzazione e una versione per gli utenti esterni, che sono in genere, gli utenti su Internet.
> - L'argomento [usare i criteri DNS per la distribuzione DNS Split-Brain](split-brain-DNS-deployment.md) illustra come è possibile usare i criteri DNS e gli ambiti di zona per distribuire una divisione\-brain sistema DNS in un unico server DNS di Windows Server 2016.



##  <a name="example-split-brain-dns-in-active-directory"></a>Suddivisione di esempio\-Brain DNS in Active Directory

Questo esempio Usa una società fittizia, Contoso, che mantiene un sito Web di crescita professionale in www.career.contoso.com.

Il sito dispone di due versioni, uno per gli utenti interni in cui sono disponibili annunci di lavoro interno. Questo sito interno è disponibile all'indirizzo IP locale 10.0.0.39. 

La seconda versione è la versione pubblica del sito stesso, disponibile all'indirizzo IP pubblico 65.55.39.10.

In assenza di criteri di DNS, l'amministratore è necessario per ospitare questi due zone su server DNS di Windows Server separati e gestirli separatamente. 

Usando i criteri DNS queste zone possono ora essere ospitate nello stesso server DNS.

Se il server DNS per contoso.com è integrata in Active Directory ed è in ascolto su due interfacce di rete, l'amministratore DNS Contoso possono seguire i passaggi descritti in questo argomento per ottenere una suddivisione\-brain distribuzione.

L'amministratore DNS consente di configurare le interfacce di server DNS con i seguenti indirizzi IP.

- La scheda di rete con connessione Internet è configurata con un indirizzo IP pubblico di 208.84.0.53 per le query esterne.
- La scheda di rete con connessione Intranet è configurata con un indirizzo IP privato della 10.0.0.56 per le query interne.

Nella figura seguente viene illustrato questo scenario.

!["Split Brain" AD integrated DNS distribuzione](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>Come suddividere i criteri DNS per\-Brain DNS in Active Directory funziona

Quando il server DNS è configurato con i criteri necessari DNS, viene valutato ogni richiesta di risoluzione nome con i criteri nel server DNS.

Il server dell'interfaccia viene utilizzato in questo esempio come criterio di distinguere tra i client interni ed esterni.

Se l'interfaccia del server su cui viene ricevuta la query corrisponde a uno qualsiasi dei criteri, l'ambito delle zone associati viene utilizzato per rispondere alla query. 

Pertanto, nel nostro esempio, le query DNS per www.career.contoso.com ricevuti sull'indirizzo IP privato (10.0.0.56) ricevano una risposta DNS contenente un indirizzo IP interno. e le query DNS che vengono ricevute nell'interfaccia di rete pubblica ricevano una risposta DNS che contiene l'indirizzo IP pubblico nell'ambito di zona predefinito (questo è quello utilizzato per la risoluzione di query normali).  

Supporto per il DNS dinamico \(DDNS\) aggiornamenti e lo scavenging è supportato solo in ambito predefinito zona. Poiché i client interni vengono serviti dall'ambito di zona predefinito, gli amministratori DNS di Contoso può continuare a usare i meccanismi esistenti (dinamici DNS o statici) per aggiornare i record in contoso.com. Per non\-ambiti di zona predefinito \(ad esempio l'ambito esterno in questo esempio\), supporto scavenging o DNS dinamico non è disponibile.

### <a name="high-availability-of-policies"></a>Disponibilità elevata dei criteri

I criteri DNS non sono integrate con Active Directory. Per questo motivo, i criteri DNS non vengono replicati agli altri server DNS che ospitano la zona integrata di Active Directory stessa. 

I criteri DNS vengono archiviati nel server DNS locale. È facilmente possibile esportare criteri DNS da un server a un altro utilizzando i seguenti comandi di Windows PowerShell di esempio.

    $policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
    
    $policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02

Per altre informazioni, vedere gli argomenti di riferimento di Windows PowerShell seguenti.

- [Get-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/get-dnsserverqueryresolutionpolicy?view=win10-ps)
- [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)


## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>Come configurare criteri DNS per divisione\-Brain DNS in Active Directory

Per configurare la distribuzione DNS Split-Brain tramite criteri di DNS, è necessario usare le sezioni seguenti, che forniscono le istruzioni di configurazione dettagliati.

### <a name="add-the-active-directory-integrated-zone"></a>Aggiungere la zona integrata di Active Directory

È possibile usare il comando seguente per aggiungere la zona contoso.com integrata di Active Directory per il server DNS.

    Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru

Per altre informazioni, vedere [Add-DnsServerPrimaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverprimaryzone?view=win10-ps).

### <a name="create-the-scopes-of-the-zone"></a>Creare ambiti della zona

È possibile usare questa sezione per partizionare la zona contoso.com per creare un ambito di zona esterno.

Un ambito di una zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona che contiene un proprio set di record DNS. Lo stesso record possono essere presenti in più ambiti, con diversi indirizzi IP o gli stessi indirizzi IP. 

Poiché si sta aggiungendo questo nuovo ambito di zona in zona integrata in Active Directory, l'ambito di zona e i record in essa verranno replicate tramite Active Directory ad altri server di replica nel dominio.

Per impostazione predefinita, un ambito di zona esista in ogni zona DNS. In questo ambito di zona ha lo stesso nome della zona e operazioni DNS legacy funzionano in questo ambito. In questo ambito di zona predefinito sarà ospitata la versione interna del www.career.contoso.com.

È possibile usare il comando seguente per creare l'ambito di zona nel server DNS.

    Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"

Per ulteriori informazioni, vedere [Aggiungi DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).

### <a name="add-records-to-the-zone-scopes"></a>Aggiungere record per gli ambiti di zona

Il passaggio successivo consiste nell'aggiungere i record che rappresenta l'host del server web in due ambiti dall'esterno della zona e predefiniti \(per i client interni\). 

Nell'ambito predefinito zona interna, www.career.contoso.com il record viene aggiunto con l'indirizzo IP 10.0.0.39, ovvero un indirizzo IP privato; e nell'ambito di zona esterne, lo stesso record \(www.career.contoso.com\) viene aggiunto con l'indirizzo IP pubblico 65.55.39.10. 

I record \(entrambi interna predefinita di zona ambito e l'ambito di zona esterni\) eseguirà automaticamente la replica tra il dominio con i relativi ambiti zona corrispondente.

È possibile usare il comando seguente per aggiungere record per gli ambiti di zona nel server DNS.

    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
    
    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”

> [!NOTE]
> Il **– ZoneScope** parametro non è incluso quando il record viene aggiunto all'ambito predefinito zona. Questa azione è uguale all'aggiunta di record a una zona normale.

Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="create-the-dns-policies"></a>Creare i criteri DNS

Dopo aver identificato le interfacce di server per la rete esterna e la rete interna e siano stati creati gli ambiti di zona, è necessario creare criteri DNS che si connettono gli ambiti di zona interni ed esterni.

> [!NOTE]
> Questo esempio Usa l'interfaccia del server \(il parametro - ServerInterface nel comando riportato di seguito viene riportato di seguito\) come criterio di distinguere tra i client interni ed esterni. Un altro metodo per distinguere tra i client interni ed esterni consiste nell'usare una subnet del client come criterio. Se è possibile identificare la subnet a cui appartengono i client interni, è possibile configurare criteri DNS per distinguere in base alle subnet del client. Per informazioni su come configurare Gestione traffico usando criteri di subnet client, vedere [usare i criteri DNS per la gestione del traffico basato su posizione geografica con i server primari](primary-geo-location.md).

Dopo aver configurato i criteri, quando una query DNS viene ricevuta sull'interfaccia pubblica, la risposta viene restituita dall'ambito esterno della zona. 

> [!NOTE]
> Criteri non sono necessari per il mapping di ambito predefinito zona interno. 

    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com

> [!NOTE]
> 208.84.0.53 è l'indirizzo IP nell'interfaccia di rete pubblica.

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

A questo punto il server DNS è configurato con i criteri necessari DNS per DNS integrato di un server di nome "split Brain" con un server Active Directory zona.

È possibile creare migliaia di criteri DNS in base del traffico di gestione e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, per le query in ingresso. 
