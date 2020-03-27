---
title: Panoramica dello scenario del lab di test relativo al bilanciamento del carico di rete di un cluster DirectAccess
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess in un cluster con bilanciamento carico di servizio di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cd1e9efd-19e9-49e7-8432-881f661c9792
ms.author: lizross
author: eross-msft
ms.openlocfilehash: abc3038cfe0dacb09c115f37289fe72f1c14b96d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308849"
---
# <a name="overview-of-the-directaccess-cluster-nlb-test-lab-scenario"></a>Panoramica dello scenario del lab di test relativo al bilanciamento del carico di rete di un cluster DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo scenario di testing, DirectAccess viene distribuito con:  
  
-   **DC1**: un server configurato come controller di dominio, server Domain Name System (DNS) e server di Dynamic Host Configuration Protocol (DHCP).  
  
-   **Edge1**: un server nella rete interna configurato come primo server di accesso remoto in un cluster di server di accesso remoto. Questo server dispone di due schede di rete. una connessa alla rete interna e l'altra connessa alla rete esterna.  
  
-   **EDGE2**: un server nella rete interna configurato come secondo server di accesso remoto in un cluster di server di accesso remoto. Questo server dispone di due schede di rete. una connessa alla rete interna e l'altra connessa alla rete esterna.  
  
-   **App1**-un server nella rete interna configurato come web e file server e come autorità di certificazione radice dell'organizzazione (CA)  
  
-   **APP2**- computer nella rete interna che è configurato come un server solo IPv4 file e web. Il computer viene utilizzato per evidenziare le funzionalità NAT64/DNS64.  
  
-   **Computer INET1**: un server configurato come un server DHCP e DNS Internet.  
  
-   **NAT1**- un computer client configurato come dispositivo di translator (NAT) indirizzo di rete tramite Condivisione connessione Internet.  
  
-   **CLIENT1**: un computer client configurato come client DirectAccess che verrà usato per testare la connettività DirectAccess durante il passaggio tra la rete interna, la rete Internet simulata e una rete domestica.  
  
Il Lab di test è costituito da tre subnet che simulano gli elementi seguenti:  
  
-   Una rete domestica denominata Homenet (192.168.137.0/24) connesso a Internet da un NAT.  
  
-   La rete esterna rappresentata dalla subnet Internet (131.107.0.0/24).  
  
-   Una rete interna denominata corpnet (10.0.0.0/24; 2001: DB8:1::/64) separata da Internet dal server di accesso remoto.  
  
I computer in ogni subnet connettersi tramite hub fisico o virtuale o un commutatore, come illustrato nella figura seguente.  
  
![Panoramica dell'ambiente di testing](../../../media/Overview-of-the-Test-Lab-Scenario_5/TLG_DA_Cluster.png)  
  


