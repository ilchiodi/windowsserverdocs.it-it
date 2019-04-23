---
title: Usare i criteri DNS per risposte DNS intelligenti basate sull'ora del giorno
description: Questo argomento fa parte del DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33fd9447a79346127714a5e5e73977611eba483c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829472"
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>Usare i criteri DNS per risposte DNS intelligenti basate sull'ora del giorno

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come distribuire il traffico dell'applicazione tra diverse istanze distribuite geograficamente di un'applicazione utilizzando i criteri DNS che si basano sull'ora del giorno.  
  
Questo scenario è utile nelle situazioni in cui si desidera indirizzare il traffico in un fuso orario per il server di applicazioni alternativo, ad esempio server Web, che si trovano in un altro fuso orario. Ciò consente di bilanciare il carico tra le istanze dell'applicazione durante il picco quando il server primario sono sottoposti a overload con traffico periodi di tempo.   
  
### <a name="bkmk_example1"></a>Esempio delle risposte DNS intelligente basate sull'ora del giorno  
Seguito è riportato un esempio di come è possibile utilizzare criteri DNS per bilanciare il traffico dell'applicazione in base all'ora del giorno.  
  
Questo esempio viene utilizzata una società fittizia, Contoso regalo servizi, che fornisce soluzioni gifting online in tutto il mondo tramite il sito Web, contosogiftservices.com.   
  
Il sito Web contosogiftservices.com è ospitato in due Data Center, uno in Seattle (America del Nord) e altro Dublino (Europa). I server DNS sono configurati per l'invio di risposte consapevoli utilizzando criteri DNS posizione geografica. Con un picco recenti nel settore, contosogiftservices.com ha un numero elevato di visitatori ogni giorno e alcuni utenti hanno segnalato problemi di disponibilità del servizio.  
  
Servizi regalo Contoso esegue un'analisi del sito e consente di individuare ogni sera tra ora locale PM 6 e 9 PM, implica che è un picco di traffico ai server Web. I server Web non sono scalabile per gestire l'aumento del traffico le ore di picco, causando un attacco denial of service ai clienti. L'overload di traffico ora punta stesso si verifica in entrambi i data center dell'Europa e americano. In altri momenti della giornata, il server di gestire i volumi di traffico che sono ben di sotto la capacità massima.  
  
Per garantire che i clienti contosogiftservices.com ottenere prestazioni ottimali dal sito Web, servizi regalo Contoso desidera reindirizzare il traffico alcuni Dublino ai server applicazioni Seattle tra PM 6 e 9 PM Dublino; e che si desidera reindirizzare il traffico alcuni Seattle ai server applicazioni Dublino tra PM 6 e 9 PM di Seattle.  
  
Nella figura seguente viene illustrato questo scenario.  
  
