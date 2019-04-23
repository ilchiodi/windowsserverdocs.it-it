---
title: Passaggi per la configurazione del lab di test
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare una distribuzione multisito DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2bda808336624a5d80ed44cbadf543fc134c6190
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827882"
---
# <a name="steps-for-configuring-the-test-lab"></a>Passaggi per la configurazione del lab di test

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

I passaggi seguenti descrivono come configurare l'infrastruttura di accesso remoto, configurare il server di accesso remoto e i client e testare la connettività DirectAccess dalla subnet Internet e Homenet.  
  
In questa Guida al lab di test si compilerà una distribuzione multisito di accesso remoto eseguendo i passaggi seguenti:  
  
-   [PASSAGGIO 1: Completare la configurazione di Base](assetId:///9eb4a9ba-9118-4ea3-8963-e643ec81c3ed). Completare tutti i passaggi di [Guida al Lab di Test: Dimostrazione della Server singolo di DirectAccess con ambiente misto IPv4 e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [PASSAGGIO 2: Installare e configurare ROUTER1](assetId:///e4b1a298-d5b0-410e-970b-c5358a9378f9). ROUTER1 fornisce il routing e inoltro di funzionalità tra le subnet Corpnet e 2-Corpnet.  
  
-   [PASSAGGIO 3: Installare e configurare CLIENT2](assetId:///6cbee1b5-f6f6-443f-8fa9-31cc5c05a0ee). CLIENT2 è un computer client Windows 7 che viene usato per illustrare il con le versioni precedenti la compatibilità di una distribuzione di Windows Server 2016, Windows Server 2012 R2 o accesso remoto di Windows Server 2012.  
  
-   [PASSAGGIO 4: Configurare APP1](assetId:///a0ee655e-c01e-4bf3-a7b3-064e9614f810). Configurare APP1 con ROUTER1 come gateway predefinito e 2-DC1 come il server DNS alternativo.  
  
-   [PASSAGGIO 5: Configurare DC1](assetId:///205ca795-93ce-4e53-aa6b-b44c87f0e14a). Configurare DC1 con un sito di Active Directory aggiuntivo e i gruppi di sicurezza aggiuntive per i computer client Windows 7.  
  
-   [PASSAGGIO 6: Installare e configurare 2-DC1](assetId:///16752f61-edbf-4ff4-9d7a-e2077b66a127). In una distribuzione multisito, è necessario due o più domini e siti. 2-DC1 fornisce il controller di dominio e i servizi DNS per il dominio corp2.corp.contoso.com.  
  
-   [PASSAGGIO 7: Installare e configurare 2-APP1](assetId:///7d04b54e-590a-4d33-9766-415789859f29). 2-APP1 è un server web e file nella rete Corpnet di 2.  
  
-   [PASSAGGIO 8: Configurare i computer INET1](assetId:///8ecc0b63-8626-4939-8d26-3d51d051d231). Computer INET1 simula Internet in questa Guida al lab di test. È necessario configurare una voce DNS che viene risolto nell'indirizzo IP pubblico di EDGE1 2.  
  
-   [PASSAGGIO 9: Configurare EDGE1](assetId:///562744dc-30f6-42fa-bd5f-60a013b2179e). Configurare il server DNS 2-Corpnet e il routing in EDGE1.  
  
-   [PASSAGGIO 10: Installare e configurare EDGE1 2](assetId:///1938c4f3-ca96-475d-9f2e-6bea3b7a4130). Sono necessari due server di accesso remoto in una distribuzione multisito. 2-EDGE1 fornisce servizi di accesso remoto per il dominio di secondo.  
  
-   [PASSAGGIO 11: Configurare la distribuzione multisita](assetId:///537e4b68-043f-49c9-94d8-15ce8c4b18e2). Dopo la configurazione di entrambi i server Accesso remoto, è possibile configurare la distribuzione multisita.  
  
-   [PASSAGGIO 12: Testare la connettività DirectAccess](assetId:///aa293b5d-4b6f-4004-95f3-0ab54804b15c). Testare la connettività DirectAccess da entrambi i computer client dalla subnet Internet tramite EDGE1 e 2-EDGE1.  
  
-   [PASSAGGIO 13: Testare la connettività DirectAccess da dietro un dispositivo NAT](assetId:///41f8195b-00a1-4991-9db8-3703514dbe0c). Testare la connettività DirectAccess da dietro un dispositivo NAT.  
  
-   [PASSAGGIO 14: Uno snapshot della configurazione](assetId:///7b56d5c9-c334-463e-9e29-d652ca110d84). Dopo aver completato il lab di test, creare uno snapshot del working distribuzione multisito di accesso remoto in modo che è possibile tornarvi in un secondo momento per testare altri scenari.  
  


