---
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: Certificati di firma di token
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 84e106ec87861960d7de651b4aaa0e88c952aa79
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="token-signing-certificates"></a>Certificati di firma di token

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I server federativi richiedono i certificati di firma token\ a utenti malintenzionati di modificare o contraffare i token di sicurezza nel tentativo di accesso non autorizzato alle risorse federative. La chiave pubblica/private\ vale a dire l'associazione utilizzato con i certificati di firma token\ è il meccanismo di convalida più importante di qualsiasi collaborazione federata poiché queste chiavi verificare che un token di sicurezza emesso da un server federativo partner valido e che il token non è stato modificato durante il transito.  
  
## <a name="token-signing-certificate-requirements"></a>Firma Token\ requisiti dei certificati  
Un certificato di firma token\ deve soddisfare i requisiti seguenti per lavorare con ADFS:  
  
-   Per un certificato di firma token\ firmare un token di sicurezza, il certificato di firma token\ deve contenere una chiave privata.  
  
-   L'account del servizio ADFS deve avere accesso alla chiave privata del certificato di firma token\ nell'archivio personale del computer locale. Questo viene preso in considerazione dal programma di installazione. È anche possibile utilizzare la gestione di ADFS snap-in per verificare l'accesso se successivamente si modifica il certificato di firma token\.  
  
> [!NOTE]  
> È consigliabile \(PKI\) infrastruttura a chiave pubblica non condividere la chiave privata per più scopi. Pertanto, non utilizzare il certificato di comunicazione del servizio che è stato installato nel server federativo come il certificato di firma token\.  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>Come vengono utilizzati i certificati di firma token\ tra i partner  
Ogni certificato di firma token\ contiene chiavi private di crittografia e le chiavi pubbliche utilizzate per firmare digitalmente \ (tramite la chiave privata) un token di sicurezza. In seguito, dopo che sono stati ricevuti da un server federativo partner, queste chiavi convalidare l'autenticità \ (tramite la chiave pubblica) del token di sicurezza crittografato.  
  
Poiché ogni token di sicurezza è firmato digitalmente da partner account, il partner risorse può verificare che il token di sicurezza è stato infatti rilasciato dal partner account e che sia stato modificato. Vengono verificate le firme digitali per la parte pubblica del certificato token\ Cantare un partner. Dopo la firma viene verificata, il server federativo di risorsa genera il proprio token di sicurezza per l'organizzazione e ne firma il token di sicurezza con il proprio certificato di firma token\.  
  
Per gli ambienti di partner di federazione, quando il certificato di firma token\ è stato emesso da un'autorità di certificazione, verificare che:  
  
1.  Il \(CRLs\) gli elenchi di revoca dei certificati del certificato sono accessibili per server Web che considera attendibile il server federativo e relying party.  
  
2.  Il certificato CA radice attendibile dal server Web che considera attendibile il server federativo e relying party.  
  
Il server Web nel partner risorse Usa la chiave pubblica del certificato di firma token\ per verificare che il token di sicurezza è firmato da server federativo di risorsa. Il server Web quindi consente l'accesso appropriato al client.  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>Considerazioni sulla distribuzione per i certificati di firma token\  
Quando si distribuisce il primo server federativo in una nuova installazione di ADFS, è necessario ottenere un certificato di firma token\ e installarlo nell'archivio certificati personali del computer locale nel server federativo. È possibile ottenere una token\ firma certificato da una richiesta da un'organizzazione CA o una CA pubblica o mediante la creazione di un certificato firmato e.  
  
Quando si distribuisce una farm AD FS, i certificati di firma token\ vengono installati in modo diverso, a seconda della modalità di creazione della server farm.  
  
Sono disponibili due opzioni di farm di server che è possibile considerare quando si ottengono i certificati di firma token\ per la distribuzione:  
  
-   La chiave privata di un certificato di firma token\ viene condiviso tra tutti i server federativi in una farm.  
  
    In un ambiente server farm federativa, è consigliabile che tutti i server federativi condividano lo stesso certificato di firma token\ \(or reuse\). È possibile installare un singolo certificato di firma token\ da una CA in un server federativo e quindi esportare la chiave privata, purché il certificato emesso è contrassegnato come esportabile.  
  
    Come illustrato nella figura seguente, la chiave privata da un singolo certificato di firma token\ può essere condiviso per tutti i server federativi in una farm. Questa opzione, confrontato con il seguente "univoco token\ certificato di firma" opzione — riduce i costi se si prevede di ottenere un certificato di firma token\ da una CA pubblica.  
  
![la firma di token](media/adfs2_fedserver_certstory_3.gif)  
  
-   Esiste un certificato di firma token\ univoco per ciascun server federativo in una farm.  
  
    Quando si utilizzano più, certificati univoci in tutta la farm, ogni server della farm firma token con la propria chiave privata univoca.  
  
    Come illustrato nella figura seguente, è possibile ottenere un certificato di firma token\ separato per ogni server federativo singolo nella farm. Questa opzione è più costosa se si prevede di ottenere i certificati di firma token\ da una CA pubblica.  
  
![la firma di token](media/adfs2_fedserver_certstory_4.gif)  
  
Per informazioni sull'installazione di un certificato quando si utilizza Microsoft Certificate Services come CA dell'organizzazione, vedere [IIS 7.0: creare un certificato del Server di dominio in IIS 7.0](https://go.microsoft.com/fwlink/?LinkId=108548).  
  
Per informazioni sull'installazione di un certificato da una CA pubblica, vedere [IIS 7.0: richiedere un certificato del Server Internet](https://go.microsoft.com/fwlink/?LinkId=108549).  
  
Per informazioni sull'installazione di un certificato firmato e, vedere [IIS 7.0: creare un certificato del Server Self\-Signed in IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108271).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
