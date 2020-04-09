---
title: Passaggio 4 verificare la distribuzione multisito
description: Questo argomento fa parte della Guida distribuire più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 345b676a-a397-4d51-9973-8b25bc05fa55
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1f17f85104b59052de2e1accc20adb2579ac79f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858354"
---
# <a name="step-4-verify-the-multisite-deployment"></a>Passaggio 4 verificare la distribuzione multisito

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento descrive come verificare di avere configurato correttamente la distribuzione multisito di accesso remoto.  
  
### <a name="to-verify-access-to-internal-resources-through-the-multisite-deployment"></a>Per verificare l'accesso alle risorse interne tramite la distribuzione multisito  
  
1.  Connettere un computer client DirectAccess alla rete aziendale e ottenere i criteri di gruppo.  
  
2.  Connettere il computer client alla rete esterna e tentare di accedere alle risorse interne.  
  
    Si dovrebbe poter accedere a tutte le risorse aziendali.  
  
3.  Testare la connettività tramite ogni server nella distribuzione multisito disattivando o disconnettendosi dalla rete esterna, tutti tranne uno dei server di accesso remoto. Nel computer client tentare di accedere alle risorse aziendali. Ripetere il test in un server multisito diverso. Potrebbero essere necessari fino a 10 minuti prima che il computer client si connetta al nuovo punto di ingresso. Questo è dovuto al fatto che il sondaggio è disattivato per 10 minuti per un punto di ingresso dopo che è stato ritenuto irraggiungibile, per ottimizzare la larghezza di banda e la durata della batteria. In alternativa, è possibile passare manualmente tra i vari punti di ingresso scegliendo il punto di ingresso desiderato dalla casella combinata visualizzata quando si esegue **daprop. exe**.  
  
    Dovrebbe essere possibile accedere a tutte le risorse aziendali tramite ciascun server multisito.  
  
4.  Connettere un computer client Windows 7&reg; alla rete aziendale e ottenere i criteri di gruppo.  
  
5.  Connettere il computer client Windows 7 alla rete esterna e tentare di accedere alle risorse interne.  
  
    Si dovrebbe poter accedere a tutte le risorse aziendali.  
  
6.  Testare la connettività per i client Windows 7 tramite ogni server nella distribuzione multisito accedendo alla console Active Directory utenti e computer e spostando il computer client nel gruppo di sicurezza che corrisponde a ogni server. Una volta che le modifiche sono state replicate in tutto il dominio, riavviare il computer client durante la connessione alla rete aziendale per ottenere i nuovi criteri di gruppo. Tentativo di accedere alle risorse aziendali. Ripetere il test in un server multisito diverso.  
  
    Dovrebbe essere possibile accedere a tutte le risorse aziendali tramite ciascun server multisito.  
  
    In un ambiente di produzione questo metodo potrebbe non essere fattibile a causa della quantità di tempo necessaria per la replica delle modifiche in tutto il dominio. Potrebbe essere necessario forzare la replica laddove possibile. I test possono essere eseguiti anche da più computer client Windows 7 che sono già membri dei diversi gruppi di sicurezza di Windows 7 nella distribuzione multisito.  
  


