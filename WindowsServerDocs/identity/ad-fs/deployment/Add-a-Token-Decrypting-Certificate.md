---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: Aggiungere un certificato di decrittografia token
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5714a41950b9c2f818ddc154a9af7a55fdb362d8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814965"
---
# <a name="add-a-token-decrypting-certificate"></a>Aggiungere un certificato di decrittografia token

I server federativi usano un token\-certificato di decrittografia quando un server federativo relying party deve decrittografare i token emessi con un certificato meno recente dopo che un nuovo certificato viene impostato come certificato di decrittografia primario. Active Directory Federation Services \(AD FS\) utilizza il certificato Secure Sockets Layer \(SSL\) per Internet Information Services \(IIS\) come certificato di decrittografia predefinito.  
  
> [!CAUTION]  
> I certificati utilizzati per la decrittografia dei token\-sono fondamentali per la stabilità del Servizio federativo. Poiché la perdita o la rimozione non pianificata di tutti i certificati configurati per questo scopo può causare l'interruzione del servizio, è necessario eseguire il backup di tutti i certificati configurati  
  
È possibile utilizzare la procedura seguente per aggiungere il token\-decrittografare il certificato nello snap-in di gestione AD FS\-in da un file esportato.  
  
L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Per informazioni dettagliate sull'uso degli account appropriati e delle appartenenze a gruppi, vedere [gruppi predefiniti locali e di dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-token-decrypting-certificate"></a>Per aggiungere un token\-la decrittografia del certificato  
  
1.  Nella schermata **Start** Digitare**ad FS Management**, quindi premere INVIO.  
  
2.  Nell'albero della console fare doppio\-fare clic su **servizio**, quindi fare clic su **certificati**.  
  
3.  Nel riquadro **azioni** fare clic sul collegamento **Aggiungi token\-decrittografia del certificato** .  
  
4.  Nella finestra di dialogo **Cerca file di certificato** passare al file del certificato che si desidera aggiungere, selezionare il file del certificato e quindi fare clic su **Apri**.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  

