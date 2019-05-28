---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: Certificati per le comunicazioni di servizi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b781d6fe99864b13d6e7f8ab65f3a14d205c2aa6
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190800"
---
# <a name="service-communications-certificates"></a>Certificati per le comunicazioni di servizi

Un server federativo richiede l'uso di certificati per le comunicazioni del servizio per gli scenari in cui viene utilizzata protezione dei messaggi WCF.  
  
## <a name="service-communication-certificate-requirements"></a>Requisiti di certificato di comunicazione del servizio  
Certificati per le comunicazioni del servizio devono soddisfare i requisiti seguenti per lavorare con AD FS:  
  
-   Certificato di comunicazione del servizio deve includere l'utilizzo chiavi avanzato di autenticazione server \(EKU\) estensione.  
  
-   Gli elenchi di revoche di certificati \(CRL\) deve risultare accessibile per tutti i certificati nella catena dal certificato di comunicazione del servizio per il certificato CA radice. La CA radice deve anche essere considerato attendibile da qualsiasi server federativi e i server Web che considera attendibile il server federativo.  
  
-   Il nome del soggetto che viene utilizzato nel certificato di comunicazione del servizio deve corrispondere al nome servizio federativo nelle proprietà del servizio federativo.  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>Considerazioni sulla distribuzione di certificati per le comunicazioni del servizio  
Configurare certificati per le comunicazioni del servizio in modo che tutti i server federativi usano lo stesso certificato. Se si sta distribuendo singolo Web federata\-Sign\-sul \(SSO\) progettazione, è consigliabile che il certificato di comunicazione del servizio vengano emessi da un'autorità di certificazione pubblica. È possibile richiedere e installare tali certificati tramite lo snap di gestione IIS\-in.  
  
È possibile usare Auto\-firmato, a Servizi certificati per le comunicazioni correttamente su server federativi in un ambiente di testing. Tuttavia, per un ambiente di produzione, si consiglia di ottenere certificati per le comunicazioni del servizio da una CA pubblica. Di seguito sono motivi perché non è consigliabile utilizzare self\-firmato, a Servizi certificati per le comunicazioni per una distribuzione in tempo reale:  
  
-   Un self\-firmato, certificato SSL deve essere aggiunto all'archivio radice attendibile su ogni server federativo nell'organizzazione partner risorse. Mentre questo da solo non abilita un utente malintenzionato di compromettere un server federativo di risorsa, concessione dell'attendibilità self\-certificati firmati aumenta la superficie di attacco di un computer e può causare vulnerabilità di sicurezza se il certificato firmatario non è Trustworthy.  
  
-   Crea un'esperienza utente non valido. I client riceveranno istruzioni degli avvisi di sicurezza quando si tenta di accedere alle risorse federative che consentono di visualizzare il messaggio seguente: "Il certificato di sicurezza rilasciato da una società che non si è scelto di considerare attendibile." Questo comportamento è previsto, perché il self\-certificato firmato non è attendibile.  
  
    > [!NOTE]  
    > Se necessario, è possibile risolvere questa condizione usando criteri di gruppo per eseguire manualmente il push down self\-firmati certificati nell'archivio radice attendibile in ogni computer client che verrà eseguito un tentativo di accedere a un sito di AD FS.  
  
-   Le autorità di certificazione forniscono certificato aggiuntivo\-basato su funzionalità, ad esempio revoca, archivio di chiavi private e rinnovo non forniti da self\-certificati autofirmati.  
  

