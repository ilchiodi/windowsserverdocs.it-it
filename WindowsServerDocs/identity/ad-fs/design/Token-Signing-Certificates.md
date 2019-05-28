---
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: Certificati per la firma di token
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9db69cfb2eb42af90b392433a6e05eaab9978160
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190814"
---
# <a name="token-signing-certificates"></a>Certificati per la firma di token

I server federativi richiedono token\-certificati a utenti malintenzionati di modificare o contraffare i token di sicurezza nel tentativo di accesso non autorizzato alle risorse federative per la firma. Privato\/chiave pubblica di associazione che viene utilizzata con token\-i certificati di firma sono il meccanismo di convalida più importante di qualsiasi collaborazione federata poiché queste chiavi verificare che un token di sicurezza emesso da un server federativo partner valido e che il token non è stato modificato durante il transito.  
  
## <a name="token-signing-certificate-requirements"></a>Token\-requisiti del certificato di firma  
Un token\-certificato di firma deve soddisfare i requisiti seguenti per lavorare con ADFS:  
  
-   Per un token\-certificato per firmare un token di sicurezza, il token di firma\-certificato di firma deve contenere una chiave privata.  
  
-   L'account del servizio ADFS deve avere accesso al token\-firma chiave privata del certificato nell'archivio personale del computer locale. Questo viene preso in considerazione dal programma di installazione. È inoltre possibile utilizzare lo snap di gestione di ADFS\-per verificare l'accesso se successivamente si modifica il token\-certificato di firma.  
  
> [!NOTE]  
> Si tratta di un'infrastruttura a chiave pubblica \(PKI\) consigliata per condividere la chiave privata per più scopi. Pertanto, non utilizzare il certificato di comunicazione del servizio che è stato installato nel server federativo come token\-certificato di firma.  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>Come token\-certificati di firma vengono usati tra i partner  
Ogni token\-certificato di firma contiene chiavi private di crittografia e le chiavi pubbliche utilizzate per firmare digitalmente \(tramite la chiave privata\) un token di sicurezza. In un secondo momento, dopo la ricezione da un server federativo del partner, queste chiavi convalidare l'autenticità \(tramite la chiave pubblica\) del token di sicurezza crittografato.  
  
Poiché ogni token di sicurezza è firmato digitalmente da partner account, il partner risorse può verificare che il token di sicurezza è stato infatti rilasciato dal partner account e che sia stato modificato. Vengono verificate le firme digitali per la parte pubblica del token di un partner\-certificato di firma. Dopo la firma viene verificata, il server federativo di risorsa genera il proprio token di sicurezza per l'organizzazione e ne firma il token di sicurezza con il proprio token\-certificato di firma.  
  
Per gli ambienti di partner di federazione, quando il token\-certificato di firma sia stato emesso da un'autorità di certificazione, verificare che:  
  
1.  Elenchi di revoche di certificati \(CRL\) del certificato sono accessibili ai server Web che considera attendibile il server federativo e relying party.  
  
2.  Il certificato CA radice attendibile dal server Web che considera attendibile il server federativo e relying party.  
  
Il server Web nel partner risorse utilizza la chiave pubblica del token\-certificato di firma per verificare che il token di sicurezza è firmato da server federativo di risorsa. Il server Web quindi consente l'accesso appropriato al client.  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>Considerazioni sulla distribuzione di token\-i certificati di firma  
Quando si distribuisce il primo server federativo in una nuova installazione di ADFS, è necessario ottenere un token\-la firma del certificato e installarlo nell'archivio certificati personali del computer locale nel server federativo. È possibile ottenere un token\-firma certificato da una richiesta da un'organizzazione CA o una CA pubblica o tramite la creazione automatica\-certificato firmato.  
  
Quando si distribuisce una farm ADFS, il token\-firma i certificati vengono installati in modo diverso, a seconda della modalità di creazione della server farm.  
  
Sono disponibili due opzioni di farm di server che è possibile considerare quando si ottiene token\-firma dei certificati per la distribuzione:  
  
-   La chiave privata di un token\-certificato di firma viene condiviso tra tutti i server federativi in una farm.  
  
    In un ambiente server farm federativa, è consigliabile che tutti i server federativi condividano \(o riutilizzare\) lo stesso token\-certificato di firma. È possibile installare un singolo token\-firma certificato da un'autorità di certificazione in un server federativo e quindi esportare la chiave privata, purché il certificato emesso è contrassegnato come esportabile.  
  
    Come illustrato nella figura seguente, la chiave privata da un singolo token\-certificato di firma può essere condiviso per tutti i server federativi in una farm. Questa opzione, rispetto al seguente "token univoco\-certificato di firma" opzione — riduce i costi se si prevede di ottenere un token\-firma certificato da una CA pubblica.  
  
![la firma di token](media/adfs2_fedserver_certstory_3.gif)  
  
-   È disponibile un token univoco\-certificato per ciascun server federativo in una farm di firma.  
  
    Quando si utilizzano più, certificati univoci in tutta la farm, ogni server della farm firma token con la propria chiave privata univoca.  
  
    Come illustrato nella figura seguente, è possibile ottenere un token separato\-firma certificato per ogni singola federazione di server nella farm. Questa opzione è più costosa se si prevede di ottenere il token\-firma dei certificati da una CA pubblica.  
  
![la firma di token](media/adfs2_fedserver_certstory_4.gif)  
  
Per informazioni sull'installazione di un certificato quando si utilizza Microsoft Certificate Services come CA dell'organizzazione, vedere [IIS 7.0: Creare un certificato di dominio in IIS 7.0](https://go.microsoft.com/fwlink/?LinkId=108548).  
  
Per informazioni sull'installazione di un certificato da un'autorità di certificazione pubblica, consultare [IIS 7.0: Richiedere un certificato del server Internet](https://go.microsoft.com/fwlink/?LinkId=108549).  
  
Per informazioni sull'installazione automatica\-firma del certificato, vedere [IIS 7.0: Creare un Self\-firma certificato del Server in IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108271).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
