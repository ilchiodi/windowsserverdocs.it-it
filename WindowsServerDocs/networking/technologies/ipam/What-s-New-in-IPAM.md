---
title: Novità di Gestione indirizzi IP
description: Questo argomento descrive le funzionalità di gestione indirizzi IP (IPAM) nuove o modificate in Windows Server 2016.
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
ms.openlocfilehash: 04838cba63805d20ba31629ed9c8e95290046320
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624683"
---
# <a name="whats-new-in-ipam"></a>Novità di Gestione indirizzi IP

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento descrive le funzionalità di gestione indirizzi IP (IPAM) nuove o modificate in Windows Server 2016.  
  
Gestione indirizzi IP offre funzionalità amministrative e di monitoraggio altamente personalizzabili per l'infrastruttura DNS e indirizzo IP in una rete aziendale o Provider del servizio Cloud (CSP). È possibile monitorare, controllare e gestire server che eseguono Dynamic Host Configuration Protocol (DHCP) e del sistema DNS (Domain Name) tramite Gestione indirizzi IP.  
  
## <a name="BKMK_IPAM2012R2"></a>Aggiornamenti nel Server di gestione indirizzi IP  
Di seguito sono le funzionalità nuove e migliorate per gestione indirizzi IP in Windows Server 2016.  
  