![Tempo di esempio di criterio DNS giorno](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="bkmk_works1"></a>Funzionamento delle risposte DNS basate sull'ora del giorno Works  
  
Quando il server DNS è configurato con un tempo di criteri DNS giorno, tra PM 6 e 9 PM in ogni posizione geografica, il server DNS esegue le operazioni seguenti.  
  
- Le risposte le prime quattro query ricevute con l'indirizzo IP del server Web nel data center locale.  
- Risposta alla query quinta che riceve con l'indirizzo IP del server Web nel centro dati remoto.   
  
Questo comportamento basata su criteri, gli offload venti per cento del carico del traffico del server Web locale al server Web remoto, semplificando al contempo il server applicazioni locale e migliorare le prestazioni del sito per i clienti.  
  
Durante gli orari, i server DNS di eseguono la gestione del traffico normale posizioni geografiche in base. Inoltre, i client DNS che inviano query da percorsi diversi da America del Nord o Europa, il carico del server DNS bilancia il traffico tra i Data Center a Seattle e New York.  
  
Quando sono configurati più criteri DNS in DNS, sono un set ordinato di regole e vengono elaborati da DNS dalla priorità più alta alla priorità più bassa. DNS viene utilizzato il primo criterio che corrisponde a circostanze, tra l'ora del giorno. Per questo motivo, i criteri più specifici devono avranno la priorità. Se si crea ora dei criteri di giorno e assegnando una priorità alta nell'elenco dei criteri, DNS elabora e utilizza questi criteri innanzitutto se corrispondano ai parametri di query client DNS e i criteri definiti nei criteri. Se non corrispondono, DNS si sposta verso il basso l'elenco dei criteri per elaborare i criteri predefiniti fino a quando non viene trovata una corrispondenza.  
  
Per ulteriori informazioni sui tipi di criteri e i criteri, vedere [Cenni preliminari sui criteri DNS](../../dns/deploy/DNS-Policies-Overview.md).  
  
### <a name="bkmk_how1"></a>Come configurare criteri DNS per risposte DNS intelligente basate sull'ora del giorno  
Per configurare criteri DNS per le risposte di ora del giorno applicazione bilanciamento del carico basato su query, è necessario eseguire la procedura seguente.  
  
- [Creare la subnet del Client DNS](#bkmk_subnets)  
- [Creare gli ambiti di zona](#bkmk_zscopes)  
- [Aggiungere i record per gli ambiti di zona](#bkmk_records)  
- [Creare i criteri DNS](#bkmk_policies)  
  
>[!NOTE]
>È necessario eseguire questi passaggi nel server DNS autorevole per la zona in cui che si desidera configurare. L'appartenenza a **DnsAdmins**, o equivalente, è necessario per eseguire le procedure seguenti.  
  
Le sezioni seguenti forniscono le istruzioni di configurazione dettagliate.  
  
>[!IMPORTANT]
>Nelle sezioni seguenti includono esempi di comandi Windows PowerShell che contengono valori di esempio per numero di parametri. Assicurarsi di sostituire i valori di esempio in questi comandi con i valori appropriati per la distribuzione prima di eseguire questi comandi.  
  
#### <a name="bkmk_subnets"></a>Creare la subnet del Client DNS  
Il primo passaggio consiste nell'identificare le subnet o spazio di indirizzi IP delle aree per cui si desidera reindirizzare il traffico. Ad esempio, se si desidera reindirizzare il traffico per Stati Uniti e in Europa, è necessario identificare la subnet o spazi di indirizzi IP di queste aree.  
  
È possibile ottenere queste informazioni da mappe Geo-IP. In base a queste distribuzioni geografica IP, è necessario creare "DNS Client subnet". Una Subnet del Client DNS è un raggruppamento logico di subnet IPv4 o IPv6 da cui le query vengono inviate a un server DNS.  
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per creare subnet del Client DNS.  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
Per ulteriori informazioni, vedere [Aggiungi DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
#### <a name="bkmk_zscopes"></a>Creare gli ambiti di zona  
Dopo aver configurata la subnet del client, è necessario partizionare la zona il cui traffico che si desidera reindirizzare in due ambiti fuso diverso, un ambito per ogni subnet Client DNS configurato.  
  
Ad esempio, se si desidera reindirizzare il traffico per www.contosogiftservices.com il nome DNS, è necessario creare due ambiti diversi zona in zona contosogiftservices.com, uno per gli Stati Uniti e uno per l'Europa.  
  
Un ambito di una zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona che contiene un proprio set di record DNS. Lo stesso record possono essere presenti in più ambiti, con diversi indirizzi IP o gli stessi indirizzi IP.  
  
>[!NOTE]
>Per impostazione predefinita, un ambito di una zona esistente nelle zone DNS. In questo ambito di zona ha lo stesso nome della zona e operazioni DNS legacy funzionano in questo ambito.  
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per creare ambiti di zona.  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
Per ulteriori informazioni, vedere [Aggiungi DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
#### <a name="bkmk_records"></a>Aggiungere i record per gli ambiti di zona  
È ora necessario aggiungere i record che rappresenta l'host del server web in ambiti due zone.  
  
Ad esempio, in **SeattleZoneScope**, il record **www.contosogiftservices.com** viene aggiunto con l'indirizzo IP 192.0.0.1, che si trova in un datacenter di Seattle. Analogamente, nel **DublinZoneScope**, il record **www.contosogiftservices.com** viene aggiunto con l'indirizzo IP 141.1.0.3 nel Data Center Dublino  
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per aggiungere record per gli ambiti di zona.  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
Il parametro ZoneScope non è incluso quando si aggiunge un record nell'ambito predefinito. Questo è uguale all'aggiunta di record per una zona DNS standard.  
  
Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
#### <a name="bkmk_policies"></a>Creare i criteri DNS  
Dopo aver creato le subnet, le partizioni (ambiti zona) ed è stato aggiunto record, è necessario creare criteri che si connettono le subnet e le partizioni, in modo che quando una query provenga da un'origine in una delle subnet dei client DNS, la risposta alla query verrà restituita dall'ambito corretto della zona. Criteri non sono necessari per il mapping tra l'ambito di orario predefinito.  
  
Dopo aver configurato i criteri DNS, il comportamento del server DNS è il seguente:  
  
1. I client DNS europee ricevono l'indirizzo IP del server Web nel Data Center Dublin nella risposta query DNS.  
2. I client DNS American ricevono l'indirizzo IP del server Web nel datacenter di Seattle nella risposta query DNS.  
3. Tra PM 6 e 9 PM Dublino, 20% delle query dai client europei ricevere l'indirizzo IP del server Web in Data Center di Seattle in loro risposta alla query DNS.  
4. Tra PM 6 e 9 PM di Seattle, 20% delle query dai client American ricevere l'indirizzo IP del server Web nel Data Center Dublin nella risposta query DNS.  
5. Metà delle query dal resto del mondo ricevere l'indirizzo IP del datacenter di Seattle e l'altra metà ricevere l'indirizzo IP del Data Center Dublin.  
  
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per creare un criterio DNS che collega la subnet del Client DNS e gli ambiti di zona.  
  
>[!NOTE]
>In questo esempio, il server DNS sia nel fuso orario GMT, pertanto l'ora di punta periodi di tempo devono essere espressi in ora GMT equivalente.  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Ora il server DNS è configurato con i criteri necessari DNS per reindirizzare il traffico in base a posizione geografica e ora del giorno.  
  
Quando il server DNS riceve richieste di risoluzione dei nomi, il server DNS valuta i campi nella richiesta DNS con i criteri del DNS configurato. Se l'indirizzo IP di origine nella richiesta di risoluzione nome corrisponde a uno qualsiasi dei criteri, l'ambito delle zone associati verrà utilizzato per rispondere alla query e l'utente viene indirizzato alla risorsa che geograficamente più vicino.  
  
È possibile creare migliaia di criteri DNS in base del traffico di gestione e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, per le query in ingresso.


