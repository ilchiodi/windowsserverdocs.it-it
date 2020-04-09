---
title: Panoramica dello scenario del lab di test
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 9afeced4-1a9b-4cb3-9fc4-d7e44c675755
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 46bcc22604a11a3fc3da764e149d900b1ada1dc8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860384"
---
# <a name="overview-of-the-test-lab-scenario"></a>Panoramica dello scenario del lab di test

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo scenario di testing, DirectAccess viene distribuito con:  
  
-   **DC1**: un server configurato come controller di dominio, server DNS e server DHCP per il dominio corp.contoso.com.  
  
-   **2-DC1**: un server configurato come controller di dominio e server DNS per il dominio corp2.corp.contoso.com.  
  
-   **EDGE1 e 2-EDGE1**-due server nella rete interna che sono configurati come server di accesso remoto. Ogni server dispone di due schede di rete; una connessa alla rete interna e l'altra connessa alla rete esterna.  
  
-   **APP1 e 2-APP1**-due server nella rete interna che sono configurati come server web e file.  
  
-   **APP2**- computer nella rete interna che è configurato come un server solo IPv4 file e web. Il computer viene utilizzato per evidenziare le funzionalità NAT64/DNS64.  
  
-   **ROUTER1**: un server che è configurato per fornire routing tra le due reti aziendale interne.  
  
-   **Computer INET1**: un server configurato come un server DHCP e DNS Internet.  
  
-   **NAT1**- un computer client configurato come dispositivo di translator (NAT) indirizzo di rete tramite Condivisione connessione Internet.  
  
-   **CLIENT1 e CLIENT2**-due computer client che sono configurati come client DirectAccess che verrà utilizzato per verificare la connettività DirectAccess durante lo spostamento tra la rete interna, la rete Internet simulata e una rete domestica. **CLIENT2** è un Windows 7&reg;  client.  
  
L'ambiente di testing è costituito da quattro subnet che simulano le operazioni seguenti:  
  
-   Una rete domestica denominata Homenet (192.168.137.0/24) connesso a Internet da un NAT.  
  
-   La rete esterna rappresentata dalla subnet Internet (131.107.0.0/24).  
  
-   Una rete interna denominata Corpnet (10.0.0.0/24; 2001:db8:1:::/ 64) separati da Internet dal server di accesso remoto EDGE1.  
  
-   Una rete interna denominata 2-Corpnet1 (10.2.0.0/24; 2001:db8:2:::/ 64) separati da Internet dal server di accesso remoto EDGE1 2.  
  
I computer in ogni subnet connettersi tramite hub fisico o virtuale o un commutatore, come illustrato nella figura seguente.  
  
![Panoramica dell'ambiente di testing](../../../media/Overview-of-the-Test-Lab-Scenario_4/TLG_DA_Multisite.png)  
  


