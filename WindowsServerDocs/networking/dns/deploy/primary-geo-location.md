---
title: Usare i criteri DNS per posizione geografica basato su gestione del traffico con i server primari
description: In questo argomento fa parte di DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bd72e13cd3d2d7f3ca886afcdcf97e824ef227f5
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>Usare i criteri DNS per posizione geografica basato su gestione del traffico con i server primari

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come configurare criteri DNS per consentire ai server DNS primario rispondere alle query client DNS in base alla posizione geografica del client e la risorsa a cui il client sta tentando di connettersi, fornendo il client con l'indirizzo IP della risorsa più vicina.  
  
>[!IMPORTANT]  
>Questo scenario viene illustrato come distribuire i criteri DNS per la gestione del traffico basato su posizione geografica quando si utilizza server DNS primari solo. È inoltre possibile eseguire la gestione del traffico basato su posizione geografica quando si dispone di server DNS primari e secondari. Se si dispone di una distribuzione primario, secondario, completare i passaggi descritti in questo argomento e seguire le istruzioni fornite nell'argomento [usare i criteri DNS per la gestione del traffico basato su posizione geografica con distribuzioni primario secondario](primary-secondary-geo-location.md).

Con nuovi criteri DNS, è possibile creare un criterio DNS che consente al server DNS di rispondere a una query di client che richiede l'indirizzo IP di un server Web. Le istanze del server Web potrebbero trovarsi in diversi Data Center in posizioni fisiche diverse. DNS è possibile valutare i client e i percorsi dei server Web, quindi rispondere alla richiesta del client, fornendo il client con un server Web l'indirizzo IP per un server Web che si trova fisicamente più vicino al client.  

È possibile utilizzare i seguenti parametri dei criteri DNS per controllare le risposte del server DNS alle query provenienti dai client DNS. 

- **Subnet del client**. Nome di una subnet del client predefinito. Usato per verificare la subnet da cui è stata inviata la query.
- **Protocollo di trasporto**. Il trasporto di protocollo utilizzato nella query. Sono possibili voci **UDP** e **TCP**.
- **Internet Protocol**. Protocollo di rete utilizzato nella query. Sono possibili voci **IPv4** e **IPv6**.
- **Indirizzo IP di interfaccia server**. Indirizzo IP dell'interfaccia di rete del server DNS che ha ricevuto la richiesta DNS.
- **Nome di dominio completo**. Il completamente dominio nome completo (FQDN) del record nella query, con la possibilità di usare un carattere jolly.
- **Tipo di query**. Tipo di record sottoposto a query (A, SRV, TXT e così via).
- **Ora del giorno**. Ora del giorno che viene ricevuta la query. 
  
È possibile combinare i seguenti criteri con un operatore logico (e/o) per formulare le espressioni di criteri. Quando queste espressioni corrispondono, i criteri devono eseguire una delle seguenti operazioni.
 
- **Ignorare**. Il server DNS elimina automaticamente la query.          
- **Nega**. Il server DNS risponde a query con una risposta di errore.          
- **Consenti**. Il server DNS risponde con risposta gestito il traffico.          
  
##  <a name="bkmk_example"></a>Posizione geografica basato su esempio Gestione traffico

Ecco un esempio di come è possibile utilizzare criteri DNS per ottenere il reindirizzamento del traffico in base al percorso fisico del client che esegue una query DNS.   
  
Questo esempio utilizza due società fittizia - servizi Cloud di Contoso, che fornisce web e il dominio che ospita soluzioni. e servizi di ristorazione Woodgrove, che fornisce servizi di recapito ristorazione in più città in tutto il mondo e che dispone di un sito Web denominato woodgrove.com.  
  
Servizi Cloud di Contoso ha due Data Center, uno negli Stati Uniti e l'altro in Europa. Data Center dell'Europa ospita un alimento ordinamento portale per woodgrove.com.   
  
