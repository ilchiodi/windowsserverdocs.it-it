---
title: Passaggi per la configurazione del lab di test
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare DirectAccess con autenticazione OTP e SecurID RSA per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a40183c-afd1-43ca-b306-05745640a37d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0607506f2b6dd49284e6b377fb87da4f731eb94d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283086"
---
# <a name="steps-for-configuring-the-test-lab"></a>Passaggi per la configurazione del lab di test

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

I passaggi seguenti descrivono come configurare l'infrastruttura di accesso remoto, configurare il server di accesso remoto e il client e verificare la connettività DirectAccess dalle subnet Homenet e Internet.  
  
In questa Guida al lab di test si compilerà un accesso remoto con ambiente OTP attenendosi alla procedura seguente:  
  
-   [PASSAGGIO 1: Completare la configurazione di DirectAccess](assetId:///4dbf877f-02fb-439b-907a-f5b3f1d8afa6). Completare tutti i passaggi di [Guida al Lab di Test: Dimostrazione della Server singolo di DirectAccess con ambiente misto IPv4 e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [PASSAGGIO 2: Configurare APP1](assetId:///c1bb590f-91d4-4ed5-bceb-b0e36eabd4ff). Configurare APP1 con modelli di certificato OTP per l'uso da EDGE1.  
  
-   [PASSAGGIO 3: Configurare DC1](assetId:///904a6edc-a771-45ed-9630-a34a680bb522). Verificare che il nome dell'entità utente definite in DC1.  
  
-   [PASSAGGIO 7: Installare e configurare RSA](assetId:///baa4c28c-add7-42e2-8afd-ccc7a559406a). Installare e configurare il server RSA, un raggio e OTP e configurare EDGE1 per OTP.  
  
-   [PASSAGGIO 11: Verificare l'integrità OTP in EDGE1](assetId:///3b397a4a-8478-47f2-a932-9e8e048c14ba). Assicurarsi che lo stato di OTP sia integro nel server di accesso remoto.  
  
-   [PASSAGGIO 8: Testare la connettività DirectAccess dalla subnet Homenet](assetId:///ba1652a6-0692-4add-91ca-34a84956ba14). Verificare la funzionalità di OTP di DirectAccess da dietro un dispositivo NAT.  
  
-   [PASSAGGIO 10: Testare la connettività DirectAccess da Internet](assetId:///321149eb-5f23-4a0b-b8fb-1244540126e9). Testare la connettività dei client DirectAccess da Internet.  
  
-   [PASSAGGIO 12: Uno snapshot della configurazione](assetId:///8a51ed3c-9c32-402f-85d1-617ce46845b4). Dopo aver completato il lab di test, creare uno snapshot del working DirectAccess con la configurazione di OTP in modo che è possibile tornarvi in un secondo momento per testare altri scenari.  
  