|Caratteristica/funzionalità|Novità o miglioramento|Descrizione|  
|--------------------------|-------------------|---------------|  
|[Gestione migliorata degli indirizzi IP](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|Miglioramento|Le funzionalità di gestione indirizzi IP sono migliorate per scenari quali la gestione delle subnet /32 IPv4 e IPv6 /128 e ricerca gratuita subnet di indirizzi IP e gli intervalli in un blocco di indirizzi IP.|  
|[Gestione avanzata dei servizi DNS](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|Nuova|Gestione indirizzi IP supporta i record di risorse DNS, server d'inoltro condizionale e gestione delle zone DNS per i server DNS integrate in Active Directory e backup di file aggiunti al dominio.|  
|[Integrated DNS, DHCP e indirizzi IP management (DDI)](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|Miglioramento|Diverse nuove esperienze e gestione del ciclo di vita integrata sono abilitate le operazioni, ad esempio per visualizzare tutti i record di risorse DNS che si riferiscono a un indirizzo IP, automatizzati inventario degli indirizzi IP in base ai record di risorse e gestione del ciclo di vita degli indirizzi IP per le operazioni sia DNS e DHCP.|  
|[Supporto per più foreste Active Directory](#bkmk_ad)|Nuova|È possibile utilizzare Gestione indirizzi IP per gestire i server DNS e DHCP di più foreste Active Directory quando esiste una relazione di trust bidirezionale tra la foresta in cui è installato Gestione indirizzi IP e ogni insieme di strutture remoti.|  
|[Eliminare i dati di utilizzo](#bkmk_purge)|Nuova|È ora possibile ridurre le dimensioni del database di gestione indirizzi IP eliminando i dati di utilizzo di indirizzi IP che superano una data specificata.|  
|[Supporto di Windows PowerShell per Role Based Access Control](#bkmk_ps)|Nuova|È possibile utilizzare Windows PowerShell per impostare gli ambiti di accesso per gli oggetti di gestione indirizzi IP.|  
  
### <a name="EIP"></a>Gestione migliorata degli indirizzi IP  
Le seguenti funzionalità migliorare le funzionalità di gestione di indirizzi di gestione indirizzi IP.  
>[!NOTE]
>Per i riferimenti ai comandi di gestione indirizzi IP Windows PowerShell, vedere [cmdlet di Server di gestione indirizzi IP (IPAM) in Windows PowerShell](https://docs.microsoft.com/en-us/powershell/module/ipamserver/).  
  
#### <a name="support-for-31-32-and-128-subnets"></a>Supporto per /31 /32 e /128 subnet  
Gestione indirizzi IP in Windows Server 2016 ora supporta /31 /32 e /128 subnet. Ad esempio, un' due indirizzo subnet (/ 31 IPv4) potrebbero essere necessari per un collegamento da punto a punto tra switch. Inoltre, alcune opzioni potrebbero richiedere indirizzi di loopback single (/ 32 per IPv4, / 128 per IPv6).  
  
#### <a name="find-free-subnets-with-find-ipamfreesubnet"></a>**Ricerca gratuita subnet con Find-IpamFreeSubnet**  
  
Questo comando restituisce le subnet che sono disponibili per l'allocazione, dato un blocco IP, la lunghezza del prefisso e numero di subnet richiesta.   
  
Se il numero di subnet disponibili è minore del numero di subnet richiesto, le subnet disponibili vengono restituite con un avviso che indica che il numero disponibile è inferiore al numero richiesto.  
  
>[!NOTE]
>Questa funzione non alloca effettivamente le subnet, ma indica solo la loro disponibilità. Tuttavia, l'output del cmdlet può essere inviato tramite pipe per il **Add-IpamSubnet** comando per creare la subnet.  
  
Per altre informazioni, vedere [Find-IpamFreeSubnet](https://docs.microsoft.com/en-us/powershell/module/ipamserver/Find-IpamFreeSubnet).  
  
#### <a name="find-free-address-ranges-with-find-ipamfreerange"></a>**Trovare gli intervalli di indirizzi gratuito con Find-IpamFreeRange**  
  
Questo nuovo comando restituisce disponibili intervalli di indirizzi IP assegnato una subnet IP, il numero di indirizzi necessari nell'intervallo e il numero di intervalli richiesto.   
  
Il comando Cerca una serie continua di indirizzi IP non allocati che corrisponde al numero di indirizzi richiesti. Il processo viene ripetuto fino a quando non viene trovato il numero richiesto di intervalli o fino a quando non sono presenti più disponibile alcun gli intervalli di indirizzi disponibili.  
  
> [!NOTE]
> Questa funzione non vengono allocati gli intervalli in realtà, fornisce solo la loro disponibilità. Tuttavia, l'output del cmdlet può essere inviato tramite pipe per la **Add-IpamRange** comando per creare l'intervallo.  
  
Per altre informazioni, vedere [Find-IpamFreeRange](https://docs.microsoft.com/en-us/powershell/module/ipamserver/Find-IpamFreeRange).  
  
### <a name="EDNS"></a>Gestione avanzata dei servizi DNS  
Gestione indirizzi IP in Windows Server 2016 supporta ora l'individuazione dei server DNS basato su file, aggiunto al dominio in una foresta di Active Directory in cui è in esecuzione Gestione indirizzi IP.  
  
Inoltre, sono state aggiunte le funzioni DNS seguenti:  
  
-   Risorse e le zone DNS record raccolta (diverse da quelle relative a DNSSEC) dal server DNS che eseguono Windows Server 2008 o versione successiva.  
  
-   Configurare (creare, modificare ed eliminare) le proprietà e operazioni su tutti i tipi di record di risorse (diverse da quelle relative a DNSSEC).  
  
-   Configurare (creare, modificare ed eliminare) le proprietà e operazioni su tutti i tipi di zone DNS, tra cui primario secondario e zone di Stub).  
  
-   Attivare attività nel sito secondario e le zone di stub, indipendentemente dal fatto che se vengano inoltrare o zone di ricerca inversa. Ad esempio, le attività, ad esempio **Trasferisci dal Master** oppure **trasferimento nuova copia della zona dal server Master**.  
  
-   Controllo di accesso basato sui ruoli per la configurazione DNS supportata (record DNS e zone DNS).  
  
-   Raccolta di server d'inoltro condizionali e configuration (creazione, eliminazione, modifica).  
  
### <a name="DDI"></a>Integrated DNS, DHCP e indirizzi IP management (DDI)  
Quando si visualizza un indirizzo IP in inventario degli indirizzi IP, è possibile scegliere in visualizzazione dettagli per visualizzare tutti i record risorsa DNS associati all'indirizzo IP.  
  
Come parte della raccolta di record risorse DNS, gestione indirizzi IP raccoglie i record PTR per le zone di ricerca inversa DNS. Per tutte le zone di ricerca inversa che vengono eseguito il mapping a qualsiasi intervallo di indirizzi IP, gestione indirizzi IP vengono creati i record di indirizzo IP per tutti i record PTR che appartengono a tale area nell'intervallo di indirizzi IP mappato corrispondente. Se esiste già l'indirizzo IP, il record PTR è semplicemente associato a quell'indirizzo IP. Gli indirizzi IP non vengono creati automaticamente se la zona di ricerca inversa non è mappata a qualsiasi intervallo di indirizzi IP.  
  
Quando viene creato un record PTR nella zona di ricerca inversa tramite Gestione indirizzi IP, l'inventario degli indirizzi IP viene aggiornato allo stesso modo, come descritto in precedenza. Durante la raccolta successiva, poiché l'indirizzo IP sarà già presente nel sistema, il record PTR verrà semplicemente associato con quell'indirizzo IP.  
  
### <a name="bkmk_ad"></a>Supporto per più foreste Active Directory  
In Windows Server 2012 R2, gestione indirizzi IP è stato in grado di individuare e gestire server DNS e DHCP che appartengono alla stessa foresta Active Directory come server di gestione indirizzi IP. A questo punto è possibile gestire i server DNS e DHCP che appartengono a un'altra foresta Active Directory quando ha una relazione di trust bidirezionale con la foresta in cui è installato il server di gestione indirizzi IP. È possibile passare al **configurare l'individuazione Server** dialogo casella e aggiungere i domini da un altro attendibile le foreste che si desidera gestire. Dopo aver individuati i server, l'esperienza di gestione è identico a quello di server che appartengono alla stessa foresta in cui è installato Gestione indirizzi IP.  
  
Per altre informazioni, vedere [gestire le risorse in più foreste di Active Directory](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="bkmk_purge"></a>Eliminare i dati di utilizzo  
Ripulire i dati di utilizzo consente di ridurre le dimensioni del database di gestione indirizzi IP, eliminando i vecchi dati di utilizzo degli indirizzi IP. Per eseguire l'eliminazione dei dati, si specifica una data e gestione indirizzi IP consente di eliminare tutte le voci di database che risalgono a più o uguale alla data è fornire.   
  
Per altre informazioni, vedere [ripulire i dati di utilizzo](../../technologies/ipam/Purge-Utilization-Data.md).  
  
### <a name="bkmk_ps"></a>Supporto di Windows PowerShell per Role Based Access Control  
È ora possibile usare Windows PowerShell per configurare Role Based Access Control. È possibile usare i comandi di Windows PowerShell per recuperare oggetti DNS e DHCP in Gestione indirizzi IP e modificare i relativi ambiti di accesso. Per questo motivo, è possibile scrivere gli script di Windows PowerShell per assegnare gli ambiti di accesso agli oggetti seguenti.  
  
-   Spazio di indirizzi IP  
  
-   Blocco di indirizzi IP  
  
-   Subnet di indirizzi IP  
  
-   Intervalli di indirizzi IP  
  
-   Server DNS  
  
-   Zone DNS  
  
-   Server d'inoltro condizionali DNS  
  
-   Record risorsa DNS  
  
-   Server DHCP  
  
-   Ambiti estesi DHCP  
  
-   Ambiti DHCP  
  
Per altre informazioni, vedere [gestire Role Based Access Control con Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) e [cmdlet di Server di gestione indirizzi IP (IPAM) in Windows PowerShell](https://docs.microsoft.com/en-us/powershell/module/ipamserver/).  

