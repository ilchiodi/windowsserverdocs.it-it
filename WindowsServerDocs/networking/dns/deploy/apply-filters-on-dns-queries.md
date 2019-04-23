---
title: Usare i criteri DNS per l'applicazione di filtri alle query DNS
description: Questo argomento fa parte del DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b00c773462569f005a73f535b1a872ae7b389db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859902"
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>Usare i criteri DNS per l'applicazione di filtri alle query DNS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come configurare criteri DNS in Windows Server&reg; 2016 per creare filtri per query sono basati su criteri che viene fornito. 

Filtri di query in Criteri di DNS consentono di configurare il server DNS per rispondere in modo personalizzato in base alla query DNS e client DNS che invia la query DNS.

Ad esempio, è possibile configurare criteri DNS con query filtro elenco di blocchi che blocca le query DNS da domini dannosi noti, che impedisce di DNS di rispondere alle query da questi domini. Poiché viene inviata alcuna risposta dal server DNS, query DNS del dominio dannoso membro verifica il timeout.

Un altro esempio consiste nel creare un filtro di query elenco Consenti che consente solo un set specifico di client per risolvere determinati nomi.

## <a name="bkmk_criteria"></a> Criteri di filtro query
È possibile creare filtri di query con qualsiasi combinazione logica (e/o/non) dei criteri seguenti.

|Nome|Descrizione|
|-----------------|---------------------|
|Subnet del client|Nome di una subnet del client predefinito. Utilizzato per verificare la subnet da cui la query è stata inviata.|
|Protocollo di trasporto|Il trasporto di protocollo utilizzato nella query. I valori possibili sono TCP e UDP.|
|Internet Protocol|Protocollo di rete utilizzato nella query. I valori possibili sono IPv4 e IPv6.|
|Indirizzo IP dell'interfaccia del server|Indirizzo IP dell'interfaccia di rete del server DNS che ha ricevuto la richiesta DNS.|
|FQDN|Nome di dominio completo del record nella query, con la possibilità di usare un carattere jolly.|
|Tipo di query|Tipo di record sottoposto \(A, SRV, TXT, ecc.\).|
|Ora del giorno|Ora del giorno che viene ricevuta la query.|

Negli esempi seguenti mostrano come creare filtri per i criteri DNS che uno dei blocchi o consentire a query la risoluzione dei nomi DNS.

>[!NOTE]
>I comandi di esempio in questo argomento usano il comando di Windows PowerShell **Aggiungi DnsServerQueryResolutionPolicy**. Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps). 

##<a name="bkmk_block1"></a>Query di blocco da un dominio

In alcuni casi si potrebbe voler bloccare la risoluzione dei nomi DNS per i domini che è stata identificata come dannoso o per i domini che non rispettano le linee guida sull'utilizzo della propria organizzazione. È possibile eseguire query di blocco per i domini tramite criteri di DNS.

I criteri che si configurano in questo esempio non viene creato in una determinata zona, è invece necessario creare un criterio a livello di Server che viene applicato a tutte le zone configurate nel server DNS. I criteri a livello di server sono il primo da valutare e pertanto prima di tutto per cui trovare una corrispondenza quando una query viene ricevuto dal server DNS.

Il comando seguente configura un criterio a livello di Server per bloccare tutte le query con il dominio **suffisso contosomalicious.com**.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>Quando si configura il **azione** parametro con il valore **IGNORE**, il server DNS è configurato per eliminare le query senza risposta affatto. In questo modo il client DNS del dominio dannoso timeout.

##<a name="bkmk_block2"></a>Query di blocco da una subnet
Con questo esempio, è possibile bloccare le query da una subnet se risulta essere infettati da alcuni tipi di malware e viene eseguito il tentativo di contattare i siti dannosi usando il server DNS. 

` Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru

Aggiungi DnsServerQueryResolutionPolicy-denominare "BlockListPolicyMalicious06"-azione Ignora - ClientSubnet "EQ, MaliciousSubnet06" - PassThru '

Nell'esempio seguente viene illustrato come è possibile usare i criteri di subnet in combinazione con i criteri di nome di dominio completo per bloccare le query per determinati domini dannosi da subnet infette.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

##<a name="bkmk_block3"></a>Blocco di un tipo di query
Potrebbe essere necessario bloccare la risoluzione dei nomi per determinati tipi di query sui server. Ad esempio, è possibile bloccare la query 'ANY', che possa essere usata da utenti malintenzionati per creare attacchi amplificazione.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

##<a name="bkmk_allow1"></a>Consenti query su solo da un dominio
Non è possibile utilizzare solo i criteri DNS alle query di blocco, è possibile usarli per approvare automaticamente le query verso subnet o domini specifici. Quando si configura consentire sono elencate, il server DNS elabora solo le query da domini consentiti, durante il blocco di tutte le altre query dagli altri domini.

Il comando seguente consente solo i computer e dispositivi in domini di contoso.com e figlio per eseguire una query server DNS.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

##<a name="bkmk_allow2"></a>Consenti query su solo da una subnet
È possibile anche creare elenchi Consenti per le subnet IP, in modo che tutte le query non ha origine da queste subnet vengono ignorate.

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

##<a name="bkmk_allow3"></a>Consentire solo determinate QTypes
È possibile applicare elenchi Consenti a QTYPEs. 

Ad esempio, se si dispone di una query di interfaccia di server DNS 164.8.1.1 ai clienti esterni, sono consentiti solo determinati QTYPEs essere sottoposti a query, anche se esistono altri QTYPEs come i record SRV o TXT che vengono utilizzati dai server interno per la risoluzione dei nomi o a scopo di monitoraggio.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

È possibile creare migliaia di criteri DNS in base del traffico di gestione e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, per le query in ingresso. 
