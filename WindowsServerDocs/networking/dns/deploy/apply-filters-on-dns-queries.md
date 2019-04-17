---
title: Usare i criteri DNS per l'applicazione di filtri alle query DNS
description: In questo argomento fa parte di DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ff10af5f88a03f806a5e5b5fa698c7c3637816bd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>Usare i criteri DNS per l'applicazione di filtri alle query DNS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come configurare criteri DNS in Windows Server&reg; 2016 per creare filtri query basate su criteri specificati. 

Filtri di query in Criteri di DNS consentono di configurare il server DNS per rispondere in modo personalizzato in base alla query DNS e client DNS che invia la query DNS.

Ad esempio, è possibile configurare criteri DNS con query filtro elenco di blocco che blocca le query DNS da domini dannosi noti, che impedisce che risponde alle query da questi domini DNS. Poiché viene inviata alcuna risposta dal server DNS, del membro dominio dannoso DNS timeout.

Un altro esempio consiste nel creare un filtro di query elenco Consenti che consente solo un set specifico di client per risolvere determinati nomi.

## <a name="bkmk_criteria"></a>Criteri di filtro di query
I criteri seguenti, è possibile creare filtri di query con una qualsiasi combinazione logico (e/o/non).

|Nome|Descrizione|
|-----------------|---------------------|
|Subnet del client|Nome di una subnet del client predefinito. Usato per verificare la subnet da cui è stata inviata la query.|
|Protocollo di trasporto|Il trasporto di protocollo utilizzato nella query. Valori possibili sono TCP e UDP.|
|Protocollo Internet|Protocollo di rete utilizzato nella query. Valori possibili sono IPv4 e IPv6.|
|Indirizzo IP dell'interfaccia del server|Indirizzo IP dell'interfaccia di rete del server DNS che ha ricevuto la richiesta DNS|
|NOME DI DOMINIO COMPLETO|Nome di dominio completo del record nella query, con la possibilità di usare un carattere jolly.|
|Tipo di query|Tipo di record sottoposto a query \ (A, SRV, TXT e così via. \)|
|Ora del giorno|Ora del giorno che viene ricevuta la query.|

Nell'esempio seguente mostrano come creare filtri per i criteri DNS che uno dei blocchi o consentire query di risoluzione dei nomi DNS.

>[!NOTE]
>I comandi di esempio in questo argomento utilizzano il comando di Windows PowerShell **Aggiungi DnsServerQueryResolutionPolicy**. Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx). 

##<a name="bkmk_block1"></a>Query di blocco da un dominio

In alcuni casi, si desidera bloccare la risoluzione dei nomi DNS per i domini che hai identificato come dannoso, o per i domini che non sono contravvengano alle linee guida per l'utilizzo dell'organizzazione. È possibile eseguire query di blocca per i domini tramite criteri di DNS.

I criteri che è configurato in questo esempio non viene creato in una determinata zona, è invece necessario creare un criterio a livello Server che viene applicata a tutte le zone configurate nel server DNS. Criteri a livello di server sono il primo a essere valutata e pertanto prima di tutto per corrispondere quando una query viene ricevuto dal server DNS.

Il comando seguente consente di configurare un criterio a livello Server per bloccare tutte le query con il dominio **suffisso contosomalicious.com**.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>Quando si configura il **azione** parametro con il valore **ignora**, il server DNS è configurato per eliminare le query senza risposta affatto. In questo modo il client DNS nel dominio dannoso per timeout.

##<a name="bkmk_block2"></a>Query di blocco da una subnet
Con questo esempio, è possibile bloccare le query provenienti da una subnet, se viene trovato sia stato infettato da alcuni malware e tenta di contattare i siti dannosi con il server DNS. 

' Aggiungere DnsServerClientSubnet-nome "MaliciousSubnet06"-IPv4Subnet 172.0.33.0/24 - PassThru

Aggiungere DnsServerQueryResolutionPolicy-nome "BlockListPolicyMalicious06"-azione Ignora - ClientSubnet "EQ, MaliciousSubnet06" - PassThru '

L'esempio seguente illustra come è possibile utilizzare i criteri di subnet in combinazione con i criteri di nome di dominio completo per bloccare le query per determinati domini dannosi dalla subnet infetta.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

##<a name="bkmk_block3"></a>Blocco di un tipo di query
Potrebbe essere necessario bloccare la risoluzione dei nomi per determinati tipi di query sui server. Ad esempio, è possibile bloccare la query 'Qualsiasi', che possa essere utilizzata per creare attacchi amplificazione.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

##<a name="bkmk_allow1"></a>Consenti query solo da un dominio
Non è possibile utilizzare solo i criteri DNS alle query di blocco, di utilizzarli per approvare automaticamente le query a domini specifici o subnet. Quando si configura consentire Elenca, il server DNS elabora solo le query di domini consentiti, bloccando tutte le altre richieste di altri domini.

Il comando seguente consente solo i computer e dispositivi in domini di contoso.com e figlio per eseguire una query il server DNS.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

##<a name="bkmk_allow2"></a>Consenti query solo da una subnet
È possibile inoltre creare elenchi Consenti per le subnet IP, in modo che tutte le query non provenienti da queste subnet vengono ignorate.

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

##<a name="bkmk_allow3"></a>Consentire solo a determinati QTypes
È possibile applicare consentire Elenca per QTYPEs. 

Ad esempio, se si dispone di una query di interfaccia di server DNS 164.8.1.1 di clienti esterni, eseguire una query, anche se esistono altri QTYPEs Analogamente ai record SRV o TXT che vengono utilizzati da server interni per la risoluzione dei nomi o a scopo di monitoraggio sono consentiti solo determinati QTYPEs.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

È possibile creare migliaia di criteri DNS in base del traffico di gestione, e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, su query in ingresso. 
