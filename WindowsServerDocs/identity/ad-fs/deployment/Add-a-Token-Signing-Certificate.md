---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Aggiungere un certificato di firma di Token
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8a94e0724d6fd2a04e2fbfc22b3054b49d87f440
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-token-signing-certificate"></a>Aggiungere un certificato di firma di Token

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Server federativo in Active Directory Federation Services \(AD FS\) richiedono i certificati di firma token\ a utenti malintenzionati di modificare o contraffare i token di sicurezza nel tentativo di accesso non autorizzato alle risorse federative. Ogni certificato di firma token\ contiene chiavi private di crittografia e le chiavi pubbliche utilizzate per firmare digitalmente \ (tramite la chiave privata) un token di sicurezza. In seguito, dopo queste chiavi sono state ricevute da un server federativo partner, essi convalidare l'autenticità \ (tramite la chiave pubblica) del token di sicurezza crittografato.  
  
> [!CAUTION]  
> I certificati utilizzati per la firma token\ sono fondamentali per la stabilità del servizio federativo. Poiché la perdita o la rimozione non pianificata di qualsiasi certificato configurato a questo scopo può causare interruzioni del servizio, è consigliabile eseguire il backup di tali certificati configurati a questo scopo.  
  
Il certificato di firma token\ devono essere concatenati a una radice attendibile nel servizio federativo. È possibile utilizzare la procedura seguente per aggiungere il certificato di firma token\ per la gestione di ADFS snap-in da un file che sono state esportate.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-add-a-token-signing-certificate"></a>Per aggiungere un certificato di firma token\  
  
1.  Nel **Start** digitare**gestione di ADFS**, quindi premere INVIO.  
  
2.  Nell'albero della console fare doppio clic **servizio**, quindi fare clic su **certificati**.  
  
3.  Nel **azioni** riquadro, fare clic su di **certificato di firma aggiungere Token\** collegamento.  
  
4.  Nel **cercare file di certificato** finestra di dialogo casella, passare al file del certificato che si desidera aggiungere, selezionare il file di certificato e quindi fare clic su **aprire**.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  

