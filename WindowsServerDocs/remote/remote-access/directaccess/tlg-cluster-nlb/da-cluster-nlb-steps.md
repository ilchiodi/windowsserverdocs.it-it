---
title: Passaggi per la configurazione del lab di test relativo al bilanciamento del carico di rete di un cluster DirectAccess
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess in un cluster con bilanciamento carico di servizio di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e508d3ee-ffa6-463f-a3dd-9e35e745c005
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 108c63298ad3382f5ece790258f2d278bb03b78b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388397"
---
# <a name="steps-for-configuring-the-directaccess-cluster-nlb-test-lab"></a>Passaggi per la configurazione del lab di test relativo al bilanciamento del carico di rete di un cluster DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Nei passaggi seguenti viene descritto come configurare l'infrastruttura di accesso remoto, configurare i server e i client di accesso remoto e testare la connettività DirectAccess dalle subnet Internet e HomeNET.  
  
In questa guida al Lab di test verrà compilato un cluster di accesso remoto abilitato per il bilanciamento del carico di rete eseguendo i passaggi seguenti:  
  
-   [Passaggio 1 completare la configurazione di DirectAccess](STEP-1-Complete-the-DirectAccess-Configuration.md). Completare tutti i passaggi della Guida al Lab [Test: Dimostrazione della configurazione del singolo server DirectAccess con IPv4 misto e IPv6 @ no__t-0.  
  
-   [PASSAGGIO 2: Configurare EDGE1 @ no__t-0. Configurare il ruolo accesso remoto in EDGE1 per il bilanciamento del carico.  
  
-   [PASSAGGIO 3: Installare e configurare EDGE2 @ no__t-0. EDGE2 funge da secondo server di accesso remoto in un cluster di accesso remoto.  
  
-   [PASSAGGIO 4: Creare il cluster di accesso remoto con bilanciamento del carico di rete @ no__t-0-EDGE1 è configurato come primo server in un cluster di accesso remoto. EDGE2 viene aggiunto al cluster e NLB è configurato per il cluster.  
  
-   [PASSAGGIO 5: Testare la connettività DirectAccess da Internet e tramite il cluster @ no__t-0. Al termine della configurazione di bilanciamento carico di servizio e cluster, è possibile testare la connettività del client DirectAccess tramite il cluster con bilanciamento del carico.  
  
-   [PASSAGGIO 6: Testare la connettività del client DirectAccess da dietro un dispositivo NAT @ no__t-0. Spostare il computer client dietro un dispositivo NAT per simulare il test della connettività del client DirectAccess da dietro un router Home.  
  
-   [PASSAGGIO 7: Testare la connettività quando si ritorna al corpnet @ no__t-0. Verificare che il computer client possa ancora accedere alle risorse aziendali quando torna a Corpnet.  
  
-   [PASSAGGIO 8: Snapshot della configurazione @ no__t-0. Dopo aver completato l'ambiente di testing, eseguire uno snapshot del cluster di bilanciamento carico di accesso remoto funzionante in modo che sia possibile ritornarvi in un secondo momento per testare altri scenari.  
  


