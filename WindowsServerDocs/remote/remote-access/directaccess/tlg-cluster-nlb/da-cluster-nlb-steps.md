---
title: Passaggi per la configurazione del lab di test relativo al bilanciamento del carico di rete di un cluster DirectAccess
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess in un cluster con bilanciamento carico di servizio di Windows per Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: e508d3ee-ffa6-463f-a3dd-9e35e745c005
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c4f683cd9745bdb61ee95d89bcfc37622a4acf2c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853014"
---
# <a name="steps-for-configuring-the-directaccess-cluster-nlb-test-lab"></a>Passaggi per la configurazione del lab di test relativo al bilanciamento del carico di rete di un cluster DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Nei passaggi seguenti viene descritto come configurare l'infrastruttura di accesso remoto, configurare i server e i client di accesso remoto e testare la connettività DirectAccess dalle subnet Internet e HomeNET.  
  
In questa guida al Lab di test verrà compilato un cluster di accesso remoto abilitato per il bilanciamento del carico di rete eseguendo i passaggi seguenti:  
  
-   [Passaggio 1 completare la configurazione di DirectAccess](STEP-1-Complete-the-DirectAccess-Configuration.md). Completare tutti i passaggi della [Guida al Lab di test: dimostrazione della configurazione del singolo server DirectAccess con IPv4 misto e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [Passaggio 2: configurare Edge1](STEP-2-Configure-EDGE1.md). Configurare il ruolo accesso remoto in EDGE1 per il bilanciamento del carico.  
  
-   [Passaggio 3: installare e configurare EDGE2](STEP-3-Install-and-Configure-EDGE2.md). EDGE2 funge da secondo server di accesso remoto in un cluster di accesso remoto.  
  
-   [Passaggio 4: creare il cluster di accesso remoto con bilanciamento del carico di rete](STEP-4-Create-the-Network-Load-Balanced-Remote-Access-Cluster.md)-Edge1 è configurato come primo server in un cluster di accesso remoto. EDGE2 viene aggiunto al cluster e NLB è configurato per il cluster.  
  
-   [Passaggio 5: testare la connettività DirectAccess da Internet e tramite il cluster](STEP-5-Test-DirectAccess-Connectivity-from-the-Internet-and-Through-the-Cluster.md). Al termine della configurazione di bilanciamento carico di servizio e cluster, è possibile testare la connettività del client DirectAccess tramite il cluster con bilanciamento del carico.  
  
-   [Passaggio 6: testare la connettività del client DirectAccess da dietro un dispositivo NAT](STEP-6-Test-DirectAccess-Client-Connectivity-from-Behind-a-NAT-Device.md). Spostare il computer client dietro un dispositivo NAT per simulare il test della connettività del client DirectAccess da dietro un router Home.  
  
-   [Passaggio 7: testare la connettività quando si ritorna a corpnet](STEP-7-Test-Connectivity-When-Returning-to-the-Corpnet.md). Verificare che il computer client possa ancora accedere alle risorse aziendali quando torna a Corpnet.  
  
-   [Passaggio 8: eseguire uno snapshot della configurazione](da-cluster-nlb-s8-snapshot.md). Dopo aver completato l'ambiente di testing, eseguire uno snapshot del cluster di bilanciamento carico di accesso remoto funzionante in modo che sia possibile ritornarvi in un secondo momento per testare altri scenari.  
  


