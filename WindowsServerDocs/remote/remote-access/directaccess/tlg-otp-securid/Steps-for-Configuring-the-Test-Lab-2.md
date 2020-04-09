---
title: Passaggi per la configurazione del lab di test
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess con autenticazione OTP e RSA SecurID per Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 0a40183c-afd1-43ca-b306-05745640a37d
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fc40bf8f1b4bcb3d28b23abd981575a6438f871e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814384"
---
# <a name="steps-for-configuring-the-test-lab"></a>Passaggi per la configurazione del lab di test

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Nei passaggi seguenti viene descritto come configurare l'infrastruttura di accesso remoto, configurare il client e il server di accesso remoto e testare la connettività DirectAccess dalle subnet HomeNET e Internet.  
  
In questa guida al Lab di test verrà creato un accesso remoto con ambiente OTP attenendosi alla procedura seguente:  
  
-   [Passaggio 1: completare la configurazione di DirectAccess](assetId:///4dbf877f-02fb-439b-907a-f5b3f1d8afa6). Completare tutti i passaggi della [Guida al Lab di test: dimostrazione della configurazione del singolo server DirectAccess con IPv4 misto e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [Passaggio 2: configurare App1](assetId:///c1bb590f-91d4-4ed5-bceb-b0e36eabd4ff). Configurare APP1 con i modelli di certificato OTP da usare con EDGE1.  
  
-   [Passaggio 3: configurare DC1](assetId:///904a6edc-a771-45ed-9630-a34a680bb522). Verificare il nome dell'entità utente definito in DC1.  
  
-   [Passaggio 7: installare e configurare RSA](assetId:///baa4c28c-add7-42e2-8afd-ccc7a559406a). Installare e configurare RSA, un server RADIUS e OTP e configurare EDGE1 per OTP.  
  
-   [Passaggio 11: verificare l'integrità OTP in Edge1](assetId:///3b397a4a-8478-47f2-a932-9e8e048c14ba). Verificare che lo stato di OTP sia integro nel server di accesso remoto.  
  
-   [Passaggio 8: testare la connettività DirectAccess dalla subnet HomeNET](assetId:///ba1652a6-0692-4add-91ca-34a84956ba14). Testare le funzionalità OTP di DirectAccess da dietro un dispositivo NAT.  
  
-   [Passaggio 10: testare la connettività DirectAccess da Internet](assetId:///321149eb-5f23-4a0b-b8fb-1244540126e9). Testare la connettività del client DirectAccess da Internet.  
  
-   [Passaggio 12: eseguire uno snapshot della configurazione](assetId:///8a51ed3c-9c32-402f-85d1-617ce46845b4). Dopo aver completato il Lab di test, eseguire uno snapshot della configurazione di DirectAccess con OTP in modo che sia possibile tornare in seguito per testare altri scenari.  
  


