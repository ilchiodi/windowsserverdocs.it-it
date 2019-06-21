---
title: Passaggio 4 verificare la distribuzione Multisita
description: Questo argomento fa parte della Guida alla distribuzione di più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 345b676a-a397-4d51-9973-8b25bc05fa55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f186084e9d2dbeabbc560ce7631e7d63ff1b99b8
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281016"
---
# <a name="step-4-verify-the-multisite-deployment"></a>Passaggio 4 verificare la distribuzione Multisita

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento viene descritto come verificare di aver configurato correttamente la distribuzione multisita di accesso remoto.  
  
### <a name="to-verify-access-to-internal-resources-through-the-multisite-deployment"></a>Per verificare l'accesso alle risorse interne tramite la distribuzione multisita  
  
1.  Connettere un computer client DirectAccess alla rete aziendale e ottenere i criteri di gruppo.  
  
2.  Connettere il computer client alla rete esterna e tentare di accedere alle risorse interne.  
  
    Si dovrebbe poter accedere a tutte le risorse aziendali.  
  
3.  Testare la connettività tramite ogni server nella distribuzione multisito per la disattivazione o la disconnessione dalla rete esterna, tutte tranne uno dei server Accesso remoto. Nel computer client, tentare di accedere alle risorse aziendali. Ripetere il test in un altro server multisito. Potrebbero occorrere fino a 10 minuti per il computer client per connettersi al nuovo punto di ingresso. Questo avviene perché individuazione tramite probe viene disattivato per 10 minuti per un punto di ingresso dopo questo viene considerato non è raggiungibile, per ottimizzare la larghezza di banda e durata delle batterie. In alternativa, è possibile spostarsi tra vari punti di ingresso manualmente scegliendo il punto di ingresso desiderato dalla casella combinata visualizzata quando si esegue **daprop.exe**.  
  
    È necessario essere in grado di accedere a tutte le risorse aziendali tramite ogni server multisito.  
  
4.  Connettersi a un Windows 7&reg; del computer client della sede centrale di rete e ottenere i criteri di gruppo.  
  
5.  Connettere il computer client Windows 7 alla rete esterna e tentare di accedere alle risorse interne.  
  
    Si dovrebbe poter accedere a tutte le risorse aziendali.  
  
6.  Testare la connettività per i client Windows 7 tramite ogni server nella distribuzione multisito per l'accesso alla console di Active Directory Users and Computers e spostare il computer client al gruppo di sicurezza che corrisponde a ogni server. Dopo le modifiche replicate in tutto il dominio, riavviare il computer client durante la connessione alla rete aziendale per ottenere i nuovi criteri di gruppo. Tentativo di accedere alle risorse aziendali. Ripetere il test in un altro server multisito.  
  
    È necessario essere in grado di accedere a tutte le risorse aziendali tramite ogni server multisito.  
  
    In un ambiente di produzione questo metodo potrebbe non essere fattibile a causa della quantità di tempo necessaria affinché le modifiche vengano replicate in tutto il dominio. È possibile forzare la replica laddove possibile. Test possono essere eseguiti anche da più computer client Windows 7 diverse che fanno già parte di gruppi di sicurezza di Windows 7 diversi nella distribuzione multisito.  
  


