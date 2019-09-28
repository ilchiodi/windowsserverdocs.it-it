---
title: Risposte DNS basate sull'ora del giorno con un server app Azure Cloud
description: Questo argomento fa parte della Guida allo scenario dei criteri DNS per Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4307ce1512980277af819e0710e0447d8dbac8c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406193"
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>Risposte DNS basate sull'ora del giorno con un server app Azure Cloud

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come distribuire il traffico dell'applicazione tra diverse istanze distribuite geograficamente di un'applicazione utilizzando i criteri DNS che si basano sull'ora del giorno. 

Questo scenario è utile nelle situazioni in cui si desidera indirizzare il traffico in un fuso orario a server applicazioni alternativi, ad esempio i server Web ospitati in Microsoft Azure, che si trovano in un altro fuso orario. Ciò consente di bilanciare il carico tra le istanze dell'applicazione durante il picco quando il server primario sono sottoposti a overload con traffico periodi di tempo. 

> [!NOTE]
> Per informazioni su come usare i criteri DNS per le risposte DNS intelligenti senza usare Azure, vedere [usare i criteri DNS per le risposte DNS intelligenti in base all'ora del giorno](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md). 

## <a name="example-of-intelligent-dns-responses-based-on-the-time-of-day-with-azure-cloud-app-server"></a>Esempio di risposte DNS intelligenti basate sull'ora del giorno con il server app cloud di Azure

Seguito è riportato un esempio di come è possibile utilizzare criteri DNS per bilanciare il traffico dell'applicazione in base all'ora del giorno.

Questo esempio viene utilizzata una società fittizia, Contoso regalo servizi, che fornisce soluzioni gifting online in tutto il mondo tramite il sito Web, contosogiftservices.com. 

Il sito Web contosogiftservices.com è ospitato solo in un singolo Data Center locale a Seattle (con IP pubblico 192.68.30.2). 

Anche il server DNS si trova nel Data Center locale. 

Con un picco recenti nel settore, contosogiftservices.com ha un numero elevato di visitatori ogni giorno e alcuni utenti hanno segnalato problemi di disponibilità del servizio. 

Contoso Gift Services esegue un'analisi del sito e rileva che ogni sera tra le 18.00 e le 21.00 ora locale è presente un aumento del traffico verso il server Web di Seattle. Il server Web non può essere ridimensionato in modo da gestire l'aumento del traffico in queste ore di picco, causando un attacco Denial of Service ai clienti. 

Per garantire che i clienti di contosogiftservices.com ottengano un'esperienza reattiva dal sito Web, contoso Gift Services decide che durante tali ore verrà affittata una macchina virtuale \(VM @ no__t-1 in Microsoft Azure per ospitare una copia del server Web.  

Contoso Gift Services ottiene un indirizzo IP pubblico da Azure per la macchina virtuale (192.68.31.44) e sviluppa l'automazione per distribuire il server Web ogni giorno in Azure tra 5-10 PM, consentendo un periodo di emergenza di un'ora.

> [!NOTE]
> Per ulteriori informazioni sulle VM di Azure, vedere la [documentazione relativa alle macchine virtuali](https://azure.microsoft.com/documentation/services/virtual-machines/) 

I server DNS sono configurati con gli ambiti di zona e i criteri DNS, in modo che tra 5-9 PM ogni giorno il 30% delle query venga inviato all'istanza del server Web in esecuzione in Azure.

Nella figura seguente viene illustrato questo scenario.

![Criteri DNS per le risposte ora del giorno](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)  

## <a name="how-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server-works"></a>Come funzionano le risposte DNS intelligenti basate sull'ora del giorno con app Azure server
 
Questo articolo illustra come configurare il server DNS per rispondere alle query DNS con due diversi indirizzi IP del server applicazioni: un server Web si trova a Seattle e l'altro si trova in un Data Center di Azure.

Dopo la configurazione di un nuovo criterio DNS che si basa sulle ore di picco dalle 18.00 alle 21.00 di Seattle, il server DNS invia il 70% delle risposte DNS ai client contenenti l'indirizzo IP del server Web Seattle e il 30% delle risposte DNS a AG NTS che contiene l'indirizzo IP del server Web di Azure, indirizzando quindi il traffico client al nuovo server Web di Azure e impedendo che il server Web di Seattle diventi sovraccarico. 

In tutti gli altri orari del giorno viene eseguita la normale elaborazione delle query e le risposte vengono inviate dall'ambito di zona predefinito che contiene un record per il server Web nel Data Center locale. 

Il valore TTL di 10 minuti nel record di Azure garantisce che il record sia scaduto dalla cache LDNS prima che la macchina virtuale venga rimossa da Azure. Uno dei vantaggi di tale scalabilità è che è possibile proteggere i dati DNS in locale e aumentare la scalabilità orizzontale in Azure come richiesto dalla richiesta.

## <a name="how-to-configure-dns-policy-for-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server"></a>Come configurare i criteri DNS per le risposte DNS intelligenti in base all'ora del giorno con app Azure server

Per configurare criteri DNS per le risposte di ora del giorno applicazione bilanciamento del carico basato su query, è necessario eseguire la procedura seguente.

- [Creare gli ambiti di zona](#create-the-zone-scopes)
- [Aggiungere record agli ambiti di zona](#add-records-to-the-zone-scopes)
- [Creazione dei criteri DNS](#create-the-dns-policies)

> [!NOTE]
> È necessario eseguire questi passaggi nel server DNS autorevole per la zona in cui che si desidera configurare. Per eseguire le procedure seguenti, è necessaria l'appartenenza a DnsAdmins o a un gruppo equivalente. 

Le sezioni seguenti forniscono le istruzioni di configurazione dettagliate.

> [!IMPORTANT]
> Nelle sezioni seguenti includono esempi di comandi Windows PowerShell che contengono valori di esempio per numero di parametri. Assicurarsi di sostituire i valori di esempio in questi comandi con i valori appropriati per la distribuzione prima di eseguire questi comandi. 


### <a name="create-the-zone-scopes"></a>Creare gli ambiti di zona

Un ambito di una zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona contenente il proprio set di record DNS. Lo stesso record può essere presente in più ambiti, con indirizzi IP diversi o con gli stessi indirizzi IP. 

> [!NOTE]
> Per impostazione predefinita, un ambito di una zona esistente nelle zone DNS. Questo ambito di zona ha lo stesso nome della zona e le operazioni DNS legacy funzionano in questo ambito. 

È possibile usare il comando di esempio seguente per creare un ambito di zona per ospitare i record di Azure.

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

Per ulteriori informazioni, vedere [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="add-records-to-the-zone-scopes"></a>Aggiungere record per gli ambiti di zona
Il passaggio successivo consiste nell'aggiungere i record che rappresentano l'host del server Web negli ambiti di zona. 

In AzureZoneScope, il record www.contosogiftservices.com viene aggiunto con l'indirizzo IP 192.68.31.44, che si trova nel cloud pubblico di Azure. 

Analogamente, nell'ambito di zona predefinito @no__t -0contosogiftservices. com @ no__t-1, viene aggiunto un record @no__t -2www. contosogiftservices. com @ no__t-3 con indirizzo IP 192.68.30.2 del server Web in esecuzione nel Data Center locale di Seattle.

Nel secondo cmdlet riportato di seguito, il parametro – ZoneScope non è incluso. Per questo motivo, i record vengono aggiunti nel ZoneScope predefinito. 

Inoltre, la durata (TTL) del record per le macchine virtuali di Azure viene mantenuta a 600S (10 minuti), in modo che il LDNS non la memorizza nella cache per un periodo di tempo più lungo, che interferisce con il bilanciamento del carico. Inoltre, le macchine virtuali di Azure sono disponibili per un'ora aggiuntiva come emergenza per garantire che anche i client con record memorizzati nella cache siano in grado di risolvere.

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  

### <a name="create-the-dns-policies"></a>Creare i criteri DNS 
Dopo la creazione degli ambiti di zona, è possibile creare criteri DNS che distribuiscono le query in ingresso in questi ambiti in modo che si verifichi quanto segue.

1. Dalle 18.00 alle 9.00 al giorno, il 30% dei client riceve l'indirizzo IP del server Web nel Data Center di Azure nella risposta DNS, mentre il 70% dei client riceve l'indirizzo IP del server Web locale di Seattle.
2. In tutti gli altri casi, tutti i client ricevono l'indirizzo IP del server Web locale di Seattle.

L'ora del giorno deve essere espressa nell'ora locale del server DNS.

È possibile usare il comando di esempio seguente per creare i criteri DNS.

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Il server DNS è ora configurato con i criteri DNS necessari per reindirizzare il traffico al server Web di Azure in base all'ora del giorno. 

Si noti l'espressione:

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” 
`

Questa espressione configura il server DNS con una combinazione di ZoneScope e Weight che indica al server DNS di inviare l'indirizzo IP del server Web Seattle 70 per cento del tempo, durante l'invio dell'indirizzo IP del server Web di Azure per il 30% del tempo.

È possibile creare migliaia di criteri DNS in base del traffico di gestione e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, per le query in ingresso.
