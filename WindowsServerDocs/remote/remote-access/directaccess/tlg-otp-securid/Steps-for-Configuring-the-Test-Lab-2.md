---
title: Passaggi per la configurazione del lab di test
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess con autenticazione OTP e RSA SecurID per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a40183c-afd1-43ca-b306-05745640a37d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ef5ce37983b8565fab8287eeaae7423be0c269f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404731"
---
# <a name="steps-for-configuring-the-test-lab"></a>Passaggi per la configurazione del lab di test

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Nei passaggi seguenti viene descritto come configurare l'infrastruttura di accesso remoto, configurare il client e il server di accesso remoto e testare la connettività DirectAccess dalle subnet HomeNET e Internet.  
  
In questa guida al Lab di test verrà creato un accesso remoto con ambiente OTP attenendosi alla procedura seguente:  
  
-   [PASSAGGIO 1: Completare la configurazione di DirectAccess @ no__t-0. Completare tutti i passaggi della Guida al Lab [Test: Dimostrazione della configurazione del singolo server DirectAccess con IPv4 misto e IPv6 @ no__t-0.  
  
-   [PASSAGGIO 2: Configurare APP1 @ no__t-0. Configurare APP1 con i modelli di certificato OTP da usare con EDGE1.  
  
-   [PASSAGGIO 3: Configurare DC1 @ no__t-0. Verificare il nome dell'entità utente definito in DC1.  
  
-   [PASSAGGIO 7: Installare e configurare RSA @ no__t-0. Installare e configurare RSA, un server RADIUS e OTP e configurare EDGE1 per OTP.  
  
-   [PASSAGGIO 11: Verificare l'integrità OTP in EDGE1 @ no__t-0. Verificare che lo stato di OTP sia integro nel server di accesso remoto.  
  
-   [PASSAGGIO 8: Testare la connettività DirectAccess dalla subnet HomeNET @ no__t-0. Testare le funzionalità OTP di DirectAccess da dietro un dispositivo NAT.  
  
-   [PASSAGGIO 10: Testare la connettività DirectAccess da Internet @ no__t-0. Testare la connettività del client DirectAccess da Internet.  
  
-   [PASSAGGIO 12: Snapshot della configurazione @ no__t-0. Dopo aver completato il Lab di test, eseguire uno snapshot della configurazione di DirectAccess con OTP in modo che sia possibile tornare in seguito per testare altri scenari.  
  


