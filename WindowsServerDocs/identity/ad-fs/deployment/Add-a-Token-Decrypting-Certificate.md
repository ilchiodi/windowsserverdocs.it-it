---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: Aggiungere un certificato di decrittografia token
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: cf89972120f3f0effa3eb1cf0fee6d29dbc8ed4e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192478"
---
# <a name="add-a-token-decrypting-certificate"></a>Aggiungere un certificato di decrittografia token

I server federativi usano un token\-certificato di decrittografia quando un server di federazione relying party deve decrittografare i token emessi con un certificato meno recente dopo che un nuovo certificato viene impostato come certificato di decrittografia primario. Active Directory Federation Services \(ADFS\) Usa Secure Sockets Layer \(SSL\) certificato per Internet Information Services \(IIS\) come la decrittografia predefinita certificato.  
  
> [!CAUTION]  
> I certificati usati per il token\-la decrittografia sono fondamentali per la stabilità del servizio federativo. Poiché la perdita o non pianificato per la rimozione di tutti i certificati configurati per questo scopo può causare l'interruzione del servizio, è consigliabile eseguire backup tutti i certificati configurati per questo scopo.  
  
È possibile usare la procedura seguente per aggiungere il token\-decrittografia di certificato per lo snap di gestione di AD FS\-in da un file che è stata esportata.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-token-decrypting-certificate"></a>Per aggiungere un token\-decrittografia di certificato  
  
1.  Nel **avviare** digitare**gestione di AD FS**, e quindi premere INVIO.  
  
2.  Nell'albero della console, fare doppio\-fare clic su **Service**, quindi fare clic su **certificati**.  
  
3.  Nel **azioni** riquadro, fare clic sul **Add Token\-certificato di decrittografia** collegamento.  
  
4.  Nel **cerca i file di certificato** finestra di dialogo passare al file del certificato che si desidera aggiungere, selezionare il file del certificato e quindi fare clic su **Open**.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  

