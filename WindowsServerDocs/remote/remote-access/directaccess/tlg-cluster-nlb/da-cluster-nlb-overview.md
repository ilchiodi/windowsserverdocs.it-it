---
title: Panoramica dello scenario del lab di test relativo al bilanciamento del carico di rete di un cluster DirectAccess
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare DirectAccess in un Cluster con bilanciamento carico di rete di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cd1e9efd-19e9-49e7-8432-881f661c9792
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a6d82713dfb12e6775402d29bfcebaa0ec8b066c
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281547"
---
# <a name="overview-of-the-directaccess-cluster-nlb-test-lab-scenario"></a>Panoramica dello scenario del lab di test relativo al bilanciamento del carico di rete di un cluster DirectAccess

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo scenario di testing, DirectAccess viene distribuito con:  
  
-   **DC1**- un server configurato come controller di dominio, server di sistema DNS (Domain Name) e il server Dynamic Host Configuration Protocol (DHCP).  
  
-   **EDGE1**- un server nella rete interna che è configurato come primo server di accesso remoto in un cluster di server di accesso remoto. Questo server ha due schede di rete; una connessa alla rete interna e l'altra connessa alla rete esterna.  
  
-   **EDGE2**- un server nella rete interna che è configurato come il secondo server di accesso remoto in un cluster di server di accesso remoto. Questo server ha due schede di rete; una connessa alla rete interna e l'altra connessa alla rete esterna.  
  
-   **APP1**- un server nella rete interna che è configurato come server web e file e come un'autorità di certificazione radice aziendale (CA)  
  
-   **APP2**- computer nella rete interna che è configurato come un server solo IPv4 file e web. Il computer viene utilizzato per evidenziare le funzionalità NAT64/DNS64.  
  
-   **Computer INET1**: un server configurato come un server DHCP e DNS Internet.  
  
-   **NAT1**- un computer client configurato come dispositivo di translator (NAT) indirizzo di rete tramite Condivisione connessione Internet.  
  
-   **CLIENT1**- un computer client configurato come un client DirectAccess che verrà usato per testare la connettività DirectAccess durante lo spostamento tra la rete interna, la rete Internet simulata e una rete domestica.  
  
Il lab di test è costituito da tre subnet che simulano le operazioni seguenti:  
  
-   Una rete domestica denominata Homenet (192.168.137.0/24) connesso a Internet da un NAT.  
  
-   La rete esterna rappresentata dalla subnet Internet (131.107.0.0/24).  
  
-   Una rete interna denominata Corpnet (10.0.0.0/24; 2001:DB8:1:::/::/ 64) separati da Internet dal server di accesso remoto.  
  
I computer in ogni subnet connettersi tramite hub fisico o virtuale o un commutatore, come illustrato nella figura seguente.  
  
![Panoramica dell'ambiente di testing](../../../media/Overview-of-the-Test-Lab-Scenario_5/TLG_DA_Cluster.png)  
  


