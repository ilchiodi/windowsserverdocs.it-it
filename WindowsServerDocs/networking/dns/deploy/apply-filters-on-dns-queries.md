---
title: Usare i criteri DNS per l'applicazione di filtri alle query DNS
description: Questo argomento fa parte della Guida allo scenario dei criteri DNS per Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 95b68995326dc3d3bf48ca36caa9b2ab4923a7c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406206"
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>Usare i criteri DNS per l'applicazione di filtri alle query DNS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come configurare i criteri DNS in Windows&reg; server 2016 per creare filtri di query in base ai criteri forniti. 

I filtri query nei criteri DNS consentono di configurare il server DNS per rispondere in modo personalizzato in base alla query DNS e al client DNS che invia la query DNS.

È ad esempio possibile configurare criteri DNS con elenco Blocca filtro query che blocca le query DNS da domini dannosi noti, evitando che il DNS risponda alle query di questi domini. Poiché non viene inviata alcuna risposta dal server DNS, si verifica il timeout della query DNS del membro del dominio dannoso.

Un altro esempio è la creazione di un elenco di Consenti filtro query che consente solo a un set specifico di client di risolvere determinati nomi.

## <a name="bkmk_criteria"></a>Criteri di filtro query
È possibile creare filtri di query con qualsiasi combinazione logica (e/o/senza) dei criteri seguenti.

|Nome|Descrizione|
|-----------------|---------------------|
|Subnet client|Nome di una subnet del client predefinito. Utilizzato per verificare la subnet da cui la query è stata inviata.|
|Protocollo di trasporto|Il trasporto di protocollo utilizzato nella query. I valori possibili sono UDP e TCP.|
|Protocollo Internet|Protocollo di rete utilizzato nella query. I valori possibili sono IPv4 e IPv6.|
|Indirizzo IP dell'interfaccia server|Indirizzo IP dell'interfaccia di rete del server DNS che ha ricevuto la richiesta DNS.|
|FQDN|Nome di dominio completo del record nella query, con la possibilità di usare un carattere jolly.|
|Tipo di query|Tipo di record su cui viene \(eseguita la query A, SRV, txt\)e così via.|
|Ora del giorno|Ora del giorno che viene ricevuta la query.|

Gli esempi seguenti illustrano come creare filtri per i criteri DNS che bloccano o consentono query di risoluzione dei nomi DNS.

>[!NOTE]
>Nei comandi di esempio di questo argomento viene usato il comando di Windows PowerShell **Add-DnsServerQueryResolutionPolicy**. Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps). 

## <a name="bkmk_block1"></a>Blocca le query da un dominio

In alcuni casi potrebbe essere necessario bloccare la risoluzione dei nomi DNS per i domini che sono stati identificati come dannosi o per i domini che non sono conformi alle linee guida di utilizzo dell'organizzazione. È possibile eseguire query di blocco per i domini usando i criteri DNS.

I criteri configurati in questo esempio non vengono creati in una particolare zona, bensì si creano criteri a livello di server applicati a tutte le zone configurate nel server DNS. I criteri a livello di server sono i primi a essere valutati e pertanto devono essere prima di tutto corrispondenti quando una query viene ricevuta dal server DNS.

Il comando di esempio seguente configura un criterio a livello di server per bloccare qualsiasi query con il **suffisso**di dominio contosomalicious.com.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>Quando si configura il parametro dell' **azione** con il valore **Ignore**, il server DNS è configurato in modo da eliminare le query senza alcuna risposta. In questo modo si verificherà il timeout del client DNS nel dominio dannoso.

## <a name="bkmk_block2"></a>Bloccare le query da una subnet
Con questo esempio, è possibile bloccare le query da una subnet se è stato rilevato che è stato infettato da malware e si sta tentando di contattare siti dannosi usando il server DNS. 

' Add-DnsServerClientSubnet-Name "MaliciousSubnet06"-IPv4Subnet 172.0.33.0/24-PassThru

Add-DnsServerQueryResolutionPolicy-Name "BlockListPolicyMalicious06"-Action IGNOre-ClientSubnet "EQ, MaliciousSubnet06"-PassThru '

Nell'esempio seguente viene illustrato come è possibile utilizzare i criteri di subnet in combinazione con i criteri FQDN per bloccare le query per determinati domini dannosi da subnet infette.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

## <a name="bkmk_block3"></a>Blocca un tipo di query
Potrebbe essere necessario bloccare la risoluzione dei nomi per determinati tipi di query sui server. Ad esempio, è possibile bloccare la query ' ANY ', che può essere usata in modo dannoso per creare attacchi di amplificazione.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

## <a name="bkmk_allow1"></a>Consenti query solo da un dominio
Non solo è possibile usare i criteri DNS per bloccare le query, ma è possibile usarli per approvare automaticamente le query da domini o subnet specifici. Quando si configurano gli elenchi Consenti, il server DNS elabora solo le query da domini consentiti, bloccando tutte le altre query da altri domini.

Il comando di esempio seguente consente solo ai computer e ai dispositivi nei domini contoso.com e figlio di eseguire query sul server DNS.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

## <a name="bkmk_allow2"></a>Consenti solo query da una subnet
È anche possibile creare elenchi Consenti per subnet IP, in modo che tutte le query che non provengono da tali subnet vengano ignorate.

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

## <a name="bkmk_allow3"></a>Consenti solo determinati QTypes
È possibile applicare gli elenchi Consenti a QTYPEs. 

Se, ad esempio, sono presenti clienti esterni che eseguono query sull'interfaccia del server DNS 164.8.1.1, è possibile eseguire query solo su determinati QTYPEs, mentre sono presenti altri QTYPEs come i record SRV o TXT usati dai server interni per la risoluzione dei nomi o per scopi di monitoraggio.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

È possibile creare migliaia di criteri DNS in base del traffico di gestione e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, per le query in ingresso. 
