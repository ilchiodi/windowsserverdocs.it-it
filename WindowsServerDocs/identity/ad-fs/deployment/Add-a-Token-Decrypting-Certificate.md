---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: Aggiungere un certificato di decrittografia di Token
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0eba51521e7ef88542bccf93d92d2e783d800b5e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-token-decrypting-certificate"></a>Aggiungere un certificato di decrittografia di Token

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I server federativi usano un certificato di decrittografia token\ quando un server di federazione relying party necessario decrittografare i token emessi con un certificato meno recente dopo che un nuovo certificato viene impostato come certificato di decrittografia primario. Active Directory Federation Services \(AD FS\) utilizza il certificato Secure Sockets Layer \(SSL\) per Internet Information Services \(IIS\) come certificato di decrittografia predefinito.  
  
> [!CAUTION]  
> I certificati utilizzati per la decrittografia token\ sono fondamentali per la stabilità del servizio federativo. Poiché la perdita o la rimozione non pianificata di qualsiasi certificato configurato a questo scopo può causare interruzioni del servizio, è consigliabile eseguire il backup di tali certificati configurati a questo scopo.  
  
È possibile utilizzare la procedura seguente per aggiungere il certificato di decrittografia token\ per la gestione di ADFS snap-in da un file che sono state esportate.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-add-a-token-decrypting-certificate"></a>Per aggiungere un certificato di decrittografia token\  
  
1.  Nel **Start** digitare**gestione di ADFS**, quindi premere INVIO.  
  
2.  Nell'albero della console fare doppio clic **servizio**, quindi fare clic su **certificati**.  
  
3.  Nel **azioni** riquadro, fare clic su di **certificato di decrittografia di aggiungere Token\** collegamento.  
  
4.  Nel **cercare file di certificato** finestra di dialogo casella, passare al file del certificato che si desidera aggiungere, selezionare il file di certificato e quindi fare clic su **aprire**.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  

