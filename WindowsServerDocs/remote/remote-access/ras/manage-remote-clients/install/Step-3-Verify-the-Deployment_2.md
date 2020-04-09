---
title: Passaggio 3 verificare la distribuzione
description: Questo argomento fa parte della Guida relativa alla gestione remota dei client DirectAccess in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 6a78a078-d2e7-4cbd-b8d5-20cfb6d1524b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8b936b335d0f5d574edbc3ec7c88b7b36fcac767
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859154"
---
# <a name="step-3-verify-the-deployment"></a>Passaggio 3 verificare la distribuzione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento descrive come verificare di avere configurato correttamente la distribuzione per la gestione remota dei client DirectAccess.  
  
### <a name="to-verify-proper-deployment"></a>Per verificare la corretta distribuzione  
  
1.  Connettere un computer client DirectAccess alla rete aziendale e ottenere l'oggetto Criteri di gruppo.  
  
2.  Nel computer client fare clic sull'icona **connessioni di rete** nell'area di notifica per accedere a gestione supporti DirectAccess.  
  
3.  Fare clic su **connessione DirectAccess**. si noterà che lo stato è **connesso localmente**.  
  
4.  Rimuovere il computer dalla rete aziendale e connetterlo a una rete pubblica.  
  
5.  Al prompt dei comandi digitare **nltest/dsgetdc: [nome di dominio completo]** . Questo comando verificherà che la rete aziendale sia accessibile al client. Se il controller di dominio non è accessibile, verrà visualizzato il messaggio di errore seguente per segnalare che il dominio non esiste: ERROR_NO_SUCH_DOMAIN.  
  


