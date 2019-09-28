---
title: Usare i criteri DNS per la gestione del traffico basata sulla geolocalizzazione con distribuzioni primarie-secondarie
description: Questo argomento fa parte della Guida allo scenario dei criteri DNS per Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6a7836160fc7363ec3d7b2fb11e194db82970f9a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406157"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>Usare i criteri DNS per la gestione del traffico basata sulla geolocalizzazione con distribuzioni primarie-secondarie

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come creare criteri DNS per la gestione del traffico basato su posizione geografica durante la distribuzione DNS include i server DNS primari e secondari.  

Nello scenario precedente, [usare i criteri DNS per la gestione del traffico basata sulla posizione geografica con i server primari](primary-geo-location.md), fornite istruzioni per la configurazione dei criteri DNS per la gestione del traffico basata sulla posizione geografica in un server DNS primario. Nell'infrastruttura di Internet, tuttavia, i server DNS sono ampiamente distribuiti in un modello primario, secondario, in cui la copia scrivibile di un'area di verrà archiviata nei server primario protette e selezionare e sola lettura della zona vengono conservate in più server secondari.   
  
Il server secondario utilizza i protocolli di trasferimento zona autorevole trasferimento (AXFR) e il trasferimento di zona incrementale (IXFR) per richiedere e ricevere gli aggiornamenti di zona che includono nuove modifiche apportate alle aree dei server DNS primari.   
  
