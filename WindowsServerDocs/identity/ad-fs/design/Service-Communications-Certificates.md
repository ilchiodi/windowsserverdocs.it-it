---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: Certificati per le comunicazioni di servizio
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 32e60f2c2d9e4fced04061ace44882b792e1a3bc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="service-communications-certificates"></a>Certificati per le comunicazioni di servizio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un server federativo richiede l'utilizzo di certificati per le comunicazioni del servizio per gli scenari in cui viene usata sicurezza dei messaggi WCF.  
  
## <a name="service-communication-certificate-requirements"></a>Requisiti di certificato di comunicazione del servizio  
Certificati per le comunicazioni del servizio devono soddisfare i requisiti seguenti per lavorare con ADFS:  
  
-   Certificato di comunicazione del servizio deve includere l'estensione del \(EKU\) Utilizzo chiavi avanzato di autenticazione server.  
  
-   Il \(CRLs\) gli elenchi di revoca certificato deve essere accessibile per tutti i certificati nella catena dal certificato di comunicazione del servizio per il certificato CA radice. La CA radice deve anche essere considerato attendibile da qualsiasi server federativi e i server Web che considera attendibile il server federativo.  
  
-   Il nome del soggetto che viene utilizzato nel certificato di comunicazione del servizio deve corrispondere al nome servizio federativo nelle proprietà del servizio federativo.  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>Considerazioni sulla distribuzione di certificati per le comunicazioni del servizio  
Configurare i certificati per le comunicazioni del servizio in modo che tutti i server federativi usano lo stesso certificato. Se si distribuisce il progetto \(SSO\) Single\-Sign\-On Web federata, è consigliabile che il certificato di comunicazione del servizio deve essere emesso da una CA pubblica. È possibile richiedere e installare tali certificati tramite Gestione IIS snap-in.  
  
È possibile utilizzare e firmati, la comunicazione dei servizi certificati correttamente nel server federativo in un ambiente di testing. Tuttavia, per un ambiente di produzione, si consiglia di ottenere certificati per le comunicazioni del servizio da una CA pubblica. Perché è necessario non utilizzare e firmato, servizio certificati per le comunicazioni per una distribuzione live i motivi sono i seguenti:  
  
-   Un e-firmato, certificato SSL deve essere aggiunto all'archivio radice attendibile in ogni server federativo nell'organizzazione partner risorse. Mentre da soli non abilita un utente malintenzionato di compromettere un server federativo di risorsa, attendibili i certificati firmati e aumenta la superficie di attacco di un computer e può causare problemi di protezione se non è attendibile il firmatario di certificato.  
  
-   Crea un'esperienza utente negativa. I client riceveranno le richieste di avviso di protezione quando si tenta di accedere alle risorse federative che visualizza il messaggio seguente: "il certificato di sicurezza emesso da una società non si è scelto di attendibilità". Questo è il comportamento previsto, perché il certificato firmato e non è attendibile.  
  
    > [!NOTE]  
    > Se necessario, è possibile ovviare a questa condizione utilizzando criteri di gruppo per manualmente il certificato di firma e all'archivio radice attendibile in ogni computer client che verrà eseguito un tentativo di accedere a un sito di ADFS.  
  
-   Autorità di certificazione forniscono basate su certificate\ funzionalità aggiuntive, come archivio di chiavi private, rinnovo e revoche di certificati, che non vengono fornite per i certificati firmati e.  
  