Per garantire che i clienti woodgrove.com ottenere prestazioni ottimali dal relativo sito Web, Woodgrove desidera europee indirizzato al Data Center dell'Europa e americano client indirizzato al Data Center degli Stati Uniti. I clienti ubicati altrove nel mondo possono essere indirizzati a uno dei Data Center.   
  
Nella figura seguente viene illustrato questo scenario.  
  
![Posizione geografica basato su esempio Gestione traffico](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)  
  
##  <a name="bkmk_works"></a>Funziona come il nome DNS processo di risoluzione  
  
Durante il processo di risoluzione nome, l'utente tenta di connettersi a www.woodgrove.com. Ciò produce una richiesta di risoluzione nome DNS che viene inviata al server DNS configurati nelle proprietà della connessione di rete nel computer dell'utente. In genere, questo è il server DNS fornito dal provider del servizio locale che agisce come un resolver di memorizzazione nella cache e viene definito come il LDNS.   
  
Se il nome DNS non è presente nella cache locale di LDNS, il server LDNS inoltra la query al server DNS autorevole per woodgrove.com. Il server DNS autorevole risponde con il record richiesto (www.woodgrove.com) al server LDNS, che a sua volta memorizza nella cache il record locale prima di inviarlo al computer dell'utente.  
  
Poiché servizi Cloud di Contoso utilizza criteri di Server DNS, server DNS autorevole che ospita contoso.com è configurato per restituire la posizione geografica basato su risposte gestito il traffico. Di conseguenza la direzione di clienti europei per il data center dell'Europa e la direzione di American client al Data Center degli Stati Uniti, come illustrato nella figura.  
  
In questo scenario, il server DNS autorevole vede in genere la richiesta di risoluzione nome provenienti dal server LDNS e molto raramente, dal computer dell'utente. Per questo motivo, l'indirizzo IP di origine nella richiesta di risoluzione nome riconosciuto dal server DNS autorevole è quello del server LDNS e non quello del computer dell'utente. Tuttavia, utilizzando l'indirizzo IP del server LDNS quando si configura la posizione geografica basato su query risposte fornisce una stima equo della posizione geografica dell'utente, perché l'utente sta eseguendo una query il server DNS della sua ISP locale.  
  
>[!NOTE]  
>Criteri DNS utilizzano l'IP del mittente nel pacchetto che contiene la query DNS TCP/UDP. Se la query raggiunge il server primario attraverso più hop resolver/LDNS, i criteri prenderà in considerazione solo l'indirizzo IP del sistema di risoluzione ultimo da cui il server DNS riceve la query.  
  
##  <a name="bkmk_config"></a>Come configurare criteri DNS per posizione geografica basato su Query risposte  
Per configurare criteri DNS per le risposte alle query in base alla posizione geografica, è necessario eseguire i passaggi seguenti.  
  
