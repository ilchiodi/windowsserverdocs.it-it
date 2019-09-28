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
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dd8b8864dff98e51bf55aad9307523df4a0c30bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404700"
---
# <a name="steps-for-configuring-the-test-lab"></a>Passaggi per la configurazione del lab di test

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Nei passaggi seguenti viene descritto come configurare l'infrastruttura di accesso remoto, configurare i server e i client di accesso remoto e testare la connettività DirectAccess dalle subnet Internet e HomeNET.  
  
In questa guida al Lab di test verrà creata una distribuzione di accesso remoto multisito eseguendo la procedura seguente:  
  
-   [PASSAGGIO 1: Completare la configurazione di base @ no__t-0. Completare tutti i passaggi della Guida al Lab [Test: Dimostrazione della configurazione del singolo server DirectAccess con IPv4 misto e IPv6 @ no__t-0.  
  
-   [PASSAGGIO 2: Installare e configurare ROUTER1 @ no__t-0. ROUTER1 fornisce funzionalità di routing e inoltro tra le subnet Corpnet e 2-Corpnet.  
  
-   [PASSAGGIO 3: Installare e configurare CLIENT2 @ no__t-0. CLIENT2 è un computer client Windows 7 usato per illustrare la compatibilità con le versioni precedenti di una distribuzione di accesso remoto di Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
-   [PASSAGGIO 4: Configurare APP1 @ no__t-0. Configurare APP1 con ROUTER1 come gateway predefinito e 2-DC1 come server DNS alternativo.  
  
-   [PASSAGGIO 5: Configurare DC1 @ no__t-0. Configurare DC1 con un sito di Active Directory aggiuntivo e altri gruppi di sicurezza per i computer client Windows 7.  
  
-   [PASSAGGIO 6: Installare e configurare 2-DC1 @ no__t-0. In una distribuzione multisito sono presenti due o più domini e siti. 2-DC1 fornisce il controller di dominio e i servizi DNS per il dominio corp2.corp.contoso.com.  
  
-   [PASSAGGIO 7: Installare e configurare 2-APP1 @ no__t-0. 2-APP1 è un Web e file server nella rete corpnet.  
  
-   [PASSAGGIO 8: Configurare computer INET1 @ no__t-0. COMPUTER INET1 simula Internet in questa guida al Lab di test. È necessario configurare una voce DNS che viene risolta nell'indirizzo IP pubblico di 2-EDGE1.  
  
-   [PASSAGGIO 9: Configurare EDGE1 @ no__t-0. Configurare il server DNS 2-corpnet e il routing in EDGE1.  
  
-   [PASSAGGIO 10: Installare e configurare 2-EDGE1 @ no__t-0. In una distribuzione multisito sono necessari due server di accesso remoto. 2-EDGE1 fornisce servizi di accesso remoto per il secondo dominio.  
  
-   [PASSAGGIO 11: Configurare la distribuzione multisito @ no__t-0. Dopo aver configurato entrambi i server di accesso remoto, è possibile configurare la distribuzione multisito.  
  
-   [PASSAGGIO 12: Testare la connettività DirectAccess @ no__t-0. Testare la connettività DirectAccess da entrambi i computer client dalla subnet Internet tramite EDGE1 e 2-EDGE1.  
  
-   [PASSAGGIO 13: Testare la connettività DirectAccess da dietro un dispositivo NAT @ no__t-0. Testare la connettività DirectAccess da dietro un dispositivo NAT.  
  
-   [PASSAGGIO 14: Snapshot della configurazione @ no__t-0. Dopo aver completato il Lab di test, eseguire uno snapshot della distribuzione multisito di accesso remoto funzionante per potervi tornare in seguito per testare altri scenari.  
  


