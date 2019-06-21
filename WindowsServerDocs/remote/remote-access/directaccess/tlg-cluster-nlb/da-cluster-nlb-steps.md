---
title: Passaggi per la configurazione del lab di test relativo al bilanciamento del carico di rete di un cluster DirectAccess
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare DirectAccess in un Cluster con bilanciamento carico di rete di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e508d3ee-ffa6-463f-a3dd-9e35e745c005
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2ebda017b41f27c2f69c7b850de44e771732415d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283323"
---
# <a name="steps-for-configuring-the-directaccess-cluster-nlb-test-lab"></a>Passaggi per la configurazione del lab di test relativo al bilanciamento del carico di rete di un cluster DirectAccess

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

I passaggi seguenti descrivono come configurare l'infrastruttura di accesso remoto, configurare il server di accesso remoto e i client e testare la connettività DirectAccess dalla subnet Internet e Homenet.  
  
In questo test Guida al lab di si compilerà un carico bilanciamento rete (NLB) abilitato cluster di accesso remoto eseguendo i passaggi seguenti:  
  
-   [PASSAGGIO 1 completare la configurazione di DirectAccess](STEP-1-Complete-the-DirectAccess-Configuration.md). Completare tutti i passaggi di [Guida al Lab di Test: Dimostrazione della Server singolo di DirectAccess con ambiente misto IPv4 e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [PASSAGGIO 2: Configurare EDGE1](STEP-2-Configure-EDGE1.md). Configurare il ruolo Accesso remoto in EDGE1 per il bilanciamento del carico.  
  
-   [PASSAGGIO 3: Installare e configurare EDGE2](STEP-3-Install-and-Configure-EDGE2.md). EDGE2 agisce come il secondo server di accesso remoto in un cluster di accesso remoto.  
  
-   [PASSAGGIO 4: Creare il cluster di accesso remoto con bilanciato del carico di rete](STEP-4-Create-the-Network-Load-Balanced-Remote-Access-Cluster.md)-EDGE1 è configurato come il primo server in un cluster di accesso remoto. EDGE2 viene aggiunto al cluster e bilanciamento carico di rete è configurato per il cluster.  
  
-   [PASSAGGIO 5: Testare la connettività DirectAccess da Internet e tramite il cluster](STEP-5-Test-DirectAccess-Connectivity-from-the-Internet-and-Through-the-Cluster.md). Al termine della configurazione di bilanciamento carico di rete e il cluster è possibile testare la connettività dei client DirectAccess tramite il cluster con bilanciamento di carico.  
  
-   [PASSAGGIO 6: Testare la connettività dei client DirectAccess da dietro un dispositivo NAT](STEP-6-Test-DirectAccess-Client-Connectivity-from-Behind-a-NAT-Device.md). Spostare il computer client dietro un dispositivo NAT per simulare test connettività dei client DirectAccess da dietro un router casalingo.  
  
-   [PASSAGGIO 7: Testare la connettività quando si torna alla rete aziendale](STEP-7-Test-Connectivity-When-Returning-to-the-Corpnet.md). Assicurarsi che il computer client possa comunque accedere alle risorse aziendali quando si torna alla rete aziendale.  
  
-   [PASSAGGIO 8: Uno snapshot della configurazione](da-cluster-nlb-s8-snapshot.md). Dopo aver completato il lab di test, creare uno snapshot del lavoro del cluster di bilanciamento carico di rete di accesso remoto in modo che è possibile tornarvi in un secondo momento per testare altri scenari.  
  


