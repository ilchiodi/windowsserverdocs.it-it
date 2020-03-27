---
title: Usare i criteri DNS per l'applicazione del bilanciamento del carico
description: Questo argomento fa parte della Guida allo scenario dei criteri DNS per Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 86ce83142cafe8ebe61aff2fb193e9b646172651
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317880"
---
# <a name="use-dns-policy-for-application-load-balancing"></a>Usare i criteri DNS per l'applicazione del bilanciamento del carico

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come configurare i criteri DNS per eseguire il bilanciamento del carico dell'applicazione.

Le versioni precedenti del DNS di Windows Server fornivano solo il bilanciamento del carico tramite risposte round robin; Tuttavia, con DNS in Windows Server 2016, è possibile configurare i criteri DNS per il bilanciamento del carico dell'applicazione.

Quando sono state distribuite più istanze di un'applicazione, è possibile usare i criteri DNS per bilanciare il carico del traffico tra le diverse istanze dell'applicazione, allocando in modo dinamico il carico del traffico per l'applicazione.

## <a name="example-of-application-load-balancing"></a>Esempio di bilanciamento del carico dell'applicazione

Di seguito è riportato un esempio di come è possibile usare i criteri DNS per il bilanciamento del carico dell'applicazione.

Questo esempio usa una società fittizia, contoso Gift Services, che fornisce servizi gifing online e che dispone di un sito Web denominato **contosogiftservices.com**.

Il sito Web di contosogiftservices.com è ospitato in più data center che dispongono di indirizzi IP diversi.

In America del Nord, ovvero il mercato primario per contoso Gift Services, il sito Web è ospitato in tre data center: Chicago, IL, Dallas, TX e Seattle, WA.

Il server Web Seattle dispone della migliore configurazione hardware e può gestire il carico doppio degli altri due siti. Contoso Gift Services desidera che il traffico delle applicazioni venga indirizzato nel modo seguente.

- Poiché il server Web di Seattle include più risorse, la metà dei client dell'applicazione viene indirizzata a questo server
- Un trimestre dei client dell'applicazione viene indirizzato a Dallas, TX Datacenter
- Un trimestre dei client dell'applicazione viene indirizzato a Chicago, IL, Datacenter

Nella figura seguente viene illustrato questo scenario.

![Bilanciamento del carico dell'applicazione DNS con criteri DNS](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>Funzionamento del bilanciamento del carico delle applicazioni

Dopo aver configurato il server DNS con criteri DNS per il bilanciamento del carico dell'applicazione utilizzando questo scenario di esempio, il server DNS risponde 50% del tempo con l'indirizzo del server Web Seattle, il 25% del tempo con l'indirizzo del server Web Dallas e il 25% del tempo con Indirizzo del server Web di Chicago.

Pertanto, per ogni quattro query ricevute dal server DNS, risponde con due risposte per Seattle e una per Dallas e Chicago.

Un possibile problema con il bilanciamento del carico con criteri DNS è la memorizzazione nella cache dei record DNS da parte del client DNS e del resolver/LDNS, che possono interferire con il bilanciamento del carico perché il client o il resolver non inviano una query al server DNS.

Per attenuare l'effetto di questo comportamento, è possibile usare un\-di tempo ridotto per\-il valore\) Live \(TTL per i record DNS che devono essere sottoposte a bilanciamento del carico.

### <a name="how-to-configure-application-load-balancing"></a>Come configurare il bilanciamento del carico dell'applicazione

Le sezioni seguenti illustrano come configurare i criteri DNS per il bilanciamento del carico dell'applicazione.

#### <a name="create-the-zone-scopes"></a>Creare gli ambiti di zona

È innanzitutto necessario creare gli ambiti della zona contosogiftservices.com per i Data Center in cui sono ospitati.

Un ambito di una zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona contenente il proprio set di record DNS. Lo stesso record può essere presente in più ambiti, con indirizzi IP diversi o con gli stessi indirizzi IP.

>[!NOTE]
>Per impostazione predefinita, un ambito di una zona esistente nelle zone DNS. Questo ambito di zona ha lo stesso nome della zona e le operazioni DNS legacy funzionano in questo ambito.

È possibile utilizzare i seguenti comandi di Windows PowerShell per creare ambiti di zona.
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

Per ulteriori informazioni, vedere [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

#### <a name="add-records-to-the-zone-scopes"></a><a name="bkmk_records"></a>Aggiungere record agli ambiti di zona

A questo punto è necessario aggiungere i record che rappresentano l'host del server Web negli ambiti di zona.

In **SeattleZoneScope**è possibile aggiungere il record www.contosogiftservices.com con l'indirizzo IP 192.0.0.1 che si trova nel Data Center di Seattle.

In **ChicagoZoneScope**è possibile aggiungere lo stesso record \(www.contosogiftservices.com\) con indirizzo IP 182.0.0.1 nel Data Center di Chicago.

Analogamente, in **DallasZoneScope**è possibile aggiungere un record \(www.contosogiftservices.com\) con indirizzo IP 162.0.0.1 nel Data Center di Chicago.

È possibile utilizzare i seguenti comandi di Windows PowerShell per aggiungere record per gli ambiti di zona.
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

#### <a name="create-the-dns-policies"></a><a name="bkmk_policies"></a>Creazione dei criteri DNS

Dopo aver creato le partizioni (ambiti di zona) ed è stato aggiunto un record, è necessario creare criteri DNS per la distribuzione delle query in ingresso in questi ambiti in modo che il 50% delle query per contosogiftservices.com venga risposto con l'indirizzo IP per il Web il server nel Data Center di Seattle e il resto vengono distribuiti equamente tra i Data Center di Chicago e Dallas.

È possibile usare i comandi seguenti di Windows PowerShell per creare un criterio DNS che bilancia il traffico delle applicazioni tra questi tre Data Center.

>[!NOTE]
>Nel comando di esempio seguente, espressione – ZoneScope "SeattleZoneScope, 2; ChicagoZoneScope, 1; DallasZoneScope, 1 "Configura il server DNS con una matrice che include la combinazione di parametri \<ZoneScope\>,\<Weight\>.
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  

A questo punto è stato creato un criterio DNS che fornisce il bilanciamento del carico dell'applicazione tra server Web in tre diversi Data Center.

È possibile creare migliaia di criteri DNS in base del traffico di gestione e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, per le query in ingresso.
