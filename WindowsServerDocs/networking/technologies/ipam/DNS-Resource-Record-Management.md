---
title: Gestione di record di risorse DNS
description: Questo argomento fa parte della Guida di gestione di gestione indirizzi IP (IPAM) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2db802fa928a4051fbe409fc0ba60d774bb0428
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284047"
---
# <a name="dns-resource-record-management"></a>Gestione di record di risorse DNS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento vengono fornite informazioni sulla gestione dei record di risorse con gestione indirizzi IP.  
  
> [!NOTE]  
> Oltre a questo argomento, sono disponibili i seguenti argomenti di gestione dei record risorsa DNS in questa sezione.  
>   
> -   [Aggiungere un Record di risorse DNS](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [Eliminare i record risorsa DNS](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [Filtrare la visualizzazione dei record di risorse DNS](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [Visualizzare i record risorsa DNS per un indirizzo IP specifico](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>Cenni preliminari sulla gestione dei record di risorse  
Quando si distribuisce gestione indirizzi IP in Windows Server 2016, è possibile eseguire l'individuazione del server per aggiungere i server DHCP e DNS per la console di gestione di server di gestione indirizzi IP. Il server di gestione indirizzi IP in modo dinamico raccoglie quindi i dati DNS ogni sei ore dal server DNS è configurato per la gestione. Gestione indirizzi IP contiene un database locale in cui archivia i dati DNS. Gestione indirizzi IP offre la notifica del giorno e ora in cui è stato raccolto i dati del server, nonché indicante il giorno successivo e ora in cui verrà effettuata la raccolta dei dati dai server DNS.  
  
La barra di stato gialla nella figura seguente mostra la posizione di interfaccia utente delle notifiche di gestione indirizzi IP.  
  
![Notifiche di Gestione indirizzi IP](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
I dati DNS che vengono raccolti includono informazioni su record di risorse e di zona DNS. È possibile configurare Gestione indirizzi IP per raccogliere le informazioni sulla zona dal server DNS preferito.  Gestione indirizzi IP raccoglie le zone di Directory basata su file sia attivo.  
  
> [!NOTE]  
> Gestione indirizzi IP raccoglie i dati esclusivamente dal server DNS Microsoft aggiunti al dominio. I server DNS di terze parti e i server unita in join non di dominio non sono supportati da Gestione indirizzi IP.  
  
Seguito è riportato un elenco di tipi di record risorse DNS che vengono raccolti da Gestione indirizzi IP.  
  
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
  
-   Indirizzamento tramite  
  
-   Posizione del servizio  
  
-   SOA  
  
-   SRV  
  
-   Testo  
  
-   Servizi noti  
  
-   WINS  
  
-   WINS-R  
  
-   X.25  
  
In Windows Server 2016, gestione indirizzi IP offre l'integrazione tra inventario degli indirizzi IP DNS zone e record di risorse:  
  
-   È possibile usare Gestione indirizzi IP per compilare automaticamente un inventario degli indirizzi IP da record di risorse.  
  
-   È possibile creare manualmente un inventario degli indirizzi IP da record di risorse AAAA e A DNS.  
  
-   È possibile visualizzare i record risorsa DNS per una zona DNS specifico e filtrare i record in base a tipo, indirizzo IP, i dati di record di risorse e altre opzioni di filtro.  
  
-   Gestione indirizzi IP viene automaticamente creato un mapping tra gli intervalli di indirizzi IP e le zone di ricerca inversa DNS.  
  
-   Gestione indirizzi IP consente di creare gli indirizzi IP per i record PTR che siano presenti nella zona di ricerca inversa e che sono inclusi in tale intervallo di indirizzi IP. Se necessario, è possibile modificare manualmente questo mapping.  
  
Gestione indirizzi IP consente di eseguire le operazioni seguenti nel record di risorse dalla console di gestione indirizzi IP.  
  
-   Creare record di risorse DNS  
  
-   Modificare i record risorsa DNS  
  
-   Eliminare i record di risorse DNS  
  
-   Creare record di risorse associato  
  
Gestione indirizzi IP registra automaticamente tutte le modifiche di configurazione DNS che vengono apportate utilizzando la console di gestione indirizzi IP.  
  
## <a name="see-also"></a>Vedere anche  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


