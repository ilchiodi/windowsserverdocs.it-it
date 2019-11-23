---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: Certificati per le comunicazioni di servizi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 624d2e26bc0277129e44eee3fdce7c7396b735a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407903"
---
# <a name="service-communications-certificates"></a>Certificati per le comunicazioni di servizi

Un server federativo richiede l'utilizzo di certificati per la comunicazione di servizi per gli scenari in cui viene utilizzata la sicurezza dei messaggi WCF.  
  
## <a name="service-communication-certificate-requirements"></a>Requisiti del certificato di comunicazione del servizio  
I certificati di comunicazione dei servizi devono soddisfare i requisiti seguenti per lavorare con AD FS:  
  
-   Il certificato di comunicazione del servizio deve includere l'utilizzo chiavi avanzato di autenticazione server \(estensione EKU\).  
  
-   Gli elenchi di revoche di certificati \(CRL\) devono essere accessibili per tutti i certificati nella catena dal certificato di comunicazione del servizio al certificato CA radice. La CA radice deve anche essere considerata attendibile da eventuali proxy server federativi e server Web che considerano attendibile il server federativo.  
  
-   Il nome del soggetto utilizzato nel certificato di comunicazione del servizio deve corrispondere al nome del Servizio federativo nelle proprietà del Servizio federativo.  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>Considerazioni sulla distribuzione per i certificati di comunicazione dei servizi  
Configurare i certificati di comunicazione del servizio in modo che tutti i server federativi usino lo stesso certificato. Se si distribuisce il\-di accesso Single Sign-on Web federativo\-in \(SSO\) progettazione, è consigliabile che il certificato di comunicazione del servizio venga emesso da una CA pubblica. È possibile richiedere e installare questi certificati tramite lo snap-in Gestione IIS\-in.  
  
È possibile usare i certificati auto\-firmati per la comunicazione dei servizi nei server federativi in un ambiente lab di test. Tuttavia, per un ambiente di produzione, è consigliabile ottenere i certificati di comunicazione del servizio da una CA pubblica. Di seguito sono riportati i motivi per cui non è consigliabile utilizzare i certificati auto\-firmati per la comunicazione dei servizi per una distribuzione in tempo reale:  
  
-   Un certificato SSL auto\-firmato deve essere aggiunto all'archivio radice attendibile in ogni server federativo nell'organizzazione partner risorse. Anche se da solo non consente a un utente malintenzionato di compromettere un server federativo di risorsa, l'attendibilità dei certificati auto\-firmati aumenta la superficie di attacco di un computer e può causare vulnerabilità di sicurezza se il firmatario del certificato non è attendibile.  
  
-   Consente di creare un'esperienza utente non valida. I client riceveranno le richieste di avviso di sicurezza quando tenteranno di accedere alle risorse federate che visualizzano il messaggio seguente: "il certificato di sicurezza è stato emesso da una società che non si è scelto di considerare attendibile". Si tratta di un comportamento previsto, perché il certificato auto\-firmato non è attendibile.  
  
    > [!NOTE]  
    > Se necessario, è possibile aggirare questa condizione utilizzando Criteri di gruppo per eseguire manualmente il push del certificato auto\-firmato nell'archivio radice attendibile in ogni computer client che tenterà di accedere a un sito di AD FS.  
  
-   Le autorità di certificazione forniscono funzionalità aggiuntive basate su\-di certificati, ad esempio l'archivio di chiavi private, il rinnovo e la revoca, che non vengono fornite da certificati auto\-firmati.  
  

