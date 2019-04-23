---
title: Risposte DNS basate sull'ora del giorno con un server app Azure Cloud
description: Questo argomento fa parte del DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ed6ac2ebc8839d0e7ecee682d7644251f8a59381
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829072"
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>Risposte DNS basate sull'ora del giorno con un server app Azure Cloud

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come distribuire il traffico dell'applicazione tra diverse istanze distribuite geograficamente di un'applicazione utilizzando i criteri DNS che si basano sull'ora del giorno. 

Questo scenario è utile nelle situazioni in cui si desidera indirizzare il traffico in un fuso orario per il server di applicazioni alternativo, ad esempio i server Web ospitate in Microsoft Azure, che si trovano in un altro fuso orario. Ciò consente di bilanciare il carico tra le istanze dell'applicazione durante il picco quando il server primario sono sottoposti a overload con traffico periodi di tempo. 

>[!NOTE]
>Per informazioni su come usare i criteri DNS per le risposte DNS intelligente senza l'utilizzo di Azure, vedere [usare i criteri DNS per DNS intelligente risposte basati sull'ora del giorno](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md). 

## <a name="bkmk_azureexample"></a>Esempio delle risposte DNS intelligente basate sull'ora del giorno con il Server di App Cloud di Azure

Seguito è riportato un esempio di come è possibile utilizzare criteri DNS per bilanciare il traffico dell'applicazione in base all'ora del giorno.

Questo esempio viene utilizzata una società fittizia, Contoso regalo servizi, che fornisce soluzioni gifting online in tutto il mondo tramite il sito Web, contosogiftservices.com. 

Il sito web contosogiftservices.com è ospitato solo in un singolo Data Center locale a Seattle (con indirizzo IP pubblico 192.68.30.2). 

Il server DNS si trova anche nel data center locale. 

Con un picco recenti nel settore, contosogiftservices.com ha un numero elevato di visitatori ogni giorno e alcuni utenti hanno segnalato problemi di disponibilità del servizio. 

Servizi regalo Contoso esegue un'analisi del sito e consente di individuare ogni sera tra ora locale PM 6 e 9 PM, implica che è un picco di traffico ai server Web di Seattle. Il server Web non è scalabile per gestire l'aumento del traffico a queste ore di picco, causando un attacco denial of service ai clienti. 

Per garantire che i clienti contosogiftservices.com ottenere prestazioni ottimali dal sito Web, servizi regalo Contoso decide che durante gli orari verrà noleggiare una macchina virtuale \(VM\) in Microsoft Azure per ospitare una copia del proprio server Web .  

Servizi regalo Contoso recupera un indirizzo IP pubblico di Azure per la macchina virtuale (192.68.31.44) e sviluppa l'automazione per distribuire il Server Web tra - 10 alle 17.00, consentendo un periodo di backup di un'ora ogni giorno in Azure.

