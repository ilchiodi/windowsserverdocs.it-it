---
title: Novità di Gestione indirizzi IP
description: Questo argomento descrive le funzionalità di gestione indirizzi IP nuove o modificate in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f2f2f1a5-ac2f-41b7-a495-98ad0e2a9b20
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fc19a58482df5dfbfb4ea324f317bbe1b27bf834
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405599"
---
# <a name="whats-new-in-ipam"></a>Novità di Gestione indirizzi IP

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento descrive le funzionalità di gestione indirizzi IP nuove o modificate in Windows Server 2016.  
  
Gestione indirizzi IP offre funzionalità amministrative e di monitoraggio altamente personalizzabili per l'indirizzo IP e l'infrastruttura DNS in una rete aziendale o del provider di servizi cloud (CSP). È possibile monitorare, controllare e gestire i server che eseguono Dynamic Host Configuration Protocol (DHCP) e Domain Name System (DNS) usando Gestione indirizzi IP.  
  
## <a name="BKMK_IPAM2012R2"></a>Aggiornamenti nel server di gestione indirizzi IP  
Di seguito sono riportate le funzionalità nuove e migliorate per gestione indirizzi IP in Windows Server 2016.  
  
|Caratteristica/funzionalità|Novità o miglioramento|Descrizione|  
|--------------------------|-------------------|---------------|  
|[Gestione degli indirizzi IP avanzata](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|Miglioramento|Le funzionalità di gestione indirizzi IP sono migliorate per scenari quali la gestione delle subnet /32 IPv4 e IPv6 /128 e ricerca gratuita subnet di indirizzi IP e gli intervalli in un blocco di indirizzi IP.|  
|[Gestione avanzata dei servizi DNS](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|Nuova|Gestione indirizzi IP supporta i record di risorse DNS, server d'inoltro condizionale e gestione delle zone DNS per i server DNS integrate in Active Directory e backup di file aggiunti al dominio.|  
|[Gestione integrata di DNS, DHCP e indirizzi IP (DDI)](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|Miglioramento|Diverse nuove esperienze e gestione del ciclo di vita integrata sono abilitate le operazioni, ad esempio per visualizzare tutti i record di risorse DNS che si riferiscono a un indirizzo IP, automatizzati inventario degli indirizzi IP in base ai record di risorse e gestione del ciclo di vita degli indirizzi IP per le operazioni sia DNS e DHCP.|  
|[Supporto per più insiemi di strutture Active Directory](#bkmk_ad)|Nuova|È possibile utilizzare Gestione indirizzi IP per gestire i server DNS e DHCP di più foreste Active Directory quando esiste una relazione di trust bidirezionale tra la foresta in cui è installato Gestione indirizzi IP e ogni insieme di strutture remoti.|  
|[Elimina dati di utilizzo](#bkmk_purge)|Nuova|È ora possibile ridurre le dimensioni del database di gestione indirizzi IP eliminando i dati di utilizzo degli indirizzi IP più vecchi di una data specificata.|  
|[Supporto di Windows PowerShell per il controllo degli accessi in base al ruolo](#bkmk_ps)|Nuova|È possibile utilizzare Windows PowerShell per impostare gli ambiti di accesso per gli oggetti di gestione indirizzi IP.|  
  
### <a name="EIP"></a>Gestione degli indirizzi IP avanzata  
Le funzionalità seguenti migliorano le funzionalità di gestione degli indirizzi di gestione indirizzi IP.  
>[!NOTE]
>Per informazioni di riferimento sui comandi di Windows PowerShell per [Gestione indirizzi IP, vedere cmdlet del server Gestione indirizzi IP in Windows PowerShell](https://docs.microsoft.com/powershell/module/ipamserver/).  
  
#### <a name="support-for-31-32-and-128-subnets"></a>Supporto per le subnet/31,/32 e/128  
Gestione indirizzi IP in Windows Server 2016 supporta ora le subnet/31,/32 e/128. Ad esempio, è possibile che sia necessaria una subnet con due indirizzi (/31 IPv4) per un collegamento Point-to-Point tra le opzioni. Inoltre, alcune opzioni possono richiedere indirizzi di loopback singoli (/32 per IPv4,/128 per IPv6).  
  
#### <a name="find-free-subnets-with-find-ipamfreesubnet"></a>**Trova le subnet gratuite con find-IpamFreeSubnet**  
  
Questo comando restituisce le subnet disponibili per l'allocazione, dati un blocco IP, la lunghezza del prefisso e il numero di subnet richieste.   
  
Se il numero di subnet disponibili è inferiore al numero di subnet richieste, le subnet disponibili vengono restituite con un avviso che indica che il numero disponibile è inferiore al numero richiesto.  
  
>[!NOTE]
>Questa funzione non alloca effettivamente le subnet, ma ne segnala solo la disponibilità. Tuttavia, l'output del cmdlet può essere inviato tramite pipe al comando **Add-IpamSubnet** per creare la subnet.  
  
Per ulteriori informazioni, vedere [Find-IpamFreeSubnet](https://docs.microsoft.com/powershell/module/ipamserver/Find-IpamFreeSubnet).  
  
#### <a name="find-free-address-ranges-with-find-ipamfreerange"></a>**Trovare gli intervalli di indirizzi gratuiti con find-IpamFreeRange**  
  
Questo nuovo comando restituisce gli intervalli di indirizzi IP disponibili, data una subnet IP, il numero di indirizzi necessari nell'intervallo e il numero di intervalli richiesti.   
  
Il comando Cerca una serie continua di indirizzi IP non allocati che corrispondono al numero di indirizzi richiesti. Il processo viene ripetuto fino a quando non viene trovato il numero richiesto di intervalli o fino a quando non sono disponibili altri intervalli di indirizzi disponibili.  
  
> [!NOTE]
> Questa funzione non alloca effettivamente gli intervalli, ma ne segnala solo la disponibilità. Tuttavia, l'output del cmdlet può essere inviato tramite pipe al comando **Add-IpamRange** per creare l'intervallo.  
  
Per ulteriori informazioni, vedere [Find-IpamFreeRange](https://docs.microsoft.com/powershell/module/ipamserver/Find-IpamFreeRange).  
  
### <a name="EDNS"></a>Gestione avanzata dei servizi DNS  
Gestione indirizzi IP in Windows Server 2016 supporta ora l'individuazione di server DNS basati su file e aggiunti a un dominio in una foresta Active Directory in cui è in esecuzione Gestione indirizzi IP.  
  
Sono state inoltre aggiunte le funzioni DNS seguenti:  
  
-   Zone DNS e raccolta di record di risorse (diverse da quelle relative a DNSSEC) dai server DNS che eseguono Windows Server 2008 o versioni successive.  
  
-   Configurare (creare, modificare ed eliminare) proprietà e operazioni su tutti i tipi di record di risorse (ad eccezione di quelli relativi a DNSSEC).  
  
-   Configurare (creare, modificare, eliminare) proprietà e operazioni su tutti i tipi di zone DNS, incluse le zone secondarie primarie e stub.  
  
-   Attività attivate in zone secondarie e Stub, indipendentemente dal fatto che siano zone di ricerca diretta o inversa. Ad esempio, attività come il **trasferimento da Master** o il **trasferimento di una nuova copia della zona dal master**.  
  
-   Controllo degli accessi in base al ruolo per la configurazione DNS supportata (record DNS e zone DNS).  
  
-   Raccolta e configurazione di server d'inoltri condizionali (create, DELETE, Edit).  
  
### <a name="DDI"></a>Gestione integrata di DNS, DHCP e indirizzi IP (DDI)  
Quando si visualizza un indirizzo IP nell'inventario degli indirizzi IP, è possibile scegliere la visualizzazione dettagli per visualizzare tutti i record di risorse DNS associati all'indirizzo IP.  
  
Come parte della raccolta di record di risorse DNS, gestione indirizzi IP raccoglie i record PTR per le zone di ricerca inversa DNS. Per tutte le zone di ricerca inversa di cui è stato eseguito il mapping a qualsiasi intervallo di indirizzi IP, gestione indirizzi IP crea i record degli indirizzi IP per tutti i record PTR appartenenti a tale zona nell'intervallo di indirizzi IP mappato corrispondente. Se l'indirizzo IP esiste già, il record PTR viene semplicemente associato a tale indirizzo IP. Gli indirizzi IP non vengono creati automaticamente se la zona di ricerca inversa non è mappata ad alcun intervallo di indirizzi IP.  
  
Quando un record PTR viene creato in una zona di ricerca inversa tramite Gestione indirizzi IP, l'inventario degli indirizzi IP viene aggiornato nello stesso modo descritto in precedenza. Durante la successiva raccolta, poiché l'indirizzo IP è già presente nel sistema, il record PTR verrà semplicemente mappato a tale indirizzo IP.  
  
### <a name="bkmk_ad"></a>Supporto per più insiemi di strutture Active Directory  
In Windows Server 2012 R2, gestione indirizzi IP è stato in grado di individuare e gestire i server DNS e DHCP appartenenti alla stessa foresta di Active Directory del server di gestione indirizzi IP. È ora possibile gestire i server DNS e DHCP appartenenti a una foresta di Active Directory diversa quando dispone di una relazione di trust bidirezionale con la foresta in cui è installato il server di gestione indirizzi IP. È possibile passare alla finestra di dialogo **Configura individuazione server** e aggiungere i domini dalle altre foreste attendibili che si desidera gestire. Una volta individuati i server, l'esperienza di gestione è identica a quella dei server che appartengono alla stessa foresta in cui è installato Gestione indirizzi IP.  
  
Per altre informazioni, vedere [gestire le risorse in più foreste Active Directory](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="bkmk_purge"></a>Elimina dati di utilizzo  
Elimina dati di utilizzo consente di ridurre le dimensioni del database di gestione indirizzi IP eliminando i dati di utilizzo degli indirizzi IP precedenti. Per eseguire l'eliminazione dei dati, è necessario specificare una data e gestione indirizzi IP elimina tutte le voci di database precedenti o uguali alla data specificata.   
  
Per altre informazioni, vedere [ripulire i dati di utilizzo](../../technologies/ipam/Purge-Utilization-Data.md).  
  
### <a name="bkmk_ps"></a>Supporto di Windows PowerShell per il controllo degli accessi in base al ruolo  
È ora possibile usare Windows PowerShell per configurare il controllo degli accessi in base al ruolo. È possibile usare i comandi di Windows PowerShell per recuperare gli oggetti DNS e DHCP in gestione indirizzi IP e modificare gli ambiti di accesso. Per questo motivo, è possibile scrivere script di Windows PowerShell per assegnare gli ambiti di accesso agli oggetti seguenti.  
  
-   Spazio degli indirizzi IP  
  
-   Blocco di indirizzi IP  
  
-   Subnet di indirizzi IP  
  
-   Intervalli di indirizzi IP  
  
-   Server DNS  
  
-   Zone DNS  
  
-   Server d'inoltri condizionali DNS  
  
-   Record di risorse DNS  
  
-   Server DHCP  
  
-   Ambiti estesi DHCP  
  
-   Ambiti DHCP  
  
Per altre informazioni, vedere [gestire il controllo degli accessi in base al ruolo con](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) i [cmdlet Server di gestione indirizzi IP e Windows PowerShell in Windows PowerShell](https://docs.microsoft.com/powershell/module/ipamserver/).  