> [!NOTE]
> Per ulteriori informazioni su AXFR, vedere Internet Engineering Task Force (IETF) [richiesta di commenti 5936](https://tools.ietf.org/rfc/rfc5936.txt). Per ulteriori informazioni su IXFR, vedere Internet Engineering Task Force (IETF) [richiesta di commenti 1995](https://tools.ietf.org/html/rfc1995).  
  
## <a name="primary-secondary-geo-location-based-traffic-management-example"></a>La posizione geografica primario secondario basato su esempio Gestione traffico  
Seguito è riportato un esempio di come è possibile utilizzare criteri DNS in una distribuzione primario secondario per ottenere il reindirizzamento del traffico in base al percorso fisico del client che esegue una query DNS.  
  
In questo esempio utilizza due società fittizia - servizi Cloud di Contoso, che fornisce web e dominio che ospita soluzioni. e servizi di ristorazione Woodgrove, che fornisce servizi per alimentare la distribuzione in più città in tutto il mondo e che dispone di un sito Web denominato woodgrove.com.  
  
Per garantire che i clienti woodgrove.com ottenere prestazioni ottimali dal relativo sito Web, Woodgrove desidera europee indirizzato al Data Center dell'Europa e americano client indirizzato al Data Center degli Stati Uniti. I clienti ubicati altrove nel mondo possono essere indirizzati a uno dei Data Center.  
  
Servizi Cloud di Contoso ha due Data Center, uno negli Stati Uniti e l'altro in Europa, in cui Contoso ospita il cibo ordinamento portale per woodgrove.com.  
  
La distribuzione DNS contoso include due server secondari: **SecondaryServer1**, con l'indirizzo IP 10.0.0.2; e **SecondaryServer2**, con l'indirizzo IP 10.0.0.3. Questi server secondari fungono da server dei nomi in due aree geografiche diverse, con SecondaryServer1 si trova in Europa e SecondaryServer2 si trova negli Stati Uniti
  
È disponibile una copia di zona primaria scrivibile in **PrimaryServer** (indirizzo IP 10.0.0.1), in cui vengono apportate le modifiche alla zona. Con i trasferimenti di zona normale per i server secondari, i server secondari sono sempre aggiornati con eventuali nuove modifiche all'area nel PrimaryServer.
  
Nella figura seguente viene illustrato questo scenario.
  
![La posizione geografica primario secondario basato su esempio Gestione traffico](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)  
   
## <a name="how-the-dns-primary-secondary-system-works"></a>Funzionamento del sistema primario secondario DNS

Quando si distribuisce gestione del traffico basato su posizione geografica in una distribuzione DNS primario, secondario, è importante comprendere come normale zona primaria secondaria prima di imparare i trasferimenti di zona ambito livello si verificano trasferimenti. Nelle sezioni seguenti vengono forniscono informazioni sulla zona e i trasferimenti di zona ambito livello.  
  
- [Trasferimenti di zona in una distribuzione primaria-secondaria DNS](#zone-transfers-in-a-dns-primary-secondary-deployment)  
- [Trasferimenti a livello di ambito di zona in una distribuzione primaria-secondaria DNS](#zone-scope-level-transfers-in-a-dns-primary-secondary-deployment)  
  
### <a name="zone-transfers-in-a-dns-primary-secondary-deployment"></a>Trasferimenti di zona in una distribuzione di DNS primario, secondario

È possibile creare una distribuzione di DNS primario, secondario e sincronizzare le zone con i passaggi seguenti.  
1. Quando si installa DNS, viene creata la zona primaria nel server DNS primario.  
2. Nel server secondario, creare le zone e specificare il server primario.   
3. Nel server primario, è possibile aggiungere i server secondari come attendibili secondari nella zona primaria.   
4. Le zone secondarie effettuare una richiesta di trasferimento di zona completo (AXFR) e riceveranno la copia della zona.   
5. Quando richiesto, i server primario di inviano notifiche per i server secondari sugli aggiornamenti di zona.  
6. Server secondario effettuare una richiesta di trasferimento di zona incrementale (IXFR). Per questo motivo, i server secondari rimangano sincronizzati con il server primario.   
  
### <a name="zone-scope-level-transfers-in-a-dns-primary-secondary-deployment"></a>Ambito livello i trasferimenti di zona in una distribuzione di DNS primario, secondario

Lo scenario di gestione del traffico richiede passaggi aggiuntivi per partizionare le zone in ambiti diversi zona. Per questo motivo, sono necessari per trasferire i dati all'interno di ambiti di zona per i server secondari e per trasferire i criteri e subnet del Client DNS per i server secondari passaggi aggiuntivi.   
  
Dopo aver configurato l'infrastruttura DNS con i server primari e secondari, i trasferimenti di zona ambito livello vengono eseguiti automaticamente da DNS, utilizzando i processi seguenti.  
  
Per garantire il trasferimento di livello di ambito di zona, server DNS, utilizzare i meccanismi di estensione per DNS (EDNS0) OPT RR. Tutte le richieste di trasferimento (AXFR o IXFR) fuso dalle aree con ambiti derivano da un EDNS0 OPT RR, il cui ID di opzione è impostata su "65433" per impostazione predefinita. Per ulteriori informazioni su EDNSO, vedere IETF [richiesta di commenti 6891](https://tools.ietf.org/html/rfc6891).  
  
Il valore di RR RIFIUTARE è il nome dell'ambito di zona per cui viene inviata la richiesta. Quando un server DNS primario riceve il pacchetto da un server secondario attendibile, interpreta la richiesta in arrivo per tale ambito di una zona.   
  
Se il server primario dispone di tale ambito di una zona risponde con i dati di trasferimento (XFR) da tale ambito. La risposta contiene un consenso ESPLICITO RR con lo stesso ID di opzione "65433" e valore impostato per lo stesso ambito di zona. I server secondari ricevano la risposta, recupero le informazioni sull'ambito dalla risposta e aggiornare quell ' ambito della zona.  
  
Dopo questo processo, il server primario mantiene un elenco dei database secondari attendibili che sono inviati a tale area ambito richiesta per le notifiche.   
  
Per qualsiasi ulteriore aggiornamento in un ambito di una zona, viene inviata una notifica IXFR per i server secondari, con il record di risorse OPT stesso. L'ambito di zona notifica che effettua la richiesta IXFR contenente tale RR OPT e segue lo stesso processo come descritto in precedenza.  
  
## <a name="how-to-configure-dns-policy-for-primary-secondary-geo-location-based-traffic-management"></a>Come configurare criteri di DNS per gestione del traffico basato su posizione geografica primaria secondaria

Prima di iniziare, assicurarsi di aver completato tutti i passaggi nell'argomento [utilizzare DNS criteri per la gestione del traffico in base a posizione geografica con i server primari](../../dns/deploy/Scenario--Use-DNS-Policy-for-Geo-Location-Based-Traffic-Management-with-Primary-Servers.md), e il server DNS primario è configurato con le zone, zona ambiti, subnet del Client DNS e criteri DNS.  
  
> [!NOTE]
> Le istruzioni in questo argomento per copiare subnet del Client DNS, gli ambiti di zona e i criteri DNS dal server primario DNS dei server DNS secondari sono per la configurazione DNS iniziale e la convalida. In futuro si potrebbe voler modificare la subnet del Client DNS, gli ambiti di zona e impostazioni dei criteri nel server primario. In questo caso, è possibile creare script di automazione per mantenere i server secondari sincronizzati con il server primario.  
  
Per configurare criteri DNS per le risposte alle query primaria secondaria la posizione geografica in base, è necessario eseguire la procedura seguente.  
  
- [Creare le zone secondarie](#create-the-secondary-zones)  
- [Configurare le impostazioni di trasferimento di zona nella zona primaria](#configure-the-zone-transfer-settings-on-the-primary-zone)  
- [Copiare le subnet del client DNS](#copy-the-dns-client-subnets)  
- [Creare gli ambiti di zona nel server secondario](#create-the-zone-scopes-on-the-secondary-server)  
- [Configurare i criteri DNS](#configure-dns-policy)  
  
Le sezioni seguenti forniscono le istruzioni di configurazione dettagliate.  
  
> [!IMPORTANT]
> Nelle sezioni seguenti includono esempi di comandi Windows PowerShell che contengono valori di esempio per numero di parametri. Assicurarsi di sostituire i valori di esempio in questi comandi con i valori appropriati per la distribuzione prima di eseguire questi comandi.  
> 
> L'appartenenza a **DnsAdmins**, o equivalente, è necessario per eseguire le procedure seguenti.  
  
### <a name="create-the-secondary-zones"></a>Creare le zone secondarie

È possibile creare la copia secondaria della zona da replicare SecondaryServer1 e SecondaryServer2 (presupponendo che i cmdlet vengono eseguiti in modalità remota da un client di gestione singolo).   
  
Ad esempio, è possibile creare la copia secondaria di www.woodgrove.com SecondaryServer1 e SecondarySesrver2.  
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per creare le zone secondarie.  
  
    
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1  
      
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2  
      

Per ulteriori informazioni, vedere [Aggiungi DnsServerSecondaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserversecondaryzone?view=win10-ps).  
  
### <a name="configure-the-zone-transfer-settings-on-the-primary-zone"></a>Configurare le impostazioni di trasferimento di zona per la zona primaria

È necessario configurare le impostazioni di zona primaria in modo che:

1. Sono consentiti i trasferimenti di zona dal server primario per il server secondario specificato.  
2. Le notifiche di aggiornamento zona vengono inviate dal server primario per i server secondari.  
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per configurare le impostazioni di trasferimento di zona in zona primaria.
  
> [!NOTE]
> Il comando di esempio seguente, il parametro **-notifica** indica che il server principale invia le notifiche sugli aggiornamenti all'elenco di selezione dei database secondari.  
  
    
    Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer  
     
  
Per ulteriori informazioni, vedere [DnsServerPrimaryZone Set](https://docs.microsoft.com/powershell/module/dnsserver/set-dnsserverprimaryzone?view=win10-ps).  
  
  
### <a name="copy-the-dns-client-subnets"></a>Copia la subnet del Client DNS

È necessario copiare la subnet del Client DNS dal server primario per i server secondari.
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per copiare le subnet per i server secondari.
  
    
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1  
      
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2  
      

Per ulteriori informazioni, vedere [Aggiungi DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="create-the-zone-scopes-on-the-secondary-server"></a>Creare gli ambiti di zona nel Server secondario

È necessario creare gli ambiti di zona sui server secondari. In DNS, gli ambiti di zona iniziano anche a richiedere XFRs dal server primario. Con le eventuali modifiche apportate gli ambiti di zona nel server primario, viene inviata una notifica contenente le informazioni sull'ambito di zona per i server secondari. I server secondari possono aggiornare i relativi ambiti zona con modifiche incrementali.  
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per creare gli ambiti di zona sui server secondari.  
  
    
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore  
      
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore  
  

> [!NOTE]
> In questi comandi di esempio, il **- ErrorAction ignora** è incluso, perché esiste un ambito di zona predefinito per ogni zona. L'ambito di zona predefinito non può essere creato o eliminato. Il pipelining comporterà un tentativo di creare tale ambito e avrà esito negativo. In alternativa, è possibile creare gli ambiti non predefiniti zona in due zone secondarie.  
  
Per ulteriori informazioni, vedere [Aggiungi DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="configure-dns-policy"></a>Configurare i criteri di DNS

Dopo aver creato le subnet, le partizioni (ambiti zona) ed è stato aggiunto record, è necessario creare criteri che si connettono le subnet e le partizioni, in modo che quando una query provenga da un'origine in una delle subnet dei client DNS, la risposta alla query verrà restituita dall'ambito corretto della zona. Criteri non sono necessari per il mapping tra l'ambito di orario predefinito.  
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per creare un criterio DNS che collega la subnet del Client DNS e gli ambiti di zona.   
    
    $policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2  
      

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Ora i server DNS secondari vengono configurati con i criteri necessari DNS per reindirizzare il traffico in base alla posizione geografica.  
  
Quando il server DNS riceve richieste di risoluzione dei nomi, il server DNS valuta i campi nella richiesta DNS con i criteri del DNS configurato. Se l'indirizzo IP di origine nella richiesta di risoluzione nome corrisponde a uno qualsiasi dei criteri, l'ambito delle zone associati verrà utilizzato per rispondere alla query e l'utente viene indirizzato alla risorsa che geograficamente più vicino.   
  
È possibile creare migliaia di criteri DNS in base del traffico di gestione e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, per le query in ingresso.