1. [Creare la subnet del Client DNS](#bkmk_subnets)  
2. [Creare gli ambiti della zona](#bkmk_scopes)  
3. [Aggiungere record per gli ambiti di zona](#bkmk_records)  
4. [Creare i criteri](#bkmk_policies)  
  
>[!NOTE]  
>È necessario eseguire questi passaggi nel server DNS autorevole per la zona in cui che si desidera configurare. Appartenenza al gruppo **DnsAdmins**, o equivalente, è necessario eseguire le procedure seguenti.  
  
Le sezioni seguenti forniscono istruzioni dettagliate di configurazione.  
  
>[!IMPORTANT]  
>Le sezioni seguenti includono esempi di comandi Windows PowerShell che contengono i valori di esempio per numero di parametri. Assicurarsi di sostituire i valori di esempio in questi comandi con i valori appropriati per la distribuzione prima di eseguire questi comandi.  
  
### <a name="bkmk_subnets"></a>Creare la subnet del Client DNS  
  
Il primo passaggio consiste nell'identificare le subnet o spazio di indirizzi IP delle aree per cui si desidera reindirizzare il traffico. Ad esempio, se si desidera reindirizzare il traffico per Stati Uniti ed Europa, è necessario identificare la subnet o spazi degli indirizzi IP di queste aree.  
  
È possibile ottenere queste informazioni da mappe Geo-IP. In base a queste distribuzioni geografica IP, è necessario creare "DNS Client subnet". Una Subnet del Client DNS è un raggruppamento logico di subnet IPv4 o IPv6 da cui le query vengono inviate a un server DNS.  
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per creare subnet del Client DNS.  
  
      
    Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"  
      
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"  
      
  
Per ulteriori informazioni, vedere [Aggiungi DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="bkmk_scopes"></a>Creare ambiti di zona  
Dopo avere configurata la subnet del client, è necessario partizionare la zona il cui traffico che si desidera reindirizzare in due ambiti fuso diverso, un ambito per ogni subnet Client DNS che è stato configurato.   
  
Ad esempio, se si desidera reindirizzare il traffico per il nome DNS www.woodgrove.com, è necessario creare due ambiti diversi zona in zona woodgrove.com, uno per gli Stati Uniti e uno per l'Europa.  
  
Un ambito di zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona che contiene un proprio set di record DNS. Lo stesso record possono essere presenti in più ambiti, con indirizzi IP diversi o gli stessi indirizzi IP.  
  
>[!NOTE]  
>Per impostazione predefinita, un ambito di una zona esistente nelle zone DNS. Questo ambito di zona con lo stesso nome della zona e operazioni DNS legacy funzionano in questo ambito.   
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per creare ambiti di zona.  
  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"  

Per ulteriori informazioni, vedere [Aggiungi DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="bkmk_records"></a>Aggiungere record per gli ambiti di zona  
Ora è necessario aggiungere i record che rappresenta l'host del server web in ambiti due zone.   
  
Ad esempio, **USZoneScope** e **EuropeZoneScope**. In USZoneScope, è possibile aggiungere il record www.woodgrove.com con l'indirizzo IP 192.0.0.1, che si trova in un Data Center degli Stati Uniti; e in EuropeZoneScope è possibile aggiungere lo stesso record (www.woodgrove.com) con l'indirizzo IP 141.1.0.1 nel data center dell'Europa.   
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per aggiungere record per gli ambiti di zona.  
  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"  
  
  
  
In questo esempio, è necessario utilizzare anche i seguenti comandi di Windows PowerShell per aggiungere i record nell'ambito predefinito zona per garantire che il resto del mondo può comunque server web di accesso di woodgrove.com da uno dei due Data Center.  
    
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"   
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
      
 
Il **ZoneScope** parametro non è incluso quando si aggiunge un record nell'ambito predefinito. Questo è uguale all'aggiunta di record per una zona DNS standard.  
  
Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
### <a name="bkmk_policies"></a>Creare i criteri  
Dopo aver creato le subnet, le partizioni (ambiti zona) ed è stato aggiunto record, è necessario creare criteri che si connettono le subnet e partizioni, in modo che quando una query provenga da un'origine in una subnet client DNS, la risposta alla query viene restituita dall'ambito corretto della zona. Criteri non sono necessari per il mapping di ambito predefinito zona.   
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per creare un criterio DNS che collega la subnet del Client DNS e gli ambiti di zona.   
  
      
    Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"  
      
    Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"  
     
Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Ora il server DNS è configurato con i criteri necessari DNS per reindirizzare il traffico in base alla posizione geografica.  
  
Quando il server DNS riceve una query di risoluzione dei nomi, il server DNS valuta i campi nella richiesta DNS con i criteri DNS configurati. Se l'indirizzo IP di origine nella richiesta di risoluzione nome corrisponde a uno qualsiasi dei criteri, l'ambito delle zone associati verrà utilizzato per rispondere alla query e l'utente viene indirizzato alla risorsa che geograficamente più vicino.   
  
È possibile creare migliaia di criteri DNS in base del traffico di gestione, e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, su query in ingresso.  
  
  
