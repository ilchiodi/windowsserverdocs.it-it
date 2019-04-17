---
title: What's New in Gestione indirizzi IP
description: Questo argomento descrive le funzionalità di gestione indirizzi IP (IPAM) nuove sono modificate in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f2f2f1a5-ac2f-41b7-a495-98ad0e2a9b20
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b90cd1ab223e38cbf5933b58a594b32d5e3d4858
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-ipam"></a>What's New in Gestione indirizzi IP

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento descrive le funzionalità di gestione indirizzi IP (IPAM) nuove sono modificate in Windows Server 2016.  
  
Gestione indirizzi IP offre funzionalità amministrative e di monitoraggio altamente personalizzabili per l'indirizzo IP e l'infrastruttura DNS in una rete aziendale o del Provider di servizi Cloud (CSP). Puoi monitorare, controllare e gestire server che eseguono Dynamic Host Configuration Protocol (DHCP) e del sistema DNS (Domain Name) utilizzando Gestione indirizzi IP.  
  
## <a name="BKMK_IPAM2012R2"></a>Aggiornamenti nel Server di gestione indirizzi IP  
Di seguito sono le funzionalità nuove e migliorate per gestione indirizzi IP in Windows Server 2016.  
  
|Funzionalità|Nuove o migliorate|Descrizione|  
|--------------------------|-------------------|---------------|  
|[Gestione indirizzi IP avanzata](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|Migliorato|Le funzionalità di gestione indirizzi IP sono migliorate per scenari come la gestione delle subnet /32 IPv4 e IPv6 /128 e ricerca gratuita subnet di indirizzi IP e gli intervalli in un blocco di indirizzi IP.|  
|[Gestione avanzata del servizio DNS](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|Nuovo|Gestione indirizzi IP supporta i record di risorse DNS, server d'inoltro condizionale e gestione delle zone DNS per i server DNS integrate in Active Directory e file di backup appartenenti a un dominio.|  
|[Integrata DNS, DHCP e degli indirizzi IP management (DDI)](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|Migliorato|Diverse nuove esperienze e gestione del ciclo di vita integrata sono abilitate le operazioni, ad esempio per visualizzare tutti i record DNS che riguardano un indirizzo IP, automatizzati inventario degli indirizzi IP in base al record di risorse DNS e gestione del ciclo di vita degli indirizzi IP per le operazioni sia DNS e DHCP.|  
|[Supporto di più foreste Active Directory](#bkmk_ad)|Nuovo|È possibile utilizzare Gestione indirizzi IP per gestire i server DNS e DHCP di più foreste di Active Directory quando esiste una relazione di trust bidirezionale tra la foresta in cui è installato Gestione indirizzi IP e ogni insieme di strutture remoti.|  
|[Elimina dati di utilizzo](#bkmk_purge)|Nuovo|È ora possibile ridurre le dimensioni del database di gestione indirizzi IP eliminando i dati di utilizzo degli indirizzi IP che risalgono a più di una data specificata.|  
|[Supporto di Windows PowerShell per controllo dell'accesso basato su ruoli](#bkmk_ps)|Nuovo|È possibile utilizzare Windows PowerShell per impostare gli ambiti di accesso per gli oggetti di gestione indirizzi IP.|  
  
### <a name="EIP"></a>Gestione indirizzi IP avanzata  
Le funzionalità seguenti migliorano le funzionalità di gestione indirizzi di gestione indirizzi IP.  
>[!NOTE]
>Per riferimento ai comandi di Windows PowerShell di gestione indirizzi IP, vedere [cmdlet di Server di gestione indirizzi IP (IPAM) in Windows PowerShell](https://technet.microsoft.com/library/jj553807.aspx).  
  
#### <a name="support-for-31-32-and-128-subnets"></a>Supporto per /31 e subnet /32 /128 subnet  
Gestione indirizzi IP in Windows Server 2016 ora supporta /31 e subnet /32 /128 subnet. Ad esempio, una subnet di due indirizzi (/ 31 IPv4) potrebbe essere necessario per un collegamento point to point tra switch. Alcune opzioni potrebbero inoltre richiedere singolo indirizzo di loopback (/ 32 per IPv4, /128 per IPv6).  
  
#### **<a name="find-free-subnets-with-find-ipamfreesubnet"></a>Ricerca gratuita subnet con Trova IpamFreeSubnet**  
  
Questo comando restituisce le subnet che sono disponibili per l'allocazione, dato un blocco IP, la lunghezza del prefisso e numero di subnet richiesta.   
  
Se il numero di subnet disponibile è inferiore al numero di richieste subnet, le subnet disponibili vengono restituite con un avviso che indica che il numero disponibile è inferiore al numero di richieste.  
  
>[!NOTE]
>Questa funzione non effettivamente allocare le subnet, viene segnalato solo la disponibilità. Tuttavia, l'output del cmdlet può essere reindirizzato al cmdlet di **Aggiungi IpamSubnet** comando per creare la subnet.  
  
Per ulteriori informazioni, vedere [trova IpamFreeSubnet](https://technet.microsoft.com/library/mt712782.aspx).  
  
#### **<a name="find-free-address-ranges-with-find-ipamfreerange"></a>Trovare intervalli di indirizzi liberi con Trova IpamFreeRange**  
  
Questo nuovo comando restituisce disponibili intervalli di indirizzi IP assegnato a una subnet IP, il numero di che sono necessari nell'intervallo di indirizzi e il numero di intervalli di richiesta.   
  
Il comando Cerca una serie di indirizzi IP non allocati che corrisponde al numero di indirizzi richiesti continua. Il processo viene ripetuto fino a quando non viene trovato il numero di intervalli richiesti o fino a quando non sono disponibili non più intervalli di indirizzi disponibili.  
  
> [!NOTE]
> Questa funzione non alloca effettivamente gli intervalli, segnala solo la disponibilità. Tuttavia, l'output del cmdlet può essere reindirizzato al cmdlet di **Add-IpamRange** comando per creare l'intervallo.  
  
Per ulteriori informazioni, vedere [trova IpamFreeRange](https://technet.microsoft.com/library/mt712772.aspx).  
  
### <a name="EDNS"></a>Gestione avanzata del servizio DNS  
Gestione indirizzi IP in Windows Server 2016 supporta ora l'individuazione dei server DNS basati su file appartenenti a un dominio in una foresta di Active Directory in cui è in esecuzione Gestione indirizzi IP.  
  
Inoltre, le funzioni seguenti di DNS sono state aggiunte:  
  
-   Risorse e le zone DNS record raccolta (diverse da quelle relative a DNSSEC) dal server DNS che eseguono Windows Server 2008 o versione successiva.  
  
-   Configurare (creare, modificare ed eliminare) le proprietà e le operazioni in tutti i tipi di record di risorse (diverse da quelle relative a DNSSEC).  
  
-   Configurare (creare, modificare, eliminare) le proprietà e le operazioni su tutti i tipi di zone DNS, tra cui primario secondario e le zone di Stub).  
  
-   Attivata attività nel secondario e le zone di stub, indipendentemente dal fatto che se sono in avanti o zone di ricerca inversa. Ad esempio, attività, ad esempio **trasferimento dal Master** o **nuova copia di zona di trasferimento dal Master**.  
  
-   Controllo di accesso basato sui ruoli per la configurazione DNS supportata (record DNS e zone DNS).  
  
-   Raccolta di server d'inoltro condizionali e configurazione (creazione, eliminazione, modifica).  
  
### <a name="DDI"></a>Integrata DNS, DHCP e degli indirizzi IP management (DDI)  
Quando si visualizza un indirizzo IP in inventario degli indirizzi IP, è possibile nella visualizzazione dettagli per vedere tutti i record di risorse DNS associati all'indirizzo IP.  
  
Come parte raccolta di record risorse DNS, gestione indirizzi IP raccoglie i record PTR per le zone di ricerca inversa DNS. Per tutte le zone di ricerca inversa che vengono mappate a qualsiasi intervallo di indirizzi IP, gestione indirizzi IP consente di creare i record di indirizzo IP per tutti i record PTR appartenenti a tale zona nell'intervallo di indirizzi IP mappato corrispondente. Se esiste già l'indirizzo IP, il record PTR è semplicemente associato a tale indirizzo IP. Gli indirizzi IP non vengono creati automaticamente se la zona di ricerca inversa non è mappata a qualsiasi intervallo di indirizzi IP.  
  
Quando viene creato un record PTR in una zona di ricerca inversa tramite Gestione indirizzi IP, l'inventario degli indirizzi IP viene aggiornato nello stesso modo, come descritto in precedenza. Durante la successiva raccolta, poiché l'indirizzo IP sarà già presente nel sistema, il record PTR verrà semplicemente associato con l'indirizzo IP.  
  
### <a name="bkmk_ad"></a>Supporto di più foreste Active Directory  
In Windows Server 2012 R2, gestione indirizzi IP è stato in grado di individuare e gestire i server DNS e DHCP appartenenti alla stessa foresta Active Directory del server di gestione indirizzi IP. Ora è possibile gestire i server DNS e DHCP che appartengono a un'altra foresta Active Directory quando ha una relazione di trust bidirezionale con la foresta in cui è installato il server di gestione indirizzi IP. È possibile utilizzare il **configurare l'individuazione di Server** finestra di dialogo e aggiungere domini dalle altre trusted foreste che si desidera gestire. Dopo che i server vengono individuati, l'esperienza di gestione è lo stesso per quanto riguarda i server che appartengono alla stessa foresta in cui è installato Gestione indirizzi IP.  
  
Per ulteriori informazioni, vedere [gestire risorse in più foreste di Active Directory](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="bkmk_purge"></a>Elimina dati di utilizzo  
Eliminazione dei dati di utilizzo consente di ridurre le dimensioni del database di gestione indirizzi IP eliminando i dati di utilizzo degli indirizzi IP precedenti. Per eseguire l'eliminazione dei dati, si specifica una data e gestione indirizzi IP elimina tutte le voci di database che sono meno recenti o uguale alla data di fornire.   
  
Per ulteriori informazioni, vedere [Ripulisci dati di utilizzo](../../technologies/ipam/Purge-Utilization-Data.md).  
  
### <a name="bkmk_ps"></a>Supporto di Windows PowerShell per controllo dell'accesso basato su ruoli  
È ora possibile utilizzare Windows PowerShell per configurare controllo dell'accesso basato su ruoli. È possibile utilizzare i comandi di Windows PowerShell per recuperare gli oggetti DNS e DHCP in Gestione indirizzi IP e modificare i relativi ambiti di accesso. Per questo motivo, è possibile scrivere script di Windows PowerShell per assegnare gli ambiti di accesso per gli oggetti seguenti.  
  
-   Spazio di indirizzi IP  
  
-   Blocco di indirizzi IP  
  
-   Subnet di indirizzi IP  
  
-   Intervalli di indirizzi IP  
  
-   Server DNS  
  
-   Zone DNS  
  
-   Server d'inoltro condizionali DNS  
  
-   Record di risorse DNS  
  
-   Server DHCP  
  
-   Ambiti estesi DHCP  
  
-   Ambiti DHCP  
  
Per ulteriori informazioni, vedere [gestire Role Based Access Control con Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) e [cmdlet di Server di gestione indirizzi IP (IPAM) in Windows PowerShell](https://technet.microsoft.com/library/jj553807.aspx).  

