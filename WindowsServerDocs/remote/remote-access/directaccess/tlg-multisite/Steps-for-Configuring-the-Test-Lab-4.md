---
title: Passaggi per la configurazione del lab di test
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8a0d77208425774c8ea2b4fc663fc43e2a02ee5b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314531"
---
# <a name="steps-for-configuring-the-test-lab"></a>Passaggi per la configurazione del lab di test

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Nei passaggi seguenti viene descritto come configurare l'infrastruttura di accesso remoto, configurare i server e i client di accesso remoto e testare la connettività DirectAccess dalle subnet Internet e HomeNET.  
  
In questa guida al Lab di test verrà creata una distribuzione di accesso remoto multisito eseguendo la procedura seguente:  
  
-   [Passaggio 1: completare la configurazione di base](assetId:///9eb4a9ba-9118-4ea3-8963-e643ec81c3ed). Completare tutti i passaggi della [Guida al Lab di test: dimostrazione della configurazione del singolo server DirectAccess con IPv4 misto e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [Passaggio 2: installare e configurare ROUTER1](assetId:///e4b1a298-d5b0-410e-970b-c5358a9378f9). ROUTER1 fornisce funzionalità di routing e inoltro tra le subnet Corpnet e 2-Corpnet.  
  
-   [Passaggio 3: installare e configurare CLIENT2](assetId:///6cbee1b5-f6f6-443f-8fa9-31cc5c05a0ee). CLIENT2 è un computer client Windows 7 usato per illustrare la compatibilità con le versioni precedenti di una distribuzione di accesso remoto di Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
-   [Passaggio 4: configurare App1](assetId:///a0ee655e-c01e-4bf3-a7b3-064e9614f810). Configurare APP1 con ROUTER1 come gateway predefinito e 2-DC1 come server DNS alternativo.  
  
-   [Passaggio 5: configurare DC1](assetId:///205ca795-93ce-4e53-aa6b-b44c87f0e14a). Configurare DC1 con un sito di Active Directory aggiuntivo e altri gruppi di sicurezza per i computer client Windows 7.  
  
-   [Passaggio 6: installare e configurare 2-DC1](assetId:///16752f61-edbf-4ff4-9d7a-e2077b66a127). In una distribuzione multisito sono presenti due o più domini e siti. 2-DC1 fornisce il controller di dominio e i servizi DNS per il dominio corp2.corp.contoso.com.  
  
-   [Passaggio 7: installare e configurare 2 App1](assetId:///7d04b54e-590a-4d33-9766-415789859f29). 2-APP1 è un Web e file server nella rete corpnet.  
  
-   [Passaggio 8: configurare computer Inet1](assetId:///8ecc0b63-8626-4939-8d26-3d51d051d231). COMPUTER INET1 simula Internet in questa guida al Lab di test. È necessario configurare una voce DNS che viene risolta nell'indirizzo IP pubblico di 2-EDGE1.  
  
-   [Passaggio 9: configurare Edge1](assetId:///562744dc-30f6-42fa-bd5f-60a013b2179e). Configurare il server DNS 2-corpnet e il routing in EDGE1.  
  
-   [Passaggio 10: installare e configurare 2 Edge1](assetId:///1938c4f3-ca96-475d-9f2e-6bea3b7a4130). In una distribuzione multisito sono necessari due server di accesso remoto. 2-EDGE1 fornisce servizi di accesso remoto per il secondo dominio.  
  
-   [Passaggio 11: configurare la distribuzione multisito](assetId:///537e4b68-043f-49c9-94d8-15ce8c4b18e2). Dopo aver configurato entrambi i server di accesso remoto, è possibile configurare la distribuzione multisito.  
  
-   [Passaggio 12: testare la connettività di DirectAccess](assetId:///aa293b5d-4b6f-4004-95f3-0ab54804b15c). Testare la connettività DirectAccess da entrambi i computer client dalla subnet Internet tramite EDGE1 e 2-EDGE1.  
  
-   [Passaggio 13: testare la connettività DirectAccess da dietro un dispositivo NAT](assetId:///41f8195b-00a1-4991-9db8-3703514dbe0c). Testare la connettività DirectAccess da dietro un dispositivo NAT.  
  
-   [Passaggio 14: eseguire uno snapshot della configurazione](assetId:///7b56d5c9-c334-463e-9e29-d652ca110d84). Dopo aver completato il Lab di test, eseguire uno snapshot della distribuzione multisito di accesso remoto funzionante per potervi tornare in seguito per testare altri scenari.  
  


