---
title: Gestione di record di risorse DNS
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8fe318a8ac17c650d8dbf2339e72b561de529c4a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405676"
---
# <a name="dns-resource-record-management"></a>Gestione di record di risorse DNS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento fornisce informazioni sulla gestione dei record di risorse DNS con gestione indirizzi IP.  
  
> [!NOTE]  
> Oltre a questo argomento, in questa sezione sono disponibili gli argomenti seguenti relativi alla gestione dei record di risorse DNS.  
>   
> -   [Aggiungere un record di risorse DNS](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [Eliminare i record di risorse DNS](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [Filtrare la visualizzazione dei record di risorse DNS](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [Visualizzare i record di risorse DNS per un indirizzo IP specifico](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>Panoramica di gestione dei record di risorse  
Quando si distribuisce gestione indirizzi IP in Windows Server 2016, è possibile eseguire l'individuazione del server per aggiungere i server DHCP e DNS alla console di gestione del server di gestione indirizzi IP. Il server di gestione indirizzi IP raccoglie quindi in modo dinamico i dati DNS ogni sei ore dai server DNS configurati per la gestione. Gestione indirizzi IP gestisce un database locale in cui vengono archiviati i dati DNS. Gestione indirizzi IP fornisce la notifica del giorno e dell'ora in cui sono stati raccolti i dati del server, nonché il giorno e l'ora successivi in cui si verificherà la raccolta dei dati dai server DNS.  
  
La barra di stato gialla nell'illustrazione seguente mostra il percorso dell'interfaccia utente delle notifiche di gestione indirizzi IP.  
  
![Notifiche di Gestione indirizzi IP](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
I dati DNS raccolti includono la zona DNS e le informazioni sui record di risorse. È possibile configurare Gestione indirizzi IP per raccogliere informazioni sulla zona dal server DNS preferito.  Gestione indirizzi IP raccoglie le zone sia basate su file che Active Directory.  
  
> [!NOTE]  
> Gestione indirizzi IP raccoglie i dati esclusivamente dai server DNS Microsoft aggiunti a un dominio. I server DNS di terze parti e i server non appartenenti a un dominio non sono supportati da Gestione indirizzi IP.  
  
Di seguito è riportato un elenco di tipi di record di risorse DNS raccolti da Gestione indirizzi IP.  
  
-   Database AFS  
  
-   Indirizzo ATM  
  
-   CNAME  
  
-   DHCID  
  
-   DNAME  
  
-   Host A o AAAA  
  
-   Informazioni sull'host  
  
-   ISDN  
  
-   MX  
  
-   Server dei nomi  
  
-   Puntatore (PTR)  
  
-   Persona responsabile  
  
-   Route  
  
-   Posizione del servizio  
  
-   SOA  
  
-   SRV  
  
-   Testo  
  
-   Servizi noti  
  
-   WINS  
  
-   WINS-R  
  
-   X. 25  
  
In Windows Server 2016, gestione indirizzi IP fornisce l'integrazione tra l'inventario degli indirizzi IP, Zone DNS e i record di risorse DNS:  
  
-   È possibile usare Gestione indirizzi IP per creare automaticamente un inventario degli indirizzi IP dai record di risorse DNS.  
  
-   È possibile creare manualmente un inventario degli indirizzi IP dai record di risorse DNS A e AAAA.  
  
-   È possibile visualizzare i record di risorse DNS per una zona DNS specifica e filtrare i record in base al tipo, all'indirizzo IP, ai dati dei record di risorse e ad altre opzioni di filtro.  
  
-   IPAM crea automaticamente un mapping tra gli intervalli di indirizzi IP e le zone di ricerca inversa DNS.  
  
-   IPAM crea indirizzi IP per i record PTR presenti nella zona di ricerca inversa e che sono inclusi nell'intervallo di indirizzi IP. Se necessario, è anche possibile modificare manualmente questo mapping.  
  
Gestione indirizzi IP consente di eseguire le operazioni seguenti nei record di risorse dalla console di gestione indirizzi IP.  
  
-   Creare record di risorse DNS  
  
-   Modificare i record di risorse DNS  
  
-   Eliminare i record di risorse DNS  
  
-   Creare record di risorse associati  
  
Gestione indirizzi IP registra automaticamente tutte le modifiche alla configurazione DNS apportate tramite la console di gestione indirizzi IP.  
  
## <a name="see-also"></a>Vedere anche  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


