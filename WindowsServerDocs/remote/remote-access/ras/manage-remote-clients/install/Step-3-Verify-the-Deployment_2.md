---
title: Passaggio 3 verificare la distribuzione
description: Questo argomento fa parte della Guida di client DirectAccess di gestire in remoto in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a78a078-d2e7-4cbd-b8d5-20cfb6d1524b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8d2b0b27d4b1d33971564672954667b49a87a4e0
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282771"
---
# <a name="step-3-verify-the-deployment"></a>Passaggio 3 verificare la distribuzione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento viene descritto come verificare di aver configurato correttamente la distribuzione per la gestione remota dei client DirectAccess.  
  
### <a name="to-verify-proper-deployment"></a>Per verificare la corretta distribuzione  
  
1.  Connettere un computer client DirectAccess alla rete aziendale e ottenere l'oggetto Criteri di gruppo.  
  
2.  Sul computer client, scegliere il **connessioni di rete** sull'icona nell'area di notifica per accedere a Gestione supporti DirectAccess.  
  
3.  Fare clic su **connessione DirectAccess**, si noterà che lo stato sia **connessi localmente**.  
  
4.  Rimuovere il computer dalla rete aziendale e connetterla a una rete pubblica.  
  
5.  Un prompt dei comandi, digitare **nltest /dsgetdc: [nome di dominio completo]** . Questo comando verifica che la rete aziendale sia accessibile al client. Se il controller di dominio non è accessibile, verrà visualizzato il seguente messaggio di errore reporting che il dominio non esiste: ERROR_NO_SUCH_DOMAIN.  
  