>[!NOTE]
>Per altre informazioni sulle macchine virtuali di Azure, vedere [macchine virtuali-documentazione](https://azure.microsoft.com/documentation/services/virtual-machines/) 

I server DNS sono configurati con gli ambiti di zona e i criteri DNS in modo che tra 5 e 9 PM ogni giorno, 30% delle query vengono inviate all'istanza del server Web è in esecuzione in Azure.

Nella figura seguente viene illustrato questo scenario.

![Criteri DNS per volta delle risposte giorno](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)  

## <a name="bkmk_azurehow"></a>Come le risposte DNS intelligente basate sull'ora del giorno con Azure il funzionamento di Server App
 
Questo articolo illustra come configurare il server DNS per rispondere alle query DNS con due indirizzi IP del server applicazioni diversi - un server web è in Seattle e l'altro si trova in un Data Center di Azure.

Dopo la configurazione di un nuovo criterio DNS che si basa sulle ore di picco del PM 6 a 9 PM di Seattle, il server DNS invia fa quattrocentonovantadue per cento delle risposte DNS per i client che contiene l'indirizzo IP del server Web di Seattle e 30 per cento delle risposte DNS per clie nts contenente l'indirizzo IP del server Web di Azure, in modo da indirizzare il traffico client al nuovo server Web di Azure e impedire che il server Web di Seattle sovraccarico. 

In tutti gli altri orari, avviene l'elaborazione di query normali e vengono inviate le risposte dall'ambito di zona predefinito che contiene un record per il server web nel data center locale. 

Il valore TTL di 10 minuti sul record di Azure garantisce che il record è scaduto dalla cache LDNS prima che la macchina virtuale viene rimosso da Azure. Uno dei vantaggi di questo tipo di ridimensionamento è che è possibile mantenere i DNS dei dati in locale e mantenere la scalabilità in Azure secondo necessità.

## <a name="bkmk_azureconfigure"></a>Come configurare criteri DNS per risposte DNS intelligente basate sull'ora del giorno con Server di App di Azure
Per configurare criteri DNS per le risposte di ora del giorno applicazione bilanciamento del carico basato su query, è necessario eseguire la procedura seguente.


- [Creare gli ambiti di zona](#bkmk_zscopes)
- [Aggiungere i record per gli ambiti di zona](#bkmk_records)
- [Creare i criteri DNS](#bkmk_policies)


>[!NOTE]
>È necessario eseguire questi passaggi nel server DNS autorevole per la zona in cui che si desidera configurare. L'appartenenza al gruppo DnsAdmins, o equivalente, è necessario eseguire le procedure seguenti. 

Le sezioni seguenti forniscono le istruzioni di configurazione dettagliate.

>[!IMPORTANT]
>Nelle sezioni seguenti includono esempi di comandi Windows PowerShell che contengono valori di esempio per numero di parametri. Assicurarsi di sostituire i valori di esempio in questi comandi con i valori appropriati per la distribuzione prima di eseguire questi comandi. 


### <a name="bkmk_zscopes"></a>Creare gli ambiti di zona
Un ambito di una zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona che contiene un proprio set di record DNS. Lo stesso record possono essere presenti in più ambiti, con diversi indirizzi IP o gli stessi indirizzi IP. 

>[!NOTE]
>Per impostazione predefinita, un ambito di una zona esistente nelle zone DNS. In questo ambito di zona ha lo stesso nome della zona e operazioni DNS legacy funzionano in questo ambito. 

È possibile usare il comando seguente per creare un ambito di zona per ospitare i record di Azure.

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

Per altre informazioni, vedere [Aggiungi DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records"></a>Aggiungere i record per gli ambiti di zona
Il passaggio successivo consiste nell'aggiungere i record che rappresenta l'host del server Web negli ambiti di zona. 

In AzureZoneScope, www.contosogiftservices.com il record viene aggiunto con l'indirizzo IP 192.68.31.44, che si trova nel cloud pubblico di Azure. 

Analogamente, nell'ambito predefinito zona \(contosogiftservices.com\), un record \(www.contosogiftservices.com\) viene aggiunto con l'indirizzo IP 192.68.30.2 del server Web in esecuzione in Seattle locale Data Center.

Nel secondo cmdlet riportato di seguito, il parametro – ZoneScope non è incluso. Per questo motivo, i record vengono aggiunti nel ZoneScope predefinito. 

Inoltre, la durata (TTL) del record per le macchine virtuali di Azure viene mantenuta nel gruppo 600 (10 minuti) in modo che il LDNS memorizzarlo nella cache per periodi prolungati - che generava un'interferenza con bilanciamento del carico. Inoltre, le VM di Azure sono disponibili per 1 ora extra di contingenza per garantire che i client anche con i record memorizzati nella cache sono in grado di risolvere.

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  

### <a name="bkmk_policies"></a>Creare i criteri DNS 
Dopo aver creati gli ambiti di zona, è possibile creare criteri DNS che distribuiscono le query in ingresso tra questi ambiti, in modo che si verifica quanto segue.

1. Da PM 6 a 9 PM di ogni giorno, 30% dei client ricevono l'indirizzo IP del server Web nel Data Center di Azure nella risposta DNS, mentre il 70% di client ricevono l'indirizzo IP del server Web locale Seattle.
2. A tutti gli altri casi, tutti i client ricevono l'indirizzo IP del server Web locale Seattle.

L'ora del giorno deve essere espresso nell'ora locale del server DNS.

È possibile usare il comando seguente per creare i criteri DNS.

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Ora il server DNS è configurato con i criteri necessari DNS per reindirizzare il traffico al server Web di Azure basati sull'ora del giorno. 

Si noti l'espressione:

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” 
`

Questa espressione consente di configurare il server DNS con una combinazione ZoneScope e peso che indica al server DNS per inviare l'indirizzo IP del server Web di Seattle fa quattrocentonovantadue per cento di tempo, durante l'invio del 30 percento del tempo l'indirizzo IP del server Web di Azure.

È possibile creare migliaia di criteri DNS in base del traffico di gestione e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, per le query in ingresso.
